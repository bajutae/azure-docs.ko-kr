---
title: '빠른 시작: 마이크, Swift에서 음성 인식 - Speech Service'
titleSuffix: Azure Cognitive Services
description: Speech SDK를 사용하여 iOS의 Swift에서 음성을 인식하는 방법 알아보기
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: 31f5339c70d52019400ca5f1fe873de4790a3bd6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75380527"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-by-using-the-speech-sdk"></a>빠른 시작: Speech SDK를 사용하여 iOS의 Swift에서 음성 인식

빠른 시작은 [음성 합성](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/swift-ios.md)에도 사용할 수 있습니다.

이 문서에서는 Azure Cognitive Services Speech SDK를 사용하여 Swift에서 마이크로 녹음한 음성을 텍스트로 변환하는 iOS 앱을 만드는 방법을 알아봅니다.

## <a name="prerequisites"></a>사전 요구 사항

시작하기 전에 다음 항목이 필요합니다.

* Speech Service에 대한 [구독 키](~/articles/cognitive-services/Speech-Service/get-started.md)입니다.
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) 이상 및 [CocoaPods](https://cocoapods.org/)가 설치된 macOS 머신

## <a name="get-the-speech-sdk-for-ios"></a>iOS용 Speech SDK 가져오기

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

이 자습서는 1.6.0 이전 버전의 SDK에서는 작동하지 않습니다.

iOS용 Cognitive Services Speech SDK는 프레임워크 번들로 배포됩니다. Xcode 프로젝트에서 [CocoaPod](https://cocoapods.org/)로 사용하거나, https://aka.ms/csspeech/iosbinary 에서 다운로드하거나, 수동으로 연결할 수 있습니다. 이 문서에서는 CocoaPod를 사용합니다.

## <a name="create-an-xcode-project"></a>Xcode 프로젝트 만들기

Xcode를 시작하고, **파일** > **새로 만들기** > **프로젝트**를 선택하여 새 프로젝트를 시작합니다.
템플릿 선택 대화 상자에서 **iOS 단일 보기 앱** 템플릿을 선택합니다.

뒤따르는 대화 상자에서 다음과 같이 선택합니다.

1. **프로젝트 옵션** 대화 상자에서:
    1. 빠른 시작 앱의 이름을 입력합니다(예: *helloworld*).
    1. Apple 개발자 계정이 이미 있는 경우 적절한 조직 이름과 조직 식별자를 입력합니다. 테스트를 위해 *testorg*와 같은 이름을 사용합니다. 앱에 서명하려면 적절한 프로비전 프로필이 필요합니다. 자세한 내용은 [Apple 개발자 사이트](https://developer.apple.com/)를 참조하세요.
    1. 프로젝트에 대한 언어로 **Swift**가 선택되어야 합니다.
    1. 스토리보드를 사용하고 문서 기반 애플리케이션을 만들려면 확인란을 사용하지 않도록 설정합니다. 샘플 앱에 대한 간단한 UI는 프로그래밍 방식으로 생성됩니다.
    1. 테스트 및 핵심 데이터에 대한 모든 확인란의 선택을 취소합니다.
1. 프로젝트 디렉터리를 선택합니다.
    1. 프로젝트에 삽입할 디렉터리를 선택합니다. 이 단계에서는 Xcode 프로젝트에 대한 모든 파일을 포함하는 선택된 디렉터리에 helloworld 디렉터리를 만듭니다.
    1. 이 예제 프로젝트에 대한 Git 리포지토리 만들기를 사용하지 않도록 설정합니다.
1. 또한 앱은 `Info.plist` 파일에서 마이크 사용을 선언해야 합니다. 개요에서 파일을 선택하고 *음성 인식에 마이크 필요*와 같은 값을 사용하여 **개인 정보 - 마이크 사용 설명** 키를 추가합니다.

    ![Info.plist의 설정](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-info-plist.png)

1. Xcode 프로젝트를 닫습니다. CocoaPods를 설정한 후 나중에 다른 인스턴스를 사용합니다.

## <a name="add-the-sample-code"></a>샘플 코드 추가

1. 이름이 `MicrosoftCognitiveServicesSpeech-Bridging-Header.h`인 새 헤더 파일을 helloworld 프로젝트 내의 helloworld 디렉터리에 배치합니다. 다음 코드를 이 파일에 붙여넣습니다.

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]

1. **Objective-C 브리징 헤더** 필드에서 helloworld 대상의 Swift 프로젝트 설정에 브리징 헤더에 대한 상대 경로 `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h`를 추가합니다.

   ![헤더 속성](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-bridging-header.png)

1. 자동 생성된 `AppDelegate.swift` 파일의 콘텐츠를 다음 코드로 바꿉니다.

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/AppDelegate.swift#code)]
1. 자동 생성된 `ViewController.swift` 파일의 콘텐츠를 다음 코드로 바꿉니다.

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/ViewController.swift#code)]
1. `ViewController.swift`에서 문자열 `YourSubscriptionKey`를 구독 키로 바꿉니다.
1. `YourServiceRegion` 문자열을 구독과 연결된 [Azure 지역](~/articles/cognitive-services/Speech-Service/regions.md)으로 바꿉니다. 예를 들어 평가판 구독의 경우 `westus`를 사용합니다.

## <a name="install-the-sdk-as-a-cocoapod"></a>CocoaPod로 SDK 설치

1. 해당 [설치 지침](https://guides.cocoapods.org/using/getting-started.html)에 설명된 대로 CocoaPod 종속성 관리자를 설치합니다.
1. 샘플 앱의 디렉터리인 helloworld로 이동합니다. 해당 디렉터리에 이름이 *Podfile*인 텍스트 파일 및 다음과 같은 콘텐츠를 배치합니다.

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/Podfile)]
1. 터미널에서 helloworld 디렉터리로 이동하여 `pod install` 명령을 실행합니다. 이 명령은 샘플 앱 및 Speech SDK를 종속성으로 모두 포함하는 `helloworld.xcworkspace` Xcode 작업 영역을 생성합니다. 이 작업 영역은 다음 단계에서 사용됩니다.

## <a name="build-and-run-the-sample"></a>샘플 빌드 및 실행

1. Xcode에서 `helloworld.xcworkspace` 작업 영역을 엽니다.
1. **보기** > **디버그 영역** > **콘솔 활성화**를 선택하여 디버그 출력을 표시합니다.
1. **제품** > **대상** 메뉴의 목록에서 앱에 대한 대상으로 개발 머신에 연결된 iOS 시뮬레이터 또는 iOS 디바이스를 선택합니다.
1. 메뉴에서 **제품** > **실행**을 선택하여 iOS 시뮬레이터의 예제 코드를 빌드하고 실행합니다. **재생** 단추를 선택할 수도 있습니다.
1. 앱에서 **Recognize** 단추를 선택하고 몇 단어를 말하면 화면 아래쪽 부분에 사용자가 말한 텍스트가 표시되어야 합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [GitHub에서 샘플 살펴보기](https://aka.ms/csspeech/samples)
