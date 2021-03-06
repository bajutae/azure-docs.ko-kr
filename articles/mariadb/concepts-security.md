---
title: 보안-Azure Database for MariaDB
description: Azure Database for MariaDB의 보안 기능에 대 한 개요입니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8f41fe1005e96b428337bc73b9d468962a079596
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79527829"
---
# <a name="security-in-azure-database-for-mariadb"></a>Azure Database for MariaDB의 보안

Azure Database for MariaDB 서버에서 데이터를 보호 하는 데 사용할 수 있는 여러 보안 계층이 있습니다. 이 문서에서는 이러한 보안 옵션을 설명 합니다.

## <a name="information-protection-and-encryption"></a>정보 보호 및 암호화

### <a name="in-transit"></a>전송 중
Azure Database for MariaDB 전송 계층 보안을 사용 하 여 전송 중인 데이터를 암호화 하 여 데이터를 보호 합니다. 암호화 (SSL/TLS)는 기본적으로 적용 됩니다.

### <a name="at-rest"></a>휴지 상태의
Azure Database for MariaDB 서비스는 미사용 데이터의 스토리지 암호화를 위해 FIPS 140-2 유효성 검사 암호화 모듈을 사용합니다. 백업을 비롯 한 데이터는 디스크에서 암호화 되며 쿼리를 실행 하는 동안 생성 된 임시 파일은 제외 됩니다. 이 서비스는 Azure 스토리지 암호화에 포함된 AES 256비트 암호화를 사용하며, 키는 시스템에서 관리됩니다. 스토리지 암호화는 항상 켜져 있고 해제할 수 없습니다.


## <a name="network-security"></a>네트워크 보안
Azure Database for MariaDB 서버에 대 한 연결은 먼저 지역 게이트웨이를 통해 라우팅됩니다. 게이트웨이는 공개적으로 액세스할 수 있는 IP를 포함 하지만 서버 IP 주소는 보호 됩니다. 게이트웨이에 대 한 자세한 내용은 [연결 아키텍처 문서](concepts-connectivity-architecture.md)를 참조 하세요.  

새로 만든 Azure Database for MariaDB 서버에는 모든 외부 연결을 차단 하는 방화벽이 있습니다. 게이트웨이는 게이트웨이에 연결 되지만 서버에 연결할 수는 없습니다. 

### <a name="ip-firewall-rules"></a>IP 방화벽 규칙
IP 방화벽 규칙은 각 요청의 원래 IP 주소에 따라 서버에 대 한 액세스 권한을 부여 합니다. 자세한 내용은 [방화벽 규칙 개요](concepts-firewall-rules.md) 를 참조 하세요.

### <a name="virtual-network-firewall-rules"></a>Virtual Network 방화벽 규칙
가상 네트워크 서비스 끝점은 Azure 백본을 통해 가상 네트워크 연결을 확장 합니다. 가상 네트워크 규칙을 사용 하 여 가상 네트워크에서 선택한 서브넷의 연결을 허용 하도록 Azure Database for MariaDB 서버를 설정할 수 있습니다. 자세한 내용은 [가상 네트워크 서비스 끝점 개요](concepts-data-access-security-vnet.md)를 참조 하세요.


## <a name="access-management"></a>액세스 관리

Azure Database for MariaDB 서버를 만드는 동안 관리자 사용자에 대 한 자격 증명을 제공 합니다. 이 관리자를 사용 하 여 추가 Aadb 사용자를 만들 수 있습니다.


## <a name="threat-protection"></a>위협 방지

비정상적인 활동을 검색 하는 [고급 위협 방지](concepts-data-access-and-security-threat-protection.md) 를 옵트인 (opt in) 하 여 서버에 액세스 하거나 악용 하려는 비정상적인 시도를 발견할 수 있습니다.

[감사 로깅은](concepts-audit-logs.md) 데이터베이스의 활동을 추적 하는 데 사용할 수 있습니다. 


## <a name="next-steps"></a>다음 단계
- [Ip](concepts-firewall-rules.md) 또는 [가상 네트워크](concepts-data-access-security-vnet.md) 에 대 한 방화벽 규칙 사용