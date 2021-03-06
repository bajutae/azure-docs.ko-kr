---
title: '하이브리드 ID: 디렉터리 통합 도구 비교 | Microsoft Docs'
description: 이 페이지는 디렉터리 통합을 위해 사용할 수 있는 다양한 디렉터리 통합 도구를 비교하는 포괄적인 테이블을 제공합니다.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 052d99a819aee415d5e7ad6dc00b8c786af0f636
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80811079"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>하이브리드 ID 디렉터리 통합 도구 비교
디렉터리 통합 도구는 수년에 걸쳐 성장하고 발전했습니다.  


- [FIM](https://docs.microsoft.com/previous-versions/windows/desktop/forefront-2010/ff182370%28v%3dvs.100%29) 과 [MIM](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) 은 계속 지원 되며 주로 온-프레미스 시스템 간의 동기화를 사용 하도록 설정 합니다.   [Fim Windows AZURE AD Connector](https://docs.microsoft.com/previous-versions/mim/dn511001(v=ws.10)?redirectedfrom=MSDN) 는 FIM과 MIM 모두에서 지원 되지만 새 배포에는 권장 되지 않습니다. (note) 또는 SAP hcm과 같은 온-프레미스 고객은 MIM을 사용 하 여 Active Directory Domain Services (AD DS)를 채운 다음 Azure AD Connect sync 또는 Azure AD Connect cloud 프로 비전을 사용 하 여 AD DS에서 Azure AD로 동기화 해야 합니다.
- [Azure AD Connect sync](how-to-connect-sync-whatis.md) 는 AD DS 포리스트와 Azure AD 간의 동기화를 위해 DirSync 및 Azure AD Sync에서 이전에 릴리스된 구성 요소 및 기능을 통합 합니다.  
- [Azure AD Connect 클라우드 프로 비전](../cloud-provisioning/what-is-cloud-provisioning.md) 은 AD DS에서 Azure ad로 동기화 하는 데 사용할 수 있는 새로운 Microsoft 에이전트로, 획득 한 회사의 ad 포리스트가 부모 회사의 ad 포리스트와 격리 되는 합병 및 획득 등의 시나리오에 유용 합니다.

Azure AD Connect 동기화와 Azure AD Connect 클라우드 프로 비전 간의 차이점에 대 한 자세한 내용은 [클라우드 프로 비전 Azure AD Connect 이란?](../cloud-provisioning/what-is-cloud-provisioning.md) 문서를 참조 하세요.

## <a name="next-steps"></a>다음 단계
[Azure Active Directory와 온-프레미스 ID 통합](whatis-hybrid-identity.md)에 대해 자세히 알아봅니다.

