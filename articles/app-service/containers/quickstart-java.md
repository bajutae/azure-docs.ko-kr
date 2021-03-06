---
title: '빠른 시작: Linux Java 앱 만들기'
description: 첫 번째 Java 앱을 App Service의 Linux 컨테이너에 배포하여 Azure App Service에서 Linux 앱을 시작하세요.
keywords: azure, 앱 서비스, 웹앱, linux, java, maven, 빠른 시작
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 8f2e99ffc9f9ee5c5553e8d933d82f83999c8ab2
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732893"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-linux"></a>빠른 시작: Linux의 App Service에서 Java 앱 만들기

[Linux의 App Service](app-service-linux-intro.md)는 Linux 운영 체제를 사용하여 확장성이 높은 자체 패치 웹 호스팅 서비스를 제공합니다. 이 빠른 시작에서는 [Azure Web App Plugin for Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)과 함께 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)를 사용하여 Linux 운영 체제에 Java 웹 보관(WAR) 파일을 배포하는 방법을 보여줍니다.

> [!NOTE]
>
> IntelliJ, Eclipse 및 VS Code와 같은 인기 있는 IDE를 사용하여 동일한 작업을 수행할 수도 있습니다. [Azure Toolkit for IntelliJ 빠른 시작](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app), [Azure Toolkit for Eclipse 빠른 시작](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app) 또는 [VS Code 빠른 시작](https://code.visualstudio.com/docs/java/java-webapp)에서 유사한 문서를 확인하세요.
>
![Azure App Service에서 실행 중인 샘플 앱](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java 앱 만들기

Cloud Shell 프롬프트에서 다음 Maven 명령을 실행하여 `helloworld`라는 새 앱을 만듭니다.

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp"
```
그런 다음, 작업 디렉터리를 프로젝트 폴더로 변경합니다.

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Maven 플러그 인 구성

Azure App Service에 대한 배포 프로세스는 Azure CLI의 계정 자격 증명을 사용합니다. 계속하려면 [Azure CLI로 로그인](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)합니다.

```azurecli
az login
```

그런 다음, 배포를 구성하고 명령 프롬프트에서 maven 명령을 실행하고 **확인(Y/N)** 프롬프트가 표시될 때까지 **ENTER** 키를 눌러 기본 구성을 사용한 다음, **'y'** 를 누르면 구성이 완료됩니다. 
```cmd
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```
샘플 프로세스는 다음과 같습니다.

```cmd
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. TOMCAT 8.5 [*]
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1558400876966
ResourceGroup : helloworld-1558400876966-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : TOMCAT 8.5-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

> [!NOTE]
> 이 문서에서는 WAR 파일에 패키지된 Java 앱만 사용합니다. 플러그 인에서 JAR 웹 애플리케이션도 지원됩니다. [Linux 기반 App Service에 Java SE JAR 파일 배포](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)를 방문하여 직접 확인해 보세요.

`pom.xml`로 다시 이동하여 업데이트된 플러그인 구성을 확인하고 필요한 경우 App Service에 대한 다른 구성을 pom 파일에서 직접 수정할 수 있습니다. 몇 가지 일반적인 사항은 다음과 같습니다.

 속성 | 필수 | Description | 버전
---|---|---|---
`<schemaVersion>` | false | 구성 스키마의 버전을 지정합니다. 지원되는 값은 `v1`, `v2`입니다. | 1.5.2
`<resourceGroup>` | true | 웹앱에 대한 Azure 리소스 그룹입니다. | 0.1.0+
`<appName>` | true | 웹앱의 이름입니다. | 0.1.0+
`<region>` | true | 웹앱이 호스트되는 지역을 지정합니다(기본값: **westeurope**). [지원되는 지역](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) 섹션에 있는 모든 유효한 지역입니다. | 0.1.0+
`<pricingTier>` | false | 웹앱에 대한 가격 책정 계층입니다. 기본값은 **P1V2**입니다.| 0.1.0+
`<runtime>` | true | 런타임 환경 구성이며, [여기](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)에서 세부 정보를 볼 수 있습니다. | 0.1.0+
`<deployment>` | true | 배포 구성이며, [여기](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)에서 세부 정보를 볼 수 있습니다. | 0.1.0+

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=config)

## <a name="deploy-the-app"></a>앱 배포

다음 명령을 사용하여 Java 앱을 Azure에 배포합니다.

```bash
mvn package azure-webapp:deploy
```

배포가 완료되면 웹 브라우저에서 다음 URL을 사용하여 배포된 애플리케이션으로 이동합니다(예: `http://<webapp>.azurewebsites.net`). 

![Azure App Service에서 실행 중인 샘플 앱](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**축하합니다.** Linux의 App Service에 첫 번째 Java 앱을 배포했습니다.

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>리소스 정리

이전 단계에서 Azure 리소스를 리소스 그룹에 만들었습니다. 나중에 이러한 리소스가 필요하지 않을 것으로 예상되는 경우 포털에서 리소스 그룹을 삭제하거나 Cloud Shell에서 다음 명령을 실행하여 리소스 그룹을 삭제합니다.

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

이 명령을 실행하는 데 1분 정도 걸릴 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Java를 사용하여 Azure SQL 데이터베이스에 연결](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Java를 사용하여 Azure DB for MySQL에 연결](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Java를 사용하여 Azure DB for PostgreSQL에 연결](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Java 앱 구성](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Jenkins를 사용하는 CI/CD](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Java 개발자 리소스용 기타 Azure](/java/azure/)

> [!div class="nextstepaction"]
> [Azure용 Maven 플러그 인에 대한 자세한 정보](https://github.com/microsoft/azure-maven-plugins)
