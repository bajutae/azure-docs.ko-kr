---
title: 애플리케이션에 로그인할 때 예기치 않은 동의 확인 프롬프트 | Microsoft Docs
description: 예기치 않게 사용자에게 Azure AD와 통합한 애플리케이션에 대한 동의 확인 프롬프트를 표시하는 경우 문제를 해결하는 방법
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83d043ecef152f977437e21e2caec40d1c40ce0d
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2020
ms.locfileid: "65781143"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>애플리케이션에 로그인할 때 예기치 않은 동의 확인 프롬프트

Azure Active Directory와 통합되는 많은 애플리케이션을 실행하기 위해 다양한 리소스에 대한 권한이 필요합니다. 또한 이러한 리소스가 Azure Active Directory와 통합되면 Azure AD 동의 프레임워크를 사용하여 액세스하기 위한 사용 권한을 요청합니다. 

이로 인해 처음으로 애플리케이션을 사용할 때 동의 확인 프롬프트가 표시되며 이 작업은 한 번만 수행하면 됩니다. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>사용자에게 동의 확인 프롬프트를 표시하는 시나리오

다양한 시나리오에서 추가 프롬프트를 예상할 수 있습니다.

* 응용 프로그램에 필요한 사용 권한 집합이 변경 되었습니다.

* 원래 애플리케이션에 동의한 사용자는 관리자가 아니었고 지금 (관리자가 아닌) 다른 사용자는 애플리케이션을 처음으로 사용합니다.

* 원래 애플리케이션에 동의한 사용자는 관리자였지만 전체 조직을 대신하여 동의하지는 않았습니다.

* 애플리케이션에서 [증분 및 동적 동의](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent)를 사용하여 처음으로 동의를 받은 후에 추가 권한을 요청합니다. 이것은 추가 애플리케이션의 선택적 기능이 초기 기능에 필요한 것 이상의 권한을 필요로 할 경우에 종종 사용됩니다.

* 동의가 처음에 부여된 후에 해지되었습니다.

* 개발자는 동의를 사용할 때마다 동의 확인 프롬프트를 요구하도록 애플리케이션을 구성했습니다(참고: 모범 사례가 아님).

## <a name="next-steps"></a>다음 단계

-   [Azure Active Directory에서 앱, 사용 권한 및 동의(v1.0 엔드포인트)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Azure Active Directory의 범위, 사용 권한 및 동의(v2.0 엔드포인트)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


