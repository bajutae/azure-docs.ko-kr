---
title: Azure Monitor 뷰의 필터 | Microsoft Docs
description: 사용자는 Azure Monitor 뷰의 필터를 사용 하 여 뷰 자체를 수정 하지 않고 특정 속성의 값을 기준으로 뷰의 데이터를 필터링 할 수 있습니다.  이 문서에서는 필터를 사용하여 사용자 지정 보기에 추가하는 방법을 설명합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/22/2018
ms.openlocfilehash: b4840ed30eb1f6dc8d6e6cef47da17807f9644d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77658577"
---
# <a name="filters-in-azure-monitor-views"></a>Azure Monitor 뷰의 필터
사용자는 [Azure Monitor 뷰의](view-designer.md) **필터** 를 사용 하 여 뷰 자체를 수정 하지 않고 특정 속성의 값을 기준으로 뷰의 데이터를 필터링 할 수 있습니다.  예를 들어, 보기의 사용자가 특정 컴퓨터 또는 컴퓨터 집합의 데이터에 대한 보기를 필터링하도록 허용할 수 있습니다.  단일 보기에 여러 필터를 만들어서 사용자가 여러 속성별로 필터링하도록 허용할 수 있습니다.  이 문서에서는 필터를 사용하여 사용자 지정 보기에 추가하는 방법을 설명합니다.

## <a name="using-a-filter"></a>필터 사용
보기의 맨 위에 있는 날짜 시간 범위를 클릭 하 여 보기의 날짜 시간 범위를 변경할 수 있는 드롭다운을 엽니다.

![필터 예제](media/view-designer-filters/filters-example-time.png)

보기에 **+** 대해 정의 된 사용자 지정 필터를 사용 하 여 필터를 추가 하려면 클릭 합니다. 드롭다운에서 필터에 대한 값을 선택하거나 값을 입력합니다. 을 클릭 하 여 필터를 계속 **+** 추가 합니다. 


![필터 예제](media/view-designer-filters/filters-example-custom.png)

필터에 대한 모든 값을 제거하면 해당 필터가 더 이상 적용되지 않습니다.


## <a name="creating-a-filter"></a>필터 만들기

[보기를 편집](view-designer.md)할 때 **필터** 탭에서 필터를 만듭니다.  필터는 보기에 대해 전역적이며 보기에서 모든 부분에 적용됩니다.  

![필터 설정](media/view-designer-filters/filters-settings.png)

다음 표에서는 필터의 설정을 설명합니다.

| 설정 | Description |
|:---|:---|
| 필드 이름 | 필터링에 사용되는 필드의 이름입니다.  이 필드는 **값에 대 한 쿼리의**요약 필드와 일치 해야 합니다. |
| 값에 대한 쿼리 | 쿼리를 실행하여 사용자에 대한 필터 드롭다운을 채웁니다.  이 쿼리는 [요약](/azure/kusto/query/summarizeoperator) 또는 [distinct](/azure/kusto/query/distinctoperator) 를 사용 하 여 특정 필드에 고유한 값을 제공 해야 하며 **필드 이름과**일치 해야 합니다.  [정렬](/azure/kusto/query/sortoperator)을 사용하여 사용자에게 표시되는 값을 정렬할 수 있습니다. |
| 태그 | 필터를 지원하는 쿼리에서 사용되는 필터의 이름은 사용자에게도 표시됩니다. |

### <a name="examples"></a>예

다음 표에는 몇 가지 공통 필터가 포함되어 있습니다.  

| 필드 이름 | 값에 대한 쿼리 | 태그 |
|:--|:--|:--|
| Computer   | 하트비트 &#124; 고유한 컴퓨터 &#124; 컴퓨터 오름차순 정렬 | 컴퓨터 |
| EventLevelName | 이벤트 &#124; 고유한 EventLevelName | 심각도 |
| SeverityLevel | Syslog &#124; 고유한 SeverityLevel | 심각도 |
| SvcChangeType | ConfigurationChange &#124; 고유한 svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>보기 쿼리 수정

적용되는 필터의 경우 보기에서 모든 쿼리를 수정하여 선택한 값을 필터링해야 합니다.  뷰에서 쿼리를 수정 하지 않으면 사용자가 선택 하는 모든 값이 아무런 영향을 주지 않습니다.

쿼리에서 필터 값을 사용하기 위한 구문은 다음과 같습니다. 

    where ${filter name}  

예를 들어 뷰에 이벤트를 반환 하 고 _컴퓨터_라는 필터를 사용 하는 쿼리가 있는 경우 다음 쿼리를 사용할 수 있습니다.

    Event | where ${Computers} | summarize count() by EventLevelName

심각도라는 다른 필터를 추가한 경우 두 필터를 모두 사용하는 다음 쿼리를 사용할 수 있습니다.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>다음 단계
* 사용자 지정 보기에 추가할 수 있는 [시각화 파트](view-designer-parts.md)에 대해 자세히 알아봅니다.
