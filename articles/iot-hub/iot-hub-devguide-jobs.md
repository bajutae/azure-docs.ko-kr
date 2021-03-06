---
title: Azure IoT Hub 작업 이해 | Microsoft 문서
description: 개발자 가이드 - IoT 허브에 연결된 여러 디바이스에서 실행할 작업 예약 작업은 tags와 desired 속성을 업데이트하고 여러 디바이스에서 직접 메서드를 호출할 수 있습니다.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: mqtt
ms.openlocfilehash: 5c14e8cfcbf8df86b0f71d6b12025594d2e648c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81730099"
---
# <a name="schedule-jobs-on-multiple-devices"></a>여러 디바이스에서 작업 예약

Azure IoT Hub를 통해 [디바이스 쌍 속성 및 태그](iot-hub-devguide-device-twins.md) 및 [직접 메서드](iot-hub-devguide-direct-methods.md)와 같은 다양한 구성 요소를 사용할 수 있습니다. 일반적으로, 백 엔드 앱을 사용하면 디바이스 관리자와 운영자는 IoT 디바이스를 대량으로 예약된 시간에 업데이트하고 상호 작용할 수 있습니다. 작업은 예약된 시간에 디바이스 집합에 대해 디바이스 쌍 업데이트 및 직접 메서드를 실행합니다. 예를 들어 운영자는 빌딩 운영에 지장을 주지 않는 시간에 빌딩 43 및 3층에서 디바이스 집합을 재부팅하기 위해 작업을 시작 및 추적하는 백 엔드 앱을 사용합니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

디바이스 집합에서 다음 작업을 예약하고 진행 상태를 추적해야 하는 경우 작업을 사용하는 것이 좋습니다.

* desired 속성 업데이트
* tags 업데이트
* 직접 메서드 호출

## <a name="job-lifecycle"></a>작업 수명 주기

작업은 솔루션 백 엔드에 의해 시작되고 IoT Hub에 의해 유지 관리됩니다. 서비스 지향 URI(`PUT https://<iot hub>/jobs/v2/<jobID>?api-version=2018-06-30`)를 통해 작업을 시작하고 서비스 지향 URI(`GET https://<iot hub>/jobs/v2/<jobID?api-version=2018-06-30`)를 통해 작업 실행 진행 상태를 쿼리할 수 있습니다. 작업이 시작된 후 실행 중인 작업의 상태를 새로 고치기 위해 작업 쿼리를 실행합니다.

> [!NOTE]
> 작업을 시작할 때 속성 이름과 값은 US-ASCII로 출력 가능한 영숫자만 포함할 수 있으며 다음 집합은 제외됩니다. `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`

## <a name="jobs-to-execute-direct-methods"></a>직접 메서드를 실행할 작업

다음 코드 조각은 작업을 사용하여 디바이스 집합에서 [직접 메서드](iot-hub-devguide-direct-methods.md)를 실행하기 위한 HTTPS 1.1 요청 세부 정보를 보여 줍니다.

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "<methodName>",
        "payload": <payload>,
        "responseTimeoutInSeconds": methodTimeoutInSeconds
    },
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

쿼리 조건은 다음 예제와 같이 단일 디바이스 ID 또는 디바이스 ID 목록에 있을 수도 있습니다.

```
"queryCondition" = "deviceId = 'MyDevice1'"
"queryCondition" = "deviceId IN ['MyDevice1','MyDevice2']"
"queryCondition" = "deviceId IN ['MyDevice1']"
```

[IoT Hub 쿼리 언어](iot-hub-devguide-query-language.md)에 IoT Hub 쿼리 언어의 더 자세한 설명이 나와 있습니다.

다음 코드 조각은 contoso-hub-1의 모든 장치에서 testMethod 라는 직접 메서드를 호출 하도록 예약 된 작업에 대 한 요청 및 응답을 보여 줍니다.

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job01?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=68iv------------------------------------v8Hxalg%3D&se=1556849884&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 317

{
    "jobId": "job01",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "testMethod",
        "payload": {},
        "responseTimeoutInSeconds": 30
    },
    "queryCondition": "*", 
    "startTime": "2019-05-04T15:53:00.077Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 65
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 01:46:18 GMT

{"jobId":"job01","type":"scheduleDeviceMethod","status":"queued"}
```

## <a name="jobs-to-update-device-twin-properties"></a>디바이스 쌍 속성을 업데이트하는 작업

다음 코드 조각은 작업을 사용하여 디바이스 쌍 속성을 업데이트하는 HTTPS 1.1 요청 세부 정보를 나타냅니다.

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleUpdateTwin",
    "updateTwin": <patch>                 // Valid JSON object
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

> [!NOTE]
> *Updatetwin* 속성에는 유효한 etag 일치가 필요 합니다. 예를 `etag="*"`들면입니다.

다음 코드 조각은 contoso-hub-1에서 테스트 장치에 대 한 장치 쌍 속성을 업데이트 하도록 예약 된 작업에 대 한 요청 및 응답을 보여 줍니다.

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job02?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=BN0U-------------------------------------RuA%3D&se=1556925787&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 339

{
    "jobId": "job02",
    "type": "scheduleUpdateTwin",
    "updateTwin": {
      "properties": {
        "desired": {
          "test1": "value1"
        }
      },
     "etag": "*"
     },
    "queryCondition": "deviceId = 'test-device'",
    "startTime": "2019-05-08T12:19:56.868Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 63
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 22:45:13 GMT

{"jobId":"job02","type":"scheduleUpdateTwin","status":"queued"}
```

## <a name="querying-for-progress-on-jobs"></a>작업 진행 상태 쿼리

다음 코드 조각은 작업 쿼리를 위한 HTTPS 1.1 요청 세부 정보를 나타냅니다.

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
```

응답에서 continuationToken이 제공됩니다.

[디바이스 쌍, 작업 및 메시지 라우팅에 대한 IoT Hub 쿼리 언어](iot-hub-devguide-query-language.md)를 사용하여 각 디바이스에서 작업 실행 상태를 쿼리할 수 있습니다.

## <a name="jobs-properties"></a>작업 속성

다음 목록은 작업 또는 작업 결과를 쿼리할 때 사용할 수 있는 속성 목록 및 해당 설명을 나타냅니다.

| 속성 | Description |
| --- | --- |
| **Id** |작업에 대해 애플리케이션에서 제공한 ID입니다. |
| **startTime** |작업에 대해 애플리케이션에서 제공한 시작 시간(ISO-8601)입니다. |
| **endTime** |작업이 완료될 때 IoT Hub에서 제공한 날짜(ISO-8601)입니다. 작업이 '완료됨' 상태에 도달한 후에만 유효합니다. |
| **type** |작업 형식: |
| | **scheduleUpdateTwin**: desired 속성 또는 태그 집합을 업데이트 하는 데 사용 되는 작업입니다. |
| | **scheduleDeviceMethod**: 장치 쌍 집합에서 장치 메서드를 호출 하는 데 사용 되는 작업입니다. |
| **status** |작업의 현재 상태입니다. 가능한 상태 값: |
| | **보류 중** : 예약되어 작업 서비스에서 선택되기를 기다립니다. |
| | **예약됨** : 이후 시간에 예약됩니다. |
| | **실행 중** : 현재 활성 상태의 작업입니다. |
| | **취소**: 작업이 취소되었습니다. |
| | **실패함** : 작업이 실패했습니다. |
| | **완료됨** : 작업이 완료되었습니다. |
| **deviceJobStatistics** |작업 실행에 대한 통계입니다. |
| | **Devicejobstatistics** 속성: |
| | **deviceJobStatistics.deviceCount**: 작업의 디바이스 수입니다. |
| | **deviceJobStatistics.failedCount**: 작업이 실패한 디바이스 수입니다. |
| | **deviceJobStatistics.succeededCount**: 작업이 성공한 디바이스 수입니다. |
| | **deviceJobStatistics.runningCount**: 현재 작업을 실행 중인 디바이스 수입니다. |
| | **deviceJobStatistics.pendingCount**: 작업 실행이 보류 중인 디바이스 수입니다. |

### <a name="additional-reference-material"></a>추가 참조 자료

이 IoT Hub 개발자 가이드의 다른 참조 자료:

* [IoT Hub 엔드포인트](iot-hub-devguide-endpoints.md) - 각 IoT Hub에서 런타임 및 관리 작업에 대해 공개하는 다양한 엔드포인트에 대해 설명합니다.

* [제한 및 할당량](iot-hub-devguide-quotas-throttling.md) - IoT Hub 서비스에 적용되는 할당량과 서비스를 사용할 때 예상되는 제한 동작에 대해 설명합니다.

* [Azure IoT 디바이스 및 서비스 SDK](iot-hub-devguide-sdks.md) - IoT Hub와 상호 작용하는 디바이스 및 서비스 앱 모두를 개발할 때 사용할 수 있는 다양한 언어 SDK를 나열합니다.

* [디바이스 쌍, 작업 및 메시지 라우팅용 IoT Hub 쿼리 언어](iot-hub-devguide-query-language.md)에서는 IoT Hub 쿼리 언어를 설명합니다. 이 쿼리 언어를 사용하여 IoT Hub에서 디바이스 쌍 및 작업에 대한 정보를 검색합니다.

* [IoT Hub MQTT 지원](iot-hub-mqtt-support.md) - MQTT 프로토콜에 대한 IoT Hub 지원에 대해 자세히 설명합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 설명한 일부 개념을 시도해 보려면 다음과 같은 IoT Hub 자습서를 참조하세요.

* [작업 예약 및 브로드캐스트](iot-hub-node-node-schedule-jobs.md)