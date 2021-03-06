---
title: Azure Security Center 계층 가격 책정
description: Azure Security Center는 무료 및 표준의 두 계층으로 제공 됩니다. 이 페이지에서는 무료에서 표준으로 업그레이드 하는 방법을 보여 줍니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2020
ms.author: memildin
ms.openlocfilehash: 2eb302462e402e701cc5ae664239c457c90ab1b9
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612795"
---
# <a name="upgrade-to-standard-tier-for-enhanced-security"></a>보안 강화를 위해 표준 계층으로 업그레이드

Azure Security Center는 Azure, 온-프레미스 및 기타 클라우드용으로 통합 보안 관리 및 고급 위협 보호 기능을 제공합니다. 또한 하이브리드 클라우드 작업을 확인하고 제어하는 기능, 위협에 대한 노출을 줄이는 적극적인 방어 기능, 그리고 빠르게 발전하는 사이버 공격에 대응할 수 있는 지능형 검색 기능을 제공합니다.

## <a name="pricing-tiers"></a>가격 책정 계층
Security Center는 두 계층으로 제공됩니다.

- **무료** 계층은 Azure Portal에서 Azure Security Center 대시보드를 처음 방문 하거나 API를 통해 프로그래밍 방식으로 사용 하도록 설정한 경우 모든 Azure 구독에서 사용할 수 있습니다. 무료 계층은 Azure 리소스를 보호할 수 있도록 보안 정책, 지속적인 보안 평가 및 실행 가능한 보안 권장 사항을 제공 합니다.

- **표준** 계층은 무료 계층의 기능을 사설 및 기타 공용 클라우드에서 실행 되는 작업으로 확장 하 여 하이브리드 클라우드 워크 로드에서 통합 보안 관리 및 위협 방지 기능을 제공 합니다. 또한 표준 계층은 기본 제공 동작 분석 및 기계 학습을 사용 하 여 공격 및 제로 일 악용을 식별 하는 위협 방지 기능과 네트워크 공격 및 맬웨어에 대 한 노출을 줄이기 위한 액세스 및 응용 프로그램 제어 기능을 추가 합니다. 또한 표준 계층은 가상 컴퓨터에 대 한 취약성 검색을 추가 합니다. 표준 계층을 무료로 사용해 볼 수 있습니다. Security Center standard는 Vm, 가상 머신 확장 집합, App Service, SQL server 및 저장소 계정을 비롯 한 Azure 리소스를 지원 합니다. Azure Security Center standard를 사용 하는 경우 리소스 유형에 따라 지원 옵트아웃 (opt out) 할 수 있습니다. 

대부분의 표준 계층 보안 경고 뿐만 아니라 Vm에 대 한 무료 계층 보안 평가에는 Log Analytics 에이전트를 설치 해야 합니다. Security Center에서 자동 프로비저닝을 사용 하도록 설정 하 여 Azure Vm에 에이전트를 자동으로 배포할 수 있습니다.

## <a name="try-standard-tier-free-for-30-days"></a>30 일간 표준 계층 체험 하기
표준 계층은 처음 30 일 동안 무료입니다. 30일 종료 시 서비스를 계속 사용하기로 선택하는 경우 사용량에 대한 요금이 자동으로 부과되기 시작합니다.

전체 Azure 구독을 표준 계층으로 업그레이드할 수 있으며,이는 구독 내의 모든 리소스에서 상속 됩니다.

표준 계층을 가져오려면 다음을 수행 합니다.

1. **Security Center** 주 메뉴에서 **가격 책정 & 설정** 을 선택 합니다.
2. 표준으로 업그레이드 하려는 구독을 선택 합니다.
3. **가격 책정 계층**을 선택합니다.
4. **표준**을 선택하여 업그레이드합니다.
5. **저장**을 클릭합니다.

[![Security Center 가격 책정](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> 위협 방지 기능을 비롯 한 모든 Security Center 기능을 사용 하도록 설정 하려면 해당 하는 작업을 포함 하는 구독에 표준 가격 책정 계층을 적용 해야 합니다. 작업 영역에 대 한 가격 책정 구성은 just-in-time VM 액세스, 적응 응용 프로그램 제어 및 Azure 리소스에 대 한 네트워크 검색을 사용 하도록 설정 하지 않습니다. 
>
> 구독 수준 또는 리소스 수준에서 **Azure Storage 계정** 에 대해 위협 방지를 사용 하도록 설정할 수 있습니다.
> 구독 수준 또는 리소스 수준에서 **AZURE SQL DATABASE SQL server** 에 대 한 위협 방지를 사용 하도록 설정할 수 있습니다.
> 리소스 수준 에서만 **Azure Database for MariaDB/MySQL/PostgreSQL** 에 대 한 위협 방지를 사용 하도록 설정할 수 있습니다.


## <a name="why-upgrade-to-standard"></a>표준으로 업그레이드 하는 이유
Security Center에서는 다음을 비롯하여 하이브리드 클라우드 작업을 위한 강화된 보안 및 위협 방지 기능을 제공합니다.

- **하이브리드 보안** – 모든 온-프레미스 및 클라우드 작업에 걸쳐 보안을 통합 확인할 수 있습니다. 또한 보안 정책을 적용하고 하이브리드 클라우드 작업의 보안을 지속적으로 평가하여 보안 표준을 준수할 수 있습니다. 방화벽 및 기타 파트너 솔루션을 포함 하 여 여러 원본에서 보안 데이터를 수집, 검색 및 분석 합니다.
- **보안 경고** -고급 분석 및 Microsoft Intelligent Security Graph를 사용 하 여 진화 하는 사이버 공격에 대 한 경계를 가져옵니다. 기본 제공 행동 분석 및 Machine Learning을 활용하여 공격 및 제로 데이 익스플로잇을 식별할 수 있습니다. 또한 네트워크, 컴퓨터 및 클라우스 서비스에서 들어오는 공격 및 위반 후 활동을 모니터링할 수 있습니다. 대화형 도구 및 상황에 맞는 위협 인텔리전스를 사용하면 조사를 손쉽게 수행할 수 있습니다.
- **가상 컴퓨터에 대 한 취약성 검색** -취약성 관리를 위한 업계의 가장 고급 솔루션을 제공 하는 모든 가상 컴퓨터에 스캐너를 쉽게 배포할 수 있습니다. Security Center 내에서 검색 결과를 직접 확인, 조사 및 재구성 합니다. 
- **액세스 및 응용 프로그램 제어** -특정 워크 로드에 맞게 조정 된 machine learning powered 허용 목록 권장 사항을 적용 하 여 맬웨어 및 기타 원치 않는 응용 프로그램을 차단 합니다. Azure Vm의 관리 포트에 대 한 적시의 제어 된 액세스로 네트워크 공격 노출 영역을 줄입니다. 따라서 무차별 암호 대입 및 기타 네트워크 공격에 대 한 노출을 크게 줄일 수 있습니다.
- **컨테이너 보안 기능** -컨테이너 화 된 환경에 대 한 취약성 관리 및 실시간 위협 방지의 혜택을 제공 합니다. 컨테이너 레지스트리 리소스를 사용 하도록 설정 하는 경우 모든 기능을 사용 하도록 설정할 때까지 최대 12 시간이 걸릴 수 있습니다.


## <a name="next-steps"></a>다음 단계
이 문서에서는 Security Center의 가격 책정 방식에 대해 알아보았습니다. 표준 계층의 향상 된 보안 및 고급 위협 방지에 대 한 자세한 내용은 다음을 참조 하세요.

- [위협 보호 및 Azure Security Center](threat-protection.md)
- [Just-in-time VM 액세스 제어](security-center-just-in-time.md)
- [컨테이너 보안 개요](container-security.md)
- [선택한 통화 및 해당 지역에 따라 가격 정보](https://azure.microsoft.com/pricing/details/security-center/)