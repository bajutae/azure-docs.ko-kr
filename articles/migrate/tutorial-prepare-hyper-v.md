---
title: Azure Migrate를 사용하여 평가/마이그레이션을 위해 Hyper-V VM 준비
description: Azure Migrate를 사용하여 Hyper-V VM의 평가/마이그레이션을 준비하는 방법을 알아봅니다.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: ea6290eae187ed4ead39f7307a84a54a23c19c29
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641627"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>평가하고 Azure로 마이그레이션할 Hyper-V VM 준비

이 문서에서는 [Azure Migrate:Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool)를 사용하여 온-프레미스 Hyper-V VM의 평가를 준비하고 [Azure Migrate:Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool)을 사용하여 Hyper-V VM을 마이그레이션하는 방법을 설명합니다.


이 자습서는 Hyper-V VM을 평가하고 Azure로 마이그레이션하는 방법을 보여 주는 시리즈의 첫 번째 자습서입니다. 이 자습서에서는 다음 작업 방법을 알아봅니다.

> [!div class="checklist"]
> * Azure를 준비합니다. Azure Migrate에서 사용할 Azure 계정 및 리소스에 대한 권한을 설정합니다.
> * 서버 평가를 위해 온-프레미스 Hyper-V 호스트 및 VM을 준비합니다. 구성 스크립트를 사용하거나 수동으로 준비할 수 있습니다.
> * Azure Migrate 어플라이언스의 배포를 준비합니다. 어플라이언스는 온-프레미스 VM을 검색하고 평가하는 데 사용됩니다.
> * 서버 마이그레이션을 위해 온-프레미스 Hyper-V 호스트 및 VM을 준비합니다.


> [!NOTE]
> 자습서에서는 개념 증명을 빠르게 설정할 수 있도록 시나리오에 대한 가장 간단한 배포 경로를 보여 줍니다. 자습서는 가능한 경우 기본 옵션을 사용하며, 가능한 모든 설정과 경로는 보여 주지 않습니다. 자세한 지침은 Hyper-V를 평가하고 마이그레이션하는 방법을 검토하세요.


Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/pricing/free-trial/)을 만듭니다.


## <a name="prepare-azure"></a>Azure 준비

### <a name="azure-permissions"></a>Azure 권한

Azure Migrate 배포를 위한 권한을 설정해야 합니다.

**Task** | **세부 정보** 
--- | --- 
**Azure Migrate 프로젝트 만들기** | Azure 계정에는 프로젝트를 만들 수 있는 기여자 또는 소유자 권한이 있어야 합니다. | 
**리소스 공급자 등록** | Azure Migrate는 경량 Azure Migrate 어플라이언스를 사용하여 Azure Migrate 서버 평가를 통해 Hyper-V VM을 검색하고 평가합니다.<br/><br/> 어플라이언스 등록 중에 리소스 공급자는 어플라이언스에서 선택한 구독에 등록됩니다. [자세히 알아보기](migrate-appliance-architecture.md#appliance-registration).<br/><br/> 리소스 공급자를 등록하려면 구독에 대한 기여자 또는 소유자 역할이 필요합니다.
**Azure AD 앱 만들기** | 어플라이언스를 등록할 때 Azure Migrate는 어플라이언스에서 실행되는 에이전트와 Azure에서 실행되는 각 서비스 간의 통신에 사용되는 Azure AD(Azure Active Directory) 앱을 만듭니다. [자세히 알아보기](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD 앱(애플리케이션 개발자에서 사용 가능) 역할을 만들 수 있는 권한이 필요합니다.



### <a name="assign-permissions-to-create-project"></a>프로젝트를 만들 수 있는 권한 할당

Azure Migrate 프로젝트를 만들 수 있는 권한이 있는지 확인합니다.

1. Azure Portal에서 구독을 열고, **액세스 제어(IAM)** 를 선택합니다.
2. **액세스 권한 확인**에서 관련 계정을 찾아 클릭하여 권한을 확인합니다.
3. **기여자** 또는 **소유자** 권한이 있어야 합니다.
    - Azure 체험 계정을 방금 만든 경우 자신이 구독에 대한 소유자입니다.
    - 구독 소유자가 아닌 경우 해당 역할을 할당해 주도록 소유자에게 문의합니다.


### <a name="assign-permissions-to-register-the-appliance"></a>어플라이언스를 등록할 수 있는 권한 할당

다음 방법 중 하나를 사용하여 어플라이언스 등록 중에 Azure AD 앱을 만들 수 있는 Azure Migrate에 대한 사용 권한을 할당할 수 있습니다.

- 테넌트/글로벌 관리자는 Azure AD 앱을 만들고 등록할 수 있는 권한을 테넌트의 사용자에게 부여할 수 있습니다.
- 테넌트/글로벌 관리자는 권한이 있는 애플리케이션 개발자 역할을 계정에 할당할 수 있습니다.

> [!NOTE]
> - 앱에는 위에서 설명한 구독에 대한 액세스 권한 이외의 다른 권한이 없습니다.
> - 이러한 권한은 새 어플라이언스를 등록할 때만 필요합니다. 어플라이언스가 설정되면 해당 권한을 제거할 수 있습니다.


#### <a name="grant-account-permissions"></a>계정 권한 부여

테넌트/글로벌 관리자는 다음과 같이 권한을 부여할 수 있습니다.

1. Azure AD에서 테넌트/글로벌 관리자는 **Azure Active Directory** > **사용자** > **사용자 설정**으로 이동해야 합니다.
2. 관리자는 **앱 등록**을 **예**로 설정해야 합니다.

    ![Azure AD 권한](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> 이것이 중요한 내용이 포함되지 않는 기본 설정입니다. [자세히 알아보기](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>애플리케이션 개발자 역할 할당

테넌트/글로벌 관리자는 애플리케이션 개발자 역할을 계정에 할당할 수 있습니다. [자세히 알아보기](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-hyper-v-for-assessment"></a>평가를 위한 Hyper-V 준비

VM 평가를 위해 Hyper-V를 수동으로 또는 구성 스크립트를 사용하여 준비할 수 있습니다. 준비 단계는 다음과 같습니다.
- Hyper-V 호스트 설정을 [확인](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements)하고 Hyper-V 호스트에 [필요한 포트](migrate-support-matrix-hyper-v.md#port-access)가 열려 있는지 확인합니다.
- Azure Migrate 어플라이언스에서 WinRM 연결을 통해 호스트에서 PowerShell 명령을 실행할 수 있도록 각 호스트에서 PowerShell 원격을 설정합니다.
- VM 디스크가 원격 SMB 공유에 있는 경우 자격 증명을 위임합니다.
- 어플라이언스가 Hyper-V 호스트에서 VM을 검색하는 데 사용할 계정을 설정합니다.
- 검색하고 평가하려는 각 VM에서 Hyper-V Integration Services를 설정합니다. Integration Services를 사용하도록 설정하는 경우 Azure Migrate에는 기본 설정으로 충분합니다.

    ![Integration Services 사용](./media/tutorial-prepare-hyper-v/integrated-services.png)


## <a name="prepare-with-a-script"></a>스크립트로 준비

이 스크립트는 다음을 수행합니다.

- 지원되는 PowerShell 버전에서 스크립트를 실행 중인지 확인합니다.
- 사용자(스크립트를 실행하는 사용자)에게 Hyper-V 호스트에 대한 관리자 권한이 있는지 확인합니다.
- Azure Migrate 서비스에서 Hyper-V 호스트와 통신하는 데 사용되는 로컬 사용자 계정(관리자 아님)을 만들 수 있습니다. 이 사용자 계정은 호스트의 다음 그룹에 추가됩니다.
    - 원격 관리 사용자
    - Hyper-V 관리자
    - 성능 모니터 사용자
- 호스트에서 지원되는 버전의 Hyper-V 및 Hyper-V 역할을 실행하는지 확인합니다.
- WinRM 서비스를 사용하도록 설정하고, 호스트에서 5985(HTTP) 및 5986(HTTPS) 포트를 엽니다(메타데이터 수집에 필요함).
- 호스트에서 PowerShell 원격을 사용하도록 설정합니다.
- 호스트에서 관리하는 모든 VM에 Hyper-V Integration Services를 사용하도록 설정되어 있는지 확인합니다.
- 필요한 경우 호스트에서 CredSSP를 사용하도록 설정합니다.

스크립트를 다음과 같이 실행합니다.

1. PowerShell 버전 4.0 이상이 Hyper-V 호스트에 설치되어 있는지 확인합니다.
2. [Microsoft 다운로드 센터](https://aka.ms/migrate/script/hyperv)에서 스크립트를 다운로드합니다. 이 스크립트는 Microsoft에서 암호화 방식으로 서명합니다.
3. MD5 또는 SHA256 해시 파일을 사용하여 스크립트 무결성의 유효성을 검사합니다. 해시태그 값은 아래와 같습니다. 다음 명령을 실행하여 스크립트에 대한 해시를 생성합니다.
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    사용 예제:
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.    스크립트 무결성의 유효성이 검사되면 다음 PowerShell 명령을 사용하여 각 Hyper-V 호스트에서 스크립트를 실행합니다.
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>해시태그 값

해시 값은 다음과 같습니다.

| **해시** | **값** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-manually"></a>수동으로 준비

스크립트를 사용하는 대신 이 섹션의 절차에 따라 Hyper-V를 수동으로 준비합니다.

### <a name="verify-powershell-version"></a>PowerShell 버전 확인

PowerShell 버전 4.0 이상이 Hyper-V 호스트에 설치되어 있는지 확인합니다.



### <a name="set-up-an-account-for-vm-discovery"></a>VM 검색용 계정 설정

Azure Migrate에는 온-프레미스 VM을 검색할 수 있는 권한이 필요합니다.

- Hyper-V 호스트/클러스터에 대한 관리자 권한이 있는 도메인 또는 로컬 사용자 계정을 설정합니다.

    - 검색에 포함하려는 모든 호스트 및 클러스터에 대해 단일 계정이 필요합니다.
    - 계정은 로컬 또는 도메인 계정일 수 있습니다. Hyper-V 호스트 또는 클러스터에 대한 관리자 권한이 있는 것이 좋습니다.
    - 또는 관리자 권한을 할당하지 않으려면 다음 권한이 필요합니다.
        - 원격 관리 사용자
        - Hyper-V 관리자
        - 성능 모니터 사용자

### <a name="verify-hyper-v-host-settings"></a>Hyper-V 호스트 설정 확인

1. 서버 평가에 대한 [Hyper-V 호스트 요구 사항](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements)을 확인합니다.
2. [필요한 포트](migrate-support-matrix-hyper-v.md#port-access)가 Hyper-V 호스트에서 열려 있는지 확인합니다.

### <a name="enable-powershell-remoting-on-hosts"></a>호스트에서 PowerShell 원격을 사용하도록 설정

각 호스트에서 다음과 같이 PowerShell 원격을 설정합니다.

1. 각 호스트에서 관리자 권한으로 PowerShell 콘솔을 엽니다.
2. 다음 명령을 실행합니다.

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>VM에서 Integration Services를 사용하도록 설정

Azure Migrate에서 VM의 운영 체제 정보를 캡처할 수 있도록 각 VM에서 Integration Services를 사용하도록 설정해야 합니다.

검색하고 평가하려는 VM의 경우 각 VM에서 [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services)를 사용하도록 설정합니다.


### <a name="enable-credssp-on-hosts"></a>호스트에서 CredSSP를 사용하도록 설정

디스크가 SMB 공유에 있는 VM이 호스트에 있는 경우 호스트에서 이 단계를 완료합니다.

- 이 명령은 모든 Hyper-V 호스트에서 원격으로 실행할 수 있습니다.
- 새 호스트 노드를 클러스터에 추가하면 검색을 위해 자동으로 추가되지만, 필요한 경우 새 노드에서 CredSSP를 사용하도록 수동으로 설정해야 합니다.

다음과 같이 사용하도록 설정합니다.

1. SMB 공유의 디스크를 사용하여 Hyper-V VM을 실행하는 Hyper-V 호스트를 식별합니다.
2. 식별된 각 Hyper-V 호스트에서 다음 명령을 실행합니다.

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

어플라이언스가 설정되면 [어플라이언스에서 CredSSP를 사용하도록 설정](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds)하여 해당 설정을 완료합니다. 이에 대해서는 이 시리즈의 다음 자습서에서 설명합니다.


## <a name="prepare-for-appliance-deployment"></a>어플라이언스 배포를 위한 준비

Azure Migrate 어플라이언스를 설정하고 다음 자습서에서 평가를 시작하기 전에 어플라이언스 배포를 준비합니다.

1. 어플라이언스 요구 사항을 [확인](migrate-appliance.md#appliance---hyper-v)합니다.
2. 어플라이언스가 [퍼블릭](migrate-appliance.md#public-cloud-urls) 및 [정부](migrate-appliance.md#government-cloud-urls) 클라우드에서 액세스해야 하는 Azure URL을 검토합니다. URL 기반 방화벽 또는 프록시를 사용하는 경우 필요한 URL에 대한 액세스를 허용하는지 확인합니다.
3. 검색 및 평가 중에 어플라이언스가 수집할 데이터를 검토합니다.
4. 어플라이언스에 대한 포트 액세스 요구 사항을 [검토](migrate-appliance.md#collected-data---hyper-v)합니다.




## <a name="prepare-for-hyper-v-migration"></a>Hyper-V 마이그레이션 준비

1. 마이그레이션을 위한 Hyper-V 호스트 요구 사항 및 VM 마이그레이션을 위해 Hyper-V 호스트 및 클러스터에서 액세스해야 하는 Azure URL을 [검토](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts)합니다.
2. Azure로 마이그레이션하려는 Hyper-V VM에 대한 요구 사항을 [검토](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms)합니다.


## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음을 수행합니다.

> [!div class="checklist"]
> * Azure 계정 권한을 설정합니다.
> * 평가 및 마이그레이션을 위해 Hyper-V 호스트 및 VM을 준비합니다.
> * Azure Migrate 어플라이언스의 배포를 준비합니다.

다음 자습서를 계속 진행하여 Azure Migrate 프로젝트를 만들고 Hyper-V VM을 검색 및 평가하여 Azure로 마이그레이션합니다.

> [!div class="nextstepaction"]
> [Hyper-V VM 평가](./tutorial-assess-hyper-v.md)
