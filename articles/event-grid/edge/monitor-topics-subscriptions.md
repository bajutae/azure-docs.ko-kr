---
title: 항목 및 이벤트 구독 모니터링-Azure Event Grid IoT Edge | Microsoft Docs
description: 항목 및 이벤트 구독 모니터링
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ce7c92f121fb458d528d63d0af0aad025b377386
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77086665"
---
# <a name="monitor-topics-and-event-subscriptions"></a>항목 및 이벤트 구독 모니터링

Edge에서 Event Grid는 토픽 및 이벤트 구독에 대 한 여러 가지 메트릭을 [프로메테우스 표시 형식](https://prometheus.io/docs/instrumenting/exposition_formats/)으로 노출 합니다. 이 문서에서는 사용 가능한 메트릭과이를 사용 하도록 설정 하는 방법을 설명 합니다.

## <a name="enable-metrics"></a>메트릭 사용

컨테이너 만들기 옵션에서 `metrics__reporterType` 환경 변수를로 `prometheus` 설정 하 여 메트릭을 내보내도록 모듈을 구성 합니다.

 ```json
        {
          "Env": [
            "metrics__reporterType=prometheus"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
 ```    

메트릭은 http 및 `5888/metrics` `4438/metrics` https에 대 한 모듈에서 사용할 수 있습니다. 예를 `http://<modulename>:5888/metrics?api-version=2019-01-01-preview` 들어 http의 경우입니다. 이 시점에서 메트릭 모듈은이 [예제 아키텍처](https://github.com/veyalla/ehm)와 같이 메트릭을 수집 하도록 끝점을 폴링할 수 있습니다.

## <a name="available-metrics"></a>사용 가능한 메트릭

토픽 및 이벤트 구독은 모두 메트릭을 내보내 이벤트 전달 및 모듈 성능에 대 한 통찰력을 제공 합니다.

### <a name="topic-metrics"></a>토픽 메트릭

| 메트릭 | Description |
| ------ | ----------- |
| EventsReceived | 토픽에 게시 된 이벤트 수
| UnmatchedEvents | 이벤트 구독과 일치 하지 않고 삭제 된 항목에 게시 된 이벤트의 수입니다.
| SuccessRequests | 토픽에서 받은 인바운드 게시 요청 수입니다.
| SystemErrorRequests | 내부 시스템 오류로 인해 실패 한 인바운드 게시 요청 수입니다.
| UserErrorRequests | 잘못 된 JSON과 같은 사용자 오류로 인해 인바운드 게시 요청의 수가 실패 했습니다.
| SuccessRequestLatencyMs | 게시 요청 응답 대기 시간 (밀리초)


### <a name="event-subscription-metrics"></a>이벤트 구독 메트릭

| 메트릭 | Description |
| ------ | ----------- |
| DeliverySuccessCounts | 구성 된 끝점에 성공적으로 배달 된 이벤트 수
| DeliveryFailureCounts | 구성 된 끝점에 배달 하지 못한 이벤트의 수입니다.
| DeliverySuccessLatencyMs | 성공적으로 배달 된 이벤트의 대기 시간 (밀리초)
| DeliveryFailureLatencyMs | 이벤트 배달 실패의 대기 시간 (밀리초)
| SystemDelayForFirstAttemptMs | 첫 번째 배달 시도 전 이벤트의 시스템 지연 시간 (밀리초)
| DeliveryAttemptsCount | 이벤트 전달 시도 횟수-성공 및 실패
| ExpiredCounts | 구성 된 끝점에 전달 되지 않은 만료 된 이벤트 수