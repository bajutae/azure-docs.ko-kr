---
title: LinkedIn 계정으로 등록 및 로그인 설정
titleSuffix: Azure AD B2C
description: 고객에게 Azure Active Directory B2C를 사용하여 애플리케이션에서 LinkedIn 계정으로 등록 및 로그인을 제공합니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d32eb80ffe296d86164a6d27a1b7a28181357243
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188103"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C를 사용하여 LinkedIn 계정으로 등록 설정 및 로그인

## <a name="create-a-linkedin-application"></a>LinkedIn 애플리케이션 만들기

LinkedIn 계정을 Azure Active Directory B2C (Azure AD B2C)에서 [id 공급자로](authorization-code-flow.md) 사용 하려면 테 넌 트에서 응용 프로그램을 나타내는 응용 프로그램을 만들어야 합니다. LinkedIn 계정이 아직 없는 경우에서 [https://www.linkedin.com/](https://www.linkedin.com/)등록할 수 있습니다.

1. LinkedIn 계정 자격 증명을 사용하여 [LinkedIn 개발자 웹 사이트](https://www.developer.linkedin.com/)에 로그인합니다.
1. **내 앱**을 선택하고 **애플리케이션 만들기**를 클릭합니다.
1. **회사 이름**, **애플리케이션 이름**, **애플리케이션 설명**, **애플리케이션 로고**, **애플리케이션 사용**, **웹 사이트 URL**, **회사 전자 메일** 및 **회사 전화**를 입력합니다.
1. **LinkedIn API 사용 조건**에 동의하고 **제출**을 클릭합니다.
1. **클라이언트 ID** 및 **클라이언트 암호**의 값을 복사 합니다. **인증 키** 아래에서 찾을 수 있습니다. 테넌트에서 LinkedIn을 ID 공급자로 구성하려면 둘 다 필요합니다. **클라이언트 보안 비밀**은 중요한 보안 자격 증명이므로
1. **권한이 부여된 리디렉션 URL**에 `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`를 입력합니다. `your-tenant-name`을 테넌트 이름으로 바꿉니다. Azure AD B2C에서 테넌트가 대문자로 정의되어 있더라도 테넌트 이름을 입력할 때는 소문자만 사용해야 합니다. **추가**를 선택한 후 **업데이트**를 클릭합니다.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>LinkedIn 계정을 ID 공급자로 구성

1. [Azure Portal](https://portal.azure.com/) 에 Azure AD B2C 테 넌 트의 전역 관리자로 로그인 합니다.
1. Azure AD B2C 테넌트를 포함하는 디렉터리를 사용하려면 위쪽 메뉴에서 **디렉터리 + 구독** 필터를 선택하고, 테넌트가 포함된 디렉터리를 선택합니다.
1. Azure Portal의 왼쪽 상단 모서리에서 **모든 서비스**를 선택하고 **Azure AD B2C**를 검색하여 선택합니다.
1. **Id 공급자**를 선택한 다음 **LinkedIn**을 선택 합니다.
1. **이름을**입력 합니다. 예: *LinkedIn*.
1. **클라이언트 id**에 대해 이전에 만든 LinkedIn 응용 프로그램의 클라이언트 id를 입력 합니다.
1. **클라이언트 암호**에 대해 기록한 클라이언트 암호를 입력 합니다.
1. **저장**을 선택합니다.

## <a name="migration-from-v10-to-v20"></a>V 1.0에서 v2.0로 마이그레이션

LinkedIn [은 최근에 해당 api를 v1.0에서 v 2.0으로 업데이트 했습니다](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). 마이그레이션의 일부로 Azure AD B2C는 등록 중에 LinkedIn 사용자의 전체 이름만 가져올 수 있습니다. 등록 하는 동안 수집 되는 특성 중 하나에 해당 하는 전자 메일 주소는 사용자가 전자 메일 주소를 수동으로 입력 하 고 유효성을 검사 해야 합니다.
