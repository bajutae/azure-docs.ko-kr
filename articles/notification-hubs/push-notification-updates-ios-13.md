---
title: Azure Notification Hubs iOS 13 업데이트 | Microsoft Docs
description: Azure Notification Hubs의 iOS 13 주요 변경 내용에 대해 알아봅니다.
author: sethmanheim
ms.author: sethm
ms.date: 10/16/2019
ms.topic: article
ms.service: notification-hubs
ms.reviewer: jowargo
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: 697e8ba9c9f27e8d5644e3a78950ff006290efe7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74228137"
---
# <a name="azure-notification-hubs-updates-for-ios-13"></a>IOS 13의 Azure Notification Hubs 업데이트

최근 Apple에서 공용 푸시 서비스를 일부 변경 했습니다. 변경 내용은 대부분 iOS 13 및 Xcode 릴리스에 맞춰져 있습니다. 이 문서에서는 Azure Notification Hubs에 대 한 이러한 변경의 영향을 설명 합니다.

## <a name="apns-push-payload-changes"></a>APNS 푸시 페이로드 변경

### <a name="apns-push-type"></a>APNS 푸시 유형

이제 Apple에서는 개발자가 APNS API의 새 `apns-push-type` 헤더를 통해 경고 또는 백그라운드 알림으로 알림을 식별 해야 합니다. [Apple의 설명서](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns)에 따라 "이 헤더의 값은 알림 페이로드의 내용을 정확 하 게 반영 해야 합니다. 일치 하지 않는 경우 또는 필요한 시스템에 헤더가 없는 경우 APNs에서 오류를 반환 하거나, 알림의 배달을 지연 하거나, 모두 삭제할 수 있습니다. "

이제 개발자는 Azure Notification Hubs를 통해 알림을 보내는 응용 프로그램에서이 헤더를 설정 해야 합니다. 기술 제한 사항으로 인해 고객은이 특성을 포함 하는 요청과 함께 APNS 자격 증명에 대 한 토큰 기반 인증을 사용 해야 합니다. APNS 자격 증명에 대해 인증서 기반 인증을 사용 하는 경우 토큰 기반 인증을 사용 하도록 전환 해야 합니다.

다음 코드 샘플에서는 Azure Notification Hubs를 통해 전송 되는 알림 요청에서이 헤더 특성을 설정 하는 방법을 보여 줍니다.

#### <a name="template-notifications---net-sdk"></a>템플릿 알림-.NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var tempprop = new Dictionary<string, string> {{"message", "value"}};
var notification = new TemplateNotification(tempprop);
notification.Headers = headers;
await hub.SendNotificationAsync(notification);
```

#### <a name="native-notifications---net-sdk"></a>네이티브 알림-.NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

#### <a name="direct-rest-calls"></a>직접 REST 호출

```csharp
var request = new HttpRequestMessage(method, $"<resourceUri>?api-version=2017-04");
request.Headers.Add("Authorization", createToken(resourceUri, KEY_NAME, KEY_VALUE));
request.Headers.Add("ServiceBusNotification-Format", "apple");
request.Headers.Add("apns-push-type", "alert");
```

이 전환 작업을 수행 하는 동안 Azure Notification Hubs에 `apns-push-type` 집합이 없는 알림이 감지 되 면 서비스는 알림 요청에서 푸시 유형을 유추 하 고 값을 자동으로 설정 합니다. 토큰 기반 인증을 사용 하 여 필요한 헤더를 설정 하도록 Azure Notification Hubs를 구성 해야 합니다. 자세한 내용은 [APNS에 대 한 토큰 기반 (HTTP/2) 인증](notification-hubs-push-notification-http2-token-authentification.md)을 참조 하세요.

## <a name="apns-priority"></a>APNS 우선 순위

또 다른 사소한 변경은 있지만 알림을 보내는 백 엔드 응용 프로그램을 변경 해야 하는 경우, 이제 `apns-priority` 헤더를 5로 설정 해야 한다는 요구 사항이 있습니다. 많은 응용 프로그램에서 `apns-priority` 헤더를 10으로 설정 하 여 (즉시 배달 표시), 설정 하지 않고 기본 값 (도 10)을 가져옵니다.

이 값을 10으로 설정 하면 백그라운드 알림에 더 이상 사용할 수 없으며 각 요청에 대 한 값을 설정 해야 합니다. 이 값이 없는 경우 Apple은 백그라운드 알림을 배달 하지 않습니다. 예를 들면 다음과 같습니다.

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "background"}, { "apns-priority", "5" }};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

## <a name="sdk-changes"></a>SDK 변경 내용

년 동안 iOS 개발자는 푸시 토큰 `description` 대리자에 전송 `deviceToken` 된 데이터의 특성을 사용 하 여 백 엔드 응용 프로그램에서 장치에 알림을 전송 하는 데 사용 하는 푸시 토큰을 추출 합니다. Xcode 11을 사용 하면 `description` 해당 특성이 다른 형식으로 변경 됩니다. 개발자가이 특성에 사용 하는 기존 코드는 이제 중단 됩니다. 이 변경 내용을 수용 하기 위해 Azure Notification Hubs SDK를 업데이트 했으므로 응용 프로그램에서 사용 하는 SDK를 최신 버전의 [azure Notification Hubs IOS SDK](https://github.com/Azure/azure-notificationhubs-ios)로 업데이트 하세요.
