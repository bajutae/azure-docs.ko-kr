---
title: 재사용 가능한 ML 환경 만들기
titleSuffix: Azure Machine Learning
description: 모델 학습 및 배포를 위한 환경을 만들고 관리 합니다. 환경에 대 한 Python 패키지 및 기타 설정을 관리 합니다.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: dc30318027962247f7504b734385d7642550ec87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536355"
---
# <a name="reuse-environments-for-training-and-deployment-by-using-azure-machine-learning"></a>Azure Machine Learning를 사용 하 여 학습 및 배포를 위한 환경 재사용
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

이 문서에서는 Azure Machine Learning [환경을](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)만들고 관리 하는 방법에 대해 알아봅니다. 환경을 사용 하면 프로젝트의 소프트웨어 종속성이 진화 함에 따라 추적 하 고 재현할 수 있습니다.

소프트웨어 종속성 관리는 개발자를 위한 일반적인 작업입니다. 광범위 한 수동 소프트웨어 구성 없이 빌드를 재현할 수 있는지 확인 하려고 합니다. Azure Machine Learning `Environment` 클래스는 Pip 및 Conda와 같은 로컬 개발 솔루션에 대 한 계정을 제공 하 고 로컬 및 분산 클라우드 개발을 위한 솔루션을 제공 합니다.

이 문서의 예제에서는 다음 작업을 수행 하는 방법을 보여 줍니다.

* 환경을 만들고 패키지 종속성을 지정 합니다.
* 환경을 검색 하 고 업데이트 합니다.
* 학습을 위해 환경을 사용 합니다.
* 웹 서비스 배포를 위한 환경을 사용 합니다.

Azure Machine Learning에서 환경의 작동 방식에 대 한 개략적인 개요는 [ML 환경 이란?](concept-environments.md)을 참조 하세요.

## <a name="prerequisites"></a>전제 조건

* [Python 용 AZURE MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning 작업 영역](how-to-manage-workspace.md)

## <a name="create-an-environment"></a>환경 만들기

다음 섹션에서는 실험을 위한 환경을 만들 수 있는 여러 가지 방법을 살펴봅니다.

### <a name="use-a-curated-environment"></a>큐 레이트 환경 사용

큐 레이트 환경 중 하나를 선택 하 여 시작할 수 있습니다. 

* _AzureML 최소_ 환경에는 실행 추적 및 자산 업로드를 사용 하도록 설정 하는 최소 패키지 집합이 포함 되어 있습니다. 사용자 환경에 대 한 시작 지점으로 사용할 수 있습니다.

* _AzureML-자습서_ 환경에는 일반적인 데이터 과학 패키지가 포함 됩니다. 이러한 패키지에는 Scikit, Pandas, Matplotlib 및 더 큰 azureml-sdk 패키지 집합이 포함 됩니다.

큐 레이트 환경은 캐시 된 Docker 이미지에 의해 지원 됩니다. 이렇게 백업 하면 실행 준비 비용이 줄어듭니다.

`Environment.get` 메서드를 사용 하 여 큐 레이트 환경 중 하나를 선택 합니다.

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

다음 코드를 사용 하 여 큐 레이트 환경과 해당 패키지를 나열할 수 있습니다.

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  _AzureML_ 접두사를 사용 하 여 고유한 환경 이름을 시작 하지 마세요. 이 접두사는 큐 레이트 환경용으로 예약 되어 있습니다.

### <a name="instantiate-an-environment-object"></a>환경 개체 인스턴스화

환경을 수동으로 만들려면 SDK에서 `Environment` 클래스를 가져옵니다. 그런 다음, 다음 코드를 사용 하 여 환경 개체를 인스턴스화합니다.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-conda-and-pip-specification-files"></a>Conda 및 pip 사양 파일 사용

Conda 사양 또는 pip 요구 사항 파일에서 환경을 만들 수도 있습니다. [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) 메서드 또는 [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) 메서드를 사용 합니다. 메서드 인수에 사용자 환경 이름과 원하는 파일의 파일 경로를 포함 합니다.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="use-existing-conda-environments"></a>기존 Conda 환경 사용

로컬 컴퓨터에 기존 Conda 환경이 있는 경우 서비스를 사용 하 여 환경 개체를 만들 수 있습니다. 이 전략을 사용 하 여 원격 실행에서 로컬 대화형 환경을 다시 사용할 수 있습니다.

다음 코드는 기존 Conda 환경 `mycondaenv`에서 환경 개체를 만듭니다. 이 메서드는 [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) 메서드를 사용 합니다.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="create-environments-automatically"></a>자동으로 환경 만들기

학습 실행을 제출 하 여 환경을 자동으로 만듭니다. 메서드를 `submit()` 사용 하 여 실행을 제출 합니다. 학습 실행을 제출할 때 새 환경을 빌드하는 데 몇 분 정도 걸릴 수 있습니다. 빌드 기간은 필요한 종속성의 크기에 따라 달라 집니다. 

실행을 제출 하기 전에 실행 구성에서 환경을 지정 하지 않으면 기본 환경이 만들어집니다.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attach training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Show each step of run 
run.wait_for_completion(show_output=True)
```

마찬가지로, 학습을 위해 [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) 개체를 사용 하는 경우 환경을 지정 하지 않고 평가기 인스턴스를 실행으로 직접 제출할 수 있습니다. `Estimator` 개체가 환경 및 계산 대상을 이미 캡슐화 합니다.

## <a name="add-packages-to-an-environment"></a>환경에 패키지 추가

Conda, pip 또는 private wheel 파일을 사용 하 여 환경에 패키지를 추가 합니다. 클래스를 [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) 사용 하 여 각 패키지 종속성을 지정 합니다. 환경에 추가 `PythonSection`합니다.

### <a name="conda-and-pip-packages"></a>Conda 및 pip 패키지

Conda 패키지 리포지토리에서 패키지를 사용할 수 있는 경우 pip 설치 대신 Conda 설치를 사용 하는 것이 좋습니다. Conda 패키지에는 일반적으로 더 안정적으로 설치 하는 미리 빌드된 이진 파일이 제공 됩니다.

다음 예에서는 환경에를 추가 합니다. 1.17.0의 버전을 `numpy`추가 합니다. `pillow` 패키지도 추가 `myenv`합니다. 이 예제에서는 [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) 메서드와 [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) 메서드를 각각 사용 합니다.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs numpy version 1.17.0 conda package
conda_dep.add_conda_package("numpy==1.17.0")

# Installs pillow package
conda_dep.add_pip_package("pillow")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

>[!IMPORTANT]
> 다른 실행에 동일한 환경 정의를 사용 하는 경우 Azure Machine Learning 서비스는 사용자 환경의 캐시 된 이미지를 재사용 합니다. 예 ```numpy```를 들어 고정 해제 된 패키지 종속성을 사용 하 여 환경을 만드는 경우 _환경을 만들 때 설치 된_패키지 버전을 계속 사용 하 게 됩니다. 또한 일치 하는 정의가 있는 이후의 모든 환경은 이전 버전을 계속 사용 합니다. 자세한 내용은 [환경 빌드, 캐싱 및 다시 사용](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)을 참조 하세요.

### <a name="private-wheel-files"></a>개인 휠 파일

비공개 pip 휠 파일을 먼저 작업 영역 저장소에 업로드 하 여 사용할 수 있습니다. 정적 [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) 메서드를 사용 하 여 업로드 합니다. 그런 다음 저장소 URL을 캡처하고 `add_pip_package()` 메서드에 URL을 전달 합니다.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>환경 관리

계산 대상과 작업 영역의 다른 사용자 간에 업데이트, 추적 및 다시 사용할 수 있도록 환경을 관리 합니다.

### <a name="register-environments"></a>환경 등록

실행을 제출 하거나 웹 서비스를 배포할 때 환경이 작업 영역에 자동으로 등록 됩니다. 메서드를 [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) 사용 하 여 환경을 수동으로 등록할 수도 있습니다. 이 작업을 수행 하면 환경에서 추적 되 고 클라우드에서 버전이 관리 되는 엔터티가 됩니다. 작업 영역 사용자 간에 엔터티를 공유할 수 있습니다.

다음 코드는 `myenv` 환경을 `ws` 작업 영역에 등록 합니다.

```python
myenv.register(workspace=ws)
```

처음으로 학습 또는 배포에서 환경을 사용 하는 경우 작업 영역에 등록 됩니다. 그런 다음 계산 대상에 빌드 및 배포 됩니다. 서비스는 환경을 캐시 합니다. 캐시 된 환경을 다시 사용 하면 새 서비스 또는 업데이트 된 서비스를 사용 하는 것 보다 훨씬 더 많은 시간이 소요 됩니다.

### <a name="get-existing-environments"></a>기존 환경 가져오기

클래스 `Environment` 는 작업 영역에서 기존 환경을 검색 하는 데 사용할 수 있는 메서드를 제공 합니다. 이름, 목록 또는 특정 학습 실행을 기준으로 환경을 검색할 수 있습니다. 이 정보는 문제 해결, 감사 및 재현 가능성에 유용 합니다.

#### <a name="view-a-list-of-environments"></a>환경 목록 보기

클래스를 [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) 사용 하 여 작업 영역에서 환경을 봅니다. 그런 다음 재사용할 환경을 선택 합니다.

#### <a name="get-an-environment-by-name"></a>이름을 기준으로 환경 가져오기

이름 및 버전으로 특정 환경을 가져올 수도 있습니다. 다음 코드에서는 메서드를 [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) 사용 하 여 `1` `ws` 작업 영역에서 `myenv` 환경의 버전을 검색 합니다.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>실행 특정 환경 학습

학습을 완료 한 후 특정 실행에 사용 된 환경을 가져오려면 [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) `Run` 클래스의 메서드를 사용 합니다.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>기존 환경 업데이트

예를 들어 Python 패키지를 추가 하 여 기존 환경을 변경 한다고 가정해 보겠습니다. 그런 다음 실행을 제출 하거나, 모델을 배포 하거나, 환경을 수동으로 등록할 때 환경의 새 버전이 만들어집니다. 버전 관리를 사용 하면 시간에 따른 환경의 변화를 확인할 수 있습니다.

기존 환경에서 Python 패키지 버전을 업데이트 하려면 해당 패키지에 대 한 버전 번호를 지정 합니다. 정확한 버전 번호를 사용 하지 않는 경우 Azure Machine Learning는 원래 패키지 버전과 기존 환경을 다시 사용 합니다.

### <a name="debug-the-image-build"></a>이미지 빌드 디버그

다음 예제에서는 [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace--image-build-compute-none-) 메서드를 사용 하 여 수동으로 환경을 Docker 이미지로 만듭니다. 을 사용 [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-)하 여 이미지 빌드에서 출력 로그를 모니터링 합니다. 그러면 빌드된 이미지가 작업 영역의 Azure Container Registry 인스턴스에 표시 됩니다. 이 정보는 디버깅에 유용 합니다.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="enable-docker"></a>Docker 사용

 Azure Machine Learning [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) `Environment` 클래스의를 사용 하 여 학습을 실행 하는 게스트 운영 체제를 세밀 하 게 사용자 지정 하 고 제어할 수 있습니다.

Docker를 사용 하도록 설정 하면 서비스에서 Docker 이미지를 빌드합니다. 또한 해당 Docker 컨테이너 내에서 사양을 사용 하는 Python 환경을 만듭니다. 이 기능은 학습 실행에 대 한 추가 격리 및 재현 가능성을 제공 합니다.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

기본적으로 새로 빌드된 Docker 이미지는 작업 영역과 연결 된 컨테이너 레지스트리에 나타납니다.  리포지토리 이름에는 *azureml/azureml_\<\>uuid*형식이 있습니다. 이름의 고유 식별자 (*uuid*) 부분은 환경 구성에서 계산 된 해시에 해당 합니다. 이러한 대응을 통해 서비스는 지정 된 환경의 이미지가 다시 사용 하기 위해 이미 있는지 여부를 확인할 수 있습니다.

또한 서비스는 Ubuntu Linux 기반 [기본 이미지](https://github.com/Azure/AzureML-Containers)중 하나를 자동으로 사용 합니다. 지정 된 Python 패키지를 설치 합니다. 기본 이미지에는 CPU 버전과 GPU 버전이 있습니다. Azure Machine Learning는 사용할 버전을 자동으로 검색 합니다.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

사용자 지정 Dockerfile을 지정할 수도 있습니다. Docker ```FROM``` 명령을 사용 하 여 Azure Machine Learning 기본 이미지 중 하나에서 시작한 후 고유한 사용자 지정 단계를 추가 하는 것이 가장 간단 합니다. 비 Python 패키지를 종속성으로 설치 해야 하는 경우이 방법을 사용 합니다.

```python
# Specify docker steps as a string. Alternatively, load the string from a file.
dockerfile = r"""
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
```

### <a name="use-user-managed-dependencies"></a>사용자 관리 종속성 사용

사용자 지정 기본 이미지에 사용 하려는 패키지가 포함 된 Python 환경이 이미 있는 경우도 있습니다.

기본적으로 Azure Machine Learning 서비스는 사용자가 지정한 종속성이 있는 Conda 환경을 빌드하고 기본 이미지에 설치한 Python 라이브러리를 사용 하는 대신 해당 환경에서 실행을 실행 합니다. 

설치 된 패키지를 사용 하려면 매개 변수 `Environment.python.user_managed_dependencies = True`를 설정 합니다. 기본 이미지에 Python 인터프리터가 포함 되어 있고 학습 스크립트에 필요한 패키지를 포함 하는지 확인 합니다.

예를 들어 NumPy 패키지가 설치 된 기본 Miniconda 환경에서 실행 하려면 먼저 패키지를 설치 하는 단계를 포함 하는 Dockerfile을 지정 합니다. 그런 다음 사용자 관리 종속성을로 `True`설정 합니다. 

변수를 `Environment.python.interpreter_path` 설정 하 여 이미지 내의 특정 Python 인터프리터에 대 한 경로를 지정할 수도 있습니다.

```python
dockerfile = """
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN conda install numpy
"""

myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
myenv.python.user_managed_dependencies=True
myenv.python.interpreter_path = "/opt/miniconda/bin/python"
```

## <a name="use-environments-for-training"></a>교육용 환경 사용

학습 실행을 제출 하려면 환경, [계산 대상](concept-compute-target.md)및 학습 Python 스크립트를 실행 구성에 결합 해야 합니다. 이 구성은 실행을 전송 하는 데 사용 되는 래퍼 개체입니다.

학습 실행을 제출할 때 새 환경을 빌드하는 데 몇 분 정도 걸릴 수 있습니다. 기간은 필요한 종속성의 크기에 따라 달라 집니다. 환경은 서비스에 의해 캐시 됩니다. 환경 정의가 변경 되지 않은 상태로 유지 되는 한, 전체 설정 시간을 한 번만 발생 합니다.

다음 로컬 스크립트 실행 예제에서는를 래퍼 개체로 사용 [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) 하는 위치를 보여 줍니다.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> 실행 기록을 사용 하지 않도록 설정 하거나 스냅숏을 실행 하려면의 설정을 사용 `ScriptRunConfig.run_config.history`합니다.

실행 구성에서 환경을 지정 하지 않으면 서비스는 실행을 제출할 때 기본 환경을 만듭니다.

### <a name="use-an-estimator-for-training"></a>학습에 평가기 사용

학습에 [평가기](how-to-train-ml-models.md) 를 사용 하는 경우 평가기 인스턴스를 직접 제출할 수 있습니다. 환경 및 계산 대상이 이미 캡슐화 되어 있습니다.

다음 코드에서는 단일 노드 학습 실행에 평가기를 사용 합니다. `scikit-learn` 모델에 대 한 원격 계산에서 실행 됩니다. 이 예에서는 이전에 계산 대상 개체 `compute_target`, 및 데이터 저장소 개체를 `ds`만든 것으로 가정 합니다.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="use-environments-for-web-service-deployment"></a>웹 서비스 배포를 위한 환경 사용

모델을 웹 서비스로 배포할 때 환경을 사용할 수 있습니다. 이 기능을 통해 재현 가능한 연결 된 워크플로를 사용할 수 있습니다. 이 워크플로에서는 학습 계산과 유추 계산에 동일한 라이브러리를 사용 하 여 모델을 학습, 테스트 및 배포할 수 있습니다.

웹 서비스를 배포 하려면 배포 개체의 환경, 유추 계산, 점수 매기기 스크립트 및 등록 된 모델을 결합 [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)합니다. 자세한 내용은 [모델을 배포 하는 방법 및 위치](how-to-deploy-and-where.md)를 참조 하세요.

이 예제에서는 학습 실행을 완료 했다고 가정 합니다. 이제 Azure Container Instances에 해당 모델을 배포 하려고 합니다. 웹 서비스를 빌드하면 모델 및 점수 매기기 파일이 이미지에 탑재 되 고 Azure Machine Learning 유추 스택이 이미지에 추가 됩니다.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference, & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>노트북 예제

이 [예제 전자 필기장은](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) 이 문서에 설명 되어 있는 개념과 메서드를 확장 합니다.

[사용자 지정 docker 기본 이미지를 사용 하 여 모델 배포](how-to-deploy-custom-docker-image.md) 사용자 지정 docker 기본 이미지를 사용 하 여 모델을 배포 하는 방법을 보여 줍니다.

이 [예제 노트북에서는](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) Spark 모델을 웹 서비스로 배포 하는 방법을 보여 줍니다.

## <a name="create-and-manage-environments-with-the-cli"></a>CLI를 사용 하 여 환경 만들기 및 관리

[AZURE MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md) 는 Python SDK의 대부분 기능을 미러링합니다. 환경을 만들고 관리 하는 데 사용할 수 있습니다. 이 섹션에서 설명 하는 명령은 기본적인 기능을 보여 줍니다.

다음 명령은 지정 된 디렉터리의 기본 환경 정의에 대 한 파일을 스 캐 폴드 합니다. 이러한 파일은 JSON 파일입니다. SDK의 해당 클래스 처럼 작동 합니다. 이 파일을 사용 하 여 사용자 지정 설정을 포함 하는 새 환경을 만들 수 있습니다. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

다음 명령을 실행 하 여 지정 된 디렉터리에서 환경을 등록 합니다.

```azurecli-interactive
az ml environment register -d myenvdir
```

다음 명령을 실행 하 여 등록 된 모든 환경을 나열 합니다.

```azurecli-interactive
az ml environment list
```

다음 명령을 사용 하 여 등록 된 환경을 다운로드 합니다.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="next-steps"></a>다음 단계

* 관리 되는 계산 대상을 사용 하 여 모델을 학습 하려면 [자습서: 모델 학습](tutorial-train-models-with-aml.md)을 참조 하세요.
* 학습 된 모델을 만든 후 모델을 [배포 하는 방법 및 위치](how-to-deploy-and-where.md)를 알아봅니다.
* [ `Environment` 클래스 SDK 참조](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)를 봅니다.
* 이 문서에 설명 된 개념 및 방법에 대 한 자세한 내용은 [예제 노트북](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments)을 참조 하세요.
