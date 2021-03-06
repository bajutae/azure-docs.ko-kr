---
title: 조건부 액세스를 사용 하 여 승인 된 클라이언트 앱-Azure Active Directory
description: Azure Active Directory에서 조건부 액세스를 사용 하 여 cloud app access에 승인 된 클라이언트 앱을 요구 하는 방법에 대해 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/04/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol, rosssmi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a215e2bb7d9d1cf9013414037383590456296cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79480898"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>방법: 조건부 액세스를 사용 하 여 cloud app access에 승인 된 클라이언트 앱 요구

사용자는 정기적으로 모바일 장치를 사용 하 여 개인 및 업무 작업을 모두 수행 합니다. 직원 들이 생산성을 높일 수 있도록 하는 동시에 잠재적으로 안전 하지 않은 응용 프로그램의 데이터 손실을 방지 하려고 합니다. 조건부 액세스를 통해 조직은 승인 된 (최신 인증 지원) 클라이언트 앱에 대 한 액세스를 제한할 수 있습니다.

이 문서에서는 Office 365, Exchange Online 및 SharePoint Online과 같은 리소스에 대 한 조건부 액세스 정책을 구성 하는 두 가지 시나리오를 제공 합니다.

- [시나리오 1: Office 365 앱에 승인 된 클라이언트 앱 필요](#scenario-1-office-365-apps-require-an-approved-client-app)
- [시나리오 2: Exchange Online 및 SharePoint Online에 승인 된 클라이언트 앱 필요](#scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app)

조건부 액세스에서이 기능은 승인 된 클라이언트 앱을 요구 하는 것으로 알려져 있습니다. 승인된 클라이언트 앱의 목록은 [승인된 클라이언트 앱 요구 사항](concept-conditional-access-grant.md#require-approved-client-app)을 참조하세요.

> [!NOTE]
> IOS 및 Android 장치에 승인 된 클라이언트 앱을 요구 하기 위해 이러한 장치는 먼저 Azure AD에 등록 해야 합니다.

## <a name="scenario-1-office-365-apps-require-an-approved-client-app"></a>시나리오 1: Office 365 앱에 승인 된 클라이언트 앱 필요

이 시나리오에서 Contoso는 Outlook mobile, OneDrive 및 Microsoft 팀과 같은 승인 된 클라이언트 앱을 사용 하는 경우 모바일 장치를 사용 하는 사용자가 모든 Office 365 서비스에 액세스할 수 있도록 결정 했습니다. 모든 사용자는 이미 Azure AD 자격 증명을 사용 하 여 로그인 하 고 Azure AD Premium P1 또는 P2 및 Microsoft Intune를 포함 하는 라이선스가 할당 되어 있습니다.

조직에서는 모바일 장치에서 승인 된 클라이언트 앱을 사용 하도록 요구 하기 위해 다음 세 단계를 완료 해야 합니다.

**1 단계: Exchange Online에 액세스할 때 승인 된 클라이언트 응용 프로그램을 사용 해야 하는 Android 및 iOS 기반 최신 인증 클라이언트에 대 한 정책입니다.**

1. 전역 관리자, 보안 관리자 또는 조건부 액세스 관리자 권한으로 **Azure Portal** 에 로그인 합니다.
1. **Azure Active Directory** > **Security**보안 > **조건부 액세스**로 이동 합니다.
1. **새 정책**을 선택합니다.
1. 정책에 이름을 지정 합니다. 조직에서 정책 이름에 대해 의미 있는 표준을 만드는 것이 좋습니다.
1. **할당**아래에서 **사용자 및 그룹** 을 선택 합니다.
   1. **포함**아래에서이 정책을 적용 하려는 **모든 사용자** 또는 특정 **사용자 및 그룹** 을 선택 합니다. 
   1. **완료**를 선택합니다.
1. **클라우드 앱 또는 작업** > **포함**아래에서 **Office 365 (미리 보기)** 를 선택 합니다.
1. **조건**아래에서 **장치 플랫폼**을 선택 합니다.
   1. **구성** 을 **예**로 설정 합니다.
   1. **Android** 및 **iOS**를 포함 합니다.
1. **조건**아래에서 **클라이언트 앱 (미리 보기)** 을 선택 합니다.
   1. **구성** 을 **예**로 설정 합니다.
   1. **모바일 앱 및 데스크톱 클라이언트** 및 **최신 인증 클라이언트**를 선택합니다.
1. **액세스 제어** > **권한 부여**에서 **액세스 허용**, **승인 된 클라이언트 앱 필요**를 선택 하 고 **선택**을 선택 합니다.
1. 설정을 확인 하 고 **정책 사용** 을 **켜기**로 설정 합니다.
1. **만들기** 를 선택 하 여 정책을 만들고 사용 하도록 설정 합니다.

**2 단계: ActiveSync (EAS)를 사용 하 여 Exchange Online에 대 한 Azure AD 조건부 액세스 정책 구성**

1. **Azure Active Directory** > **Security**보안 > **조건부 액세스**로 이동 합니다.
1. **새 정책**을 선택합니다.
1. 정책에 이름을 지정 합니다. 조직에서 정책 이름에 대해 의미 있는 표준을 만드는 것이 좋습니다.
1. **할당**아래에서 **사용자 및 그룹** 을 선택 합니다.
   1. **포함**아래에서이 정책을 적용 하려는 **모든 사용자** 또는 특정 **사용자 및 그룹** 을 선택 합니다. 
   1. **완료**를 선택합니다.
1. **클라우드 앱 또는 작업** > **포함**아래에서 **Office 365 Exchange Online**을 선택 합니다.
1. **조건**:
   1. **클라이언트 앱 (미리 보기)**:
      1. **구성** 을 **예**로 설정 합니다.
      1. **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택 합니다.
1. **액세스 제어** > **권한 부여**에서 **액세스 허용**, **승인 된 클라이언트 앱 필요**를 선택 하 고 **선택**을 선택 합니다.
1. 설정을 확인 하 고 **정책 사용** 을 **켜기**로 설정 합니다.
1. **만들기** 를 선택 하 여 정책을 만들고 사용 하도록 설정 합니다.

**3 단계: iOS 및 Android 클라이언트 응용 프로그램에 대 한 Intune 앱 보호 정책을 구성 합니다.**

Android 및 iOS 용 앱 보호 정책을 만드는 단계에 대해서는 [앱 보호 정책을 만들고 할당 하는 방법](/intune/apps/app-protection-policies)문서를 검토 합니다. 

## <a name="scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app"></a>시나리오 2: Exchange Online 및 SharePoint Online에 승인 된 클라이언트 앱 필요

이 시나리오에서 Contoso는 Outlook mobile과 같은 승인 된 클라이언트 앱을 사용 하는 경우 사용자가 모바일 장치에서 메일 및 SharePoint 데이터에만 액세스할 수 있도록 결정 했습니다. 모든 사용자는 이미 Azure AD 자격 증명을 사용 하 여 로그인 하 고 Azure AD Premium P1 또는 P2 및 Microsoft Intune를 포함 하는 라이선스가 할당 되어 있습니다.

조직에서는 모바일 장치 및 Exchange ActiveSync 클라이언트에서 승인 된 클라이언트 앱을 사용 하도록 요구 하기 위해 다음 세 단계를 완료 해야 합니다.

**1 단계: Exchange Online 및 SharePoint Online에 액세스할 때 승인 된 클라이언트 응용 프로그램을 사용 해야 하는 Android 및 iOS 기반 최신 인증 클라이언트에 대 한 정책입니다.**

1. 전역 관리자, 보안 관리자 또는 조건부 액세스 관리자 권한으로 **Azure Portal** 에 로그인 합니다.
1. **Azure Active Directory** > **Security**보안 > **조건부 액세스**로 이동 합니다.
1. **새 정책**을 선택합니다.
1. 정책에 이름을 지정 합니다. 조직에서 정책 이름에 대해 의미 있는 표준을 만드는 것이 좋습니다.
1. **할당**아래에서 **사용자 및 그룹** 을 선택 합니다.
   1. **포함**아래에서이 정책을 적용 하려는 **모든 사용자** 또는 특정 **사용자 및 그룹** 을 선택 합니다. 
   1. **완료**를 선택합니다.
1. **클라우드 앱 또는 작업** > **포함**아래에서 **office 365 Exchange online** 및 **office 365 SharePoint online**을 선택 합니다.
1. **조건**아래에서 **장치 플랫폼**을 선택 합니다.
   1. **구성** 을 **예**로 설정 합니다.
   1. **Android** 및 **iOS**를 포함 합니다.
1. **조건**아래에서 **클라이언트 앱 (미리 보기)** 을 선택 합니다.
   1. **구성** 을 **예**로 설정 합니다.
   1. **모바일 앱 및 데스크톱 클라이언트** 및 **최신 인증 클라이언트**를 선택합니다.
1. **액세스 제어** > **권한 부여**에서 **액세스 허용**, **승인 된 클라이언트 앱 필요**를 선택 하 고 **선택**을 선택 합니다.
1. 설정을 확인 하 고 **정책 사용** 을 **켜기**로 설정 합니다.
1. **만들기** 를 선택 하 여 정책을 만들고 사용 하도록 설정 합니다.

**2 단계: 승인 된 클라이언트 앱을 사용 해야 하는 Exchange ActiveSync 클라이언트에 대 한 정책입니다.**

1. **Azure Active Directory** > **Security**보안 > **조건부 액세스**로 이동 합니다.
1. **새 정책**을 선택합니다.
1. 정책에 이름을 지정 합니다. 조직에서 정책 이름에 대해 의미 있는 표준을 만드는 것이 좋습니다.
1. **할당**아래에서 **사용자 및 그룹** 을 선택 합니다.
   1. **포함**아래에서이 정책을 적용 하려는 **모든 사용자** 또는 특정 **사용자 및 그룹** 을 선택 합니다. 
   1. **완료**를 선택합니다.
1. **클라우드 앱 또는 작업** > **포함**아래에서 **Office 365 Exchange Online**을 선택 합니다.
1. **조건**:
   1. **클라이언트 앱 (미리 보기)**:
      1. **구성** 을 **예**로 설정 합니다.
      1. **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택 합니다.
1. **액세스 제어** > **권한 부여**에서 **액세스 허용**, **승인 된 클라이언트 앱 필요**를 선택 하 고 **선택**을 선택 합니다.
1. 설정을 확인 하 고 **정책 사용** 을 **켜기**로 설정 합니다.
1. **만들기** 를 선택 하 여 정책을 만들고 사용 하도록 설정 합니다.

**3 단계: iOS 및 Android 클라이언트 응용 프로그램에 대 한 Intune 앱 보호 정책을 구성 합니다.**

Android 및 iOS 용 앱 보호 정책을 만드는 단계에 대해서는 [앱 보호 정책을 만들고 할당 하는 방법](/intune/apps/app-protection-policies)문서를 검토 합니다. 

## <a name="next-steps"></a>다음 단계

[조건부 액세스란?](overview.md)

[조건부 액세스 구성 요소](concept-conditional-access-policies.md)

[일반 조건부 액세스 정책](concept-conditional-access-policy-common.md)
