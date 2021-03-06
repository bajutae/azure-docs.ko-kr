---
title: 매핑 데이터 흐름의 조건부 분할 변환
description: Azure Data Factory 매핑 데이터 흐름에서 조건부 분할 변환을 사용 하 여 데이터를 여러 스트림으로 분할
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: bd9241e526d7cf42f0697afb8635c085a08c80d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606478"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>매핑 데이터 흐름의 조건부 분할 변환

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

조건부 분할 변환은 일치 조건에 따라 데이터 행을 서로 다른 스트림으로 라우팅합니다. 조건부 분할 변환은 프로그래밍 언어의 CASE 의사 결정 구조와 유사 합니다. 변환은 식을 평가하고, 결과에 따라 데이터 행을 지정된 스트림으로 보냅니다.

## <a name="configuration"></a>Configuration

**분할** 설정은 데이터 행이 첫 번째 일치 하는 스트림 또는 일치 하는 모든 스트림으로 흐르도록 할지 여부를 결정 합니다.

데이터 흐름 식 작성기를 사용 하 여 분할 조건에 대 한 식을 입력 합니다. 새 조건을 추가 하려면 기존 행에서 더하기 아이콘을 클릭 합니다. 조건에 일치 하지 않는 행에 대해서도 기본 스트림을 추가할 수 있습니다.

![조건부 분할](media/data-flow/conditionalsplit1.png "조건부 분할 옵션")

## <a name="data-flow-script"></a>데이터 흐름 스크립트

### <a name="syntax"></a>구문

```
<incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
```

### <a name="example"></a>예제

아래 예제는 들어오는 스트림을 `SplitByYear` `CleanData`취하는 이라는 조건부 분할 변환입니다. 이 변환에는 두 개의 `year < 1960` 분할 `year > 1980`조건 및가 있습니다. `disjoint`는 데이터가 첫 번째 일치 조건으로 이동 하기 때문에 false입니다. 첫 번째 조건과 일치 하는 모든 행이 출력 `moviesBefore1960`스트림으로 이동 합니다. 두 번째 조건과 일치 하는 나머지 행은 모두 출력 `moviesAFter1980`스트림으로 이동 합니다. 다른 모든 행은 기본 스트림을 `AllOtherMovies`통해 흐릅니다.

Data Factory UX에서이 변환은 아래 이미지와 같습니다.

![조건부 분할](media/data-flow/conditionalsplit1.png "조건부 분할 옵션")

이 변환에 대 한 데이터 흐름 스크립트는 아래 코드 조각에 있습니다.

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>다음 단계

조건부 분할에 사용 되는 일반적인 데이터 흐름 변환은 [조인 변환](data-flow-join.md), [조회 변환](data-flow-lookup.md)및 [선택 변환](data-flow-select.md) 입니다.
