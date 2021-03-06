---
title: Azure Cosmos 계정에 대 한 Azure 개인 링크 구성
description: 가상 네트워크에서 개인 IP 주소를 사용 하 여 azure Cosmos 계정에 액세스 하도록 Azure 개인 링크를 설정 하는 방법에 대해 알아봅니다.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: thweiss
ms.openlocfilehash: 4b49d2aa61587d0156755bdd5c47b3eeb90090a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81270692"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account"></a>Azure Cosmos 계정에 대 한 Azure 개인 링크 구성

Azure 개인 링크를 사용 하 여 개인 끝점을 통해 Azure Cosmos 계정에 연결할 수 있습니다. 개인 끝점은 가상 네트워크 내의 서브넷에 있는 개인 IP 주소의 집합입니다. 그런 다음 개인 IP 주소를 통해 Azure Cosmos 계정에 대 한 액세스를 제한할 수 있습니다. 개인 링크를 제한 된 NSG 정책과 결합 하면 데이터 exfiltration의 위험을 줄일 수 있습니다. 개인 끝점에 대 한 자세한 내용은 [Azure 개인 링크](../private-link/private-link-overview.md) 문서를 참조 하세요.

개인 링크를 사용 하면 사용자가 가상 네트워크 또는 모든 피어 링 가상 네트워크 내에서 Azure Cosmos 계정에 액세스할 수 있습니다. 개인 링크에 매핑된 리소스는 VPN 또는 Azure Express 경로를 통해 개인 피어 링을 통해 온-프레미스 에서도 액세스할 수 있습니다. 

자동 또는 수동 승인 방법을 사용 하 여 개인 링크로 구성 된 Azure Cosmos 계정에 연결할 수 있습니다. 자세히 알아보려면 개인 링크 설명서의 [승인 워크플로](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) 섹션을 참조 하세요. 

이 문서에서는 개인 끝점을 만드는 단계를 설명 합니다. 자동 승인 방법을 사용 하 고 있다고 가정 합니다.

> [!NOTE]
> 개인 끝점 지원은 현재 게이트웨이 연결 모드 에서만 일반적으로 사용할 수 있습니다. 직접 모드의 경우에는 미리 보기 기능으로 사용할 수 있습니다.

## <a name="create-a-private-endpoint-by-using-the-azure-portal"></a>Azure Portal를 사용 하 여 개인 끝점을 만듭니다.

Azure Portal를 사용 하 여 기존 Azure Cosmos 계정에 대 한 개인 끝점을 만들려면 다음 단계를 사용 합니다.

1. **모든 리소스** 창에서 Azure Cosmos 계정을 선택 합니다.

1. 설정 목록에서 **개인 끝점 연결** 을 선택 하 고 **개인 끝점**을 선택 합니다.

   ![Azure Portal에서 개인 끝점을 만들기 위한 선택 항목](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. **개인 끝점 만들기-기본 사항** 창에서 다음 세부 정보를 입력 하거나 선택 합니다.

    | 설정 | 값 |
    | ------- | ----- |
    | **프로젝트 세부 정보** | |
    | Subscription | 구독을 선택합니다. |
    | Resource group | 리소스 그룹을 선택합니다.|
    | **인스턴스 세부 정보** |  |
    | 속성 | 개인 끝점의 이름을 입력 합니다. 이 이름을 사용 하는 경우 고유한 이름을 만듭니다. |
    |지역| 개인 링크를 배포 하려는 지역을 선택 합니다. 가상 네트워크가 있는 위치와 동일한 위치에 개인 끝점을 만듭니다.|
    |||
1. 완료되면 **다음: 리소스**를 선택합니다.
1. **프라이빗 엔드포인트 만들기 - 리소스**에서 다음 정보를 입력하거나 선택합니다.

    | 설정 | 값 |
    | ------- | ----- |
    |연결 방법  | **내 디렉터리에서 Azure 리소스에 연결을**선택 합니다. <br/><br/> 그런 다음 리소스 중 하나를 선택 하 여 개인 링크를 설정할 수 있습니다. 또는 공유 된 리소스 ID 또는 별칭을 사용 하 여 다른 사용자의 리소스에 연결할 수 있습니다.|
    | Subscription| 구독을 선택합니다. |
    | 리소스 유형 | **Microsoft.azurecosmosdb/databaseAccounts**를 선택 합니다. |
    | 리소스 |Azure Cosmos 계정을 선택 합니다. |
    |대상 하위 리소스 |매핑할 Azure Cosmos DB API 유형을 선택 합니다. 이 기본값은 SQL, MongoDB 및 Cassandra Api에 대해 선택 하는 옵션 중 하나입니다. Gremlin 및 Table Api의 경우 이러한 Api는 SQL API와 상호 운용할 수 있으므로 **sql** 을 선택할 수도 있습니다. |
    |||

1. 완료되면 **다음: 구성**을 선택합니다.
1. **개인 끝점 만들기-구성**에서 다음 정보를 입력 하거나 선택 합니다.

    | 설정 | 값 |
    | ------- | ----- |
    |**네트워킹**| |
    | 가상 네트워크| 가상 네트워크를 선택 합니다. |
    | 서브넷 | 서브넷을 선택 합니다. |
    |**사설 DNS 통합**||
    |프라이빗 DNS 영역과 통합 |**예**를 선택합니다. <br><br/> 개인 끝점과 개인적으로 연결 하려면 DNS 레코드가 필요 합니다. 개인 DNS 영역에 개인 끝점을 통합 하는 것이 좋습니다. 사용자 고유의 DNS 서버를 사용 하거나 가상 컴퓨터의 호스트 파일을 사용 하 여 DNS 레코드를 만들 수도 있습니다. |
    |프라이빗 DNS 영역 |**Privatelink.documents.azure.com**를 선택 합니다. <br><br/> 개인 DNS 영역은 자동으로 결정 됩니다. Azure Portal를 사용 하 여 변경할 수 없습니다.|
    |||

1. **검토 + 만들기**를 선택합니다. **검토 + 만들기** 페이지에서 Azure는 구성의 유효성을 검사 합니다.
1. **유효성 검사 통과** 메시지가 표시되면 **만들기**를 선택합니다.

Azure Cosmos 계정에 대 한 개인 링크를 승인한 경우 Azure Portal에서 **방화벽 및 가상 네트워크** 창의 **모든 네트워크** 옵션을 사용할 수 없습니다.

다음 표에서는 서로 다른 Azure Cosmos 계정 API 형식, 지원 되는 하위 리소스 및 해당 개인 영역 이름 간의 매핑을 보여 줍니다. SQL API를 통해 Gremlin 및 Table API 계정에 액세스할 수도 있으므로 이러한 Api에 대 한 두 가지 항목이 있습니다.

|Azure Cosmos 계정 API 유형  |지원 되는 하위 리소스 (또는 그룹 Id) |개인 영역 이름  |
|---------|---------|---------|
|Sql    |   Sql      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  Sql       |  privatelink.documents.azure.com    |
|테이블    |    테이블     |   privatelink.table.cosmos.azure.com    |
|테이블     |   Sql      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>개인 IP 주소 가져오기

개인 끝점이 프로 비전 되 면 IP 주소를 쿼리할 수 있습니다. Azure Portal에서 IP 주소를 보려면 다음을 수행 합니다.

1. **모든 리소스**를 선택합니다.
1. 이전에 만든 개인 끝점을 검색 합니다. 이 경우 **cdbPrivateEndpoint3**입니다.
1. **개요** 탭을 선택 하 여 DNS 설정 및 IP 주소를 확인 합니다.

![Azure Portal의 개인 IP 주소](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

개인 끝점 마다 여러 IP 주소가 생성 됩니다.

* Azure Cosmos 계정의 전역 (지역에 관계 없음) 끝점에 대 한 하나
* Azure Cosmos 계정이 배포 되는 각 지역에 대해 하나씩

## <a name="create-a-private-endpoint-by-using-azure-powershell"></a>Azure PowerShell를 사용 하 여 개인 끝점 만들기

다음 PowerShell 스크립트를 실행 하 여 기존 Azure Cosmos 계정에 대해 이름이 "MyPrivateEndpoint" 인 개인 끝점을 만듭니다. 변수 값을 사용자 환경에 대 한 세부 정보로 바꿉니다.

```azurepowershell-interactive
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>개인 끝점을 개인 DNS 영역과 통합

개인 끝점을 만든 후 다음 PowerShell 스크립트를 사용 하 여 개인 DNS 영역과 통합할 수 있습니다.

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>개인 IP 주소 가져오기

개인 끝점이 프로 비전 된 후 다음 PowerShell 스크립트를 사용 하 여 IP 주소 및 FQDN 매핑을 쿼리할 수 있습니다.

```azurepowershell-interactive
$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-endpoint-by-using-azure-cli"></a>Azure CLI를 사용 하 여 개인 끝점 만들기

다음 Azure CLI 스크립트를 실행 하 여 기존 Azure Cosmos 계정에 대해 이름이 "myPrivateEndpoint" 인 개인 끝점을 만듭니다. 변수 값을 사용자 환경에 대 한 세부 정보로 바꿉니다.

```azurecli-interactive
# Resource group where the Azure Cosmos account and virtual network resources are located
ResourceGroupName="myResourceGroup"

# Subscription ID where the Azure Cosmos account and virtual network resources are located
SubscriptionId="<your Azure subscription ID>"

# Name of the existing Azure Cosmos account
CosmosDbAccountName="mycosmosaccount"

# API type of your Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
CosmosDbApiType="Sql"

# Name of the virtual network to create
VNetName="myVnet"

# Name of the subnet to create
SubnetName="mySubnet"

# Name of the private endpoint to create
PrivateEndpointName="myPrivateEndpoint"

# Name of the private endpoint connection to create
PrivateConnectionName="myConnection"

az network vnet create \
 --name $VNetName \
 --resource-group $ResourceGroupName \
 --subnet-name $SubnetName

az network vnet subnet update \
 --name $SubnetName \
 --resource-group $ResourceGroupName \
 --vnet-name $VNetName \
 --disable-private-endpoint-network-policies true

az network private-endpoint create \
    --name $PrivateEndpointName \
    --resource-group $ResourceGroupName \
    --vnet-name $VNetName  \
    --subnet $SubnetName \
    --private-connection-resource-id "/subscriptions/$SubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.DocumentDB/databaseAccounts/$CosmosDbAccountName" \
    --group-ids $CosmosDbApiType \
    --connection-name $PrivateConnectionName
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>개인 끝점을 개인 DNS 영역과 통합

개인 끝점을 만든 후 다음 Azure CLI 스크립트를 사용 하 여 개인 DNS 영역과 통합할 수 있습니다.

```azurecli-interactive
zoneName="privatelink.documents.azure.com"

az network private-dns zone create --resource-group $ResourceGroupName \
   --name  $zoneName

az network private-dns link vnet create --resource-group $ResourceGroupName \
   --zone-name  $zoneName\
   --name myzonelink \
   --virtual-network $VNetName \
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name $PrivateEndpointName --resource-group $ResourceGroupName --query 'networkInterfaces[0].id' -o tsv)
 
# Copy the content for privateIPAddress and FQDN matching the Azure Cosmos account 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
 
#Create DNS records 
az network private-dns record-set a create --name recordSet1 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName
az network private-dns record-set a add-record --record-set-name recordSet2 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName -a <Private IP Address>
```

## <a name="create-a-private-endpoint-by-using-a-resource-manager-template"></a>리소스 관리자 템플릿을 사용 하 여 개인 끝점 만들기

가상 네트워크 서브넷에 개인 끝점을 만들어 개인 링크를 설정할 수 있습니다. Azure Resource Manager 템플릿을 사용 하 여이를 달성할 수 있습니다.

다음 코드를 사용 하 여 "PrivateEndpoint_template. json" 이라는 리소스 관리자 템플릿을 만듭니다. 이 템플릿은 기존 가상 네트워크에 있는 기존 Azure Cosmos SQL API 계정에 대 한 개인 끝점을 만듭니다.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

**템플릿에 대 한 매개 변수 파일을 정의 합니다.**

템플릿에 대 한 매개 변수 파일을 만들고 이름을 "PrivateEndpoint_parameters. json"으로 만듭니다. 매개 변수 파일에 다음 코드를 추가 합니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

**PowerShell 스크립트를 사용 하 여 템플릿 배포**

다음 코드를 사용 하 여 PowerShell 스크립트를 만듭니다. 스크립트를 실행 하기 전에 구독 ID, 리소스 그룹 이름 및 기타 변수 값을 사용자 환경에 대 한 세부 정보로 바꿉니다.

```azurepowershell-interactive
### This script creates a private endpoint for an existing Azure Cosmos account in an existing virtual network

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

PowerShell 스크립트에서 변수에는 `GroupId` 하나의 값만 포함 될 수 있습니다. 이 값은 계정의 API 유형입니다. 허용 되는 값 `Sql`은 `MongoDB`, `Cassandra`, `Gremlin`, 및 `Table`입니다. 일부 Azure Cosmos 계정 유형은 여러 Api를 통해 액세스할 수 있습니다. 다음은 그 예입니다.

* Gremlin API 계정은 Gremlin 및 SQL API 계정 모두에서 액세스할 수 있습니다.
* Table API 계정은 테이블 및 SQL API 계정 모두에서 액세스할 수 있습니다.

이러한 계정에 대해 각 API 형식에 대해 하나의 개인 끝점을 만들어야 합니다. 해당 API 형식은 `GroupId` 배열에서 지정 됩니다.

템플릿이 성공적으로 배포 되 면 다음 이미지에 표시 된 것과 유사한 출력을 볼 수 있습니다. 개인 `provisioningState` 끝점이 올바르게 `Succeeded` 설정 된 경우이 값은입니다.

![리소스 관리자 템플릿에 대 한 배포 출력](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

템플릿을 배포한 후에는 개인 IP 주소가 서브넷 내에서 예약 됩니다. Azure Cosmos 계정의 방화벽 규칙은 개인 끝점의 연결만 허용 하도록 구성 됩니다.

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>사설 DNS 영역에 개인 끝점 통합

다음 코드를 사용 하 여 "PrivateZone_template. json" 이라는 리소스 관리자 템플릿을 만듭니다. 이 템플릿은 기존 가상 네트워크에서 기존 Azure Cosmos SQL API 계정에 대 한 개인 DNS 영역을 만듭니다.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "type": "string"
        },
        "VNetId": {
            "type": "string"
        }        
    },
    "resources": [
        {
            "name": "[parameters('privateZoneName')]",
            "type": "Microsoft.Network/privateDnsZones",
            "apiVersion": "2018-09-01",
            "location": "global",
            "properties": {                
            }
        },
        {
            "type": "Microsoft.Network/privateDnsZones/virtualNetworkLinks",
            "apiVersion": "2018-09-01",
            "name": "[concat(parameters('privateZoneName'), '/myvnetlink')]",
            "location": "global",
            "dependsOn": [
                "[resourceId('Microsoft.Network/privateDnsZones', parameters('privateZoneName'))]"
            ],
            "properties": {
                "registrationEnabled": false,
                "virtualNetwork": {
                    "id": "[parameters('VNetId')]"
                }
            }
        }        
    ]
}
```

다음 코드를 사용 하 여 "PrivateZoneRecords_template. json" 이라는 리소스 관리자 템플릿을 만듭니다.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "type": "string"
        },
        "IPAddress": {
            "type":"string"
        }        
    },
    "resources": [
         {
            "type": "Microsoft.Network/privateDnsZones/A",
            "apiVersion": "2018-09-01",
            "name": "[parameters('DNSRecordName')]",
            "properties": {
                "ttl": 300,
                "aRecords": [
                    {
                        "ipv4Address": "[parameters('IPAddress')]"
                    }
                ]
            }
        }    
    ]
}
```

**템플릿에 대 한 매개 변수 파일을 정의 합니다.**

템플릿에 대 한 다음 두 개의 매개 변수 파일을 만듭니다. "PrivateZone_parameters. json"을 만듭니다. 다음 코드와 바꿉니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "value": ""
        },
        "VNetId": {
            "value": ""
        }
    }
}
```

"PrivateZoneRecords_parameters. json"을 만듭니다. 다음 코드와 바꿉니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "value": ""
        },
        "IPAddress": {
            "type":"object"
        }
    }
}
```

**PowerShell 스크립트를 사용 하 여 템플릿 배포**

다음 코드를 사용 하 여 PowerShell 스크립트를 만듭니다. 스크립트를 실행 하기 전에 구독 ID, 리소스 그룹 이름 및 기타 변수 값을 사용자 환경에 대 한 세부 정보로 바꿉니다.

```azurepowershell-interactive
### This script:
### - creates a private zone
### - creates a private endpoint for an existing Cosmos DB account in an existing VNet
### - maps the private endpoint to the private zone

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private zone to create
$PrivateZoneName = "myPrivateZone.documents.azure.com"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateZoneTemplateFilePath = "PrivateZone_template.json"
$PrivateZoneParametersFilePath = "PrivateZone_parameters.json"
$PrivateZoneRecordsTemplateFilePath = "PrivateZoneRecords_template.json"
$PrivateZoneRecordsParametersFilePath = "PrivateZoneRecords_parameters.json"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Login your Azure account and select the target subscription
Login-AzAccount 
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private zone
New-AzResourceGroupDeployment -Name "PrivateZoneDeployment" `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $PrivateZoneTemplateFilePath `
    -TemplateParameterFile $PrivateZoneParametersFilePath `
    -PrivateZoneName $PrivateZoneName `
    -VNetId $VNetResourceId

## Step 5: Create the private endpoint
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName
$deploymentOutput

## Step 6: Map the private endpoint to the private zone
$networkInterface = Get-AzResource -ResourceId $deploymentOutput.Outputs.privateEndpointNetworkInterface.Value -ApiVersion "2019-04-01"
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) {
    foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) {
        $recordName = $fqdn.split('.',2)[0]
        $dnsZone = $fqdn.split('.',2)[1]
        Write-Output "Deploying PrivateEndpoint DNS Record $($PrivateZoneName)/$($recordName) Template on $($resourceGroupName)"
        New-AzResourceGroupDeployment -Name "PrivateEndpointDNSDeployment" `
            -ResourceGroupName $ResourceGroupName `
            -TemplateFile $PrivateZoneRecordsTemplateFilePath `
            -TemplateParameterFile $PrivateZoneRecordsParametersFilePath `
            -DNSRecordName "$($PrivateZoneName)/$($RecordName)" `
            -IPAddress $ipconfig.properties.privateIPAddress
    }
}
```

## <a name="configure-custom-dns"></a>사용자 지정 DNS 구성

개인 끝점을 만든 서브넷 내에서 개인 DNS 영역을 사용 해야 합니다. 각 개인 IP 주소가 DNS 항목에 매핑되도록 끝점을 구성 합니다. 앞에 표시 `fqdns` 된 응답에서 속성을 참조 하세요.

개인 끝점을 만들 때 Azure의 개인 DNS 영역과 통합할 수 있습니다. 대신 사용자 지정 DNS 영역을 사용 하도록 선택 하는 경우 개인 끝점에 예약 된 모든 개인 IP 주소에 대 한 DNS 레코드를 추가 하도록 구성 해야 합니다.

## <a name="private-link-combined-with-firewall-rules"></a>방화벽 규칙과 결합 된 개인 링크

방화벽 규칙과 함께 개인 링크를 사용 하는 경우 다음과 같은 상황 및 결과가 발생할 수 있습니다.

* 방화벽 규칙을 구성 하지 않으면 기본적으로 모든 트래픽이 Azure Cosmos 계정에 액세스할 수 있습니다.

* 공용 트래픽 또는 서비스 끝점을 구성 하 고 개인 끝점을 만드는 경우 해당 유형의 방화벽 규칙에 따라 들어오는 트래픽 유형이 서로 다릅니다.

* 공용 트래픽 또는 서비스 끝점을 구성 하지 않고 개인 끝점을 만드는 경우 Azure Cosmos 계정은 개인 끝점을 통해서만 액세스할 수 있습니다. 공용 트래픽 또는 서비스 끝점을 구성 하지 않으면 승인 된 모든 개인 끝점을 거부 하거나 삭제 한 후 계정이 전체 네트워크에 열립니다.

## <a name="blocking-public-network-access-during-account-creation"></a>계정을 만드는 동안 공용 네트워크 액세스 차단

이전 섹션에서 설명한 것 처럼 특정 방화벽 규칙이 설정 되지 않은 경우 개인 끝점을 추가 하면 개인 끝점을 통해 Azure Cosmos 계정에 액세스할 수 있습니다. 이는 Azure Cosmos 계정이 만들어진 후 개인 끝점을 추가 하기 전에 공용 트래픽으로 도달할 수 있음을 의미 합니다. 개인 끝점을 만들기 전에도 공용 네트워크 액세스를 사용 하지 않도록 설정 하려면 계정을 만드는 동안 `publicNetworkAccess` 플래그를로 `Disabled` 설정 하면 됩니다. 이 플래그를 사용 하는 방법을 보여 주는 예제는 [이 Azure Resource Manager 템플릿](https://azure.microsoft.com/resources/templates/101-cosmosdb-private-endpoint/) 을 참조 하세요.

## <a name="update-a-private-endpoint-when-you-add-or-remove-a-region"></a>영역을 추가 하거나 제거 하는 경우 개인 끝점 업데이트

Azure Cosmos 계정에 지역을 추가 하거나 제거 하려면 해당 계정에 대 한 DNS 항목을 추가 하거나 제거 해야 합니다. 영역을 추가 하거나 제거한 후에는 추가 되거나 제거 된 DNS 항목과 해당 개인 IP 주소를 반영 하도록 서브넷의 개인 DNS 영역을 업데이트할 수 있습니다.

예를 들어, "미국 서 부", "미국 중부" 및 "유럽 서부"의 세 지역에서 Azure Cosmos 계정을 배포 한다고 가정 합니다. 계정에 대 한 개인 끝점을 만들 때 4 개의 개인 Ip가 서브넷에 예약 되어 있습니다. 세 지역 각각에 대해 하나의 IP가 있고 전역/지역에 구애 받지 않는 끝점에 대 한 하나의 IP가 있습니다.

나중에 Azure Cosmos 계정에 새 지역 (예: "미국 동부")을 추가할 수 있습니다. 새 지역을 추가한 후에는 해당 DNS 레코드를 개인 DNS 영역 또는 사용자 지정 DNS에 추가 해야 합니다.

지역을 제거할 때와 동일한 단계를 사용할 수 있습니다. 영역을 제거한 후에는 개인 DNS 영역 또는 사용자 지정 DNS에서 해당 DNS 레코드를 제거 해야 합니다.

## <a name="current-limitations"></a>현재 제한 사항

Azure Cosmos 계정에 개인 링크를 사용 하는 경우 다음 제한 사항이 적용 됩니다.

* 직접 모드 연결을 사용 하 여 Azure Cosmos 계정에 개인 링크를 사용 하는 경우 TCP 프로토콜만 사용할 수 있습니다. HTTP 프로토콜은 아직 지원 되지 않습니다.

* 개인 끝점 지원은 현재 게이트웨이 연결 모드 에서만 일반적으로 사용할 수 있습니다. 직접 모드의 경우에는 미리 보기 기능으로 사용할 수 있습니다.

* MongoDB 계정에 대 한 Azure Cosmos DB API를 사용 하는 경우 전용 끝점은 서버 버전 3.6의 계정 (즉, 형식의 `*.mongo.cosmos.azure.com`끝점을 사용 하는 계정)에 대해서만 지원 됩니다. 서버 버전 3.2 (즉, 형식의 `*.documents.azure.com`끝점을 사용 하는 계정)의 계정에 대해서는 개인 링크가 지원 되지 않습니다. 개인 링크를 사용 하려면 이전 계정을 새 버전으로 마이그레이션해야 합니다.

* 개인 링크가 있는 MongoDB 계정에 대 한 Azure Cosmos DB API를 사용 하는 경우 Robo 3T, 스튜디오 3T 및 Mongoose와 같은 도구를 사용할 수 없습니다. `appName=<account name>` 매개 변수가 지정 된 경우에만 끝점이 개인 링크를 지원할 수 있습니다. 예제는 `replicaSet=globaldb&appName=mydbaccountname`입니다. 이러한 도구는 연결 문자열의 앱 이름을 서비스에 전달 하지 않으므로 개인 링크를 사용할 수 없습니다. 그러나 3.6 버전의 SDK 드라이버를 사용 하 여 이러한 계정에 계속 액세스할 수 있습니다.

* 개인 링크를 포함 하는 경우에는 가상 네트워크를 이동 하거나 삭제할 수 없습니다.

* Azure Cosmos 계정이 개인 끝점에 연결 된 경우에는 삭제할 수 없습니다.

* Azure Cosmos 계정은 계정에 연결 된 모든 개인 끝점에 매핑되지 않은 지역으로 장애 조치 (failover) 할 수 없습니다.

* 네트워크 관리자는 Azure Cosmos 계정 범위에서 "*/PrivateEndpointConnectionsApproval" 이상의 권한을 부여 받아야 자동으로 승인 된 개인 끝점을 만들 수 있습니다.

### <a name="limitations-to-private-dns-zone-integration"></a>개인 DNS 영역 통합에 대 한 제한 사항

개인 DNS 영역의 DNS 레코드는 개인 끝점을 삭제 하거나 Azure Cosmos 계정에서 지역을 제거할 때 자동으로 제거 되지 않습니다. 이전에 DNS 레코드를 수동으로 제거 해야 합니다.

* 이 개인 DNS 영역에 연결 된 새 개인 끝점을 추가 합니다.
* 이 개인 DNS 영역에 연결 된 개인 끝점이 있는 모든 데이터베이스 계정에 새 영역을 추가 합니다.

DNS 레코드를 정리 하지 않으면 예기치 않은 데이터 평면 문제가 발생할 수 있습니다. 이러한 문제에는 개인 끝점 제거 또는 지역 제거 후에 추가 된 지역에 대 한 데이터 중단이 포함 됩니다.

## <a name="next-steps"></a>다음 단계

Azure Cosmos DB 보안 기능에 대해 자세히 알아보려면 다음 문서를 참조 하세요.

* Azure Cosmos DB에 대 한 방화벽을 구성 하려면 [방화벽 지원](firewall-support.md)을 참조 하세요.

* Azure Cosmos 계정에 대 한 가상 네트워크 서비스 끝점을 구성 하는 방법을 알아보려면 [가상 네트워크에서 액세스 구성](how-to-configure-vnet-service-endpoint.md)을 참조 하세요.

* 개인 링크에 대 한 자세한 내용은 [Azure 개인 링크](../private-link/private-link-overview.md) 설명서를 참조 하세요.
