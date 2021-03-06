---
title: Azure Synapse Analytics를 사용하여 Apache Spark에 .NET 사용
description: .NET 및 Apache Spark를 사용하여 Azure Synapse Analytics Notebook에서 일괄 처리, 실시간 스트리밍, 기계 학습 및 임시 쿼리 작성을 수행하는 방법에 대해 알아봅니다.
author: mamccrea
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: mamccrea
ms.reviewer: jrasnick
ms.openlocfilehash: 3882352c7e1d484818a58d7bd4410cbd66bd6637
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587802"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Azure Synapse Analytics를 사용하여 Apache Spark에 .NET 사용

[Apache Spark용 .NET](https://dot.net/spark)은 Spark에 대한 무료, 오픈소스 및 플랫폼 간 .NET 지원입니다. Apache Spark용 .NET에서는 C# 및 F#을 통해 Spark API에 액세스할 수 있도록 Spark에 대한 .NET 바인딩을 제공합니다. Apache Spark용 .NET을 사용하면 .NET을 사용하여 Spark용 사용자 정의 함수를 작성하고 실행할 수 있습니다. Spark용 .NET API를 사용하면 Spark SQL 및 구조적 스트리밍을 포함하여 데이터를 분석하는 데 도움이 되는 Spark의 모든 요소에 액세스할 수 있습니다.

Spark 일괄 작업 정의 또는 대화형 Azure Synapse Analytics Notebook을 통해 Apache Spark용 .NET을 사용하여 데이터를 분석할 수 있습니다. 이 문서에서는 두 가지 기법을 사용하여 Azure Synapse에서 Apache Spark용 .NET을 사용하는 방법에 대해 알아봅니다.

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Spark 작업 정의를 사용하여 일괄 작업 제출

이 자습서를 방문하여 Azure Synapse Analytics를 사용하여 [Synapse Spark 풀에 대한 Apache Spark 작업 정의를 만드는 방법](apache-spark-job-definitions.md)을 알아보세요. 앱을 패키징하여 Azure Synapse에 제출하지 않은 경우 다음 단계를 완료합니다.

1. 다음 명령을 실행하여 앱을 게시합니다. *mySparkApp*은 해당 앱의 경로로 바꾸어야 합니다.

   **Windows:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

   **Linux:**

### <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>Azure Synapse Analytics Notebook의 Apache Spark용 .NET

새 Notebook을 만들 때는 비즈니스 논리를 표현할 언어 커널을 선택합니다. C#을 비롯한 여러 언어에 대한 커널 지원이 있습니다.

Azure Synapse Analytics Notebook에서 Apache Spark용 .NET을 사용하려면 **.NET Spark(C#)** 를 커널로 선택하고 Notebook을 기존 Spark 풀에 연결합니다.

   ```bash
   zip -r publish.zip
   ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>Azure Synapse Analytics Notebook의 Apache Spark용 .NET 

Notebook은 Apache Spark 파이프라인과 시나리오에 맞게 .NET 프로토타입을 제작하는 데 유용한 옵션입니다. 데이터를 빠르고 효율적으로 작업하고, 이해하고, 필터링하고, 표시하고, 시각화할 수 있습니다. 데이터 엔지니어, 데이터 과학자, 비즈니스 분석가 및 기계 학습 엔지니어는 모두 공유 대화형 문서를 통해 협업을 수행할 수 있습니다. 데이터 탐색의 즉각적인 결과를 볼 수 있으며 동일한 Notebook에서 데이터를 시각화할 수 있습니다.

### <a name="how-to-use-notebooks"></a>Notebook을 사용하는 방법

새 Notebook을 만들 때는 비즈니스 논리를 표현할 언어 커널을 선택합니다. C#을 비롯한 여러 언어에 대한 커널 지원이 있습니다.

Azure Synapse Analytics Notebook에서 Apache Spark용 .NET을 사용하려면 **.NET Spark(C#)** 를 커널로 선택하고 Notebook을 기존 Spark 풀에 연결합니다.

.Net Spark Notebook은 .NET 대화형 환경을 기반으로 하며, 기본적으로 Spark 세션 변수 `spark`가 미리 정의된 Spark용 .NET을 사용할 수 있는 대화형 C# 환경을 제공합니다.

### <a name="sparknet-c-kernel-features"></a>Spark.NET C# 커널 기능

Azure Synapse Analytics Notebook에서 Apache Spark용 .NET을 사용하는 경우 다음 기능을 사용할 수 있습니다.

* 선언적 HTML: 헤더, 글머리 기호 목록 및 표시 이미지와 같은 HTML 구문을 사용하여 셀에서 출력을 생성합니다.
* 간단한 C# 문(예: 할당, 콘솔에 인쇄, 예외 throw 등)
* 여러 줄로 된 C# 코드 블록(예: if 문, foreach 루프, 클래스 정의 등)
* 표준 C# 라이브러리(예: System, LINQ, 열거형 등)에 대한 액세스
* [C# 8.0 언어 기능](/dotnet/csharp/whats-new/csharp-8?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 지원
* Apache Spark 세션에 대한 액세스를 제공하는 'spark'라는 미리 정의된 변수
* [Apache Spark 내에서 실행할 수 있는 .NET 사용자 정의 함수](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql)를 정의하는 기능 지원
* `XPlot.Plotly` 라이브러리에서 다양한 차트(예: 꺾은선형, 가로 막대형 또는 히스토그램) 및 레이아웃(예: 단일, 오버레이 등)을 사용하여 Spark 작업의 출력을 시각화하는 기능 지원
* NuGet 패키지를 C# Notebook에 포함하는 기능

## <a name="next-steps"></a>다음 단계

* [Apache Spark용 .NET 설명서](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
* [.NET Interactive](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)