---
title: '빠른 시작: REST API 및 Node.js를 사용하여 원격 이미지 분석'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 Node.js와 함께 Computer Vision API를 사용하여 이미지를 분석합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18, seo-javascript-september2018, seo-javascript-october2019
ms.openlocfilehash: 0fada9129d3b05111b2988a9debb55c88414ae3f
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685119"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-with-nodejs"></a>빠른 시작: Node.js와 함께 Computer Vision REST API를 사용하여 원격 이미지 분석

이 빠른 시작에서는 Computer Vision REST API를 Node.js와 함께 사용하여 원격으로 저장된 이미지를 분석하여 시각적 기능을 추출합니다. [이미지 분석](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) 메서드를 사용하면 이미지 콘텐츠를 기반으로 하여 시각적 특징을 추출할 수 있습니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services)을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

- [Node.js](https://nodejs.org) 4.x 이상이 설치되어 있어야 합니다.
- [npm](https://www.npmjs.com/)이 설치되어 있어야 합니다.
- Computer Vision에 대한 구독 키가 있어야 합니다. [Cognitive Services 사용해보기](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)에서 평가판 키를 가져올 수 있습니다. 또는 [Cognitive Services 계정 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)의 지침에 따라 Computer Vision을 구독하고 키를 가져옵니다. 그런 다음, 각각 `COMPUTER_VISION_SUBSCRIPTION_KEY` 및 `COMPUTER_VISION_ENDPOINT`라는 키 및 서비스 엔드포인트 문자열에 대한 [환경 변수를 만듭니다](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication).

## <a name="create-and-run-the-sample"></a>샘플 만들기 및 실행

샘플을 만들고 실행하려면 다음 단계를 수행합니다.

1. npm [`request`](https://www.npmjs.com/package/request) 패키지를 설치합니다.
   1. 관리자로 명령 프롬프트 창을 엽니다.
   1. 다음 명령 실행:

      ```console
      npm install request
      ```

   1. 패키지가 성공적으로 설치되면 명령 프롬프트 창을 닫습니다.

1. 다음 코드를 텍스트 편집기에 복사합니다.
1. 필요에 따라 `imageUrl` 값을 분석하려는 다른 이미지의 URL로 바꿉니다.
1. 필요한 경우 `language` 요청 매개 변수의 값을 다른 언어로 바꿉니다.
1. 코드를 `.js` 확장명의 파일로 저장합니다. `analyze-image.js`)을 입력합니다.
1. 명령 프롬프트 창을 엽니다.
1. 프롬프트에서 `node` 명령을 사용하여 파일을 실행합니다. `node analyze-image.js`)을 입력합니다.

```javascript
'use strict';

const request = require('request');

let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']
if (!subscriptionKey) { throw new Error('Set your environment variables for your subscription key and endpoint.'); }

var uriBase = endpoint + 'vision/v3.0/analyze';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="examine-the-response"></a>응답 검사

성공적인 응답이 JSON을 통해 반환됩니다. 샘플은 다음 예제와 유사하게 명령 프롬프트 창에서 성공한 응답을 구문 분석하고 표시합니다.

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="clean-up-resources"></a>리소스 정리

더 이상 필요하지 않은 경우 파일을 삭제한 후 npm `request` 패키지를 제거합니다. 패키지를 제거하려면 다음 단계를 수행합니다.

1. 관리자로 명령 프롬프트 창을 엽니다.
2. 다음 명령 실행:

   ```console
   npm uninstall request
   ```

3. 패키지가 성공적으로 제거되면 명령 프롬프트 창을 닫습니다.

## <a name="next-steps"></a>다음 단계

다음으로, 이미지를 분석하고, 유명인 및 랜드마크를 검색하고, 썸네일을 만들고, 인쇄 및 필기 텍스트를 추출하는 데 사용되는 Computer Vision API를 살펴봅니다.

> [!div class="nextstepaction"]
> [Computer Vision API 살펴보기](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
