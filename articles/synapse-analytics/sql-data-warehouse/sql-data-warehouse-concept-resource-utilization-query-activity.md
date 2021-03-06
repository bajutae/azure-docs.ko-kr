---
title: 관리 효율성 및 모니터링-쿼리 작업, 리소스 사용률
description: Azure Synapse Analytics를 관리 하 고 모니터링 하는 데 사용할 수 있는 기능에 대해 알아봅니다. Azure Portal과 DMV(Dynamic Management Views)를 사용하여 데이터 웨어하우스의 쿼리 작업 및 리소스 사용률을 이해합니다.
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/09/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d38c0df45da3a751a456846813543a4ce5de98eb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416208"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Azure Synapse Analytics에서 리소스 사용률 및 쿼리 작업 모니터링

Azure Synapse Analytics는 데이터 웨어하우스 워크 로드와 관련 된 정보를 노출 하기 위해 Azure Portal 내에서 풍부한 모니터링 환경을 제공 합니다. 데이터 웨어하우스를 모니터링할 때는 구성 가능한 보존 기간, 경고, 권장 사항, 메트릭과 로그용 사용자 지정 가능한 차트 및 대시보드를 제공하는 도구인 Azure Portal을 사용하는 것이 좋습니다. 또한 포털에서는 Log analytics를 사용 하 여 Azure Monitor (로그)와 같은 다른 Azure 모니터링 서비스와 통합 하 여 데이터 웨어하우스 뿐만 아니라 통합 모니터링 환경을 위한 전체 Azure 분석 플랫폼에 대해 전체적인 모니터링 환경을 제공할 수 있습니다. 이 설명서에서는 SQL Analytics를 사용 하 여 분석 플랫폼을 최적화 하 고 관리 하는 데 사용할 수 있는 모니터링 기능에 대해 설명 합니다.

## <a name="resource-utilization"></a>리소스 사용률

SQL Analytics의 Azure Portal에서 다음 메트릭을 사용할 수 있습니다. 이러한 메트릭은 [Azure Monitor](../../azure-monitor/platform/data-collection.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#metrics)를 통해 나타납니다.

| 메트릭 이름             | Description                                                  | 집계 형식 |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU 비율          | 데이터 웨어하우스에 대한 모든 노드에서의 CPU 사용률      | 평균, 최소값, 최대값    |
| 데이터 IO 비율      | 데이터 웨어하우스에 대한 모든 노드에서의 IO 사용률       | 평균, 최소값, 최대값    |
| 메모리 비율       | 데이터 웨어하우스의 모든 노드에 걸친 메모리 사용률 (SQL Server) | 평균, 최소값, 최대값   |
| 활성 쿼리          | 시스템에서 실행 중인 활성 쿼리 수입니다.             | 합계              |
| 대기 중인 쿼리          | 실행을 시작 하기 위해 대기 중인 쿼리 수          | 합계              |
| 성공적인 연결  | 데이터베이스에 대 한 성공한 연결 수 (로그인) | 합계, 개수       |
| 실패한 연결      | 데이터베이스에 대해 실패 한 연결 (로그인) 수 | 합계, 개수       |
| 방화벽에 의해 차단     | 차단된 데이터 웨어하우스에 대한 로그인 횟수     | 합계, 개수       |
| DWU 제한               | 데이터 웨어하우스의 서비스 수준 목표                | 평균, 최소값, 최대값    |
| DWU 백분율          | CPU 비율과 데이터 IO 비율 사이의 최댓값        | 평균, 최소값, 최대값    |
| DWU 사용됨                | DWU 한도 * DWU 비율                                   | 평균, 최소값, 최대값    |
| 캐시 적중 비율    | (캐시 적중/캐시 누락) * 100, 여기서 캐시 적중은 로컬 SSD 캐시에서 적중된 모든 columnstore 세그먼트에 대한 합계이며, 캐시 누락은 모든 노드에 걸쳐 로컬 SSD 캐시에서 누락된 columnstore 세그먼트에 대한 합계입니다. | 평균, 최소값, 최대값    |
| 캐시 사용 비율   | (사용된 캐시/캐시 용량) * 100, 여기서 사용된 캐시는 모든 노드에 걸친 로컬 SSD 캐시의 모든 바이트에 대한 합계이며, 캐시 용량은 모든 노드에 걸친 로컬 SSD 캐시의 스토리지 용량에 대한 합계입니다 | 평균, 최소값, 최대값    |
| 로컬 tempdb 백분율 | 모든 컴퓨팅 노드의 로컬 tempdb 사용률 - 5분마다 값을 내보냅니다. | 평균, 최소값, 최대값    |
| 데이터 저장소 크기 (GB) | 데이터베이스의 총 크기입니다. 여기에는 사용 된 공간, 예약 된 공간 및 할당 되지 않은 공간이 포함 됩니다. 데이터베이스의 할당 되지 않은 공간은 쿼리 및 로드 성능을 최적화 하기 위해 유지 됩니다. | 합계 |
| 재해 복구 크기 (GB) | 24 시간 마다 수행 된 지역 백업 전체 크기 | 합계 |
| 스냅숏 저장소 크기 (GB) | 데이터베이스 복원 지점이 제공 되는 데 걸린 총 스냅숏 크기입니다. 여기에는 자동화 된 사용자 정의 스냅숏이 포함 됩니다. | 합계 |

메트릭을 보고 경고를 설정할 때 고려해 야 할 사항:

- DWU는 SQL 풀에서 **의 사용에 대 한 상위 수준 표시** 만을 나타내며 포괄적인 사용률 지표를 의미 하지는 않습니다. 규모를 확장 또는 축소할 지를 결정 하려면 DWU의 영향을 받을 수 있는 모든 요소 (예: 동시성, 메모리, tempdb 및 적응 캐시 용량)를 고려 합니다. [다른 DWU 설정에서 워크 로드를 실행](sql-data-warehouse-manage-compute-overview.md#finding-the-right-size-of-data-warehouse-units) 하 여 비즈니스 목표를 충족 하는 데 가장 적합 한 방식을 결정 하는 것이 좋습니다.
- 논리 서버가 아닌 특정 데이터 웨어하우스에 대해 실패 한 연결 및 성공한 연결 보고
- 메모리 백분율은 데이터 웨어하우스가 유휴 상태에 있는 경우에도 사용률을 반영 합니다 .이는 활성 작업 메모리 사용을 반영 하지 않습니다. 다른 (tempdb, gen2 cache)와 함께이 메트릭을 사용 하 고 추적 하 여 추가 캐시 용량을 조정 하면 요구 사항에 맞게 워크 로드 성능이 향상 되는 경우에 대 한 전체적인 의사 결정을 내릴 수 있습니다.

## <a name="query-activity"></a>쿼리 작업

T-sql을 통해 SQL Analytics를 모니터링할 때의 프로그래밍 경험을 위해 서비스는 Dmv (동적 관리 뷰) 집합을 제공 합니다. 이러한 보기는 워크로드로 인한 성능 병목 상태를 적극적으로 해결하고 식별할 때 유용합니다.

Synapse SQL에 적용 되는 Dmv 목록을 보려면이 [설명서](../sql/reference-tsql-system-views.md#sql-pool-dynamic-management-views-dmvs)를 참조 하세요. 

## <a name="metrics-and-diagnostics-logging"></a>메트릭 및 진단 로깅 

메트릭 및 로그는 Azure Monitor, 특히 [Azure Monitor logs](../../azure-monitor/log-query/log-query-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) 구성 요소로 내보낼 수 있으며 [로그 쿼리](../../azure-monitor/log-query/get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)를 통해 프로그래밍 방식으로 액세스할 수 있습니다. SQL Analytics의 로그 대기 시간은 약 10-15 분입니다. 대기 시간에 영향을 주는 요인에 대 한 자세한 내용은 다음 설명서를 참조 하세요.

## <a name="next-steps"></a>다음 단계

다음 방법 가이드에서는 데이터 웨어하우스를 모니터링 하 고 관리 하는 일반적인 시나리오 및 사용 사례에 대해 설명 합니다.

- [DMV를 사용하여 데이터 웨어하우스 워크로드 모니터링](sql-data-warehouse-manage-monitor.md)
