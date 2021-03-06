---
title: Azure Stream Analytics JobConfig. json 필드
description: 이 문서에서는 Visual Studio Code에서 작업을 만드는 데 사용 되는 Azure Stream Analytics JobConfig 파일에 대해 지원 되는 필드를 나열 합니다.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 27ed553035ce9d7abf57ffe93078df9c17b8408c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77617958"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>Azure Stream Analytics JobConfig. json 필드

다음 필드는 [Visual Studio Code를 사용 하 여 Azure Stream Analytics 작업을 만드는](quick-create-vs-code.md)데 사용 되는 *jobconfig* 파일에서 지원 됩니다.

```json
{
    "DataLocale": "string",
    "OutputErrorPolicy": "string",
    "EventsLateArrivalMaxDelayInSeconds": "integer",
    "EventsOutOfOrderMaxDelayInSeconds": "integer",
    "EventsOutOfOrderPolicy": "string",
    "StreamingUnits": "integer",
    "CompatibilityLevel": "string",
    "UseSystemAssignedIdentity": "boolean",
    "GlobalStorage": {
        "AccountName": "string",
        "AccountKey": "string",
    },
    "DataSourceCredentialDomain": "string",
    "ScriptType": "string",
    "Tags": {}
}
```

|속성|유형|필수|값|
|----|----|--------|-----|
|DataLocale|string|아니요|Stream analytics 작업의 데이터 로캘입니다. 값은 지원 되는의 이름 이어야 합니다. 지정 하지 않으면 기본적으로 ' en-us '로 설정 됩니다.|
|OutputErrorPolicy|string|아니요|출력에 도착 하 고 형식이 잘못 되었기 때문에 외부 저장소에 쓸 수 없는 이벤트에 적용할 정책을 나타냅니다 (누락 된 열 값, 잘못 된 유형 또는 크기의 열 값). -Stop 또는 Drop|
|EventsLateArrivalMaxDelayInSeconds|integer|아니요|이벤트가 늦게 도착 하는 최대 지속할 지연 시간 (초)입니다. 지원 되는 범위는-1에서 1814399 (20.23:59 일)이 고,-1은 무기한 대기를 지정 하는 데 사용 됩니다. 속성이 없으면 값이-1 인 것으로 해석 됩니다.|
|EventsOutOfOrderMaxDelayInSeconds|integer|아니요|순서가 잘못 된 이벤트를 다시 순서 대로 조정할 수 있는 최대 지속할 지연 시간 (초)입니다.|
|EventsOutOfOrderPolicy|string|아니요|입력 이벤트 스트림에 잘못 된 순서로 도착 하는 이벤트에 적용할 정책을 나타냅니다. -조정 또는 삭제|
|StreamingUnits|integer|예|스트리밍 작업에서 사용 하는 스트리밍 단위 수를 지정 합니다.|
|CompatibilityLevel|string|아니요|스트리밍 작업의 특정 런타임 동작을 제어 합니다. -허용 되는 값은 "1.0", "1.1", "1.2"입니다.|
|UseSystemAssignedIdentity|boolean|아니요|관리 되는 Azure Active Directory Id를 사용 하 여 다른 Azure 서비스와 통신 하도록이 작업을 설정 하려면 true로 설정 합니다.|
|GlobalStorage. AccountName|string|아니요|글로벌 저장소 계정은 SQL 참조 데이터 스냅숏과 같은 stream analytics 작업과 관련 된 콘텐츠를 저장 하는 데 사용 됩니다.|
|GlobalStorage|string|아니요|전역 저장소 계정에 해당 하는 키입니다.|
|DataSourceCredentialDomain|string|아니요|자격 증명 로컬 저장소에 대 한 예약 된 속성입니다.|
|ScriptType|string|예|이 소스 파일의 형식을 나타내는 예약 된 속성입니다. 사용할 수 있는 값은 JobConfig. json에 대 한 "JobConfig"입니다.|
|태그들|JSON 키-값 쌍|아니요|태그는 여러 리소스 및 리소스 그룹에 동일한 태그를 적용 하 여 리소스를 범주화 하 고 통합 된 청구를 볼 수 있는 이름/값 쌍입니다. 태그 이름은 대/소문자를 구분 하지 않으며 태그 값은 대/소문자를 구분 합니다.|

## <a name="next-steps"></a>다음 단계

* [Visual Studio Code에서 Azure Stream Analytics 작업 만들기](quick-create-vs-code.md)
* [Visual Studio Code를 통해 샘플 데이터를 사용하여 로컬로 Stream Analytics 쿼리 테스트](visual-studio-code-local-run.md)
* [Test Stream Analytics queries locally against live stream input by using Visual Studio Code](visual-studio-code-local-run-live-input.md)
*[CI/CD npm 패키지를 사용 하 여 Azure Stream Analytics 작업 Visual Studio Code 배포](setup-cicd-vs-code.md) 를 사용 하 여 라이브 스트림 입력에 대해 로컬로 쿼리 Stream Analytics 테스트