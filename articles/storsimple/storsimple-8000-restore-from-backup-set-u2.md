---
title: StorSimple 8000 시리즈의 백업에서 볼륨 복원 | Microsoft Docs
description: StorSimple 디바이스 관리자 서비스 Backup 카탈로그를 사용하여 백업 세트에서 StorSimple 볼륨을 복원하는 방법을 설명합니다.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 6a2e022697ced90d968075b7a4abe4163be7a539
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60723393"
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>백업 세트에서 StorSimple 볼륨 복원

## <a name="overview"></a>개요

이 자습서에서는 기존 백업 세트를 사용하여 StorSimple 8000 시리즈 디바이스에서 수행되는 복원 작업을 설명합니다. **Backup 카탈로그** 블레이드를 사용하여 로컬 또는 클라우드 백업에서 볼륨을 복원합니다. **Backup 카탈로그** 블레이드에는 수동 또는 자동화된 백업을 수행할 때 만들어지는 모든 백업 세트가 표시됩니다. 백그라운드에서 데이터를 다운로드하는 동안 백업 세트의 복원 작업에서는 즉시 볼륨을 온라인으로 전환합니다.

복원을 시작하는 다른 방법은 **디바이스 &gt; [사용자 디바이스] &gt; 볼륨**으로 이동하는 것입니다. **볼륨** 블레이드에서 볼륨을 선택하고, 마우스 오른쪽 단추를 클릭하여 상황에 맞는 메뉴를 호출하고, **복원**을 선택합니다.

## <a name="before-you-restore"></a>복원하기 전에

복원을 시작하기 전에 다음 주의 사항을 검토하세요.

* **볼륨을 오프라인해야 합니다.** – 복원 작업을 시작하기 전에 호스트와 디바이스 모두에서 볼륨을 오프라인해야 합니다. 복원 작업은 디바이스에서 볼륨을 온라인으로 자동으로 전환하지만 호스트에서 디바이스를 수동으로 온라인 상태로 만들어야 합니다. 볼륨이 디바이스에서 온라인이 되는 즉시 호스트에서 볼륨을 온라인으로 전환할 수 있습니다. 복원 작업이 완료 될 때까지 기다릴 필요가 없습니다. 절차를 보려면 볼륨을 [오프 라인](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)으로 전환을 참조 하세요.

* **복원 후 볼륨 유형** – 삭제된 볼륨은 스냅샷의 유형에 따라 복원됩니다. 즉, 로컬로 고정된 볼륨은 로컬로 고정된 볼륨으로 복원되고 계층화된 볼륨은 계층화된 볼륨으로 복원됩니다.

    기존 볼륨의 경우 현재 볼륨의 사용 유형은 스냅샷에 저장되는 유형을 재정의합니다. 예를 들어 볼륨 유형이 계층화였을 때 수행한 스냅샷의 볼륨을 복원하고 볼륨 유형이 이제 로컬로 고정되는 경우(수행된 변환 작업으로 인해) 볼륨은 로컬로 고정된 볼륨으로 복원됩니다. 마찬가지로, 기존 로컬로 고정된 볼륨이 확장되고 이후에 볼륨이 더 작은 경우에 수행된 이전 스냅샷에서 복원된 경우 복원된 볼륨은 현재 확장된 크기를 유지합니다.

    볼륨을 복원하는 동안 계층화된 볼륨에서 로컬로 고정된 볼륨으로 또는 로컬로 고정된 볼륨에서 계층화된 볼륨으로 볼륨을 변환할 수 없습니다. 복원 작업이 완료될 때까지 기다린 다음 볼륨을 다른 종류로 변환할 수 있습니다. 볼륨 변환에 대한 자세한 내용은 [볼륨 유형 변경](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)으로 이동합니다. 

* **볼륨 크기는 복원된 볼륨에 반영됩니다.** – 삭제된(로컬로 고정된 볼륨이 완전히 프로비전되었기 때문에) 로컬로 고정된 볼륨을 복원하는 경우 중요한 고려 사항입니다. 이전에 삭제된 로컬로 고정된 볼륨을 복원하기 전에 충분한 공간이 있는지 확인합니다.

* **복원하는 동안 볼륨을 확장할 수 없습니다.** – 볼륨을 확장하기 전에 복원 작업이 완료될 때까지 기다립니다. 볼륨 확장에 대한 정보는 [볼륨 수정](storsimple-8000-manage-volumes-u2.md#modify-a-volume)으로 이동합니다.

* **로컬 볼륨을 복원하는 동안 백업을 수행할 수 있습니다.** – 해당 절차를 보려면 [StorSimple 디바이스 관리자 서비스를 사용하여 백업 정책 관리](storsimple-8000-manage-backup-policies-u2.md)로 이동합니다.

* **복원 작업을 취소할 수 있습니다.** - 복원 작업을 취소하는 경우 볼륨은 복원 작업을 시작하기 전의 상태로 롤백됩니다. 절차를 보려면 [작업 취소](storsimple-8000-manage-jobs-u2.md#cancel-a-job)로 이동합니다.

## <a name="how-does-restore-work"></a>복원 작업 방법

업데이트 4 이상을 실행하는 디바이스의 경우 heatmap 기반 복원이 구현됩니다. 데이터에 액세스하는 호스트의 요청이 디바이스에 도달하면 이러한 요청이 추적되고 heatmap이 만들어집니다. 요청 비율이 높으면 열이 많은 데이터 청크가 발생하고 요청 비율이 낮으면 열이 더 낮은 청크로 변환됩니다. _hot_으로 표시되려면 최소한 두 번 이상 데이터에 액세스해야 합니다. 수정된 파일도 _hot_으로 표시됩니다. 복원을 시작하면 heatmap을 기반으로 데이터의 자동 관리 하이드레이션이 발생합니다. 업데이트 4 이전 버전의 경우 데이터는 액세스 시에만 복원 중에 다운로드됩니다.

열 지도 기반 복원에는 다음 주의 사항이 적용됩니다.

* 열 지도 추적은 계층화된 볼륨에 대해서만 사용하도록 설정되고 로컬로 고정된 볼륨은 지원되지 않습니다.

* 다른 디바이스에 볼륨을 복제하는 경우에는 열 지도 기반 복원이 지원되지 않습니다. 

* 현재 위치 복원이 있고 복원할 볼륨에 대한 로컬 스냅샷은 디바이스에 있는 경우 (데이터를 이미 로컬에서 사용할 수 있으므로) 리하이드레이션하지 않습니다. 

* 기본적으로 복원할 때 heatmap을 기반으로 데이터를 사전에 리하이드레이션하는 리하이드레이션 작업이 시작됩니다. 

업데이트 4에서 Windows PowerShell cmdlet은 리하이드레이션 작업 실행을 쿼리하고, 리하이드레이션 작업을 취소하며, 리하이드레이션 작업 작업의 상태를 가져오는 데 사용할 수 있습니다.

* `Get-HcsRehydrationJob` - 이 cmdlet은 리하이드레이션 작업의 상태를 가져옵니다. 하나의 볼륨에 대해 단일 리하이드레이션 작업이 트리거됩니다.

* `Set-HcsRehydrationJob` - 이 cmdlet을 사용하면 리하이드레이션이 진행 중일 때 해당 작업을 일시 중지, 중지 또는 재개할 수 있습니다.

리하이드레이션 cmdlet에 대한 자세한 내용은 [StorSimple용 Windows PowerShell cmdlet 참조](https://technet.microsoft.com/library/dn688168.aspx)로 이동하세요.

자동 리하이드레이션을 사용하면 일반적으로 일시적인 더 높은 읽기 성능이 예상됩니다. 실제 개선되는 정도는 액세스 패턴, 데이터 변동 및 데이터 형식 등 다양한 요인에 따라 달라집니다. 

리하이드레이션 작업을 취소하려면 PowerShell cmdlet을 사용하면 됩니다. 모든 향후 복원에 대해 리하이드레이션 작업을 영구적으로 사용 하지 않도록 설정 하려는 경우 [Microsoft 지원에 문의 하세요](storsimple-8000-contact-microsoft-support.md).

## <a name="how-to-use-the-backup-catalog"></a>백업 카탈로그를 사용하는 방법

**Backup 카탈로그** 블레이드는 백업 세트 선택 범위를 좁히는 데 도움이 되는 쿼리를 제공합니다. 다음 매개 변수를 기반으로 검색되는 백업 세트를 필터링할 수 있습니다.

* **시간 범위** – 백업 세트를 만든 날짜 및 시간 범위입니다.
* **디바이스** – 백업 세트를 만든 디바이스입니다.
* **Backup 정책** 또는 **볼륨** – 백업 정책 또는 이 백업 세트와 연결된 볼륨입니다.

그런 다음 필터링된 백업 세트는 다음 특성을 기반으로 표로 정리됩니다.

* **이름** – 백업 세트와 연결된 백업 정책 또는 볼륨의 이름입니다.
* **유형** - 백업 세트는 로컬 스냅숏 또는 클라우드 스냅숏일 수 있습니다. 로컬 스냅샷은 디바이스에 로컬로 저장된 모든 볼륨 데이터의 백업인 반면 클라우드 스냅샷은 클라우드에 상주하는 볼륨 데이터의 백업을 참조합니다. 로컬 스냅샷은 데이터 복구 기능으로 클라우드 스냅샷을 선택하는 반면 빠른 액세스를 제공합니다.
* **크기** – 백업 세트의 실제 크기입니다.
* **만든** 날짜 – 백업이 만들어진 날짜 및 시간입니다. 
* **볼륨** -백업 세트와 연결된 볼륨의 수입니다.
* **시작** – 일정에 따라 자동으로 또는 사용자가 수동으로 백업을 시작할 수 있습니다. (백업을 예약하기 위해 백업 정책을 사용할 수 있습니다. 또는 **백업 수행** 옵션을 사용하여 대화형 또는 요청 시 백업을 수행할 수 있습니다.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>백업에서 StorSimple 볼륨을 복원하는 방법

**Backup 카탈로그** 블레이드를 사용하여 특정 백업에서 StorSimple 볼륨을 복원할 수 있습니다. 그러나 볼륨을 복원하면 백업이 수행된 시점의 상태로 볼륨이 되돌려진다는 점에 유의하세요. 백업 작업 후 추가된 모든 데이터가 손실됩니다.

> [!WARNING]
> 백업에서 복원되면 백업에서 기존 볼륨을 대체합니다. 백업이 수행된 후 작성된 모든 데이터가 손실될 수 있습니다.


### <a name="to-restore-your-volume"></a>볼륨을 복원하려면
1. StorSimple 디바이스 관리자 서비스로 이동한 다음 **Backup 카탈로그**를 클릭합니다.

2. 백업 세트를 다음과 같이 선택합니다.
   
   1. 시간 범위를 지정합니다.
   2. 해당 디바이스를 선택합니다.
   3. 드롭다운 목록에서 선택하려는 백업에 대 한 볼륨 또는 백업 정책을 선택합니다.
   4. **적용**을 클릭하여 이 쿼리를 실행합니다.

      선택한 볼륨와 연결된 백업을 또는 백업 정책이 백업 세트의 목록에 나타나야 합니다.
   
      ![Backup 세트 목록](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. 백업 세트를 확장하여 연결된 볼륨을 봅니다. 이 볼륨은 복원하려면 호스트와 디바이스에서 오프라인 상태여야 합니다. 디바이스의 **볼륨** 블레이드에서 볼륨에 액세스한 다음, [볼륨을 오프라인 상태로 전환](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)의 단계에 따라 오프라인 상태로 전환합니다.
   
   > [!IMPORTANT]
   > 해당 볼륨을 오프 라인으로 전환하기 전에, 먼저 호스트에서 해당 볼륨을 오프라인으로 전환했는지 확인합니다. 호스트에서 볼륨을 오프라인으로 전환하지 않은 경우 잠재적으로 데이터가 손상될 수입니다.
   
4. **Backup 카탈로그** 탭으로 다시 이동하고 Backup 세트를 선택합니다. 마우스 오른쪽 단추를 클릭하고 상황에 맞는 메뉴에서 **복원**을 선택합니다.

    ![Backup 세트 목록](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. 확인하라는 메시지가 표시됩니다. 복원 정보를 검토하고 확인 확인란을 선택합니다.
   
    ![확인 페이지](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7. **복원**을 클릭 합니다. 그러면 **작업** 페이지에 액세스하여 볼 수 있는 복원 작업이 시작됩니다.

   ![확인 페이지](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. 복원이 완료되면 볼륨의 콘텐츠가 백업의 볼륨으로 바뀌는 것을 확인합니다.


## <a name="if-the-restore-fails"></a>복원에 실패하는 경우

복원 작업이 어떤 이유로든 실패하는 경우에 알림을 받습니다. 이 경우 백업이 여전히 유효한지 확인하려면 백업 목록을 새로 고칩니다. 백업이 유효하고 클라우드로부터 복원하는 경우 연결 문제를 초래할 수 있습니다.

복원 작업을 완료하려면 호스트에서 오프라인으로 볼륨을 가져오고 복원 작업을 다시 시도합니다. 복원 프로세스 중에 수행된 볼륨 데이터에 대한 수정은 손실됩니다.

## <a name="next-steps"></a>다음 단계
* [StorSimple 볼륨을 관리](storsimple-8000-manage-volumes-u2.md)하는 방법을 알아봅니다.
* [StorSimple 디바이스 관리자 서비스를 사용하여 StorSimple 디바이스를 관리](storsimple-8000-manager-service-administration.md)하는 방법을 알아봅니다.

