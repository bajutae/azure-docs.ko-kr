---
title: Azure Cosmos DB Gremlin 제한
description: 그래프 엔진의 런타임 제한 사항에 대 한 참조 설명서
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 10/04/2019
ms.author: lbosq
ms.openlocfilehash: 76ad787990c355d29613c05ca9fce31885a2eccc
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2020
ms.locfileid: "72029855"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB Gremlin 제한
이 문서에서는 Azure Cosmos DB Gremlin 엔진의 제한에 대해 설명 하 고 고객이 탐색에 영향을 미칠 수 있는 방법에 대해 설명 합니다.

Cosmos DB Gremlin는 Cosmos DB 인프라를 기반으로 빌드됩니다. 이로 인해 [Azure Cosmos DB 서비스 제한](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) 에 설명 된 모든 제한이 여전히 적용 됩니다. 

## <a name="limits"></a>제한

Gremlin 제한에 도달 하는 경우 조정 오류를 나타내는 **x m m-** 429의 코드를 사용 하 여 순회를 취소 합니다. 자세한 내용은 [Gremlin 서버 응답 헤더](gremlin-limits.md) 를 참조 하세요.

**리소스**    | **기본 제한** | **설명**
--- | --- | ---
*스크립트 길이* | **64KB** | 요청당 Gremlin 순회 스크립트의 최대 길이입니다.
*연산자 깊이* | **400** |  순회의 총 고유 단계 수입니다. 예를 들어 ```g.V().out()``` 에는 연산자에 ```.repeat()``` 대 한 매개 변수 이기 때문 ```.times(100)``` 에 2: v ( ```g.V('label').repeat(out()).times(100)``` ) 및 out ()의 연산자를 포함 하 고 연산자 깊이가 3: v (), repeat () 및 out ()입니다.
*병렬 처리 수준* | **32** | 단일 요청에서 스토리지 레이어에 쿼리한 스토리지 파티션의 최대 수입니다. 수백 개의 파티션이 있는 그래프는이 제한의 영향을 받습니다.
*반복 제한* | **32** | ```.repeat()``` 연산자가 실행할 수 있는 최대 반복 횟수입니다. 대부분의 경우 ```.repeat()``` 단계의 각 반복은 너비 우선 순회를 실행 합니다. 즉, 모든 트래버스는 꼭 짓 점 사이의 최대 32 홉으로 제한 됩니다.
*순회 시간 제한* | **30초** | 이 시간을 초과 하면 순회가 취소 됩니다. Cosmos DB Graph는 대부분의 순회가 밀리초 내에 완료되는 OLTP 데이터베이스입니다. Cosmos DB Graph에서 OLAP 쿼리를 실행 하려면 [그래프 데이터 프레임](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) 및 [Cosmos DB Spark 커넥터](https://github.com/Azure/azure-cosmosdb-spark)를 사용 하 여 [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) 를 사용 합니다.
*유휴 연결 시간 제한* | **1시간** | Gremlin 서비스가 유휴 websocket 연결을 열어 둘 때까지 걸리는 시간입니다. TCP keep-alive 패킷 또는 HTTP 연결 유지 요청은이 제한을 초과 하는 연결 수명을 연장 하지 않습니다. Cosmos DB Graph 엔진은 실행 중인 활성 Gremlin 요청이 없는 경우 websocket 연결을 유휴 상태로 간주 합니다.
*시간당 리소스 토큰* | **100** | Gremlin 클라이언트가 한 지역의 Gremlin 계정에 연결하는 데 사용하는 고유 리소스 토큰 수입니다. 응용 프로그램이 매시간 고유한 토큰 제한을 초과 하 `"Exceeded allowed resource token limit of 100 that can be used concurrently"` 는 경우는 다음 인증 요청 시 반환 됩니다.

## <a name="next-steps"></a>다음 단계
* [Azure Cosmos DB Gremlin 응답 헤더](gremlin-headers.md) 
* [Gremlin을 사용한 Azure Cosmos DB 리소스 토큰](how-to-use-resource-tokens-gremlin.md)
