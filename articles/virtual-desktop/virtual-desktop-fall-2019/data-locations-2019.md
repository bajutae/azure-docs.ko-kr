---
title: Windows 가상 데스크톱에 대 한 데이터 위치-Azure
description: Windows 가상 데스크톱의 데이터 및 메타 데이터가 저장 되는 위치에 대 한 간략 한 개요입니다.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b8cbee74a8f9639fc2c3838f38dabf2a62cf339e
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614448"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Windows 가상 데스크톱에 대 한 데이터 위치

>[!IMPORTANT]
>이 콘텐츠는 Windows 가상 데스크톱 개체 Azure Resource Manager를 지원 하지 않는 낙하 2019 릴리스에 적용 됩니다. 스프링 2020 업데이트에 도입 된 Azure Resource Manager Windows 가상 데스크톱 개체를 관리 하려는 경우 [이 문서](../data-locations.md)를 참조 하세요.

Windows 가상 데스크톱은 현재 모든 지리적 위치에서 사용할 수 있습니다. 처음에는 서비스 메타 데이터를 미국 (미국) 지리에만 저장할 수 있습니다. 관리자는 호스트 풀 가상 컴퓨터 및 관련 서비스 (예: 파일 서버)를 만들 때 사용자 데이터를 저장할 위치를 선택할 수 있습니다. Azure [데이터 센터 맵에서](https://azuredatacentermap.azurewebsites.net/)azure 지역에 대해 자세히 알아보세요.

>[!NOTE]
>Microsoft는 사용자 또는 사용자가 사용자와 앱 특정 데이터에 액세스할 수 있는 지역을 제어 하거나 제한 하지 않습니다.

>[!IMPORTANT]
>Windows 가상 데스크톱은 미국에 있는 데이터 센터에 테 넌 트 이름, 호스트 풀 이름, 앱 그룹 이름 및 사용자 계정 이름과 같은 글로벌 메타 데이터 정보를 저장 합니다. 저장 된 메타 데이터는 미사용 상태에서 암호화 되 고 지역 중복 미러는 미국 내에서 유지 관리 됩니다. 앱 설정 및 사용자 데이터와 같은 모든 고객 데이터는 고객이 선택한 위치에 상주 하며 서비스에서 관리 되지 않습니다.

서비스 메타 데이터는 재해 복구를 위해 미국에서 복제 됩니다.