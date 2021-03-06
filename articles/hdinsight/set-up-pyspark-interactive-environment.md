---
title: Azure HDInsight Tools를 사용 하 여 대화형 환경 PySpark
description: Azure HDInsight Tools for Visual Studio Code를 사용하여 쿼리와 스크립트를 만들고 제출하는 방법에 대해 알아봅니다.
keywords: VScode,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,대화형 Hive,대화형 쿼리
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/23/2020
ms.openlocfilehash: 47f774f7b848e34aa7dc219ee4b08fb083cbd813
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188211"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code용 PySpark 대화형 환경 설정

다음 단계에서는 VS Code에서 PySpark 대화형 환경을 설정하는 방법을 보여 줍니다.

**python/pip** 명령을 사용하여 홈 경로에서 가상 환경을 빌드합니다. 다른 버전을 사용하려면 **python/pip** 명령의 기본 버전을 수동으로 변경해야 합니다. 자세한 내용은 [update-alternatives](https://linux.die.net/man/8/update-alternatives)를 참조하세요.

1. [Python](https://www.python.org/downloads/) 및 [pip](https://pip.pypa.io/en/stable/installing/)를 설치 합니다.

   * 에서 Python을 [https://www.python.org/downloads/](https://www.python.org/downloads/)설치 합니다.
   * (Python 설치 [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) 에서 설치 되지 않은 경우)에서 pip를 설치 합니다.
   * 다음 명령을 사용 하 여 Python 및 pip가 성공적으로 설치 되었는지 확인 합니다. (선택 사항)

        ![Python pip 버전 명령 확인](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > MacOS 기본 버전을 사용 하는 대신 Python을 수동으로 설치 하는 것이 좋습니다.

2. 아래 명령을 실행하여 **virtualenv**를 설치합니다.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>기타 패키지

오류 메시지가 표시 되 면 다음 명령을 실행 하 여 필요한 패키지를 설치 합니다.

   ![Python 용 libkrb5-dev 패키지 설치](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

VS Code를 다시 시작하고 **HDInsight: PySpark 대화형**을 실행하는 스크립트 편집기로 돌아옵니다.

## <a name="next-steps"></a>다음 단계

### <a name="demo"></a>데모

* HDInsight for VS Code: [비디오](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>도구 및 확장

* [Azure HDInsight Tool for Visual Studio Code 사용](hdinsight-for-vscode.md)
* [IntelliJ용 Azure 도구 키트를 사용하여 Apache Spark Scala 애플리케이션 만들기 및 제출](spark/apache-spark-intellij-tool-plugin.md)
* [컴퓨터에 Jupyter를 설치하고 HDInsight Spark 클러스터에 연결](spark/apache-spark-jupyter-notebook-install-locally.md)
