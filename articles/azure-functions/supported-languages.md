---
title: Azure Functions에서 지원되는 언어
description: 어떤 언어가 지원되는 언어(GA)이고 어떤 언어가 실험적 또는 미리 보기인지 알아봅니다.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 029ea753439dca3093bf214a5adfb6d58a1fe567
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74942259"
---
# <a name="supported-languages-in-azure-functions"></a>Azure Functions에서 지원되는 언어

이 문서에서는 Azure Functions에서 사용할 수 있는 언어에 대해 제공되는 지원 수준을 설명합니다.

## <a name="levels-of-support"></a>지원 수준

세 가지 지원 수준이 있습니다.

* **일반 공급(GA)** - 완전하게 지원되며 프로덕션용으로 승인되었습니다.
* **미리 보기** - 아직 지원되지 않지만 향후 GA 상태가 될 예정입니다.
* **실험적** - 지원되지 않으며 향후 중단될 수 있습니다. 최종 미리 보기 또는 GA 상태가 보장되지 않습니다.

## <a name="languages-by-runtime-version"></a>런타임 버전별 언어 

[세 가지 버전의 Azure Functions 런타임을](functions-versions.md) 사용할 수 있습니다. 다음 표는 각 런타임 버전에서 지원되는 언어를 나타냅니다.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>실험적 언어

버전 1.x에서 실험적 언어는 제대로 확장되지 않고 모든 바인딩을 지원하지도 않습니다.

실험적 기능에 대해서는 공식적인 지원이 없으므로 기반으로 사용하지 말아야 합니다. 실험적 언어 관련 문제에 대한 지원 사례를 열 수는 없습니다. 

이후 런타임 버전은 실험적 언어를 지원 하지 않습니다. 프로덕션 환경에서 언어를 지원할 수 있는 경우만 새 언어에 대한 지원을 추가합니다. 

### <a name="language-extensibility"></a>언어 확장성

버전 2.x부터 런타임은 [언어 확장성](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility)을 제공 하도록 설계 되었습니다. 2.x 런타임의 JavaScript 및 Java 언어는 이 확장성을 사용하여 빌드됩니다.

## <a name="next-steps"></a>다음 단계

지원 되는 언어로 함수를 개발 하는 방법에 대해 자세히 알아보려면 다음 리소스를 참조 하세요.

+ [C # 클래스 라이브러리 개발자 참조](functions-dotnet-class-library.md)
+ [C # 스크립트 개발자 참조](functions-reference-csharp.md)
+ [Java 개발자 참조](functions-reference-java.md)
+ [JavaScript 개발자 참조](functions-reference-node.md)
+ [PowerShell 개발자 참조](functions-reference-powershell.md)
+ [Python 개발자 참조](functions-reference-python.md)
+ [TypeScript 개발자 참조](functions-reference-node.md#typescript)
