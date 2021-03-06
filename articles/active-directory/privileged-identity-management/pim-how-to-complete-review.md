---
title: PIM에서 Azure AD 역할의 액세스 검토 완료-Azure AD | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM)에서 Azure AD 역할에 대 한 액세스 검토를 완료 하 고 결과를 확인 하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe2d85d605b9ee418a5709ddcdb448c56be1d918
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74022275"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management에서 Azure AD 역할에 대 한 액세스 검토를 완료 합니다.

[액세스 검토가 시작](pim-how-to-start-security-review.md)되면 권한 있는 역할 관리자가 권한이 있는 액세스를 검토할 수 있습니다.  PIM (Privileged Identity Management)은 사용자에 게 액세스를 검토 하도록 요청 하는 Azure Active Directory (Azure AD) 조직의 사용자에 게 전자 메일을 자동으로 보냅니다. 사용자가 메일을 받지 못한 경우 [액세스 검토를 수행하는 방법](pim-how-to-perform-security-review.md)에 대한 지침을 보낼 수 있습니다.

액세스 검토 기간이 끝나거나 모든 사용자가 자체 검토를 완료하면 이 문서의 단계에 따라 검토를 관리하고 결과를 표시합니다.

## <a name="manage-access-reviews"></a>액세스 검토 관리

1. [Azure Portal](https://portal.azure.com/) 로 이동 하 고 대시보드에서 **Azure AD Privileged Identity Management** 서비스를 선택 합니다.
1. 대시보드의 **액세스 검토** 섹션을 선택합니다.
1. 관리하려는 액세스 검토를 선택합니다.

액세스 검토의 세부 정보 블레이드에는 검토를 관리 하는 다양 한 옵션이 있습니다.

![Privileged Identity Management 액세스 검토 단추-스크린샷](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>알림

액세스 검토가 설정되어 사용자가 자체적으로 검토하는 경우 **알림** 단추가 알림을 보냅니다.

### <a name="stop"></a>중지

모든 액세스 검토는 종료 날짜가 있지만, **중지** 단추를 사용하여 일찍 완료할 수 있습니다. 만일 이 때까지 검토하지 않은 사용자가 있다면, 검토를 중지한 후에는 그들을 검토할 수 없습니다. 검토를 중단한 후에는 다시 시작할 수 없습니다.

### <a name="apply"></a>적용

종료 날짜가 되었거나 수동으로 중지하여 액세스 검토가 완료되면, **적용** 단추가 검토 결과를 구현합니다. 사용자의 액세스가 검토 중에 거부되었다면, 이는 그들의 역할 할당을 제거할 단계입니다.  

### <a name="export"></a>내보내기

액세스 검토 결과를 수동으로 적용하려는 경우 검토를 내보낼 수 있습니다. **내보내기** 단추는 CSV 파일 다운로드를 시작합니다. Excel 또는 CSV 파일을 여는 다른 프로그램에서 결과를 관리할 수 있습니다.

### <a name="delete"></a>삭제

더 이상 검토가 필요 없는 경우 검토를 삭제합니다. **삭제** 단추는 Privileged Identity Management 서비스에서 검토를 제거 합니다.

> [!IMPORTANT]
> 이 파괴적인 변경을 확인 하는 것은 필요 하지 않으므로 검토를 삭제할 것인지 확인 합니다.

## <a name="next-steps"></a>다음 단계

- [Privileged Identity Management에서 Azure AD 역할에 대 한 액세스 검토를 시작 합니다.](pim-how-to-start-security-review.md)
- [Privileged Identity Management에서 내 Azure AD 역할에 대 한 액세스 검토를 수행 합니다.](pim-how-to-perform-security-review.md)
