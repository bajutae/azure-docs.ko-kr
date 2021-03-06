---
title: Azure Disk Encryption 문제 해결 가이드
description: 이 문서에서는 Windows Vm 용 Microsoft Azure 디스크 암호화에 대 한 문제 해결 팁을 제공 합니다.
author: msmbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 11c1e0bf10725173a2a341addf4c3f845bbb7fba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82085691"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Encryption 문제 해결 가이드

이 가이드는 조직에서 Azure Disk Encryption을 사용하는 IT 전문가, 정보 보안 분석가 및 클라우드 관리자를 위한 것입니다. 이 문서는 디스크 암호화 관련 문제를 해결하는 데 도움을 드리기 위해 작성되었습니다.

아래 단계를 수행 하기 전에 먼저 암호화 하려는 vm이 지원 되는 [vm 크기 및 운영 체제](disk-encryption-overview.md#supported-vms-and-operating-systems)중 어느 것이 고 모든 필수 구성 요소를 충족 하는지 확인 합니다.

- [네트워킹 요구 사항](disk-encryption-overview.md#networking-requirements)
- [그룹 정책 요구 사항](disk-encryption-overview.md#group-policy-requirements)
- [암호화 키 저장소 요구 사항](disk-encryption-overview.md#encryption-key-storage-requirements)

 

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>방화벽 뒤에 있는 Azure Disk Encryption 문제 해결

방화벽, 프록시 요구 사항 또는 NSG(네트워크 보안 그룹) 설정으로 연결이 제한되면 필요한 작업을 수행할 수 있는 확장의 기능이 중단될 수 있습니다. 이로 인해 "VM에서 사용할 수 없는 확장 상태"와 같은 상태 메시지가 표시됩니다. 예상된 시나리오에서 암호화가 완료되지 못할 수 있습니다. 다음 섹션에서는 조사할 수 있는 몇 가지 일반적인 방화벽 문제가 나와 있습니다.

### <a name="network-security-groups"></a>네트워크 보안 그룹
적용되는 모든 네트워크 보안 그룹 설정은 엔드포인트에서도 디스크 암호화에 대해 문서화된 네트워크 구성 [필수 조건](disk-encryption-overview.md#networking-requirements)을 충족해야 합니다.

### <a name="azure-key-vault-behind-a-firewall"></a>방화벽 뒤에 있는 Azure Key Vault

[Azure AD 자격 증명](disk-encryption-windows-aad.md#)에 암호화가 사용하도록 설정된 경우 대상 VM은 Azure Active Directory 엔드포인트 및 Key Vault 엔드포인트 모두에 대한 연결을 허용해야 합니다. 현재 Azure Active Directory 인증 엔드포인트는 [Office 365 URL 및 IP 주소 범위](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) 설명서의 56 및 59 섹션에서 유지 관리됩니다. Key Vault 지침은 [방화벽 뒤에 있는 Azure Key Vault에 액세스](../../key-vault/general/access-behind-firewall.md)하는 방법에 관한 설명서에서 제공됩니다.

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service 
VM은 VM 내에서만 액세스할 수 있는 잘 알려진 라우팅이 불가능한 IP 주소(`169.254.169.254`)를 사용하는 [Azure Instance Metadata 서비스](../windows/instance-metadata-service.md) 엔드포인트에 액세스할 수 있어야 합니다.  이 주소에 대 한 로컬 HTTP 트래픽 (예: X 전달 된 헤더 추가)을 변경 하는 프록시 구성은 지원 되지 않습니다.

## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Server Core 문제 해결

Windows Server 2016 Server Core에서 bdehdcfg 구성 요소는 기본적으로 사용할 수 없습니다. 이 구성 요소는 Azure Disk Encryption에 필요합니다. VM의 수명 동안 한 번만 수행되는 OS 볼륨에서 시스템 볼륨을 분할하는 데 사용됩니다. 이러한 이진 파일은 이후 암호화 작업 중 필요하지 않습니다.

이 문제를 해결하려면 Windows Server 2016 Data Center VM의 다음 4개 파일을 Server Core의 동일한 위치에 복사합니다.

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. 다음 명령을 입력합니다.

   ```
   bdehdcfg.exe -target default
   ```

1. 이 명령은 550MB 시스템 파티션을 만듭니다. 시스템 다시 부팅합니다.

1. DiskPart를 사용하여 볼륨을 확인한 다음 계속합니다.  

다음은 그 예입니다.

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```

## <a name="troubleshooting-encryption-status"></a>암호화 상태 문제 해결 

포털은 VM 내에서 암호화 되지 않은 경우에도 디스크를 암호화 된 상태로 표시할 수 있습니다.  낮은 수준의 명령을 사용 하 여 VM 내에서 디스크의 암호를 직접 해독 하는 데 더 높은 수준의 Azure Disk Encryption 관리 명령을 사용 하는 대신이 오류가 발생할 수 있습니다.  더 높은 수준의 명령은 VM 내에서 디스크의 암호를 해독 하는 것이 아니라 vm 외부의 중요 한 플랫폼 수준 암호화 설정 및 VM에 연결 된 확장 설정도 업데이트 합니다.  이러한 설정이 그대로 유지 되지 않으면 플랫폼이 암호화 상태를 보고 하거나 VM을 올바르게 프로 비전 할 수 없습니다.   

PowerShell을 사용 하 여 Azure Disk Encryption를 사용 하지 않도록 설정 하려면 [AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) 뒤에 [AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension)을 사용 합니다. 암호화를 사용 하지 않도록 설정 하기 전에 AzVMDiskEncryptionExtension를 실행 하면 오류가 발생 합니다.

CLI를 사용 하 여 Azure Disk Encryption를 사용 하지 않도록 설정 하려면 [az vm Encryption disable](/cli/azure/vm/encryption)을 사용 합니다. 

## <a name="next-steps"></a>다음 단계

이 문서에서는 일반적인 Azure Disk Encryption 문제와 해당 문제 해결 방법에 대해 자세히 알아보았습니다. 이 서비스 및 기능에 대한 자세한 내용은 다음 문서를 참조하세요.

- [Azure Security Center에서 디스크 암호화 적용](../../security-center/security-center-apply-disk-encryption.md)
- [휴지 상태의 Azure 데이터 암호화](../../security/fundamentals/encryption-atrest.md)
