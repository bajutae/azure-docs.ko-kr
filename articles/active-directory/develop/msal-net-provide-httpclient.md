---
title: HttpClient & 프록시 제공 (MSAL.NET) | Microsoft
titleSuffix: Microsoft identity platform
description: Microsoft Authentication Library for .NET(MSAL.NET)을 사용하여 Azure AD에 연결하는 자체 HttpClient 및 프록시를 제공하는 방법에 대해 알아보세요.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: dbf08e23b2bc1f657363f69df55763437e6c8a90
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76695055"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>MSAL.NET을 사용하여 자체 HttpClient 및 프록시 제공
[공용 클라이언트 애플리케이션을 초기화](msal-net-initializing-client-applications.md)할 때 `.WithHttpClientFactory method`를 사용하여 자체 HttpClient를 제공할 수 있습니다.  자체 HttpClient를 제공하면 HTTP 프록시 세부 제어, 사용자 에이전트 헤더 사용자 지정 또는 MSAL에서 특정 HttpClient를 사용하도록 강제(예: ASP.NET Core 웹앱/API에서)와 같은 고급 시나리오가 지원됩니다.

## <a name="initialize-with-httpclientfactory"></a>HttpClientFactory를 사용하여 초기화
다음 예제에서는 `HttpClientFactory`를 만든 다음, 이를 통해 공용 클라이언트 애플리케이션을 초기화하는 방법을 보여줍니다.

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient 및 Xamarin iOS
Xamarin iOS를 사용할 때는 iOS 7 및 최신 버전용 `NSURLSession` 기반 처리기를 명시적으로 사용하는 `HttpClient`를 만드는 것이 좋습니다. MSAL.NET은 iOS 7 및 최신 버전용 `HttpClient`를 사용하는 `NSURLSessionHandler`를 자동으로 만듭니다. 자세한 내용은 [HttpClient에 대한 Xamarin iOS 문서](/xamarin/cross-platform/macios/http-stack)를 참조하세요.