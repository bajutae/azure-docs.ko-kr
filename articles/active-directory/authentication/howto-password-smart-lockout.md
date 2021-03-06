---
title: 스마트 잠금을 사용 하 여 공격 방지-Azure Active Directory
description: Azure Active Directory 스마트 잠금을 통해 암호를 추측하려는 무차별 암호 대입 공격으로부터 조직을 보호합니다.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61ae942ed189dc4245a9a0b282daf4cad5323536
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80652574"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory 스마트 잠금

스마트 잠금 기능을 사용 하면 사용자의 암호를 추측 하려는 잘못 된 행위자를 잠그거나 무차별 암호 대입 메서드를 사용 하 여 가져올 수 있습니다. 이 기능은 유효한 사용자로부터 오는 로그인을 인식하고 이를 공격자 및 기타 알 수 없는 원본 중 하나와 다르게 취급합니다. 스마트 잠금은 사용자가 계정에 계속 액세스하여 생산성을 유지할 수 있게 해주면서도 공격자를 차단합니다.

스마트 잠금 기능은 기본적으로 로그인 실패가 10회 발생하면 1분 동안 계정에 로그인 시도를 차단합니다. 후속 로그인 시도가 실패할 때마다 계정이 다시 잠기는데, 처음에는 1분간 잠기고 그 이후에는 더 길게 잠깁니다.

스마트 잠금 기능은 동일한 암호에 대해 잠금 카운터가 증가하는 것을 방지하기 위해 마지막 세 개의 잘못된 암호 해시를 추적합니다. 동일한 잘못된 암호를 여러 번 입력하면 이 동작으로 인해 계정이 잠기지 않습니다.

 > [!NOTE]
 > 통과 인증을 사용하도록 설정한 고객은 인증이 클라우드가 아닌 온-프레미스에서 발생하므로 해시 추적 기능을 사용할 수 없습니다.

AD FS 2016 및 AF FS 2019을 사용 하는 페더레이션된 배포는 [엑스트라넷 잠금 및 엑스트라넷 스마트 잠금 AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)를 사용 하 여 비슷한 이점을 제공할 수 있습니다.

스마트 잠금은 보안 및 유용성의 적절한 혼합을 제공하는 기본 설정으로 모든 Azure AD 고객에 대해 항상 활성 상태입니다. 사용자의 조직에 특정 한 값을 사용 하 여 스마트 잠금 설정의 사용자 지정에는 사용자에 대 한 유료 Azure AD 라이선스가 필요 합니다.

스마트 잠금을 사용 해도 정품 사용자가 잠기지 않도록 보장 하지는 않습니다. 스마트 잠금이 사용자 계정을 잠근 경우에는 정품 사용자의 잠금을 해제 하는 것이 가장 좋습니다. 잠금 서비스는 잘못 된 행위자가 정품 사용자 계정에 대 한 액세스 권한을 얻을 수 없도록 하려고 시도 합니다.  

* 각 Azure Active Directory 데이터 센터는 독립적으로 잠금을 추적합니다. 사용자가 각 데이터 센터에 방문하면 사용자는 (threshold_limit * datacenter_count) 시도 횟수를 갖게 됩니다.
* 스마트 잠금은 익숙한 위치 및 낯선 위치를 사용하여 악의적 행위자와 진정한 사용자를 구별합니다. 낯설고 익숙한 위치는 모두 별도의 잠금 카운터를 갖게 됩니다.

스마트 잠금은 암호 해시 동기화 또는 통과 인증을 사용하여 공격자에 의해 차단되는 온-프레미스 Active Directory 계정을 보호하도록 하이브리드 배포와 통합될 수 있습니다. Azure AD에서 스마트 잠금 정책을 적절하게 설정하여 공격자가 온-프레미스 Active Directory에 도달하기 전에 필터링할 수 있습니다.

[통과 인증](../hybrid/how-to-connect-pta.md)을 사용하는 경우 다음을 확인해야 합니다.

* Azure AD 잠금 임계값이 Active Directory 계정 잠금 임계값**보다 작습니다**. Active Directory의 계정 잠금 임계값을 Azure AD의 잠금 임계값보다 최소 2~3배 이상 크게 설정합니다. 
* Azure AD 잠금 기간은 Active Directory 계정 잠금 해제 카운터 다시 설정 기간 보다 길게 설정 해야 합니다. Azure AD 기간은 초 단위로 설정 되지만 AD 기간은 분 단위로 설정 됩니다. 

예를 들어 Azure AD 카운터가 AD 보다 더 높은 경우 Azure AD는 온-프레미스 광고가 1 분 (60 초)으로 설정 된 상태에서 120 초 (2 분)가 됩니다.

> [!IMPORTANT]
> 현재 관리자는 스마트 잠금 기능에 의해 잠긴 경우 사용자의 클라우드 계정을 잠금 해제할 수 없습니다. 잠금 기간이 만료될 때까지 기다려야 합니다. 그러나 사용자는 신뢰할 수 있는 장치 또는 위치에서 SSPR (셀프 서비스 암호 재설정)를 사용 하 여 잠금을 해제할 수 있습니다.

## <a name="verify-on-premises-account-lockout-policy"></a>온-프레미스 계정 잠금 정책 확인

온-프레미스 Active Directory 계정 잠금 정책을 확인하려면 다음 지침을 사용합니다.

1. 그룹 정책 관리 도구를 엽니다.
2. 조직의 계정 잠금 정책(예: **기본 도메인 정책**)을 포함하는 그룹 정책을 편집합니다.
3. **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **계정 정책** > **계정 잠금 정책**으로 이동 합니다.
4. **계정 잠금 임계값** 을 확인 하 고 값 **뒤에 계정 잠금 카운터를 다시 설정** 합니다.

![온-프레미스 Active Directory 계정 잠금 정책 수정](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD 스마트 잠금 값 관리

조직의 요구 사항에 따라 스마트 잠금 값은 사용자 지정되어야 할 수 있습니다. 사용자의 조직에 특정 한 값을 사용 하 여 스마트 잠금 설정의 사용자 지정에는 사용자에 대 한 유료 Azure AD 라이선스가 필요 합니다.

조직에 대한 스마트 잠금 값을 확인하거나 수정하려면 다음 단계를 사용합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. *Azure Active Directory*를 검색하고 선택합니다. **보안** > **인증 방법** > **암호 보호**를 선택 합니다.
1. 첫 번째 잠금 전에 계정에서 허용되는 실패한 로그인 횟수에 따라 **잠금 임계값**을 설정합니다. 기본값은 10입니다.
1. **잠금 지속 기간(초)** 을 각 잠금의 길이(초)로 설정합니다. 기본값은 60초(1분)입니다.

> [!NOTE]
> 잠금 후 첫 번째 로그인도 실패하는 경우 계정은 다시 잠깁니다. 계정이 반복적으로 잠기는 경우 잠금 지속 시간이 증가합니다.

![Azure Portal에서 Azure AD 스마트 잠금 정책 사용자 지정](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>스마트 잠금 기능이 작동 하는지 여부를 확인 하는 방법

스마트 잠금 임계값이 트리거되면 계정이 잠겨 있는 동안 다음 메시지가 표시 됩니다.

**무단 사용을 방지 하기 위해 계정이 임시로 잠겨 있습니다. 나중에 다시 시도 하세요. 문제가 계속 되 면 관리자에 게 문의 하세요.**

## <a name="next-steps"></a>다음 단계

* [Azure AD를 사용하여 조직에서 불량 암호를 금지하는 방법을 알아봅니다.](howto-password-ban-bad.md)
* [사용자가 고유의 계정을 잠금 해제하도록 셀프 서비스 암호 재설정을 구성합니다.](quickstart-sspr.md)
