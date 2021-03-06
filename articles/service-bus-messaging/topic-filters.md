---
title: Azure Service Bus 토픽 필터 | Microsoft Docs
description: 이 문서에서는 구독자가 필터를 지정 하 여 토픽에서 수신 하려는 메시지를 정의 하는 방법을 설명 합니다.
services: service-bus-messaging
documentationcenter: ''
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: spelluru
ms.openlocfilehash: 6e780268d4b8c1a512ce82b1ca10a2f6b7b894b7
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125760"
---
# <a name="topic-filters-and-actions"></a>토픽 필터 및 작업

구독자는 토픽에서 수신할 메시지를 정의할 수 있습니다. 이러한 메시지는 하나 이상의 명명된 구독 규칙의 형태로 지정됩니다. 각 규칙은 특정 메시지를 선택하는 조건과 선택한 메시지에 주석을 다는 작업으로 구성됩니다. 일치하는 각 규칙 조건에 대해 구독은 메시지의 복사본을 생성하며 일치하는 규칙마다 다르게 주석을 달 수 있습니다.

새로 생성된 각각의 토픽 구독에는 초기 기본 구독 규칙이 있습니다. 규칙에 대한 필터 조건을 명시적으로 지정하지 않는 경우 적용된 필터는 **true** 필터이며 이를 통해 모든 메시지를 구독으로 선택할 수 있습니다. 기본 규칙에는 연결된 주석 작업이 없습니다.

Service Bus는 세 가지 필터 조건을 지원합니다.

-   *부울 필터* - **TrueFilter** 및 **FalseFilter**는 모든 도착 메시지가 구독에 대해 선택되거나(**true**) 선택되지 않도록(**false**) 합니다.

-   *SQL 필터* - **SqlFilter**는 broker에서 도착 메시지의 사용자 정의 속성 및 시스템 속성과 비교하여 평가되는 SQL과 비슷한 조건식을 유지합니다. 모든 시스템 속성은 조건식에서 `sys.`로 시작되어야 합니다. [필터 조건에 대 한 SQL 언어 하위 집합](service-bus-messaging-sql-filter.md) 은 속성 ( `EXISTS` ), null 값 ( `IS NULL` ), logical NOT/AND/OR, 관계형 연산자, 단순 숫자 산술 및와의 단순 텍스트 패턴 일치 여부를 테스트 합니다 `LIKE` .

-   *상관 관계 필터* - **CorrelationFilter**는 도착 메시지의 사용자 및 시스템 속성 중 하나 이상과 일치하는 조건 집합을 보유합니다. 일반적으로 **CorrelationId** 속성을 일치 시키는 것 이지만 응용 프로그램은 다음 속성과 일치 하도록 선택할 수도 있습니다.

    - **ContentType**
     - **레이블**
     - **MessageId**
     - **ReplyTo**
     - **ReplyToSessionId**
     - **SessionId** 
     - **받는 사람**
     - 사용자 정의 속성입니다. 
     
     속성에 대한 도착 메시지의 값이 상관 필터에 지정된 값과 같을 때 일치가 존재합니다. 문자열 식의 경우 비교에서 대/소문자가 구분됩니다. 여러 일치 속성을 지정할 때 필터는 이들을 논리적 AND 조건으로 결합합니다. 즉, 필터가 일치하려면 모든 조건이 일치해야 합니다.

모든 필터는 메시지 속성을 평가합니다. 필터는 메시지 본문을 평가할 수 없습니다.

복잡한 필터 규칙에는 처리 용량이 필요합니다. 특히 SQL 필터 규칙을 사용 하면 구독, 토픽 및 네임 스페이스 수준에서 전체 메시지 처리량이 줄어듭니다. 가능 하면 응용 프로그램에서 SQL과 유사한 필터에 대 한 상관 관계 필터를 선택 해야 합니다. 이러한 필터는 처리 하는 것이 훨씬 더 효율적이 고 처리량에 미치는 영향은 적습니다.

## <a name="actions"></a>동작

SQL 필터 조건을 사용하면 속성 및 값을 추가, 제거 또는 교체하여 메시지에 주석을 달 수 있는 작업을 정의할 수 있습니다. 이 작업은 SQL UPDATE 문에 개괄적으로 의지하는 [SQL-like 식을 사용](service-bus-messaging-sql-filter.md)합니다. 작업은 메시지가 일치 된 후 구독에 메시지를 선택 하기 전에 메시지에 대해 수행 됩니다. 메시지 속성에 대한 변경 사항은 구독에 복사된 메시지에 대한 프라이빗입니다.

## <a name="usage-patterns"></a>사용 패턴

토픽에 대한 가장 간단한 사용 시나리오는 모든 구독이 토픽에 전송된 각 메시지의 복사본을 가져오는 브로드캐스트 패턴을 사용하는 것입니다.

필터와 작업을 통해 분할과 라우팅이라는 두 가지 패턴 그룹을 추가로 사용할 수 있습니다.

분할은 필터를 사용하여 예측 가능하며 상호 배타적인 방식으로 기존의 여러 토픽 구독에 메시지를 배포합니다. 분할 패턴은 전체 데이터의 하위 집합을 보유하는 기능적으로 동일한 구획에서 다수의 많은 컨텍스트를 처리하도록 시스템 규모가 확장될 때 사용됩니다(예: 고객 프로필 정보). 분할을 사용하면 게시자가 분할 모델에 대한 지식이 없어도 토픽에 메시지를 제출할 수 있습니다. 그런 다음 메시지는 올바른 구독으로 이동되고 파티션의 메시지 처리기에서 검색이 가능해집니다.

라우팅은 필터를 사용하여 예측 가능한 방식으로 토픽 구독에 메시지를 배포하지만 반드시 배타적이지는 않습니다. [자동 전달](service-bus-auto-forwarding.md) 기능과 함께 토픽 필터를 사용하여 Azure 지역 내에 메시지를 배포하기 위해 Service Bus 네임스페이스 내에 복잡한 라우팅 그래프를 만들 수 있습니다. Azure Service Bus 네임스페이스 간에 브리지 역할을 하는 Azure Functions 또는 Azure Logic Apps를 사용하면 LOB(기간 업무) 애플리케이션에 직접 통합하여 복잡한 글로벌 토폴로지를 만들 수 있습니다.


> [!NOTE]
> 현재 Azure Portal에서는 구독에 대 한 필터 규칙을 지정할 수 없습니다. 지원 되는 Sdk 또는 Azure Resource Manager 템플릿 중 하나를 사용 하 여 구독 규칙을 정의할 수 있습니다. 

## <a name="next-steps"></a>다음 단계
다음 샘플을 참조 하세요. 

- [.NET-필터가 포함 된 기본 송신 및 수신 자습서](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveTutorialwithFilters/BasicSendReceiveTutorialWithFilters)
- [.NET 토픽 필터](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters)
- [Azure Resource Manager 템플릿](https://docs.microsoft.com/azure/templates/microsoft.servicebus/2017-04-01/namespaces/topics/subscriptions/rules)


