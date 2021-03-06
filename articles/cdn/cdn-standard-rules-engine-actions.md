---
title: Azure CDN에 대 한 표준 규칙 엔진의 작업 | Microsoft Docs
description: Azure Content Delivery Network에 대 한 표준 규칙 엔진 (Azure CDN)의 동작에 대 한 참조 설명서입니다.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: 29138b4fc6716ae5361cc4d7f97ceba41b90c2da
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81259955"
---
# <a name="actions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN에 대 한 표준 규칙 엔진의 작업

Azure Content Delivery Network (Azure CDN)에 대 한 [표준 규칙 엔진](cdn-standard-rules-engine.md) 에서 규칙은 하나 이상의 일치 조건 및 동작으로 구성 됩니다. 이 문서에서는 Azure CDN에 대 한 표준 규칙 엔진에서 사용할 수 있는 작업에 대해 자세히 설명 합니다.

규칙의 두 번째 부분은 동작입니다. 작업은 일치 조건 또는 일치 조건 집합에서 식별 하는 요청 유형에 적용 되는 동작을 정의 합니다.

## <a name="actions"></a>작업

Azure CDN에 대 한 표준 규칙 엔진에서 다음 작업을 사용할 수 있습니다. 

### <a name="cache-expiration"></a>캐시 만료

이 작업을 사용 하 여 규칙 일치 조건에서 지정 하는 요청에 대해 끝점의 TTL (time to live) 값을 덮어씁니다.

#### <a name="required-fields"></a>Required fields

캐시 동작 |  Description              
---------------|----------------
캐시 무시 | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 콘텐츠가 캐시 되지 않습니다.
재정의 | 이 옵션을 선택 하 고 규칙을 일치 시킬 때 원점에서 반환 된 TTL 값을 작업에 지정 된 값으로 덮어씁니다.
누락 된 경우 설정 | 이 옵션을 선택 하 고 규칙을 일치 시킬 때 원본에서 TTL 값이 반환 되지 않은 경우 규칙은 TTL을 작업에 지정 된 값으로 설정 합니다.

#### <a name="additional-fields"></a>추가 필드

일 | 시간 | 분 | 초
-----|-------|---------|--------
Int | Int | Int | Int 

### <a name="cache-key-query-string"></a>캐시 키 쿼리 문자열

이 작업을 사용 하 여 쿼리 문자열에 따라 캐시 키를 수정할 수 있습니다.

#### <a name="required-fields"></a>Required fields

동작 | Description
---------|------------
포함 | 이 옵션을 선택 하 고 규칙이 일치 하면 캐시 키가 생성 될 때 매개 변수에 지정 된 쿼리 문자열이 포함 됩니다. 
모든 고유한 URL 캐시 | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 각 고유 URL에는 자체 캐시 키가 있습니다. 
제외 | 이 옵션을 선택 하 고 규칙이 일치 하면 캐시 키가 생성 될 때 매개 변수에 지정 된 쿼리 문자열이 제외 됩니다.
쿼리 문자열 무시 | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 캐시 키가 생성 될 때 쿼리 문자열이 고려 되지 않습니다. 

### <a name="modify-request-header"></a>요청 헤더 수정

이 작업을 사용 하면 원본에 전송 된 요청에 있는 헤더를 수정할 수 있습니다.

#### <a name="required-fields"></a>Required fields

작업 | HTTP 헤더 이름 | 값
-------|------------------|------
추가 | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 **헤더 이름** 에 지정 된 헤더가 지정 된 값을 가진 요청에 추가 됩니다. 헤더가 이미 있으면 값이 기존 값에 추가 됩니다. | 문자열
Overwrite | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 **헤더 이름** 에 지정 된 헤더가 지정 된 값을 가진 요청에 추가 됩니다. 헤더가 이미 있는 경우 지정 된 값이 기존 값을 덮어씁니다. | 문자열
삭제 | 이 옵션을 선택 하면 규칙이 일치 하 고 규칙에 지정 된 헤더가 표시 되 고 헤더가 요청에서 삭제 됩니다. | 문자열

### <a name="modify-response-header"></a>응답 헤더 수정

이 작업을 사용 하 여 클라이언트에 반환 되는 응답에 있는 헤더를 수정할 수 있습니다.

#### <a name="required-fields"></a>Required fields

작업 | HTTP 헤더 이름 | 값
-------|------------------|------
추가 | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 지정 된 **값**을 사용 하 여 **헤더 이름** 에 지정 된 헤더를 응답에 추가 합니다. 헤더가 이미 있으면 **값** 이 기존 값에 추가 됩니다. | 문자열
Overwrite | 이 옵션을 선택 하 고 규칙을 일치 시킬 경우 지정 된 **값**을 사용 하 여 **헤더 이름** 에 지정 된 헤더를 응답에 추가 합니다. 헤더가 이미 있는 경우 **값** 은 기존 값을 덮어씁니다. | 문자열
삭제 | 이 옵션을 선택 하면 규칙이 일치 하 고 규칙에 지정 된 헤더가 있는 경우 헤더는 응답에서 삭제 됩니다. | 문자열

### <a name="url-redirect"></a>URL 리디렉션

이 작업을 사용 하 여 클라이언트를 새 URL로 리디렉션합니다. 

#### <a name="required-fields"></a>Required fields

필드 | Description 
------|------------
Type | 요청자에 게 반환할 응답 형식을 선택 합니다. (302), 이동한 (301), 임시 리디렉션 (307) 및 영구 리디렉션 (308).
프로토콜 | 요청, HTTP, HTTPS를 일치 시킵니다.
Hostname | 요청을 리디렉션할 호스트 이름을 선택 합니다. 들어오는 호스트를 유지 하려면 비워 두세요.
경로 | 리디렉션에 사용할 경로를 정의 합니다. 들어오는 경로를 유지 하려면 비워 두십시오.  
쿼리 문자열 | 리디렉션에 사용 되는 쿼리 문자열을 정의 합니다. 들어오는 쿼리 문자열을 유지 하려면 비워 두십시오. 
조각 | 리디렉션에 사용할 조각을 정의 합니다. 들어오는 조각을 유지 하려면 비워 두십시오. 

절대 URL을 사용 하는 것이 좋습니다. 상대 URL을 사용 하면 Azure CDN Url을 잘못 된 경로로 리디렉션할 수 있습니다. 

### <a name="url-rewrite"></a>URL 다시 쓰기

원본으로 라우팅하는 요청의 경로를 다시 작성 하려면이 작업을 사용 합니다.

#### <a name="required-fields"></a>Required fields

필드 | Description 
------|------------
원본 패턴 | 대체할 URL 경로에서 원본 패턴을 정의 합니다. 현재 소스 패턴은 접두사 기반 일치를 사용 합니다. 모든 URL 경로를 일치 시키려면 소스 패턴 값으로 슬래시**/**()를 사용 합니다.
대상 | 재작성에 사용할 대상 경로를 정의 합니다. 대상 경로는 원본 패턴을 덮어씁니다.
일치 하지 않는 경로 유지 | **예**로 설정 된 경우 소스 패턴 뒤의 나머지 경로는 새 대상 경로에 추가 됩니다. 

## <a name="next-steps"></a>다음 단계

- [Azure CDN 개요](cdn-overview.md)
- [표준 규칙 엔진 참조](cdn-standard-rules-engine-reference.md)
- [표준 규칙 엔진의 일치 조건](cdn-standard-rules-engine-match-conditions.md)
- [표준 규칙 엔진을 사용하여 HTTPS 적용](cdn-standard-rules-engine.md)
