---
title: 지속성 함수의 단일 항목 - Azure
description: Azure Functions의 Durable Functions 확장에서 싱글톤을 사용하는 방법을 설명합니다.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 4eff7c4c91ed664fcf1f4fc7a8be2d43d24e5c6b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76262812"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>지속성 함수의 단일 항목 오케스트레이터(Azure Functions)

백그라운드 작업의 경우 특정 orchestrator의 인스턴스가 한 번에 하나만 실행 되도록 해야 하는 경우가 많습니다. 특정 인스턴스 ID를 만들 때 orchestrator에 할당 하 여 [Durable Functions](durable-functions-overview.md) 에서 이러한 유형의 singleton 동작을 확인할 수 있습니다.

## <a name="singleton-example"></a>단일 항목 예제

다음 예에서는 단일 백그라운드 작업 오케스트레이션을 만드는 HTTP 트리거 함수를 보여 줍니다. 이 코드는 지정한 인스턴스 ID에 대해 인스턴스가 하나만 존재하는지 확인합니다.

# <a name="c"></a>[C#](#tab/csharp)

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

> [!NOTE]
> 이전 c # 코드는 Durable Functions 2.x에 대 한 것입니다. 1.x `OrchestrationClient` Durable Functions의 경우 `DurableClient` 특성 대신 특성을 사용 해야 하며 대신 `DurableOrchestrationClient` 매개 변수 형식을 사용 해야 합니다. `IDurableOrchestrationClient` 버전 간의 차이점에 대 한 자세한 내용은 [Durable Functions 버전](durable-functions-versions.md) 문서를 참조 하세요.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

**function.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

---

기본적으로 인스턴스 ID는 임의로 GUID에서 생성됩니다. 그러나 이전 예제에서 인스턴스 ID는 URL의 경로 데이터로 전달 됩니다. 코드는 ( `GetStatusAsync`c #) 또는 `getStatus` (JavaScript)를 호출 하 여 지정 된 ID를 가진 인스턴스가 이미 실행 중인지 확인 합니다. 이러한 인스턴스가 실행 되 고 있지 않으면 해당 ID를 사용 하 여 새 인스턴스를 만듭니다.

> [!NOTE]
> 이 샘플에는 잠재적 경합 상태가 있습니다. **HttpStartSingle**의 두 인스턴스가 동시에 실행되면 두 함수의 호출 모두에서 성공을 보고하지만 실제로는 하나의 오케스트레이션 인스턴스만 시작됩니다. 요구 사항에 따라 바람직하지 않은 부작용이 있을 수 있습니다. 이러한 이유로 두 요청이 이 트리거 함수를 동시에 실행할 수 없도록 하는 것이 중요합니다.

오 케 스트레이 터 함수의 구현 세부 정보는 실제로 중요 하지 않습니다. 시작하고 완료하는 일반 오케스트레이터 함수일 수 있거나 무기한 실행하는 오케스트레이터 함수일 수 있습니다(즉, [영구 오케스트레이션](durable-functions-eternal-orchestrations.md)). 중요한 점은 한 번에 하나의 인스턴스만 실행된다는 점입니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [오케스트레이션의 네이티브 HTTP 기능에 대해 알아보기](durable-functions-http-features.md)
