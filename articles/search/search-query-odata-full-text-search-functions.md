---
title: OData 전체 텍스트 검색 함수 참조
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search 쿼리에서 OData 전체 텍스트 검색 함수, ismatch 및 검색. ismatchscoring 매기기를 검색 합니다.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 06eb29f2f3245d3f4fd047fb86b2b57fb1f0989e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "72793361"
---
# <a name="odata-full-text-search-functions-in-azure-cognitive-search---searchismatch-and-searchismatchscoring"></a>Azure Cognitive Search `search.ismatch` 의 OData 전체 텍스트 검색 기능`search.ismatchscoring`

Azure Cognitive Search는 및 `search.ismatch` `search.ismatchscoring` 함수를 통해 [OData 필터 식](query-odata-filter-orderby-syntax.md) 의 전체 텍스트 검색을 지원 합니다. 이러한 함수를 사용 하면 `search` [검색 API](https://docs.microsoft.com/rest/api/searchservice/search-documents)의 최상위 매개 변수를 사용 하는 것 만으로 불가능 한 방식으로 전체 텍스트 검색을 엄격한 부울 필터링과 결합할 수 있습니다.

> [!NOTE]
> 및 `search.ismatch` `search.ismatchscoring` 함수는 [검색 API](https://docs.microsoft.com/rest/api/searchservice/search-documents)의 필터 에서만 지원 됩니다. [제안](https://docs.microsoft.com/rest/api/searchservice/suggestions) 또는 [자동 완성](https://docs.microsoft.com/rest/api/searchservice/autocomplete) api에서 지원 되지 않습니다.

## <a name="syntax"></a>구문

다음 EBNF ([Extended Backus-Backus-naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form))는 `search.ismatch` 및 `search.ismatchscoring` 함수의 문법을 정의 합니다.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

대화형 구문 다이어그램도 사용할 수 있습니다.

> [!div class="nextstepaction"]
> [Azure Cognitive Search에 대 한 OData 구문 다이어그램](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> 전체 EBNF [Azure Cognitive Search에 대 한 OData 식 구문 참조](search-query-odata-syntax-reference.md) 를 참조 하세요.

### <a name="searchismatch"></a>ismatch

함수 `search.ismatch` 는 전체 텍스트 검색 쿼리를 필터 식의 일부로 계산 합니다. 검색 쿼리와 일치하는 문서가 결과 집합에 반환됩니다. 이 함수의 다음 오버로드를 사용할 수 있습니다.

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

매개 변수는 다음 표에 정의 되어 있습니다.

| 매개 변수 이름 | 유형 | Description |
| --- | --- | --- |
| `search` | `Edm.String` | [단순](query-simple-syntax.md) 또는 [전체](query-lucene-syntax.md) Lucene 쿼리 구문에서 검색 쿼리입니다. |
| `searchFields` | `Edm.String` | 검색할 검색 가능한 필드의 쉼표로 구분 된 목록입니다. 인덱스의 모든 검색 가능 필드를 기본값으로 설정 합니다. 매개 변수에서 [필드 지정 search](query-lucene-syntax.md#bkmk_fields) 를 사용 하는 경우 Lucene 쿼리의 필드 지정자는이 매개 변수에 지정 된 모든 필드를 재정의 합니다. `search` |
| `queryType` | `Edm.String` | `'simple'`또는 `'full'`; 기본값은 `'simple'`입니다. `search` 매개 변수에 사용된 쿼리 언어를 지정합니다. |
| `searchMode` | `Edm.String` | `'any'`또는 `'all'`의 기본값은 `'any'`입니다. 문서를 일치 항목으로 계산 하기 위해 `search` 매개 변수에서 검색어를 일치 시켜야 하는지 여부를 나타냅니다. `search` 매개 변수에서 [Lucene 부울 연산자](query-lucene-syntax.md#bkmk_boolean) 를 사용 하는 경우이 매개 변수 보다 우선 적용 됩니다. |

위의 모든 매개 변수는 [검색 API의 해당 하는 검색 요청 매개 변수와](https://docs.microsoft.com/rest/api/searchservice/search-documents)동일 합니다.

함수 `search.ismatch` 는 부울 [논리 연산자](search-query-odata-logical-operators.md)를 사용 `Edm.Boolean`하 여 다른 필터 하위 식으로 작성할 수 있는 형식의 값을 반환 합니다.

> [!NOTE]
> Azure Cognitive Search는 람다 식 내 `search.ismatch` 에서 `search.ismatchscoring` 또는 사용을 지원 하지 않습니다. 즉, 동일한 개체에서 전체 텍스트 검색 조건과 엄격한 필터 일치를 상호 연결할 수 있는 개체 컬렉션에 대해 필터를 작성할 수 없습니다. 이러한 제한 사항 및 예제에 대 한 자세한 내용은 [Azure Cognitive Search에서 컬렉션 필터 문제 해결](search-query-troubleshoot-collection-filters.md)을 참조 하세요. 이러한 제한이 있는 이유에 대 한 자세한 내용은 [Azure Cognitive Search의 컬렉션 필터 이해](search-query-understand-collection-filters.md)를 참조 하세요.


### <a name="searchismatchscoring"></a>검색. ismatchscoring 매기기

함수와 `search.ismatchscoring` 마찬가지로 `search.ismatch` 함수는 매개 변수로 전달 된 `true` 전체 텍스트 검색 쿼리와 일치 하는 문서에 대해를 반환 합니다. 이 두 함수 간의 차이점은 `search.ismatchscoring` 쿼리와 일치하는 문서의 관련성 점수가 전반적인 문서 점수에 기여하지만, `search.ismatch`의 경우 문서 점수가 변경되지 않는다는 것입니다. 이 함수의 다음 오버로드는 `search.ismatch`의 매개 변수와 동일한 매개 변수에서 사용할 수 있습니다.

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

동일한 필터 `search.ismatch` 식 `search.ismatchscoring` 에 및 함수를 모두 사용할 수 있습니다.

## <a name="examples"></a>예

"waterfront" 단어를 포함하는 문서를 찾습니다. 이 필터 쿼리는 `search=waterfront`를 사용한 [검색 요청](https://docs.microsoft.com/rest/api/searchservice/search-documents)과 동일합니다.

    search.ismatchscoring('waterfront')

단어 "hostel"을 포함하고 등급이 4 이상인 문서 또는 단어 "motel"을 포함하고 등급이 5인 문서를 찾습니다. 이 요청은 `search.ismatchscoring` 함수를 사용해야만 표시할 수 있습니다.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

단어 "luxury"가 없는 문서를 찾습니다.

    not search.ismatch('luxury')

구 "ocean view"를 포함하거나 등급이 5인 문서를 찾습니다. `search.ismatchscoring` 쿼리는 필드 `HotelName` 및 `Rooms/Description`에 대해서만 실행됩니다.

분리의 두 번째 절과 일치 하는 문서만 반환 됩니다. 즉, 호텔은 5 `Rating` 와 동일 합니다. 이러한 문서가 식의 점수가 매겨진 부분과 일치 하지 않는 것을 명확 하 게 하기 위해 점수가 0 인 것으로 반환 됩니다.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

호텔 설명에서 "호텔" 및 "공항" 이라는 용어가 5 개의 단어 내에 있고 흡연가 일부 대화방에서 허용 되지 않는 문서를 찾습니다. 이 쿼리는 [전체 Lucene 쿼리 언어](query-lucene-syntax.md)를 사용합니다.

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>다음 단계  

- [Azure Cognitive Search의 필터](search-filters.md)
- [Azure Cognitive Search에 대 한 OData 식 언어 개요](query-odata-filter-orderby-syntax.md)
- [Azure Cognitive Search에 대 한 OData 식 구문 참조](search-query-odata-syntax-reference.md)
- [Azure Cognitive Search REST API &#40;문서 검색&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
