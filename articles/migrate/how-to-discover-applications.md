---
title: Azure Migrate를 사용 하 여 온-프레미스 서버에서 앱, 역할 및 기능 검색
description: Azure Migrate Server 평가를 사용 하 여 온-프레미스 서버에서 앱, 역할 및 기능을 검색 하는 방법을 알아봅니다.
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: ff9f5489b513cd1405e6b093d7537e4cbcead041
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744621"
---
# <a name="discover-machine-apps-roles-and-features"></a>컴퓨터 앱, 역할 및 기능 검색

이 문서에서는 Azure Migrate: 서버 평가를 사용 하 여 온-프레미스 서버에서 응용 프로그램, 역할 및 기능을 검색 하는 방법을 설명 합니다.

앱 인벤토리를 검색 하 고 온-프레미스 컴퓨터에서 실행 되는 역할/기능을 사용 하면 워크 로드에 맞게 조정 된 Azure에 대 한 마이그레이션 경로를 파악 하 고 계획할 수 있습니다.

> [!NOTE]
> 앱 검색은 현재 VMware Vm에 대 한 미리 보기 상태 이며 검색 으로만 제한 됩니다. 앱 기반 평가는 아직 제공 되지 않습니다. 온-프레미스 VMware Vm, Hyper-v Vm 및 물리적 서버에 대 한 컴퓨터 기반 평가

Azure Migrate를 사용 하는 앱 검색: 서버 평가는 에이전트 없이 수행 됩니다. 컴퓨터 및 Vm에는 아무것도 설치 되지 않습니다. 서버 평가는 Azure Migrate 어플라이언스를 사용 하 여 컴퓨터 게스트 자격 증명과 함께 검색을 수행 합니다. 어플라이언스는 VMware Api를 사용 하 여 VMware 컴퓨터에 원격으로 액세스 합니다.


## <a name="before-you-start"></a>시작하기 전에

1. Azure Migrate 프로젝트를 [만들었는지](how-to-add-tool-first-time.md) 확인 합니다.
2. Azure Migrate: 서버 평가 도구를 프로젝트에 [추가](how-to-assess.md) 했는지 확인 합니다.
4. Azure Migrate 어플라이언스를 사용 하 여 VMware Vm을 검색 하 고 평가 하기 위한 [vmware 요구 사항을](migrate-support-matrix-vmware.md#vmware-requirements) 확인 하세요.
5. Azure Migrate 어플라이언스를 배포 하기 위한 [요구 사항을](migrate-appliance.md) 확인 합니다.
6. 응용 프로그램 검색에 대 한 [지원 및 요구 사항을 확인](migrate-support-matrix-vmware.md#application-discovery) 합니다.



## <a name="deploy-the-azure-migrate-appliance"></a>Azure Migrate 어플라이언스 배포

1. Azure Migrate 어플라이언스를 배포 하기 위한 요구 사항을 [검토](migrate-appliance.md#appliance---vmware) 합니다.
2. 어플라이언스에서 [공용](migrate-appliance.md#public-cloud-urls) 및 [정부 클라우드에서](migrate-appliance.md#government-cloud-urls)액세스 해야 하는 Azure url을 검토 합니다.
3. 검색 및 평가 중에 어플라이언스에서 수집하는 [데이터를 검토](migrate-appliance.md#collected-data---vmware)합니다.
4. 어플라이언스에 대한 포트 액세스 요구 사항에 [유의](migrate-support-matrix-vmware.md#port-access)하세요.
5. [Azure Migrate 어플라이언스를 배포](how-to-set-up-appliance-vmware.md) 하 여 검색을 시작 합니다. 어플라이언스를 배포 하려면 OVA 템플릿을 다운로드 하 고 VMware로 가져와 vmware VM으로 어플라이언스를 만듭니다. 어플라이언스를 구성한 다음 Azure Migrate에 등록 합니다.
6. 어플라이언스를 배포할 때 연속 검색을 시작 하려면 다음을 지정 합니다.
    - 연결 하려는 vCenter Server의 이름입니다.
    - VCenter Server에 연결 하기 위해 어플라이언스에 대해 만든 자격 증명입니다.
    - Windows/Linux Vm에 연결 하기 위해 어플라이언스에 대해 만든 계정 자격 증명입니다.

어플라이언스를 배포 하 고 자격 증명을 제공한 후 어플라이언스는 앱, 기능 및 역할의 검색과 함께 VM 메타 데이터 및 성능 데이터의 연속 검색을 시작 합니다.  앱 검색 기간은 보유 하 고 있는 Vm 수에 따라 달라 집니다. 500 Vm의 앱 검색은 일반적으로 1 시간 정도 걸립니다.

## <a name="prepare-a-user-account"></a>사용자 계정 준비

검색에 사용할 계정을 만들고 어플라이언스에 추가 합니다.

### <a name="create-a-user-account-for-discovery"></a>검색을 위한 사용자 계정 만들기

서버 평가가 검색을 위해 VM에 액세스할 수 있도록 사용자 계정을 설정 합니다. 계정 요구 사항에 [대해 알아봅니다](migrate-support-matrix-vmware.md#application-discovery) .


### <a name="add-the-user-account-to-the-appliance"></a>사용자 계정을 어플라이언스에 추가 합니다.

사용자 계정을 어플라이언스에 추가 합니다.

1. 어플라이언스 관리 앱을 엽니다. 
2. **VCenter 세부 정보 제공** 패널로 이동 합니다.
3. **검색 응용 프로그램 및 vm에 대 한 종속성**에서 **자격 증명 추가** 를 클릭 합니다.
3. **운영 체제**를 선택 하 고 계정의 이름을 입력 하 고 **사용자 이름**/**암호** 를 입력 합니다.
6. **저장**을 클릭합니다.
7. **저장 및 검색 시작을**클릭 합니다.

    ![VM 사용자 계정 추가](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)





## <a name="review-and-export-the-inventory"></a>인벤토리 검토 및 내보내기

검색이 종료 된 후 앱 검색을 위한 자격 증명을 제공한 경우 Azure Portal에서 앱 인벤토리를 검토 하 고 내보낼 수 있습니다.

1. **Azure Migrate 서버** > **Azure Migrate: 서버 평가**에서 표시 된 수를 클릭 하 여 검색 된 **서버** 페이지를 엽니다.

    > [!NOTE]
    > 또한이 단계에서는 검색 된 컴퓨터에 대 한 종속성 매핑을 선택적으로 설정 하 여 평가 하려는 컴퓨터에서 종속성을 시각화할 수 있습니다. [자세한 정보를 알아보세요](how-to-create-group-machine-dependencies.md).

2. **검색 된 응용 프로그램**에서 표시 된 수를 클릭 합니다.
3. **응용 프로그램 인벤토리에서**검색 된 앱, 역할 및 기능을 검토할 수 있습니다.
4. 인벤토리를 내보내려면 **검색 된 서버**에서 **앱 인벤토리 내보내기**를 클릭 합니다.

앱 인벤토리는 Excel 형식으로 내보내고 다운로드 됩니다. **응용 프로그램 인벤토리** 시트에는 모든 컴퓨터에서 검색 된 모든 앱이 표시 됩니다.

## <a name="next-steps"></a>다음 단계

- 검색 된 서버의 리프트 및 시프트 마이그레이션에 대 한 [평가를 만듭니다](how-to-create-assessment.md) .
- [Azure Migrate: 데이터베이스 평가](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017)를 사용 하 여 SQL Server 데이터베이스를 평가 합니다.
