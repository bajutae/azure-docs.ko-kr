---
title: 'PowerShell 스크립트: Azure Lab Services에서 허용되는 VM 크기 설정 | Microsoft Docs'
description: 이 문서에는 Azure Lab Services에서 허용 되는 VM (가상 머신) 크기를 설정 하는 샘플 PowerShell 스크립트가 포함 되어 있습니다.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2020
ms.author: spelluru
ms.openlocfilehash: 5b3dbee7d0ac928c4f18f25348e714aba9c1cd13
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82100773"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>PowerShell을 사용하여 Azure Lab Services에서 허용되는 VM 크기 설정

이 샘플 PowerShell 스크립트는 Azure Lab Services에서 허용되는 VM(가상 머신) 크기를 설정합니다.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>사전 요구 사항
* **랩**. 스크립트를 사용하려면 기존 랩이 있어야 합니다. 

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 

| 명령 | 메모 |
|---|---|
| AzResource | 지정된 매개 변수를 기반으로 리소스를 검색합니다. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | 리소스를 가져옵니다. |
| [집합 AzResource](/powershell/module/az.resources/set-azresource) | 리소스를 수정합니다. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | 리소스를 만듭니다. |

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/)를 참조하세요.

추가 Azure Lab Services PowerShell 스크립트 샘플은 [Azure Lab Services PowerShell 샘플](../samples-powershell.md)에서 확인할 수 있습니다.
