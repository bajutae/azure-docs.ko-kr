---
title: Azure Automation Linux Hybrid Runbook Worker
description: 이 문서에서는 로컬 데이터 센터 또는 클라우드 환경의 Linux 기반 컴퓨터에서 Runbook을 실행할 수 있도록 Azure Automation Hybrid Runbook Worker를 설치하는 방법에 대한 정보를 제공합니다.
services: automation
ms.subservice: process-automation
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: 82f6d9e56e5d5745077ef512cb3392c16b95961f
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872175"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Linux Hybrid Runbook Worker 배포

Azure Automation의 Hybrid Runbook Worker 기능을 사용하여 역할을 호스팅하는 컴퓨터에서 직접 그리고 환경의 리소스에 대해 Runbook을 실행하여 해당 로컬 리소스를 관리할 수 있습니다. Linux Hybrid Runbook Worker는 Runbook을 승격이 필요한 명령을 실행하도록 승격할 수 있는 특수 사용자로 실행합니다. Runbook은 Azure Automation에 저장 및 관리된 후 하나 이상의 지정된 컴퓨터에 전달됩니다.

이 문서에서는 Linux 컴퓨터에 Hybrid Runbook Worker을 설치 하는 방법, 작업자를 제거 하는 방법 및 Hybrid Runbook Worker 그룹을 제거 하는 방법을 설명 합니다.

## <a name="supported-linux-operating-systems"></a>지원되는 Linux 운영 체제

Hybrid Runbook Worker 기능은 다음 배포를 지원합니다.

* Amazon Linux 2012.09 ~ 2015.09(x86/x64)
* CentOS Linux 5, 6 및 7(x86/x64)
* Oracle Linux 5, 6 및 7(x86/x64)
* Red Hat Enterprise Linux Server 5, 6 및 7(x86/x64)
* Debian GNU/Linux 6, 7, 8(x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS 및 18.04 (x86/x64)
* SUSE Linux Enterprise Server 11 및 12(x86/x64)

## <a name="supported-runbook-types"></a>지원되는 Runbook 유형

Linux Hybrid Runbook Worker는 Azure Automation에서 Runbook 유형의 전체 집합을 지원 하지 않습니다.

다음의 Runbook 유형은 Linux Hybrid Worker에서 작동합니다.

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell Runbook은 Linux 컴퓨터에 PowerShell Core를 설치해야 합니다. 설치하는 방법을 알아보려면 [Linux에 PowerShell Core 설치](/powershell/scripting/install/installing-powershell-core-on-linux)를 참조하세요.

다음의 Runbook 유형은 Linux Hybrid Worker에서 작동하지 않습니다.

* PowerShell 워크플로
* 그래픽
* 그래픽 PowerShell 워크플로

## <a name="deployment-requirements"></a>배포 요구 사항

Linux Hybrid Runbook Worker에 대한 최소 요구 사항은 다음과 같습니다.

* 두 개의 코어
* 4GB의 RAM
* 443 포트(아웃바운드)

### <a name="package-requirements"></a>패키지 요구 사항

| **필수 패키지** | **설명** | **최소 버전**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C 라이브러리| 2.5-12 |
|Openssl| OpenSSL 라이브러리 | 1.0 (TLS 1.1 및 TLS 1.2 지원 됨)|
|Curl | cURL 웹 클라이언트 | 7.15.5|
|Python-ctypes | Python 2.x가 필요 합니다. |
|PAM | 플러그형 인증 모듈|
| **선택적 패키지** | **설명** | **최소 버전**|
| PowerShell Core | PowerShell Runbook을 실행하려면 PowerShell을 설치해야 합니다. 설치하는 방법에 대해 알아보려면 [Linux에 PowerShell Core 설치](/powershell/scripting/install/installing-powershell-core-on-linux)를 참조하세요.  | 6.0.0 |

## <a name="install-a-linux-hybrid-runbook-worker"></a>Linux Hybrid Runbook Worker 설치

Linux 컴퓨터에 Hybrid Runbook Worker를 설치 하 고 구성 하려면 간단한 수동 프로세스를 수행 합니다. Azure Log Analytics 작업 영역에서 Automation Hybrid Worker 솔루션을 활성화한 다음, 일련의 명령을 실행하여 컴퓨터를 작업자로 등록하고 그룹에 추가해야 합니다.

계속 진행하기 전에 Automation 계정이 연결된 Log Analytics 작업 영역을 기록해 둡니다. 또한 Automation 계정에 대한 기본 키를 기록해 둡니다. 작업 영역과 기본 키는 모두 Azure Portal에서 찾을 수 있습니다. 자동화 계정을 선택하고 **작업 영역**을 선택하면 작업 영역 ID를, **키**를 선택하면 기본 키를 확인할 수 있습니다. Hybrid Runbook Worker에 필요한 포트 및 주소 정보는 [네트워크 구성](automation-hybrid-runbook-worker.md#network-planning)을 참조하세요.

>[!NOTE]
> Linux Hybrid Worker 설치 하는 동안 해당 sudo 권한이 있는 [nxautomation 계정이](automation-runbook-execution.md#log-analytics-agent-for-linux) 있어야 합니다. 작업자를 설치 하려고 시도 했지만 해당 계정이 없거나 적절 한 권한이 없는 경우 설치에 실패 합니다.

1. 다음 방법 중 하나를 사용하여 Azure에서 Automation Hybrid Worker 솔루션을 활성화합니다.

   * [작업 영역에 Azure Monitor 로그 솔루션 추가](../log-analytics/log-analytics-add-solutions.md)의 절차를 사용 하 여 구독에 Automation Hybrid Worker 솔루션을 추가 합니다.
   * 다음 cmdlet을 실행합니다.

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. 다음 명령을 실행하여 Linux용 Log Analytics 에이전트를 설치합니다. \<WorkspaceID\> 및 \<WorkspaceKey\>를 작업 영역에서 적절한 값으로 바꿉니다.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. *-w*, *-k*, *-g* 및 *-e* 매개 변수의 값을 변경하여 다음 명령을 실행합니다. *-G* 매개 변수의 경우이 값을 새 Linux Hybrid Runbook Worker 가입할 Hybrid Runbook Worker 그룹의 이름으로 바꿉니다. 해당 이름이 Automation 계정에 없는 경우 해당 이름의 Hybrid Runbook Worker 그룹이 새로 만들어집니다.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. 명령이 완료되면 Azure Portal의 Hybrid Worker 그룹 페이지는 새 그룹 및 멤버 수를 표시합니다. 기존 그룹인 경우 멤버 수가 증가합니다. Hybrid Worker 그룹 페이지의 목록에서 그룹을 선택하고 **Hybrid Worker** 타일을 선택합니다. Hybrid Worker 페이지에서 나열된 그룹의 각 멤버를 확인합니다.

> [!NOTE]
> Azure VM에 대 한 Linux 용 Azure Monitor 가상 머신 확장을 사용 하는 경우 자동 업그레이드 `autoUpgradeMinorVersion` 버전에서 Hybrid Runbook Worker 문제를 일으킬 수 있으므로 false로 설정 하는 것이 좋습니다. 확장을 수동으로 업그레이드 하는 방법을 알아보려면 [Azure CLI 배포](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment)를 참조 하세요.

## <a name="turn-off-signature-validation"></a>서명 유효성 검사 해제

기본적으로 Linux Hybrid Runbook Worker는 서명 유효성 검사를 요구합니다. 작업자에 대해 서명 되지 않은 runbook을 실행 하는 경우 `Signature validation failed` 오류가 표시 됩니다. 서명 유효성 검사를 해제하려면 다음 명령을 실행합니다. 두 번째 매개 변수를 Log Analytics 작업 영역 ID로 바꿉니다.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-linux-computer"></a><a name="remove-linux-hybrid-runbook-worker"></a>온-프레미스 Linux 컴퓨터에서 Hybrid Runbook Worker 제거

Hybrid Runbook Worker에서 명령을 `ls /var/opt/microsoft/omsagent` 사용 하 여 작업 영역 ID를 가져올 수 있습니다. 작업 영역 ID를 사용 하 여 이름이 지정 된 폴더가 만들어집니다.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> 이 코드는 컴퓨터에서 Linux 용 Log Analytics 에이전트를 제거 하지 않습니다. Hybrid Runbook Worker 역할의 기능 및 구성만 제거 합니다.

## <a name="remove-a-hybrid-worker-group"></a>Hybrid Worker 그룹 제거

Linux 컴퓨터의 Hybrid Runbook Worker 그룹을 제거 하려면 Windows Hybrid Worker 그룹의 경우와 동일한 단계를 사용 합니다. [Hybrid Worker 그룹 제거를](automation-windows-hrw-install.md#remove-a-hybrid-worker-group)참조 하세요.

## <a name="next-steps"></a>다음 단계

* 온-프레미스 데이터 센터 또는 다른 클라우드 환경의 프로세스를 자동화하도록 Runbook을 구성하는 방법을 알아보려면 [Hybrid Runbook Worker에서 Runbook 실행](automation-hrw-run-runbooks.md)을 참조하세요.
* Hybrid Runbook Worker 문제를 해결하는 방법을 알아보려면 [Linux Hybrid Runbook Worker 문제 해결](troubleshoot/hybrid-runbook-worker.md#linux)을 참조하세요.
