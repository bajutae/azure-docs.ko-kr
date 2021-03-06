---
title: Azure Monitor에서 활동 로그 경고 만들기, 보기 및 관리
description: Azure Portal, Azure Resource Manager 템플릿 및 Azure PowerShell를 사용 하 여 활동 로그 경고를 만듭니다.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 06/25/2019
ms.openlocfilehash: bfbe2bc3ae3edf9285d3ec006ab0451f070cabd6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80132401"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Azure Monitor를 사용 하 여 활동 로그 경고 만들기, 보기 및 관리  

## <a name="overview"></a>개요

활동 로그 경고는 경고에 지정된 조건과 일치하는 새 활동 로그 이벤트가 발생할 때 활성화되는 경고입니다.

이러한 경고는 Azure 리소스에 대 한 것 이며 Azure Resource Manager 템플릿을 사용 하 여 만들 수 있습니다. 또한 Azure Portal에서 생성, 업데이트 또는 삭제할 수 있습니다. 일반적으로 Azure 구독의 리소스에 대 한 특정 변경이 발생할 때 알림을 수신 하는 활동 로그 경고를 만듭니다. 경고는 일반적으로 특정 리소스 그룹 또는 리소스로 범위가 지정 됩니다. 예를 들어 샘플 리소스 그룹 **myProductionResourceGroup** 의 가상 머신이 삭제 되 면 알림이 표시 되도록 할 수 있습니다. 또는 구독에서 사용자에 게 새 역할이 할당 된 경우 알림이 표시 될 수도 있습니다.

> [!IMPORTANT]
> 활동 로그 경고 만들기에 대 한 인터페이스를 통해 서비스 상태 알림에 대 한 경고를 만들 수 없습니다. 서비스 상태 알림을 만들고 사용 하는 방법에 대 한 자세한 내용은 [서비스 상태 알림에서 활동 로그 경고 수신](alerts-activity-log-service-notifications.md)을 참조 하세요.

경고 규칙을 만들 때 다음을 확인 합니다.

- 범위의 구독이 경고가 생성 된 구독과 다릅니다.
- 조건은 경고가 구성 된 수준, 상태, 호출자, 리소스 그룹, 리소스 ID 또는 리소스 유형 이벤트 범주 여야 합니다.
- 경고 구성 JSON에는 "anyOf" 조건 또는 중첩 조건이 없습니다. 기본적으로 "allOf" 또는 "anyOf" 조건이 추가로 포함 되지 않은 하나의 "allOf" 조건만 허용 됩니다.
- 범주가 "관리" 인 경우 경고에서 앞의 조건 중 하나 이상을 지정 해야 합니다. 활동 로그에서 이벤트가 생성될 때마다 활성화되는 경고를 만들 필요는 없습니다.

## <a name="azure-portal"></a>Azure portal

Azure Portal를 사용 하 여 활동 로그 경고 규칙을 만들고 수정할 수 있습니다. 이 환경은 관심 있는 특정 이벤트에 대 한 원활한 경고 생성을 보장 하기 위해 Azure 활동 로그와 통합 됩니다.

### <a name="create-with-the-azure-portal"></a>Azure Portal를 사용 하 여 만들기

다음 절차를 사용합니다.

1. Azure Portal에서 **모니터** > **경고**를 선택 합니다.
2. **경고** 창의 왼쪽 위 모서리에서 **새 경고 규칙** 을 선택 합니다.

     ![새 경고 규칙](media/alerts-activity-log/AlertsPreviewOption.png)

     **규칙 만들기** 창이 나타납니다.

      ![새 경고 규칙 옵션](media/alerts-activity-log/create-new-alert-rule-options.png)

3. **경고 조건 정의**에서 다음 정보를 입력 하 고 **완료**를 선택 합니다.

   - **경고 대상:** 새 경고의 대상을 확인 하 고 선택 하려면 /  **구독 별 필터링**을 사용 하 여**리소스 유형별로**필터링 합니다. 표시 된 목록에서 리소스 또는 리소스 그룹을 선택 합니다.

     > [!NOTE]
     > 
     > 활동 로그 신호의 [Azure Resource Manager](../../azure-resource-manager/management/overview.md) 추적 된 리소스, 리소스 그룹 또는 전체 구독만을 선택할 수 있습니다. 

     **경고 대상 샘플 보기**

     ![대상 선택](media/alerts-activity-log/select-target.png)

   - **대상 조건**에서 **조건 추가**를 선택 합니다. 대상에 대해 사용 가능한 모든 신호가 표시 됩니다. 여기에는 다양 한 **활동 로그**범주의 사용 가능한 신호가 포함 됩니다. 범주 이름이 **모니터 서비스** 이름에 추가 됩니다.

   - **활동 로그** 유형에 대해 가능한 다양한 작업에 대해 표시된 목록에서 신호를 선택합니다.

     이 대상 신호에 대한 로그 기록 타임라인 및 해당 경고 논리를 선택할 수 있습니다.

     **조건 추가 화면**

     ![조건 추가](media/alerts-activity-log/add-criteria.png)

     - **기록 시간**: 선택한 작업에 사용할 수 있는 이벤트는 최근 6 시간, 12 시간 또는 24 시간 또는 지난 주 동안 그릴 수 있습니다.

     - **경고 논리**:

       - **이벤트 수준**: 이벤트의 심각도 수준 ( _자세한 정보_, _정보_, _경고_, _오류_또는 _중요_)입니다.
       - **상태**: 이벤트의 상태: _시작 됨_, _실패_또는 _성공_입니다.
       - **이벤트를 시작한 사람**: 호출자 라고도 합니다. 작업을 수행한 사용자의 이메일 주소 또는 Active Directory 식별자입니다.

       이 샘플 신호 그래프에는 경고 논리가 적용 되어 있습니다.

       ![선택한 조건](media/alerts-activity-log/criteria-selected.png)

4. **경고 세부 정보 정의**에서 다음 세부 정보를 제공 합니다.

    - **경고 규칙 이름**: 새 경고 규칙의 이름입니다.
    - **설명**: 새 경고 규칙에 대 한 설명입니다.
    - **리소스 그룹에 경고 저장**:이 새 규칙을 저장할 리소스 그룹을 선택 합니다.

5. **작업 그룹** 아래의 드롭다운 메뉴에서 이 새 경고 규칙에 할당할 작업 그룹을 지정합니다. 또는 [새 작업 그룹을 만들고](../../azure-monitor/platform/action-groups.md) 새 규칙에 할당 합니다. 새 그룹을 만들려면 **+ 새 그룹**을 선택 합니다.

6. 규칙을 만든 후에 설정 하려면 규칙을 만들 **때 규칙 사용** 옵션에 대해 **예** 를 선택 합니다.
7. **경고 규칙 만들기**를 선택합니다.

    활동 로그에 대 한 새 경고 규칙이 생성 되 고 창의 오른쪽 위 모퉁이에 확인 메시지가 표시 됩니다.

    규칙을 사용/사용하지 않도록 설정, 편집 또는 삭제할 수 있습니다. 활동 로그 규칙을 관리 하는 방법에 대해 자세히 알아보세요.


활동 로그에서 경고 규칙을 만들 수 있는 조건을 이해 하는 간단한 비유는 [Azure Portal 활동 로그](activity-log-view.md#azure-portal)를 통해 이벤트를 탐색 하거나 필터링 하는 것입니다. **Azure Monitor 활동 로그** 화면에서 필요한 이벤트를 필터링 하거나 찾은 다음 **활동 로그 경고 추가** 단추를 사용 하 여 경고를 만들 수 있습니다. 그런 다음 앞에서 설명한 대로 4 ~ 7 단계를 수행 합니다.
    
 ![활동 로그에서 경고 추가](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Azure Portal 보기 및 관리

1. Azure Portal에서 **모니터** > **경고**를 선택 합니다. 창의 왼쪽 위 모서리에서 **경고 규칙 관리** 를 선택 합니다.

    ![경고 규칙 관리](media/alerts-activity-log/manage-alert-rules.png)

    사용 가능한 규칙 목록이 나타납니다.

2. 수정할 활동 로그 규칙을 검색 합니다.

    ![활동 로그 경고 규칙 검색](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    사용 가능한 필터, _구독_, _리소스 그룹_, _리소스_, _신호 유형_또는 _상태_를 사용 하 여 편집 하려는 활동 규칙을 찾을 수 있습니다.

   > [!NOTE]
   > 
   > **설명**, **대상 조건**및 **작업 그룹만**편집할 수 있습니다.

3. 규칙을 선택 하 고 두 번 클릭 하 여 규칙 옵션을 편집 합니다. 필요한 항목을 변경한 다음 **저장**을 선택 합니다.

   ![경고 규칙 관리](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. 규칙을 사용 하거나 사용 하지 않도록 설정 하거나 삭제할 수 있습니다. 2 단계에서 설명한 대로 규칙을 선택한 후 창의 맨 위에 있는 적절 한 옵션을 선택 합니다.


## <a name="azure-resource-manager-template"></a>Azure Resource Manager 템플릿
Azure Resource Manager 템플릿을 사용 하 여 활동 로그 경고 규칙을 만들려면 유형의 `microsoft.insights/activityLogAlerts`리소스를 만듭니다. 그런 다음 모든 관련된 속성을 입력합니다. 활동 로그 경고 규칙을 만드는 템플릿은 다음과 같습니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
이전 샘플 JSON은이 연습을 목적으로 sampleActivityLogAlert와 같이 저장할 수 있으며 [Azure Portal에서 Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md)를 사용 하 여 배포할 수 있습니다.

다음 필드는 조건 필드의 Azure Resource Manager 템플릿에서 사용할 수 있는 옵션입니다. "Resource Health", "Advisor" 및 "Service Health"에는 특수 한 필드에 대 한 추가 속성 필드가 있습니다. 
1. resourceId: 경고가 생성 되어야 하는 활동 로그 이벤트에서 영향을 받는 리소스의 리소스 ID입니다.
2. category: 활동 로그 이벤트의 범주입니다. 예: 관리, ServiceHealth, ResourceHealth, 자동 크기 조정, 보안, 권장, 정책.
3. 호출자: 활동 로그 이벤트의 작업을 수행한 사용자의 이메일 주소 또는 Azure Active Directory 식별자입니다.
4. 수준: 경고를 생성 해야 하는 활동 로그 이벤트의 활동 수준입니다. 예: 중요, 오류, 경고, 정보, 자세한 정보.
5. operationName: 활동 로그 이벤트의 작업 이름입니다. 예: Microsoft .Resources/배포/쓰기
6. resourceGroup: 활동 로그 이벤트에서 영향을 받는 리소스에 대 한 리소스 그룹의 이름입니다.
7. resourceProvider: [Azure 리소스 공급자 및 형식 설명](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fresource-providers-and-types&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373543634&sdata=4RjpTkO5jsdOgPdt%2F%2FDOlYjIFE2%2B%2BuoHq5%2F7lHpCwQw%3D&reserved=0)입니다. 리소스 공급자를 Azure 서비스에 매핑하는 목록은 [azure 서비스에 대 한 리소스 공급자](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fazure-services-resource-providers&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373553639&sdata=0ZgJPK7BYuJsRifBKFytqphMOxMrkfkEwDqgVH1g8lw%3D&reserved=0)를 참조 하세요.
8. 상태: 작업 이벤트의 작업 상태를 설명 하는 문자열입니다. 예: 시작 됨, 진행 중, 성공, 실패, 활성, 해결 됨
9. 하위 상태: 일반적으로 해당 REST 호출의 HTTP 상태 코드 이지만 하위 상태를 설명 하는 다른 문자열을 포함할 수도 있습니다.   예: OK (HTTP 상태 코드: 200), 만들어짐 (HTTP 상태 코드: 201), 수락 됨 (HTTP 상태 코드: 202), 콘텐츠 없음 (HTTP 상태 코드: 204), 잘못 된 요청 (HTTP 상태 코드: 400), 찾을 수 없음 (http 상태 코드: 404), 충돌 (http 상태 코드: 409), 내부 서버 오류 (HTTP 상태 코드: 500), 서비스를 사용할 수 없음 (http 상태 코드: 503), 게이트웨이 시간 초과 (http 상태 코드: 504).
10. resourceType: 이벤트의 영향을 받은 리소스의 형식입니다. 예: Microsoft .Resources/배포

다음은 그 예입니다.

```json
"condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        }

```
활동 로그 필드에 대 한 자세한 내용은 [여기](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-monitor%2Fplatform%2Factivity-log-schema&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373563632&sdata=6QXLswwZgUHFXCuF%2FgOSowLzA8iOALVgvL3GMVhkYJY%3D&reserved=0)에서 찾을 수 있습니다.



> [!NOTE]
> 새 활동 로그 경고 규칙이 활성화 되는 데 최대 5 분이 걸릴 수 있습니다.

## <a name="rest-api"></a>REST API 
[활동 로그 경고 API Azure Monitor](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) 은 REST API입니다. Azure Resource Manager REST API와 완전히 호환 됩니다. 리소스 관리자 cmdlet 또는 Azure CLI를 사용 하 여 PowerShell을 통해 사용할 수 있습니다.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>PowerShell을 사용 하 여 리소스 관리자 템플릿 배포
PowerShell을 사용 하 여 이전 [Azure Resource Manager 템플릿](#azure-resource-manager-template) 섹션에 표시 된 샘플 리소스 관리자 템플릿을 배포 하려면 다음 명령을 사용 합니다.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

여기서 sampleActivityLogAlert에는 경고 규칙을 만드는 데 필요한 매개 변수에 제공 된 값이 포함 됩니다.

### <a name="use-activity-log-powershell-cmdlets"></a>활동 로그 PowerShell cmdlet 사용

활동 로그 경고에서는 다음과 같은 전용 PowerShell cmdlet을 사용할 수 있습니다.

- [AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert): 새 활동 로그 경고를 만들거나 기존 활동 로그 경고를 업데이트 합니다.
- [AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert): 하나 이상의 활동 로그 경고 리소스를 가져옵니다.
- [AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert): 기존 활동 로그 경고를 사용 하도록 설정 하 고 해당 태그를 설정 합니다.
- [AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert): 기존 활동 로그 경고를 사용 하지 않도록 설정 하 고 해당 태그를 설정 합니다.
- [AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert): 활동 로그 경고를 제거 합니다.

## <a name="azure-cli"></a>Azure CLI

설정된 [az monitor activity-log alert](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert)의 전용 Azure CLI 명령을 활동 로그 경고 규칙을 관리하는 데 사용할 수 있습니다.

새 활동 로그 경고 규칙을 만들려면 다음 명령을이 순서로 사용 합니다.

1. [az monitor 활동-log alert create](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): 새 활동 로그 경고 규칙 리소스를 만듭니다.
1. [az monitor 활동-로그 경고 범위](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): 만든 활동 로그 경고 규칙에 대 한 범위를 추가 합니다.
1. [az monitor 활동-로그 경고 작업-그룹](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): 활동 로그 경고 규칙에 작업 그룹을 추가 합니다.

활동 로그 경고 규칙 리소스 하나를 검색 하려면 Azure CLI 명령 [az monitor activity-log alert show](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
)를 사용 합니다. 리소스 그룹에서 모든 활동 로그 경고 규칙 리소스를 보려면 [az monitor 활동-log alert list](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list)를 사용 합니다.
활동 로그 경고 규칙 리소스는 Azure CLI 명령 [az monitor 활동-로그 경고 삭제](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete)를 사용 하 여 제거할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [활동 로그의 webhook 스키마](../../azure-monitor/platform/activity-log-alerts-webhook.md)에 대해 알아봅니다.
- [활동 로그 개요](../../azure-monitor/platform/activity-log-alerts.md)를 참조 하세요.
- [작업 그룹](../../azure-monitor/platform/action-groups.md)에 대해 자세히 알아보세요.  
- [서비스 상태 알림에](../../azure-monitor/platform/service-notifications.md)대해 알아봅니다.
