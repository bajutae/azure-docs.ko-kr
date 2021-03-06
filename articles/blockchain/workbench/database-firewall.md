---
title: Azure Blockchain 워크 벤치 데이터베이스 방화벽 구성
description: 외부 클라이언트와 응용 프로그램의 연결을 허용 하도록 Azure Blockchain 워크 벤치 미리 보기 데이터베이스 방화벽을 구성 하는 방법에 대해 알아봅니다.
ms.date: 09/09/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: e6739b7ead9299f020465d3c50ed01826334ca76
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74325994"
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Azure Blockchain Workbench 데이터베이스 방화벽 구성

이 문서에서는 Azure Portal을 사용하여 방화벽 규칙을 구성하는 방법을 보여줍니다. 방화벽 규칙을 사용하여 외부 클라이언트 또는 애플리케이션이 Azure Blockchain Workbench 데이터베이스에 연결하도록 허용할 수 있습니다.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain Workbench 데이터베이스에 연결

규칙을 구성할 데이터베이스에 연결하려면:

1. Azure Blockchain 워크 벤치 리소스에 대 한 **소유자** 권한이 있는 계정으로 Azure Portal에 로그인 합니다.
2. 왼쪽의 탐색 창에서 **리소스 그룹**을 선택합니다.
3. Blockchain Workbench 배포에 대한 리소스 그룹의 이름을 선택합니다.
4. **형식**을 선택하여 리소스 목록을 정렬하고 **SQL 서버**를 선택합니다.
5. 다음 화면 캡처의 리소스 목록 예제는 두 개의 데이터베이스 *master* 및 *lsgn-sdk*를 보여줍니다. *lsgn-sdk*에 대한 방화벽 규칙을 구성합니다.

![Blockchain Workbench 리소스 나열](./media/database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>데이터베이스 방화벽 규칙 만들기

방화벽 규칙을 만들려면:

1. "lsgn-sdk" 데이터베이스 링크를 선택합니다.
2. 메뉴 모음에서 **서버 방화벽 설정**을 선택합니다.

   ![서버 방화벽 설정](./media/database-firewall/configure-server-firewall.png)

3. 조직의 규칙을 만들려면:

   * **규칙 이름**을 입력합니다.
   * 주소 범위의 **시작 IP**에 대한 IP 주소를 입력합니다.
   * 주소 범위의 **끝 IP**에 대한 IP 주소를 입력합니다.

   ![방화벽 규칙 만들기](./media/database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > 컴퓨터의 IP 주소만 추가하려면 **+ 클라이언트 IP 추가**를 선택합니다.
        
1. 방화벽 구성을 저장하려면 **저장**을 선택합니다.
2. 애플리케이션 또는 도구에서 연결하여 데이터베이스에 대해 구성한 IP 주소 범위를 테스트합니다. SQL Server Management Studio를 예로 들 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench의 데이터베이스 뷰](database-views.md)