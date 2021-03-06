---
title: 음성 컨테이너 설치
titleSuffix: Azure Cognitive Services
description: 텍스트 음성 변환 차트 구성 옵션에 대해 자세히 설명 합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 22168974ab8b285413b4fa6e947c05f65a73ae12
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80874353"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>텍스트 음성 변환 (하위 차트: 차트/textToSpeech)

"파라솔" 차트를 재정의 하려면 매개 변수에 접두사 `textToSpeech.` 를 추가 하 여 더 구체적으로 만듭니다. 예를 들어이 매개 변수는 재정의 `textToSpeech.numberOfConcurrentRequest` `numberOfConcurrentRequest`와 같이 해당 매개 변수를 재정의 합니다.

|매개 변수|Description|기본값|
| -- | -- | -- |
| `enabled` | **텍스트 음성 변환** 서비스의 사용 여부를 지정 합니다. | `false` |
| `numberOfConcurrentRequest` | **텍스트 음성 변환** 서비스에 대 한 동시 요청 수입니다. 이 차트는이 값에 따라 CPU 및 메모리 리소스를 자동으로 계산 합니다. | `2` |
| `optimizeForTurboMode`| 텍스트 파일을 통해 서비스에서 텍스트 입력을 최적화 해야 하는지 여부입니다. 인 `true`경우이 차트는 서비스에 더 많은 CPU 리소스를 할당 합니다. | `false` |
| `image.registry`| **텍스트 음성 변환** docker 이미지 레지스트리 | `containerpreview.azurecr.io` |
| `image.repository` | **텍스트 음성 변환** docker 이미지 리포지토리입니다. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | **텍스트 음성 변환** docker 이미지 태그입니다. | `latest` |
| `image.pullSecrets` | **텍스트 음성 변환** docker 이미지를 가져오기 위한 이미지 비밀입니다. | |
| `image.pullByHash`| Docker 이미지를 해시로 끌어올 지 여부를 나타냅니다. 인 `true`경우 `image.hash` 가 필요 합니다. | `false` |
| `image.hash`| **텍스트 음성 변환** docker 이미지 해시입니다. 인 경우 `image.pullByHash: true`에만 사용 됩니다.  | |
| `image.args.eula`(필수) | 라이선스를 수락 했음을 나타냅니다. 유일 하 게 유효한 값은`accept` | |
| `image.args.billing`(필수) | 청구 끝점 URI 값은 Azure Portal의 음성 개요 페이지에서 사용할 수 있습니다. | |
| `image.args.apikey`(필수) | 청구 정보를 추적하는 데 사용됩니다. ||
| `service.type` | **텍스트 음성 변환** 서비스의 Kubernetes 서비스 유형입니다. 자세한 내용은 [Kubernetes service types 지침](https://kubernetes.io/docs/concepts/services-networking/service/) 및 클라우드 공급자 지원 확인을 참조 하세요. | `LoadBalancer` |
| `service.port`|  **텍스트 음성 변환** 서비스의 포트입니다. | `80` |
| `service.annotations` | 서비스 메타 데이터에 대 한 **텍스트 음성 변환** 주석입니다. 주석은 키 값 쌍입니다. <br>`annotations:`<br>&nbsp;&nbsp;`some/annotation1: value1`<br>&nbsp;&nbsp;`some/annotation2: value2` | |
| `service.autoScaler.enabled` | [수평 Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) 사용 되는지 여부를 나타냅니다. 인 `true`경우 Kubernetes `text-to-speech-autoscaler` 클러스터에가 배포 됩니다. | `true` |
| `service.podDisruption.enabled` | [Pod 중단 예산이](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) 사용 되는지 여부를 나타냅니다. 인 `true`경우 Kubernetes `text-to-speech-poddisruptionbudget` 클러스터에가 배포 됩니다. | `true` |