---
title: 템플릿에서 배포 스크립트 사용 | Microsoft Docs
description: Azure Resource Manager 템플릿에서 배포 스크립트를 사용 합니다.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: jgao
ms.openlocfilehash: 5b938e2072daec56261e529ab8a2a8b15b55d143
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872326"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>템플릿에서 배포 스크립트 사용 (미리 보기)

Azure 리소스 템플릿에서 배포 스크립트를 사용 하는 방법에 대해 알아봅니다. 이라는 `Microsoft.Resources/deploymentScripts`새 리소스 유형을 사용 하면 사용자가 템플릿 배포에서 배포 스크립트를 실행 하 고 실행 결과를 검토할 수 있습니다. 이러한 스크립트는 다음과 같은 사용자 지정 단계를 수행 하는 데 사용할 수 있습니다.

- 디렉터리에 사용자 추가
- 데이터 평면 작업 (예: blob 복사 또는 시드 데이터베이스)을 수행 합니다.
- 라이선스 키 조회 및 유효성 검사
- 자체 서명된 인증서 만들기
- Azure AD에서 개체 만들기
- 사용자 지정 시스템에서 IP 주소 블록 조회

배포 스크립트의 이점은 다음과 같습니다.

- 쉽게 코딩 하 고, 사용 하 고, 디버그할 수 있습니다. 즐겨 사용 하는 개발 환경에서 배포 스크립트를 개발할 수 있습니다. 스크립트는 템플릿 또는 외부 스크립트 파일에 포함 될 수 있습니다.
- 스크립트 언어 및 플랫폼을 지정할 수 있습니다. 현재 Linux 환경에서 Azure PowerShell 및 Azure CLI 배포 스크립트가 지원 됩니다.
- 스크립트를 실행 하는 데 사용 되는 id를 지정할 수 있습니다. 현재 [Azure 사용자 할당 관리 id](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) 만 지원 됩니다.
- 스크립트에 명령줄 인수를 전달 하도록 허용 합니다.
- 는 스크립트 출력을 지정 하 고 배포에 다시 전달할 수 있습니다.

배포 스크립트 리소스는 Azure Container Instance를 사용할 수 있는 지역 에서만 사용할 수 있습니다.  [Azure 지역에서 Azure Container Instances에 대 한 리소스 가용성을](../../container-instances/container-instances-region-availability.md)참조 하세요.

> [!IMPORTANT]
> 스크립트를 실행 하 고 문제를 해결 하려면 저장소 계정 및 컨테이너 인스턴스가 필요 합니다. 기존 저장소 계정을 지정 하는 옵션이 있습니다. 그렇지 않으면 컨테이너 인스턴스와 함께 저장소 계정이 스크립트 서비스에 의해 자동으로 생성 됩니다. 자동으로 생성 된 두 리소스는 일반적으로 배포 스크립트 실행이 터미널 상태로 될 때 스크립트 서비스에 의해 삭제 됩니다. 리소스가 삭제될 때까지 해당 리소스에 대한 요금이 청구됩니다. 자세히 알아보려면 [배포 스크립트 리소스 정리](#clean-up-deployment-script-resources)를 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

- **대상 리소스 그룹에 대 한 참가자의 역할이 있는 사용자 할당 관리 id**입니다. 이 ID는 배포 스크립트를 실행하는 데 사용됩니다. 리소스 그룹 외부에서 작업을 수행 하려면 추가 권한을 부여 해야 합니다. 예를 들어 새 리소스 그룹을 만들려는 경우 구독 수준에 id를 할당 합니다.

  > [!NOTE]
  > 스크립트 서비스는 기존 저장소 계정을 지정 하지 않는 한 저장소 계정을 만들고 백그라운드에서 컨테이너 인스턴스를 만듭니다.  구독에서 Azure storage 계정 (ContainerInstance) 및 Azure container instance () 리소스 공급자를 등록 하지 않은 경우 구독 수준에서 참가자 역할이 있는 사용자 할당 관리 id가 필요 합니다.

  Id를 만들려면 Azure Portal를 사용 하거나 [Azure CLI](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)를 사용 하거나 [Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)를 사용 하 여 [사용자 할당 관리 id 만들기](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)를 참조 하세요. 이 ID는 템플릿을 배포할 때 필요합니다. ID의 형식은 다음과 같습니다.

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  다음 CLI 또는 PowerShell 스크립트를 사용 하 여 리소스 그룹 이름 및 id 이름을 제공 하 여 ID를 가져옵니다.

  # <a name="cli"></a>[CLI](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g jgaoidentity1008rg -n jgaouami --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell** 또는 **Azure CLI**. 지원 되는 Azure PowerShell 버전 목록을 보려면 [여기](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)를 참조 하세요. 지원 되는 Azure CLI 버전 목록을 보려면 [여기](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)를 참조 하세요.

    >[!IMPORTANT]
    > 배포 스크립트는 MCR (Microsoft Container Registry)에서 사용 가능한 CLI 이미지를 사용 합니다. 배포 스크립트의 CLI 이미지를 인증 하는 데 한 달이 소요 됩니다. 30 일 이내에 릴리스된 CLI 버전을 사용 하지 마세요. 이미지의 릴리스 날짜를 확인 하려면 [Azure CLI 릴리스 정보](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest)를 참조 하세요. 지원 되지 않는 버전을 사용 하는 경우 오류 메시지에 지원 되는 버전이 나열 됩니다.

    템플릿을 배포 하는 데 이러한 버전이 필요 하지 않습니다. 그러나 이러한 버전은 배포 스크립트를 로컬로 테스트 하는 데 필요 합니다. [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요. 미리 구성 된 Docker 이미지를 사용할 수 있습니다.  [개발 환경 구성](#configure-development-environment)을 참조 하세요.

## <a name="sample-templates"></a>샘플 템플릿

다음 json은 예입니다.  최신 템플릿 스키마는 [여기](/azure/templates/microsoft.resources/deploymentscripts)에서 찾을 수 있습니다.

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "runPowerShellInline",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell", // or "AzureCLI"
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "containerSettings": {
      "containerGroupName": "mycustomaci"
    },
    "storageAccountSettings": {
      "storageAccountName": "myStorageAccount",
      "storageAccountKey": "myKey"
    },
    "azPowerShellVersion": "3.0",  // or "azCliVersion": "2.0.80"
    "arguments": "[concat('-name ', parameters('name'))]",
    "environmentVariables": [
      {
        "name": "someSecret",
        "secureValue": "if this is really a secret, don't put it here... in plain text..."
      }
    ],
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ", // or "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> 예제는 데모용입니다.  **Scriptcontent** 및 **primary를** 템플릿에서 공존할 수 없습니다.

속성 값 정보:

- **Id**: 배포 스크립트 서비스는 사용자 할당 관리 id를 사용 하 여 스크립트를 실행 합니다. 현재 사용자 할당 관리 id만 지원 됩니다.
- **kind**: 스크립트 유형을 지정합니다. 현재 Azure PowerShell 및 Azure CLI 스크립트는 지원 됩니다. 값은 **Azurepowershell** 및 **azurepowershell**입니다.
- **Forceupdatetag**: 템플릿 배포 간에이 값을 변경 하면 배포 스크립트가 강제로 다시 실행 됩니다. 매개 변수의 defaultValue로 설정 해야 하는 newGuid () 또는 utcNow () 함수를 사용 합니다. 자세한 내용은 [스크립트를 두 번 이상 실행](#run-script-more-than-once)을 참조하세요.
- **containerSettings**: Azure Container Instance를 사용자 지정 하는 설정을 지정 합니다.  **containerGroupName** 는 컨테이너 그룹 이름을 지정 하는 데 사용할 수 있습니다.  지정 하지 않으면 그룹 이름이 자동으로 생성 됩니다.
- **Storageaccountsettings**: 기존 저장소 계정을 사용 하는 설정을 지정 합니다. 지정 하지 않으면 저장소 계정이 자동으로 만들어집니다. [기존 저장소 계정 사용](#use-an-existing-storage-account)을 참조 하세요.
- **azPowerShellVersion**/**azCliVersion**: 사용할 모듈 버전을 지정 합니다. 지원 되는 PowerShell 및 CLI 버전 목록은 [필수 구성 요소](#prerequisites)를 참조 하세요.
- **arguments**: 매개 변수의 값을 지정합니다. 값은 공백으로 구분됩니다.
- 환경 **변수**: 스크립트에 전달할 환경 변수를 지정 합니다. 자세한 내용은 [배포 스크립트 개발](#develop-deployment-scripts)을 참조 하세요.
- **scriptContent**: 스크립트 콘텐츠를 지정합니다. 외부 스크립트를 실행 하려면 대신을 `primaryScriptUri` 사용 합니다. 예제는 [인라인 스크립트 사용](#use-inline-scripts) 및 [외부 스크립트 사용](#use-external-scripts)을 참조 하세요.
- **primaryScriptUri**: 지원 되는 파일 확장명을 사용 하 여 기본 배포 스크립트에 공개적으로 액세스할 수 있는 Url을 지정 합니다.
- **Supporting스크립트가**있는: `ScriptContent` 또는 `PrimaryScriptUri`에서 호출 된 지원 파일에 공개적으로 액세스할 수 있는 url 배열을 지정 합니다.
- **timeout**: [ISO 8601 형식](https://en.wikipedia.org/wiki/ISO_8601)에 지정된 최대 허용 스크립트 실행 시간을 지정합니다. 기본값은 **P1D**입니다.
- **Cleanuppreference 설정**입니다. 스크립트 실행이 터미널 상태가 될 때 배포 리소스 정리의 기본 설정을 지정 합니다. 기본 설정은 **항상**입니다. 즉, 터미널 상태 (성공, 실패, 취소 됨)에도 불구 하 고 리소스를 삭제 합니다. 자세한 내용은 [배포 스크립트 리소스 정리](#clean-up-deployment-script-resources)를 참조하세요.
- **보존 기간**: 배포 스크립트 실행이 터미널 상태에 도달한 후 서비스에서 배포 스크립트 리소스를 유지 하는 간격을 지정 합니다. 이 기간이 만료 되 면 배포 스크립트 리소스가 삭제 됩니다. 기간은 [ISO 8601 패턴](https://en.wikipedia.org/wiki/ISO_8601)을 기반으로 합니다. 기본값은 7 일을 의미 하는 **P1D**입니다. 이 속성은 cleanupPreference가 *OnExpiration*으로 설정된 경우에 사용됩니다. *Onexpiration* 속성이 현재 활성화 되어 있지 않습니다. 자세한 내용은 [배포 스크립트 리소스 정리](#clean-up-deployment-script-resources)를 참조하세요.

### <a name="additional-samples"></a>추가 샘플

- [키 자격 증명 모음에 인증서 만들기 및 할당](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [사용자 할당 관리 id를 만들어 리소스 그룹에 할당 하 고 배포 스크립트를 실행](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json)합니다.

> [!NOTE]
> 사용자 할당 id를 만들고 사용 권한을 미리 부여 하는 것이 좋습니다. 배포 스크립트를 실행 하는 동일한 템플릿에서 id를 만들고 사용 권한을 부여 하는 경우 로그인 및 사용 권한 관련 오류가 발생할 수 있습니다. 사용 권한이 적용 되려면 약간의 시간이 걸립니다.

## <a name="use-inline-scripts"></a>인라인 스크립트 사용

다음 템플릿에는 `Microsoft.Resources/deploymentScripts` 형식을 사용 하 여 정의 된 리소스가 하나 있습니다. 강조 표시 된 부분은 인라인 스크립트입니다.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> 인라인 배포 스크립트는 큰따옴표로 묶여 있으므로 배포 스크립트 내의 문자열을 작은따옴표로 묶어야 합니다. PowerShell의 이스케이프 문자는 **&#92;** 입니다. 이전 JSON 샘플에 표시 된 대로 문자열 대체를 사용 하는 것을 고려할 수도 있습니다. Name 매개 변수의 기본값을 참조 하세요.

스크립트는 하나의 매개 변수를 사용 하 고 매개 변수 값을 출력 합니다. **Deploymentscriptoutputs** 는 출력을 저장 하는 데 사용 됩니다.  출력 섹션에서 **값** 줄은 저장 된 값에 액세스 하는 방법을 보여 줍니다. `Write-Output`는 디버깅 목적으로 사용 됩니다. 출력 파일에 액세스 하는 방법에 대 한 자세한 내용은 [배포 스크립트 디버그](#debug-deployment-scripts)를 참조 하세요.  속성 설명은 [샘플 템플릿](#sample-templates)을 참조 하세요.

스크립트를 실행 하려면 **시도** 를 선택 하 여 Cloud Shell 연 후 다음 코드를 셸 창에 붙여넣습니다.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

출력은 다음과 같이 표시됩니다.

![리소스 관리자 템플릿 배포 스크립트 hello 세계 출력](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>외부 스크립트 사용

인라인 스크립트 외에도 외부 스크립트 파일을 사용할 수 있습니다. 파일 확장명이 **ps1** 인 기본 PowerShell 스크립트만 지원 됩니다. CLI 스크립트의 경우 스크립트는 유효한 bash 스크립트 이면 기본 스크립트에 확장을 포함 하거나 확장을 사용 하지 않을 수 있습니다. 외부 스크립트 파일을 사용 하려면를 `scriptContent` 로 `primaryScriptUri`바꿉니다. 예를 들어:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

예제를 보려면 [여기](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json)를 선택 합니다.

외부 스크립트 파일에 액세스할 수 있어야 합니다.  Azure storage 계정에 저장 된 스크립트 파일을 보호 하려면 [SAS 토큰을 사용 하 여 개인 ARM 템플릿 배포](./secure-template-with-sas-token.md)를 참조 하세요.

배포 스크립트에서 참조 하는 스크립트의 무결성을 보장 하는 것은 **PrimaryScriptUri** 또는 **supporting스크립트**를 지 원하는 것입니다.  신뢰 하는 스크립트만 참조 합니다.

## <a name="use-supporting-scripts"></a>지원 스크립트 사용

복잡 한 논리를 하나 이상의 지원 스크립트 파일로 분리할 수 있습니다. `supportingScriptURI` 속성을 사용 하면 필요한 경우 지원 스크립트 파일에 uri 배열을 제공할 수 있습니다.

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

지원 스크립트 파일은 인라인 스크립트와 기본 스크립트 파일 모두에서 호출 될 수 있습니다. 지원 스크립트 파일에는 파일 확장명에 대 한 제한이 없습니다.

지원 파일은 런타임에 azscripts/azscriptinput에 복사 됩니다. 인라인 스크립트 및 기본 스크립트 파일에서 지원 파일을 참조 하려면 상대 경로를 사용 합니다.

## <a name="work-with-outputs-from-powershell-script"></a>PowerShell 스크립트에서 출력 작업

다음 템플릿에서는 두 deploymentScripts 리소스 간에 값을 전달 하는 방법을 보여 줍니다.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

첫 번째 리소스에서는 **$DeploymentScriptOutputs**라는 변수를 정의 하 고이 변수를 사용 하 여 출력 값을 저장 합니다. 템플릿 내의 다른 리소스에서 출력 값에 액세스 하려면 다음을 사용 합니다.

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>CLI 스크립트에서 출력 작업

PowerShell 배포 스크립트와는 달리 CLI/bash 지원에서는 스크립트 출력을 저장 하는 공통 변수를 노출 하지 않고 스크립트 출력 파일이 있는 위치를 저장 하는 **AZ_SCRIPTS_OUTPUT_PATH** 라는 환경 변수가 있습니다. 리소스 관리자 템플릿에서 배포 스크립트를 실행 하는 경우이 환경 변수는 Bash 셸에서 자동으로 설정 됩니다.

배포 스크립트 출력은 AZ_SCRIPTS_OUTPUT_PATH 위치에 저장 해야 하며 출력은 유효한 JSON 문자열 개체 여야 합니다. 파일의 내용을 키-값 쌍으로 저장 해야 합니다. 예를 들어 문자열의 배열은 {"MyResult": ["foo", "bar"]}로 저장 됩니다.  예를 들어, ["foo", "bar"]와 같은 배열 결과만 저장 하는 것은 유효 하지 않습니다.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

[jq](https://stedolan.github.io/jq/) 는 이전 샘플에서 사용 됩니다. 컨테이너 이미지와 함께 제공 됩니다. [개발 환경 구성](#configure-development-environment)을 참조 하세요.

## <a name="develop-deployment-scripts"></a>배포 스크립트 개발

### <a name="handle-non-terminating-errors"></a>종료 되지 않는 오류 처리

배포 스크립트에서 [**$ErrorActionPreference**](/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7#erroractionpreference
) 변수를 사용 하 여 PowerShell이 종료 되지 않는 오류에 응답 하는 방식을 제어할 수 있습니다. 스크립트 서비스에서 값을 설정/변경 하지 않습니다.  $ErrorActionPreference에 대해 설정한 값에도 불구 하 고, 스크립트에서 오류가 발생할 경우 배포 스크립트에서 리소스 프로 비전 상태를 *Failed* 로 설정 합니다.

### <a name="pass-secured-strings-to-deployment-script"></a>배포 스크립트에 보안 문자열 전달

컨테이너 인스턴스에서 환경 변수 (고 environmentvariable)를 설정 하면 컨테이너에서 실행 되는 응용 프로그램 또는 스크립트의 동적 구성을 제공할 수 있습니다. 배포 스크립트는 Azure Container Instance와 동일한 방식으로 안전 하지 않은 보안 환경 변수를 처리 합니다. 자세한 내용은 [container instances에서 환경 변수 설정](../../container-instances/container-instances-environment-variables.md#secure-values)을 참조 하세요.

## <a name="debug-deployment-scripts"></a>배포 스크립트 디버그

스크립트 서비스는 기존 저장소 계정을 지정 하지 않는 한 [저장소 계정을](../../storage/common/storage-account-overview.md) 만들고 스크립트 실행을 위한 [컨테이너 인스턴스](../../container-instances/container-instances-overview.md) 를 만듭니다. 이러한 리소스는 스크립트 서비스에서 자동으로 생성 되는 경우 리소스 이름에 **azscripts** 접미사가 있습니다.

![리소스 관리자 템플릿 배포 스크립트 리소스 이름](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

사용자 스크립트, 실행 결과 및 stdout 파일은 저장소 계정의 파일 공유에 저장 됩니다. **Azscripts**라는 폴더가 있습니다. 폴더에는 입력 및 출력 파일에 대 한 두 개의 폴더 ( **azscriptinput** 및 **azscriptoutput**)가 있습니다.

출력 폴더에는 **executionresult.json** 및 스크립트 출력 파일이 포함되어 있습니다. **Executionresult. json**에서 스크립트 실행 오류 메시지를 볼 수 있습니다. 출력 파일은 스크립트가 성공적으로 실행 되는 경우에만 생성 됩니다. 입력 폴더에는 시스템 PowerShell 스크립트 파일과 사용자 배포 스크립트 파일이 포함되어 있습니다. 사용자 배포 스크립트 파일을 수정 된 것으로 바꾸고 Azure container instance에서 배포 스크립트를 다시 실행할 수 있습니다.

REST API를 사용 하 여 리소스 그룹 수준 및 구독 수준에서 배포 스크립트 리소스 배포 정보를 가져올 수 있습니다.

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

다음 예제에서는 [ARMClient](https://github.com/projectkudu/ARMClient)를 사용 합니다.

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

다음과 유사하게 출력됩니다.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

출력은 배포 상태와 배포 스크립트 리소스 Id를 표시 합니다.

다음 REST API은 로그를 반환 합니다.

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

배포 스크립트 리소스를 삭제 하기 전에만 작동 합니다.

포털에서 deploymentScripts 리소스를 보려면 **숨겨진 유형 표시**를 선택 합니다.

![리소스 관리자 템플릿 배포 스크립트, 숨겨진 유형 표시, 포털](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="use-an-existing-storage-account"></a>기존 스토리지 계정 사용

스크립트를 실행 하 고 문제를 해결 하려면 저장소 계정 및 컨테이너 인스턴스가 필요 합니다. 기존 저장소 계정을 지정 하는 옵션이 있습니다. 그렇지 않으면 컨테이너 인스턴스와 함께 저장소 계정이 스크립트 서비스에 의해 자동으로 생성 됩니다. 기존 저장소 계정을 사용 하기 위한 요구 사항:

- 지원 되는 저장소 계정 종류는 범용 v2, 범용 v1 및 FileStorage 계정입니다. FileStorage만 프리미엄 SKU를 지원 합니다. 자세한 내용은 [저장소 계정 유형](../../storage/common/storage-account-overview.md)을 참조 하세요.
- 저장소 계정 방화벽 규칙은 아직 지원 되지 않습니다. 자세한 내용은 [Azure Storage 방화벽 및 가상 네트워크 구성](../../storage/common/storage-network-security.md)을 참조하세요.
- 배포 스크립트의 사용자 할당 관리 id에는 저장소 계정을 관리할 수 있는 권한이 있어야 합니다. 여기에는 파일 공유 읽기, 만들기, 삭제 등이 포함 됩니다.

기존 저장소 계정을 지정 하려면의 `Microsoft.Resources/deploymentScripts`property 요소에 다음 json을 추가 합니다.

```json
"storageAccountSettings": {
  "storageAccountName": "myStorageAccount",
  "storageAccountKey": "myKey"
},
```

- **Storageaccountname**: 저장소 계정의 이름을 지정 합니다.
- **storageAccountKey "**: 저장소 계정 키 중 하나를 지정 합니다. 함수를 [`listKeys()`](./template-functions-resource.md#listkeys) 사용 하 여 키를 검색할 수 있습니다. 예를 들어:

    ```json
    "storageAccountSettings": {
        "storageAccountName": "[variables('storageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
    }
    ```

전체 `Microsoft.Resources/deploymentScripts` 정의 샘플은 [샘플 템플릿](#sample-templates) 을 참조 하세요.

기존 저장소 계정이 사용 되는 경우 스크립트 서비스는 고유한 이름의 파일 공유를 만듭니다. 스크립트 서비스에서 파일 공유를 정리 하는 방법은 [배포 스크립트 리소스 정리](#clean-up-deployment-script-resources) 를 참조 하세요.

## <a name="clean-up-deployment-script-resources"></a>배포 스크립트 리소스 정리

스크립트를 실행 하 고 문제를 해결 하려면 저장소 계정 및 컨테이너 인스턴스가 필요 합니다. 기존 저장소 계정을 지정 하는 옵션이 있습니다. 그렇지 않으면 컨테이너 인스턴스와 함께 저장소 계정이 스크립트 서비스에 의해 자동으로 생성 됩니다. 배포 스크립트 실행이 터미널 상태 이면 스크립트 서비스에서 자동으로 생성 된 두 리소스를 삭제 합니다. 리소스가 삭제될 때까지 해당 리소스에 대한 요금이 청구됩니다. 가격 정보는 [Container Instances 가격 책정](https://azure.microsoft.com/pricing/details/container-instances/) 및 [Azure Storage 가격 책정](https://azure.microsoft.com/pricing/details/storage/)을 참조 하세요.

이러한 리소스의 수명 주기는 템플릿의 다음 속성에 의해 제어 됩니다.

- **Cleanuppreference**설정: 스크립트 실행이 터미널 상태가 될 때의 우선 순위를 정리 합니다. 지원되는 값은

  - **항상**: 스크립트 실행이 터미널 상태에서 가져온 후 자동으로 만들어진 리소스를 삭제 합니다. 기존 저장소 계정이 사용 되는 경우 스크립트 서비스는 저장소 계정에 생성 된 파일 공유를 삭제 합니다. 리소스를 정리 하 고 나면 deploymentScripts 리소스가 계속 존재할 수 있기 때문에 스크립트 서비스는 리소스를 삭제 하기 전에 스크립트 실행 결과 (예: stdout, output, return value 등)를 유지 합니다.
  - **Onsuccess**: 스크립트가 성공적으로 실행 된 경우에만 자동으로 생성 된 리소스를 삭제 합니다. 기존 저장소 계정이 사용 되는 경우 스크립트 서비스는 스크립트 실행에 성공한 경우에만 파일 공유를 제거 합니다. 리소스에 계속 액세스 하 여 디버그 정보를 찾을 수 있습니다.
  - **Onexpiration**: **보존 기간** 설정이 만료 된 경우에만 자동으로 리소스를 삭제 합니다. 기존 저장소 계정이 사용 되는 경우 스크립트 서비스는 파일 공유를 제거 하지만 저장소 계정은 유지 합니다.

- 보존 **기간**: 스크립트 리소스가 유지 되는 시간 간격을 지정 합니다 .이 시간 후에 만료 되 고 삭제 됩니다.

> [!NOTE]
> 다른 용도로는 스크립트 서비스에 의해 생성 되는 저장소 계정 및 컨테이너 인스턴스도 사용 하지 않는 것이 좋습니다. 스크립트 수명 주기에 따라 두 리소스가 제거 될 수 있습니다.

## <a name="run-script-more-than-once"></a>스크립트를 두 번 이상 실행

배포 스크립트 실행은 idempotent 작업입니다. 모든 deploymentScripts 리소스 속성 (인라인 스크립트 포함)이 변경 되지 않은 경우 템플릿을 다시 배포할 때 스크립트가 실행 되지 않습니다. 배포 스크립트 서비스는 템플릿의 리소스 이름을 같은 리소스 그룹에 있는 기존 리소스와 비교 합니다. 동일한 배포 스크립트를 여러 번 실행 하려는 경우 두 가지 옵션이 있습니다.

- DeploymentScripts 리소스의 이름을 변경 합니다. 예를 들어 리소스 이름 또는 리소스 이름의 일부로 [utcNow](./template-functions-date.md#utcnow) 템플릿 함수를 사용 합니다. 리소스 이름을 변경 하면 새 deploymentScripts 리소스가 생성 됩니다. 스크립트 실행 기록을 유지 하는 것이 좋습니다.

    > [!NOTE]
    > UtcNow 함수는 매개 변수의 기본값에만 사용할 수 있습니다.

- `forceUpdateTag` 템플릿 속성에 다른 값을 지정 합니다.  예를 들어 utcNow를 값으로 사용 합니다.

> [!NOTE]
> Idempotent 배포 스크립트를 작성 합니다. 이렇게 하면 실수로 다시 실행 되는 경우 시스템 변경이 발생 하지 않습니다. 예를 들어 배포 스크립트를 사용 하 여 Azure 리소스를 만드는 경우 리소스를 만들기 전에 존재 하지 않는지 확인 합니다. 그러면 스크립트가 성공 하거나 리소스를 다시 만들지 않습니다.

## <a name="configure-development-environment"></a>개발 환경 구성

배포 스크립트 개발 환경으로 미리 구성 된 docker 컨테이너 이미지를 사용할 수 있습니다. 다음 절차에서는 Windows에서 docker 이미지를 구성 하는 방법을 보여 줍니다. Linux 및 Mac의 경우 인터넷에서 정보를 찾을 수 있습니다.

1. 개발 컴퓨터에 [Docker Desktop](https://www.docker.com/products/docker-desktop) 을 설치 합니다.
1. Docker Desktop을 엽니다.
1. 표시줄에서 Docker 바탕 화면 아이콘을 선택 하 고 **설정**을 선택 합니다.
1. **공유 드라이브**를 선택 하 고 컨테이너에 사용할 수 있는 로컬 드라이브를 선택한 다음 **적용** 을 선택 합니다.

    ![리소스 관리자 템플릿 배포 스크립트 docker 드라이브](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. 프롬프트에 windows 자격 증명을 입력 합니다.
1. 명령 프롬프트 또는 Windows PowerShell (PowerShell ISE 사용 안 함) 중 하나를 사용 하 여 터미널 창을 엽니다.
1. 배포 스크립트 컨테이너 이미지를 로컬 컴퓨터로 끌어옵니다.

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    이 예제에서는 버전 PowerShell 합니다을 사용 합니다.

    MCR (Microsoft Container Registry)에서 CLI 이미지를 꺼내려면:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    이 예제에서는 버전 CLI 2.0.80를 사용 합니다. 배포 스크립트는 [여기](https://hub.docker.com/_/microsoft-azure-cli)에 있는 기본 CLI 컨테이너 이미지를 사용 합니다.

1. Docker 이미지를 로컬로 실행 합니다.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    ** &lt;호스트 드라이버 문자>** 및 ** &lt;호스트 디렉터리 이름>** 를 공유 드라이브의 기존 폴더로 바꿉니다.  컨테이너의 **/data** 폴더에 폴더를 매핑합니다. 예를 들어 D:\docker를 매핑하려면 다음을 수행 합니다.

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-** 컨테이너 이미지를 활성 상태로 유지 하는 것을 의미 합니다.

    CLI 예제:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. 메시지가 표시 되 면 **공유** 를 선택 합니다.
1. 다음 스크린샷에서는 d:\docker 폴더에 helloworld 파일이 있는 경우 PowerShell 스크립트를 실행 하는 방법을 보여 줍니다.

    ![리소스 관리자 템플릿 배포 스크립트 docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

스크립트를 성공적으로 테스트 한 후에는이 스크립트를 배포 스크립트로 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 배포 스크립트를 사용 하는 방법을 알아보았습니다. 배포 스크립트 자습서를 진행 하려면 다음을 수행 합니다.

> [!div class="nextstepaction"]
> [자습서: Azure Resource Manager 템플릿에서 배포 스크립트 사용](./template-tutorial-deployment-script.md)