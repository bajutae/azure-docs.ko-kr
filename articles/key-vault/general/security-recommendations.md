---
title: Azure Key Vault에 대한 보안 추천 사항
description: Azure Key Vault에 대 한 보안 권장 사항 이 지침을 구현 하면 공유 책임 모델에 설명 된 대로 보안 의무를 달성 하는 데 도움이 됩니다.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
ms.date: 09/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 0da1a3019124f62aba6a959ce9104c85bd85d3fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81616496"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Key Vault에 대한 보안 추천 사항

이 문서에는 Azure Key Vault에 대 한 보안 권장 사항이 포함 되어 있습니다. 이러한 권장 사항을 구현 하면 공유 책임 모델에 설명 된 대로 보안 의무를 달성 하는 데 도움이 됩니다. Microsoft에서 서비스 공급자 역할을 수행 하는 데 필요한 사항에 대 한 자세한 내용은 [클라우드 컴퓨팅을 위한 공유 책임](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)을 참조 하세요.

이 문서에 포함 된 일부 권장 사항은 Azure Security Center에서 자동으로 모니터링할 수 있습니다. Azure Security Center는 Azure에서 리소스를 보호 하는 첫 번째 방어선입니다. Azure 리소스의 보안 상태를 주기적으로 분석 하 여 잠재적인 보안 취약성을 식별 합니다. 그런 다음이를 해결 하는 방법에 대 한 권장 사항을 제공 합니다.

- Azure Security Center 권장 사항에 대 한 자세한 내용은 [Azure Security Center의 보안 권장 사항](../../security-center/security-center-recommendations.md)을 참조 하세요.
- Azure Security Center에 대 한 자세한 내용은 [Azure Security Center 항목](../../security-center/security-center-intro.md) 을 참조 하세요.

## <a name="data-protection"></a>데이터 보호

| 권장 | 주석 | Security Center |
|-|----|--|
|일시 삭제 사용 | [일시 삭제](overview-soft-delete.md)) 삭제 된 자격 증명 모음 및 자격 증명 모음 개체를 복구할 수 있습니다. |  - |
| 자격 증명 모음 데이터에 대 한 액세스 제한  | 최소 권한의 원칙에 따라 자격 증명 모음 데이터에 대 한 액세스 권한이 있는 조직의 구성원 제한 |  - |

## <a name="identity-and-access-management"></a>ID 및 액세스 관리

| 권장 | 주석 | Security Center |
|-|----|--|
| 참가자 액세스 권한이 있는 사용자 수 제한 | 사용자에 게 키 자격 증명 모음 관리 평면에 대 한 참가자 권한이 있는 경우 사용자는 Key Vault 액세스 정책을 설정 하 여 데이터 평면에 대 한 액세스 권한을 자신에 게 부여할 수 있습니다. 키 자격 증명 모음에 대 한 참가자 역할 액세스 권한이 있는 사용자를 강력 하 게 제어 해야 합니다. 액세스 권한이 있는 사용자에 대 한 액세스 권한이 있는 사용자만 자격 증명 모음에 액세스 하 고 관리할 수 있어야 합니다. [키 자격 증명 모음에 대 한 보안 액세스를](secure-your-key-vault.md)읽을 수 있습니다.) | - |

## <a name="monitoring"></a>모니터링

| 권장 | 주석 | Security Center |
|-|----|--|
 Key Vault의 진단 로그를 사용하도록 설정해야 합니다. | 로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. | [예](../../security-center/security-center-identity-access.md) |
| Azure 주요 자격 증명 모음 로그에 액세스할 수 있는 사용자 제한 | [Key Vault 로그](logging.md)) 자격 증명 모음에서 수행 된 작업에 대 한 정보 (예: 자격 증명 모음, 키, 암호의 생성 또는 삭제)를 저장 하 고 조사 하는 동안 사용할 수 있습니다. |  - |

## <a name="networking"></a>네트워킹

| 권장 | 주석 | Security Center |
|-|----|--|
|네트워크 노출 제한 | 네트워크 액세스는 자격 증명 모음 액세스를 요구 하는 솔루션에서 사용 하는 가상 네트워크로 제한 되어야 합니다. [Azure Key Vault에 대 한 Virtual network 서비스 끝점에 대 한](overview-vnet-service-endpoints.md)정보 검토) | - |

## <a name="next-steps"></a>다음 단계

응용 프로그램 공급자를 확인 하 여 추가 보안 요구 사항이 있는지 확인 합니다. 보안 응용 프로그램 개발에 대 한 자세한 내용은 [보안 개발 설명서](../../security/fundamentals/abstract-develop-secure-apps.md)를 참조 하세요.
