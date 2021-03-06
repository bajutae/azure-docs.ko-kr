---
title: Azure Migrate Server Migration을 사용 하 여 VMware 마이그레이션 옵션 선택 | Microsoft Docs
description: Azure Migrate Server Migration을 사용 하 여 VMware Vm을 Azure로 마이그레이션하기 위한 옵션에 대 한 개요를 제공 합니다.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 52e7103ea3ebcd83369a866cc3f75b0bf0e889a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76028722"
---
# <a name="select-a-vmware-migration-option"></a>VMware 마이그레이션 옵션 선택

Azure Migrate 서버 마이그레이션 도구를 사용하여 VMware VM을 Azure로 마이그레이션할 수 있습니다. 이 도구는 VMware VM 마이그레이션을 위해 다음 두 가지 옵션을 제공합니다.

- 에이전트 없는 복제를 통한 마이그레이션. VM에 아무 것도 설치하지 않고 해당 VM을 마이그레이션합니다.
- 복제용 에이전트를 통한 마이그레이션. VM에 복제용 에이전트를 설치합니다.




## <a name="compare-migration-methods"></a>마이그레이션 방법 비교

이러한 선택한 비교를 사용 하 여 사용할 방법을 결정 하는 데 도움을 줍니다. [에이전트 없는 에이전트와 에이전트 기반](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) 마이그레이션에 [대 한 전체](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) 지원 요구 사항도 검토할 수 있습니다.

**설정** | **에이전트 없음** | **에이전트 기반**
--- | --- | ---
**Azure 권한** | Azure Migrate 프로젝트를 만들고 Azure Migrate 어플라이언스를 배포할 때 만든 Azure AD 앱을 등록 하려면 권한이 필요 합니다. | Azure 구독에 대 한 참가자 권한이 필요 합니다. 
**동시 복제** | VCenter Server에서 최대 100 Vm을 동시에 복제할 수 있습니다.<br/> 마이그레이션을 위해 Vm이 50 개 이상인 경우 Vm의 여러 일괄 처리를 만듭니다.<br/> 한 번에 더 많은 복제는 성능에 영향을 줍니다. | 해당 없음
**어플라이언스 배포** | [Azure Migrate 어플라이언스](migrate-appliance.md) 는 온-프레미스에 배포 됩니다. | [Azure Migrate 복제 어플라이언스](migrate-replication-appliance.md) 는 온-프레미스에 배포 됩니다.
**Site Recovery 호환성** | 형식이. | Site Recovery를 사용 하 여 컴퓨터에 대 한 복제를 설정한 경우 Azure Migrate 서버 마이그레이션에 복제할 수 없습니다.
**대상 디스크** | 관리 디스크 | 관리 디스크
**디스크 제한** | OS 디스크: 2tb<br/><br/> 데이터 디스크: 4tb<br/><br/> 최대 디스크: 60 | OS 디스크: 2tb<br/><br/> 데이터 디스크: 8TB<br/><br/> 최대 디스크: 63
**통과 디스크** | 지원되지 않음 | 지원됨
**UEFI 부팅** | 지원되지 않음 | Azure에서 마이그레이션된 VM은 자동으로 BIOS 부팅 VM으로 변환 됩니다.<br/><br/> OS 디스크에는 최대 4 개의 파티션이 있어야 하 고 볼륨은 NTFS로 포맷 되어야 합니다.


## <a name="deployment-steps-comparison"></a>배포 단계 비교

제한 사항을 검토 한 후 각 솔루션을 배포 하는 단계를 이해 하면 선택할 옵션을 결정 하는 데 도움이 될 수 있습니다.

**Task** | **세부 정보** |**에이전트 없음** | **에이전트 기반**
--- | --- | --- | ---
**평가** | 마이그레이션 전에 서버를 평가 합니다.  평가는 선택 사항입니다. 마이그레이션하기 전에 컴퓨터를 평가 하는 것이 좋습니다. <br/><br/> 평가를 위해 Azure Migrate는 Vm을 검색 하 고 평가 하는 경량 어플라이언스를 설정 합니다. | 평가 후 에이전트 없는 마이그레이션을 실행 하는 경우 평가에 대해 설정 된 동일한 Azure Migrate 어플라이언스는 에이전트 없는 마이그레이션에 사용 됩니다.  |  평가 후에 에이전트 기반 마이그레이션을 실행 하는 경우 평가를 위해 설정 된 어플라이언스는 에이전트 없는 마이그레이션 중에 사용 되지 않습니다. 어플라이언스를 그대로 두거나 추가 검색 및 평가를 수행 하지 않으려는 경우 제거할 수 있습니다.
**마이그레이션을 위해 VMware 서버 및 Vm 준비** | VMware 서버 및 Vm에 대 한 다양 한 설정을 구성 합니다. | 필수 | 필수
**Server Migration 도구 추가** | Azure Migrate 프로젝트에 Azure Migrate 서버 마이그레이션 도구를 추가 합니다. | 필수 | 필수
**Azure Migrate 어플라이언스 배포** | VM 검색 및 평가를 위해 VMware VM에서 경량 어플라이언스를 설정 합니다. | 필수 | 필요하지 않음.
**Vm에 모바일 서비스 설치** | 복제 하려는 각 VM에 모바일 서비스를 설치 합니다. | 필요하지 않음 | 필수
**Azure Migrate Server Migration replication 어플라이언스 배포** | VMware VM에서 어플라이언스를 설정 하 여 vm을 검색 하 고, Vm에서 실행 되는 모바일 서비스와 Azure Migrate 서버 마이그레이션을 연결 합니다. | 필요하지 않음 | 필수
**Vm을 복제**합니다. VM 복제를 사용하도록 설정. | 복제 설정을 구성 하 고 복제할 Vm을 선택 합니다. | 필수 | 필수
**테스트 마이그레이션 실행** | 테스트 마이그레이션을 실행하여 모든 것이 예상대로 작동하는지 확인합니다. | 필수 | 필수
**전체 마이그레이션 실행** | Vm을 마이그레이션합니다. | 필수 | 필수




## <a name="next-steps"></a>다음 단계

에이전트 없는 마이그레이션을 사용 하 여 [VMware vm을 마이그레이션합니다](tutorial-migrate-vmware.md) .



