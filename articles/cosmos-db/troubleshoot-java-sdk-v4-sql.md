---
title: Java SDK v4 Azure Cosmos DB 진단 및 문제 해결
description: 클라이언트 쪽 로깅 및 기타 타사 도구와 같은 기능을 사용 하 여 Java SDK v4의 Azure Cosmos DB 문제를 식별, 진단 및 해결 합니다.
author: anfeldma-ms
ms.service: cosmos-db
ms.date: 05/08/2020
ms.author: anfeldma
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.openlocfilehash: bdec785ccec2c388eb737da3ec494b525941e2a6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982601"
---
# <a name="troubleshoot-issues-when-you-use-azure-cosmos-db-java-sdk-v4-with-sql-api-accounts"></a>SQL API 계정에서 Azure Cosmos DB Java SDK v4를 사용할 때 발생 하는 문제 해결

> [!div class="op_single_selector"]
> * [Java SDK v4](troubleshoot-java-sdk-v4-sql.md)
> * [Async Java SDK v2](troubleshoot-java-async-sdk.md)
> * [.NET](troubleshoot-dot-net-sdk.md)
> 

> [!IMPORTANT]
> 이 문서에서는 Azure Cosmos DB Java SDK v4에 대 한 문제 해결에 대해서만 다룹니다. 자세한 내용은 Azure Cosmos DB Java SDK v4 릴리스 정보, [Maven 리포지토리](https://mvnrepository.com/artifact/com.azure/azure-cosmos)및 [성능 팁](performance-tips-java-sdk-v4-sql.md) 을 참조 하세요. 현재 v4 보다 이전 버전을 사용 하는 경우 v4로 업그레이드 하는 데 도움이 필요한 경우 [Azure Cosmos DB JAVA SDK v4로 마이그레이션](migrate-java-v4-sdk.md) 가이드를 참조 하세요.
>

이 문서에서는 Azure Cosmos DB SQL API 계정을 사용 하 여 Azure Cosmos DB Java SDK v4를 사용 하는 경우 일반적인 문제, 해결 방법, 진단 단계 및 도구에 대해 설명 합니다.
Azure Cosmos DB Java SDK v4는 Azure Cosmos DB SQL API에 액세스 하는 클라이언트 쪽 논리적 표현을 제공 합니다. 이 문서에서는 문제가 발생하는 경우 사용자에게 도움이 되는 도구 및 방법을 설명합니다.

이 목록을 사용하여 시작합니다.

* 이 문서의 [일반적인 문제 및 해결 방법] 섹션을 살펴봅니다.
* [GitHub에서 사용 가능한 오픈 소스](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos)인 Azure Cosmos DB 중앙 리포지토리의 Java SDK를 살펴보세요. 여기에는 능동적으로 모니터링되는 [문제 섹션](https://github.com/Azure/azure-sdk-for-java/issues)이 있습니다. 해결 방법이 있는 유사한 문제가 이미 제출되었는지 확인합니다. 한 가지 유용한 팁은 *cosmos: v4 항목* 태그를 기준으로 문제를 필터링 하는 것입니다.
* Azure Cosmos DB Java SDK v4에 대 한 [성능 팁](performance-tips-java-sdk-v4-sql.md) 을 검토 하 고 제안 된 방법을 따릅니다.
* 솔루션을 찾지 못한 경우 이 문서의 나머지 부분을 읽어봅니다. 그런 후, [GitHub 문제](https://github.com/Azure/azure-sdk-for-java/issues)를 제출합니다. GitHub 문제에 태그를 추가 하는 옵션이 있는 경우 *cosmos: v4 항목* 태그를 추가 합니다.

## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>일반적인 이슈 및 해결 방법

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>네트워크 문제, Netty 읽기 시간 제한 오류, 낮은 처리량, 높은 대기 시간

#### <a name="general-suggestions"></a>일반 제안
최상의 성능을 얻으려면:
* 앱이 Azure Cosmos DB 계정과 동일한 지역에서 실행되고 있는지 확인합니다. 
* 앱이 실행되는 호스트에서 CPU 사용량을 확인합니다. CPU 사용량이 50% 이상인 경우 더 높은 구성의 호스트에서 앱을 실행 합니다. 또는 더 많은 컴퓨터에서 부하를 분산할 수 있습니다.
    * Azure Kubernetes Service에서 응용 프로그램을 실행 하는 경우 [Azure Monitor를 사용 하 여 CPU 사용률을 모니터링할](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-analyze)수 있습니다.

#### <a name="connection-throttling"></a>연결 제한
연결 제한은 [호스트 컴퓨터의 연결 제한] 또는 [Azure SNAT(PAT) 포트 고갈] 중 하나로 인해 발생할 수 있습니다.

##### <a name="connection-limit-on-a-host-machine"></a><a name="connection-limit-on-host"></a>호스트 컴퓨터의 연결 제한
일부 Linux 시스템(예: 'Red Hat')에는 열려 있는 파일의 총 수에 상한이 있습니다. Linux의 소켓은 파일로 구현되므로 이 숫자는 총 연결 수도 제한합니다.
다음 명령을 실행합니다.

```bash
ulimit -a
```
"nofile"로 식별되는 허용되는 열린 파일의 최대 수는 연결 풀 크기의 두 배 이상이어야 합니다. 자세한 내용은 Azure Cosmos DB Java SDK v4 [성능 팁](performance-tips-java-sdk-v4-sql.md)을 참조 하세요.

##### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT(PAT) 포트 고갈

공용 IP 주소 없이 앱이 Azure Virtual Machines에 배포되는 경우 기본적으로 [Azure SNAT 포트](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports)는 VM 외부의 모든 엔드포인트에 대한 연결을 설정하는 데 사용됩니다. VM에서 Azure Cosmos DB 엔드포인트로 허용되는 연결 수는 [Azure SNAT 구성](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports)으로 제한됩니다.

 Azure SNAT 포트는 VM에 개인 IP 주소 및 VM에서 공용 IP 주소에 연결하려고 하는 프로세스가 있는 경우에만 사용됩니다. Azure SNAT 제한을 피하는 두 가지 해결 방법이 있습니다.

* Azure Virtual Machines 가상 네트워크의 서브넷에 Azure Cosmos DB 서비스 엔드포인트를 추가합니다. 자세한 내용은 [Azure Virtual Network 서비스 엔드포인트](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)를 참조하세요. 

    서비스 엔드포인트를 사용하도록 설정한 경우 요청이 더 이상 공용 IP에서 Azure Cosmos DB로 전송되지 않습니다. 대신 가상 네트워크 및 서브넷 ID가 전송됩니다. 공용 IP만 허용되는 경우 이 변경 내용으로 인해 방화벽이 삭제될 수 있습니다. 방화벽을 사용하는 경우 서비스 엔드포인트를 사용하도록 설정하면 [Virtual Network ACL](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl)을 사용하여 방화벽에 서브넷을 추가합니다.
* Azure VM에 공용 IP를 할당합니다.

##### <a name="cant-reach-the-service---firewall"></a><a name="cant-connect"></a>서비스에 연결할 수 없음-방화벽
``ConnectTimeoutException``SDK에서 서비스에 연결할 수 없음을 나타냅니다.
직접 모드를 사용 하는 경우 다음과 유사한 오류가 발생할 수 있습니다.
```
GoneException{error=null, resourceAddress='https://cdb-ms-prod-westus-fd4.documents.azure.com:14940/apps/e41242a5-2d71-5acb-2e00-5e5f744b12de/services/d8aa21a5-340b-21d4-b1a2-4a5333e7ed8a/partitions/ed028254-b613-4c2a-bf3c-14bd5eb64500/replicas/131298754052060051p//', statusCode=410, message=Message: The requested resource is no longer available at the server., getCauseInfo=[class: class io.netty.channel.ConnectTimeoutException, message: connection timed out: cdb-ms-prod-westus-fd4.documents.azure.com/101.13.12.5:14940]
```

앱 컴퓨터에서 방화벽이 실행 되 고 있는 경우 직접 모드에서 사용 되는 포트 범위 1만 ~ 2만를 엽니다.
또한 [호스트 컴퓨터에 대 한 연결 제한을](#connection-limit-on-host)따릅니다.

#### <a name="http-proxy"></a>HTTP 프록시

HTTP 프록시를 사용하는 경우 SDK `ConnectionPolicy`에서 구성된 연결 수를 지원할 수 있는지 확인합니다.
그렇지 않으면 연결 문제가 발생할 수 있습니다.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>잘못된 코딩 패턴: Netty IO 스레드 차단

SDK는 [Netty](https://netty.io/) IO 라이브러리를 사용하여 Azure Cosmos DB와 통신합니다. SDK에는 비동기 API가 있으며 Netty의 비차단 IO Api가 사용 됩니다. SDK의 IO 작업은 IO Netty 스레드에서 수행됩니다. IO Netty 스레드의 수는 앱 머신의 CPU 코어 수와 동일하게 구성됩니다. 

Netty IO 스레드는 비차단 Netty IO 작업에만 사용해야 합니다. SDK는 앱의 코드에 대한 Netty IO 스레드 중 하나에서 API 호출 결과를 반환합니다. 앱이 Netty 스레드에서 결과를 수신한 후 오래 지속되는 작업을 수행하는 경우 SDK는 해당 내부 IO 작업을 수행하기에 충분한 IO 스레드가 없을 수도 있습니다. 이러한 앱 코딩으로 인해 낮은 처리량, 높은 대기 시간 및 `io.netty.handler.timeout.ReadTimeoutException` 실패가 발생할 수 있습니다. 해결 방법은 작업에 시간이 걸리는 것을 알고 있는 경우 스레드를 전환하는 것입니다.

예를 들어 컨테이너에 항목을 추가 하는 다음 코드 조각을 살펴보겠습니다 (데이터베이스 및 컨테이너 설정에 대 한 지침은 [여기](create-sql-api-java.md) 참조). Netty 스레드에서 몇 밀리초 넘게 소요 되는 오래 지속 되는 작업을 수행할 수 있습니다. 이 경우 결과적으로 IO 작업을 처리하기 위한 Netty IO 스레드가 없는 상태가 될 수 있습니다. 따라서 ReadTimeoutException 오류가 발생합니다.

### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-readtimeout"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
  int requestTimeoutInSeconds = 10;
  ConnectionPolicy policy = new ConnectionPolicy();
  policy.setRequestTimeout(Duration.ofMillis(requestTimeoutInSeconds * 1000));
  AtomicInteger failureCount = new AtomicInteger();
  // Max number of concurrent item inserts is # CPU cores + 1
  Flux<Family> familyPub = 
      Flux.just(Families.getAndersenFamilyItem(), Families.getWitherspoonFamilyItem(), Families.getCarltonFamilyItem());
  familyPub.flatMap(family -> {
      return container.createItem(family);
  }).flatMap(r -> {
      try {
          // Time-consuming work is, for example,
          // writing to a file, computationally heavy work, or just sleep.
          // Basically, it's anything that takes more than a few milliseconds.
          // Doing such operations on the IO Netty thread
          // without a proper scheduler will cause problems.
          // The subscriber will get a ReadTimeoutException failure.
          TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
      } catch (Exception e) {
      }
      return Mono.empty();
  }).doOnError(Exception.class, exception -> {
      failureCount.incrementAndGet();
  }).blockLast();
  assert(failureCount.get() > 0);
}
```

해결 방법은 시간이 걸리는 작업을 수행하는 스레드를 변경하는 것입니다. 앱에 대한 스케줄러의 싱글톤 인스턴스를 정의합니다.

### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-scheduler"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = Schedulers.fromExecutor(ex);
```
계산이 많은 작업 또는 차단 IO 작업과 같이 시간이 걸리는 작업을 수행해야 할 수 있습니다. 이 경우 `.publishOn(customScheduler)` API를 사용하여 `customScheduler`가 제공하는 작업자로 스레드를 전환합니다.

### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-apply-custom-scheduler"></a>Java SDK V4 (Maven com. azure:: azure-cosmos) Async API

```java
container.createItem(family)
    .publishOn(customScheduler) // Switches the thread.
    .subscribe(
        // ...
    );
```
`publishOn(customScheduler)`을 사용하여 Netty IO 스레드를 해제하고 사용자 지정 스케줄러에서 제공한 사용자 고유의 사용자 지정 스레드로 전환합니다. 이와 같이 수정하면 문제가 해결됩니다. 이제 더 이상 `io.netty.handler.timeout.ReadTimeoutException` 오류가 발생하지 않습니다.

### <a name="request-rate-too-large"></a>너무 많은 요청 빈도
이 오류는 서버 쪽 오류입니다. 프로비전된 처리량을 다 사용했음을 나타냅니다. 나중에 다시 시도하세요. 이 오류가 자주 발생하는 경우 컬렉션 처리량을 늘리는 것이 좋습니다.

* **getRetryAfterInMilliseconds 간격으로 백오프 구현**

    성능 테스트 중에는 작은 비율의 요청이 제한될 때까지 로드를 늘려야 합니다. 제한될 경우 클라이언트 애플리케이션은 서버에서 지정한 재시도 간격 제한을 백오프해야 합니다. 백오프를 통해 재시도 간 기간을 최소화할 수 있습니다.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Azure Cosmos DB 에뮬레이터 연결 오류

Azure Cosmos DB 에뮬레이터 HTTPS 인증서는 자체 서명입니다. SDK를 에뮬레이터와 함께 사용하려면 에뮬레이터 인증서를 Java TrustStore로 가져와야 합니다. 자세한 내용은 [Azure Cosmos DB 에뮬레이터 인증서 내보내기](local-emulator-export-ssl-certificates.md)를 참조하세요.

### <a name="dependency-conflict-issues"></a>종속성 충돌 문제

Azure Cosmos DB Java SDK는 많은 종속성을 가져옵니다. 일반적으로 프로젝트 종속성 트리에는 Java SDK Azure Cosmos DB에 종속 된 이전 버전의 아티팩트가 포함 된 경우 응용 프로그램을 실행할 때 예기치 않은 오류가 발생할 수 있습니다. 응용 프로그램이 예기치 않게 예외를 throw 하는 이유를 디버깅 하는 경우 종속성 트리가 실수로 이전 버전의 Azure Cosmos DB Java SDK 종속성을 가져오지 않았는지 다시 한 번 확인 하는 것이 좋습니다.

이러한 문제에 대 한 해결 방법은 이전 버전에서 가져온 프로젝트 종속성을 식별 하 고 해당 이전 버전에 대 한 전이적 종속성을 제외 하 고 Azure Cosmos DB Java SDK에서 최신 버전을 가져올 수 있도록 하는 것입니다.

Azure Cosmos DB Java SDK가 종속 된 이전 버전의 프로젝트 종속성이 무엇 인지 식별 하려면 다음 명령을 프로젝트 pom .xml 파일에 대해 실행 합니다.
```bash
mvn dependency:tree
```
자세한 내용은 [maven 종속성 트리 가이드](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html)를 참조 하세요.

이전 버전에 따라 달라 지는 프로젝트의 종속성을 확인 한 후에는 다음 예제에 따라 pom 파일에서 해당 lib에 대 한 종속성을 수정 하 고 전이적 종속성을 제외할 수 있습니다. 즉, *reactor-core* 가 오래 된 종속성 이라고 가정 합니다.

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-reactor}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-reactor}</artifactId>
  <version>${version-of-lib-which-brings-in-reactor}</version>
  <exclusions>
    <exclusion>
      <groupId>io.projectreactor</groupId>
      <artifactId>reactor-core</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

자세한 내용은 [전이적 종속성 제외 가이드](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html)를 참조 하세요.


## <a name="enable-client-sdk-logging"></a><a name="enable-client-sice-logging"></a>클라이언트 SDK 로깅 사용

Azure Cosmos DB Java SDK v4는 log4j 및 logback 같은 인기 있는 로깅 프레임 워크로 로그인을 지 원하는 로깅 외관으로 SLF4j을 사용 합니다.

예를 들어 로깅 프레임워크로 log4j를 사용하려는 경우 다음 라이브러리를 Java 클래스 경로에 추가합니다.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

log4j 구성도 추가합니다.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

자세한 내용은 [sfl4j 로깅 설명서](https://www.slf4j.org/manual.html)를 참조하세요.

## <a name="os-network-statistics"></a><a name="netstats"></a>OS 네트워크 통계
netstat 명령을 실행하여 `ESTABLISHED` 및 `CLOSE_WAIT`와 같은 상태에 있는 연결 수를 확인합니다.

Linux에서 다음 명령을 실행할 수 있습니다.
```bash
netstat -nap
```

Windows에서는 다른 인수 플래그를 사용 하 여 동일한 명령을 실행할 수 있습니다.
```bash
netstat -abn
```

Azure Cosmos DB 엔드포인트에 대한 연결로만 결과를 필터링합니다.

`ESTABLISHED` 상태에서 Azure Cosmos DB 엔드포인트에 대한 연결 수는 구성된 연결 풀 크기보다 크지 않아야 합니다.

Azure Cosmos DB 엔드포인트에 대한 많은 연결이 `CLOSE_WAIT` 상태일 수 있습니다. 1,000개가 넘을 수 있습니다. 이렇게 숫자가 높으면 연결이 설정되었다가 빠르게 삭제되는 것을 나타냅니다. 이 경우 잠재적으로 문제가 발생할 수 있습니다. 자세한 내용은 [일반적인 문제 및 해결 방법] 섹션을 참조하세요.

 <!--Anchors-->
[일반적인 이슈 및 해결 방법]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[호스트 컴퓨터의 연결 제한]: #connection-limit-on-host
[Azure SNAT (PAT) 포트 고갈]: #snat


