---
title: 셀프 서비스 암호 재설정을 사용 하 여 온-프레미스 비밀 번호 쓰기 저장-Azure Active Directory
description: Azure Active Directory의 암호 변경 또는 다시 설정 이벤트를 온-프레미스 디렉터리 환경에 다시 쓸 수 있는 방법에 대해 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89431c2bf1838d3264b03c8a5f2ce62cd6df3631
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82127836"
---
# <a name="how-does-self-service-password-reset-writeback-work-in-azure-active-directory"></a>Azure Active Directory에서 셀프 서비스 암호 다시 설정 쓰기 저장은 어떻게 작동 하나요?

Azure Active Directory (Azure AD) 셀프 서비스 암호 재설정 (SSPR)을 통해 사용자는 클라우드에서 암호를 재설정할 수 있지만 대부분의 회사에는 사용자가 있는 온-프레미스 Active Directory Domain Services (AD DS) 환경이 있습니다. 비밀번호 쓰기 저장은 [Azure AD Connect](../hybrid/whatis-hybrid-identity.md)를 통해 활성화되며 클라우드의 암호 변경 내용을 실시간으로 기존 온-프레미스 디렉터리에 다시 쓸 수 있는 기능입니다. 이 구성에서 사용자가 클라우드에서 SSPR를 사용 하 여 암호를 변경 하거나 다시 설정 하면 업데이트 된 암호도 온-프레미스 AD DS 환경에 다시 기록 됩니다.

비밀 번호 쓰기 저장은 다음과 같은 하이브리드 id 모델을 사용 하는 환경에서 지원 됩니다.

* [암호 해시 동기화](../hybrid/how-to-connect-password-hash-synchronization.md)
* [통과 인증](../hybrid/how-to-connect-pta.md)
* [Active Directory Federation Services](../hybrid/how-to-connect-fed-management.md)

비밀번호 쓰기 저장에서 제공하는 기능은 다음과 같습니다.

* **온-프레미스 Active Directory Domain Services (AD DS) 암호 정책 적용**: 사용자가 자신의 암호를 재설정 하는 경우 해당 디렉터리에 커밋하기 전에 온-프레미스 AD DS 정책을 충족 하는지 확인 합니다. 이 검토에는 기록, 복잡성, 나이, 암호 필터 및 AD DS에서 정의한 기타 암호 제한을 확인 하는 작업이 포함 됩니다.
* **지연 피드백 없음**: 비밀번호 쓰기 저장은 동기식 작업입니다. 사용자의 암호가 정책에 맞지 않거나 어떤 이유로 든 다시 설정 하거나 변경할 수 없는 경우 즉시 사용자에 게 알림이 제공 됩니다.
* **액세스 패널 및 Office 365에서 암호 변경을 지원**합니다. 페더레이션 또는 암호 해시 동기화 된 사용자가 만료 되었거나 만료 되지 않은 암호를 변경 하는 경우 해당 암호가 AD DS 다시 작성 됩니다.
* 는 **관리자가 Azure Portal에서 암호를 다시 설정할 때 암호 쓰기 저장을 지원**합니다. 관리자가 [Azure Portal](https://portal.azure.com)에서 사용자의 암호를 다시 설정 하는 경우 해당 사용자가 페더레이션 또는 암호 해시 동기화 된 경우 암호는 온-프레미스에 다시 기록 됩니다. 이 기능은 현재 Office 관리자 포털에서 지원되지 않습니다.
* **인바운드 방화벽 규칙이 필요 하지 않습니다**. 비밀 번호 쓰기 저장은 Azure Service Bus 릴레이를 기본 통신 채널로 사용 합니다. 모든 통신은 포트 443을 통해 아웃바운드됩니다.

> [!NOTE]
> 온-프레미스 AD의 보호 그룹 내에 있는 관리자 계정은 비밀번호 쓰기 저장에 사용할 수 없습니다. 관리자는 클라우드에서 암호를 변경할 수 있지만 암호 재설정을 사용 하 여 잊어버린 암호를 재설정할 수 없습니다. 보호 그룹에 대한 자세한 내용은 [Active Directory의 보호 계정 및 그룹](/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory)을 참조하세요.

## <a name="how-password-writeback-works"></a>암호 쓰기 저장의 작동 원리

페더레이션 또는 암호 해시 동기화된 사용자가 클라우드에서 자신의 암호를 재설정하거나 변경하려고 시도하면 다음 작업이 수행됩니다.

1. 사용자가 어떤 종류의 암호를 가지고 있는지 확인하기 위한 검사가 수행됩니다. 암호가 온-프레미스에서 관리되는 경우:
   * 쓰기 저장 서비스를 사용 중인지 확인하기 위한 검사가 수행됩니다. 사용 중인 경우 사용자가 계속 진행할 수 있습니다.
   * 쓰기 저장 서비스가 중지된 경우 사용자에게 지금은 암호를 재설정할 수 없다고 알려줍니다.
1. 다음으로, 사용자는 적절한 인증 게이트를 통과하여 **암호 재설정** 페이지로 이동됩니다.
1. 사용자는 새 암호를 선택하고 다시 확인합니다.
1. 사용자가 **제출**을 선택하면 쓰기 저장 설정 프로세스에서 만든 대칭 키로 일반 텍스트 암호가 암호화됩니다.
1. 암호화된 암호는 HTTPS 채널을 통해 테넌트별 Service Bus Relay(쓰기 저장 설정 프로세스에서 자동으로 설정됨)로 전송되는 페이로드에 포함됩니다. 이 릴레이는 온-프레미스 설치만 알고 있는 임의로 생성된 암호에 의해 보호됩니다.
1. 메시지가 서비스 버스에 도달하면 암호 재설정 엔드포인트가 자동으로 절전 모드에서 해제되어 보류 중인 재설정 요청이 있는지 확인합니다.
1. 그런 다음, 서비스에서 클라우드 앵커 특성을 사용하여 해당 사용자를 찾습니다. 이 조회가 성공 하려면 다음 조건을 충족 해야 합니다.

   * 사용자 개체가 Active Directory 커넥터 공간에 있어야 합니다.
   * 사용자 개체가 해당 MV(metaverse) 개체에 연결되어야 합니다.
   * 사용자 개체가 해당 Azure Active Directory 커넥터 개체에 연결되어야 합니다.
   * Active Directory 커넥터 개체에서 MV로의 링크에는 동기화 규칙 `Microsoft.InfromADUserAccountEnabled.xxx`이 있어야 합니다.

   클라우드에서 호출이 들어오면 동기화 엔진은 **cloudAnchor** 특성을 사용하여 Azure Active Directory 커넥터 공간 개체를 조회합니다. 그런 후 다시 링크를 따라 MV 개체로 이동한 후 Active Directory 개체로 이동합니다. 동일한 사용자에 대해 여러 Active Directory 개체(다중 포리스트)가 있을 수 있기 때문에 동기화 엔진은 `Microsoft.InfromADUserAccountEnabled.xxx` 링크에 의존하여 정확한 개체를 선택합니다.

1. 사용자 계정을 찾은 후에는 적절한 Active Directory 포리스트에서 직접 암호를 재설정하려는 시도가 수행됩니다.
1. 암호 설정 작업이 성공하면 사용자에게 암호가 변경되었음을 알려줍니다.

   > [!NOTE]
   > 암호 해시 동기화를 사용 하 여 사용자의 암호 해시가 Azure AD에 동기화 되는 경우 온-프레미스 암호 정책이 클라우드 암호 정책 보다 약 할 가능성이 있습니다. 이 경우 온-프레미스 정책이 적용됩니다. 이 정책을 사용하면 Single Sign-On을 제공하기 위해 암호 해시 동기화를 사용하든 아니면 페더레이션을 사용하든, 온-프레미스 정책이 클라우드에서 적용됩니다.

1. 암호 설정 작업이 실패하면 다시 시도하라는 오류 메시지가 표시됩니다. 작업은 다음과 같은 이유로 인해 실패할 수 있습니다.
    * 서비스가 종료되었습니다.
    * 선택한 암호가 조직의 정책을 충족 하지 않습니다.
    * 로컬 Active Directory에서 사용자를 찾을 수 없습니다.

   오류 메시지는 관리자 개입 없이 문제 해결을 시도할 수 있도록 사용자에게 지침을 제공합니다.

## <a name="password-writeback-security"></a>비밀번호 쓰기 저장 보안

비밀번호 쓰기 저장은 매우 안전한 서비스입니다. 정보가 보호 되도록 하려면 다음과 같이 4 계층 보안 모델을 사용 하도록 설정 합니다.

* **테넌트별 Service Bus Relay**
   * 사용자가 이 서비스를 설정하면 임의로 생성된 강력한 암호에 의해 보호되는 테넌트별 Service Bus Relay가 설정되며, Microsoft는 절대로 이 암호에 액세스할 수 없습니다.
* **잠겨 있는 강력한 암호 암호화 키**
   * Service bus relay를 만든 후에는 네트워크를 통해 들어올 때 암호를 암호화 하는 데 사용 되는 강력한 대칭 키가 that'is 됩니다. 이 키는 클라우드의 회사의 암호 저장소에만 존재하며 디렉터리의 다른 암호와 마찬가지로 강력하게 잠겨 있고 감사됩니다.
* **업계 표준 TLS(전송 계층 보안)**
   1. 클라우드에서 암호 재설정 또는 변경 작업이 발생하면 공개 키를 사용하여 일반 텍스트 암호가 암호화됩니다.
   1. 암호화 된 암호는 서비스 버스 릴레이에 Microsoft TLS/SSL 인증서를 사용 하 여 암호화 된 채널을 통해 전송 되는 HTTPS 메시지에 저장 됩니다.
   1. Service Bus에 메시지가 도착한 후에 온-프레미스 에이전트가 시작하고 이전에 생성된 강력한 암호를 사용하여 Service Bus에 인증합니다.
   1. 온-프레미스 에이전트는 암호화된 메시지를 선택하고 프라이빗 키를 사용하여 암호를 해독합니다.
   1. 온-프레미스 에이전트는 AD DS SetPassword API를 통해 암호를 설정하려고 합니다. 이 단계에서는 클라우드에서 Active Directory 온-프레미스 암호 정책(예: 복잡성, 나이, 기록, 필터)을 적용할 수 있습니다.
* **메시지 만료 정책**
   * 온-프레미스 서비스가 종료되었기 때문에 메시지가 Service Bus에 남아 있는 경우 시간이 초과되고 몇 분 후에 제거됩니다. 메시지의 시간 초과 및 제거를 통해 메시지 보안이 더욱 강화됩니다.

### <a name="password-writeback-encryption-details"></a>비밀번호 쓰기 저장 암호화 세부 정보

사용자가 암호 재설정을 제출한 후 재설정 요청은 여러 암호화 단계를 거친 후 온-프레미스 환경에 도달합니다. 이러한 암호화 단계는 최대의 서비스 안정성과 보안을 보장합니다. 다음과 같이 설명할 수 있습니다.

1. **2048 비트 Rsa 키를 사용 하는 암호 암호화**: 사용자가 온-프레미스에 다시 쓸 암호를 제출한 후에는 제출 된 암호 자체를 2048 비트 RSA 키로 암호화 합니다.
1. **Aes-gcm을 사용 하는 패키지 수준 암호화**: 전체 패키지, 암호 및 필요한 메타 데이터는 AES-gcm을 사용 하 여 암호화 됩니다. 이 암호화는 기본 ServiceBus 채널에 직접 액세스할 수 있는 사람이 콘텐츠를 보거나 변조하는 것을 방지합니다.
1. **모든 통신은 TLS/SSL을 통해 발생**합니다. ServiceBus와의 모든 통신은 SSL/TLS 채널에서 발생 합니다. 이 암호화는 권한이 없는 제3자의 콘텐츠를 보호합니다.
1. **6개월마다 자동 키 롤오버**: 최상의 서비스 보안과 안전성을 보장하기 위해, 모든 키는 6개월마다 또는 Azure AD Connect에서 비밀번호 쓰기 저장이 해제되었다가 다시 설정될 때마다 롤오버됩니다.

### <a name="password-writeback-bandwidth-usage"></a>비밀번호 쓰기 저장 대역폭 사용

비밀번호 쓰기 저장은 다음과 같은 경우에만 온-프레미스 에이전트로 요청을 다시 보내는 낮은 대역폭 서비스입니다.

* Azure AD Connect를 통해 이 기능이 사용되거나 사용되지 않도록 설정될 때 2개의 메시지가 전송됩니다.
* 서비스가 실행되는 동안 한 메시지를 서비스 하트 비트로 5분마다 한 번씩 전송합니다.
* 두 개의 메시지는 새 암호를 전송할 때 각각 전송됩니다.
   * 첫 번째 메시지는 작업을 수행하는 요청입니다.
   * 두 번째 메시지는 작업의 결과를 포함하고 다음과 같은 경우에 전송됩니다.
      * 사용자 셀프 서비스 비밀번호 다시 설정 중에 새 비밀번호가 제출될 때마다
      * 사용자 비밀번호 변경 작업 중에 새 비밀번호가 제출될 때마다
      * 관리자가 시작한 사용자 비밀번호 재설정 중에 새 비밀번호가 제출될 때마다(Azure 관리 포털에서만)

#### <a name="message-size-and-bandwidth-considerations"></a>메시지 크기 및 대역폭 고려 사항

앞에서 설명한 각 메시지의 일반적으로 1KB 미만입니다. 극단적인 부하에서도 비밀번호 쓰기 저장 서비스 자체에서 초당 몇 Kb의 대역폭을 소비합니다. 각 메시지가 비밀번호 업데이트 작업에서 요구할 때만 실시간으로 전송되고 메시지 크기가 너무 작기 때문에 쓰기 저장 기능의 대역폭 사용은 실제 측정 가능한 영향을 미치기에는 너무 작습니다.

## <a name="supported-writeback-operations"></a>지원되는 쓰기 저장 작업

암호는 다음과 같은 경우에 모두 다시 기록됩니다.

* **지원되는 최종 사용자 작업**
   * 모든 최종 사용자 셀프 서비스 자발적 변경 암호 작업
   * 모든 최종 사용자 셀프 서비스 강제 변경 암호 작업 (예: 암호 만료)
   * [암호 재설정 포털](https://passwordreset.microsoftonline.com)에서 시작 되는 모든 최종 사용자 셀프 서비스 암호 재설정입니다.

* **지원 되는 관리자 작업**
   * 모든 관리자 셀프 서비스 자발적 변경 암호 작업
   * 모든 관리자 셀프 서비스 적용 변경 암호 작업 (예: 암호 만료)
   * [암호 재설정 포털](https://passwordreset.microsoftonline.com)에서 시작 되는 모든 관리자 셀프 서비스 암호 재설정입니다.
   * [Azure Portal](https://portal.azure.com)에서 관리자가 시작한 최종 사용자 암호 재설정
   * [MICROSOFT GRAPH API 베타](https://docs.microsoft.com/graph/api/passwordauthenticationmethod-resetpassword?view=graph-rest-beta&tabs=http)에서 관리자가 시작한 최종 사용자 암호 재설정

## <a name="unsupported-writeback-operations"></a>지원되지 않는 쓰기 저장 작업

암호는 다음과 같은 경우에 다시 기록 되지 않습니다.

* **지원 되지 않는 최종 사용자 작업**
   * PowerShell 버전 1, 버전 2 또는 Microsoft Graph API를 사용 하 여 자신의 암호를 다시 설정 하는 모든 최종 사용자입니다.
* **지원되지 않는 관리자 작업**
   * PowerShell 버전 1, 버전 2 또는 Microsoft Graph API에서 관리자 시작 최종 사용자 암호 재설정 ( [MICROSOFT GRAPH api beta](https://docs.microsoft.com/graph/api/passwordauthenticationmethod-resetpassword?view=graph-rest-beta&tabs=http) 가 지원 됨)
   * 관리자가 시작한 최종 사용자 암호를 [Microsoft 365 관리 센터](https://admin.microsoft.com)에서 다시 설정 합니다.

> [!WARNING]
> 사용자 및 컴퓨터 Active Directory 같은 온-프레미스 AD DS 관리 도구에서 "다음 로그온 할 때 암호를 변경 해야 합니다." 확인란을 사용 Active Directory 관리 센터 하거나 Azure AD Connect의 미리 보기 기능으로 지원 됩니다. 자세한 내용은 [Azure AD Connect 동기화를 사용하여 암호 해시 동기화 구현](../hybrid/how-to-connect-password-hash-synchronization.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

SSPR 쓰기 저장을 시작 하려면 다음 자습서를 완료 하세요.

> [!div class="nextstepaction"]
> [자습서: SSPR (셀프 서비스 암호 재설정) 쓰기 저장 (writeback) 사용](tutorial-enable-writeback.md)
