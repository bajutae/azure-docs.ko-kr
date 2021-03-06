---
title: GitHub 작업을 사용 하는 Azure 스프링 클라우드 CI/CD
description: GitHub 작업을 사용 하 여 Azure 스프링 클라우드 용 CI/CD 워크플로를 구축 하는 방법
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/15/2019
ms.openlocfilehash: 559c894a2212466761de820de7486ae203337802
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77538467"
---
# <a name="azure-spring-cloud-cicd-with-github-actions"></a>GitHub 작업을 사용 하는 Azure 스프링 클라우드 CI/CD

GitHub 작업은 자동화 된 소프트웨어 개발 수명 주기 워크플로를 지원 합니다. Azure 스프링 클라우드의 GitHub 작업을 사용 하 여 Azure에 빌드, 테스트, 패키지, 릴리스 및 배포를 위해 리포지토리에 워크플로를 만들 수 있습니다. 

## <a name="prerequisites"></a>전제 조건
이 예에서는 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)필요 합니다.

## <a name="set-up-github-repository-and-authenticate"></a>GitHub 리포지토리 설정 및 인증
Azure 로그인 작업에 권한을 부여 하려면 Azure 서비스 주체 자격 증명이 필요 합니다. Azure 자격 증명을 가져오려면 로컬 컴퓨터에서 다음 명령을 실행 합니다.
```
az login
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth 
```
특정 리소스 그룹에 액세스 하려면 범위를 줄일 수 있습니다.
```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```
명령은 JSON 개체를 출력 해야 합니다.
```JSON
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    ...
}
```

이 예제에서는 GitHub의 [피기백 메트릭](https://github.com/Azure-Samples/piggymetrics) 샘플을 사용 합니다.  샘플을 분기 하 고 GitHub 리포지토리 페이지를 연 다음 **설정** 탭을 클릭 합니다. **비밀** 메뉴를 열고 **새 비밀 추가**를 클릭 합니다.

 ![새 비밀 추가](./media/github-actions/actions1.png)

비밀 이름을로 `AZURE_CREDENTIALS` 설정 하 고 해당 값을 *GitHub 리포지토리를 설정 하 고 인증*하는 제목 아래에 있는 JSON 문자열로 설정 합니다.

 ![비밀 데이터 설정](./media/github-actions/actions2.png)

[Github 작업에서 Key Vault를 사용 하 여 Azure 스프링 인증](./spring-cloud-github-actions-key-vault.md)에 설명 된 대로 github 작업의 Key Vault에서 azure 로그인 자격 증명을 가져올 수도 있습니다.

## <a name="provision-service-instance"></a>서비스 인스턴스 프로 비전
Azure 스프링 클라우드 서비스 인스턴스를 프로 비전 하려면 Azure CLI을 사용 하 여 다음 명령을 실행 합니다.
```
az extension add --name spring-cloud
az group create --location eastus --name <resource group name>
az spring-cloud create -n <service instance name> -g <resource group name>
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/xxx/piggymetrics --label config
```
## <a name="build-the-workflow"></a>워크플로 빌드
워크플로는 다음 옵션을 사용 하 여 정의 됩니다.

### <a name="prepare-for-deployment-with-azure-cli"></a>Azure CLI를 사용 하 여 배포 준비
명령이 `az spring-cloud app create` 현재 idempotent이 아닙니다.  기존 Azure 스프링 클라우드 앱 및 인스턴스에이 워크플로를 권장 합니다.

준비를 위해 다음 Azure CLI 명령을 사용 합니다.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

### <a name="deploy-with-azure-cli-directly"></a>Azure CLI를 사용 하 여 직접 배포
리포지토리에서 파일 `.github/workflow/main.yml` 을 만듭니다.

```
name: AzureSpringCloud
on: push

env:
  GROUP: <resource group name>
  SERVICE_NAME: <service instance name>

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
    
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
      
    - name: Install ASC AZ extension
      run: az extension add --name spring-cloud
   
    - name: Deploy with AZ CLI commands
      run: |
        az configure --defaults group=$GROUP
        az configure --defaults spring-cloud=$SERVICE_NAME
        az spring-cloud app deploy -n gateway --jar-path ${{ github.workspace }}/gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service --jar-path ${{ github.workspace }}/account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service --jar-path ${{ github.workspace }}/auth-service/target/auth-service.jar
```
### <a name="deploy-with-azure-cli-action"></a>Azure CLI 작업으로 배포
Az `run` command는 최신 버전의 Azure CLI을 사용 합니다. 주요 변경 내용이 있는 경우 Azure/CLI `action`에서 Azure CLI의 특정 버전을 사용할 수도 있습니다. 

> [!Note] 
> 이 명령은 새 컨테이너에서 실행 되므로 `env` 작동 하지 않으며 상호 작업 파일 액세스에 추가 제한이 있을 수 있습니다.

리포지토리에서 github/workflow/main .yml 파일을 만듭니다.
```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
        
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
              
    - name: Azure CLI script
      uses: azure/CLI@v1
      with:
        azcliversion: 2.0.75
        inlineScript: |
          az extension add --name spring-cloud
          az configure --defaults group=<service group name>
          az configure --defaults spring-cloud=<service instance name>
          az spring-cloud app deploy -n gateway --jar-path $GITHUB_WORKSPACE/gateway/target/gateway.jar
          az spring-cloud app deploy -n account-service --jar-path $GITHUB_WORKSPACE/account-service/target/account-service.jar
          az spring-cloud app deploy -n auth-service --jar-path $GITHUB_WORKSPACE/auth-service/target/auth-service.jar
```

## <a name="deploy-with-maven-plugin"></a>Maven 플러그 인을 사용 하 여 배포
또 다른 옵션은 Jar을 배포 하 고 앱 설정을 업데이트 하는 데 [Maven 플러그 인](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-maven) 을 사용 하는 것입니다. 명령은 `mvn azure-spring-cloud:deploy` idempotent 이며 필요한 경우 자동으로 앱을 만듭니다. 해당 앱을 미리 만들 필요가 없습니다.

```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
        
    # Maven plugin can cosume this authentication method automatically
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    # Maven deploy, make sure you have correct configurations in your pom.xml
    - name: deploy to Azure Spring Cloud using Maven
      run: |
        mvn azure-spring-cloud:deploy
```

## <a name="run-the-workflow"></a>워크플로 실행
Github **Actions** 에 푸시 `.github/workflow/main.yml` 하면 github 작업을 자동으로 사용 하도록 설정 해야 합니다. 새 커밋을 푸시할 때 동작이 트리거됩니다. 브라우저에서이 파일을 만드는 경우에는 작업을 이미 실행 해야 합니다.

작업이 활성화 되었는지 확인 하려면 GitHub 리포지토리 페이지에서 **작업** 탭을 클릭 합니다.

 ![작업 사용 확인](./media/github-actions/actions3.png)

오류가 발생 한 경우 (예: Azure 자격 증명을 설정 하지 않은 경우) 오류를 수정 하 고 나 서 검사를 다시 실행할 수 있습니다. GitHub 리포지토리 페이지에서 **작업**을 클릭 하 고, 특정 워크플로 작업을 선택한 다음, **다시 실행 확인** 단추를 클릭 하 여 검사를 다시 실행 합니다.

 ![검사 다시 실행](./media/github-actions/actions4.png)

## <a name="next-steps"></a>다음 단계
* [스프링 클라우드 GitHub 작업 Key Vault](./spring-cloud-github-actions-key-vault.md)
* [Azure Active Directory 서비스 주체](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)
* [Azure에 대 한 GitHub 작업](https://github.com/Azure/actions/)
