---
title: 가상 네트워크 서비스 끝점을 사용 하 여 Azure Cosmos DB 계정에 안전 하 게 액세스
description: 이 문서에서는 Azure Cosmos 계정에 대한 가상 네트워크 및 서브넷 액세스 제어를 설명합니다.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: c1c5bdd1d210a1933699cad52dbf123b50048e01
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80421323"
---
# <a name="access-azure-cosmos-db-from-virtual-networks-vnet"></a>가상 네트워크(VNet)에서 Azure Cosmos DB 액세스

가상 네트워크(VNet)의 특정 서브넷에서만 액세스할 수 있도록 Azure Cosmos 계정을 구성할 수 있습니다. 가상 네트워크 내의 서브넷에 있는 Azure Cosmos DB에 액세스하도록 [서비스 엔드포인트](../virtual-network/virtual-network-service-endpoints-overview.md)를 설정하면 해당 서브넷의 트래픽이 서브넷 및 Virtual Network의 ID를 통해 Azure Cosmos DB로 보내집니다. Azure Cosmos DB 서비스 엔드포인트를 사용하도록 설정하고 나면, Azure Cosmos 계정에 추가하여 서브넷에 대한 액세스를 제한할 수 있습니다.

기본적으로 요청에 유효한 권한 부여 토큰이 포함되어 있으면 어떠한 원본에서든 Azure Cosmos 계정에 액세스할 수 있습니다. VNet 내에서 하나 이상의 서브넷을 추가하면 해당 서브넷에서 시작된 요청만 유효한 응답을 받습니다. 다른 원본에서 시작된 요청은 403(금지됨) 응답을 받게 됩니다. 

## <a name="frequently-asked-questions"></a>자주 묻는 질문

가상 네트워크의 액세스 구성에 대해 몇 가지 자주 묻는 질문은 다음과 같습니다.

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>가상 네트워크 서비스 엔드포인트 및 IP 액세스 제어 정책 모두 Azure Cosmos 계정에서 지정할 수 있나요? 

가상 네트워크 서비스 엔드포인트 및 IP 액세스 제어 정책(일명 방화벽) 모두 Azure Cosmos 계정에서 사용하도록 설정할 수 있습니다. 이러한 두 기능은 상호 보완적이며 전체적으로 Azure Cosmos 계정의 격리 및 보안을 보장합니다. IP 방화벽을 사용하면 고정 IP가 사용자 계정에 액세스할 수 있습니다. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>가상 네트워크 내의 서브넷에 대한 액세스를 제한하려면 어떻게 해야 하나요? 

서브넷의 Azure Cosmos 계정에 대한 액세스를 제한하려면 두 단계가 필요합니다. 먼저 서브넷의 트래픽이 해당 서브넷과 가상 네트워크 ID를 Azure Cosmos DB로 전송하도록 허용합니다. 서브넷에서 Azure Cosmos DB에 대한 서비스 엔드포인트를 사용하도록 설정하면 됩니다. 다음은 이 서브넷을 계정에 액세스할 수 있는 소스로 지정하는 규칙을 Azure Cosmos 계정에 추가하는 것입니다.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>가상 네트워크 ACL 및 IP 방화벽은 요청 또는 연결을 거부하나요? 

IP 방화벽 또는 가상 네트워크 액세스 규칙이 추가되면 허용된 원본의 요청만 유효한 응답을 가져옵니다. 다른 요청은 거부되며 403(금지됨)이 수신됩니다. Azure Cosmos 계정의 방화벽을 연결 수준 방화벽과 구별하는 것이 중요합니다. 원본에서 서비스에 계속 연결할 수 있으며 연결 자체는 거부 되지 않습니다.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>서비스 엔드포인트를 서브넷의 Azure Cosmos DB에 대해 사용하도록 설정했을 때 내 요청이 차단되기 시작했습니다. 어떻게 된 건가요?

Azure Cosmos DB에 대한 서비스 엔드포인트가 서브넷에서 사용하도록 설정되면 계정에 도달하는 트래픽의 원본이 공용 IP에서 가상 네트워크 및 서브넷으로 전환됩니다. Azure Cosmos 계정에 IP 기반 방화벽이 있는 경우에만 서브넷을 사용하는 서비스의 요청이 더 이상 IP 방화벽 규칙과 일치하지 않아 거부됩니다. IP 기반 방화벽에서 가상 네트워크 기반 액세스 제어로 원활하게 마이그레이션하는 단계로 이동하세요.

### <a name="are-additional-rbac-permissions-needed-for-azure-cosmos-accounts-with-vnet-service-endpoints"></a>VNET 서비스 끝점을 사용 하는 Azure Cosmos 계정에 대 한 추가 RBAC 권한이 필요 한가요?

Azure Cosmos 계정에 VNet 서비스 끝점을 추가한 후 계정 설정을 변경 하려면 Azure Cosmos 계정에 구성 된 모든 Vnet에 대 한 `Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action` 작업에 액세스 해야 합니다. 이 권한은 속성을 평가 하기 전에 권한 부여 프로세스에서 리소스 (예: 데이터베이스 및 가상 네트워크 리소스)에 대 한 액세스의 유효성을 검사 하기 때문에 필요 합니다.
 
사용자가 Azure CLI를 사용 하 여 VNET Acl을 지정 하지 않더라도 권한 부여는 VNet 리소스 작업에 대 한 사용 권한의 유효성을 검사 합니다. 현재 Azure Cosmos 계정의 제어 평면은 Azure Cosmos 계정의 전체 상태를 설정 하도록 지원 합니다. 제어 평면 호출에 대 한 매개 변수 중 하나 `virtualNetworkRules`는입니다. 이 매개 변수를 지정 하지 않으면 Azure CLI에서를 검색 하 `virtualNetworkRules` 고 업데이트 호출에서이 값을 사용 하는 get database 호출을 수행 합니다.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>피어링된 가상 네트워크도 Azure Cosmos 계정에 액세스할 수 있나요? 
Azure Cosmos 계정에 추가된 가상 네트워크 및 해당 서브넷만 액세스할 수 있습니다. 피어링된 가상 네트워크 내 서브넷이 계정에 추가될 때까지는 피어링된 해당 VNet이 계정에 액세스할 수 없습니다.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>단일 Cosmos 계정에 액세스할 수 있는 최대 서브넷 수는? 
현재 Azure Cosmos 계정에 대해 허용 되는 최대 256 서브넷이 있을 수 있습니다.

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>VPN 및 Express Route의 액세스를 사용하도록 설정할 수 있나요? 
온-프레미스에서 Express 경로를 통해 Azure Cosmos 계정에 액세스 하는 경우 Microsoft 피어 링을 사용 하도록 설정 해야 합니다. IP 방화벽 또는 가상 네트워크 액세스 규칙을 입력한 후에는 Azure Cosmos 계정 IP 방화벽에서 Microsoft 피어링에 사용되는 공용 IP 주소를 추가하여 Azure Cosmos 계정에 대한 온-프레미스 서비스 액세스를 허용할 수 있습니다. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>NSG(네트워크 보안 그룹) 규칙을 업데이트해야 하나요? 
NSG 규칙은 가상 네트워크를 사용하여 서브넷 간의 연결을 제한하는 데 사용됩니다. 서브넷에 Azure Cosmos DB에 대한 서비스 엔드포인트를 추가한 경우 Azure Cosmos 계정에 대한 NSG에서 아웃바운드 연결을 열 필요가 없습니다. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>모든 VNet에서 서비스 엔드포인트를 사용할 수 있나요?
아니요, Azure Resource Manager 가상 네트워크만 서비스 엔드포인트를 사용하도록 설정할 수 있습니다. 클래식 가상 네트워크는 서비스 끝점을 지원 하지 않습니다.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Azure Cosmos DB에 대해 서비스 엔드포인트 액세스가 가능하면 "공용 Azure 데이터 센터 내에서 연결을 허용"해도 되나요?  
이는 Azure Data factory, Azure Cognitive Search 또는 지정 된 Azure 지역에 배포 된 서비스와 같은 다른 Azure 자사 서비스에서 Azure Cosmos DB 계정을 액세스 하려는 경우에만 필요 합니다.


## <a name="next-steps"></a>다음 단계

* [가상 네트워크 내의 서브넷에 대한 Azure Cosmos 계정 액세스를 제한하는 방법](how-to-configure-vnet-service-endpoint.md)
* [Azure Cosmos 계정에 대해 IP 방화벽을 구성하는 방법](how-to-configure-firewall.md)

