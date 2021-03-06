---
title: 서버 매개 변수 구성-Azure PowerShell-Azure Database for MySQL
description: 이 문서에서는 PowerShell을 사용 하 여 Azure Database for MySQL에서 서비스 매개 변수를 구성 하는 방법을 설명 합니다.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: conceptual
ms.date: 4/29/2020
ms.openlocfilehash: 0de816d25bbc1563885413d8dbd52dc7bda7d538
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615085"
---
# <a name="customize-azure-database-for-mysql-server-parameters-using-powershell"></a>PowerShell을 사용 하 여 Azure Database for MySQL 서버 매개 변수 사용자 지정

PowerShell을 사용 하 여 Azure Database for MySQL 서버에 대 한 구성 매개 변수를 나열 하 고, 표시 하 고, 업데이트할 수 있습니다. 엔진 구성의 하위 집합은 서버 수준에서 노출되고 수정할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법 가이드를 완료하려면 다음이 필요합니다.

- 로컬에 설치 되거나 브라우저에 [Azure Cloud Shell](https://shell.azure.com/) 된 [Az PowerShell 모듈](/powershell/azure/install-az-ps)
- [Azure Database for MySQL 서버](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az PowerShell module은 preview에 있지만 다음 명령을 사용 하 여 Az PowerShell 모듈에서 별도로 설치 해야 합니다 `Install-Module -Name Az.MySql -AllowPrerelease`.
> Az PowerShell module은 일반적으로 사용할 수 있게 되 면 이후 Az PowerShell 모듈 릴리스에 포함 되며 Azure Cloud Shell 내에서 기본적으로 사용할 수 있습니다.

PowerShell을 로컬로 사용 하도록 선택 하는 경우 [AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet을 사용 하 여 Azure 계정에 연결 합니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Azure Database for MySQL에 대한 서버 구성 매개 변수 나열

서버에 있는 수정 가능한 모든 매개 변수와 해당 값을 나열 하려면 `Get-AzMySqlConfiguration` cmdlet을 실행 합니다.

다음 예에서는 **myresourcegroup**리소스 그룹의 **mydemoserver** 서버에 대 한 서버 구성 매개 변수를 나열 합니다.

```azurepowershell-interactive
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

나열된 각 매개 변수의 정의를 보려면 [서버 시스템 변수](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)에서 MySQL 참조 섹션을 참조하세요.

## <a name="show-server-configuration-parameter-details"></a>서버 구성 매개 변수 세부 정보 표시

서버에 대 한 특정 구성 매개 변수에 대 한 세부 정보를 표시 `Get-AzMySqlConfiguration` 하려면 cmdlet을 실행 하 고 **Name** 매개 변수를 지정 합니다.

이 예에서는 리소스 그룹 **myresourcegroup**에서 서버 **mydemoserver** 에 대 한 **\_\_저속 쿼리 로그** 서버 구성 매개 변수의 세부 정보를 보여 줍니다.

```azurepowershell-interactive
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>서버 구성 매개 변수 값 수정

특정 서버 구성 매개 변수의 값을 수정할 수 있습니다. 그러면 MySQL 서버 엔진에 대한 기본 구성 값이 업데이트됩니다. 구성을 업데이트 하려면 `Update-AzMySqlConfiguration` cmdlet을 사용 합니다.

리소스 그룹 **myresourcegroup**에서 서버 **mydemoserver** 의 **\_\_저속 쿼리 로그** 서버 구성 매개 변수를 업데이트 합니다.

```azurepowershell-interactive
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [PowerShell을 사용 하 여 Azure Database for MySQL 서버에서 저장소를 자동으로 확장](howto-auto-grow-storage-powershell.md)합니다.
