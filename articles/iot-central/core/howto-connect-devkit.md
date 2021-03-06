---
title: Azure IoT Central 애플리케이션에 DevKit 디바이스 연결 | Microsoft Docs
description: 장치 개발자는 IoT 플러그 앤 플레이 (미리 보기)를 사용 하 여 MXChip IoT DevKit 장치를 Azure IoT Central 응용 프로그램에 연결 하는 방법을 알아봅니다.
author: liydu
ms.author: liydu
ms.date: 12/03/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: bcf1dd2f89cf049d7da5b56170b2c13874c83ba4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81756801"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Azure IoT Central 애플리케이션에 MXChip IoT DevKit 디바이스 연결

*이 문서는 장치 개발자에 게 적용 됩니다.*

이 문서에서는 DevKit (MXChip IoT DevKit) 장치를 Azure IoT Central 응용 프로그램에 연결 하는 방법을 보여 줍니다. 장치는 DevKit 장치에 대해 인증 된 IoT 플러그 앤 플레이 (미리 보기) 모델을 사용 하 여 IoT Central에 대 한 연결을 구성 합니다.

이 방법 문서에서는 다음을 수행 합니다.

- IoT Central 응용 프로그램에서 연결 정보를 가져옵니다.
- 장치를 준비 하 고 IoT Central 응용 프로그램에 연결 합니다.
- IoT Central의 장치에서 원격 분석 및 속성을 확인 합니다.

## <a name="prerequisites"></a>전제 조건

이 문서의 단계를 완료하려면 다음 리소스가 필요합니다.

- [Devkit 장치](https://aka.ms/iot-devkit-purchase)입니다.
- IoT Central 응용 프로그램입니다. [IoT Central 응용 프로그램 만들기](./quick-deploy-iot-central.md)의 단계를 따를 수 있습니다.

## <a name="get-device-connection-details"></a>장치 연결 정보 가져오기

1. Azure IoT Central 응용 프로그램에서 **장치 템플릿** 탭을 선택 하 고 **+ 새로 만들기**를 선택 합니다. **미리 구성 된 장치 템플릿 사용**섹션에서 **MXChip IoT devkit**를 선택 합니다.

    ![MXChip IoT DevKit 용 장치 템플릿](media/howto-connect-devkit/device-template.png)

1. **다음: 사용자 지정** 을 선택 하 고 **만들기**를 선택 합니다.

1. **장치** 탭을 선택 합니다. 장치 목록에서 **MXChip IoT DevKit** 를 선택 하 고 **+ 새로** 만들기를 선택 하 여 템플릿에서 새 장치를 만듭니다.

    ![새 장치](media/howto-connect-devkit/new-device.png)

1. 팝업 창에서 **장치 ID** 를로 입력 `SampleDevKit` 하 고 **장치 이름을** 으로 `MXChip IoT DevKit - Sample`입력 합니다. **시뮬레이트된** 옵션이 해제 되어 있는지 확인 합니다. 그런 다음 **만들기**를 선택합니다.

    ![장치 ID 및 이름](media/howto-connect-devkit/device-id-name.png)

1. 만든 장치를 선택 하 고 **연결**을 선택 합니다. **Id 범위**, **장치 id**및 **기본 키**를 적어둡니다. 이 방법 문서 뒷부분에서 이러한 값이 필요 합니다.

    ![장치 연결 정보](media/howto-connect-devkit/device-connection-info.png)

## <a name="prepare-the-device"></a>장치 준비

1. GitHub에서 DevKit 장치에 대해 [미리 작성 된 최신 Azure IoT Central 플러그 앤 플레이 (미리 보기) 펌웨어](https://github.com/Azure-Samples/mxchip-iot-devkit-pnp/raw/master/bin/iotc_devkit.bin) 를 다운로드 합니다.

1. USB 케이블을 사용하여 DevKit 디바이스를 개발 컴퓨터에 연결합니다. Windows에서, DevKit 디바이스의 스토리지에 매핑된 드라이브에서 파일 탐색기 창이 열립니다. 예를 들어 드라이브 이름이 **AZ3166 (D:)** 일 수 있습니다.

1. **Iotc_devkit** 파일을 드라이브 창으로 끌어 놓습니다. 복사가 완료되면 새 펌웨어를 사용하여 디바이스가 다시 부팅됩니다.

    > [!NOTE]
    > **Wi-fi가 없는**경우와 같이 화면에 오류가 표시 되 면 devkit가 아직 WiFi에 연결 되지 않았기 때문입니다.

1. DevKit에서 **단추 b**를 길게 누른 다음 **다시 설정** 단추를 눌렀다가 놓고 **단추 b**를 해제 합니다. 장치는 이제 액세스 포인트 모드입니다. 확인 하려면 화면에 "IoT DevKit-AP"와 구성 포털 IP 주소가 표시 됩니다.

1. 컴퓨터 또는 태블릿에서 장치 화면에 표시 된 WiFi 네트워크 이름에 연결 합니다. WiFi 네트워크는 **AZ-** 다음에 MAC 주소를 사용 하 여 시작 합니다. 이 네트워크에 연결 하면 인터넷에 액세스할 수 없습니다. 이 상태는 정상적인 상태 이며 장치를 구성 하는 동안 잠시 동안만이 네트워크에 연결 합니다.

1. 웹 브라우저를 열고로 [http://192.168.0.1/](http://192.168.0.1/)이동 합니다. 다음 웹 페이지가 표시됩니다.

    ![구성 UI](media/howto-connect-devkit/config-ui.png)

    웹 페이지에서 다음을 입력 합니다.

    - WiFi 네트워크 (SSID)의 이름입니다.
    - 사용자의 WiFi 네트워크 암호입니다.
    - 연결 세부 정보: 이전에 기록해 둔 **장치 id**, **Id 범위**및 **SAS 기본 키** 를 입력 합니다.

    > [!NOTE]
    > 현재 IoT DevKit는 2.4 GHz Wi-fi에만 연결할 수 있으며, 하드웨어 제한으로 인해 5ghz는 지원 되지 않습니다.

1. **장치 구성**을 선택 하면 devkit 장치가 다시 부팅 되 고 응용 프로그램이 실행 됩니다.

    ![다시 부팅 UI](media/howto-connect-devkit/reboot-ui.png)

    DevKit 화면에 응용 프로그램이 실행 되 고 있음을 확인 하는 메시지가 표시 됩니다.

    ![DevKit 실행 중](media/howto-connect-devkit/devkit-running.png)

DevKit는 먼저 IoT Central 응용 프로그램에 새 장치를 등록 한 다음 데이터 보내기를 시작 합니다.

## <a name="view-the-telemetry"></a>원격 분석 보기

이 단계에서는 Azure IoT Central 응용 프로그램에서 원격 분석을 봅니다.

IoT Central 응용 프로그램에서 **장치** 탭을 선택 하 고 추가한 장치를 선택 합니다. **개요** 탭에서 devkit 장치의 원격 분석을 볼 수 있습니다.

![IoT Central 장치 개요](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>코드 검토

코드를 검토 하거나 코드를 수정 하 고 컴파일하려면 [코드 샘플](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/)로 이동 합니다.

## <a name="next-steps"></a>다음 단계

장치 개발자 인 경우 몇 가지 제안 되는 다음 단계는 다음과 같습니다.

- [Azure IoT Central에서 장치 연결](./concepts-get-connected.md) 에 대해 읽기
- [Azure CLI를 사용 하 여 장치 연결을 모니터링](./howto-monitor-devices-azure-cli.md) 하는 방법 알아보기
