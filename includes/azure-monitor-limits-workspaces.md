---
title: 파일 포함
description: 포함 파일
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 864b37c9e59786546ad2c29faf8457cfc3a21f6b
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2020
ms.locfileid: "82161162"
---
**데이터 수집 볼륨 및 보존** 

| 계층 | 일별 제한 | 데이터 보존 | 설명 |
|:---|:---|:---|:---|
| 현재 GB 당 가격 책정 계층<br>(4 월 2018 도입) | 제한 없음 | 30-730 일 | 31 일을 초과 하는 데이터 보존은 추가 요금으로 사용할 수 있습니다. Azure Monitor 가격에 대해 자세히 알아봅니다. |
| 레거시 무료 계층<br>(4 월 2016 도입) | 500MB | 7 일 | 작업 영역이 일일 500 제한에 도달 하면 다음 날부터 데이터 수집을 중지 하 고 다시 시작 합니다. 1일은 UTC를 기준으로 합니다. Azure Security Center에 의해 수집 된 데이터는이 500 MB의 제한에 포함 되지 않으며이 한도를 초과 하 여 계속 수집 됩니다.  |
| GB 당 레거시 독립 실행형 계층<br>(4 월 2016 도입) | 제한 없음 | 30 ~ 730 일 | 31 일을 초과 하는 데이터 보존은 추가 요금으로 사용할 수 있습니다. Azure Monitor 가격에 대해 자세히 알아봅니다. |
| OMS (노드당 레거시)<br>(4 월 2016 도입) | 제한 없음 | 30 ~ 730 일 | 31 일을 초과 하는 데이터 보존은 추가 요금으로 사용할 수 있습니다. Azure Monitor 가격에 대해 자세히 알아봅니다. |
| 레거시 표준 계층 | 제한 없음 | 30일  | 보존을 조정할 수 없습니다. |
| 레거시 프리미엄 계층 | 제한 없음 | 365일  | 보존을 조정할 수 없습니다. |

**구독 당 작업 영역 수입니다.**

| 가격 책정 계층    | 작업 영역 제한 | 주석
|:---|:---|:---|
| 무료 계층  | 10 | 이 제한은 늘릴 수 없습니다. |
| 다른 모든 계층 | 제한 없음 | 리소스 그룹 내의 리소스 수와 구독 당 리소스 그룹 수에 의해 제한 됩니다. |

**Azure Portal**

| 범주 | 제한 | 주석 |
|:---|:---|:---|
| 로그 쿼리에서 반환 되는 최대 레코드 | 10000 | 쿼리에서 쿼리 범위, 시간 범위 및 필터를 사용 하 여 결과를 줄입니다. |


**데이터 수집기 API**

| 범주 | 제한 | 주석 |
|:---|:---|:---|
| 단일 게시물의 최대 크기 | 30mb | 큰 볼륨을 여러 개의 게시물로 분할 합니다. |
| 필드 값의 최대 크기  | 32KB | 32KB보다 긴 필드는 잘립니다. |

**검색 API**

| 범주 | 제한 | 주석 |
|:---|:---|:---|
| 단일 쿼리로 반환 되는 최대 레코드 | 500,000 | |
| 반환 되는 데이터의 최대 크기 | 6400만 바이트 (~ 61 MiB)| |
| 최대 쿼리 실행 시간 | 10분 | 자세한 내용은 [시간 제한](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) 을 참조 하세요.  |
| 최대 요청 빈도 | AAD 사용자 또는 클라이언트 IP 주소 당 30 초 당 200 요청 | 자세한 내용은 [요율 제한](https://dev.loganalytics.io/documentation/Using-the-API/Limits) 을 참조 하세요. |

**일반 작업 영역 제한**

| 범주 | 제한 | 주석 |
|:---|:---|:---|
| 테이블의 최대 열         | 500 | |
| 열 이름에 대 한 최대 문자 | 500 | |
| 데이터 내보내기 | 현재 사용할 수 없음 | Azure Function 또는 논리 앱을 사용 하 여 데이터를 집계 하 고 내보냅니다. | 

**데이터 수집 볼륨 요금**


Azure Monitor는 점점 더 빠른 속도로 매달 테라바이트 단위의 데이터를 보내는 수천 명의 고객을 처리하는 대규모 데이터 서비스입니다. [진단 설정을](../articles/azure-monitor/platform/diagnostic-settings.md) 사용 하 여 Azure 리소스에서 전송 된 데이터에 대 한 기본 수집 볼륨 요금은 작업 영역 당 약 **6gb/최소값** 입니다. 실제 크기는 로그 길이와 압축 비율에 따라 데이터 형식에 따라 달라질 수 있으므로이 값은 근사치입니다. 이 제한은 에이전트 또는 [데이터 수집기 API](../articles/azure-monitor/platform/data-collector-api.md)에서 전송 된 데이터에는 적용 되지 않습니다.

데이터를 더 높은 속도로 단일 작업 영역으로 보내는 경우 일부 데이터가 삭제 되 고, 임계값을 계속 초과 하는 동안 6 시간 마다 이벤트가 작업 영역에서 *작업* 테이블로 전송 됩니다. 수집 볼륨이 계속 해 서 속도 제한을 초과 하거나 곧 도달할 것으로 예상 되는 경우에는 전자 메일을 LAIngestionRate@microsoft.com 보내거나 지원 요청을 열어 작업 영역에 대 한 증가를 요청할 수 있습니다.
 
작업 영역에서 이러한 이벤트에 대해 알리려면 0 보다 많은 결과 클 경고 논리 기반을 사용 하는 다음 쿼리를 사용 하 여 [로그 경고 규칙](../articles/azure-monitor/platform/alerts-log.md) 을 만듭니다.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


>[!NOTE]
>Log Analytics 사용 하는 기간에 따라 레거시 가격 책정 계층에 대 한 액세스 권한이 있을 수 있습니다. [Log Analytics 레거시 가격 책정 계층](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers)에 대해 자세히 알아보세요. 
