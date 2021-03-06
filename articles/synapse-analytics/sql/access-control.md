---
title: 작업 영역, 데이터 및 파이프라인에 대한 액세스 관리
description: Azure Synapse Analytics 작업 영역(미리 보기)에서 작업 영역, 데이터 및 파이프라인에 대한 액세스 제어를 관리하는 방법에 대해 알아봅니다.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 89d2105ab080309639c4341072c3f5f36608dfce
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421107"
---
# <a name="manage-access-to-workspaces-data-and-pipelines"></a>작업 영역, 데이터 및 파이프라인에 대한 액세스 관리

Azure Synapse Analytics 작업 영역(미리 보기)에서 작업 영역, 데이터 및 파이프라인에 대한 액세스 제어를 관리하는 방법에 대해 알아봅니다.

> [!NOTE]
> GA를 위해 Synapse 관련 Azure RBAC 역할을 도입하여 RBAC를 더욱 개량할 예정입니다.

## <a name="access-control-for-workspace"></a>작업 영역 액세스 제어

Azure Synapse 작업 영역에 프로덕션 배포하는 경우 사용자 및 관리자를 쉽게 프로비저닝할 수 있도록 환경을 구성하는 것이 좋습니다.

> [!NOTE]
> 여기서는 여러 보안 그룹을 만든 다음, 일관적으로 보안 그룹을 사용하도록 작업 영역을 구성하는 방법을 사용하겠습니다. 그룹이 설정되면 관리자는 보안 그룹 내의 구성원 자격만 관리하면 됩니다.

### <a name="step-1-set-up-security-groups-with-names-following-this-pattern"></a>1단계: 다음 패턴을 따르는 이름으로 보안 그룹 설정

1. `Synapse_WORKSPACENAME_Users`라는 보안 그룹 만들기
2. `Synapse_WORKSPACENAME_Admins`라는 보안 그룹 만들기
3. `ProjectSynapse_WORKSPACENAME_Users`에 `Synapse_WORKSPACENAME_Admins` 추가

### <a name="step-2-prepare-the-default-adls-gen2-account"></a>2단계: 기본 ADLS Gen2 계정 준비

작업 영역을 프로비저닝할 때 작업 영역에서 사용할 ADLSGEN2 계정과 파일 시스템의 컨테이너를 선택해야 했습니다.

1. [Azure Portal](https://portal.azure.com)을 엽니다.
2. ADLSGEN2 계정으로 이동
3. Azure Synapse 작업 영역에 대해 선택한 컨테이너(파일 시스템)로 이동
4. **액세스 제어(IAM)** 클릭
5. 다음 역할 할당:
   1. **Reader** 역할:  `Synapse_WORKSPACENAME_Users`
   2. **Storage Blob 데이터 소유자** 역할:  `Synapse_WORKSPACENAME_Admins`
   3. **Storage Blob 데이터 기여자** 역할: `Synapse_WORKSPACENAME_Users`
   4. **Storage Blob 데이터 소유자** 역할:  `WORKSPACENAME`
  
### <a name="step-3-configure-the-workspace-admin-list"></a>3단계: 작업 영역 관리자 목록 구성

1. [**Azure Synapse 웹 UI**](https://web.azuresynapse.net)로 이동
2. **관리**  > **보안** > **액세스 제어**로 이동
3. **관리자 추가**를 클릭하고 `Synapse_WORKSPACENAME_Admins` 선택

### <a name="step-4-configure-sql-admin-access-for-the-workspace"></a>4단계: 작업 영역에 대한 SQL 관리자 액세스 구성

1. [Azure Portal](https://portal.azure.com)로 이동
2. 작업 영역으로 이동
3. **설정** > **Active Directory 관리자**로 이동
4. **관리자 설정** 클릭
5. `Synapse_WORKSPACENAME_Admins` 선택
6. **선택** 클릭
7. **저장** 클릭

### <a name="step-5-add-and-remove-users-and-admins-to-security-groups"></a>5단계: 보안 그룹에 사용자 및 관리자 추가/제거

1. 관리자 액세스 권한이 필요한 사용자를 `Synapse_WORKSPACENAME_Admins`에 추가
2. 나머지 사용자는 `Synapse_WORKSPACENAME_Users`에 추가

## <a name="access-control-to-data"></a>데이터에 대한 액세스 제어

기본 데이터에 대한 액세스 제어는 다음과 같은 세 부분으로 나뉩니다.

- 스토리지 계정에 대한 데이터 평면 액세스(위의 2단계에서 이미 구성)
- SQL 데이터베이스에 대한 데이터 평면 액세스(SQL 풀과 SQL 주문형 모두에 대해)
- 스토리지 계정을 통해 SQL 주문형 데이터베이스에 대한 자격 증명 만들기

## <a name="access-control-to-sql-databases"></a>SQL 데이터베이스에 대한 액세스 제어

> [!TIP]
> 사용자에게 모든 SQL 데이터베이스에 대한 액세스 권한을 부여하려면 **각** SQL 데이터베이스에 대해 아래 단계를 실행해야 합니다.

### <a name="sql-on-demand"></a>SQL 주문형

사용자에게 **단일** SQL 주문형 데이터베이스에 대한 액세스 권한을 부여하려면 다음 예제의 단계를 따릅니다.

1. 로그인 만들기

    ```sql
    use master
    go
    CREATE LOGIN [John.Thomas@microsoft.com] FROM EXTERNAL PROVIDER;
    go
    ```

2. 사용자 만들기

    ```sql
    use yourdb -- Use your DB name
    go
    CREATE USER john FROM LOGIN [John.Thomas@microsoft.com];
    ```

3. 지정된 역할의 구성원에 사용자 추가

    ```sql
    use yourdb -- Use your DB name
    go
    alter role db_owner Add member john -- Type USER name from step 2
    ```

### <a name="sql-pools"></a>SQL 풀

사용자에게 **단일** SQL 데이터베이스에 대한 액세스 권한을 부여하려면 다음 단계를 따릅니다.

1. 컨텍스트 선택기(데이터베이스 선택 드롭다운)에서 원하는 데이터베이스를 대상으로 다음 명령을 실행하여 데이터베이스에 사용자를 만듭니다.

    ```sql
    --Create user in SQL DB
    CREATE USER [<alias@domain.com>] FROM EXTERNAL PROVIDER;
    ```

2. 다음과 같이 사용자에게 데이터베이스에 액세스할 수 있는 역할을 부여합니다.

    ```sql
    --Create user in SQL DB
    EXEC sp_addrolemember 'db_owner', '<alias@domain.com>';
    ```

> [!IMPORTANT]
> *db_owner* 권한을 원치 않는 경우 *db_datareader* 및 *db_datawriter*로 읽기/쓰기 권한을 수행할 수 있습니다.
> Spark 사용자가 SQL 풀에서/풀로 바로 데이터를 읽고 쓰려면 *db_owner* 권한이 필요합니다.

사용자를 만든 후에는 다음과 같이 SQL 주문형에서 스토리지 계정을 쿼리할 수 있는지 확인합니다.

- SQL 주문형의 **master** 데이터베이스를 대상으로 다음 명령을 실행합니다.

    ```sql
    CREATE CREDENTIAL [https://<storageaccountname>.dfs.core.windows.net]
    WITH IDENTITY='User Identity';
    ```

## <a name="access-control-to-workspace-pipeline-runs"></a>작업 영역 파이프라인 실행에 대한 액세스 제어

### <a name="workspace-managed-identity"></a>작업 영역 관리 ID

> [!IMPORTANT]
> SQL 풀을 참조하는 데이터 세트 또는 작업이 포함된 파이프라인을 성공적으로 실행하려면 SQL 풀에 대한 액세스 권한을 작업 영역 ID에 직접 부여해야 합니다.

각 SQL 풀에서 다음 명령을 실행하여 작업 영역 관리 ID가 SQL 풀 데이터베이스에서 파이프라인을 실행할 수 있도록 허용합니다.

```sql
--Create user in DB
CREATE USER [<workspacename>] FROM EXTERNAL PROVIDER;

--Granting permission to the identity
GRANT CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;
```

동일한 SQL 풀에서 다음 스크립트를 실행하여 이 권한을 제거할 수 있습니다.

```sql
--Revoking permission to the identity
REVOKE CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;

--Deleting the user in the DB
DROP USER [<workspacename>];
```

## <a name="next-steps"></a>다음 단계

Synapse SQL의 액세스 및 제어에 대한 개요는 [Synapse SQL 액세스 제어](../sql/access-control.md)를 참조하세요. 데이터베이스 보안 주체에 대한 자세한 내용은 [보안 주체](https://msdn.microsoft.com/library/ms181127.aspx)를 참조하세요. 데이터베이스 역할에 대한 추가 정보는 [데이터베이스 역할](https://msdn.microsoft.com/library/ms189121.aspx) 문서에서 찾을 수 있습니다.
