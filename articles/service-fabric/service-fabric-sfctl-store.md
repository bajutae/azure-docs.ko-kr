---
title: Azure Service Fabric CLI-sfctl 저장소
description: Sfctl, Azure Service Fabric 명령줄 인터페이스에 대해 알아봅니다. 클러스터 이미지 저장소에서 파일 수준 작업을 수행 하기 위한 명령 목록을 포함 합니다.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 75c62b54ff3aa7f3af344aa3e1ca81d431ae0ab2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76905618"
---
# <a name="sfctl-store"></a>sfctl store
클러스터 이미지 저장소에서 기본 파일 수준 작업을 수행합니다.

## <a name="commands"></a>명령

|명령|Description|
| --- | --- |
| 삭제 | 기존 이미지 저장소 콘텐츠를 삭제합니다. |
| root-info | 이미지 저장소의 루트에서 콘텐츠 정보를 가져옵니다. |
| stat | 이미지 저장소 콘텐츠 정보를 가져옵니다. |

## <a name="sfctl-store-delete"></a>sfctl store delete
기존 이미지 저장소 콘텐츠를 삭제합니다.

지정된 이미지 저장소 상대 경로 내에서 발견되는 기존 이미지 저장소 콘텐츠를 삭제합니다. 이 명령은 업로드된 애플리케이션 패키지가 프로비저닝된 후에 이를 삭제하는 데 사용할 수 있습니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --content-path [필수] | 루트에서 이미지 저장소의 파일 또는 폴더에 대한 상대 경로입니다. |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

## <a name="sfctl-store-root-info"></a>sfctl 저장소 루트 정보
이미지 저장소의 루트에서 콘텐츠 정보를 가져옵니다.

이미지 저장소의 루트에 있는 이미지 저장소 콘텐츠에 대한 정보를 반환합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

## <a name="sfctl-store-stat"></a>sfctl store stat
이미지 저장소 콘텐츠 정보를 가져옵니다.

지정된 contentPath에 있는 이미지 저장소 콘텐츠에 대한 정보를 반환합니다. contentPath는 이미지 저장소의 루트를 기준으로 합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --content-path [필수] | 루트에서 이미지 저장소의 파일 또는 폴더에 대한 상대 경로입니다. |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |


## <a name="next-steps"></a>다음 단계
- Service Fabric CLI [설정](service-fabric-cli.md)
- [샘플 스크립트](/azure/service-fabric/scripts/sfctl-upgrade-application)를 사용하여 Microsoft Azure Service Fabric CLI를 사용하는 방법에 대해 알아봅니다.