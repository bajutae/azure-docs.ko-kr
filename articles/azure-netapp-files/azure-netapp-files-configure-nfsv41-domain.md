---
title: Azure NetApp Files에 대 한 NFSv 4.1 기본 도메인 구성 | Microsoft Docs
description: Azure NetApp Files에서 NFSv 4.1을 사용 하도록 NFS 클라이언트를 구성 하는 방법을 설명 합니다.
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: b-juche
ms.openlocfilehash: 77178a23206eadae941794c92b8dd99fe2ca1e05
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73906287"
---
# <a name="configure-nfsv41-default-domain-for-azure-netapp-files"></a>Azure NetApp Files에 대한 NFSv 4.1 기본 도메인 구성

NFSv4는 인증 도메인의 개념을 소개 합니다. 현재 Azure NetApp Files는 서비스에서 NFS 클라이언트로의 루트 전용 사용자 매핑을 지원 합니다. Azure NetApp Files에서 NFSv 4.1 기능을 사용 하려면 NFS 클라이언트를 업데이트 해야 합니다.

## <a name="default-behavior-of-usergroup-mapping"></a>사용자/그룹 매핑의 기본 동작

NFSv4 도메인은로 `nobody` `localdomain`설정 되어 있으므로 루트 매핑은 사용자에 게 기본적으로 사용 됩니다. Azure NetApp Files NFSv 4.1 볼륨을 루트로 탑재 하면 다음과 같은 파일 사용 권한이 표시 됩니다.  

![NFSv 4.1에 대 한 사용자/그룹 매핑의 기본 동작](../media/azure-netapp-files/azure-netapp-files-nfsv41-default-behavior-user-group-mapping.png)

위의 예제에서 볼 수 있듯이의 `file1` 사용자는 이어야 `root`하지만 기본적으로에 `nobody` 매핑됩니다.  이 문서에서는 `file1` 사용자를로 `root`설정 하는 방법을 보여 줍니다.  

## <a name="steps"></a>단계 

1. NFS 클라이언트 `/etc/idmapd.conf` 에서 파일을 편집 합니다.   
    줄 `#Domain` 의 주석 처리를 제거 합니다. 즉 `#` , 줄에서을 제거 하 고 값 `localdomain` 을로 `defaultv4iddomain.com`변경 합니다. 

    초기 구성: 
    
    ![NFSv 4.1의 초기 구성](../media/azure-netapp-files/azure-netapp-files-nfsv41-initial-config.png)

    업데이트 된 구성:
    
    ![NFSv 4.1의 업데이트 된 구성](../media/azure-netapp-files/azure-netapp-files-nfsv41-updated-config.png)

2. 현재 탑재 된 NFS 볼륨을 분리 합니다.
3. 파일을 `/etc/idmapd.conf` 업데이트 합니다.
4. 호스트 ( `rpcbind` `service rpcbind restart`)에서 서비스를 다시 시작 하거나 단순히 호스트를 다시 부팅 합니다.
5. 필요에 따라 NFS 볼륨을 탑재 합니다.   

    [Windows 또는 Linux 가상 머신에 대 한 볼륨 탑재 또는 분리](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)를 참조 하세요. 

다음 예에서는 결과 사용자/그룹 변경 내용을 보여 줍니다. 

![NFSv 4.1에 대 한 결과 구성](../media/azure-netapp-files/azure-netapp-files-nfsv41-resulting-config.png)

예제에 나와 있는 것 처럼 사용자/그룹이 이제에서 `nobody` 로 `root`변경 되었습니다.

## <a name="behavior-of-other-non-root-users-and-groups"></a>다른 (루트가 아닌) 사용자 및 그룹의 동작

Azure NetApp Files는 NFSv 4.1 볼륨의 파일 또는 폴더와 연결 된 사용 권한이 있는 로컬 사용자 (호스트에서 로컬로 생성 된 사용자)를 지원 합니다. 그러나 서비스는 현재 여러 노드 간에 사용자/그룹 매핑을 지원 하지 않습니다. 따라서 한 호스트에서 만든 사용자는 기본적으로 다른 호스트에서 만든 사용자에 게 매핑되지 않습니다. 

다음 예제에서에는 `Host1` 세 개의 기존 테스트 사용자 계정 (`testuser01`, `testuser02`, `testuser03`)이 있습니다. 

![NFSv 4.1에 대 한 결과 구성](../media/azure-netapp-files/azure-netapp-files-nfsv41-host1-users.png)

에서 `Host2`테스트 사용자 계정은 만들어지지 않았지만 동일한 볼륨이 두 호스트에 모두 탑재 되어 있는지 확인 합니다.

![NFSv 4.1에 대 한 결과 구성](../media/azure-netapp-files/azure-netapp-files-nfsv41-host2-users.png)

## <a name="next-step"></a>다음 단계 

[Windows 또는 Linux 가상 머신에 대한 볼륨 탑재 또는 탑재 해제](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)

