---
title: Azure Active Directory 포털에서 그룹 목록 다운로드 | Microsoft Docs
description: Azure Active Directory에서 Azure 관리 센터에 대량으로 그룹 속성을 다운로드 합니다.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59983678c1b14d6aa87a7b500605e3abeb6a9b85
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81533544"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Azure Active Directory에서 그룹 목록 대량 다운로드

Azure Active Directory (Azure AD) 포털을 사용 하 여 조직에 있는 모든 그룹의 목록을 쉼표로 구분 된 값 (CSV) 파일로 대량 다운로드할 수 있습니다.

## <a name="to-download-a-list-of-groups"></a>그룹 목록을 다운로드 하려면

1. 조직의 관리자 계정으로 [Azure Portal](https://portal.azure.com) 에 로그인 합니다.
1. Azure AD에서 **그룹** > **다운로드 그룹**을 선택 합니다.
1. **그룹 다운로드** 페이지에서 **시작** 을 선택 하 여 그룹을 나열 하는 CSV 파일을 받습니다.

   ![그룹 다운로드 명령은 모든 그룹 페이지에 있습니다.](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>다운로드 상태 확인

**대량 작업 결과** 페이지에서 보류 중인 모든 대량 요청의 상태를 볼 수 있습니다.

[![](media/groups-bulk-download/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>대량 다운로드 서비스 제한

그룹 목록을 다운로드 하는 각 대량 작업은 최대 1 시간 동안 실행할 수 있습니다. 이를 통해 최소 30만 그룹 목록을 다운로드할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [그룹 구성원 대량 제거](groups-bulk-remove-members.md)
- [그룹 구성원 다운로드](groups-bulk-download-members.md)
