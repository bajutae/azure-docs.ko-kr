---
title: Azure 가상 머신 기술 자산 만들기
description: Azure Marketplace에 대 한 VM (가상 머신) 제품에 대 한 기술 자산을 만들고 구성 하는 방법에 대해 알아봅니다.
author: dannyevers
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/13/2020
ms.openlocfilehash: 4d2d33f9d83132147b5b257ffcd6d659f272b8ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81730714"
---
# <a name="create-your-azure-virtual-machine-technical-assets"></a>Azure 가상 머신 기술 자산 만들기

> [!IMPORTANT]
> Azure Virtual Machine 제품의 관리를 Cloud 파트너 포털에서 파트너 센터로 전환 하 고 있습니다. 제품이 마이그레이션될 때까지 [Cloud 파트너 포털에 대 한 virtual machine 제품에 대 한 기술 자산 만들기](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-technical-assets) 의 지침에 따라 제품을 관리 하세요.

이 문서에서는 Azure Marketplace에 대 한 VM (가상 머신) 제품에 대 한 기술 자산을 만들고 구성 하는 방법을 설명 합니다. VM에는 운영 체제 VHD (가상 하드 디스크) 및 선택적으로 연결 된 데이터 디스크 Vhd 라는 두 가지 구성 요소가 포함 되어 있습니다.

* **운영 체제 VHD** – 제품과 함께 배포 되는 운영 체제 및 솔루션을 포함 합니다. VHD를 준비 하는 프로세스는 Linux 기반, Windows 기반 또는 사용자 지정 기반 VM 인지에 따라 달라 집니다.
* **데이터 디스크 vhd** -VM의 전용 영구 저장소입니다. 운영 체제 VHD (예: C: 드라이브)를 사용 하 여 영구 정보를 저장 하지 마세요.

VM 이미지는 하나의 운영 체제 디스크와 최대 16 개의 데이터 디스크를 포함 합니다. 디스크가 비어 있는 경우에도 데이터 디스크당 VHD 하나를 사용 합니다.

> [!NOTE]
> 사용 중인 운영 체제에 관계 없이 솔루션에 필요한 최소 개수의 데이터 디스크만 추가 합니다. 고객은 배포 시 이미지의 일부인 디스크를 제거할 수 없지만, 배포 중에 또는 배포 후에 항상 디스크를 추가할 수 있습니다.

> [!IMPORTANT]
> 계획의 모든 VM 이미지에는 동일한 수의 데이터 디스크가 있어야 합니다.

## <a name="fundamental-technical-knowledge"></a>기본 기술 지식

이러한 자산의 디자인, 빌드 및 테스트는 시간이 걸리고 제품을 빌드하는 데 사용 되는 Azure 플랫폼 및 기술에 대 한 기술 지식이 필요 합니다. 엔지니어링 팀은 솔루션 도메인 외에 다음과 같은 Microsoft 기술에 대해 알고 있어야 합니다.

* [Azure 서비스](https://azure.microsoft.com/services/)에 대한 기본적 이해
* [Azure 애플리케이션을 디자인 및 설계](https://azure.microsoft.com/solutions/architecture/)하는 방법
* [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) 및 [Azure 네트워킹](https://azure.microsoft.com/services/?filter=networking)에 대한 실무 지식
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)에 대한 실무 지식
* [JSON](https://www.json.org/)에 대한 실무 지식

## <a name="suggested-tools--optional"></a>권장 도구-선택 사항

Vm 및 Vhd를 관리할 수 있도록 다음 스크립팅 환경 중 하나를 사용 하는 것이 좋습니다.

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [Azure CLI](https://code.visualstudio.com/)

또한 개발 환경에 다음 도구를 추가 하는 것이 좋습니다.

* [Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Visual Studio Code](https://code.visualstudio.com/)
  * 확장: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  * 확장: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  * 확장: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

[Azure 개발자 도구](https://azure.microsoft.com/product-categories/developer-tools/) 페이지에서 사용 가능한 도구를 검토 하 고 Visual Studio를 사용 하는 경우 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)합니다.

## <a name="create-a-vm-image-using-an-approved-base"></a>승인 된 기본을 사용 하 여 VM 이미지 만들기

> [!NOTE]
> 자신의 온-프레미스에서 빌드된 이미지를 사용 하 여 가상 머신 기술 자산을 만들려면 [고유한 이미지를 사용 하 여 VM 만들기](#create-a-vm-using-your-own-image)로 이동 합니다.

이 섹션에서는 RDP (원격 데스크톱 프로토콜) 사용, VM 크기 선택, 최신 Windows 업데이트 설치 및 VHD 이미지 일반화와 같이 승인 된 기반 사용의 다양 한 측면을 설명 합니다.

다음 섹션에서는 windows 기반 Vhd에 대해 주로 다룹니다. Linux 기반 Vhd를 만드는 방법에 대 한 자세한 내용은 [Azure에서 보증 배포의 linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)를 참조 하세요.

> [!WARNING]
> 이 항목의 지침에 따라 Azure를 사용 하 여 미리 구성 된 보증 운영 체제를 포함 하는 VM을 만듭니다. 솔루션과 호환 되지 않는 경우 승인 된 운영 체제를 사용 하 여 온-프레미스 VM을 만들고 구성할 수 있습니다. 그런 다음, [Azure에 업로드할 Windows VHD 또는 VHDX 준비](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)에서 설명한 대로 업로드할 수 있도록 구성하고 준비할 수 있습니다.

### <a name="select-an-approved-base"></a>승인된 기본 이미지 선택

Windows 운영 체제 또는 Linux를 기본으로 선택 합니다.

#### <a name="windows"></a>Windows

Windows 기반 VM 이미지에 대 한 운영 체제 VHD는 Windows Server 또는 SQL Server 포함 하는 Azure 승인 기본 이미지를 기반으로 해야 합니다. 시작 하려면 Azure Portal에서 다음 이미지 중 하나에서 VM을 만듭니다.

* Windows Server([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
* [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)
* [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)

> [!NOTE]
> 현재 Azure Portal 또는 Azure PowerShell를 사용 하는 경우 2014 년 9 월 8 일 이후에 게시 된 Windows Server 이미지가 승인 됩니다.

#### <a name="linux"></a>Linux

Azure는 승인 된 Linux 배포판의 범위를 제공 합니다. 현재 목록은 [Azure 보증 배포판의 Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)를 참조하세요.

### <a name="create-vm-in-the-azure-portal"></a>Azure Portal에서 VM 만들기

[Azure Portal](https://ms.portal.azure.com/)에서 기본 VM 이미지를 만들려면 다음 단계를 수행 합니다.

1. VM 제품을 게시 하는 데 사용 하려는 Azure 구독과 연결 된 Microsoft 계정를 사용 하 여 [Azure Portal](https://ms.portal.azure.com/) 에 로그인 합니다.
2. 새 리소스 그룹을 만들고, **리소스 그룹 이름**, **구독** 및 **리소스 그룹 위치**를 제공합니다. 자세한 내용은 [리소스 관리](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)를 참조 하세요.
3. 왼쪽에서 **가상 컴퓨터** 를 선택 하 여 가상 컴퓨터 세부 정보 페이지를 표시 합니다.
4. **+ 추가** 를 선택 하 여 **가상 머신 만들기 환경을**엽니다.
5. 드롭다운 목록에서 이미지를 선택 하거나 **모든 공용 및 개인 이미지 찾아보기** 를 클릭 하 여 사용 가능한 모든 가상 머신 이미지를 검색 하거나 찾아봅니다.
6. 다음 권장 사항을 사용하여 배포할 VM의 크기를 선택합니다.
    * 온-프레미스에서 VHD를 개발 하려는 경우 크기는 중요 하지 않습니다. 더 작은 VM 중 하나를 사용하는 것이 좋습니다.
    * Azure에서 이미지를 개발하려는 경우 선택된 이미지에 대한 권장 VM 크기 중 하나를 사용하는 것이 좋습니다.

7. **디스크** 섹션에서 **고급** 섹션을 확장 하 고 **관리 디스크 사용** 옵션을 **아니요**로 설정 합니다.
8. VM을 만드는 데 필요한 다른 세부 정보를 제공 합니다.
9. **검토 + 만들기** 를 선택 하 여 선택 항목을 검토 합니다. **유효성 검사 통과** 메시지가 표시되면 **만들기**를 선택합니다.

Azure에서 지정한 가상 머신의 프로 비전을 시작 합니다. 왼쪽의 **Virtual Machines** 탭을 선택 하 여 진행 상황을 추적할 수 있습니다. 만들어진 후 상태가 **실행 중**으로 변경 됩니다.

새 Azure 기반 VHD를 만드는 데 문제가 발생 하 [는 경우 vhd를 만드는 동안 발생 하는 일반적인 문제 (faq)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/common-issues-during-vhd-creation)를 참조 하세요.

### <a name="connect-to-your-azure-vm"></a>Azure VM에 연결

이 섹션에서는 Azure에서 만든 VM에 연결 하 고 로그인 하는 방법을 설명 합니다. 성공적으로 연결 되 면 해당 호스트 서버에 로컬에 로그인 한 것 처럼 VM을 사용할 수 있습니다.

#### <a name="connect-to-a-windows-based-vm"></a>Windows 기반 VM에 연결

원격 데스크톱 클라이언트를 사용 하 여 Azure에서 호스트 되는 Windows 기반 VM에 연결 합니다. 대부분의 Windows 버전에는 기본적으로 RDP(원격 데스크톱 프로토콜)에 대한 지원이 포함되어 있습니다. 다른 운영 체제의 경우 [원격 데스크톱 클라이언트](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)에서 클라이언트에 대 한 자세한 정보를 찾을 수 있습니다.

이 문서에서는 Windows를 실행 하는 [Azure 가상 머신에 연결 하 고 로그온 하는 방법](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon): 기본 제공 windows RDP 지원을 사용 하 여 VM에 연결 하는 방법에 대해 자세히 설명 합니다.

> [!TIP]
> 프로세스 중에 보안 경고가 표시 될 수 있습니다. 예를 들어 ".rdp 파일은 알 수 없는 게시자의 파일입니다." 또는 "사용자 자격 증명을 확인할 수 없습니다."와 같은 경고가 발생 합니다. 이러한 경고는 무시해도 됩니다.

#### <a name="connect-to-a-linux-based-vm"></a>Linux 기반 VM에 연결

Linux 기반 VM에 연결 하려면 SSH (보안 셸 프로토콜) 클라이언트가 필요 합니다. 다음 단계에서는 무료 [PuTTY](https://www.ssh.com/ssh/putty/) shh 터미널을 사용 합니다.

1. [Azure 포털](https://ms.portal.azure.com/)로 이동합니다.
2. **가상 머신**을 검색하여 선택합니다.
3. 연결 하려는 VM을 선택 합니다.
4. VM이 아직 실행 되 고 있지 않으면 시작 합니다.
5. VM의 이름을 선택 하 여 해당 **개요** 페이지를 엽니다.
6. VM의 공용 IP 주소 및 DNS 이름을 적어둡니다 (이러한 값이 설정 되지 않은 경우 [네트워크 인터페이스를 만들어야](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)함).
7. PuTTY 애플리케이션을 엽니다.
8. [PuTTY 구성] 대화 상자에서 VM의 IP 주소 또는 DNS 이름을 입력합니다.

    :::image type="content" source="media/avm-putty.png" alt-text="PuTTY 터미널 설정을 보여 줍니다. 호스트 이름 또는 IP 주소와 포트 상자가 강조 표시 됩니다.":::

9. **열기** 를 선택 하 여 PuTTY 터미널을 엽니다.
10. 메시지가 표시 되 면 Linux VM 계정의 계정 이름 및 암호를 입력 합니다.

연결 문제가 있는 경우 SSH 클라이언트에 대 한 설명서를 참조 하세요. 예를 들어, [10 장: 일반적인 오류 메시지](https://www.ssh.com/ssh/putty/putty-manuals)입니다.

프로 비전 된 Linux VM에 데스크톱을 추가 하는 방법을 포함 하 여 자세한 내용은 [Azure에서 LINUX vm에 연결 하는 원격 데스크톱 설치 및 구성](https://docs.microsoft.com/azure/virtual-machines/linux/use-remote-desktop)을 참조 하세요.

## <a name="create-a-vm-using-your-own-image"></a>사용자 고유의 이미지를 사용 하 여 VM 만들기

이 섹션에서는 사용자 제공 VM (가상 머신) 이미지를 만들고 배포 하는 방법을 설명 합니다. Azure에서 배포 된 VHD (가상 하드 디스크)에서 운영 체제 및 데이터 디스크 VHD 이미지를 제공 하 여이 작업을 수행할 수 있습니다.

> [!NOTE]
> 승인 된 기본 이미지를 선택적으로 사용 하려면 [승인 된 기본 이미지를 사용 하 여 VM 이미지 만들기](#create-a-vm-image-using-an-approved-base)의 지침을 따르세요.

1. Azure Storage 계정에 이미지를 업로드 합니다.
2. VM 이미지를 배포 합니다.
3. VM 이미지를 캡처합니다.

### <a name="upload-your-images-to-an-azure-storage-account"></a>Azure storage 계정에 이미지 업로드

1. [Azure 포털](https://portal.azure.com/)에 로그온합니다.
2. 일반화 된 운영 체제 VHD 및 데이터 디스크 Vhd를 Azure storage 계정에 업로드 합니다.

### <a name="deploy-your-image"></a>이미지 배포

Azure Portal 또는 Azure PowerShell를 사용 하 여 이미지를 만듭니다.

#### <a name="deploy-using-the-azure-portal"></a>Azure Portal을 사용하여 배포

1. 홈 페이지에서 **리소스 만들기**를 선택 하 고 "템플릿 배포"를 검색 한 후 **만들기**를 선택 합니다.
2. **편집기에서 고유한 템플릿 빌드**를 선택 합니다.

    :::image type="content" source="media/avm-custom-deployment.png" alt-text="사용자 지정 배포 페이지를 보여 줍니다.":::

3. 편집기에이 [JSON 템플릿을](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-deploy-json-template) 붙여넣고 **저장**을 선택 합니다.
4. 표시된 **사용자 지정 배포** 속성 페이지에 대한 매개 변수 값을 제공합니다.

    | 매개 변수 | Description |
    | ------------ | ------------- |
    | 사용자 스토리지 계정 이름 | 셀 2의 콘텐츠 |
    | 사용자 스토리지 컨테이너 이름 | 일반화된 VHD가 있는 스토리지 계정 이름 |
    | 공용 IP에 대한 DNS 이름 | 공용 IP DNS 이름입니다. 제품이 배포 된 후 Azure Portal의 공용 IP 주소에 대 한 DNS 이름을 정의 합니다. |
    | 관리자 사용자 이름 | 새 VM에 대한 관리자 계정의 사용자 이름 |
    | 관리자 암호 | 새 VM에 대한 관리자 계정의 암호 |
    | OS 유형 | VM 운영 체제: Windows 또는 Linux |
    | 구독 ID | 선택한 구독의 식별자 |
    | 위치 | 배포의 지리적 위치 |
    | VM 크기 | [AZURE VM 크기](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)(예: Standard_A2 |
    | 공용 IP 주소 이름 | 공용 IP 주소의 이름 |
    | VM 이름 | 새 VM의 이름 |
    | Virtual Network 이름 | VM에서 사용하는 가상 네트워크의 이름 |
    | NIC 이름 | 가상 네트워크를 실행하는 네트워크 인터페이스 카드의 이름 |
    | VHD URL | 완전한 OS 디스크 VHD URL |
    |  |  |

5. 이러한 값을 제공한 후 **구매**를 선택 합니다.

Azure에서 배포를 시작 합니다. 지정 된 저장소 계정 경로에 지정 된 관리 되지 않는 VHD를 사용 하 여 새 VM을 만듭니다. 포털의 왼쪽에서 **Virtual Machines** 를 선택 하 여 Azure Portal 진행률을 추적할 수 있습니다. VM을 만들면 상태가 시작 중에서 실행 중으로 변경 됩니다.

#### <a name="deploy-using-azure-powershell"></a>Azure PowerShell을 사용하여 배포

```powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

### <a name="capture-the-vm-image"></a>VM 이미지 캡처

사용자의 접근 방법에 해당 하는 다음 지침을 사용 합니다.

* Azure PowerShell: [AZURE vm에서 관리 되지 않는 vm 이미지를 만드는 방법](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)
* Azure CLI: [가상 머신 또는 VHD의 이미지를 만드는 방법](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)
* API: [Virtual Machines - 캡처](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

## <a name="configure-the-virtual-machine"></a>가상 컴퓨터 구성

이 섹션에서는 Azure VM의 크기를 조정 하 고, 업데이트 하 고, 일반화 하는 방법을 설명 합니다. 이러한 단계는 Azure Marketplace에 배포할 VM을 준비 하는 데 필요 합니다.

### <a name="sizing-the-vhds"></a>VHD 크기 조정

운영 체제와 함께 미리 구성 된 Vm 중 하나 (필요한 경우 추가 서비스)를 선택한 경우 표준 Azure VM 크기를 이미 선택 했습니다. 사전 구성된 OS로 솔루션을 시작하는 것이 좋습니다. 그러나 OS를 수동으로 설치 하는 경우 VM 이미지에서 기본 VHD의 크기를 조정 해야 합니다.

* Windows의 경우 운영 체제 VHD를 127-128gb 고정 형식 VHD로 만들어야 합니다.
* Linux의 경우이 VHD는 30 – 50gb의 고정 형식 VHD로 만들어져야 합니다.

실제 크기가 127 – 128gb 보다 작은 경우 VHD는 확장 가능 해야 합니다 (스파스/동적). 제공 된 기본 Windows 및 SQL Server 이미지는 이미 이러한 요구 사항을 충족 하므로 VHD의 형식 또는 크기를 변경 하지 마세요.

데이터 디스크는 1TB이하여야 합니다. 크기를 결정할 때 고객은 배포 시 이미지 내에서 Vhd 크기를 조정할 수 없습니다. 데이터 디스크 VHD는 고정 형식 VHD로 만들어야 합니다. 또한 확장 가능 해야 합니다 (스파스/동적). 처음에는 데이터 디스크가 비어 있거나 데이터를 포함할 수 있습니다.

### <a name="install-the-most-current-updates"></a>최신 업데이트 설치

운영 체제 Vm의 기본 이미지는 게시 된 날짜 까지의 최신 업데이트를 포함 해야 합니다. 만든 운영 체제 VHD를 게시 하기 전에 최신 보안 및 유지 관리 패치를 모두 사용 하 여 OS 및 설치 된 모든 서비스를 업데이트 해야 합니다.

Windows Server의 경우 **업데이트 확인** 명령을 실행 합니다.

Linux 배포판의 경우 일반적으로 명령줄 도구 또는 그래픽 유틸리티를 통해 업데이트를 다운로드하고 설치합니다. 예를 들어 Ubuntu Linux는 [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) 명령과 [업데이트 관리자](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) 도구를 제공하여 OS를 업데이트합니다.

### <a name="perform-additional-security-checks"></a>추가 보안 검사 수행

Azure Marketplace에서 솔루션 이미지에 대 한 높은 수준의 보안을 유지 합니다. 다음 문서에서는 [Azure Marketplace 이미지에 대 한 보안 권장 사항을](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)제공 하는 데 도움이 되는 보안 구성 및 절차에 대 한 검사 목록을 제공 합니다. 이러한 권장 사항 중 일부는 Linux 기반 이미지에만 적용되지만, 대부분은 모든 VM 이미지에 적용됩니다.

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>사용자 지정 구성 및 예약된 작업 수행

추가 구성이 필요한 경우 시작 시 실행 되는 예약 된 작업을 사용 하 여 VM을 배포한 후 VM에 대 한 최종 변경을 수행 합니다. 다음 권장 사항도 고려하세요.

* 한 번 실행 되는 작업 인 경우 작업이 성공적으로 완료 된 후 자신을 삭제 해야 합니다.
* 이러한 두 드라이브만 항상 존재 하는 것으로 보장 되므로 구성은 C 또는 D 이외의 드라이브에 의존해 서는 안 됩니다. C 드라이브는 운영 체제 디스크이 고 D 드라이브는 임시 로컬 디스크입니다.

Linux 사용자 지정에 대한 자세한 내용은 [Linux용 가상 머신 확장 및 기능](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux)을 참조하세요.

## <a name="generalize-the-image"></a>이미지 일반화

Azure Marketplace의 모든 이미지는 일반적으로 다시 사용할 수 있어야 합니다. 이렇게 하려면 운영 체제 VHD를 일반화 해야 합니다 .이 작업은 VM에서 모든 인스턴스별 식별자와 소프트웨어 드라이버를 제거 하는 작업입니다.

### <a name="windows"></a>Windows

Windows OS 디스크는 [sysprep 도구](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)를 사용하여 일반화됩니다. 나중에 OS를 업데이트하거나 재구성하면 sysprep을 다시 실행해야 합니다.

> [!WARNING]
> 업데이트는 자동으로 실행 될 수 있으므로 sysprep을 실행 한 후 배포 될 때까지 VM을 끕니다. 이렇게 종료 하면 후속 업데이트에서 운영 체제나 설치 된 서비스에 대 한 인스턴스별 변경을 수행 하지 않습니다. Sysprep을 실행 하는 방법에 대 한 자세한 내용은 [VHD를 일반화 하는 단계](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep)를 참조 하세요.

### <a name="linux"></a>Linux

다음 프로세스는 Linux VM을 일반화 별도의 VM으로 다시 배포 합니다. 자세한 내용은 [가상 컴퓨터 또는 VHD의 이미지를 만드는 방법](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)을 참조 하세요. "캡처된 이미지에서 VM 만들기" 섹션에 도달 하면를 중지할 수 있습니다.

1. **Azure Linux 에이전트 제거**

    1. SSH 클라이언트를 사용하여 Linux VM에 연결합니다.
    2. SSH 창에서 다음 명령을 입력 `sudo waagent -deprovision+user`합니다.
    3. **Y** 를 입력 하 여 계속 합니다. **-force** 매개 변수를 이전 명령에 추가 하 여 확인 단계를 방지할 수 있습니다.
    d. 명령이 완료 되 면 **Exit** 를 입력 하 여 SSH 클라이언트를 닫습니다.

2. **가상 머신 중지**

    1. Azure Portal에서 리소스 그룹 (RG)을 선택 하 고 VM을 할당 취소 합니다.
    2. 이제 VHD가 일반화 되어이 VHD를 사용 하 여 새 VM을 만들 수 있습니다.

## <a name="next-steps"></a>다음 단계

새 Azure 기반 VHD를 만드는 데 어려움이 있는 경우 [일반적인 VHD 만들기 문제](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-common-vhd-creation-issues)를 참조하세요.

그렇지 않은 경우는 다음과 같습니다.

* [Vm 이미지](https://docs.microsoft.com/azure/marketplace/partner-center-portal/get-sas-uri) 를 인증 하는 방법에 대 한 자세한 내용은 Azure 인증 도구용 *인증 테스트 도구* 를 가져올 위치 및 vm 이미지를 인증 하는 데 사용 하는 방법을 비롯 하 여 AZURE MARKETPLACE 인증을 위해 vm 이미지를 테스트 하 고 제출 하는 방법을 설명 합니다
