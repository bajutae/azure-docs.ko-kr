---
title: Azure 관리 액세스에 대 한 RBAC 역할 설정
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search 관리에 대 한 관리 작업을 제어 하 고 위임할 Azure Portal의 RBAC (역할 기반 관리 제어).
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 9262d01e35bd03a9116a30b070b023f578f0b15a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74112551"
---
# <a name="set-rbac-roles-for-administrative-access-to-azure-cognitive-search"></a>Azure Cognitive Search에 대 한 관리 액세스에 대 한 RBAC 역할 설정

Azure에서는 포털 또는 Resource Manager API를 통해 관리되는 모든 서비스에 대해 [전역 역할 기반 권한 부여 모델](../role-based-access-control/role-assignments-portal.md) 을 제공합니다. 소유자, 참가자 및 읽기 권한자 역할은 사용자, 그룹 및 각 역할에 할당 된 보안 주체의 Active Directory에 대 한 *서비스 관리* 수준을 결정 합니다. 

> [!Note]
> 인덱스의 일부 또는 문서의 하위 집합을 보호하기 위한 역할 기반 액세스 제어가 있습니다. 검색 결과에 대한 ID 기반 액세스의 경우 보안 필터를 만들어서 ID를 기준으로 결과를 잘라 요청자에게 액세스 권한이 없어야 하는 문서를 제거할 수 있습니다. 자세한 내용은 [보안 필터](search-security-trimming-for-azure-search.md) 및 [Active Directory로 보안](search-security-trimming-for-azure-search-with-aad.md)을 참조하세요.

## <a name="management-tasks-by-role"></a>역할별 관리 작업

Azure Cognitive Search의 경우 역할은 다음 관리 작업을 지 원하는 사용 권한 수준과 연결 됩니다.

| 역할 | 작업 |
| --- | --- |
| 소유자 |api-key, 인덱스, 인덱서, 인덱서 데이터 원본 및 인덱서 일정 등 서비스 또는 해당 서비스의 개체를 만들거나 삭제합니다.<p>개수 및 스토리지 크기를 포함하여 서비스 상태를 봅니다.<p>역할 멤버 자격을 추가하거나 삭제합니다(소유자만 역할 멤버 자격을 관리할 수 있음).<p>구독 관리자 및 서비스 소유자는 소유자 역할의 자동 멤버 자격을 갖습니다. |
| 참가자 |RBAC 역할 관리를 제외하고 소유자와 같은 수준의 액세스 권한입니다. 예를 들어, 참여자는 개체를 만들거나 삭제하거나, [api-keys](search-security-api-keys.md)를 보고 다시 생성할 수 있지만 역할 멤버 자격을 수정할 수는 없습니다. |
| [Search 서비스 참여자 기본 역할](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | 참가자 역할과 동일합니다. |
| 판독기 |서비스 기본 정보 및 메트릭을 봅니다. 이 역할의 멤버는 인덱스, 인덱서, 데이터 원본 또는 주요 정보를 볼 수 없습니다.  |

역할은 서비스 엔드포인트에 대한 액세스 권한을 부여하지 않습니다. 인덱스 관리, 인덱스 채우기 및 검색 데이터 쿼리와 같은 Search 서비스 작업은 역할이 아니라 api-key를 통해 제어합니다. 자세한 내용은 [api-key 관리](search-security-api-keys.md)를 참조하세요.

## <a name="see-also"></a>참고 항목

+ [Powershell을 사용하여 관리](search-manage-powershell.md) 
+ [Azure Cognitive Search의 성능 및 최적화](search-performance-optimization.md)
+ [Azure Portal에서 역할 기반 Access Control를 시작](../role-based-access-control/overview.md)합니다.