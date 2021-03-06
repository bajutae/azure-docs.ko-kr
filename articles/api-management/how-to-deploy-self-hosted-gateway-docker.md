---
title: 자체 호스팅 게이트웨이를 Docker에 배포 | Microsoft Docs
description: Azure API Management의 자체 호스팅 게이트웨이 구성 요소를 Docker에 배포 하는 방법에 대해 알아봅니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.topic: article
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: b9e990988770e8aca015ae8b1159bb4f5e50df57
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82205093"
---
# <a name="deploy-an-azure-api-management-self-hosted-gateway-to-docker"></a>Azure API Management 자체 호스팅 게이트웨이를 Docker에 배포

이 문서에서는 Azure API Management의 자체 호스팅 게이트웨이 구성 요소를 Docker 환경에 배포 하는 단계를 제공 합니다.

> [!NOTE]
> Docker에서 자체 호스팅 게이트웨이를 호스트 하는 것은 평가 및 개발 사용 사례에 가장 적합 합니다. Kubernetes는 프로덕션에 사용 하는 것이 좋습니다. Kubernetes에 자체 호스팅 게이트웨이를 배포 하는 방법을 알아보려면 [이](how-to-deploy-self-hosted-gateway-kubernetes.md) 문서를 참조 하세요.

## <a name="prerequisites"></a>전제 조건

- 다음 빠른 시작: [Azure API Management 인스턴스 만들기](get-started-create-service-instance.md) 를 완료 합니다.
- Docker 환경을 만듭니다. [데스크톱에 대 한 Docker](https://www.docker.com/products/docker-desktop) 는 개발 및 평가를 위해 좋은 옵션입니다. Docker 자체에 대 한 모든 Docker 버전, 해당 기능 및 포괄적인 설명서에 대 한 자세한 내용은 [docker 설명서](https://docs.docker.com) 를 참조 하세요.
- [API Management 인스턴스에서 게이트웨이 리소스 프로 비전](api-management-howto-provision-self-hosted-gateway.md)

> [!NOTE]
> 자체 호스팅 게이트웨이는 x86-64 Linux 기반 Docker 컨테이너로 패키지 됩니다.

## <a name="deploy-the-self-hosted-gateway-to-docker"></a>자체 호스팅 게이트웨이를 Docker에 배포

1. **배포 및 인프라**에서 **게이트웨이** 를 선택 합니다.
2. 배포 하려는 게이트웨이 리소스를 선택 합니다.
3. **배포**를 선택 합니다.
4. **토큰** 텍스트 상자의 액세스 토큰은 기본 **만료** 및 **비밀 키** 값을 사용 하 여 자동으로 자동 생성 되었습니다. 필요한 경우 또는 두 컨트롤에서 원하는 값을 선택 하 여 새 토큰을 생성 합니다.
4. **배포 스크립트**에서 **Docker** 가 선택 되어 있는지 확인 합니다.
5. **환경** 옆에 있는 **env** 파일 링크를 선택 하 여 파일을 다운로드 합니다.
6. **실행** 텍스트 상자의 오른쪽 끝에 있는 **복사** 아이콘을 선택 하 여 Docker 명령을 클립보드로 복사 합니다.
7. 명령을 터미널 (또는 명령) 창에 붙여넣습니다. 필요에 따라 포트 매핑 및 컨테이너 이름을 조정 합니다. 명령은 다운로드 한 환경 파일이 현재 디렉터리에 있는 것으로 가정 합니다.
```
    docker run -d -p 80:8080 -p 443:8081 --name <gateway-name> --env-file env.conf mcr.microsoft.com/azure-api-management/gateway:<tag>
```
8. 명령을 실행합니다. 명령은 Microsoft Container Registry에서 다운로드 한 [컨테이너 이미지](https://aka.ms/apim/sputnik/dhub) 를 사용 하 여 컨테이너를 실행 하 고 컨테이너의 HTTP (8080) 및 HTTPS (8081) 포트를 호스트의 포트 80 및 443에 매핑하기 위해 Docker 환경에 지시 합니다.
9. 아래 명령을 실행 하 여 게이트웨이 컨테이너가 실행 중인지 확인 합니다.
```console
docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                         NAMES
895ef0ecf13b        mcr.microsoft.com/azure-api-management/gateway:latest   "/bin/sh -c 'dotnet …"   5 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp, 0.0.0.0:443->8081/tcp   my-gateway
```
10. Azure Portal로 돌아가서 **개요** 를 클릭 하 고 방금 배포한 자체 호스팅 게이트웨이 컨테이너가 정상 상태를 보고 하는지 확인 합니다.

![게이트웨이 상태](media/how-to-deploy-self-hosted-gateway-docker/status.png)

> [!TIP]
> 명령을 <code>console docker container logs <gateway-name></code> 사용 하 여 자체 호스팅 게이트웨이 로그의 스냅숏을 봅니다.
>
> 명령을 <code>docker container logs --help</code> 사용 하 여 모든 로그 보기 옵션을 표시 합니다.

## <a name="next-steps"></a>다음 단계

* 자체 호스팅 게이트웨이에 대해 자세히 알아보려면 [Azure API Management 자체 호스팅 게이트웨이 개요](self-hosted-gateway-overview.md)를 참조 하세요.
* [자체 호스팅 게이트웨이의 사용자 지정 도메인 이름을 구성](api-management-howto-configure-custom-domain-gateway.md)합니다.
