---
title: Azure 운영 보안 검사 목록 | Microsoft Docs
description: 이 문서에서는 Azure 운영 보안에 대한 일단의 검사 목록을 제공합니다.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: fb940857a0f88590cb9bbbf56b9e6a791299309f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75980954"
---
# <a name="azure-operational-security-checklist"></a>Azure 운영 보안 검사 목록
Azure에 애플리케이션을 배포하는 것이 빠르고, 쉽고, 비용 효율적입니다. 프로덕션 환경에 클라우드 애플리케이션을 배포하기 전에 고려해야 하는 필수 및 권장 운영 보안 작업 목록과 대조하여 애플리케이션을 평가하는 데 도움이 되는 검사 목록이 있으면 유용합니다.

## <a name="introduction"></a>소개

Azure는 애플리케이션을 배포하는 데 사용할 수 있는 인프라 서비스 제품군을 제공합니다. Azure 운영 보안은 사용자가 Microsoft Azure에서 자신의 데이터, 애플리케이션 및 기타 자산을 보호할 수 있는 서비스, 제어 및 기능을 나타냅니다.

-   클라우드 플랫폼에서 최대한 활용하려면 Azure 서비스를 활용하고 검사 목록을 따르는 것이 좋습니다.
-   시작하기 전에 애플리케이션의 운영 준비 상태를 평가하는 데 시간과 자원을 투자하는 조직은 그렇지 않은 조직들보다 만족도가 훨씬 높습니다. 이 작업을 수행할 때 검사 목록은 애플리케이션을 일관되고 전체적으로 평가하도록 하는 매우 중요한 메커니즘입니다.
-   운영 평가 수준은 조직의 클라우드 완성도 수준과 응용 프로그램의 개발 단계, 가용성 요구 사항 및 데이터 민감도 요구 사항에 따라 달라 집니다.

## <a name="checklist"></a>검사 목록

이 검사 목록은 기업이 Azure에 정교한 엔터프라이즈 애플리케이션을 배포할 때 다양한 운영 보안 고려 사항을 검토할 수 있도록 돕기 위한 것입니다. 또한 조직의 보안 클라우드 마이그레이션 및 운영 전략을 계획하는 데에도 사용할 수 있습니다.

|검사 목록 범주| Description|
| ------------ | -------- |
| [<br>보안 역할 & 액세스 제어](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>[RBAC(역할 기반 액세스 제어)](../../role-based-access-control/role-assignments-portal.md)를 사용하여 특정 범위의 사용자, 그룹 및 애플리케이션에 권한을 사용자별로 할당할 수 있습니다.</li></ul> |
| [<br>저장소 & 데이터 수집](../../storage/blobs/security-recommendations.md)|<ul><li>관리 평면 보안을 사용하여 [RBAC(역할 기반 Access Control)](../../role-based-access-control/role-assignments-portal.md)를 통해 Storage 계정을 보호합니다.</li><li>데이터 평면 보안을 사용하여 [SAS(공유 액세스 서명)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) 및 저장된 액세스 정책을 통해 데이터에 대한 액세스를 보호합니다.</li><li>전송 수준 암호화 사용 - HTTPS 및 [Azure 파일 공유](../../storage/files/storage-dotnet-how-to-use-files.md)에 대해 [SMB(서버 메시지 블록 프로토콜) 3.0](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)에서 사용하는 암호화 사용</li><li>[클라이언트 쪽 암호화](../../storage/common/storage-client-side-encryption.md)를 사용하여 스토리지 계정에 보내는 데이터를 보호합니다(암호화 키를 단독으로 제어해야 하는 경우). </li><li>[SSE(스토리지 서비스 암호화)](../../storage/common/storage-service-encryption.md)를 사용하여 Azure Storage의 데이터를 자동으로 암호화하고, [Azure Disk Encryption](../azure-security-disk-encryption-overview.md)을 사용하여 OS 및 데이터 디스크의 가상 머신 디스크 파일을 암호화합니다.</li><li>[Azure Storage 분석](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)을 사용하여 권한 부여 유형을 모니터링합니다. Blob Storage와 마찬가지로 사용자가 공유 액세스 서명 또는 스토리지 계정 키를 사용했는지 확인할 수 있습니다.</li><li>[CORS(Cross-Origin Resource Sharing)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services)를 사용하여 다른 도메인의 스토리지 리소스에 액세스합니다.</li></ul> |
|[<br>보안 정책 & 권장 사항](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>[Azure Security Center](../../security-center/security-center-install-endpoint-protection.md)를 사용하여 엔드포인트 솔루션을 배포합니다.</li><li>[WAF(웹 애플리케이션 방화벽)](../../application-gateway/waf-overview.md)를 추가하여 웹 애플리케이션을 보호합니다.</li><li>   Microsoft 파트너의 [방화벽](../../sentinel/connect-data-sources.md) 을 사용 하 여 보안 보호를 강화 합니다. </li><li>Azure 구독에 대 한 보안 연락처 세부 정보를 적용 합니다. [Microsoft 보안 대응 센터 (MSRC)](https://technet.microsoft.com/security/dn528958.aspx) 는 불법적인 또는 권한이 없는 당사자가 고객 데이터에 액세스 한 것을 발견 한 경우 사용자에 게 연락 합니다.</li></ul> |
| [<br>Id & 액세스 관리](identity-management-best-practices.md)|<ul><li>[Azure AD를 사용하여 온-프레미스 디렉터리를 클라우드 디렉터리와 동기화합니다](../../active-directory/hybrid/whatis-hybrid-identity.md).</li><li>[Single Sign-On](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/)을 사용하여 사용자가 Azure AD에서 조직의 계정에 기반하여 SaaS 애플리케이션에 액세스할 수 있게 합니다.</li><li>[암호 재설정 등록 활동](../../active-directory/active-directory-passwords-reporting.md) 보고서를 사용하여 등록하는 사용자를 모니터링합니다.</li><li>사용자에 대한 [MFA(Multi-Factor Authentication)](../../active-directory/authentication/multi-factor-authentication.md)를 사용하도록 설정합니다.</li><li>개발자가 [Microsoft SDL(Security Development Lifecycle)](https://www.microsoft.com/download/details.aspx?id=12379)과 같은 앱에 보안 ID 기능을 사용합니다.</li><li>Azure AD Premium 비정상 보고서 및 [Azure AD ID 보호 기능](../../active-directory/identity-protection/overview.md)을 사용하여 의심스러운 활동을 적극적으로 모니터링합니다.</li></ul> |
|[<br>지속적인 보안 모니터링](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>맬웨어 평가 솔루션 [Azure Monitor 로그](../../log-analytics/log-analytics-queries.md) 를 사용 하 여 인프라의 맬웨어 방지 보호 상태를 보고 합니다.</li><li>[업데이트 평가](../../automation/automation-update-management.md)를 사용하여 잠재적 보안 문제에 대한 전반적 노출을 확인하고 이러한 업데이트가 환경에 중요한지 여부 또는 얼마나 중요한지를 결정합니다.</li><li>[ID 및 액세스](../../security-center/security-center-monitoring.md)는 다음과 같은 사용자에 대한 개요를 제공합니다 </li><ul><li>사용자 ID 상태</li><li>실패 한 로그인 시도 횟수</li><li> 이러한 시도에서 사용된 사용자 계정, 잠겨 있는 계정</li> <li>암호가 변경되거나 다시 설정된 계정 </li><li>현재 로그인한 계정 수</li></ul></ul> |
| [<br>Azure Security Center 검색 기능](../../security-center/security-center-alerts-overview.md#detect-threats)|<ul><li>[검색 기능](../../security-center/security-center-alerts-overview.md#detect-threats)을 사용하여 Microsoft Azure 리소스를 대상으로 하는 활성 위협을 식별합니다.</li><li>[통합된 위협 인텔리전스](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/19/get-threat-intelligence-reports-with-azure-security-center/)를 사용하여 Microsoft 제품과 서비스, [Microsoft DCU(Digital Crimes Unit)](https://www.microsoft.com/trustcenter/security/cybercrime), [MSRC(Microsoft 보안 대응 센터)](response-center.md) 및 외부 피드에서 글로벌 위협 인텔리전스를 활용하여 알려진 위험 인물을 찾습니다.</li><li>[동작 분석](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/): 알려진 패턴을 적용하여 악의적인 동작을 검색합니다. </li><li>[변칙 검색](https://msdn.microsoft.com/library/azure/dn913096.aspx) - 통계적 프로파일링을 사용하여 기록 초기 계획을 작성합니다.</li></ul> |
| [<br>개발자 작업 (DevOps)](https://docs.microsoft.com/azure/architecture/checklist/dev-ops)|<ul><li>안전하고 안정적인 애플리케이션 호스팅 플랫폼을 제공하도록 지원하기 위해 [IaC(Infrastructure as Code)](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/)를 사용하여 네트워크 및 가상 머신의 만들기 및 해제에 대한 자동화 및 유효성 검사를 사용하도록 설정하는 것이 좋습니다.</li><li>[연속 통합 및 배포](/visualstudio/containers/overview#continuous-delivery-and-continuous-integration-cicd)는 진행 중인 코드를 병합 및 테스트하여 결함을 초기에 찾아내도록 합니다. </li><li>[Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview)는 파이프라인의 각 단계를 통해 자동화된 배포를 관리합니다.</li><li>응용 프로그램 상태 및 고객 사용을 위한 프로덕션 환경을 포함 하 여 실행 중인 응용 프로그램의 [앱 성능 모니터링](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) 은 가설을 형성 하 고 신속 하 게 평가 하거나 전략을 파악 합니다.</li><li>[부하 테스트 및 자동 크기 조정](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)을 사용하면 앱의 성능 문제를 찾아내어 배포 품질을 향상시키고 비즈니스 요구 사항에 맞춰 항상 앱을 가동하고 사용할 수 있는지 확인할 수 있습니다.</li></ul> |


## <a name="conclusion"></a>결론
많은 조직에서 Azure에 클라우드 애플리케이션을 성공적으로 배포하고 운영했습니다. 제공 된 검사 목록에는 필수적 이며 성공적인 배포 및 불만 없는 작업의 가능성을 높이는 데 도움이 되는 몇 가지 검사 목록이 강조 표시 되어 있습니다. Azure에서는 기존 및 새 애플리케이션 배포에 대한 이러한 운영 및 전략적 고려 사항을 적용하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계
보안에 대해 자세히 알아보려면 다음 문서를 참조하세요.

- [디자인 및 운영 보안](https://www.microsoft.com/trustcenter/security/designopsecurity).
- [계획 및 작업을 Azure Security Center](../../security-center/security-center-planning-and-operations-guide.md)합니다.
