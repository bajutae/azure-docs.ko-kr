---
title: Azure Cosmos DB에서 JSON 작업
description: 중첩된 JSON 속성을 쿼리 및 액세스하고 Azure Cosmos DB에서 특수 문자를 사용하는 방법에 대해 알아봅니다.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: tisande
ms.openlocfilehash: a569b0122f9122b141b64ded21dbd9be1d766a41
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83699130"
---
# <a name="working-with-json-in-azure-cosmos-db"></a>Azure Cosmos DB에서 JSON 작업

Azure Cosmos DB의 SQL(Core) API에서 항목은 JSON으로 저장됩니다. 형식 시스템과 식이 JSON 형식만 처리하도록 제한됩니다. 자세한 내용은 [JSON 사양](https://www.json.org/)을 참조하세요.

JSON 작업의 몇 가지 중요한 측면을 요약하겠습니다.

- JSON 개체는 항상 `{` 왼쪽 중괄호로 시작하고 `}` 오른쪽 중괄호로 끝납니다.
- JSON 속성은 서로 [중첩](#nested-properties)될 수 있습니다.
- JSON 속성 값은 배열이 될 수 있습니다.
- JSON 속성 이름은 대/소문자를 구분합니다.
- JSON 속성 이름은 임의의 문자열 값(공백이나 문자가 아닌 문자 포함)이 될 수 있습니다.

## <a name="nested-properties"></a>중첩 속성

점 접근자를 사용하여 중첩된 JSON에 액세스할 수 있습니다. 다른 속성을 사용할 수 있는 것과 동일한 방법으로 쿼리에서 중첩된 JSON 속성을 사용할 수 있습니다.

다음은 중첩된 JSON을 사용하는 문서입니다.

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "address": {
      "state": "WA",
      "county": "King",
      "city": "Seattle"
      },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

이 경우 `state`, `country` 및 `city` 속성이 모두 `address` 속성 내에 중첩됩니다.

다음 예제에서는 두 개의 중첩된 속성 `f.address.state` 및 `f.address.city`를 프로젝션합니다.

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

결과는 다음과 같습니다.

```json
    [{
      "state": "WA",
      "city": "Seattle"
    }]
```

## <a name="working-with-arrays"></a>배열 작업

중첩된 속성 외에도 JSON은 배열을 지원합니다.

배열이 포함된 예제 문서는 다음과 같습니다.

```json
{
  "id": "WakefieldFamily",
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
      },
      {
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8
      }
  ],
}
```

배열을 사용하여 작업하는 경우 해당 위치를 참조하여 배열 내의 특정 요소에 액세스할 수 있습니다.

```sql
SELECT *
FROM Families f
WHERE f.children[0].givenName = "Jesse"
```

그러나 대부분의 경우 배열로 작업할 때 [하위 쿼리](sql-query-subquery.md) 또는 [셀프 조인](sql-query-join.md)을 사용합니다.

예를 들어 고객의 은행 계좌에 대한 일일 잔액을 보여 주는 문서는 다음과 같습니다.

```json
{
  "id": "Contoso-Checking-Account-2020",
  "balance": [
      {
        "checkingAccount": 1000,
        "savingsAccount": 5000
      },
      {
        "checkingAccount": 100,
        "savingsAccount": 5000
      },
      {
        "checkingAccount": -10,
        "savingsAccount": 5000,
      },
      {
        "checkingAccount": 5000,
        "savingsAccount": 5000,
      }
         ...
  ]
}
```

어느 시점에서 잔액이 마이너스인 모든 고객을 보여 주는 쿼리를 실행하려면 하위 쿼리가 있는 [EXISTS](sql-query-subquery.md#exists-expression)를 사용할 수 있습니다.

```sql
SELECT c.id
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.balance
    WHERE n.checkingAccount < 0
)
```

## <a name="reserved-keywords-and-special-characters-in-json"></a>JSON의 예약 키워드 및 특수 문자

따옴표가 붙은 속성 연산자 `[]`를 사용하여 속성에 액세스할 수 있습니다. 예를 들어 `SELECT c.grade` and `SELECT c["grade"]` 와 동일합니다. 이 구문은 공백, 특수 문자가 포함되어 있거나 SQL 키워드 또는 예약어와 이름이 동일한 속성을 이스케이프해야 할 때 유용합니다.

예를 들어 `order`라는 속성이 있는 문서와 특수 문자를 포함하는 속성 `price($)`가 있습니다.

```json
{
  "id": "AndersenFamily",
  "order": {
         "orderId": "12345",
         "productId": "A17849",
         "price($)": 59.33
   },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

`order` 속성이나 `price($)` 속성을 포함하는 쿼리를 실행하면 구문 오류가 발생합니다.

```sql
SELECT * FROM c where c.order.orderid = "12345"
```

```sql
SELECT * FROM c where c.order.price($) > 50
```

결과는 다음과 같습니다.

`
Syntax error, incorrect syntax near 'order'
`

다음과 같이 동일한 쿼리를 다시 작성해야 합니다.

```sql
SELECT * FROM c WHERE c["order"].orderId = "12345"
```

```sql
SELECT * FROM c WHERE c["order"]["price($)"] > 50
```

## <a name="json-expressions"></a>JSON 식

다음 예제와 같이 프로젝션은 JSON 식도 지원합니다.

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

결과는 다음과 같습니다.

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle",
        "name": "AndersenFamily"
      }
    }]
```

앞의 예제에서 `SELECT` 절은 JSON 개체를 만들어야 하며, 샘플에서 키를 제공하지 않으므로 절은 암시적 인수 변수 이름 `$1`을 사용합니다. 다음 쿼리는 `$1` 및 `$2`라는 두 개의 암시적 인수 변수를 반환합니다.

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

결과는 다음과 같습니다.

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle"
      },
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="aliasing"></a>별칭 지정

쿼리에서 별칭 값을 명시적으로 지정할 수 있습니다. 쿼리에 이름이 동일한 2개의 속성이 있을 경우 프로젝션된 결과에서 구분되도록 별칭 지정을 사용하여 속성 중 하나 또는 둘 다의 이름을 바꾸어야 합니다.

### <a name="examples"></a>예

두 번째 값을 `NameInfo`로 프로젝션할 때 다음 예제와 같이 별칭 지정에 사용되는 `AS` 키워드는 선택 사항입니다.

```sql
    SELECT
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

결과는 다음과 같습니다.

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

### <a name="aliasing-with-reserved-keywords-or-special-characters"></a>예약 키워드 또는 특수 문자로 별칭 지정

별칭 지정을 사용하여 공백, 특수 문자 또는 예약어를 포함하는 속성 이름으로 값을 프로젝션할 수 없습니다. 값의 프로젝션을 변경하려는 경우(예: 공백이 있는 속성 이름) [JSON 식](#json-expressions)을 사용할 수 있습니다.

예를 들면 다음과 같습니다.

```sql
    SELECT
           {"JSON expression with a space": { "state": f.address.state, "city": f.address.city }},
           {"JSON expression with a special character!": { "name": f.id }}
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

## <a name="next-steps"></a>다음 단계

- [시작](sql-query-getting-started.md)
- [SELECT 절](sql-query-select.md)
- [WHERE 절](sql-query-where.md)
