---
title: 그룹을 사용 하 여 SaaS 앱에 대 한 액세스 관리-Azure AD | Microsoft Docs
description: Azure Active Directory에서 그룹을 사용하여 Azure Active Directory와 통합되는 SaaS 애플리케이션에 대한 액세스 권한을 할당하는 방법입니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51375f057543c86fe021822eb9722ffd1be16804
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74026849"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>그룹을 사용하여 SaaS 애플리케이션에 대한 액세스 관리

Azure AD Premium 라이선스 계획과 함께 Azure Active Directory (Azure AD)를 사용 하 여 그룹을 사용 하 여 Azure AD와 통합 된 SaaS 응용 프로그램에 대 한 액세스 권한을 할당할 수 있습니다. 예를 들어 5가지 SaaS 애플리케이션을 사용하는 마케팅 부서에 대해 액세스 권한을 할당하려는 경우 마케팅 부서의 사용자가 포함된 그룹을 만든 다음 마케팅 부서에 필요한 이 5가지 SaaS 애플리케이션에 해당 그룹을 할당할 수 있습니다. 이러한 방식으로 한 곳에서 마케팅 부서의 멤버 자격을 관리하여 시간을 절약할 수 있습니다. 사용자는 마케팅 그룹 멤버로 추가되는 경우 애플리케이션에 할당되고 마케팅 그룹에서 제거되면 애플리케이션에서 할당이 제거됩니다. 이 기능은 Azure AD 애플리케이션 갤러리 내에서 추가할 수 있는 수많은 애플리케이션과 함께 사용할 수 있습니다.

> [!IMPORTANT]
> Azure AD Premium 평가판을 시작 하거나 Azure AD Premium 라이선스 계획을 구매한 후에만이 기능을 사용할 수 있습니다.
> 그룹 기반 할당은 보안 그룹에 대해서만 지원 됩니다.
> 중첩 그룹 구성원은 이번 애플리케이션에 대한 그룹 기반 할당에서 지원되지 않습니다.

## <a name="to-assign-access-for-a-user-or-group-to-a-saas-application"></a>사용자 또는 그룹에 대해 SaaS 애플리케이션 액세스 권한을 할당하려면

1. [Azure AD 관리 센터](https://aad.portal.azure.com)에서 **Enterprise 애플리케이션**을 선택합니다.
2. 애플리케이션 갤러리에서 추가한 애플리케이션을 선택하여 엽니다.
3. **사용자 및 그룹**을 선택한 다음 **사용자 추가**를 선택합니다.
4. **할당 추가**에서 **사용자 및 그룹**을 선택하여 **사용자 및 그룹** 선택 목록을 엽니다.
6. 사용자가 원하는 만큼 그룹 또는 사용자를 선택한 다음 **선택**을 클릭하거나 눌러 **할당 추가** 목록에 추가합니다. 이 단계에서 사용자에게 역할을 할당할 수도 있습니다.
7. **할당**을 선택하여 선택한 엔터프라이즈 애플리케이션에 사용자 또는 그룹을 할당합니다.

## <a name="next-steps"></a>다음 단계
이러한 문서는 Azure Active Directory에 대한 추가 정보를 제공합니다.

* [Azure Active Directory 그룹을 사용 하 여 리소스에 대 한 액세스 관리](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory의 애플리케이션 관리](../manage-apps/what-is-application-management.md)
* [그룹 설정을 구성하는 Azure Active Directory cmdlets](groups-settings-cmdlets.md)
* [Azure Active Directory란?](../fundamentals/active-directory-whatis.md)
* [Azure Active Directory와 온-프레미스 ID 통합](../hybrid/whatis-hybrid-identity.md)
