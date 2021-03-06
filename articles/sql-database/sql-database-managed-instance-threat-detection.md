---
title: Advanced Threat Protection-관리 되는 인스턴스 구성
description: Advanced Threat Protection은 관리 되는 인스턴스의 데이터베이스에 대 한 잠재적인 보안 위협을 나타내는 비정상적인 데이터베이스 활동을 검색 합니다.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 69292a934af8b8777f11ab58ed3fe306abf8b408
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2020
ms.locfileid: "73822557"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>Azure SQL Database 관리 되는 인스턴스에서 Advanced Threat Protection 구성

[관리 되는 인스턴스에](sql-database-managed-instance-index.yml) 대 한 [Advanced Threat Protection](sql-database-threat-detection-overview.md) 은 비정상적인 활동을 감지 하 여 데이터베이스에 액세스 하거나 악용 하려는 비정상적인 시도를 감지 합니다. Advanced Threat Protection은 **잠재적 sql 삽입**, **비정상적인 위치 또는 데이터 센터에서 액세스**, **익숙하지 않은 사용자 또는 잠재적으로 유해한 응용 프로그램에서 액세스**, **무차별 암호 대입 SQL 자격 증명** 등을 식별할 수 있습니다. [advanced threat protection 경고](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts)에서 자세한 정보를 참조 하세요.

[이메일 알림](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) 또는 [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)을 통해 탐지된 위협에 대한 알림을 받을 수 있습니다.

Advanced [Threat Protection](sql-database-threat-detection-overview.md) 은 고급 SQL 보안 기능을 위한 통합 패키지인 광고 ( [advanced data security](sql-database-advanced-data-security.md) ) 제품의 일부입니다. 중앙의 SQL ADS 포털을 통해 Advanced Threat Protection에 액세스하고 관리할 수 있습니다.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure Portal에서 Advanced Threat Protection 설정

1. 에서 [https://portal.azure.com](https://portal.azure.com)Azure Portal를 시작 합니다.
2. 보호할 관리되는 인스턴스의 구성 페이지로 이동합니다. **설정** 페이지에서 **고급 데이터 보안**을 선택 합니다.
3. 고급 데이터 보안 구성 페이지에서
   - 고급 데이터 보안 **을** 설정 합니다.
   - 비정상적인 데이터베이스 활동이 감지 되 면 보안 경고를 수신 하도록 **전자 메일 목록을** 구성 합니다.
   - 비정상적인 위협 감사 레코드가 저장되는 **Azure Storage 계정**을 선택합니다.
   - 구성할 **고급 위협 방지 유형을** 선택 합니다. [Advanced Threat Protection 경고](sql-database-threat-detection-overview.md)에 대해 자세히 알아보세요.
4. **저장** 을 클릭 하 여 새로운 또는 업데이트 된 고급 데이터 보안 정책을 저장 합니다.

   ![고급 위협 보호](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > 스크린샷의 가격은 항상 현재 가격을 반영 하지는 않으며 예입니다.

## <a name="next-steps"></a>다음 단계

- [Advanced Threat Protection](sql-database-threat-detection-overview.md)에 대해 자세히 알아보세요.
- [관리되는 인스턴스란?](sql-database-managed-instance.md)에서 관리되는 인스턴스에 대해 자세히 알아봅니다.
- [단일 데이터베이스에 대 한 Advanced Threat Protection](sql-database-threat-detection.md)에 대해 자세히 알아보세요.
- [관리 되는 인스턴스 감사](https://go.microsoft.com/fwlink/?linkid=869430)에 대해 자세히 알아보세요.
- [Azure security center](https://docs.microsoft.com/azure/security-center/security-center-intro)에 대해 자세히 알아보세요.
