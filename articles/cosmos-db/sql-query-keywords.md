---
title: Azure Cosmos DB에 대 한 SQL 키워드
description: Azure Cosmos DB에 대 한 SQL 키워드에 대해 알아봅니다.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: tisande
ms.openlocfilehash: 069548b9b69ef6f7f6bde85ede830d97f3d312db
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81261570"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure Cosmos DB의 키워드

이 문서에서는 SQL 쿼리에서 Azure Cosmos DB는 데 사용할 수 있는 키워드에 대해 자세히 설명 합니다.

## <a name="between"></a>BETWEEN

키워드를 `BETWEEN` 사용 하 여 문자열 또는 숫자 값의 범위에 대해 쿼리를 표현할 수 있습니다. 예를 들어 다음 쿼리는 첫 번째 자식의 등급이 1-5 (포함) 인 모든 항목을 반환 합니다.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

다음 예제와 같이 `BETWEEN` `SELECT` 절에서 키워드를 사용할 수도 있습니다.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

SQL API에서 ANSI SQL과 달리 혼합 형식의 속성에 대해 범위 쿼리를 표현할 수 있습니다. 예를 들어 `grade` 는 일부 항목 `5` 에서와 같은 숫자와 다른 문자열 `grade4` 의 문자열 일 수 있습니다. 이러한 경우 JavaScript에서와 같이 서로 다른 두 형식 간의 비교로 인해 `Undefined`항목이 생략 됩니다.

> [!TIP]
> 쿼리 실행 시간을 단축 하려면 `BETWEEN` 절이 필터링 하는 숫자 속성 또는 경로에 대해 범위 인덱스 유형을 사용 하는 인덱싱 정책을 만듭니다.

## <a name="distinct"></a>DISTINCT

키워드 `DISTINCT` 는 쿼리의 프로젝션에서 중복 요소를 제거 합니다.

이 예제에서 쿼리는 각 성에 대 한 값을 프로젝션 합니다.

```sql
SELECT DISTINCT VALUE f.lastName
FROM Families f
```

결과는 다음과 같습니다.

```json
[
    "Andersen"
]
```

고유한 개체를 프로젝션 할 수도 있습니다. 이 경우 두 문서 중 하나에 lastName 필드가 없으므로 쿼리가 빈 개체를 반환 합니다.

```sql
SELECT DISTINCT f.lastName
FROM Families f
```

결과는 다음과 같습니다.

```json
[
    {
        "lastName": "Andersen"
    },
    {}
]
```

DISTINCT는 하위 쿼리 내의 프로젝션에서 사용할 수도 있습니다.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

이 쿼리는 중복 항목을 제거 하 고 각 자식의 givenName 포함 된 배열을 프로젝션 합니다. 이 배열은 ChildNames로 별칭이 지정 되며 외부 쿼리에서 프로젝션 됩니다.

결과는 다음과 같습니다.

```json
[
    {
        "id": "AndersenFamily",
        "ChildNames": []
    },
    {
        "id": "WakefieldFamily",
        "ChildNames": [
            "Jesse",
            "Lisa"
        ]
    }
]
```

집계 시스템 함수 및의 하위 쿼리를 사용 하 `DISTINCT` 는 쿼리는 지원 되지 않습니다. 예를 들어 다음 쿼리는 지원 되지 않습니다.

```sql
SELECT COUNT(1) FROM (SELECT DISTINCT f.lastName FROM f)
```

## <a name="in"></a>IN

IN 키워드를 사용 하 여 지정 된 값이 목록에 있는 값과 일치 하는지 여부를 확인 합니다. 예를 들어 다음 쿼리는 `id` 가 또는 `WakefieldFamily` `AndersenFamily`인 모든 패밀리 항목을 반환 합니다.

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

다음 예에서는 상태가 지정 된 값을 갖는 모든 항목을 반환 합니다.

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

SQL API는 [JSON 배열](sql-query-object-array.md#Iteration)에 대 한 반복을 지원 하며, FROM 소스에 있는 in 키워드를 통해 새 구문을 추가 합니다.

`IN` 필터에 파티션 키를 포함 하는 경우 쿼리는 관련 파티션만 자동으로 필터링 합니다.

## <a name="top"></a>맨 위로 이동

TOP 키워드는 정의 되지 않은 `N` 순서로 쿼리 결과의 첫 번째 수를 반환 합니다. 모범 사례로 `ORDER BY` 절과 함께 TOP을 사용 하 여 결과를 정렬 된 첫 번째 `N` 값으로 제한할 수 있습니다. 이러한 두 절을 결합 하는 것이 가장 높은 영향을 주는 행을 예측 가능 하 게 나타내는 유일한 방법입니다.

다음 예제와 같이 상수 값 이나 매개 변수가 있는 쿼리를 사용 하는 변수 값으로 TOP을 사용할 수 있습니다.

```sql
    SELECT TOP 1 *
    FROM Families f
```

결과는 다음과 같습니다.

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a name="next-steps"></a>다음 단계

- [시작](sql-query-getting-started.md)
- [조인](sql-query-join.md)
- [하위 쿼리](sql-query-subquery.md)