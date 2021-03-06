---
title: Azure Resource Manager 템플릿을 사용 하 여 업데이트 관리 등록 | Microsoft Docs
description: Azure Resource Manager 템플릿을 사용 하 여 Azure Automation 업데이트 관리 솔루션을 등록할 수 있습니다.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 04/24/2020
ms.openlocfilehash: dd8706c1e95e6b1e4ca4a38d4a336f6186464696
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872196"
---
# <a name="onboard-update-management-solution-using-azure-resource-manager-template"></a>Azure Resource Manager 템플릿을 사용 하 여 업데이트 관리 솔루션 등록

[Azure Resource Manager 템플릿을](../azure-resource-manager/templates/template-syntax.md) 사용 하 여 리소스 그룹에서 Azure Automation 업데이트 관리 솔루션을 사용 하도록 설정할 수 있습니다. 이 문서에서는 다음을 자동화 하는 샘플 템플릿을 제공 합니다.

* Azure Monitor Log Analytics 작업 영역을 만듭니다.
* Azure Automation 계정 만들기
* 아직 연결 되지 않은 경우 Automation 계정을 Log Analytics 작업 영역에 연결 합니다.
* Azure Automation 업데이트 관리 솔루션을 온 보 딩 합니다.

템플릿은 하나 이상의 Azure 또는 비 Azure Vm의 온 보 딩을 자동화 하지 않습니다.

구독에서 지원 되는 지역에 배포 된 Log Analytics 작업 영역 및 Automation 계정이 이미 있는 경우에는 연결 되지 않습니다. 작업 영역에 업데이트 관리 솔루션이 아직 배포 되지 않았습니다. 이 템플릿을 사용 하 여 성공적으로 링크를 만들고 업데이트 관리 솔루션을 배포 합니다. 

>[!NOTE]
>Linux의 업데이트 관리 일부로 **nxautomation** 사용자 등록은 서명 된 runbook만 실행 합니다.

>[!NOTE]
>이 문서는 새 Azure PowerShell Az 모듈을 사용하도록 업데이트되었습니다. AzureRM 모듈은 적어도 2020년 12월까지 버그 수정을 수신할 예정이므로 계속 사용하셔도 됩니다. 새 Az 모듈 및 AzureRM 호환성에 대한 자세한 내용은 [새 Azure PowerShell Az 모듈 소개](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)를 참조하세요. Hybrid Runbook Worker에 대한 Az 모듈 설치 지침은 [Azure PowerShell 모듈 설치](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)를 참조하세요. Automation 계정의 경우 [Azure Automation에서 Azure PowerShell 모듈을 업데이트하는 방법](automation-update-azure-modules.md)을 사용하여 모듈을 최신 버전으로 업데이트할 수 있습니다.

## <a name="api-versions"></a>API 버전

다음 표에서는이 템플릿에 사용 되는 리소스에 대 한 API 버전을 보여 줍니다.

| 리소스 | 리소스 유형 | API 버전 |
|:---|:---|:---|
| 작업 영역 | workspaces | 2017-03-15-preview |
| Automation 계정 | Automation | 2015-10-31 | 
| 솔루션 | solutions | 2015-11-01-preview |

## <a name="before-using-the-template"></a>템플릿을 사용 하기 전에

PowerShell을 로컬로 설치 하 고 사용 하도록 선택 하는 경우이 문서에는 Azure PowerShell Az 모듈이 필요 합니다. `Get-Module -ListAvailable Az`을 실행하여 버전을 찾습니다. 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요. PowerShell을 로컬로 실행 하는 경우 [AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) 를 실행 하 여 Azure와의 연결을 만들어야 합니다. Azure PowerShell 배포에서는 [AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)를 사용 합니다.

CLI를 로컬로 설치 하 고 사용 하도록 선택 하는 경우이 문서에서는 Azure CLI 버전 2.1.0 이상을 실행 해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)를 참조하세요. Azure CLI에서이 배포는 [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)를 사용 합니다. 

JSON 템플릿이 사용자에 게 묻는 메시지를 표시 하도록 구성 되어 있습니다.

* 작업 영역의 이름입니다.
* 작업 영역을 만들 지역입니다.
* Automation 계정의 이름
* 계정을 만들 지역입니다.

JSON 템플릿은 사용자 환경의 표준 구성에 사용할 수 있는 다른 매개 변수에 대 한 기본값을 지정 합니다. 조직에서 공유 액세스를 위해 Azure storage 계정에 템플릿을 저장할 수 있습니다. 템플릿 작업에 대 한 자세한 내용은 [리소스 관리자 템플릿 및 Azure CLI를 사용 하 여 리소스 배포](../azure-resource-manager/templates/deploy-cli.md)를 참조 하세요.

템플릿의 다음 매개 변수는 Log Analytics 작업 영역에 대 한 기본값으로 설정 됩니다.

* sku - 2018년 4월 가격 책정 모델에서 배포된 새로운 GB당 가격 책정 계층이 기본값
* 데이터 보존-기본값은 30 일입니다.
* 용량 예약-기본값은 100입니다.

>[!WARNING]
>새 2018년 4월 가격 책정 모델을 선택한 구독에서 Log Analytics 작업 영역을 만들거나 구성할 때 유효한 유일한 Log Analytics 가격 책정 계층은 **PerGB2018**입니다.
>

JSON 템플릿은 사용자 환경에서 표준 구성으로 사용 될 수 있는 다른 매개 변수에 대 한 기본값을 지정 합니다. 조직에서 공유 액세스를 위해 Azure storage 계정에 템플릿을 저장할 수 있습니다. 템플릿 작업에 대 한 자세한 내용은 [리소스 관리자 템플릿 및 Azure CLI를 사용 하 여 리소스 배포](../azure-resource-manager/templates/deploy-cli.md)를 참조 하세요.

새 자동화 계정에 연결 된 Log Analytics 작업 영역을 만들고, 구성 하 고, 사용 하려고 할 때 오류가 발생 하지 않도록 하려면 다음 구성 세부 Azure Monitor Azure Automation 정보를 이해 하는 것이 중요 합니다.

* 액세스 제어 모드, 가격 책정 계층, 보존 및 용량 예약 수준과 같은 작업 영역 구성 옵션을 완전히 이해 하려면 [추가 정보](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) 를 검토 합니다.

* 구독에서 Log Analytics 작업 영역 및 Automation 계정을 연결 하는 데는 특정 영역만 지원 되므로 [작업 영역 매핑](how-to/region-mappings.md) 을 검토 하 여 지원 되는 영역 인라인 또는 매개 변수 파일을 지정 합니다.

* Azure Monitor 로그를 처음 사용 하 고 작업 영역을 배포 하지 않은 경우 [작업 영역 디자인](../azure-monitor/platform/design-logs-deployment.md) 지침을 검토 하 여 액세스 제어에 대해 알아보고 조직에 권장 되는 디자인 구현 전략을 이해 해야 합니다.

## <a name="deploy-template"></a>템플릿 배포

1. 다음 JSON 구문을 파일에 복사하여 붙여넣습니다.

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "Workspace name"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
        }
    },
    "resources": [
        {
        "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]",
                    "name": "CapacityReservation",
                    "capacityReservationLevel": 100
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[resourceGroup().location]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
        },
        {
            "apiVersion": "2015-11-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
      ]
    }
    ```

2. 요구 사항을 충족하도록 템플릿을 편집합니다. 매개 변수를 인라인 값으로 전달 하는 대신 [리소스 관리자 매개 변수 파일](../azure-resource-manager/templates/parameter-files.md) 을 만드는 것이 좋습니다.

3. 이 파일을 로컬 폴더에 **Deployum솔루션 템플릿. json**으로 저장 합니다.

4. 이제 이 템플릿을 배포할 수 있습니다. PowerShell 또는 Azure CLI 중 하나를 사용할 수 있습니다. 작업 영역 및 Automation 계정 이름을 입력 하 라는 메시지가 표시 되 면 모든 Azure 구독에서 전역적으로 고유한 이름을 제공 합니다.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    배포가 완료될 때까지 몇 분 정도 걸릴 수 있습니다. 완료되면 다음과 유사하게 결과가 포함된 메시지가 표시됩니다.

    ![배포가 완료되었을 때 결과 예](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>다음 단계

업데이트 관리 솔루션을 배포한 후에는 관리를 위해 Vm을 사용 하도록 설정 하 고, 업데이트 평가를 검토 하 고, 업데이트를 배포 하 여 준수 상태로 전환할 수 있습니다.

- 하나 이상의 Azure 컴퓨터에 대 한 [Azure Automation 계정과](automation-onboard-solutions-from-automation-account.md) 비 azure 컴퓨터에 대해 수동으로

- Azure Portal의 가상 컴퓨터 페이지에서 단일 Azure VM의 경우 이 시나리오는 [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) 및 [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) vm에서 사용할 수 있습니다.

- [여러 Azure vm](manage-update-multi.md) 의 경우 Azure Portal의 **가상 컴퓨터** 페이지에서 vm을 선택 합니다. 