---
title: Azure Synapse에 대 한 열 수준 보안 이란?
description: 열 수준 보안을 통해 고객은 사용자의 실행 컨텍스트 또는 그룹 멤버 자격에 따라 데이터베이스 테이블 열에 대 한 액세스를 제어 하 고, 응용 프로그램에서 보안의 디자인과 코딩을 간소화 하 고, 열 액세스에 대 한 제한을 구현할 수 있습니다.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/19/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: b0a783ad5db86ca783ff1cebceec8d77ab528047
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687930"
---
# <a name="column-level-security"></a>열 수준 보안

열 수준 보안을 통해 고객은 사용자의 실행 컨텍스트 또는 그룹 멤버 자격에 따라 테이블 열에 대 한 액세스를 제어할 수 있습니다.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
이 비디오가 게시 된 후 Azure Synapse에서 [행 수준 보안](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 을 사용할 수 있습니다.

열 수준 보안은 응용 프로그램의 보안 설계 및 코딩을 간소화 하 여 열 액세스를 제한 하 여 중요 한 데이터를 보호할 수 있도록 합니다. 예를 들어 특정 사용자가 해당 부서와 관련된 테이블의 특정 열에만 액세스할 수 있도록 합니다. 액세스 제한 논리는 다른 애플리케이션 계층의 데이터와 다소 떨어진 데이터베이스 계층에 위치합니다. 데이터베이스는 모든 계층에서 데이터 액세스를 시도할 때마다 액세스 제한을 적용 합니다. 이러한 제한으로 인해 전체 보안 시스템의 노출 영역을 줄임으로써 보안을 보다 안정적이 고 강력 하 게 만들 수 있습니다. 또한 열 수준 보안을 사용 하면 사용자에 대 한 액세스 제한을 적용 하기 위해 열을 필터링 하는 뷰를 도입할 필요가 없습니다.

[GRANT](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) t-sql 문으로 열 수준 보안을 구현할 수 있습니다. 이 메커니즘을 사용하면 SQL 및 AAD(Azure Active Directory) 인증이 모두 지원됩니다.

![cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>구문

```syntaxsql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>예제

다음 예에서는 `TestUser` `SSN` `Membership` 테이블의 열에 액세스 하는 것을 제한 하는 방법을 보여 줍니다.

주민 `Membership` 등록 번호를 저장 하는 데 사용 되는 SSN 열이 있는 테이블을 만듭니다.

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

중요 `TestUser` 한 데이터가 포함 된 SSN 열을 제외 하 고 모든 열에 액세스할 수 있습니다.

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

다음으로 `TestUser` 실행 되는 쿼리는 SSN 열을 포함 하는 경우 실패 합니다.

```sql
SELECT * FROM Membership;

-- Msg 230, Level 14, State 1, Line 12
-- The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>사용 사례

현재 열 수준 보안을 사용 하는 방법에 대 한 몇 가지 예는 다음과 같습니다.

- 금융 서비스 회사는 계정 관리자만 고객 SSN(사회 보장 번호), 전화 번호 및 기타 PII(개인 식별 정보)에 액세스할 수 있도록 허용합니다.
- 의료 보험 공급자는 의사 및 간호사만이 데이터를 볼 수 있도록 하는 동시에 중요 의료 기록에 대 한 액세스를 허용 합니다.
