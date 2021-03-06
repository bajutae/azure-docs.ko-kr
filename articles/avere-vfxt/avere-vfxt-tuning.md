---
title: Avere vFXT 클러스터 튜닝 - Azure
description: Avere vFXT for Azure의 성능을 최적화하기 위한 사용자 지정 설정에 대해 간략히 설명합니다.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: df20f050ff87fdb59a3e5cca373098240f8bfbb9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76152938"
---
# <a name="cluster-tuning"></a>클러스터 튜닝

대부분의 vFXT 클러스터는 사용자 지정 성능 설정에서 이점을 얻을 수 있습니다. 클러스터는 이러한 설정을 통해 특정 워크플로, 데이터 세트 및 도구에서 가장 효율적으로 작동합니다.

이 사용자 지정은 Avere 제어판에서 사용할 수 없는 기능을 구성 하는 작업을 포함할 수 있으므로 지원 담당자의 도움을 받아야 합니다.

이 섹션에서는 수행할 수 있는 몇 가지 사용자 지정 튜닝에 대해 설명 합니다.

## <a name="general-optimizations"></a>일반 최적화

이러한 변경은 데이터 세트 품질 또는 워크플로 스타일에 따라 권장될 수 있습니다.

* 쓰기 집약적 작업이 많은 경우 쓰기 캐시의 크기를 기본값인 20%에서 늘립니다.
* 데이터 세트에 작은 파일이 많이 포함된 경우 클러스터 캐시의 파일 수 제한을 늘립니다.
* 두 저장소 간에 데이터를 복사하거나 이동하는 작업이 포함된 경우 데이터 이동에 사용되는 스레드 수를 조정합니다.
  * 속도를 높이려면 사용되는 병렬 스레드 수를 늘릴 수 있습니다.
  * 백 엔드 스토리지 볼륨이 오버로드되는 경우 사용되는 병렬 스레드 수를 줄이는 것이 좋습니다.
* 클러스터에서 NFSv4 ACL을 사용하는 코어 파일러에 대한 데이터를 캐시하는 경우 액세스 모드 캐싱을 사용하도록 설정하여 특정 클라이언트에 대한 파일 권한 부여를 간소화합니다.

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>클라우드 NAS 또는 클라우드 게이트웨이 최적화

클라우드 NAS 또는 게이트웨이 시나리오에서 vFXT 클러스터는 클라우드 컨테이너에 대 한 NAS 스타일의 액세스를 제공 합니다. VFXT 클러스터와 클라우드 저장소 간의 데이터 속도를 높이려면 담당자가 캐시에서 저장소 볼륨으로 데이터를 보다 적극적으로 푸시하는 설정을 변경 하는 것이 좋습니다. 다음은 그 예입니다.

* 클러스터와 스토리지 컨테이너 간의 TCP 연결 수를 늘립니다.

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>클라우드 버스팅 또는 하이브리드 WAN 최적화

클라우드 버스트 시나리오 또는 하이브리드 저장소 WAN 최적화 시나리오에서 vFXT 클러스터는 클라우드와 온-프레미스 하드웨어 저장소 간의 통합을 제공 합니다. 다음과 같이 변경 하면 도움이 될 수 있습니다.

* 클러스터와 코어 파일러 간에 허용되는 TCP 연결 수를 늘립니다.
* 원격 코어 파일러에 WAN 최적화 설정을 사용합니다(이 설정은 다른 Azure 지역의 원격 온-프레미스 파일러 또는 클라우드 코어 파일러에 사용할 수 있음).
* TCP 소켓 버퍼 크기를 늘립니다.<sup>*</sup>
* 중복 캐시 된 파일을 줄이기 위해 "항상 전달" 설정을 사용 하도록 설정<sup>*</sup>

<sup>*</sup>이러한 조정은 워크 로드 및 성능 요구 사항에 따라 일부 시스템에는 적용 되지 않을 수 있습니다.

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Avere vFXT for Azure 최적화 도움말

지원 직원에 게 이러한 최적화에 대해 문의 하려면 시스템에 대 한 [도움말 보기](avere-vfxt-open-ticket.md)에 설명 된 절차를 따르십시오.
