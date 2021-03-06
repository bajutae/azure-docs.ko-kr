---
title: 대화형 요청 프롬프트 동작 (MSAL .js) | Microsoft
titleSuffix: Microsoft identity platform
description: JavaScript 용 Microsoft 인증 라이브러리 (MSAL .js)를 사용 하 여 대화형 호출에서 프롬프트 동작을 사용자 지정 하는 방법을 알아봅니다.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 778e89655019a49a30904fbe8d8e6aedf1833e9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76695979"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>MSAL .js 대화형 요청의 프롬프트 동작

사용자가 여러 사용자 계정을 사용 하 여 활성 Azure AD 세션을 설정 하면 Azure AD 로그인 페이지에서 사용자에 게 로그인을 계속 하기 전에 사용자에 게 계정을 선택 하 라는 메시지가 표시 됩니다. Azure AD에 인증 된 단일 세션만 있는 경우 사용자에 게 계정 선택 환경이 표시 되지 않습니다.

Msal .js 라이브러리 (v 0.2.4에서 시작)는 대화형 요청`loginRedirect`(, `loginPopup` `acquireTokenRedirect` 및 `acquireTokenPopup`) 중에 프롬프트 매개 변수를 보내지 않으므로 프롬프트 동작을 적용 하지 않습니다. 메서드를 사용 하는 `acquireTokenSilent` 자동 토큰 요청의 경우 msal는로 `none`설정 된 프롬프트 매개 변수를 전달 합니다.

응용 프로그램 시나리오에 따라 메서드에 전달 된 요청 매개 변수에서 prompt 매개 변수를 설정 하 여 대화형 요청에 대 한 프롬프트 동작을 제어할 수 있습니다. 예를 들어 계정 선택 환경을 호출 하려면 다음을 수행 합니다.

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


Azure AD를 사용 하 여 인증할 때 다음 프롬프트 값을 전달할 수 있습니다.

**로그인:** 이 값은 사용자가 인증 요청에 대 한 자격 증명을 입력 하도록 합니다.

**select_account:** 이 값은 세션의 모든 계정을 나열 하는 계정 선택 환경을 사용자에 게 제공 합니다.

**동의:** 이 값은 사용자가 앱에 대 한 사용 권한을 부여할 수 있도록 하는 OAuth 동의 대화 상자를 호출 합니다.

**없음:** 이 값을 사용 하면 사용자에 게 대화형 프롬프트가 표시 되지 않습니다. 예기치 않은 동작이 있을 수 있으므로이 값을 MSAL의 대화형 메서드에 전달 하지 않는 것이 좋습니다. 대신 `acquireTokenSilent` 메서드를 사용 하 여 자동 호출을 수행 합니다.

## <a name="next-steps"></a>다음 단계

MSAL .js 라이브러리 `prompt` 에서 사용 하는 [OAuth 2.0 암시적 허용](v2-oauth2-implicit-grant-flow.md) 프로토콜의 매개 변수에 대 한 자세한 내용을 참조 하세요.
