---
title: 드라이버 및 도구 호환성-Azure Database for MariaDB
description: 이 문서에서는 Azure Database for MariaDB와 호환 되는 Aadb 드라이버 및 관리 도구에 대해 설명 합니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: a0cee198f028fd90e04dac15e98d7cd33aee9201
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79532351"
---
# <a name="mariadb-drivers-and-management-tools-compatible-with-azure-database-for-mariadb"></a>Azure Database for MariaDB와 호환 되는 MariaDB 드라이버 및 관리 도구

이 문서에서는 Azure Database for MariaDB와 호환 되는 드라이버 및 관리 도구에 대해 설명 합니다.

## <a name="mariadb-drivers"></a>MariaDB 드라이버

Azure Database for MariaDB은 community edition의 Aadb 서버를 사용 합니다. 따라서 다양한 프로그래밍 언어 및 드라이버와 호환됩니다. MariaDB의 API 및 프로토콜은 MySQL에서 사용 되는 API와 호환 됩니다. 즉, MySQL과 함께 작동 하는 커넥터는 MariaDB 에서도 작동 해야 합니다.

목표는 세 가지 최신 버전의 MariaDB 드라이버를 지 원하는 것이 고, 오픈 소스 커뮤니티의 작성자가 지속적으로 기능을 개선 하 고, 계속 해 서 Aadb 드라이버의 유용성을 개선 하는 것입니다. 다음 표에는 테스트를 거쳐 Azure Database for MariaDB 10.2와 호환 되는 것으로 확인 된 드라이버 목록이 나와 있습니다.

**드라이버** | **링크** | **호환되는 버전** | **호환되지 않는 버전** | **참고**
---|---|---|---|---
PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | PHP 7.0과 SSL MySQLi 연결의 경우 연결 문자열에 MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT를 추가합니다. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO 집합: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` 옵션을 false로 설정합니다.
.NET | [GitHub의 MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [Nuget의 설치 패키지](https://www.nuget.org/packages/MySqlConnector/) | 0.27 이상 | 0.26.5 이하 |
MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | 인코딩 버그로 인해 UTF8이 아닌 Windows 시스템 중 일부에서 연결이 실패할 수 있습니다.
Node.js |  [Github의 MySQLjs](https://github.com/mysqljs/mysql/) <br> NPM의 설치 패키지:<br> NPM에서 `npm install mysql` 실행 | 2.15 | 2.14.1 이하
이동 | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 이하 | 1.3 `allowNativePasswords=true` 버전에 대 한 연결 문자열에서를 사용 합니다. 버전 1.4에는 수정 프로그램이 `allowNativePasswords=true` 포함 되어 있으므로 더 이상 필요 하지 않습니다.
Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 이하 |
Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 이하 |

## <a name="management-tools"></a>관리 도구

호환성 장점은 데이터베이스 관리 도구까지 확장됩니다. 데이터베이스 조작이 사용자 권한 범위 내에서 작동 하는 한 기존 도구는 계속 해 서 Azure Database for MariaDB를 사용 해야 합니다. 다음 표에는 테스트를 거쳐 Azure Database for MariaDB 10.2와 호환 되는 것으로 확인 된 세 가지 공통 데이터베이스 관리 도구가 나와 있습니다.

| | **MySQL Workbench 6.x 이상** | **Navicat 12** | **PHPMyAdmin 4.x 이상**
---|---|---|---
만들기, 업데이트, 읽기, 쓰기, 삭제 | X | X | X
SSL 연결 | X | X | X
SQL 쿼리 자동 완성 | X | X |
데이터 가져오기 및 내보내기 | X | X | X
여러 형식으로 내보내기 | X | X | X
Backup 및 복원 |  | X |
서버 매개 변수 표시 | X | X | X
클라이언트 연결 표시 | X | X | X

## <a name="next-steps"></a>다음 단계

- [Azure Database for MariaDB에 대한 연결 문제 해결](howto-troubleshoot-common-connection-issues.md)