---
title: 개발 도구
titleSuffix: Azure Data Science Virtual Machine
description: Data Science Virtual Machine에서 사용할 수 있는 도구 및 통합 개발 환경에 대해 알아봅니다.
keywords: 데이터 과학 도구, 데이터 과학 가상 머신, 데이터 과학용 도구, linux 데이터 과학
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c48cf6a7a82e90d3c9d8dc4c35e37dfb944af99f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80282684"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine의 개발 도구

DSVM (Data Science Virtual Machine)은 매우 생산적인 IDE (통합 개발 환경)에서 널리 사용 되는 여러 도구를 번들로 묶습니다. DSVM에서 제공되는 몇 가지 도구는 다음과 같습니다.

## <a name="visual-studio-community-edition"></a>Visual Studio Community Edition

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | 범용 IDE      |
| 지원 되는 DSVM 버전      | Windows: Visual Studio 2017, Windows 2019: Visual Studio 2019      |
| 일반적인 사용 용도      | 소프트웨어 개발    |
| DSVM에 구성 및 설치 하는 방법      | 데이터 과학 워크로드(Python 및 R 도구), Azure 워크로드(Hadoop, Data Lake), Node.js, SQL Server 도구, [Azure Machine Learning for Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| 사용 및 실행 방법      | 바탕 화면 바로`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`가기 () 그래픽, 바탕 화면 아이콘 또는 **시작** 메뉴를 사용 하 여 Visual Studio를 엽니다. 프로그램을 검색(Windows 로고 키+S)한 다음, **Visual Studio**를 찾습니다. 여기서는 C#, Python, R, Node.js와 같은 언어로 프로젝트를 만들 수 있습니다.   |
| DSVM의 관련 도구      |     Visual Studio Code, RStudio, Juno  |

> [!NOTE]
> 평가 기간이 만료되었다는 메시지가 표시될 수 있습니다. Microsoft 계정 자격 증명을 입력합니다. 또는 새 체험 계정을 만들어 Visual Studio Community에 액세스할 수 있습니다.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | 범용 IDE      |
| 지원 되는 DSVM 버전      | Windows, Linux     |
| 일반적인 사용 용도      | 코드 편집기 및 Git 통합   |
| 사용 및 실행 방법      | Windows의 바탕`C:\Program Files (x86)\Microsoft VS Code\Code.exe`화면 바로 가기 (), Linux의 바탕`code`화면 바로 가기 또는 터미널 ()    |
| DSVM의 관련 도구      |     Visual Studio, RStudio, Juno  |

## <a name="rstudio-desktop"></a>RStudio Desktop

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | R 언어용 클라이언트 IDE   |
| 지원 되는 DSVM 버전      | Windows, Linux      |
| 일반적인 사용 용도      |  R 개발     |
| 사용 및 실행 방법      | Windows의 바탕`C:\Program Files\RStudio\bin\rstudio.exe`화면 바로 가기 (), Linux`/usr/bin/rstudio`의 바탕 화면 바로 가기 ()      |
| DSVM의 관련 도구      |   Visual Studio, Visual Studio Code, Juno      |

## <a name="rstudio-server"></a>RStudio Server

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | R 언어용 클라이언트 IDE   |
| 무엇인가요?   | R용 웹 기반 IDE    |
| 지원 되는 DSVM 버전      | Linux      |
| 일반적인 사용 용도      |  R 개발     |
| 사용 및 실행 방법      | _Systemctl 사용 rstudio-server_를 사용 하 여 서비스를 사용 하도록 설정 하 고 _systemctl 시작 rstudio-server_를 사용 하 여 서비스를 시작 합니다. 그런 다음 http:\//avvv\8787과 같습니다에서 Rstudio 서버에 로그인 합니다.       |
| DSVM의 관련 도구      |   Visual Studio, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | Julia 언어용 클라이언트 IDE   |
| 지원 되는 DSVM 버전      | Windows, Linux      |
| 일반적인 사용 용도      |  Julia 개발     |
| 사용 및 실행 방법      | Windows의 바탕`C:\JuliaPro-0.5.1.1\Juno.bat`화면 바로 가기 (), Linux`/opt/JuliaPro-VERSION/Juno`의 바탕 화면 바로 가기 ()      |
| DSVM의 관련 도구      |   Visual Studio, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| 무엇인가요?   | Python 언어용 클라이언트 IDE    |
| 지원 되는 DSVM 버전      | Windows 2019, Linux      |
| 일반적인 사용 용도      |  Python 개발     |
| 사용 및 실행 방법      | Windows의 바탕`C:\Program Files\tk`화면 바로 가기 ()입니다. Linux의 바탕`/usr/bin/pycharm`화면 바로 가기 ()      |
| DSVM의 관련 도구      |   Visual Studio, Visual Studio Code, RStudio      |
