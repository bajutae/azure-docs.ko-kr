---
title: 변경 피드를 사용 하 여 종단 간 Azure Cosmos DB Java SDK v4 응용 프로그램 샘플 만들기
description: 이 가이드에서는 변경 피드를 사용 하 여 컨테이너의 구체화 된 뷰를 유지 하면서 문서를 Azure Cosmos DB 컨테이너에 삽입 하는 간단한 Java SQL API 응용 프로그램을 안내 합니다.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: anfeldma
ms.openlocfilehash: 5e8656e891d250547174aa3deb27a94eebaa0ba3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125675"
---
# <a name="how-to-create-a-java-application-that-uses-azure-cosmos-db-sql-api-and-change-feed-processor"></a>Azure Cosmos DB SQL API 및 변경 피드 프로세서를 사용 하는 Java 응용 프로그램을 만드는 방법

> [!IMPORTANT]  
> Java SDK v4 Azure Cosmos DB에 대 한 자세한 내용은 java sdk v4 릴리스 정보, [Maven 리포지토리](https://mvnrepository.com/artifact/com.azure/azure-cosmos), AZURE COSMOS DB java sdk v4 [성능 팁](performance-tips-java-sdk-v4-sql.md)및 Azure Cosmos DB java sdk v4 [문제 해결 가이드](troubleshoot-java-sdk-v4-sql.md)를 Azure Cosmos DB 참조 하세요.
>

이 방법 가이드에서는 변경 피드 및 변경 피드 프로세서를 사용 하 여 컨테이너의 구체화 된 뷰를 유지 하면서 Azure Cosmos DB SQL API를 사용 하 여 Azure Cosmos DB 컨테이너에 문서를 삽입 하는 간단한 Java 응용 프로그램을 안내 합니다. Java 응용 프로그램은 Azure Cosmos DB Java SDK v4를 사용 하 여 Azure Cosmos DB SQL API와 통신 합니다.

## <a name="prerequisites"></a>필수 구성 요소

* Azure Cosmos DB 계정의 URI 및 키

* Maven

* Java 8

## <a name="background"></a>배경

Azure Cosmos DB 변경 피드는 문서 삽입에 대 한 응답으로 동작을 트리거하는 이벤트 기반 인터페이스를 제공 합니다. 많은 용도로 사용됩니다. 예를 들어 읽기 및 쓰기가 많은 응용 프로그램의 경우, 변경 피드를 사용 하는 것이 수집 문서 이므로 컨테이너의 실시간 **구체화 된 뷰** 를 만드는 것이 가장 좋습니다. 구체화된 뷰 컨테이너는 동일한 데이터를 보유하지만 효율적인 읽기를 위해 분할되어 애플리케이션의 읽기 및 쓰기를 효율적으로 만듭니다.

변경 피드 이벤트를 관리 하는 작업은 주로 SDK에 기본 제공 되는 변경 피드 프로세서 라이브러리를 통해 처리 됩니다. 이 라이브러리는 여러 작업자에 게 변경 피드 이벤트를 배포 하는 데 필요한 경우에만 강력한 기능을 제공 합니다. 변경 피드 라이브러리에 콜백을 제공 하면 됩니다.

이 간단한 예제에서는 단일 작업자를 사용 하 여 구체화 된 뷰에서 문서를 만들고 삭제 하는 변경 피드 프로세서 라이브러리를 보여 줍니다.

## <a name="setup"></a>설치 프로그램

아직 수행하지 않은 경우 앱 예제 리포지토리를 복제합니다.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example.git
```

리포지토리 디렉터리에서 터미널을 엽니다. 다음을 실행하여 앱을 빌드합니다.

```bash
mvn clean package
```

## <a name="walkthrough"></a>연습

1. 첫 번째 확인으로, Azure Cosmos DB 계정이 있어야 합니다. 브라우저에서 **Azure Portal** 를 열고 Azure Cosmos DB 계정으로 이동한 다음 왼쪽 창에서 **데이터 탐색기**으로 이동 합니다.

    ![Azure Cosmos DB 계정](media/create-sql-api-java-changefeed/cosmos_account_empty.JPG)

1. 다음 명령을 사용하여 터미널에서 앱을 실행합니다.

    ```bash
    mvn exec:java -Dexec.mainClass="com.azure.cosmos.workedappexample.SampleGroceryStore" -DACCOUNT_HOST="your-account-uri" -DACCOUNT_KEY="your-account-key" -Dexec.cleanupDaemonThreads=false
    ```

1. 다음 내용이 보이면 Enter 키를 누른 다음,

    ```bash
    Press enter to create the grocery store inventory system...
    ```

    그런 다음 브라우저에서 Azure Portal 데이터 탐색기로 돌아갑니다. **GroceryStoreDatabase** 데이터베이스가 빈 컨테이너 세 개와 함께 추가된 것을 볼 수 있습니다. 

    * **InventoryContainer** - UUID인 ```id``` 항목으로 분할되어 있는 예제 식료품점의 인벤토리 레코드입니다.
    * **InventoryContainer-pktype** - ```type``` 항목에 대한 쿼리에 최적화된 인벤토리 레코드의 구체화된 뷰입니다.
    * **InventoryContainer** -임대 컨테이너는 항상 변경 피드에 필요 합니다. 임대는 변경 피드를 읽는 동안 앱의 진행 상황을 추적 합니다.


    ![빈 컨테이너](media/create-sql-api-java-changefeed/cosmos_account_resources_lease_empty.JPG)


1. 이제 터미널에 프롬프트가 보입니다.

    ```bash
    Press enter to start creating the materialized view...
    ```

    Enter 키를 누릅니다. 이제 다음 코드 블록은 다른 스레드에서 변경 피드 프로세서를 실행 하 고 초기화 합니다. 

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

    ```java
    changeFeedProcessorInstance = getChangeFeedProcessor("SampleHost_1", feedContainer, leaseContainer);
    changeFeedProcessorInstance.start()
        .subscribeOn(Schedulers.elastic())
        .doOnSuccess(aVoid -> {
            isProcessorRunning.set(true);
        })
        .subscribe();

    while (!isProcessorRunning.get()); //Wait for change feed processor start
    ```

    ```"SampleHost_1"```은 변경 피드 프로세서 작업자의 이름입니다. ```changeFeedProcessorInstance.start()```는 변경 피드 프로세서를 실제로 시작합니다.

    브라우저에서 Azure Portal 데이터 탐색기로 돌아갑니다. **InventoryContainer-leases** 컨테이너에서 **항목**을 클릭하여 콘텐츠를 봅니다. 변경 피드 프로세서가 임대 컨테이너를 채운 것이 보입니다. 즉, 프로세서가 ```SampleHost_1``` 작업자에게 **InventoryContainer**의 일부 파티션에 대한 임대를 할당했습니다.

    ![임대](media/create-sql-api-java-changefeed/cosmos_leases.JPG)

1. 터미널에서 Enter를 다시 누릅니다. 그러면 **InventoryContainer**에 문서 10개가 삽입되도록 트리거됩니다. 각 문서 삽입은 변경 피드에 JSON으로 나타납니다. 다음 콜백 코드는 JSON 문서를 구체화 된 뷰로 미러링 하 여 이러한 이벤트를 처리 합니다.

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

    ```java
    public static ChangeFeedProcessor getChangeFeedProcessor(String hostName, CosmosAsyncContainer feedContainer, CosmosAsyncContainer leaseContainer) {
        ChangeFeedProcessorOptions cfOptions = new ChangeFeedProcessorOptions();
        cfOptions.setFeedPollDelay(Duration.ofMillis(100));
        cfOptions.setStartFromBeginning(true);
        return ChangeFeedProcessor.changeFeedProcessorBuilder()
            .setOptions(cfOptions)
            .setHostName(hostName)
            .setFeedContainer(feedContainer)
            .setLeaseContainer(leaseContainer)
            .setHandleChanges((List<JsonNode> docs) -> {
                for (JsonNode document : docs) {
                        //Duplicate each document update from the feed container into the materialized view container
                        updateInventoryTypeMaterializedView(document);
                }

            })
            .build();
    }

    private static void updateInventoryTypeMaterializedView(JsonNode document) {
        typeContainer.upsertItem(document).subscribe();
    }
    ```

1. 코드가 5~10초 동안 실행되도록 합니다. 그런 다음 Azure Portal 데이터 탐색기으로 돌아가서 **InventoryContainer > 항목**으로 이동 합니다. 인벤토리 컨테이너에 항목이 삽입되는 것이 보입니다. 파티션 키(```id```)에 유의합니다.

    ![피드 컨테이너](media/create-sql-api-java-changefeed/cosmos_items.JPG)

1. 이제 Data Explorer에서 **InventoryContainer-pktype > 항목**으로 이동합니다. 이 뷰는 변경 피드를 통해 프로그래밍 방식으로 삽입 되었기 때문에이 컨테이너 미러의 **InventoryContainer** 항목에 대 한 구체화 된 뷰입니다. 파티션 키(```type```)에 유의합니다. 이 구체화된 뷰는 ```type```에 대한 쿼리 필터링에 최적화되어 있는데, ```id```으로 분할되어 있기 때문에 **InventoryContainer**에는 비효율적입니다.

    ![구체화된 뷰](media/create-sql-api-java-changefeed/cosmos_materializedview2.JPG)

1. 단일 ```upsertItem()``` 호출을 사용하여 **InventoryContainer**와 **InventoryContainer-pktype**에서 문서를 삭제하겠습니다. 먼저 Azure Portal 데이터 탐색기를 살펴보겠습니다. ```/type == "plums"```인 문서를 삭제하겠습니다. 아래에 빨간색으로 둘러싸여 있습니다

    ![구체화된 뷰](media/create-sql-api-java-changefeed/cosmos_materializedview-emph-todelete.JPG)

    Enter 키를 다시 눌러서 예제 코드에서 ```deleteDocument()``` 함수를 호출합니다. 아래에 표시된 이 함수는 문서 TTL(Time to Live)을 5초로 설정하는 ```/ttl == 5```를 사용하여 새 버전의 문서를 upsert합니다. 
    
    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

    ```java
    public static void deleteDocument() {

        String jsonString =    "{\"id\" : \"" + idToDelete + "\""
                + ","
                + "\"brand\" : \"Jerry's\""
                + ","
                + "\"type\" : \"plums\""
                + ","
                + "\"quantity\" : \"50\""
                + ","
                + "\"ttl\" : 5"
                + "}";

        ObjectMapper mapper = new ObjectMapper();
        JsonNode document = null;

        try {
            document = mapper.readTree(jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }

        feedContainer.upsertItem(document,new CosmosItemRequestOptions()).block();
    }    
    ```

    변경 피드가 ```feedPollDelay``` 100ms로 설정 되어 있으므로 변경 피드가 위에 표시 된 거의 즉시이 업데이트에 응답 합니다 ```updateInventoryTypeMaterializedView()``` . 마지막 함수 호출은 **InventoryContainer-pktype**에 TTL이 5초인 새 문서를 upsert합니다.

    이렇게 하면 약 5초 후에 문서가 만료되고 두 컨테이너에서 삭제됩니다.

    이 절차는 변경 피드가 항목을 삭제 하는 것이 아니라 항목 삽입 또는 업데이트에 대 한 이벤트만 발급 하기 때문에 필요 합니다.

1. Enter를 한 번 더 눌러서 프로그램을 닫고 리소스를 정리합니다.
