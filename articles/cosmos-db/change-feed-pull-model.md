---
title: 변경 피드 풀 모델
description: Azure Cosmos DB 변경 피드 끌어오기 모델을 사용하여 끌어오기 모델과 변경 피드 프로세서의 차이점 및 변경 피드를 읽는 방법 알아보기
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/10/2020
ms.reviewer: sngun
ms.openlocfilehash: 0e6e243ceb73ca2a1180e59ba6c6b4095ed6069a
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83116716"
---
# <a name="change-feed-pull-model-in-azure-cosmos-db"></a>Azure Cosmos DB의 변경 피드 끌어오기 모델

변경 피드 끌어오기 모델을 사용하면 Azure Cosmos DB 변경 피드를 원하는 속도로 사용할 수 있습니다. [변경 피드 프로세서](change-feed-processor.md)로 이미 수행하듯이, 변경 피드 끌어오기 모델을 사용하여 다수의 변경 피드 소비자에 대한 변경 처리를 병렬화할 수 있습니다.

> [!NOTE]
> 변경 피드 끌어오기 모델은 현재 [Azure Cosmos DB .NET SDK에서만 미리 보기](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview)로 제공됩니다. 다른 SDK 버전에서는 아직 미리 보기를 사용할 수 없습니다.

## <a name="consuming-an-entire-containers-changes"></a>전체 컨테이너의 변경 내용 사용

끌어오기 모델을 사용하여 변경 피드를 처리하는 `FeedIterator`를 만들 수 있습니다. `FeedIterator`를 처음 만들 때, `ChangeFeedRequestOptions` 내에 선택적 `StartTime`을 지정할 수 있습니다. 지정하지 않고 남겨두면 `StartTime`은 현재 시간이 됩니다.

`FeedIterator`는 두 가지 버전으로 제공됩니다. 엔터티 개체를 반환하는 아래 예제 외에, `Stream` 지원을 통해 응답을 얻을 수도 있습니다. 스트림을 사용하면 데이터를 먼저 역직렬화하지 않고도 읽을 수 있어서, 클라이언트 리소스를 절약할 수 있습니다.

엔터티 개체(이 경우, `User` 개체)를 반환하는 `FeedIterator`를 가져오는 예는 다음과 같습니다.

```csharp
FeedIterator<User> iteratorWithPOCOS = container.GetChangeFeedIterator<User>();
```

`Stream`을 반환하는 `FeedIterator`를 가져오는 예는 다음과 같습니다.

```csharp
FeedIterator iteratorWithStreams = container.GetChangeFeedStreamIterator();
```

`FeedIterator`를 사용하면, 전체 컨테이너의 변경 피드를 원하는 속도로 쉽게 처리할 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
FeedIterator<User> iteratorForTheEntireContainer= container.GetChangeFeedIterator(new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

while (iteratorForTheEntireContainer.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForTheEntireContainer.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="consuming-a-partition-keys-changes"></a>파티션 키의 변경 내용 사용

특정 파티션 키의 변경 내용만 처리하려는 경우가 있습니다. 특정 파티션 키에 대한 `FeedIterator`를 가져와서 전체 컨테이너와 같은 방식으로 변경 내용을 처리할 수 있습니다.

```csharp
FeedIterator<User> iteratorForThePartitionKey = container.GetChangeFeedIterator(new PartitionKey("myPartitionKeyValueToRead"), new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

while (iteratorForThePartitionKey.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForThePartitionKey.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="using-feedrange-for-parallelization"></a>병렬 처리에 FeedRange 사용

[변경 피드 프로세서](change-feed-processor.md)에서 작업은 여러 소비자에게 자동으로 분산됩니다. 변경 피드 끌어오기 모델에서 `FeedRange`를 사용하여 변경 피드에 대한 처리를 병렬화할 수 있습니다. `FeedRange`는 파티션 키 값의 범위를 나타냅니다.

컨테이너에 대한 범위 목록을 가져오는 방법을 보여주는 예는 다음과 같습니다.

```csharp
IReadOnlyList<FeedRange> ranges = await container.GetFeedRangesAsync();
```

컨테이너에 대한 FeedRanges 목록을 가져올 때 [실제 파티션](partition-data.md#physical-partitions)당 하나의 `FeedRange`를 가져옵니다.

`FeedRange`를 사용하면 여러 머신 또는 스레드의 변경 피드 처리를 병렬화하는 `FeedIterator`를 만들 수 있습니다. 전체 컨테이너에 대해 단일 `FeedIterator`를 얻는 방법을 보여주는 이전 예제와 달리`FeedRange`를 사용하면 변경 피드를 병렬로 처리할 수 있는 여러 FeedIterator를 얻을 수 있습니다.

FeedRanges를 사용하려는 경우에는 FeedRanges를 가져와서 해당 머신에 배포하는 오케스트레이터 프로세스가 필요합니다. 이러한 배포는 다음과 같을 수 있습니다.

* `FeedRange.ToJsonString`을 사용하고 이 문자열 값을 배포합니다. 소비자는 이 값을 `FeedRange.FromJsonString`과 함께 사용할 수 있습니다.
* 배포를 처리 중이면 `FeedRange` 개체 참조를 전달하는 것입니다.

다음은 병렬로 읽는 개별 가상 머신 두 대를 사용하여 컨테이너의 변경 피드 시작 부분부터 읽는 방법을 보여주는 샘플입니다.

머신 1:

```csharp
FeedIterator<User> iteratorA = container.GetChangeFeedIterator<Person>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorA.HasMoreResults)
{
   FeedResponse<User> users = await iteratorA.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

머신 2:

```csharp
FeedIterator<User> iteratorB = container.GetChangeFeedIterator<User>(ranges[1], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorB.HasMoreResults)
{
   FeedResponse<User> users = await iteratorB.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="saving-continuation-tokens"></a>연속 토큰 저장

연속 토큰을 생성하여 `FeedIterator`의 위치를 저장할 수 있습니다. 연속 토큰은 FeedIterator가 마지막으로 처리한 변경 내용의 지점을 추적하는 문자열 값입니다. 이를 통해, 나중에 해당 지점에서 `FeedIterator`를 재개할 수 있습니다. 다음은 컨테이너를 만든 이후의 변경 피드를 읽는 코드입니다. 변경 내용이 더 이상 없으면 연속 토큰을 유지하여 변경 피드 사용을 나중에 재개할 수 있도록 합니다.

```csharp
FeedIterator<User> iterator = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

string continuation = null;

while (iterator.HasMoreResults)
{
   FeedResponse<User> users = await iterator.ReadNextAsync();
   continuation = users.ContinuationToken;

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}

// Some time later
FeedIterator<User> iteratorThatResumesFromLastPoint = container.GetChangeFeedIterator<User>(continuation);
```

## <a name="comparing-with-change-feed-processor"></a>변경 피드 프로세서와 비교

많은 시나리오에서 [변경 피드 프로세서](change-feed-processor.md) 또는 끌어오기 모델을 사용하여 변경 피드를 처리할 수 있습니다. 끌어오기 모델의 연속 토큰 및 변경 피드 프로세서의 임대 컨테이너는 변경 피드에서 마지막으로 처리된 항목(또는 항목의 모음)에 대한 "책갈피"입니다.
하지만 연속 토큰을 임대 컨테이너로(또는 그 반대로) 변환할 수 없습니다.

다음 시나리오에서는 끌어오기 모델 사용을 고려해야 합니다.

- 변경 피드의 기존 데이터를 한 번만 읽으려고 합니다.
- 특정 파티션 키의 변경 내용만 읽으려고 합니다.
- 밀어넣기 모델을 원하지 않으며, 변경 피드를 내 속도에 맞게 사용하려고 합니다.

변경 피드 프로세서와 끌어오기 모델의 주요 차이는 다음과 같습니다.

|  | 변경 피드 프로세서| 끌어오기 모델 |
| --- | --- | --- |
| 처리 중인 변경 피드의 현재 지점 추적 | 임대(Azure Cosmos DB 컨테이너에 저장됨) | 연속 토큰(메모리에 저장되거나 수동으로 유지됨) |
| 이전 변경 내용 재생 가능 여부 | 예, 밀어넣기 모델 사용 | 예, 끌어오기 모델 사용|
| 이후 변경 내용 폴링 | 사용자 지정 `WithPollInterval`에 기반하여 변경 내용을 자동으로 확인 | 설명서 |
| 전체 컨테이너의 변경 내용 처리 | 예, 동일한 컨테이너에서 사용하는 여러 스레드/머신을 자동으로 병렬화| 예, FeedToken을 사용하여 수동으로 병렬화 |
| 단일 파티션 키의 변경 내용 처리 | 지원되지 않음 | 예|
| 지원 수준 | 일반 공급 | 미리 보기 |

## <a name="next-steps"></a>다음 단계

* [변경 피드 개요](change-feed.md)
* [변경 피드 프로세서 사용](change-feed-processor.md)
* [Azure Functions 트리거](change-feed-functions.md)