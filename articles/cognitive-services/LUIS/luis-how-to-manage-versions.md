---
title: 버전 관리-LUIS
titleSuffix: Azure Cognitive Services
description: 버전을 통해 다양한 모델을 빌드하고 게시할 수 있습니다. 모델에 변경 사항을 적용하기 전에 현재 활성 모델을 앱의 다른 버전에 을 복제하는 것이 좋습니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 2de39a2822056a4539c2dca915a483ab5bbc4965
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653988"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>스테이징 또는 프로덕션 앱에 영향을 주지 않고 버전을 사용하여 편집 및 테스트

버전을 통해 다양한 모델을 빌드하고 게시할 수 있습니다. 모델에 변경 사항을 적용하기 전에 현재 활성 모델을 앱의 다른 [버전](luis-concept-version.md)에 복제하는 것이 좋습니다.

활성 버전은 LUIS 포털 **빌드** 섹션에서 의도, 엔터티, 기능 및 패턴으로 편집 하는 버전입니다. 제작 Api를 사용 하는 경우 버전 관련 REST API 호출에 경로에 버전이 포함 되어 있으므로 활성 버전을 설정할 필요가 없습니다.

버전을 사용하려면 **내 앱** 페이지에서 해당 이름을 선택하여 앱을 연 다음, 위쪽 표시줄에서 **관리**를 선택하고 왼쪽 탐색에서 **버전**을 선택합니다.

버전 목록에는 게시 된 버전, 게시 된 버전 및 현재 활성 상태인 버전이 표시 됩니다.

## <a name="clone-a-version"></a>버전 복제

1. 복제할 버전을 선택한 다음, 도구 모음에서 **복제**를 선택합니다.

2. **복제 버전** 대화 상자에서 “0.2”와 같은 새 버전에 사용할 이름을 입력합니다.

   ![버전 복제 대화 상자](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)

     > [!NOTE]
     > 버전 ID는 문자, 숫자 또는 ‘.’으로만 구성할 수 있으며 10자 이하여야 합니다.

   지정한 이름으로 새 버전이 생성되고 활성 버전으로 설정됩니다.

## <a name="set-active-version"></a>활성 버전 설정

목록에서 버전을 선택 하 고 도구 모음에서 **활성화** 를 선택 합니다.

## <a name="import-version"></a>버전 가져오기

`.json`또는 응용 프로그램 버전을 가져올 수 있습니다 `.lu` .

1. 도구 모음에서 **가져오기** 를 선택 하 고 형식을 선택 합니다.

2. **새 버전 가져오기** 팝업 창에서 10자의 새 버전 이름을 입력합니다. 파일의 버전이 이미 앱에 있는 경우 버전 ID를 설정 하기만 하면 됩니다.

    ![관리 섹션, 버전 페이지, 새 버전 가져오기](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    버전을 가져오면 새 버전이 활성 버전이 됩니다.

### <a name="import-errors"></a>가져오기 오류

* 토크 오류: 가져올 때 **토크 오류** 메시지가 표시 되 면 현재 앱에서 사용 하는 것과 다른 [토크 토크](luis-language-support.md#custom-tokenizer-versions) 를 사용 하는 버전을 가져오려고 시도 하는 것입니다. 이 문제를 해결 하려면 [토크 토크 버전 간 마이그레이션](luis-language-support.md#migrating-between-tokenizer-versions)을 참조 하세요.

<a name = "export-version"></a>

## <a name="other-actions"></a>다른 작업

* 버전을 **삭제**하려면 목록에서 버전을 선택한 다음, 도구 모음에서 **삭제**를 선택합니다. **확인을**선택 합니다.
* 버전의 **이름을 바꾸려면** 목록에서 버전을 선택한 다음, 도구 모음에서 **이름 바꾸기**를 선택합니다. 새 이름을 입력하고 **완료**를 선택합니다.
* 버전을 **내보내려면** 목록에서 버전을 선택한 다음, 도구 모음에서 **앱 내보내기**를 선택합니다. 백업용으로 내보낼 JSON을 선택 하 고 [LUIS 컨테이너에서이 앱을 사용](luis-container-howto.md)하려면 **컨테이너에 대해 내보내기** 를 선택 합니다.

