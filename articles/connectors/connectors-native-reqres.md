---
title: HTTPS를 사용 하 여 호출 수신 및 응답
description: Azure Logic Apps를 사용 하 여 외부 서비스에서 인바운드 HTTPS 요청 처리
services: logic-apps
ms.suite: integration
ms.reviewers: jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/06/2020
tags: connectors
ms.openlocfilehash: c6d8dc087e6306173fc4d55368cd3c4c624d5302
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82978572"
---
# <a name="receive-and-respond-to-inbound-https-requests-in-azure-logic-apps"></a>Azure Logic Apps에서 인바운드 HTTPS 요청을 수신 하 고 응답 합니다.

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) 및 기본 제공 요청 트리거 및 응답 작업을 사용 하 여 들어오는 HTTPS 요청을 수신 하 고 응답 하는 자동화 된 작업 및 워크플로를 만들 수 있습니다. 예를 들어 논리 앱을 사용할 수 있습니다.

* 온-프레미스 데이터베이스에서 데이터에 대 한 HTTPS 요청을 수신 하 고 응답 합니다.
* 외부 webhook 이벤트가 발생 하는 경우 워크플로를 트리거합니다.
* 다른 논리 앱에서 HTTPS 호출을 받고 응답 합니다.

요청 트리거는 논리 앱에 대 한 인바운드 호출에 대 한 권한을 부여 하기 위해 Azure AD OAuth ( [Azure Active Directory Open Authentication](../active-directory/develop/about-microsoft-identity-platform.md) )를 지원 합니다. 이 인증을 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 [AZURE AD OAuth 인증 사용 Azure Logic Apps에서 보안 액세스 및 데이터](../logic-apps/logic-apps-securing-a-logic-app.md#enable-oauth)를 참조 하세요.

> [!NOTE]
> 요청 트리거는 들어오는 호출에 TLS (전송 계층 보안) 1.2 *만* 지원 합니다. 보내는 호출은 TLS 1.0, 1.1 및 1.2을 지원 합니다. 자세한 내용은 [TLS 1.0 문제 해결](https://docs.microsoft.com/security/solving-tls1-problem)을 참조 하세요.
>
> TLS 핸드셰이크 오류가 발생 하는 경우 TLS 1.2를 사용 해야 합니다. 
> 들어오는 호출의 경우 지원 되는 암호 그룹은 다음과 같습니다.
>
> * TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 구독이 없는 경우 [무료 Azure 계정에 등록할](https://azure.microsoft.com/free/)수 있습니다.

* [논리 앱](../logic-apps/logic-apps-overview.md)에 대 한 기본 지식. 논리 앱을 처음 접하는 경우 [첫 번째 논리 앱을 만드는 방법](../logic-apps/quickstart-create-first-logic-app-workflow.md)에 대해 알아보세요.

<a name="add-request"></a>

## <a name="add-request-trigger"></a>요청 트리거 추가

이 기본 제공 트리거 *는 들어오는 https* 요청만 수신할 수 있는 수동으로 호출할 수 있는 https 끝점을 만듭니다. 이 이벤트가 발생 하면 트리거가 발생 하 고 논리 앱을 실행 합니다. 트리거의 기본 JSON 정의 및이 트리거를 호출 하는 방법에 대 한 자세한 내용은 [요청 트리거 형식](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) 및 [HTTPS 끝점을 사용 하 여 Azure Logic Apps에서 워크플로 호출, 트리거 또는 중첩](../logic-apps/logic-apps-http-endpoint.md)을 참조 하세요.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다. 빈 논리 앱을 만듭니다.

1. 논리 앱 디자이너가 열리면 검색 상자에를 필터로 입력 `http request` 합니다. 트리거 목록에서 논리 앱 워크플로의 첫 단계인 **HTTP 요청을 받을 때** 트리거를 선택 합니다.

   ![요청 트리거 선택](./media/connectors-native-reqres/select-request-trigger.png)

   요청 트리거는 다음과 같은 속성을 표시 합니다.

   ![요청 트리거](./media/connectors-native-reqres/request-trigger.png)

   | 속성 이름 | JSON 속성 이름 | 필수 | Description |
   |---------------|--------------------|----------|-------------|
   | **HTTP POST URL** | {없음} | 예 | 논리 앱을 저장 한 후에 생성 되 고 논리 앱을 호출 하는 데 사용 되는 끝점 URL입니다. |
   | **요청 본문 JSON 스키마** | `schema` | 예 | 들어오는 요청 본문의 속성 및 값을 설명 하는 JSON 스키마입니다. |
   |||||

1. 필요에 따라 **본문 Json 스키마 요청** 상자에서 들어오는 요청의 본문을 설명 하는 JSON 스키마를 입력 합니다. 예를 들면 다음과 같습니다.

   ![JSON 스키마 예제](./media/connectors-native-reqres/provide-json-schema.png)

   디자이너는이 스키마를 사용 하 여 요청에서 속성에 대 한 토큰을 생성 합니다. 이렇게 하면 논리 앱에서 트리거를 통해 요청에서 데이터를 구문 분석 하 고, 사용 하 고, 전달할 수 있습니다.

   샘플 스키마는 다음과 같습니다.

   ```json
   {
      "type": "object",
      "properties": {
         "account": {
            "type": "object",
            "properties": {
               "name": {
                  "type": "string"
               },
               "ID": {
                  "type": "string"
               },
               "address": {
                  "type": "object",
                  "properties": {
                     "number": {
                        "type": "string"
                     },
                     "street": {
                        "type": "string"
                     },
                     "city": {
                        "type": "string"
                     },
                     "state": {
                        "type": "string"
                     },
                     "country": {
                        "type": "string"
                     },
                     "postalCode": {
                        "type": "string"
                     }
                  }
               }
            }
         }
      }
   }
   ```

   JSON 스키마를 입력 하면 요청에 헤더를 `Content-Type` 포함 하 고 해당 헤더 값을로 `application/json`설정 하는 미리 알림이 디자이너에 표시 됩니다. 자세한 내용은 [콘텐츠 형식 처리](../logic-apps/logic-apps-content-type.md)를 참조 하세요.

   !["Content-type" 헤더를 포함 하는 미리 알림](./media/connectors-native-reqres/include-content-type.png)

   JSON 형식의이 헤더는 다음과 같습니다.

   ```json
   {
      "Content-Type": "application/json"
   }
   ```

   예상 페이로드 (데이터)를 기반으로 하는 JSON 스키마를 생성 하려면 [JSONSchema.net](https://jsonschema.net)와 같은 도구를 사용 하거나 다음 단계를 수행할 수 있습니다.

   1. 요청 트리거에서 **샘플 페이로드를 사용하여 스키마 생성**을 선택합니다.

      ![페이로드에서 스키마 생성](./media/connectors-native-reqres/generate-from-sample-payload.png)

   1. 샘플 페이로드를 입력 하 고 **완료**를 선택 합니다.

      ![페이로드에서 스키마 생성](./media/connectors-native-reqres/enter-payload.png)

      샘플 페이로드는 다음과 같습니다.

      ```json
      {
         "account": {
            "name": "Contoso",
            "ID": "12345",
            "address": { 
               "number": "1234",
               "street": "Anywhere Street",
               "city": "AnyTown",
               "state": "AnyState",
               "country": "USA",
               "postalCode": "11111"
            }
         }
      }
      ```

1. 추가 속성을 지정 하려면 **새 매개 변수 추가** 목록을 열고 추가 하려는 매개 변수를 선택 합니다.

   | 속성 이름 | JSON 속성 이름 | 필수 | Description |
   |---------------|--------------------|----------|-------------|
   | **방법이** | `method` | 예 | 들어오는 요청에서 논리 앱을 호출 하는 데 사용 해야 하는 메서드입니다. |
   | **상대 경로** | `relativePath` | 예 | 논리 앱의 끝점 URL에서 수락할 수 있는 매개 변수의 상대 경로입니다. |
   |||||

   이 예제에서는 **메서드** 속성을 추가 합니다.

   ![메서드 매개 변수 추가](./media/connectors-native-reqres/add-parameters.png)

   **메서드** 속성은 목록에서 메서드를 선택할 수 있도록 트리거에 표시 됩니다.

   ![Select 메서드](./media/connectors-native-reqres/select-method.png)

1. 이제 워크플로의 다음 단계로 다른 작업을 추가 합니다. 트리거 아래에서 추가 하려는 작업을 찾을 수 있도록 **다음 단계** 를 선택 합니다.

   예를 들어 사용자 지정 된 응답을 반환 하는 데 사용할 수 있으며이 항목의 뒷부분에서 설명 하는 [응답 작업을 추가](#add-response)하 여 요청에 응답할 수 있습니다.

   논리 앱은 1 분 동안만 들어오는 요청을 열어 둡니다. 논리 앱 워크플로에 응답 작업이 포함 되어 있다고 가정 하면 논리 앱이이 시간 경과 후 응답을 반환 하지 않는 경우 논리 앱은를 `504 GATEWAY TIMEOUT` 호출자에 게 반환 합니다. 그렇지 않고 논리 앱에 응답 동작이 포함 되지 않은 경우 논리 앱은 즉시 호출자에 게 `202 ACCEPTED` 응답을 반환 합니다.

1. 완료되면 논리 앱을 저장합니다. 디자이너 도구 모음에서 **저장**을 선택합니다.

   이 단계에서는 논리 앱을 트리거하는 요청을 보내는 데 사용할 URL을 생성 합니다. 이 URL을 복사 하려면 URL 옆에 있는 복사 아이콘을 선택 합니다.

   ![논리 앱을 트리거하는 데 사용할 URL](./media/connectors-native-reqres/generated-url.png)

1. 논리 앱을 트리거하려면 생성 된 URL에 HTTP POST를 보냅니다.

   예를 들어 [Postman](https://www.getpostman.com/) 과 같은 도구를 사용 하 여 HTTP POST를 보낼 수 있습니다. 요청 트리거에 대 한 인바운드 호출에 대 한 권한 부여를 위해 인증 (Azure AD OAuth) [Azure Active Directory 열기를 사용 하도록 설정한](../logic-apps/logic-apps-securing-a-logic-app.md#enable-oauth) 경우 [SAS (공유 액세스 서명) URL](../logic-apps/logic-apps-securing-a-logic-app.md#sas) 을 사용 하거나 인증 토큰을 사용 하 여 트리거를 호출 하지만 둘 다 사용할 수는 없습니다. 인증 토큰은 권한 부여 헤더 `Bearer` 에 형식을 지정 해야 합니다. 자세한 내용은 [Azure Logic Apps의 보안 액세스 및 데이터-요청 기반 트리거에](../logic-apps/logic-apps-securing-a-logic-app.md#secure-triggers)대 한 액세스를 참조 하세요.

트리거의 기본 JSON 정의 및이 트리거를 호출 하는 방법에 대 한 자세한 내용은 다음 항목을 참조 하세요. [요청 트리거 형식](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) 및 [호출, 트리거 또는 중첩 워크플로를 사용 하 여 Azure Logic Apps](../logic-apps/logic-apps-http-endpoint.md)합니다.

### <a name="trigger-outputs"></a>트리거 출력

요청 트리거의 출력에 대 한 자세한 내용은 다음과 같습니다.

| JSON 속성 이름 | 데이터 형식 | Description |
|--------------------|-----------|-------------|
| `headers` | Object | 요청의 헤더를 설명 하는 JSON 개체입니다. |
| `body` | Object | 요청의 본문 콘텐츠를 설명 하는 JSON 개체입니다. |
||||

<a name="add-response"></a>

## <a name="add-a-response-action"></a>응답 동작 추가

응답 작업을 사용 하 여 들어오는 HTTPS 요청에 대 한 페이로드 (데이터)에 응답 하 고 HTTPS 요청에 의해 트리거되는 논리 앱 에서만 응답을 받을 수 있습니다. 워크플로의 어떤 지점에서 든 응답 작업을 추가할 수 있습니다. 이 트리거에 대 한 기본 JSON 정의에 대 한 자세한 내용은 [응답 작업 형식](../logic-apps/logic-apps-workflow-actions-triggers.md#response-action)을 참조 하세요.

논리 앱은 1 분 동안만 들어오는 요청을 열어 둡니다. 논리 앱 워크플로에 응답 작업이 포함 되어 있다고 가정 하면 논리 앱이이 시간 경과 후 응답을 반환 하지 않는 경우 논리 앱은를 `504 GATEWAY TIMEOUT` 호출자에 게 반환 합니다. 그렇지 않고 논리 앱에 응답 동작이 포함 되지 않은 경우 논리 앱은 즉시 호출자에 게 `202 ACCEPTED` 응답을 반환 합니다.

> [!IMPORTANT]
> 응답 작업에 이러한 헤더가 포함 된 경우 Logic Apps는 경고 또는 오류를 표시 하지 않고 생성 된 응답 메시지에서 이러한 헤더를 제거 합니다.
>
> * `Allow`
> * `Content-*`다음 예외 발생: `Content-Disposition`, `Content-Encoding`및`Content-Type`
> * `Cookie`
> * `Expires`
> * `Last-Modified`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps이 헤더를 사용 하 여 응답 작업이 있는 논리 앱을 저장 하는 것을 중지 하지는 않지만 Logic Apps는 이러한 헤더를 무시 합니다.

1. 논리 앱 디자이너에서 응답 작업을 추가 하려는 단계 아래에 있는 **새 단계**를 선택 합니다.

   예를 들어 앞에서 요청 트리거를 사용 합니다.

   ![새 단계 추가](./media/connectors-native-reqres/add-response.png)

   단계 사이에 작업을 추가 하려면 해당 단계 사이의 화살표 위로 포인터를 이동 합니다. 표시 되는 더하기 기호**+**()를 선택 하 고 **작업 추가**를 선택 합니다.

1. **작업 선택**아래의 검색 상자에 "응답"을 필터로 입력 하 고 **응답** 작업을 선택 합니다.

   ![응답 작업을 선택 합니다.](./media/connectors-native-reqres/select-response-action.png)

   이 예제에서는 편의상 요청 트리거가 축소 되어 있습니다.

1. 응답 메시지에 필요한 값을 추가 합니다. 

   일부 필드에서 상자 내부를 클릭 하면 동적 콘텐츠 목록이 열립니다. 그런 다음 워크플로의 이전 단계에서 사용 가능한 출력을 나타내는 토큰을 선택할 수 있습니다. 이전 예제에 지정 된 스키마의 속성은 이제 동적 콘텐츠 목록에 표시 됩니다.

   예를 들어 **헤더** 상자의 경우이 항목의 `Content-Type` 앞부분에서 설명한 대로 키 이름으로를 포함 하 고 `application/json` 키 값을로 설정 합니다. **본문** 상자의 동적 콘텐츠 목록에서 트리거 본문 출력을 선택할 수 있습니다.

   ![응답 작업 세부 정보](./media/connectors-native-reqres/response-details.png)

   JSON 형식의 헤더를 보려면 **텍스트 뷰로 전환**을 선택 합니다.

   ![머리글-텍스트 뷰로 전환](./media/connectors-native-reqres/switch-to-text-view.png)

   응답 작업에서 설정할 수 있는 속성에 대 한 자세한 내용은 다음과 같습니다. 

   | 속성 이름 | JSON 속성 이름 | 필수 | Description |
   |---------------|--------------------|----------|-------------|
   | **상태 코드** | `statusCode` | 예 | 응답에 반환할 상태 코드 |
   | **헤더** | `headers` | 예 | 응답에 포함할 하나 이상의 헤더를 설명 하는 JSON 개체입니다. |
   | **본문** | `body` | 예 | 응답 본문 |
   |||||

1. 응답 본문에 대 한 JSON 스키마와 같은 추가 속성을 지정 하려면 **새 매개 변수 추가** 목록을 열고 추가 하려는 매개 변수를 선택 합니다.

1. 완료되면 논리 앱을 저장합니다. 디자이너 도구 모음에서 **저장**을 선택합니다. 

## <a name="next-steps"></a>다음 단계

* [Logic Apps용 커넥터](../connectors/apis-list.md)