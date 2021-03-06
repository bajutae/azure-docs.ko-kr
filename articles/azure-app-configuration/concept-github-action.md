---
title: 앱 구성에 GitHub 리포지토리 동기화
description: Github 리포지토리를 업데이트할 때 GitHub 작업을 사용 하 여 앱 구성 인스턴스를 자동으로 업데이트 합니다.
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/20/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 602ccddf97938022df3c5903b573608558fe5d35
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80585490"
---
# <a name="sync-your-github-repository-to-app-configuration"></a>앱 구성에 GitHub 리포지토리 동기화

기존 소스 제어 사례를 계속 사용 하려는 팀은 GitHub 작업을 사용 하 여 GitHub 리포지토리를 앱 구성 저장소와 자동으로 동기화 할 수 있습니다. 이렇게 하면 다음과 같은 앱 구성 혜택을 얻을 수 있을 때 처럼 구성 파일을 일반적으로 변경할 수 있습니다. <br>
&nbsp;&nbsp;&nbsp;&nbsp;• 코드 외부에서 중앙 집중식 구성 <br>
&nbsp;&nbsp;&nbsp;&nbsp;• 전체 앱을 다시 배포 하지 않고 구성을 업데이트 합니다. <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Azure App Service 및 함수와 같은 서비스와의 통합 

GitHub 작업 [워크플로](https://help.github.com/articles/about-github-actions#workflow) 는 github 리포지토리의 자동화 된 프로세스를 정의 합니다. *Azure 앱 구성 동기화* 작업은 원본 리포지토리가 변경 될 때 앱 구성 인스턴스로 업데이트를 트리거합니다. 저장소 `/.github/workflows/` 경로에 있는 yaml (.yml) 파일을 사용 하 여 단계와 매개 변수를 정의 합니다. 앱 코드와 마찬가지로 앱 구성 파일을 푸시, 검토 또는 분기 하는 경우 구성 업데이트를 트리거할 수 있습니다.

GitHub [설명서](https://help.github.com/actions/automating-your-workflow-with-github-actions/configuring-a-workflow) 는 github 워크플로 및 작업에 대 한 자세한 보기를 제공 합니다. 

## <a name="enable-github-actions-in-your-repository"></a>리포지토리에서 GitHub 작업 사용
이 GitHub 동작 사용을 시작 하려면 리포지토리로 이동 하 고 [ **작업** ] 탭을 선택 하세요. [ **새 워크플로**]를 클릭 한 다음 **워크플로를 직접 설정**하세요. 마지막으로 marketplace에서 "Azure 앱 구성 동기화"를 검색 합니다.
> [!div class="mx-imgBorder"]
> ![작업 탭을 선택 합니다.](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![앱 구성 동기화 작업을 선택 합니다.](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>푸시 후 구성 파일 동기화
이 작업은 변경 내용이 푸시 될 때 Azure 앱 구성 파일을 `appsettings.json`동기화 합니다. 개발자가에 대 `appsettings.json`한 변경 내용을 푸시할 때 앱 구성 동기화 작업은 앱 구성 인스턴스를 새 값으로 업데이트 합니다.

이 워크플로의 첫 번째 섹션은 작업이 *마스터* 분기에 포함 `appsettings.json` 된 *푸시* *에 대해* 트리거 되도록 지정 합니다. 두 번째 섹션은 작업이 트리거된 후 실행 되는 작업을 나열 합니다. 작업은 관련 파일을 체크 아웃 하 고 리포지토리의 암호로 저장 된 연결 문자열을 사용 하 여 앱 구성 인스턴스를 업데이트 합니다.  GitHub에서 암호를 사용 하는 방법에 대 한 자세한 내용은 암호화 된 암호 만들기 및 사용에 대 한 [github의 문서](https://help.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) 를 참조 하세요.

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your                        
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }} 
          separator: ':' 
```

## <a name="use-a-dynamic-label-on-sync"></a>동기화 시 동적 레이블 사용
이전 작업은가 업데이트 될 때마다 `appsettings.json` 앱 구성 인스턴스를 업데이트 합니다. 이 작업은 각 동기화에 동적 레이블을 삽입 하 여 각 동기화를 고유 하 게 식별 하 고 코드 변경 내용을 구성 변경 내용에 매핑할 수 있도록 합니다.

이 워크플로의 첫 번째 섹션은 작업이 *마스터* 분기에 포함 `appsettings.json` 된 *푸시* *에 대해* 트리거 되도록 지정 합니다. 두 번째 섹션은 커밋 해시에 따라 구성 업데이트에 대 한 고유 레이블을 만드는 작업을 실행 합니다. 그런 다음이 작업은이 업데이트에 대 한 새 값과 고유 레이블로 앱 구성 인스턴스를 업데이트 합니다.

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # Creates a label based on the branch name and the first 8 characters          
      # of the commit hash 
      - id: determine_label 
        run: echo ::set-output name=LABEL::"${GITHUB_REF#refs/*/}/${GITHUB_SHA:0:8}" 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: ${{ steps.determine_label.outputs.LABEL }} 
```

## <a name="use-strict-sync"></a>엄격한 동기화 사용
Strict 모드를 사용 하는 경우 동기화는 앱 구성 인스턴스가 지정 된 접두사 및 레이블의 구성 파일과 정확히 일치 하는지 확인 합니다. 구성 파일에 없는 접두사와 레이블이 같은 키-값 쌍은 삭제 됩니다. 
 
Strict 모드를 사용 하지 않는 경우 동기화는 구성 파일의 키 값만 설정 합니다. 키-값 쌍은 삭제 되지 않습니다. 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: 'Label' 
          prefix: 'Prefix:' 
          strict: true 
```

## <a name="use-max-depth-to-limit-github-action"></a>최대 깊이를 사용 하 여 GitHub 작업 제한
중첩 된 JSON 특성의 기본 동작은 전체 개체를 평면화 하는 것입니다.  아래 JSON은이 키-값 쌍을 정의 합니다.

| 키 | 값 |
| --- | --- |
| 개체: Inner: InnerKey | InnerValue |

```json
{ "Object": 
    { "Inner":
        {
        "InnerKey": "InnerValue"
        }
    }
}
```

중첩 된 개체를 구성 인스턴스로 푸시하는 값으로 사용 하려면 *깊이* 값을 사용 하 여 적절 한 깊이에서 평면화를 중지할 수 있습니다. 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          depth: 2 
```

깊이가 2 인 경우 위의 예제는 이제 다음과 같은 키-값 쌍을 반환 합니다.

| 키 | 값 |
| --- | --- |
| 개체: 내부 | {"InnerKey": "Innerkey"} |

## <a name="understand-action-inputs"></a>작업 입력 이해
입력 매개 변수는 런타임에 작업에서 사용 하는 데이터를 지정 합니다.  다음 표에는 앱 구성 동기화에서 허용 하는 입력 매개 변수와 각 매개 변수의 예상 값이 포함 되어 있습니다.  GitHub 작업의 작업 입력에 대 한 자세한 내용은 GitHub [설명서](https://help.github.com/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#inputs)를 참조 하세요.

> [!Note]
> 입력 Id는 대/소문자를 구분 하지 않습니다.


| 입력 이름 | 필수 여부 | 값 |
|----|----|----|
| Configurationfile.ini | 예 | 리포지토리의 구성 파일에 대 한 상대 경로입니다.  Glob 패턴은 지원 되며 여러 파일을 포함할 수 있습니다. |
| format | 예 | 구성 파일의 파일 형식입니다.  유효한 형식은 JSON, YAML, properties입니다. |
| connectionString | 예 | 앱 구성 인스턴스에 대 한 연결 문자열입니다. 연결 문자열은 GitHub 리포지토리에서 암호로 저장 해야 하며, 워크플로에서는 암호 이름만 사용 해야 합니다. |
| 구분 기호 | 예 | 구성 파일을 키-값 쌍으로 평면화 할 때 사용 되는 구분 기호입니다.  유효한 값은입니다. , ; : - _ __ / |
| 접두사 | 아니요 | 키의 시작 부분에 추가할 접두사입니다. |
| label | 아니요 | 키-값 쌍을 설정할 때 사용 되는 레이블입니다. 지정 하지 않으면 null 레이블이 사용 됩니다. |
| strict | 아니요 | Strict 모드를 사용 하는지 여부를 결정 하는 부울 값입니다. 기본값은 False입니다. |
| 크기 | 아니요 | 구성 파일을 평면화 하는 최대 깊이입니다.  깊이는 양수 여야 합니다.  기본값에는 최대 깊이가 없습니다. |
| tags | 아니요 | 키-값 쌍에 대해 설정 된 태그를 지정 합니다.  필요한 형식은 다음 모양의 JSON 개체에 대 한 문자열 형식 형식입니다. {[propertyName: string]: string;} 각 속성 이름-값은 태그가 됩니다. |

## <a name="next-steps"></a>다음 단계

이 문서에서는 앱 구성 동기화 GitHub 작업 및 앱 구성 인스턴스에 대 한 업데이트를 자동화 하는 데 사용할 수 있는 방법에 대해 알아보았습니다. Azure 앱 구성이 키-값 쌍의 변경에 반응 하는 방법을 알아보려면 다음 [문서](./concept-app-configuration-event.md)를 계속 진행 합니다.
