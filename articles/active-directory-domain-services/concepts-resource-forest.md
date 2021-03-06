---
title: Azure AD Domain Services에 대 한 리소스 포리스트 개념 | Microsoft Docs
description: 리소스 포리스트의 Azure Active Directory Domain Services, 제한 된 사용자 인증 옵션 또는 보안 문제를 포함 하 여 하이브리드 환경에서 조직의 혜택을 얻는 방법에 대해 알아봅니다.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: e0e5dde246dbcd5e5cb2e4ae923872a59a539d87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80476410"
---
# <a name="resource-forest-concepts-and-features-for-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services에 대 한 리소스 포리스트 개념 및 기능

Azure Active Directory Domain Services (AD DS)는 레거시 온-프레미스 lob (기간 업무) 응용 프로그램에 대 한 로그인 환경을 제공 합니다. 온-프레미스 및 클라우드 사용자의 사용자, 그룹 및 암호 해시는 Azure AD DS 관리 되는 도메인에 동기화 됩니다. 이러한 동기화 된 암호 해시는 온-프레미스 AD DS, Office 365 및 Azure Active Directory에 사용할 수 있는 단일 자격 증명 집합을 사용자에 게 제공 하는 것입니다.

보안 및 추가 보안 이점을 제공 하지만 일부 조직에서는 이러한 사용자 암호 해시를 Azure AD 또는 Azure AD DS와 동기화 할 수 없습니다. 조직의 사용자는 스마트 카드 인증만 사용 하기 때문에 암호를 알지 못할 수 있습니다. 이러한 제한 사항으로 인해 일부 조직에서는 Azure AD DS를 사용 하 여 온-프레미스 클래식 응용 프로그램을 Azure로 리프트 앤 시프트 하지 못합니다.

이러한 요구와 제한을 해결 하기 위해 리소스 포리스트를 사용 하는 Azure AD DS 관리 되는 도메인을 만들 수 있습니다. 이 개념 문서에서는 포리스트의 정의와 보안 인증 방법을 제공 하는 다른 리소스를 신뢰 하는 방법을 설명 합니다. Azure AD DS 리소스 포리스트는 현재 미리 보기로 제공됩니다.

> [!IMPORTANT]
> Azure AD DS 리소스 포리스트는 현재 Azure HDInsight 또는 Azure Files를 지원 하지 않습니다. 기본 Azure AD DS 사용자 포리스트는 이러한 추가 서비스를 모두 지원 합니다.

## <a name="what-are-forests"></a>포리스트 란?

*포리스트* 는 Active Directory Domain Services (AD DS)에서 하나 이상의 *도메인*을 그룹화 하는 데 사용 되는 논리적 구문입니다. 그런 다음 도메인은 사용자 또는 그룹에 대 한 개체를 저장 하 고 인증 서비스를 제공 합니다.

Azure AD DS에서 포리스트는 도메인을 하나만 포함 합니다. 온-프레미스 AD DS 포리스트에는 여러 도메인이 포함 되는 경우가 많습니다. 특히 인수 및 합병 후에 규모가 많은 조직의 경우 여러 온-프레미스 포리스트가 있을 수 있으며, 각 포리스트는 여러 도메인을 포함 합니다.

기본적으로 Azure AD DS 관리 되는 도메인은 *사용자* 포리스트로 생성 됩니다. 이 유형의 포리스트는 온-프레미스 AD DS 환경에서 만든 모든 사용자 계정을 포함하여 Azure AD의 모든 개체를 동기화합니다. 사용자 계정은 도메인에 가입 된 VM에 로그인 하는 등 Azure AD DS 관리 되는 도메인에 대해 직접 인증할 수 있습니다. 사용자 포리스트는 암호 해시를 동기화 할 수 있고 사용자가 스마트 카드 인증과 같은 독점적인 로그인 방법을 사용 하지 않는 경우 작동 합니다.

Azure AD DS *리소스* 포리스트에서는 사용자가 온-프레미스 AD DS에서 단방향 포리스트 *트러스트* 를 통해 인증 합니다. 이 방법을 사용 하면 사용자 개체 및 암호 해시가 Azure AD DS에 동기화 되지 않습니다. 사용자 개체 및 자격 증명은 온-프레미스 AD DS에만 존재 합니다. 이 접근 방식을 통해 기업은 Azure에서 LDAPS, Kerberos 또는 NTLM 등의 클래식 인증에 의존 하는 리소스 및 응용 프로그램 플랫폼을 호스트할 수 있지만 모든 인증 문제나 문제는 제거 됩니다. Azure AD DS 리소스 포리스트는 현재 미리 보기로 제공됩니다.

또한 리소스 포리스트는 응용 프로그램을 한 번에 한 구성 요소로 리프트 앤 시프트 하는 기능을 제공 합니다. 많은 레거시 온-프레미스 응용 프로그램은 종종 웹 서버 또는 프런트 엔드 및 많은 데이터베이스 관련 구성 요소를 사용 하는 다중 계층으로 구성 됩니다. 이러한 계층을 통해 한 번에 전체 응용 프로그램을 클라우드로 리프트 앤 시프트 할 수 있습니다. 리소스 포리스트를 사용 하면 응용 프로그램을 Azure로 더 쉽게 이동할 수 있도록 단계별 접근 방식으로 응용 프로그램을 클라우드로 전환할 수 있습니다.

## <a name="what-are-trusts"></a>트러스트 란?

도메인이 둘 이상 있는 조직에서는 사용자가 다른 도메인의 공유 리소스에 액세스 해야 하는 경우가 종종 있습니다. 이러한 공유 리소스에 액세스 하려면 한 도메인의 사용자가 다른 도메인에 인증 해야 합니다. 서로 다른 도메인의 클라이언트와 서버 간에 이러한 인증 및 권한 부여 기능을 제공 하려면 두 도메인 간에 *트러스트가* 있어야 합니다.

도메인 트러스트를 사용 하는 경우 각 도메인에 대 한 인증 메커니즘은 다른 도메인에서 들어오는 인증을 신뢰 합니다. 트러스트를 통해 들어오는 인증 요청을 신뢰할 수 있는 기관 ( *트러스트* 된 도메인)에서 가져온 것인지 확인 하 여 리소스 도메인 ( *트러스팅* 도메인)의 공유 리소스에 대 한 제어 된 액세스를 제공할 수 있습니다. 트러스트는 유효성 검사 된 인증 요청만 도메인 간에 이동 하도록 허용 하는 브리지 역할을 합니다.

트러스트는 인증 요청을 통과 하는 방법은 구성 방법에 따라 달라 집니다. 트러스트는 다음 중 한 가지 방법으로 구성할 수 있습니다.

* **단방향-트러스트** 된 도메인에서 트러스팅 도메인의 리소스로의 액세스를 제공 합니다.
* **양방향** -각 도메인에서 다른 도메인의 리소스에 대 한 액세스를 제공 합니다.

다음 방법 중 하나를 통해 트러스트 관계를 추가로 처리 하도록 트러스트를 구성할 수도 있습니다.

* **비 전이적** -트러스트는 두 트러스트 파트너 도메인 간에만 존재 합니다.
* **전이적** 트러스트는 파트너 트러스트 중 하나가 다른 모든 도메인으로 자동 확장 됩니다.

도메인을 만들 때 트러스트 관계가 자동으로 설정 되는 경우도 있습니다. 다른 경우에는 신뢰 유형을 선택 하 고 명시적으로 적절 한 관계를 설정 해야 합니다. 사용 되는 특정 트러스트 유형 및 이러한 트러스트 관계의 구조는 Active Directory Directory 서비스를 구성 하는 방법 및 서로 다른 버전의 Windows가 네트워크에 공존 하는지 여부에 따라 달라 집니다.

## <a name="trusts-between-two-forests"></a>두 포리스트 간 트러스트

단방향 또는 양방향 포리스트 트러스트를 수동으로 만들어 단일 포리스트 내의 도메인 트러스트를 다른 포리스트로 확장할 수 있습니다. 포리스트 트러스트는 포리스트 루트 도메인과 두 번째 포리스트 루트 도메인 간에만 존재 하는 전이적 트러스트입니다.

* 단방향 포리스트 트러스트는 한 포리스트의 모든 사용자가 다른 포리스트의 모든 도메인을 신뢰 하도록 허용 합니다.
* 양방향 포리스트 트러스트는 두 포리스트의 모든 도메인 간에 전이적 트러스트 관계를 형성 합니다.

포리스트 트러스트의 전이성은 두 개의 포리스트 파트너로 제한 됩니다. 포리스트 트러스트가 파트너 중 하나에서 트러스트 된 추가 포리스트로 확장 되지 않습니다.

![Azure AD DS에서 온-프레미스 AD DS로의 포리스트 트러스트에 대한 다이어그램](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

조직의 Active Directory 구조에 따라 서로 다른 도메인 및 포리스트 트러스트 구성을 만들 수 있습니다. Azure AD DS는 단방향 포리스트 트러스트만 지원 합니다. 이 구성에서 Azure AD DS 리소스는 온-프레미스 포리스트의 모든 도메인을 신뢰할 수 있습니다.

## <a name="supporting-technology-for-trusts"></a>신뢰 지원 기술

트러스트는 DNS와 같은 다양 한 서비스 및 기능을 사용 하 여 파트너 포리스트에서 도메인 컨트롤러를 찾습니다. 또한 트러스트는 NTLM 및 Kerberos 인증 프로토콜과 Windows 기반 권한 부여 및 액세스 제어 메커니즘에 따라 Active Directory 도메인 및 포리스트 간에 보안 통신 인프라를 제공 하는 데 도움이 됩니다. 다음 서비스 및 기능은 성공적인 신뢰 관계를 지 원하는 데 도움이 됩니다.

### <a name="dns"></a>DNS

DC (도메인 컨트롤러) 위치 및 이름 지정을 위해 DNS가 필요 AD DS. AD DS 성공적으로 작동 하기 위해 DNS에서 다음 지원이 제공 됩니다.

* 네트워크 호스트 및 서비스에서 Dc를 찾을 수 있도록 하는 이름 확인 서비스입니다.
* 엔터프라이즈에서 해당 디렉터리 서비스 도메인의 이름에 조직 구조를 반영할 수 있도록 하는 명명 구조입니다.

일반적으로 DNS 도메인 네임 스페이스는 AD DS 도메인 네임 스페이스를 미러링 하는 배포 됩니다. AD DS 배포 하기 전에 기존 DNS 네임 스페이스가 있는 경우 DNS 네임 스페이스는 일반적으로 Active Directory를 위해 분할 되며, Active Directory 포리스트 루트에 대 한 DNS 하위 도메인 및 위임이 만들어집니다. 그러면 각 Active Directory 자식 도메인에 대 한 추가 DNS 도메인 이름이 추가 됩니다.

DNS는 Active Directory Dc의 위치를 지 원하는 데에도 사용 됩니다. DNS 영역은 네트워크 호스트 및 서비스에서 Active Directory Dc를 찾을 수 있도록 하는 DNS 리소스 레코드로 채워집니다.

### <a name="applications-and-net-logon"></a>응용 프로그램 및 Net Logon

응용 프로그램과 Net Logon 서비스는 모두 Windows 분산 보안 채널 모델의 구성 요소입니다. Windows Server 및 Active Directory와 통합 된 응용 프로그램은 인증 프로토콜을 사용 하 여 인증을 수행할 수 있는 보안 경로를 설정할 수 있도록 Net Logon 서비스와 통신 합니다.

### <a name="authentication-protocols"></a>인증 프로토콜

Active Directory Dc는 다음 프로토콜 중 하나를 사용 하 여 사용자 및 응용 프로그램을 인증 합니다.

* **Kerberos 버전 5 인증 프로토콜**
    * Kerberos 버전 5 프로토콜은 Windows를 실행 하 고 타사 운영 체제를 지 원하는 온-프레미스 컴퓨터에서 사용 하는 기본 인증 프로토콜입니다. 이 프로토콜은 RFC 1510에 지정 되어 있으며 이러한 프로토콜을 사용 하는 클라이언트 및 서버 응용 프로그램 뿐만 아니라 Active Directory, SMB (서버 메시지 블록), HTTP 및 RPC (원격 프로시저 호출)와 완벽 하 게 통합 됩니다.
    * Kerberos 프로토콜을 사용 하는 경우 서버는 DC에 연결할 필요가 없습니다. 대신, 클라이언트는 서버 계정 도메인의 DC에서 하나를 요청 하 여 서버에 대 한 티켓을 가져옵니다. 그런 다음 서버는 다른 인증 기관에 문의 하지 않고 티켓의 유효성을 검사 합니다.
    * 트랜잭션과 관련 된 컴퓨터에서 Kerberos 버전 5 프로토콜을 지원 하지 않는 경우 NTLM 프로토콜이 사용 됩니다.

* **NTLM 인증 프로토콜**
    * NTLM 프로토콜은 이전 운영 체제에서 사용 하는 클래식 네트워크 인증 프로토콜입니다. 호환성을 위해 Active Directory 도메인에서 이전 Windows 기반 클라이언트 및 서버를 위해 설계 된 응용 프로그램에서 제공 하는 네트워크 인증 요청을 처리 하는 데 사용 되며 타사 운영 체제에도 사용 됩니다.
    * 클라이언트와 서버 간에 NTLM 프로토콜을 사용 하는 경우 서버는 DC의 도메인 인증 서비스에 연결 하 여 클라이언트 자격 증명을 확인 해야 합니다. 서버는 클라이언트 자격 증명을 클라이언트 계정 도메인의 DC에 전달 하 여 클라이언트를 인증 합니다.
    * 두 개의 Active Directory 도메인 또는 포리스트가 트러스트로 연결 된 경우 이러한 프로토콜을 사용 하 여 만든 인증 요청은 두 포리스트의 리소스에 대 한 액세스를 제공 하기 위해 라우팅될 수 있습니다.

## <a name="authorization-and-access-control"></a>권한 부여 및 액세스 제어

권한 부여 및 신뢰 기술은 함께 작동 하 여 Active Directory 도메인 이나 포리스트 간에 보안 통신 인프라를 제공 합니다. 권한 부여는 도메인의 리소스에 대 한 사용자의 액세스 수준을 결정 합니다. 트러스트는 다른 도메인의 사용자를 인증 하는 경로를 제공 하 여 사용자의 도메인 간 권한 부여를 용이 하 게 하므로 해당 도메인의 공유 리소스에 대 한 요청을 인증할 수 있습니다.

트러스팅 도메인에서 만든 인증 요청이 트러스트 된 도메인에서 확인 되는 경우 대상 리소스에 전달 됩니다. 그런 다음 대상 리소스는 해당 액세스 제어 구성에 따라 트러스트 된 도메인에 있는 사용자, 서비스 또는 컴퓨터의 특정 요청에 대 한 권한을 부여할 것인지 여부를 결정 합니다.

트러스트는 트러스팅 도메인에 전달 되는 인증 요청의 유효성을 검사 하는 메커니즘을 제공 합니다. 리소스 컴퓨터의 액세스 제어 메커니즘은 트러스트 된 도메인의 요청자에 게 부여 된 최종 액세스 수준을 결정 합니다.

## <a name="next-steps"></a>다음 단계

트러스트에 대 한 자세한 내용은 [Azure AD DS에서 포리스트 트러스트를 사용 하는 방법][concepts-trust] 을 참조 하세요.

리소스 포리스트를 사용 하 여 Azure AD DS 관리 되는 도메인 만들기를 시작 하려면 [azure AD DS 관리 되는 도메인 만들기 및 구성][tutorial-create-advanced]을 참조 하세요. 그런 다음 [온-프레미스 도메인 (미리 보기)에 대 한 아웃 바운드 포리스트 트러스트를 만들][create-forest-trust]수 있습니다.

<!-- LINKS - INTERNAL -->
[concepts-trust]: concepts-forest-trust.md
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-forest-trust]: tutorial-create-forest-trust.md
