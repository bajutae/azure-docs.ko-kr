---
title: 파일 포함
description: 포함 파일
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 05/06/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 5a5f831e72da6bedaf12d3ed82e79f0250a09062
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82875999"
---
| 리소스 | 제한 |
| --- | --- |
| 데이터 처리량 |30gbps<sup>1</sup> |
|규칙|1만. 모든 규칙 유형을 결합 합니다.|
|최대 DNAT 규칙|298<br>규칙의 프로토콜이 TCP와 UDP에 대해 모두 구성 된 경우 두 개의 규칙으로 계산 됩니다.|
|AzureFirewallSubnet 최소 크기 |/26|
|네트워크 및 애플리케이션 규칙의 포트 범위|1 - 65535|
|공용 IP 주소|250 DNAT 및 SNAT 포트 모두에 대해 최대.|
|IP 그룹 IP 주소|50 IP 그룹 또는 낮음: 각 방화벽 인스턴스당 개별 IP 주소를 최대 5000 합니다.<br>51-100 IP 그룹: 500 개별 IP 주소 (각 방화벽 인스턴스당)<br><br>자세한 내용은 [Azure 방화벽의 IP 그룹 (미리 보기)](../articles/firewall/ip-groups.md#ip-address-limits) 을 참조 하세요.
|경로 테이블|기본적으로 AzureFirewallSubnet에는 NextHopType 값이 **인터넷**으로 설정 된 0.0.0.0/0 경로가 있습니다.<br><br>Azure Firewall에는 직접 인터넷 연결이 있어야 합니다. AzureFirewallSubnet가 BGP를 통해 온-프레미스 네트워크에 대 한 기본 경로를 학습 하는 경우 직접 인터넷 연결을 유지 하려면 **internet** 로 설정 된 **NextHopType** 값을 사용 하 여 0.0.0.0/0 udr으로 재정의 해야 합니다. 기본적으로 Azure Firewall은 온-프레미스 네트워크에 대한 강제 터널링을 지원하지 않습니다.<br><br>그러나 구성에 온-프레미스 네트워크에 대한 강제 터널링이 필요한 경우 Microsoft는 사례별로 지원할 예정입니다. 사용자의 사례를 검토할 수 있도록 지원 부서에 연락해주시기 바랍니다. 수락되면 사용자의 구독을 허용하고 필요한 방화벽 인터넷 연결이 유지되도록 합니다.|

<sup>1</sup> 이러한 제한을 늘려야 하는 경우 Azure 지원에 문의 하세요.
