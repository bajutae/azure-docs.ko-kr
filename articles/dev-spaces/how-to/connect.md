---
title: 개발 컴퓨터를 AKS 클러스터에 연결(미리 보기)
services: azure-dev-spaces
ms.date: 11/04/2019
ms.topic: conceptual
description: Azure Dev Spaces를 사용 하 여 개발 컴퓨터를 AKS 클러스터에 연결 하는 방법을 알아봅니다.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, 컨테이너
ms.openlocfilehash: 772f221a8047a71902f36fa98ded6c24b5e02d27
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80235012"
---
# <a name="connect-your-development-computer-to-an-aks-cluster-preview"></a>개발 컴퓨터를 AKS 클러스터에 연결(미리 보기)

Azure Dev Spaces를 사용 하 여 개발 컴퓨터에서 컨테이너를 사용 하거나 사용 하지 않고 코드를 실행 하 고 디버그할 수 있으며, 나머지 응용 프로그램 또는 서비스를 사용 하 여 Kubernetes 클러스터에 계속 연결할 수 있습니다. 개발 컴퓨터를 클러스터에 연결 하면 응용 프로그램을 빠르게 개발 하 고 Docker 또는 Kubernetes 구성을 만들지 않고도 종단 간 테스트를 수행할 수 있습니다. 동일한 클러스터를 사용할 수 있는 다른 작업 또는 사용자에 게 영향을 주지 않고 AKS 클러스터에 연결할 수도 있습니다.

Azure Dev Spaces 연결 된 AKS 클러스터와 개발 컴퓨터 간의 트래픽을 리디렉션합니다. 이러한 트래픽 리디렉션을 통해 AKS 클러스터에서 실행 되는 서비스와 개발 컴퓨터의 코드가 동일한 AKS 클러스터에 있는 것 처럼 통신할 수 있습니다. 코드는 개발 컴퓨터에서 실행 되므로 해당 코드를 실행 하 고 디버그 하는 데 사용 하는 개발 도구를 유연 하 게 사용할 수도 있습니다. 또한 Azure Dev Spaces는 개발 컴퓨터의 AKS 클러스터에서 pod에 사용할 수 있는 환경 변수 및 탑재 된 파일을 복제 하는 방법을 제공 합니다.

이 가이드에서는 다음을 수행하는 방법을 배우게 됩니다.

* Azure Dev Spaces를 Azure의 관리형 Kubernetes 클러스터에 설정합니다.
* 여러 마이크로서비스가 포함된 대규모 애플리케이션을 개발 공간에 배포합니다.
* Azure Dev Spaces를 사용 하 여 개발 컴퓨터에서 실행 되는 코드와 AKS 클러스터 간에 트래픽을 리디렉션합니다.

> [!IMPORTANT]
> 이 기능은 현재 미리 보기로 제공됩니다. [부속 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)에 동의하면 미리 보기를 사용할 수 있습니다. 이 기능의 몇 가지 측면은 일반 공급(GA) 전에 변경될 수 있습니다.

## <a name="before-you-begin"></a>시작하기 전에

이 가이드에서는 [Azure Dev Spaces 자전거 공유 샘플 응용 프로그램](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) 을 사용 하 여 개발 컴퓨터를 AKS 클러스터에 연결 하는 방법을 보여 줍니다. [Azure Dev Spaces 자전거 공유 샘플 응용 프로그램 추가 정보](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/README.md) 의 지침에 따라 샘플 응용 프로그램을 실행 합니다. 또는 AKS 클러스터에 고유한 응용 프로그램이 있는 경우 아래 단계를 수행 하 여 고유한 서비스와 pod의 이름을 사용할 수 있습니다.

### <a name="limitations"></a>제한 사항

* UDP는 지금은 지원 되지 않습니다.

### <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 Azure 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free)을 만들 수 있습니다.
* [Azure CLI 설치][azure-cli]
* MacOS 또는 Windows 10에서 [Azure Dev Spaces][azds-vs-code] 확장을 설치 하 고 실행 하는 [Visual Studio Code][vs-code] .
* [Azure Dev Spaces 자전거 공유 샘플 응용 프로그램](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) 또는 AKS 클러스터에서 실행 되는 자체 응용 프로그램입니다.

## <a name="connect-your-development-computer"></a>개발 컴퓨터 연결

Visual Studio Code에서 개발 *-공백/샘플/BikeSharingApp/자전거* 를 열고 Azure Dev Spaces 확장을 사용 하 여 개발 컴퓨터를 AKS 클러스터에 연결 합니다.

Azure Dev Spaces 확장을 사용 하려면 *보기* , *명령 팔레트*를 차례로 클릭 하 여 Visual Studio Code 명령 팔레트를 엽니다. 입력 `Azure Dev Spaces: Redirect` 을 시작 하 고, `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`또는 `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`중 하나를 클릭 합니다.

![명령](../media/how-to-connect/connect-commands.png)

### <a name="select-a-redirection-option"></a>리디렉션 옵션 선택

을 실행 `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`하는 경우 기존 Kubernetes 서비스를 선택 하 라는 메시지가 표시 됩니다.

![서비스 선택](../media/how-to-connect/connect-choose-service.png)

이 옵션은이 서비스에 대 한 AKS 클러스터의 모든 트래픽을 개발 컴퓨터에서 실행 중인 응용 프로그램의 버전으로 리디렉션합니다. 이 서비스의 AKS 클러스터에서 여러 pod를 실행 하는 경우이 서비스에 대 한 모든 트래픽은 개발 컴퓨터에만 라우팅됩니다. 또한 Azure Dev Spaces는 응용 프로그램의 모든 아웃 바운드 트래픽을 AKS 클러스터로 다시 라우팅합니다.

을 실행 `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`하는 경우 특정 pod를 선택 하 라는 메시지가 표시 됩니다.

![Pod 선택](../media/how-to-connect/connect-choose-pod.png)

이 옵션은 특정 pod에 연결 합니다. 이 옵션은 트래픽을 전송 하거나 수신 하지 않고 종료 된 pod을 복제 하지 않는 pod와 상호 작용 하는 데 유용 합니다. Pod가 트래픽을 전송 하 고 받는 경우이 옵션은 선택한 pod의 서비스와 관련 `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` 된 모든 pod에 대해 AKS 클러스터의 모든 트래픽을 리디렉션하는 것과 비슷한 방식으로 동작 합니다.

를 실행 `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`하는 경우 기존 pod 또는 서비스를 선택 하 라는 메시지가 표시 되지 않습니다. 이 옵션은 개발 컴퓨터에서 실행 되는 응용 프로그램의 모든 아웃 바운드 트래픽을 AKS 클러스터로 리디렉션합니다.

이 예에서는 *자전거* 서비스 `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` 를 선택 하 고 선택 합니다.

### <a name="select-a-connection-mode"></a>연결 모드 선택

리디렉션 옵션을 선택한 후에는 *바꾸기* 또는 *복제* 연결 모드를 선택 하 라는 메시지가 표시 됩니다.

![바꾸기 또는 복제](../media/how-to-connect/connect-replace-clone.png)

*Replace* 옵션은 AKS 클러스터의 현재 pod 또는 서비스를 대체 하 고 해당 서비스에 대 한 모든 트래픽을 개발 컴퓨터로 리디렉션합니다. 이 옵션은 개발 컴퓨터에서 응용 프로그램을 시작할 때까지 리디렉션하는 서비스와 상호 작용 하는 AKS 클러스터의 다른 서비스에 방해가 될 수 있습니다. *복제* 옵션을 사용 하면 기존 자식 개발 공간을 선택 하거나 pod 나 서비스에 대 한 트래픽을 개발 컴퓨터로 리디렉션하는 새 자식 개발 공간을 만들 수 있습니다. 이 옵션을 사용 하면 해당 자식 개발 공간으로의 트래픽만 개발 컴퓨터로 리디렉션되도록 다른 서비스를 중단 하지 않고 격리로 작업할 수 있습니다. *복제* 옵션을 사용 하려면 AKS 클러스터가 Azure Dev Spaces 사용 하도록 설정 되어 있어야 합니다.

이 예에서는 *바꾸기*를 선택 합니다.

> [!NOTE]
> 기존 서비스의 pod에 여러 컨테이너가 있는 경우 응용 프로그램의 컨테이너를 선택 하 라는 메시지도 표시 됩니다.

### <a name="select-a-port-for-your-application"></a>응용 프로그램에 대 한 포트 선택

연결 모드를 선택한 후에는 로컬 응용 프로그램의 TCP 포트를 입력 하 라는 메시지가 표시 됩니다. 응용 프로그램에서 여러 포트를 여는 경우 쉼표로 구분 합니다 (예: *80, 81*). 응용 프로그램에서 네트워크 요청을 허용 하지 않는 경우 *0*을 입력 합니다. 이 예제에서는 *3000*를 입력 합니다.

![연결 선택 포트](../media/how-to-connect/connect-choose-port.png)

### <a name="confirm-you-are-connected"></a>연결 되어 있는지 확인

응용 프로그램의 TCP 포트를 선택 하면 Azure Dev Spaces AKS 클러스터에 대 한 연결이 설정 됩니다. Azure Dev Spaces는 AKS 클러스터에 에이전트를 삽입 하 여 AKS 클러스터와 개발 컴퓨터 간에 트래픽을 리디렉션합니다. 이 연결을 설정 하는 데 몇 분 정도 걸릴 수 있습니다. 또한 Azure Dev Spaces는 개발 컴퓨터에서 *호스트* 파일을 수정 하기 위해 관리자 액세스를 요청 합니다.

> [!IMPORTANT]
> AKS 클러스터에 대 한 연결이 설정 되 Azure Dev Spaces 면 연결 모드 *바꾸기* 를 선택 하는 경우 개발 컴퓨터에서 서비스를 시작할 때까지 AKS 클러스터의 다른 서비스가 올바르게 작동 하지 않을 수 있습니다. 대신 *복제* 연결 모드를 선택 하 여 리디렉션에 사용할 자식 개발 공간을 만들고 부모 공간에 대 한 중단을 방지할 수 있습니다. 또한 서비스에 개발 컴퓨터에서 사용할 수 없는 종속성이 있는 경우 응용 프로그램을 수정 하거나 [추가 구성을](#additional-configuration) 제공 해야 할 수 있습니다.

Azure Dev Spaces AKS 클러스터에 대 한 연결을 설정한 후에 *AZDS* 라는 제목의 터미널 창이 열립니다. 이 터미널 창에는 AKS 클러스터에서 구성 된 모든 환경 변수 및 DNS 항목이 있습니다. 이 터미널 창에서 실행 하거나 Visual Studio Code 디버거를 사용 하 여 실행 하는 모든 코드는 AKS 클러스터에 연결 됩니다.

![터미널](../media/how-to-connect/connect-terminal.png)

또한 Azure Dev Spaces는 *Dev Spaces* 가 모든 출력을 연결 하는 창을 만듭니다.

![출력](../media/how-to-connect/connect-output.png)

또한 Azure Dev Spaces에는 연결 상태를 보여 주는 상태 표시줄 항목이 있습니다.

![상태](../media/how-to-connect/connect-status.png)

상태 표시줄에 Dev Spaces가 표시 되는지 확인 합니다 *. 로컬 포트 3000에서 dev/bikes에 연결*됩니다.

### <a name="configure-your-application-on-your-development-computer"></a>개발 컴퓨터에서 응용 프로그램 구성

*AZDS Connect-자전거* 터미널 창을 열고 다음을 실행 `npm install`합니다.

```console
$ npm install

> fsevents@1.2.9 install ./dev-spaces/samples/BikeSharingApp/Bikes/node_modules/fsevents
> node install
...
```

*디버그* 를 클릭 하 고 *구성 열기*를 클릭 합니다. 환경을 선택 하 라는 메시지가 표시 되 면 *node.js*를 선택 합니다. `.vscode/launch.json` 파일이 생성 됩니다. 해당 파일의 내용을 다음으로 바꿉니다.

```json
{
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch via NPM",
            "runtimeExecutable": "npm",
            "runtimeArgs": [
                "run-script",
                "debug"
            ],
            "port": 9229
        }
    ]
}
```

[Package. json](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/package.json) 을 열고 디버깅 스크립트를 추가 합니다.

```json
  "devDependencies": {
    "nodemon": "^1.18.10"
  },
  "scripts": {
    "debug": "node --nolazy --inspect-brk=9229 server.js"
  }
```

### <a name="start-your-application-on-your-development-computer"></a>개발 컴퓨터에서 응용 프로그램 시작

왼쪽의 *디버그* 아이콘을 클릭 하 고 맨 위에 있는 *NPM를 통해* 시작 옆의 시작 단추를 클릭 합니다.

![NPM를 통해 시작](../media/how-to-connect/launch-npm.png)

응용 프로그램이 시작 되 고 Azure Dev Spaces AKS 클러스터와 개발 컴퓨터 간의 트래픽이 리디렉션됩니다. *디버그 콘솔*아래와 비슷한 메시지가 표시 됩니다.

```console
/usr/local/bin/npm run-script debug 
...
Debugger attached.
Collection: bikes
MongoDB connection string: mongodb://databases-mongo
Connected to MongoDB
Listening on port 3000
```

Azure Dev Spaces 상태 표시줄을 클릭 하 고 응용 프로그램의 공용 URL을 선택 하 여 *bikesharingweb* 서비스로 이동 합니다. 이전에 실행 한 `azds list-uris` 명령에서 공용 URL을 찾을 수도 있습니다. 클러스터에서 Azure Dev Spaces를 사용 하지 않는 경우 사용 중인 네임 스페이스의 응용 프로그램에 대 한 IP 또는 URL을 사용 합니다. 위의 예제에서 *bikesharingweb* 서비스에 대한 공용 URL은 `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`입니다. 사용자로 *Aurelia Briggs (고객)* 를 선택 하 고 임대에 대 한 자전거를 선택 합니다.

### <a name="set-a-break-point"></a>중단점 설정

[Node.js](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233) 를 열고 줄 233의 아무 곳 이나 클릭 하 여 커서를 놓습니다. *F9* 또는 *디버그* 를 클릭 한 다음 *중단점 설정/해제*를 클릭 하 여 중단점을 설정 합니다.

공용 URL을 열어 *bikesharingweb* 서비스로 이동 합니다. 사용자로 *Aurelia Briggs (고객)* 를 선택 하 고 임대에 대 한 자전거를 선택 합니다. 자전거에 대 한 이미지는 로드 되지 않습니다. Visual Studio Code로 돌아가서 233 줄이 강조 표시 됩니다. 설정 하는 중단점에서 233 줄의 서비스를 일시 중지 했습니다. 서비스를 다시 시작하려면 *F5* 키를 누르거나 *디버그*, *계속*을 차례로 클릭합니다. 브라우저로 돌아가서 자전거에 대 한 자리 표시자 이미지가 표시 되는지 확인 합니다.

의 `server.js` 줄 233에 커서를 놓고 *F9*를 눌러 중단점을 제거 합니다.

### <a name="update-your-application"></a>애플리케이션 업데이트

232 `server.js` 줄과 233 줄을 제거 하려면 편집 합니다.

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

이제 섹션은 다음과 같습니다.

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

변경 내용을 저장 하 고 *디버그* 를 클릭 한 다음 *디버깅을 다시 시작*합니다. 브라우저를 새로 고치고 자전거에 대 한 자리 표시자 이미지가 더 이상 표시 되지 않는지 확인 합니다.

*디버그*와 *디버깅 중지*를 차례로 클릭하여 디버거를 중지합니다. Azure Dev Spaces 상태 표시줄을 클릭 하 여 AKS 클러스터에서 연결을 끊습니다.

## <a name="additional-configuration"></a>추가 구성

Azure Dev Spaces는 라우팅 트래픽을 처리 하 고 추가 구성 없이 환경 변수를 복제할 수 있습니다. AKS 클러스터의 컨테이너에 탑재 된 파일 (예: ConfigMap 파일)을 다운로드 해야 하는 경우를 `azds-local.env` 만들어 해당 파일을 개발 컴퓨터에 다운로드할 수 있습니다.

예를 들면 `azds-local.env`다음과 같습니다.

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}


# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

## <a name="using-logging-and-diagnostics"></a>로깅 및 진단 사용

로깅 출력은 개발 컴퓨터를 AKS 클러스터에 연결한 후 개발 *공간 연결* 창에 기록 됩니다.

![출력](../media/how-to-connect/connect-output.png)

Azure Dev Spaces 상태 표시줄을 클릭 하 고 *진단 정보 표시*를 선택 합니다. 이 명령은 로깅 출력에 현재 환경 변수 및 DNS를 인쇄 합니다.

![진단으로 출력](../media/how-to-connect/connect-output-diagnostics.png)

또한 [개발 컴퓨터의 *임시* 디렉터리][azds-tmp-dir]에 있는 디렉터리 `Azure Dev Spaces` 에서 진단 로그를 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

끌어오기 요청이 리포지토리의 주 분기에 병합 되기 전에 Azure Dev Spaces 및 GitHub 작업을 사용 하 여 끌어오기 요청에서 직접 AKS의 변경 내용을 테스트 하는 방법에 대해 알아봅니다.

> [!div class="nextstepaction"]
> [Azure Kubernetes Service & GitHub 작업][gh-actions]

[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-quickstart]: ../quickstart-team-development.md
[vs-code]: https://code.visualstudio.com/download