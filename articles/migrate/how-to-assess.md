---
title: Azure Migrate에서 평가 도구 추가
description: Azure Migrate에서 평가 도구를 추가 하는 방법에 대해 알아봅니다.
ms.topic: how-to
ms.date: 04/26/2020
ms.openlocfilehash: 7e7d7cf26cab30cd7449e4643fdfb59faece47f2
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82901050"
---
# <a name="add-assessment-tools"></a>평가 도구 추가

이 문서에서는 [Azure Migrate](migrate-overview.md)에서 평가 도구를 추가 하는 방법을 설명 합니다. 

- 평가 도구를 추가 하려는 데 아직 Azure Migrate 프로젝트가 없는 경우이 [문서](how-to-add-tool-first-time.md)를 따르세요.
- ISV 도구 또는 Movere를 추가한 경우에는이 도구를 사용 하 여 작업을 준비 하는 [단계를 수행](prepare-isv-movere.md)합니다.

## <a name="select-an-assessment-scenario"></a>평가 시나리오 선택

1. Azure Migrate 프로젝트에서 **개요**를 클릭합니다.
2. 사용할 평가 시나리오를 선택 합니다.

    - Azure로의 마이그레이션을 위한 컴퓨터 및 작업을 검색 하 고 평가 하려면 **서버 평가 및 마이그레이션**을 선택 합니다.
    - 온-프레미스 SQL 컴퓨터를 평가 하려면 **데이터베이스 평가 및 마이그레이션**을 선택 합니다.
    - 온-프레미스 웹 앱을 평가 하려면 **웹 앱 평가 및 마이그레이션**을 선택 합니다.

    ![평가 시나리오](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>서버 평가 도구 선택 

1. **서버 평가 및 마이그레이션을**클릭 합니다.
2. **Azure Migrate 서버**에서 평가 도구를 추가 하지 않은 경우 평가 도구에서 평가 **도구를** **추가 하려면 여기를 클릭**하세요 .를 선택 합니다. 평가 도구를 이미 추가한 경우 **더 많은 평가 도구 추가**에서 **변경**을 선택 합니다.

    > [!NOTE]
    > 다른 프로젝트로 이동 해야 하는 경우 **다른 마이그레이션 프로젝트에 대 한 세부 정보 보기**옆에 있는 **Azure Migrate 서버**에서 **여기**를 클릭 하십시오 .를 클릭 합니다.

3. **Azure Migrate**에서 사용 하려는 평가 도구를 선택 합니다.

    - Azure Migrate Server 평가를 사용 하는 경우 Azure Migrate 프로젝트에서 직접 평가를 설정 하 고, 실행 하 고, 볼 수 있습니다.
    - 다른 평가 도구를 사용 하는 경우 해당 사이트에 대해 제공 된 링크로 이동 하 여 제공 된 지침에 따라 평가를 실행 합니다.


## <a name="select-a-database-assessment-tool"></a>데이터베이스 평가 도구 선택

1. **데이터베이스 평가 및 마이그레이션을** 클릭 합니다.
2. **데이터베이스**에서 **도구 추가**를 클릭 합니다.
3. 도구 추가 > **평가 도구 선택**에서 데이터베이스를 평가 하는 데 사용할 도구를 선택 합니다.

## <a name="select-a-web-app-assessment-tool"></a>웹 앱 평가 도구 선택

1. **웹 앱 평가 및 마이그레이션을**클릭 합니다.
2. Azure App Service에 대 한 마이그레이션 도구의 링크를 따릅니다. 마이그레이션 도구를 사용 하 여 다음을 수행 합니다.

    - **온라인으로 앱 평가**: Azure App Service Migration Assistant를 사용 하 여 온라인 URL을 사용 하 여 앱을 평가할 수 있습니다.
    - **.Net/php**: 내부 .NET 및 php 앱의 경우 Migration Assistant를 다운로드 하 여 실행할 수 있습니다.



## <a name="next-steps"></a>다음 단계

[VMware](tutorial-prepare-vmware.md) Vm, [hyper-v](tutorial-prepare-hyper-v.md)또는 [물리적 서버](tutorial-prepare-physical.md) 에 대 한 Azure Migrate 서버 평가를 사용 하 여 평가를 사용해 보세요.
