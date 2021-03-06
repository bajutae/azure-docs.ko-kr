---
title: 사용자 지정 정책을 사용 하는 Single sign-on 세션 관리
titleSuffix: Azure AD B2C
description: Azure AD B2C에서 사용자 지정 정책을 사용하여 SSO 세션을 관리하는 방법에 대해 알아봅니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4aa9f4839c8bfc04cee4bb03ea0eac98cb8b25c0
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82926122"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Azure Active Directory B2C에서 Single Sign-On 세션 관리

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Sso (Single sign-on) 세션](session-overview.md) 관리에서는 사용자 지정 정책에서 다른 기술 프로필과 동일한 의미 체계를 사용 합니다. 오케스트레이션 단계를 실행하면 이 단계와 관련된 기술 프로필을 `UseTechnicalProfileForSessionManagement` 참조에 대해 쿼리합니다. 참조된 SSO 세션 공급자가 있는 경우 사용자가 세션 참가자인지를 확인합니다. 그렇다면 SSO 세션 공급자를 사용하여 세션을 다시 채웁니다. 마찬가지로 오케스트레이션 단계의 실행이 완료되면 SSO 세션 공급자가 지정된 경우 이 공급자를 사용하여 세션에서 정보를 저장합니다.

Azure AD B2C는 사용할 수 있는 SSO 세션 공급자 수를 정의합니다.

|세션 공급자  |Scope  |
|---------|---------|
|[NoopSSOSessionProvider](#noopssosessionprovider)     |  없음       |       
|[DefaultSSOSessionProvider](#defaultssosessionprovider)    | 내부 세션 관리자를 Azure AD B2C 합니다.      |       
|[ExternalLoginSSOSessionProvider](#externalloginssosessionprovider)     | Azure AD B2C과 OAuth1, OAuth2 또는 Openid connect Connect id 공급자 사이에 있습니다.        |         |
|[OAuthSSOSessionProvider](#oauthssosessionprovider)     | OAuth2 또는 Openid connect connect 신뢰 당사자 응용 프로그램과 Azure AD B2C 사이        |        
|[SamlSSOSessionProvider](#samlssosessionprovider)     | Azure AD B2C와 SAML id 공급자 사이 SAML 서비스 공급자 (신뢰 당사자 응용 프로그램)와 Azure AD B2C 사이에 있습니다.  |        




기술 프로필의 `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />` 요소를 사용하여 SSO 관리 클래스를 지정합니다.

## <a name="input-claims"></a>입력 클레임

`InputClaims` 요소가 비어 있거나 없습니다.

## <a name="persisted-claims"></a>지속형 클레임

응용 프로그램에 반환 되거나 후속 단계에서 사전 조건에서 사용 되어야 하는 클레임은 세션에 저장 되거나 디렉터리의 사용자 프로필에서 읽은 후에 야 합니다. 지속형 클레임을 사용 하면 인증 경험 누락 된 클레임에서 실패 하지 않습니다. 세션에서 클레임을 추가하려면 기술 프로필의 `<PersistedClaims>` 요소를 사용합니다. 세션을 다시 채우는 데 공급자를 사용하는 경우 유지되는 클레임은 클레임 모음에 추가됩니다.

## <a name="output-claims"></a>출력 클레임

는 `<OutputClaims>` 세션에서 클레임을 검색 하는 데 사용 됩니다.

## <a name="session-providers"></a>세션 공급자

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

이름이 지정하는 대로 이 공급자는 아무 작업도 수행하지 않습니다. 특정 기술 프로필에 대한 SSO 동작을 무시하기 위해 이 공급자를 사용할 수 있습니다. 다음 `SM-Noop` 기술 프로필은 [사용자 지정 정책 시작 팩](custom-policy-get-started.md#custom-policy-starter-pack)에 포함 되어 있습니다.

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

세션에서 클레임을 저장하는 데 이 공급자를 사용할 수 있습니다. 일반적으로이 공급자는 로컬 및 페더레이션된 계정을 관리 하는 데 사용 되는 기술 프로필에서 참조 됩니다. 다음 `SM-AAD` 기술 프로필은 [사용자 지정 정책 시작 팩](custom-policy-get-started.md#custom-policy-starter-pack)에 포함 되어 있습니다.

```XML
<TechnicalProfile Id="SM-AAD">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="signInName" />
    <PersistedClaim ClaimTypeReferenceId="authenticationSource" />
    <PersistedClaim ClaimTypeReferenceId="identityProvider" />
    <PersistedClaim ClaimTypeReferenceId="newUser" />
    <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```


다음 `SM-MFA` 기술 프로필은 [사용자 지정 정책 시작 팩](custom-policy-get-started.md#custom-policy-starter-pack) `SocialAndLocalAccountsWithMfa`에 포함 되어 있습니다. 이 기술 프로필은 multi-factor authentication 세션을 관리 합니다.

```XML
<TechnicalProfile Id="SM-MFA">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isActiveMFASession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

이 공급자는 "id 공급자 선택" 화면을 표시 하지 않고 페더레이션 id 공급자에서 로그 아웃 하는 데 사용 됩니다. 일반적으로 Facebook 또는 Azure Active Directory와 같은 페더레이션된 id 공급자에 대해 구성 된 기술 프로필에서 참조 됩니다. 다음 `SM-SocialLogin` 기술 프로필은 [사용자 지정 정책 시작 팩](custom-policy-get-started.md#custom-policy-starter-pack)에 포함 되어 있습니다.

```XML
<TechnicalProfile Id="SM-SocialLogin">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="AlwaysFetchClaimsFromProvider">true</Item>
  </Metadata>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="AlternativeSecurityId" />
  </PersistedClaims>
</TechnicalProfile>
```

#### <a name="metadata"></a>메타데이터

| 특성 | 필수 | Description|
| --- | --- | --- |
| AlwaysFetchClaimsFromProvider | 예 | 현재 사용 되지 않습니다 .를 무시할 수 있습니다. |

### <a name="oauthssosessionprovider"></a>OAuthSSOSessionProvider

이 공급자는 OAuth2 또는 Openid connect Connect 신뢰 당사자와 Azure AD B2C 간의 Azure AD B2C 세션을 관리 하는 데 사용 됩니다.

```xml
<TechnicalProfile Id="SM-jwt-issuer">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

이 공급자는 신뢰 당사자 응용 프로그램 또는 페더레이션된 SAML id 공급자 간의 Azure AD B2C SAML 세션을 관리 하는 데 사용 됩니다. SSO 공급자를 사용 하 여 SAML id 공급자 세션을 저장 하는 `RegisterServiceProviders` 경우를로 `false`설정 해야 합니다. `SM-Saml-idp` [SAML id 공급자 기술 프로필](saml-identity-provider-technical-profile.md)에서 사용 하는 기술 프로필은 다음과 같습니다.

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

B2C SAML 세션을 저장 하는 데 공급자를 사용 하 `RegisterServiceProviders` 는 경우은 `true`로 설정 되어야 합니다. SAML 세션 로그아웃을 완료하는 데 `SessionIndex` 및 `NameID`가 필요합니다.

`SM-Saml-issuer` [SAML 발급자 기술 프로필](saml-issuer-technical-profile.md) 에서 사용 하는 기술 프로필은 다음과 같습니다.

```XML
<TechnicalProfile Id="SM-Saml-issuer">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```

#### <a name="metadata"></a>메타데이터

| 특성 | 필수 | Description|
| --- | --- | --- |
| IncludeSessionIndex | 예 | 현재 사용 되지 않습니다 .를 무시할 수 있습니다.|
| RegisterServiceProviders | 예 | 공급자가 어설션을 발급한 모든 SAML 서비스 공급자를 등록해야 함을 의미합니다. 가능한 값은 `true`(기본값) 또는 `false`입니다.|


## <a name="next-steps"></a>다음 단계

- [Azure AD B2C 세션](session-overview.md)에 대해 자세히 알아보세요.
- [사용자 지정 정책에서 세션 동작을 구성](session-behavior-custom-policy.md)하는 방법에 대해 알아봅니다.
