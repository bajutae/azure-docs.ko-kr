---
title: LUIS 미리 작성 한 엔터티 전자 메일 참조
titleSuffix: Azure Cognitive Services
description: 이 아티클에는 LUIS(Language Understanding)의 이메일 미리 빌드된 엔터티가 포함됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: f7aa2a341a82d30f7022af49e626a3cd358854b7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273474"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>LUIS 앱용 이메일 미리 빌드된 엔터티
이메일 추출에는 발언의 전체 이메일 주소가 포함됩니다. 이 엔터티를 이미 학습했기 때문에 이메일을 포함하는 예제 발언을 애플리케이션 의도에 추가할 필요가 없습니다. 이메일 엔터티는 `en-us` 문화권에서만 지원됩니다.

## <a name="resolution-for-prebuilt-email"></a>미리 빌드된 이메일의 해결

쿼리에 대해 반환 되는 엔터티 개체는 다음과 같습니다.

`please send the information to patti@contoso.com`

#### <a name="v3-response"></a>[V3 응답](#tab/V3)

다음 JSON은 `verbose` 매개 변수를로 `false`설정 하는입니다.

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 자세한 정보 표시 응답](#tab/V3-verbose)

다음 JSON은 `verbose` 매개 변수를로 `true`설정 하는입니다.

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ],
    "$instance": {
        "email": [
            {
                "type": "builtin.email",
                "text": "patti@contoso.com",
                "startIndex": 31,
                "length": 17,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 응답](#tab/V2)

다음 예제에서는 **builtin.email** 엔터티의 해결을 보여줍니다.

```json
"entities": [
    {
        "entity": "patti@contoso.com",
        "type": "builtin.email",
        "startIndex": 31,
        "endIndex": 55,
        "resolution": {
        "value": "patti@contoso.com"
        }
    }
]
```
* * *

## <a name="next-steps"></a>다음 단계

[V3 예측 엔드포인트](luis-migration-api-v3.md)에 대해 자세히 알아봅니다.

[number](luis-reference-prebuilt-number.md), [ordinal](luis-reference-prebuilt-ordinal.md) 및 [percentage](luis-reference-prebuilt-percentage.md)에 대해 알아봅니다.
