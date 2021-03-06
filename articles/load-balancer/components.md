---
title: Azure Load Balancer 구성 요소
description: Azure Load Balancer 구성 요소의 개요
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: allensu
ms.openlocfilehash: 4a84c43b57ec4f632a2bfabb10d112e4975249bf
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82733110"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer 구성 요소

Azure Load Balancer에는 해당 작업을 위한 몇 가지 주요 구성 요소가 포함되어 있습니다. 이러한 구성 요소는 Azure Portal, Azure CLI, Azure PowerShell 또는 템플릿을 통해 구독에서 구성할 수 있습니다.

## <a name="frontend-ip-configurations"></a>프런트 엔드 IP 구성

부하 분산 장치의 IP 주소입니다. 클라이언트의 연락 지점입니다. 이러한 주소는 다음 중 하나일 수 있습니다.

- **공용 IP 주소**
- **개인 IP 주소**

IP 주소를 선택하면 생성된 부하 분산 장치의 **형식**이 결정됩니다. 개인 IP 주소를 선택하면 내부 부하 분산 장치를 만듭니다. 공용 IP 주소를 선택하면 공용 부하 분산 장치를 만듭니다.

|  | 공용 Load Balancer  | 내부 부하 분산 장치 |
| ---------- | ---------- | ---------- |
| 프런트 엔드 IP 구성| 공용 IP 주소 | 개인 IP 주소|
| Description | 공용 부하 분산 장치는 들어오는 트래픽의 공용 IP와 포트를 VM의 개인 IP 및 포트에 매핑합니다. 부하 분산 장치는 VM의 응답 트래픽에 대해 다른 방식으로 트래픽을 매핑합니다. 부하 분산 규칙을 적용하여 특정 유형의 트래픽을 여러 VM 또는 서비스에 분산할 수 있습니다. 예를 들어 웹 요청 트래픽의 부하를 여러 웹 서버에 분산할 수 있습니다.| 내부 부하 분산 장치는 가상 네트워크 내에 있는 리소스에 트래픽을 분산합니다. Azure는 부하가 분산된 가상 네트워크의 프런트 엔드 IP 주소에 대한 액세스를 제한합니다. 프런트 엔드 IP 주소와 가상 네트워크는 인터넷 엔드포인트에 직접 노출되지 않습니다. 내부 LOB(기간 업무) 애플리케이션은 Azure에서 실행되며 Azure 내에서 또는 온-프레미스 리소스에서 액세스할 수 있습니다. |
| 지원되는 SKU | 기본, 표준 | 기본, 표준 |

![계층화된 부사 분산 장치 예제](./media/load-balancer-overview/load-balancer.png)

## <a name="backend-pool"></a>백 엔드 풀

들어오는 요청을 처리하는 가상 머신 또는 가상 머신 확장 집합 인스턴스의 그룹입니다. 대량의 들어오는 트래픽을 충족하도록 비용 효율적으로 크기를 조정하기 위해 컴퓨팅 지침에서는 일반적으로 백 엔드 풀에 더 많은 인스턴스를 추가하는 것을 권장합니다. 

부하 분산 장치는 인스턴스의 크기를 확장하거나 축소하는 경우 자동 재구성을 통해 자체적으로 즉시 재구성됩니다. 백 엔드 풀에서 VM을 추가 또는 제거하면 추가 작업 없이 부하 분산 장치가 다시 구성됩니다. 백 엔드 풀의 범위는 가상 네트워크의 모든 가상 머신입니다. 

백 엔드 풀을 디자인하는 방법을 고려할 때는 가장 적은 수의 개별 백 엔드 풀 리소스에 맞게 디자인하고, 관리 작업의 길이를 최적화합니다. 데이터 평면 성능 또는 크기 조정에는 차이가 없습니다.

## <a name="health-probes"></a>상태 프로브

상태 프로브는 백 엔드 풀의 인스턴스 상태를 확인하는 데 사용됩니다. 상태 프로브에 대한 비정상 임계값을 정의할 수 있습니다. 프로브가 응답하지 않으면 부하 분산 장치는 비정상 인스턴스에 새 연결의 전송을 중지합니다. 프로브 오류는 기존 연결에 영향을 주지 않습니다. 애플리케이션에서 다음을 수행할 때까지 연결이 유지됩니다.

- 흐름을 종료
- 유휴 제한 시간 발생
- VM이 종료

Load Balancer는 다음에 대한 다양한 상태 프로브 유형을 제공합니다.

- TCP
- HTTP
- HTTPS

기본 부하 분산 장치는 HTTPS 프로브를 지원하지 않습니다. 기본 부하 분산 장치는 설정된 연결을 포함하여 모든 TCP 연결을 종료합니다.

## <a name="load-balancing-rules"></a>부하 분산 규칙

부하 분산 규칙은 부하 분산 장치에 수행할 작업을 지시합니다. 부하 분산 규칙은 지정된 프런트 엔드 IP 구성 및 포트를 여러 백 엔드 IP 주소 및 포트에 매핑합니다.

## <a name="inbound-nat-rules"></a>인바운드 NAT 규칙

인바운드 NAT 규칙은 프런트 엔드 IP 주소에서 가상 네트워크 내의 백 엔드 인스턴스로 트래픽을 전달합니다. 포트 전달은 부하 분산과 동일한 해시 기반 배포를 통해 수행됩니다. 

사용 예제는 가상 네트워크 내에서 VM 인스턴스를 분리하는 RDP(원격 데스크톱 프로토콜) 또는 SSH(Secure Shell) 세션입니다. 여러 개의 내부 엔드포인트를 동일한 프런트 엔드 IP 주소의 포트에 매핑할 수 있습니다. 프런트 엔드 IP 주소를 사용하여 추가 점프 상자 없이 VM을 원격으로 관리할 수 있습니다.

## <a name="outbound-rules"></a>아웃바운드 규칙

아웃바운드 규칙은 백 엔드 풀에서 식별된 모든 가상 머신 또는 인스턴스에 대한 아웃바운드 NAT(Network Address Translation)를 구성합니다.

기본 부하 분산 장치는 아웃바운드 규칙을 지원하지 않습니다.
![Azure Load Balancer](./media/load-balancer-overview/load-balancer-overview.png)

## <a name="next-steps"></a>다음 단계

- Load Balancer를 사용하여 시작하려면 [공용 표준 Load Balancer 만들기](quickstart-load-balancer-standard-public-portal.md)를 참조하세요.
- [Azure Load Balancer에 대해 자세히 알아보세요](load-balancer-overview.md).
- [공용 IP 주소](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)에 대한 자세한 정보
- [개인 IP 주소](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)에 대한 자세한 정보
- [표준 Load Balancer 및 가용성 영역](load-balancer-standard-availability-zones.md) 사용에 대해 자세히 알아보세요.
- [표준 Load Balancer 진단](load-balancer-standard-diagnostics.md)에 대해 자세히 알아보세요.
- [유휴 상태의 TCP 재설정](load-balancer-tcp-reset.md)에 대해 알아보세요.
- [HA 포트 부하 분산 규칙을 사용하는 표준 Load Balancer](load-balancer-ha-ports-overview.md)에 대해 자세히 알아보세요.
- [다중 프런트 엔드를 사용하는 Load Balancer](load-balancer-multivip-overview.md)에 대해 자세히 알아보세요.
- [네트워크 보안 그룹](../virtual-network/security-overview.md)에 대해 자세히 알아보세요.
- [프로브 유형](load-balancer-custom-probe-overview.md#types)에 대해 알아봅니다.
- [부사 분산 장치 제한](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)에 대해 자세히 알아봅니다.
- [포트 전달](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal) 사용에 대해 알아봅니다.
- [부하 분산 장치 아웃바운드 규칙](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)에 대해 알아봅니다.
