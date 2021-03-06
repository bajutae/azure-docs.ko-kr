---
title: Microsoft 상업적 marketplace의 SaaS 등록 Api
description: Microsoft AppSource 및 Azure Marketplace에서 SaaS 제품을 통합 하는 데 사용할 수 있는 처리 Api를 소개 합니다.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: ba1b158bc529b148a8e3138d122c13ead19e073e
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858078"
---
# <a name="saas-fulfillment-apis-in-microsoft-commercial-marketplace"></a>Microsoft 상업적 marketplace의 SaaS 등록 Api

SaaS 처리 Api를 통해 Isv (독립 소프트웨어 공급 업체)는 Microsoft AppSource 및 Azure Marketplace에서 SaaS 응용 프로그램을 통합할 수 있습니다. 이러한 Api를 통해 ISV 응용 프로그램은 direct, 파트너-led (대리점) 및 현장 led와 같은 모든 상거래 사용 채널에 참여할 수 있습니다. Microsoft AppSource 및 Azure Marketplace에서 불가능 SaaS 제품을 나열 하는 데 필요 합니다.

> [!WARNING]
> 이 API의 현재 버전은 모든 새 SaaS 제품에 사용 되는 버전 2입니다.  API의 버전 1은 더 이상 사용 되지 않으며 기존 제안을 지원 하기 위해 유지 관리 되 고 있습니다.

## <a name="business-model-support"></a>비즈니스 모델 지원

이 API는 다음과 같은 비즈니스 모델 기능을 지원 합니다. 당신은 할 수 있어요:

* 제안에 대 한 여러 계획을 지정 합니다. 이러한 계획은 다양 한 기능을 가지 며 가격이 다르게 책정 될 수 있습니다.
* 사이트별 또는 사용자 단위 청구 모델을 기준으로 제품을 제공 합니다.
* 매월 및 연간 (유료) 청구 옵션을 제공 합니다.
* 협상 된 비즈니스 계약에 따라 고객에 게 개인 가격을 제공 합니다.


## <a name="next-steps"></a>다음 단계

아직 수행 하지 않은 경우 [AZURE AD 응용 프로그램 등록](./pc-saas-registration.md)에 설명 된 대로 [Azure Portal](https://ms.portal.azure.com) 에서 SaaS 응용 프로그램을 등록 합니다.  그런 다음 개발용이 인터페이스의 최신 버전인 [SaaS 처리 API 버전 2](./pc-saas-fulfillment-api-v2.md)를 사용 합니다.
