---
title: CloudSimple 사설 클라우드에서 Azure VMware 솔루션 삭제
description: CloudSimple 사설 클라우드를 삭제 하는 방법을 설명 합니다.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6bc3e7030c500ea2d6072a1cce0f0b3d9fc62801
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77024758"
---
# <a name="delete-a-cloudsimple-private-cloud"></a>CloudSimple 사설 클라우드를 삭제 합니다.

CloudSimple은 사설 클라우드를 삭제할 수 있는 유연성을 제공 합니다.  사설 클라우드는 하나 이상의 vSphere 클러스터로 구성 됩니다. 각 클러스터에는 3 ~ 16 개의 노드가 있을 수 있습니다. 사설 클라우드를 삭제 하면 모든 클러스터가 삭제 됩니다.

## <a name="before-you-begin"></a>시작하기 전에

사설 클라우드 삭제는 전체 사설 클라우드를 삭제 합니다.  사설 클라우드의 모든 구성 요소가 삭제 됩니다.  데이터를 유지 하려면 온-프레미스 저장소 또는 Azure storage에 데이터를 백업 했는지 확인 합니다.

사설 클라우드의 구성 요소는 다음과 같습니다.

* CloudSimple 노드
* 가상 머신
* VLAN/서브넷
* 사설 클라우드에 저장 된 모든 사용자 데이터
* VLAN/서브넷에 대 한 모든 방화벽 규칙 첨부 파일

## <a name="sign-in-to-azure"></a>Azure에 로그인

[https://portal.azure.com](https://portal.azure.com)에서 Azure Portal에 로그인합니다.

## <a name="delete-a-private-cloud"></a>프라이빗 클라우드 삭제

1. [CloudSimple 포털에 액세스](access-cloudsimple-portal.md)합니다.

2. **리소스** 페이지를 엽니다.

3. 삭제 하려는 사설 클라우드를 클릭 합니다.

4. 요약 페이지에서 **삭제**를 클릭 합니다.

    ![사설 클라우드 삭제](media/delete-private-cloud.png)

5. 확인 페이지에서 사설 클라우드의 이름을 입력 하 고 **삭제**를 클릭 합니다. 

    ![사설 클라우드 삭제-확인](media/delete-private-cloud-confirm.png)

사설 클라우드가 삭제 되도록 표시 되어 있습니다.  삭제 프로세스는 3 시간 후에 시작 하 고 사설 클라우드를 삭제 합니다.

> [!CAUTION]
> 사설 클라우드를 삭제 한 후 노드를 삭제 해야 합니다.  노드의 측정은 노드가 구독에서 삭제 될 때까지 계속 됩니다.

## <a name="next-steps"></a>다음 단계

* [노드 삭제](delete-nodes.md)
