---
title: Azure Automation Windows Hybrid Runbook Worker
description: 이 문서에서는 로컬 데이터 센터 또는 클라우드 환경의 Windows 기반 컴퓨터에서 Runbook을 실행할 수 있도록 해주는 Azure Automation Hybrid Runbook Worker를 설치하는 방법에 대한 정보를 제공합니다.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 163650a05bf47e6cb8a8832bb85477740d88b0cd
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82787378"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows Hybrid Runbook Worker 배포

Azure Automation의 Hybrid Runbook Worker 기능을 사용하여 역할을 호스팅하는 컴퓨터에서 직접 그리고 환경의 리소스에 대해 Runbook을 실행하여 해당 로컬 리소스를 관리할 수 있습니다. Azure Automation는 runbook을 저장 하 고 관리 한 다음 하나 이상의 지정 된 컴퓨터에 배달 합니다. 이 문서에서는 Windows 컴퓨터에 Hybrid Runbook Worker를 배포 하는 방법을 설명 합니다.

Runbook Worker를 성공적으로 배포한 후에는 [Hybrid Runbook Worker에서 Runbook 실행](automation-hrw-run-runbooks.md)을 검토하여 온-프레미스 데이터 센터 또는 다른 클라우드 환경의 프로세스를 자동화하도록 Runbook을 구성하는 방법을 알아봅니다.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>이 문서는 새 Azure PowerShell Az 모듈을 사용하도록 업데이트되었습니다. AzureRM 모듈은 적어도 2020년 12월까지 버그 수정을 수신할 예정이므로 계속 사용하셔도 됩니다. 새 Az 모듈 및 AzureRM 호환성에 대한 자세한 내용은 [새 Azure PowerShell Az 모듈 소개](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)를 참조하세요. Hybrid Runbook Worker에 대한 Az 모듈 설치 지침은 [Azure PowerShell 모듈 설치](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)를 참조하세요. Automation 계정의 경우 [Azure Automation에서 Azure PowerShell 모듈을 업데이트하는 방법](automation-update-azure-modules.md)을 사용하여 모듈을 최신 버전으로 업데이트할 수 있습니다.

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Windows Hybrid Runbook Worker 설치 및 구성

Windows Hybrid Runbook Worker를 설치 하 고 구성 하려면 다음 방법 중 하나를 사용할 수 있습니다.

* Azure Vm의 경우 [windows 용 가상 머신 확장](../virtual-machines/extensions/oms-windows.md)을 사용 하 여 windows 용 Log Analytics 에이전트를 설치 합니다. 확장은 Azure 가상 컴퓨터에 Log Analytics 에이전트를 설치 하 고 Azure Resource Manager 템플릿 또는 PowerShell을 사용 하 여 기존 Log Analytics 작업 영역에 가상 컴퓨터를 등록 합니다. 에이전트가 설치 되 면 Automation 계정의 Hybrid Runbook Worker 그룹에 VM을 추가할 수 있습니다. [수동 배포](#manual-deployment) 섹션에서 3 단계와 4 단계를 참조 하세요.

* Automation runbook을 사용 하 여 Windows 컴퓨터를 구성 하는 프로세스를 완전히 자동화할 수 있습니다. 이 방법은 데이터 센터 또는 다른 클라우드 환경의 컴퓨터에 권장 되는 방법입니다.

* 단계별 절차에 따라 Azure가 아닌 VM에 Hybrid Runbook Worker 역할을 수동으로 설치 하 고 구성 합니다.

> [!NOTE]
> DSC (필요한 상태 구성)를 사용 하 여 Hybrid Runbook Worker 역할을 지 원하는 서버의 구성을 관리 하려면 DSC 노드로 서버를 추가 해야 합니다.

### <a name="minimum-requirements-for-windows-hybrid-runbook-worker"></a>Windows Hybrid Runbook Worker에 대 한 최소 요구 사항

Windows Hybrid Runbook Worker에 대한 최소 요구 사항은 다음과 같습니다.

* Windows Server 2012 이상
* Windows PowerShell 5.1 이상 ([WMF 5.1 다운로드](https://www.microsoft.com/download/details.aspx?id=54616))
* .NET Framework 4.6.2 이상
* 두 개의 코어
* 4GB의 RAM
* 443 포트(아웃바운드)

### <a name="network-configuration"></a>네트워크 구성

Hybrid Runbook Worker에 대한 상세한 네트워킹 요구 사항은 [네트워크 구성](automation-hybrid-runbook-worker.md#network-planning)을 참조하세요.

### <a name="server-onboarding-for-management-with-state-configuration-dsc"></a>상태 구성 (DSC)을 사용 하 여 관리를 위한 서버 온 보 딩

상태 구성 (DSC)을 사용한 관리를 위해 서버를 등록 하는 방법에 대 한 자세한 내용은 [dsc (상태 구성 별 관리를 위한 컴퓨터 등록)](automation-dsc-onboarding.md)를 참조 하세요.

[업데이트 관리](automation-update-management.md) 사용 하도록 설정 하면 Log Analytics 작업 영역에 연결 된 모든 Windows 컴퓨터가 Runbook 업데이트를 지원 하기 위한 Hybrid Runbook Worker으로 자동 구성 됩니다. 그러나이 작업자는 Automation 계정에 이미 정의 된 Hybrid Runbook Worker 그룹에 등록 되어 있지 않습니다.

### <a name="addition-of-the-computer-to-a-hybrid-runbook-worker-group"></a>Hybrid Runbook Worker 그룹에 컴퓨터 추가

Automation 계정의 Hybrid Runbook Worker 그룹에 작업자 컴퓨터를 추가할 수 있습니다. Azure Automation 기능 및 Hybrid Runbook Worker 그룹 멤버 자격에 동일한 계정을 사용 하는 한 Automation runbook을 지원 해야 합니다. 이 기능은 Hybrid Runbook Worker의 7.2.12024.0 버전에 추가되었습니다.

## <a name="automated-deployment"></a>자동화된 배포

대상 컴퓨터에서 다음 단계를 수행 하 여 Windows Hybrid Worker 역할의 설치와 구성을 자동화 합니다.

### <a name="step-1---download-the-powershell-script"></a>1 단계-PowerShell 스크립트 다운로드

[PowerShell 갤러리](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker)에서 **new-onpremisehybridworker.ps1** 스크립트를 다운로드 합니다. 다운로드는 Hybrid Runbook Worker 역할을 실행 하는 컴퓨터 또는 사용자 환경의 다른 컴퓨터에서 직접 만들어야 합니다. 스크립트를 다운로드 한 후에는 작업자에 복사 합니다. **New-onpremisehybridworker.ps1** 스크립트는 실행 하는 동안 아래에 설명 된 매개 변수를 사용 합니다.

| 매개 변수 | 상태 | 설명 |
| --------- | ------ | ----------- |
| `AAResourceGroupName` | 필수 | Automation 계정과 연결된 리소스 그룹의 이름입니다. |
| `AutomationAccountName` | 필수 | Automation 계정의 이름입니다.
| `Credential` | Optional | Azure 환경에 로그인 할 때 사용할 자격 증명입니다. |
| `HybridGroupName` | 필수 | 이 시나리오를 지원하는 Runbook에 대한 대상으로 지정할 Hybrid Runbook Worker 그룹의 이름입니다. |
| `OMSResourceGroupName` | Optional | Log Analytics 작업 영역에 대한 리소스 그룹의 이름입니다. 이 리소스 그룹을 지정 하지 않으면 값이 사용 `AAResourceGroupName` 됩니다. |
| `SubscriptionID` | 필수 | Automation 계정과 연결 된 Azure 구독의 식별자입니다. |
| `TenantID` | Optional | Automation 계정과 연결 된 테 넌 트 조직의 식별자입니다. |
| `WorkspaceName` | Optional | Log Analytics 작업 영역 이름입니다. Log Analytics 작업 영역이 없는 경우 스크립트에서 하나를 만들어 구성합니다. |

> [!NOTE]
> 기능을 사용 하도록 설정 하는 경우 Azure Automation은 Log Analytics 작업 영역 및 Automation 계정을 연결 하는 데 필요한 특정 영역만 지원 합니다. 지원 되는 매핑 쌍 목록은 [Automation 계정 및 Log Analytics 작업 영역에 대 한 지역 매핑](how-to/region-mappings.md)을 참조 하세요.

### <a name="step-2---open-windows-powershell-command-line-shell"></a>2 단계-Windows PowerShell 명령줄 셸 열기

관리자 모드의 **시작** 화면에서 **Windows PowerShell** 을 엽니다.

### <a name="step-3---run-the-powershell-script"></a>3 단계-PowerShell 스크립트 실행

PowerShell 명령줄 셸에서 다운로드 한 스크립트가 포함 된 폴더로 이동 합니다. ,, `AutomationAccountName`,, 및 `AAResourceGroupName` `OMSResourceGroupName` `HybridGroupName` `SubscriptionID` `WorkspaceName`매개 변수에 대 한 값을 변경 합니다. 그런 다음, 스크립트를 실행합니다.

스크립트를 실행한 후에 Azure 인증을 요청하는 메시지가 나타납니다. 구독 관리자 역할의 멤버이자 구독의 공동 관리자인 계정으로 로그인해야 합니다.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionID <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>4 단계-NuGet 설치

NuGet을 설치 하 고 Azure 자격 증명을 사용 하 여 인증 하는 것에 동의 하 라는 메시지가 표시 됩니다. 최신 NuGet 버전이 없으면 [사용 가능한 Nuget 배포 버전](https://www.nuget.org/downloads)에서 가져올 수 있습니다.

### <a name="step-5---verify-the-deployment"></a>5 단계-배포 확인

스크립트가 완료되면 Hybrid Worker 그룹 페이지에 새 그룹 및 멤버 수가 표시됩니다. 기존 그룹인 경우 멤버 수가 증가합니다. Hybrid Worker 그룹 페이지의 목록에서 그룹을 선택 하 고 **Hybrid worker** 타일을 선택할 수 있습니다. Hybrid Worker 페이지에서 나열 된 그룹의 각 멤버를 볼 수 있습니다.

## <a name="manual-deployment"></a>수동 배포

대상 컴퓨터에서 Automation 환경에 대해 처음 두 단계를 한 번 수행 합니다. 그런 다음 각 작업자 컴퓨터에 대해 나머지 단계를 수행 합니다.

### <a name="step-1---create-a-log-analytics-workspace"></a>1 단계-Log Analytics 작업 영역 만들기

Log Analytics 작업 영역이 아직 없는 경우 작업 영역을 만들기 전에 [Azure Monitor 로그 디자인 지침](../azure-monitor/platform/design-logs-deployment.md) 을 검토 합니다.

### <a name="step-2---add-an-azure-automation-feature-to-the-log-analytics-workspace"></a>2 단계-Log Analytics 작업 영역에 Azure Automation 기능 추가

자동화 기능은 Hybrid Runbook Worker 지원을 포함 하 여 Azure Automation에 대 한 기능을 추가 합니다. Log Analytics 작업 영역에 솔루션을 추가 하면 다음 단계에 설명 된 대로 설치 하는 작업자 구성 요소를 에이전트 컴퓨터에 자동으로 푸시합니다.

작업 영역에 Automation 솔루션을 추가 하려면 다음 PowerShell cmdlet을 실행 합니다.

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="step-3---install-the-log-analytics-agent-for-windows"></a>3 단계-Windows 용 Log Analytics 에이전트 설치

Windows 용 Log Analytics 에이전트는 Azure Monitor Log Analytics 작업 영역에 컴퓨터를 연결 합니다. 컴퓨터에 에이전트를 설치 하 고 작업 영역에 연결 하면 Hybrid Runbook Worker에 필요한 구성 요소가 자동으로 다운로드 됩니다.

컴퓨터에 에이전트를 설치 하려면 [Azure Monitor 로그에 Windows 컴퓨터 연결](../log-analytics/log-analytics-windows-agent.md)의 지침을 따르세요. 이 과정을 여러 컴퓨터에 반복하여 사용자의 환경에 여러 작업자를 추가합니다.

몇 분 후에 에이전트가 Log Analytics 작업 영역에 성공적으로 연결 되 면 다음 쿼리를 실행 하 여 작업 영역에 하트 비트 데이터를 보내고 있는지 확인할 수 있습니다.

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

검색 결과에는 컴퓨터에 대 한 하트 비트 레코드가 표시 되 고이 레코드가 서비스에 연결 되 고 보고 되 고 있음을 나타냅니다. 기본적으로 모든 에이전트는 하트 비트 레코드를 할당 된 작업 영역으로 전달 합니다. 

에이전트 설치 및 설정을 완료 하려면 다음 단계를 따르십시오.

1. 솔루션을 사용 하도록 설정 하 여 에이전트 컴퓨터를 등록 합니다. [작업 영역의 온보드 컴퓨터](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#onboard-machines-in-the-workspace)를 참조 하세요.
2. 에이전트가 자동화 솔루션을 올바르게 다운로드 했는지 확인 합니다. 
3. Hybrid Runbook Worker 버전을 확인 하려면 **C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation** 으로 이동 하 여 **버전** 하위 폴더를 확인 합니다.

### <a name="step-4---install-the-runbook-environment-and-connect-to-azure-automation"></a>4 단계-runbook 환경을 설치 하 고 Azure Automation에 연결

Log Analytics 작업 영역에 보고 하도록 에이전트를 구성 하는 경우 Automation 솔루션은 `HybridRegistration` `Add-HybridRunbookWorker` cmdlet을 포함 하는 PowerShell 모듈을 푸시합니다. 이 cmdlet을 사용 하 여 컴퓨터에 runbook 환경을 설치 하 고 Azure Automation에 등록 합니다.

관리자 모드에서 PowerShell 세션을 열고 다음 명령을 실행하여 모듈을 가져옵니다.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

이제 다음 구문을 `Add-HybridRunbookWorker` 사용 하 여 cmdlet을 실행 합니다.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Azure portal의 키 관리 페이지에서 이 cmdlet에 필요한 정보를 가져올 수 있습니다. Automation 계정의 설정 페이지에서 **키** 를 선택 하 여이 페이지를 엽니다.

![키 관리 페이지](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* `GroupName` 매개 변수의 경우 Hybrid Runbook Worker 그룹의 이름을 사용 합니다. 이 그룹이 자동화 계정에 이미 있으면 현재 컴퓨터가 추가되고, 그룹이 없으면 추가됩니다.
* 매개 변수의 경우 키 관리 페이지의 URL 항목을 사용 합니다. **URL** `EndPoint`
* `Token` 매개 변수의 경우 키 관리 페이지의 **기본 액세스 키** 항목을 사용 합니다.
* 필요한 경우 `Verbose` 매개 변수를 설정 하 여 설치에 대 한 세부 정보를 받습니다.

### <a name="step-5----install-powershell-modules"></a>5 단계-PowerShell 모듈 설치

Runbook은 Azure Automation 환경에 설치된 모듈에 정의된 활동 및 cmdlet을 사용할 수 있습니다. 이러한 모듈은 온-프레미스 컴퓨터에 자동으로 배포 되지 않으므로 수동으로 설치 해야 합니다. Azure 모듈은 예외입니다. 이 모듈은 기본적으로 설치 되며, Azure Automation에 대 한 모든 Azure 서비스 및 활동에 대 한 cmdlet에 대 한 액세스를 제공 합니다.

Hybrid Runbook Worker의 주요 목적은 로컬 리소스를 관리 하는 것 이므로 이러한 리소스를 지 원하는 모듈 (특히 `PowerShellGet` 모듈)을 설치 해야 합니다. Windows PowerShell 모듈을 설치 하는 방법에 대 한 자세한 내용은 [Windows powershell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell)을 참조 하세요.

설치 된 모듈은 `PSModulePath` 환경 변수에서 참조 하는 위치에 있어야 하이브리드 작업자에서 자동으로 가져올 수 있습니다. 자세한 내용은 [Install Modules In PSModulePath](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7)를 참조 하세요.

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-windows-computer"></a><a name="remove-windows-hybrid-runbook-worker"></a>온-프레미스 Windows 컴퓨터에서 Hybrid Runbook Worker 제거

1. Azure Portal에서 Automation 계정으로 이동합니다.
2. **계정 설정**에서 **키**를 선택하고 **URL** 및 **기본 액세스 키**의 값을 확인합니다.

3. 관리자 모드에서 PowerShell 세션을 열고 URL 및 기본 액세스 키 값을 사용 하 여 다음 명령을 실행 합니다. 제거 프로세스 `Verbose` 의 자세한 로그에는 매개 변수를 사용 합니다. Hybrid Worker 그룹에서 부실한 컴퓨터를 제거하려면 선택적 `machineName` 매개 변수를 사용합니다.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

## <a name="remove-a-hybrid-worker-group"></a>Hybrid Worker 그룹 제거

Hybrid Runbook Worker 그룹을 제거 하려면 먼저 그룹의 구성원 인 모든 컴퓨터에서 Hybrid Runbook Worker을 제거 해야 합니다. 그런 후 다음 단계를 사용 하 여 그룹을 제거 합니다.

1. Azure Portal에서 Automation 계정을 엽니다.
2. **프로세스 자동화**에서 **Hybrid worker 그룹** 을 선택 합니다. 삭제할 그룹을 선택합니다. 해당 그룹에 대한 속성 페이지가 표시됩니다.

   ![속성 페이지](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. 선택한 그룹에 대한 속성 페이지에서 **삭제**를 선택합니다. 이 작업의 확인을 요청하는 메시지가 나타납니다. 계속하려면 **예**를 선택합니다.

   ![확인 메시지](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   이 프로세스를 마치는 데 몇 초 정도 걸릴 수 있습니다. 메뉴의 **알림**에서 진행 상황을 추적할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* 온-프레미스 데이터 센터 또는 다른 클라우드 환경의 프로세스를 자동화하도록 Runbook을 구성하는 방법을 알아보려면 [Hybrid Runbook Worker에서 Runbook 실행](automation-hrw-run-runbooks.md)을 참조하세요.
* Hybrid Runbook Worker 문제를 해결 하는 방법에 대 한 자세한 내용은 [Windows Hybrid Runbook Worker 문제 해결](troubleshoot/hybrid-runbook-worker.md#windows)을 참조 하세요.

