---
title: Azure Cosmos DB에서 변경 피드 지원 사용
description: Azure Cosmos DB 변경 피드 지원을 사용 하 여 문서의 변경 내용 추적, 트리거와 같은 이벤트 기반 처리, 캐시 및 분석 시스템을 최신 상태로 유지
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2020
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: e36e95aeb25c83ccd94f11e25bfe9f1b8f7bfdad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80984864"
---
# <a name="change-feed-in-azure-cosmos-db"></a>Azure Cosmos DB의 변경 피드

Azure Cosmos DB의 변경 피드 지원은 모든 변경 사항에 대해 Azure Cosmos 컨테이너를 수신하여 작동합니다. 그런 다음 변경된 문서가 수정된 순서로 정렬된 목록이 출력됩니다. 변경 사항은 지속적이며, 비동기적 및 증분적으로 처리할 수 있고 출력을 하나 이상의 소비자 사이에 분산하여 병렬 처리가 가능합니다.

[변경 피드 디자인 패턴](change-feed-design-patterns.md)에 대해 자세히 알아보세요.

## <a name="supported-apis-and-client-sdks"></a>지원되는 API 및 클라이언트 SDK

이 기능은 현재 다음 Azure Cosmos DB API 및 클라이언트 SDK에서 지원됩니다.

| **클라이언트 드라이버** | **SQL API** | **Cassandra 용 Azure Cosmos DB API** | **Azure Cosmos DB의 API for MongoDB** | **Gremlin API**|**테이블 API** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | 예 | 예 | 예 | 예 | 아니요 |
|Java|예|예|예|예|아니요|
|Python|예|예|예|예|아니요|
|Node/JS|예|예|예|예|아니요|

## <a name="change-feed-and-different-operations"></a>변경 피드 및 다양한 작업

현재 변경 피드의 모든 삽입 및 업데이트가 표시 됩니다. 특정 작업 유형에 대 한 변경 피드는 필터링 할 수 없습니다. 가능한 한 가지 방법은 업데이트를 위해 항목에 "소프트 마커"를 추가 하 고 변경 피드의 항목을 처리할 때이에 따라 필터를 추가 하는 것입니다.

현재 변경 피드는 로그 삭제를 하지 않습니다. 이전 예제와 마찬가지로 삭제할 항목에 소프트 마커를 추가할 수 있습니다. 예를 들어 "deleted" 라는 항목에 특성을 추가 하 고이를 "true"로 설정 하 고 항목에 대 한 TTL을 설정 하 여 자동으로 삭제할 수 있습니다. 기록 항목에 대 한 변경 피드 (항목에 해당 하는 가장 최근의 변경 내용에는 중간 변경을 포함 하지 않음)를 읽을 수 있습니다. 예를 들어 5 년 전에 추가 된 항목입니다. 컨테이너의 원본으로 다시 변경 피드를 읽을 수 있지만, 항목이 삭제 되 면 변경 피드에서 제거 됩니다.

### <a name="sort-order-of-items-in-change-feed"></a>변경 피드에서 항목의 정렬 순서

변경 피드 항목은 수정 시간 순서대로 도착합니다. 이 정렬 순서는 논리적 파티션 키 당 보장 됩니다.

### <a name="consistency-level"></a>일관성 수준

최종 일관성 수준에서 변경 피드를 사용 하는 동안 후속 변경 피드 읽기 작업 간에 중복 된 이벤트가 있을 수 있습니다 (한 읽기 작업의 마지막 이벤트는 다음 중 첫 번째 작업으로 표시 됨).

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>다중 지역 Azure Cosmos 계정의 변경 피드

다중 지역 Azure Cosmos 계정에서 쓰기 지역이 장애 조치(failover)되는 경우 변경 피드는 수동 장애 조치(failover) 작업에서 작동하고 계속 인접합니다.

### <a name="change-feed-and-time-to-live-ttl"></a>변경 피드 및 TTL(Time to Live)

항목에서 TTL(Time to Live) 속성이 -1로 설정된 경우 변경 피드는 영원히 유지됩니다. 데이터를 삭제하지 않는 이상, 데이터는 변경 피드에 계속 남아 있습니다.  

### <a name="change-feed-and-_etag-_lsn-or-_ts"></a>변경 피드 및 _etag, _lsn 또는 _ts

_etag 형식은 내부적이며 언제든지 변경될 수 있으므로 의존하면 안 됩니다. _ts는 수정 또는 생성 타임스탬프입니다. _ts를 시간순 비교에 사용할 수 있습니다. _lsn은 변경 피드에만 추가 되는 일괄 처리 ID입니다. 트랜잭션 ID를 나타냅니다. 많은 항목에는 동일한 _lsn이 있을 수 있습니다. FeedResponse의 ETag는 항목에 표시된 _etag와 다릅니다. _etag는 내부 식별자이며 항목의 버전에 대해 알려주는 동시성 컨트롤에 사용되는 반면 ETag는 피드 시퀀싱에 사용됩니다.

## <a name="working-with-change-feed"></a>변경 피드를 사용하여 작업

다음 옵션을 통해 변경 피드를 사용하여 작업할 수 있습니다.

* [Azure Functions로 변경 피드 사용](change-feed-functions.md)
* [변경 피드 프로세서와 함께 변경 피드 사용](change-feed-processor.md) 

변경 피드는 컨테이너 내에서 각 논리 파티션 키에 대해 사용할 수 있으며, 아래 이미지와 같이 한 명 이상의 소비자에게 배포하여 병렬 처리할 수 있습니다.

![Azure Cosmos DB 변경 피드의 분산 처리](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>변경 피드의 기능

* 변경 피드는 모든 Azure Cosmos 계정에 기본적으로 사용됩니다.

* Azure Cosmos 데이터베이스와 연결된 모든 지역에서 [프로비전된 처리량](request-units.md)을 사용하여 Azure Cosmos DB의 다른 작업과 마찬가지로 변경 피드에서 읽을 수 있습니다.

* 변경 피드는 컨테이너 내의 항목에 수행된 삽입 및 업데이트 작업을 포함합니다. 삭제 대신 항목(예: 문서) 내에서 "soft-delete" 플래그를 설정하여 삭제를 캡처할 수 있습니다. 또는 [TTL 기능](time-to-live.md)을 사용하여 항목에 대한 제한된 만료 기간을 설정할 수 있습니다. 예를 들어 24시간 및 삭제를 캡처하는 해당 속성의 값을 사용합니다. 이 솔루션을 사용하여 TTL 만료 기간보다 짧은 시간 간격 내에 변경 내용을 처리해야 합니다.

* 각 항목에 대한 변경 내용이 정확히 한 번 변경 피드에 표시되고, 클라이언트는 검사점 논리를 관리해야 합니다. 검사점 관리의 복잡성을 방지 하려는 경우 변경 피드 프로세서는 자동 검사점 및 "최소 한 번" 의미 체계를 제공 합니다. 변경 [피드 프로세서와 함께 변경 피드 사용을](change-feed-processor.md)참조 하세요.

* 지정된 항목에 대한 가장 최근의 변경 내용만이 변경 로그에 포함됩니다. 중간 변경 내용을 사용할 수 없습니다.

* 변경 피드는 각 논리 파티션 키 값 내에서 수정된 순서로 정렬됩니다. 파티션 키 값에 보장된 순서가 없습니다.

* 특정 시점에서 변경 내용을 동기화할 수 있습니다. 즉, 변경 내용을 사용할 수 있는 고정 데이터 보존 기간이 없습니다.

* Azure Cosmos 컨테이너의 모든 논리 파티션 키에 대해 변경 내용을 동시에 사용할 수 있습니다. 이 기능을 사용하면 대규모 컨테이너의 변경 내용을 여러 소비자에 의해 병렬로 처리할 수 있습니다.

* 응용 프로그램은 동일한 컨테이너에서 동시에 여러 변경 피드를 요청할 수 있습니다. 초기 시작점을 제공하기 위해 ChangeFeedOptions.StartTime을 사용할 수 있습니다. 예를 들어 지정된 클록 시간에 해당하는 연속 토큰을 찾을 수 있습니다. 지정 된 경우 ContinuationToken는 StartTime 및 StartFromBeginning 값 보다 우선 합니다. ChangeFeedOptions.StartTime의 정확도는 5초입니다.

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra 및 MongoDB에 대 한 Api의 변경 피드

변경 피드 기능은 MongoDB API에서 변경 스트림으로 표시 되 고 Cassandra API 조건자를 사용 하 여 쿼리 됩니다. MongoDB API에 대 한 구현 세부 정보에 대 한 자세한 내용은 [MongoDB에 대 한 AZURE COSMOS DB api의 변경 스트림](mongodb-change-streams.md)을 참조 하세요.

기본 Apache Cassandra는 cdc (변경 데이터 캡처)를 제공 합니다 .이 메커니즘은 보관을 위해 특정 테이블에 플래그를 지정 하 고 CDC 로그의 구성 가능한 크기에 도달 하면 해당 테이블에 대 한 쓰기를 거부 하는 메커니즘입니다. Cassandra에 대 한 Azure Cosmos DB API의 변경 피드 기능은 CQL을 통해 조건자를 사용 하 여 변경 내용을 쿼리 하는 기능을 향상 시킵니다. 구현 세부 정보에 대 한 자세한 내용은 [Cassandra에 대 한 AZURE COSMOS DB API에서 변경 피드](cassandra-change-feed.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

이제 다음 문서에서 변경 피드에 대해 자세히 알아볼 수 있습니다.

* [변경 피드를 읽는 옵션](read-change-feed.md)
* [Azure Functions로 변경 피드 사용](change-feed-functions.md)
* [변경 피드 프로세서 사용](change-feed-processor.md)
