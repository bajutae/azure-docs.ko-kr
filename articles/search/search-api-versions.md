---
title: .NET 및 REST에 대 한 API 버전 관리
titleSuffix: Azure Cognitive Search
description: .NET SDK의 Azure Cognitive Search REST Api 및 클라이언트 라이브러리에 대 한 버전 정책입니다.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 178f56354120bf7a65c51f1c9cf54e34bd011d97
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79137291"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure Cognitive Search의 API 버전

Azure Cognitive Search는 정기적으로 기능 업데이트를 롤업 합니다. 항상 그렇지는 않지만 경우에 따라 이러한 업데이트에는 이전 버전과 호환성을 유지하기 위해 API의 새 버전이 필요하기도 합니다. 새 버전을 게시하면 코드에서 검색 서비스 업데이트를 통합 하는 시기와 방법을 제어할 수 있습니다.

규칙에 따라 Azure Cognitive Search 팀은 새 API 버전을 사용 하도록 코드를 업그레이드 하는 데 몇 가지 노력이 수반 될 수 있으므로 필요한 경우에만 새 버전을 게시 합니다. API의 일부를 변경하여 더 이상 이전 버전과 호환되지 않는 경우에만 새 버전이 필요합니다. 그러한 변경은 기존 기능의 수정 또는 기존 API 노출 영역을 변경하는 새로운 기능으로 인해 발생할 수 있습니다.

SDK 업데이트와 동일한 규칙이 적용됩니다. Azure Cognitive Search SDK는 해당 버전의 세 부분 (주, 부 및 빌드 번호 (예: 1.1.0))을 포함 하는 [의미 체계 버전 관리](https://semver.org/) 규칙을 따릅니다. 이전 버전과 호환되지 않는 변경의 경우에만 SDK의 새로운 주 버전이 릴리스됩니다. 호환성을 유지하는 기능 업데이트의 경우 부 버전이, 버그 수정의 경우 빌드 버전이 증가합니다.

> [!NOTE]
> Azure Cognitive Search 서비스 인스턴스는 최신 버전을 포함 하 여 여러 REST API 버전을 지원 합니다. 더 이상 최신 버전이 아닌 버전을 계속 사용할 수는 있지만 코드를 마이그레이션하여 최신 버전을 사용하는 것이 좋습니다. REST API를 사용하는 경우 api-version 매개 변수를 통해 모든 요청에 API 버전을 지정해야 합니다. .NET SDK를 사용하는 경우 사용 중인 SDK의 버전에 따라 해당하는 REST API 버전이 결정됩니다. 이전 버전의 SDK를 사용하는 경우 최신 API 버전을 지원하기 위해 서비스가 업그레이드된 경우에도 변경 내용 없이 해당 코드를 계속 실행할 수 있습니다.

## <a name="snapshot-of-current-versions"></a>현재 버전의 스냅샷
다음은 Azure Cognitive Search에 대 한 모든 프로그래밍 인터페이스의 현재 버전에 대 한 스냅숏입니다.


| 인터페이스 | 가장 최근의 주 버전 | 상태 |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |일반 공급, 릴리스 2019 년 5 월 |
| [.NET SDK 미리 보기](https://aka.ms/search-sdk-preview) |8.0-미리 보기 |Preview, 4 월 2019 릴리스 |
| [서비스 REST API](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |일반 공급 |
| [서비스 REST API 2019-05-06-미리 보기](search-api-preview.md) |2019-05-06-미리 보기 |미리 보기 |
| [.NET 관리 SDK](https://aka.ms/search-mgmt-sdk) |3.0 |일반 공급 |
| [관리 REST API](https://docs.microsoft.com/rest/api/searchmanagement/) |2020-03-13|일반 공급 |

REST API의 경우 각 호출의 `api-version` 을 포함해야 합니다. `api-version`을 사용하면 API 미리 보기 등 특정 버전을 대상으로 하기 쉽습니다. 다음 예제에서는 `api-version` 매개 변수를 지정하는 방법을 보여 줍니다.

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> 각 요청에는 `api-version`이 있지만, 모든 API 요청에 대해 동일한 버전을 사용하는 것이 좋습니다. 새 API 버전이 이전 버전에서 인식 하지 못하는 특성 또는 작업을 도입하는 경우 특히 유용합니다. 혼합 API 버전에는 의도하지 않은 결과가 있을 수 있기 때문에 사용하지 말아야 합니다.
>
> 서비스 REST API 및 관리 REST API는 따로따로 버전이 지정됩니다. 버전 번호 체계는 동일합니다.

일반 공급(또는 GA) API는 프로덕션에서 사용할 수 있으며 Azure 서비스 수준 계약에 적용을 받습니다. 미리 보기 버전에는 GA 버전으로 항상 마이그레이션되지 않는 실험적 기능이 있습니다. **프로덕션 애플리케이션에서 미리 보기 API를 사용하지 않는 것이 좋습니다.**

## <a name="update-to-the-latest-version-of-the-rest-api-by-october-15-2020"></a>2020 년 10 월 15 일까 지 최신 버전의 REST API 업데이트
Azure Cognitive Search REST API의 다음 버전은 사용 중지 되며 2020 년 10 월 15 일: **2014-07-31-미리 보기**, **2014-10-20-미리**보기, **2015-02-28-미리 보기**및 **2015-02-28**부터 더 이상 지원 되지 않습니다. 또한 3.0.0 보다 오래 된 Azure Cognitive Search .NET SDK 버전은 이러한 REST API 버전 중 하나를 대상으로 하기 때문에 **사용** 이 중지 됩니다. 이 날짜 이후에는 더 이상 사용 되지 않는 REST API 또는 SDK 버전 중 하나를 사용 하는 응용 프로그램이 더 이상 작동 하지 않으므로 업그레이드 해야 합니다. 이 유형을 변경 하는 경우와 마찬가지로 12 개월의 통지를 제공 하므로 적절 한 시간을 조정할 수 있습니다.  계속 해 서 Azure Cognitive Search을 사용 하려면 [REST API](search-api-migration.md) 를 대상으로 하는 기존 코드를 [REST API 버전 2019-05-06](https://docs.microsoft.com/rest/api/searchservice/) 이상 또는 .Net SDK를 년 10 월 15 2020 일에 [버전 3.0](search-dotnet-sdk-migration.md) 이상으로 마이그레이션 하세요.  최신 버전으로 업데이트 하는 방법에 대 한 질문이 있는 경우 코드를 업데이트 azuresearch_contact@microsoft.com 하는 데 충분 한 시간을 갖도록 2020 년 5 월 15 일에 메일을 보내 주시기 바랍니다.

## <a name="about-preview-and-generally-available-versions"></a>미리 보기 및 일반 공급 버전 정보
Azure Cognitive Search는 먼저 REST API를 통해 실험적 기능을 미리 릴리스한 후 시험판 버전의 .NET SDK를 통해 미리 릴리스 합니다.

미리 보기 기능은 기능 디자인 및 구현에 관한 사용자 의견을 수집하기 위해 테스트 및 실험에 사용할 수 있습니다. 이러한 이유로 미리 보기 기능은 시간에 따라 이전 버전과 호환되지 않는 방식으로 변경될 수 있습니다. 이는 이전 버전과 호환되는 일부 수정 및 향상된 기능 외에는 변경되지 않으며 안정적인 GA 버전의 기능과 대조됩니다. 또한 미리 보기 기능이 항상 GA로 릴리스되는 것은 아닙니다.

이러한 이유로 미리 보기 버전에 종속된 프로덕션 코드를 작성하지 않는 것이 좋습니다. 이전 미리 보기 버전을 사용하는 경우 일반 공급(GA) 버전으로 마이그레이션하는 것이 좋습니다.

.NET SDK의 코드 마이그레이션에 대한 지침은 [.NET SDK 업그레이드](search-dotnet-sdk-migration-version-9.md)에서 찾을 수 있습니다.

일반 공급은 Azure Cognitive Search이 이제 SLA (서비스 수준 계약)를 사용 함을 의미 합니다. SLA는 [Azure Cognitive Search 서비스 수준 계약](https://azure.microsoft.com/support/legal/sla/search/v1_0/)에서 찾을 수 있습니다.
