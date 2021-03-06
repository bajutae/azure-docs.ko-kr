---
title: Azure 기본 제공 역할-Azure RBAC
description: 이 문서에서는 azure RBAC (역할 기반 액세스 제어)에 대 한 Azure 기본 제공 역할에 대해 설명 합니다. Actions, NotActions, DataActions 및 NotDataActions를 나열 합니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/04/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: b5ba4b84fcd9c1722e8ab2f4031ec1551357e406
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82869981"
---
# <a name="azure-built-in-roles"></a>Azure 기본 제공 역할

Azure [RBAC (역할 기반 액세스 제어)](overview.md) 에는 사용자, 그룹, 서비스 사용자 및 관리 되는 id에 할당할 수 있는 몇 가지 azure 기본 제공 역할이 있습니다. 역할 할당은 Azure 리소스에 대한 액세스를 제어하는 방법입니다. 기본 제공 역할이 조직의 특정 요구를 충족 하지 않는 경우 고유한 [Azure 사용자 지정 역할](custom-roles.md)을 만들 수 있습니다.

이 문서에는 항상 진화 하는 Azure 기본 제공 역할이 나열 되어 있습니다. 최신 역할을 가져오려면 [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) 또는 [az role definition list](/cli/azure/role/definition#az-role-definition-list)를 사용합니다. Azure Active Directory (Azure AD)에 대 한 관리자 역할을 찾고 [있는 경우 Azure Active Directory의 관리자 역할 권한](../active-directory/users-groups-roles/directory-assign-admin-roles.md)을 참조 하세요.

## <a name="all"></a>모두

다음 표에서는 각 기본 제공 역할의 간단한 설명 및 고유 ID를 제공 합니다. 역할 이름을 선택 하 여 각 역할에 대 `Actions`한 `NotActions`, `DataActions`, 및 `NotDataActions` 목록을 표시 합니다. 이러한 작업의 의미와 관리 및 데이터 평면에 적용 되는 방법에 대 한 자세한 내용은 [Azure 역할 정의 이해](role-definitions.md)를 참조 하세요.


> [!div class="mx-tableFixed"]
> | 기본 제공 역할 | 설명 | ID |
> | --- | --- | --- |
> | **일반** |  |  |
> | [기여자](#contributor) | 리소스 액세스 권한을 부여 하는 것을 제외 하 고 모든 항목을 관리할 수 있습니다. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [소유자](#owner) | 리소스 액세스를 비롯한 모든 것을 관리할 수 있습니다. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [판독기](#reader) | 모든 것을 볼 수 있지만 변경은 할 수 없습니다. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [사용자 액세스 관리자](#user-access-administrator) | Azure 리소스에 대한 사용자 액세스를 관리할 수 있습니다. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Compute** |  |  |
> | [Classic Virtual Machine 참가자](#classic-virtual-machine-contributor) | 클래식 가상 머신을 관리할 수 있지만 가상 머신이나 연결된 가상 네트워크 또는 스토리지 계정에 액세스할 수는 없습니다. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [가상 컴퓨터 관리자 로그인](#virtual-machine-administrator-login) | 포털에서 Virtual Machines를 보고 관리자 권한으로 로그인합니다. | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Virtual Machine 참가자](#virtual-machine-contributor) | 가상 머신을 관리할 수 있지만 가상머신이나 연결된 가상 네트워크 또는 스토리지 계정에 액세스할 수는 없습니다. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [가상 컴퓨터 사용자 로그인](#virtual-machine-user-login) | 포털에서 Virtual Machines를 보고 일반 사용자 권한으로 로그인합니다. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **네트워킹** |  |  |
> | [CD 엔드포인트 참가자](#cdn-endpoint-contributor) | CDN 끝점을 관리할 수 있지만 다른 사용자에 게 액세스 권한을 부여할 수는 없습니다. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [CDN 엔드포인트 독자](#cdn-endpoint-reader) | CDN 끝점을 볼 수 있지만 변경할 수는 없습니다. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [CDN 프로필 기여자](#cdn-profile-contributor) | CDN 프로필과 해당 끝점을 관리할 수 있지만 다른 사용자에 게 액세스 권한을 부여할 수는 없습니다. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [CDN 프로필 독자](#cdn-profile-reader) | CDN 프로필과 해당 끝점을 볼 수 있지만 변경할 수는 없습니다. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [클래식 네트워크 기여자](#classic-network-contributor) | 기본 네트워크를 관리할 수 있지만 액세스할 수는 없습니다. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [DNS 영역 참가자](#dns-zone-contributor) | Azure DNS의 DNS 영역과 레코드 집합을 관리할 수 있지만 액세스할 수 있는 사람을 제어할 수는 없습니다. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [네트워크 기여자](#network-contributor) | 네트워크를 관리할 수 있지만 액세스할 수는 없습니다. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Traffic Manager 기여자](#traffic-manager-contributor) | Traffic Manager 프로필을 관리할 수 있지만 액세스할 수 있는 사람을 제어할 수는 없습니다. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **스토리지** |  |  |
> | [Avere 기여자](#avere-contributor) | Avere vFXT 클러스터를 만들고 관리할 수 있습니다. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Avere 연산자](#avere-operator) | Avere vFXT 클러스터에서 클러스터를 관리 하는 데 사용 됩니다. | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [백업 참가자](#backup-contributor) | 백업 서비스를 관리할 수 있지만 자격 증명 모음을 만들고 다른 사용자에 게 액세스 권한을 부여할 수 없습니다. | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | [Backup 운영자](#backup-operator) | 백업 제거를 제외한 백업 서비스를 관리하고 자격 증명 모음 만들고 다른 사람에게 액세스 권한을 부여할 수 있습니다. | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Backup 읽기 권한자](#backup-reader) | 백업 서비스를 볼 수 있지만 변경할 수는 없습니다. | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [클래식 Storage 계정 기여자](#classic-storage-account-contributor) | 클래식 Storage 계정을 관리할 수 있지만 여기에 액세스할 수는 없습니다. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [클래식 스토리지 계정 키 운영자 서비스 역할](#classic-storage-account-key-operator-service-role) | 클래식 스토리지 계정 키 운영자가 클래식 스토리지 계정에서 키를 나열하고 다시 생성할 수 있습니다. | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Data Box 기여자](#data-box-contributor) | 다른 사람에게 액세스 권한을 부여하는 것을 제외한 모든 항목을 Data Box 서비스에서 관리할 수 있습니다. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Data Box 읽기 권한자](#data-box-reader) | 주문하기나 주문 세부 정보 편집 및 다른 사용자에게 액세스 권한 부여 외에 Data Box 서비스를 관리할 수 있습니다. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Data Lake Analytics 개발자](#data-lake-analytics-developer) | 사용자 자신의 작업을 제출, 모니터링 및 관리할 수 있지만 Data Lake Analytics 계정을 만들거나 삭제할 수는 없습니다. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [읽기 권한자 및 데이터 액세스](#reader-and-data-access) | 모든 것을 볼 수 있지만, 스토리지 계정 또는 포함된 리소스를 삭제하거나 만들 수는 없습니다. 또한 스토리지 계정 키에 액세스하여 스토리지 계정에 포함된 모든 데이터를 읽고 쓸 수 있습니다. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Storage 계정 기여자](#storage-account-contributor) | 저장소 계정 관리를 허용 합니다. 공유 키 권한 부여를 통해 데이터에 액세스 하는 데 사용할 수 있는 계정 키에 대 한 액세스를 제공 합니다. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [스토리지 계정 키 운영자 서비스 역할](#storage-account-key-operator-service-role) | 저장소 계정 액세스 키를 나열 하 고 다시 생성 하도록 허용 합니다. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Storage Blob 데이터 기여자](#storage-blob-data-contributor) | 컨테이너 및 blob Azure Storage 읽고, 쓰고, 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Storage Blob 데이터 소유자](#storage-blob-data-owner) | POSIX 액세스 제어 할당을 포함 하 여 Azure Storage blob 컨테이너 및 데이터에 대 한 모든 권한을 제공 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Storage Blob 데이터 읽기 권한자](#storage-blob-data-reader) | 컨테이너 및 blob Azure Storage 읽고 나열 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [저장소 Blob 위임자](#storage-blob-delegator) | Azure AD 자격 증명으로 서명 된 컨테이너 또는 blob에 대 한 공유 액세스 서명을 만드는 데 사용할 수 있는 사용자 위임 키를 가져옵니다. 자세한 내용은 [사용자 위임 SAS 만들기](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas)를 참조 하세요. | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [저장소 파일 데이터 SMB 공유 참가자](#storage-file-data-smb-share-contributor) | Azure 파일 공유의 파일/디렉터리에 대 한 읽기, 쓰기 및 삭제 액세스를 허용 합니다. 이 역할에는 Windows 파일 서버에 대 한 기본 제공 기능이 없습니다. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [저장소 파일 데이터 SMB 공유 승격 된 참가자](#storage-file-data-smb-share-elevated-contributor) | Azure 파일 공유에서 파일/디렉터리에 대 한 Acl을 읽고, 쓰고, 삭제 하 고, 수정할 수 있습니다. 이 역할은 Windows 파일 서버의 변경 내용에 대 한 파일 공유 ACL에 해당 합니다. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [저장소 파일 데이터 SMB 공유 판독기](#storage-file-data-smb-share-reader) | Azure 파일 공유의 파일/디렉터리에 대 한 읽기 액세스를 허용 합니다. 이 역할은 Windows 파일 서버에서 읽기의 파일 공유 ACL에 해당 합니다. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [저장소 큐 데이터 기여자](#storage-queue-data-contributor) | Azure Storage 큐 및 큐 메시지를 읽고, 쓰고, 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [저장소 큐 데이터 메시지 처리기](#storage-queue-data-message-processor) | Azure Storage 큐에서 메시지를 피킹 (peeking) 하 고 검색 하 고 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [저장소 큐 데이터 메시지 보낸 사람](#storage-queue-data-message-sender) | Azure Storage 큐에 메시지를 추가 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [저장소 큐 데이터 판독기](#storage-queue-data-reader) | Azure Storage 큐 및 큐 메시지를 읽고 나열 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요. | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Web** |  |  |
> | [Azure Maps 데이터 판독기](#azure-maps-data-reader) | Azure maps 계정에서 맵 관련 데이터를 읽을 수 있는 액세스 권한을 부여 합니다. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Search 서비스 기여자](#search-service-contributor) | Search 서비스를 관리할 수 있지만 액세스할 수는 없습니다. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [웹 계획 참가자](#web-plan-contributor) | 웹 사이트의 웹 계획을 관리할 수 있지만 액세스할 수는 없습니다. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [웹 사이트 기여자](#website-contributor) | 웹 사이트(웹 계획은 제외)를 관리할 수 있지만 액세스할 수는 없습니다. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **컨테이너** |  |  |
> | [AcrDelete](#acrdelete) | acr 삭제 | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | acr 이미지 서명자 | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | acr pull | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | acr push | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | acr 격리 데이터 읽기 권한자 | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | acr 격리 데이터 작성자 | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Azure Kubernetes Service 클러스터 관리자 역할](#azure-kubernetes-service-cluster-admin-role) | 클러스터 관리자 자격 증명 작업을 나열합니다. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Azure Kubernetes Service 클러스터 사용자 역할](#azure-kubernetes-service-cluster-user-role) | 클러스터 사용자 자격 증명 작업을 나열합니다. | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **데이터베이스** |  |  |
> | [Cosmos DB 계정 독자 역할](#cosmos-db-account-reader-role) | Azure Cosmos DB 계정 데이터를 읽을 수 있음. Azure Cosmos DB 계정 관리는 [DocumentDB 계정 참가자](#documentdb-account-contributor)를 참조하세요. | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Cosmos DB 연산자](#cosmos-db-operator) | 에서는 Azure Cosmos DB 계정을 관리할 수 있지만 데이터에 액세스할 수는 없습니다. 계정 키 및 연결 문자열에 대 한 액세스를 금지 합니다. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Cosmos DB 데이터베이스 또는 계정의 컨테이너에 대한 복원 요청을 제출할 수 있습니다. | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [DocumentDB 계정 기여자](#documentdb-account-contributor) | Azure Cosmos DB 계정을 관리할 수 있습니다. Azure Cosmos DB는 이전의 DocumentDB입니다. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Redis Cache 참가자](#redis-cache-contributor) | Redis Cache를 관리할 수 있지만 액세스할 수는 없습니다. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [SQL DB 기여자](#sql-db-contributor) | SQL 데이터베이스를 관리할 수 있지만 액세스할 수는 없습니다. 또한 보안 관련 정책이나 부모 SQL 서버를 관리할 수 없습니다. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [SQL Managed Instance 기여자](#sql-managed-instance-contributor) | SQL 관리 되는 인스턴스 및 필요한 네트워크 구성을 관리할 수 있지만 다른 사용자에 게는 액세스할 수 없습니다. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [SQL 보안 관리자](#sql-security-manager) | SQL Server 및 데이터베이스의 보안과 관련된 정책을 관리할 수 있지만 여기에 액세스할 수는 없습니다. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [SQL Server 기여자](#sql-server-contributor) | SQL server 및 데이터베이스를 관리할 수 있지만 보안 관련 정책에는 액세스할 수 없습니다. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **분석** |  |  |
> | [Azure Event Hubs 데이터 소유자](#azure-event-hubs-data-owner) | Azure Event Hubs 리소스에 대 한 모든 액세스를 허용 합니다. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Azure Event Hubs 데이터 수신기](#azure-event-hubs-data-receiver) | Azure Event Hubs 리소스에 대 한 수신 액세스를 허용 합니다. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Azure Event Hubs 데이터 발신자](#azure-event-hubs-data-sender) | Azure Event Hubs 리소스에 대 한 액세스를 보낼 수 있습니다. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Data Factory 참가자](#data-factory-contributor) | 데이터 팩터리를 만들고 관리하며 해당 하위 리소스도 만들고 관리합니다. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [데이터 제거자](#data-purger) | 분석 데이터를 제거할 수 있습니다. | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [HDInsight 클러스터 운영자](#hdinsight-cluster-operator) | HDInsight 클러스터 구성을 읽고 수정할 수 있습니다. | 61ed4efc-fab3-44fd-b111-e24485cc132a |
> | [HDInsight 도메인 서비스 기여자](#hdinsight-domain-services-contributor) | HDInsight Enterprise Security Package에 필요한 도메인 서비스 관련 작업을 읽고, 만들고, 수정하고, 삭제할 수 있음 | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Log Analytics 참가자](#log-analytics-contributor) | Log Analytics 참가자는 모든 모니터링 데이터를 읽고 모니터링 설정을 편집할 수 있습니다. 모니터링 설정 편집에는 VM에 VM 확장 추가, Azure Storage에서 로그 컬렉션을 구성할 수 있는 스토리지 계정 키 읽기, Automation 계정 생성 및 구성, 솔루션 추가 및 모든 Azure 리소스에 대한 Azure 진단을 구성하는 기능도 포함되어 있습니다. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Log Analytics 독자](#log-analytics-reader) | Log Analytics 독자는 모든 Azure 리소스에 대한 Azure 진단의 구성 보기를 비롯하여 모니터링 설정 보기 및 모든 모니터링 데이터를 보고 검색할 수 있습니다. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **블록체인** |  |  |
> | [Blockchain 구성원 노드 액세스 (미리 보기)](#blockchain-member-node-access-preview) | Blockchain 멤버 노드에 대 한 액세스를 허용 합니다. | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **AI + 기계 학습** |  |  |
> | [Cognitive Services 기여자](#cognitive-services-contributor) | Cognitive Services의 키를 만들고, 읽고, 업데이트하고, 삭제 및 관리할 수 있습니다. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [데이터 판독기 Cognitive Services (미리 보기)](#cognitive-services-data-reader-preview) | Cognitive Services 데이터를 읽을 수 있습니다. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Cognitive Services 사용자](#cognitive-services-user) | Cognitive Services의 키를 읽고 나열할 수 있습니다. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **혼합 현실** |  |  |
> | [공간 앵커 계정 기여자](#spatial-anchors-account-contributor) | 계정의 공간 앵커를 관리할 수 있지만 삭제할 수는 없습니다. | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [공간 앵커 계정 소유자](#spatial-anchors-account-owner) | 삭제를 포함 하 여 계정의 공간 앵커를 관리할 수 있습니다. | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [공간 앵커 계정 판독기](#spatial-anchors-account-reader) | 계정의 공간 앵커 속성을 찾고 읽을 수 있습니다. | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **통합** |  |  |
> | [API Management 서비스 참가자](#api-management-service-contributor) | 서비스 및 API를 관리할 수 있습니다. | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [API Management 서비스 운영자 역할](#api-management-service-operator-role) | 서비스를 관리할 수 있지만 API는 관리할 수 없습니다. | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [Azure API Management 읽기 권한자 역할](#api-management-service-reader-role) | 서비스 및 API에 대한 읽기 전용 액세스 | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [앱 구성 데이터 소유자](#app-configuration-data-owner) | 앱 구성 데이터에 대 한 모든 액세스를 허용 합니다. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [앱 구성 데이터 판독기](#app-configuration-data-reader) | 앱 구성 데이터에 대 한 읽기 액세스를 허용 합니다. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Azure Service Bus 데이터 소유자](#azure-service-bus-data-owner) | Azure Service Bus 리소스에 대 한 모든 액세스를 허용 합니다. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Azure Service Bus 데이터 수신기](#azure-service-bus-data-receiver) | Azure Service Bus 리소스에 대 한 수신 액세스를 허용 합니다. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Azure Service Bus 데이터 발신자](#azure-service-bus-data-sender) | Azure Service Bus 리소스에 대 한 액세스를 보낼 수 있습니다. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Azure Stack 등록 소유자](#azure-stack-registration-owner) | Azure Stack 등록을 관리할 수 있습니다. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [EventGrid EventSubscription 기여자](#eventgrid-eventsubscription-contributor) | EventGrid 이벤트 구독 작업을 관리할 수 있습니다. | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [EventGrid EventSubscription 읽기 권한자](#eventgrid-eventsubscription-reader) | EventGrid 이벤트 구독을 읽을 수 있습니다. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [지능형 시스템 계정 기여자](#intelligent-systems-account-contributor) | 인텔리전트 시스템 계정을 관리할 수 있지만 액세스할 수는 없습니다. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [논리 앱 기여자](#logic-app-contributor) | 논리 앱을 관리할 수 있지만 해당 앱에 대 한 액세스 권한은 변경할 수 없습니다. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [논리 앱 운영자](#logic-app-operator) | 논리 앱을 읽고 사용 하지 않도록 설정할 수 있지만 편집 하거나 업데이트할 수는 없습니다. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **ID** |  |  |
> | [관리 ID 기여자](#managed-identity-contributor) | 사용자 할당 ID를 만들고, 읽고, 업데이트하고, 삭제합니다. | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [관리 ID 운영자](#managed-identity-operator) | 사용자 할당 ID를 읽고 할당합니다. | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **보안** |  |  |
> | [Azure 센티널 기여자](#azure-sentinel-contributor) | Azure 센티널 기여자 | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Azure 센티널 판독기](#azure-sentinel-reader) | Azure 센티널 판독기 | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Azure 센티널 응답자](#azure-sentinel-responder) | Azure 센티널 응답자 | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Key Vault 참가자](#key-vault-contributor) | 키 자격 증명 모음을 관리할 수 있지만 액세스할 수는 없습니다. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [보안 관리자](#security-admin) | Security Center에 대 한 사용 권한을 확인 하 고 업데이트 합니다. 보안 읽기 권한자 역할과 동일한 권한 및 보안 정책을 업데이트 하 고 경고 및 권장 사항을 해제할 수도 있습니다. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [보안 평가 참가자](#security-assessment-contributor) | Security Center에 대 한 평가를 푸시할 수 있습니다. | 612c2aa1-cb24-443b-ac28-3ab7272de6f5 |
> | [보안 관리자(레거시)](#security-manager-legacy) | 레거시 역할입니다. 대신 보안 관리자를 사용 하세요. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [보안 Reader](#security-reader) | Security Center에 대 한 사용 권한을 봅니다. 권장 사항, 경고, 보안 정책 및 보안 상태를 볼 수 있지만 변경할 수는 없습니다. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [DevTest Labs 사용자](#devtest-labs-user) | Azure DevTest Labs의 가상 머신을 연결, 시작, 다시 시작 및 종료할 수 있습니다. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [랩 작성자](#lab-creator) | Azure Lab 계정 하에서 관리 랩을 만들고, 관리하고, 삭제할 수 있습니다. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **모니터** |  |  |
> | [Application Insights 구성 요소 기여자](#application-insights-component-contributor) | Application Insights 구성 요소를 관리할 수 있음 | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Application Insights 스냅샷 디버거](#application-insights-snapshot-debugger) | Application Insights 스냅샷 디버거를 사용하여 수집한 디버그 스냅샷을 보고 다운로드할 수 있는 사용자 권한을 제공합니다. 이러한 사용 권한은 [소유자](#owner) 또는 [기여자](#contributor) 역할에 포함되지 않습니다. 사용자에 게 Application Insights 스냅숏 디버거 역할을 부여 하는 경우 사용자에 게 직접 역할을 부여 해야 합니다. 역할은 사용자 지정 역할에 추가 될 때 인식 되지 않습니다. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Monitoring Contributor](#monitoring-contributor) | 모든 모니터링 데이터를 읽고 모니터링 설정을 편집할 수 있음 [Azure Monitor에서의 역할, 권한 및 보안 시작](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)도 참조하세요. | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [모니터링 메트릭 게시자](#monitoring-metrics-publisher) | Azure 리소스에 대한 게시 메트릭 사용 | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [Monitoring Reader](#monitoring-reader) | 모든 모니터링 데이터를 읽을 수 있음(메트릭, 로그 등) [Azure Monitor에서의 역할, 권한 및 보안 시작](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)도 참조하세요. | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [통합 문서 기여자](#workbook-contributor) | 공유 통합 문서를 저장할 수 있습니다. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [통합 문서 판독기](#workbook-reader) | 통합 문서를 읽을 수 있습니다. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **관리 + 거 버 넌 스** |  |  |
> | [Automation 작업 연산자](#automation-job-operator) | Automation Runbook을 사용하여 작업을 만들고 관리합니다. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Automation 운영자](#automation-operator) | 자동화 연산자는 작업을 시작, 중지, 일시 중단 및 다시 시작할 수 있습니다. | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Automation Runbook 연산자](#automation-runbook-operator) | Runbook 작업을 만들려면 Runbook 속성을 읽어보세요. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Azure 연결 된 컴퓨터 온 보 딩](#azure-connected-machine-onboarding) | Azure 연결 된 컴퓨터를 등록할 수 있습니다. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Azure 연결 된 컴퓨터 리소스 관리자](#azure-connected-machine-resource-administrator) | Azure에 연결 된 컴퓨터를 읽고, 쓰고, 삭제 하 고, 다시 등록할 수 있습니다. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [청구 읽기 권한자](#billing-reader) | 결제 데이터에 대해 읽기 권한 허용 | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [청사진 기여자](#blueprint-contributor) | 청사진 정의를 관리할 수 있지만 할당할 수는 없습니다. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [청사진 연산자](#blueprint-operator) | 는 게시 된 기존 청사진을 할당할 수 있지만 새 청사진을 만들 수는 없습니다. 사용자 할당 관리 id를 사용 하 여 할당을 수행 하는 경우에만이 기능이 작동 합니다. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Cost Management 참가자](#cost-management-contributor) | 비용을 확인하고 비용 구성(예: 예산, 내보내기)을 관리할 수 있음 | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Cost Management 읽기 권한자](#cost-management-reader) | 비용 데이터 및 구성(예: 예산, 내보내기)을 확인할 수 있음 | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [관리 되는 응용 프로그램 참가자 역할](#managed-application-contributor-role) | 관리 되는 응용 프로그램 리소스를 만들 수 있습니다. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [관리되는 애플리케이션 운영자 역할](#managed-application-operator-role) | 관리되는 애플리케이션 리소스에서 작업을 읽고 수행할 수 있습니다. | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Managed Applications 읽기 권한자](#managed-applications-reader) | 관리 앱 및 요청 JIT 액세스에서 리소스를 읽을 수 있습니다. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [관리 되는 서비스 등록 할당 삭제 역할](#managed-services-registration-assignment-delete-role) | 관리 되는 서비스 등록 할당 삭제 역할을 통해 테 넌 트 사용자는 테 넌 트에 할당 된 등록 할당을 삭제할 수 있습니다. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [관리 그룹 참가자](#management-group-contributor) | 관리 그룹 참가자 역할 | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [관리 그룹 읽기 권한자](#management-group-reader) | 관리 그룹 읽기 권한자 역할 | ac63b705-f282-497d-ac71-919bf39d939d |
> | [NewRelic APM 계정 기여자](#new-relic-apm-account-contributor) | New Relic Application Performance Management 계정 및 애플리케이션을 관리할 수 있지만 액세스할 수는 없습니다. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [Policy Insights 데이터 기록기 (미리 보기)](#policy-insights-data-writer-preview) | 리소스 정책에 대 한 읽기 액세스를 허용 하 고 리소스 구성 요소 정책 이벤트에 대 한 쓰기 권한을 허용 합니다. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [리소스 정책 참가자](#resource-policy-contributor) | 리소스 정책을 만들거나 수정할 수 있는 권한이 있는 사용자, 지원 티켓을 만들고 리소스/계층 구조를 읽을 수 있습니다. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Site Recovery 기여자](#site-recovery-contributor) | 자격 증명 모음 만들기 및 역할 할당을 제외한 Site Recovery 서비스를 관리할 수 있습니다. | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Site Recovery 운영자](#site-recovery-operator) | 장애 조치(failover) 및 장애 복구(failback)를 수행할 수 있지만 다른 Site Recovery 관리 작업은 수행할 수 없습니다. | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Site Recovery 구독자](#site-recovery-reader) | Site Recovery 상태를 볼 수 있지만 다른 관리 작업은 수행할 수 없습니다. | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [지원 요청 참가자](#support-request-contributor) | 지원 요청을 만들고 관리할 수 있습니다. | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [태그 참여자](#tag-contributor) | 엔터티 자체에 대 한 액세스를 제공 하지 않고 엔터티의 태그를 관리할 수 있습니다. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **기타** |  |  |
> | [BizTalk 참가자](#biztalk-contributor) | BizTalk Services를 관리할 수 있지만 액세스할 수는 없습니다. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Scheduler 작업 컬렉션 참가자](#scheduler-job-collections-contributor) | Scheduler 작업 컬렉션을 관리할 수 있지만 액세스할 수는 없습니다. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |


## <a name="general"></a>일반


### <a name="contributor"></a>참가자

리소스 액세스 권한을 부여 하는 것을 제외 하 고 모든 항목을 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | * | 모든 유형의 리소스 만들기 및 관리 |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | 역할, 정책 할당, 정책 정의 및 정책 집합 정의 삭제 |
> | Microsoft.Authorization/*/Write | 역할, 역할 할당, 정책 할당, 정책 정의 및 정책 집합 정의 만들기 |
> | Microsoft.Authorization/elevateAccess/Action | 테넌트 범위에서 호출자에게 사용자 액세스 관리자 액세스 권한을 부여합니다. |
> | Microsoft.Blueprint/blueprintAssignments/write | 청사진 할당을 만들거나 업데이트 합니다. |
> | Microsoft.Blueprint/blueprintAssignments/delete | 청사진 할당 삭제 |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything except access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="owner"></a>소유자

리소스 액세스를 비롯한 모든 것을 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | * | 모든 유형의 리소스 만들기 및 관리 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything, including access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader"></a>판독기

모든 것을 볼 수 있지만 변경은 할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything, but not make any changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "permissions": [
    {
      "actions": [
        "*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="user-access-administrator"></a>사용자 액세스 관리자

Azure 리소스에 대한 사용자 액세스를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.Authorization/* | 권한 부여 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage user access to Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "User Access Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="compute"></a>컴퓨팅


### <a name="classic-virtual-machine-contributor"></a>클래식 Virtual Machine 참가자

클래식 가상 머신을 관리할 수 있지만 가상 머신이나 연결된 가상 네트워크 또는 스토리지 계정에 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ClassicCompute/domainNames/* | 클래식 컴퓨팅 도메인 이름 만들기 및 관리 |
> | Microsoft.ClassicCompute/virtualMachines/* | 가상 머신 만들기 및 관리 |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | 예약된 IP를 연결합니다. |
> | Microsoft.ClassicNetwork/reservedIps/read | 예약된 IP를 가져옵니다. |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | 가상 네트워크를 조인합니다. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | 가상 네트워크를 가져옵니다. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | 스토리지 계정 디스크를 반환합니다. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | 스토리지 계정 이미지를 반환합니다. (사용되지 않음, 대신 ‘Microsoft.ClassicStorage/storageAccounts/vmImages’ 사용) |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | 스토리지 계정의 액세스 키를 나열합니다. |
> | Microsoft.ClassicStorage/storageAccounts/read | 지정된 계정의 스토리지 계정을 반환합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "name": "d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/domainNames/*",
        "Microsoft.ClassicCompute/virtualMachines/*",
        "Microsoft.ClassicNetwork/networkSecurityGroups/join/action",
        "Microsoft.ClassicNetwork/reservedIps/link/action",
        "Microsoft.ClassicNetwork/reservedIps/read",
        "Microsoft.ClassicNetwork/virtualNetworks/join/action",
        "Microsoft.ClassicNetwork/virtualNetworks/read",
        "Microsoft.ClassicStorage/storageAccounts/disks/read",
        "Microsoft.ClassicStorage/storageAccounts/images/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-administrator-login"></a>가상 머신 관리자 로그인

포털에서 Virtual Machines를 보고 관리자 권한으로 로그인합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Network/publicIPAddresses/read | 공용 IP 주소 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.Network/loadBalancers/read | 부하 분산 장치 정의를 가져옵니다. |
> | Microsoft.Network/networkInterfaces/read | 네트워크 인터페이스 정의를 가져옵니다.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | 가상 머신에 일반 사용자로 로그인합니다. |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | 가상 머신에 Windows 관리자 또는 Linux 루트 사용자 권한으로 로그인합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as administrator",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "name": "1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.Compute/virtualMachines/loginAsAdmin/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Administrator Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-contributor"></a>가상 머신 참가자

가상 머신을 관리할 수 있지만 가상머신이나 연결된 가상 네트워크 또는 스토리지 계정에 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Compute/availabilitySets/* | 컴퓨팅 가용성 집합 만들기 및 관리 |
> | Microsoft.Compute/locations/* | 컴퓨팅 위치 만들기 및 관리 |
> | Microsoft.Compute/virtualMachines/* | 가상 머신 만들기 및 관리 |
> | Microsoft.Compute/virtualMachineScaleSets/* | 가상 머신 크기 집합 만들기 및 관리 |
> | Microsoft.Compute/disks/write | 새 디스크를 만들거나 기존 디스크를 업데이트합니다. |
> | Microsoft.Compute/disks/read | 디스크의 속성을 가져옵니다. |
> | Microsoft.Compute/disks/delete | 디스크를 삭제합니다. |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | 응용 프로그램 게이트웨이 백 엔드 주소 풀을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | 부하 분산 장치 백 엔드 주소 풀을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | 부하 분산 장치 인바운드 NAT 풀을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | 부하 분산 장치 인바운드 nat 규칙을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/probes/join/action | 부하 분산 장치 프로브 사용을 허용합니다. 예를 들어 이 권한이 있으면 VM 확장 집합의 healthProbe 속성이 프로브를 참조할 수 있습니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/read | 부하 분산 장치 정의를 가져옵니다. |
> | Microsoft.Network/locations/* | 네트워크 위치 만들기 및 관리 |
> | Microsoft.Network/networkInterfaces/* | 네트워크 인터페이스 만들기 및 관리 |
> | Microsoft.Network/networkSecurityGroups/join/action | 네트워크 보안 그룹을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/networkSecurityGroups/read | 네트워크 보안 그룹 정의를 가져옵니다. |
> | Microsoft.Network/publicIPAddresses/join/action | 공용 ip 주소를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/publicIPAddresses/read | 공용 IP 주소 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | 가상 네트워크를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | 백업 보호 의도 만들기 |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | 보호된 항목의 개체 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | 백업 보호 항목을 만듭니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | 모든 보호 정책을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | 보호 정책을 만듭니다. |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/write | 자격 증명 모음 만들기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 만듭니다. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/locations/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/virtualMachineScaleSets/*",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/delete",
        "Microsoft.DevTestLab/schedules/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/loadBalancers/probes/join/action",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/locations/*",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Network/networkSecurityGroups/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/write",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.SqlVirtualMachine/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-user-login"></a>가상 머신 사용자 로그인

포털에서 Virtual Machines를 보고 일반 사용자 권한으로 로그인합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Network/publicIPAddresses/read | 공용 IP 주소 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.Network/loadBalancers/read | 부하 분산 장치 정의를 가져옵니다. |
> | Microsoft.Network/networkInterfaces/read | 네트워크 인터페이스 정의를 가져옵니다.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | 가상 머신에 일반 사용자로 로그인합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as a regular user.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb879df8-f326-4884-b1cf-06f3ad86be52",
  "name": "fb879df8-f326-4884-b1cf-06f3ad86be52",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine User Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="networking"></a>네트워킹


### <a name="cdn-endpoint-contributor"></a>CD 엔드포인트 참가자

CDN 끝점을 관리할 수 있지만 다른 사용자에 게 액세스 권한을 부여할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "name": "426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-endpoint-reader"></a>CDN 엔드포인트 독자

CDN 끝점을 볼 수 있지만 변경할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "name": "871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-contributor"></a>CDN 프로필 참가자

CDN 프로필과 해당 끝점을 관리할 수 있지만 다른 사용자에 게 액세스 권한을 부여할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN profiles and their endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "name": "ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-reader"></a>CDN 프로필 독자

CDN 프로필과 해당 끝점을 볼 수 있지만 변경할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN profiles and their endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f96442b-4075-438f-813d-ad51ab4019af",
  "name": "8f96442b-4075-438f-813d-ad51ab4019af",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-network-contributor"></a>클래식 네트워크 참가자

기본 네트워크를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ClassicNetwork/* | 클래식 네트워크 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "name": "b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicNetwork/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="dns-zone-contributor"></a>DNS 영역 참가자

Azure DNS의 DNS 영역과 레코드 집합을 관리할 수 있지만 액세스할 수 있는 사람을 제어할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/dnsZones/* | DNS 영역 및 레코드 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DNS zones and record sets in Azure DNS, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/befefa01-2a29-4197-83a8-272ff33ce314",
  "name": "befefa01-2a29-4197-83a8-272ff33ce314",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/dnsZones/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="network-contributor"></a>네트워크 참가자

네트워크를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/* | 네트워크 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7",
  "name": "4d97b98b-1d4f-4787-a291-c67834d212e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="traffic-manager-contributor"></a>Traffic Manager 참가자

Traffic Manager 프로필을 관리할 수 있지만 액세스할 수 있는 사람을 제어할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Traffic Manager profiles, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "name": "a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/trafficManagerProfiles/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Traffic Manager Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="storage"></a>스토리지


### <a name="avere-contributor"></a>Avere 기여자

Avere vFXT 클러스터를 만들고 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft. Compute/*/cread |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft. Compute/disks/* |  |
> | Microsoft. 네트워크/*/읽기 |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/read | 가상 네트워크 서브넷 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | 가상 네트워크를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | 스토리지 계정 또는 SQL 데이터베이스 같은 리소스를 서브넷에 조인합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/networkSecurityGroups/join/action | 네트워크 보안 그룹을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft 저장소/*/읽기 |  |
> | Microsoft.Storage/storageAccounts/* | 스토리지 계정 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft .Resources/subscription/resourceGroups/resources/read | 리소스 그룹에 대한 리소스를 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Blob 삭제 결과를 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Blob 또는 Blob 목록을 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Blob 쓰기 결과 반환 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create and manage an Avere vFXT cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "name": "4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/disks/*",
        "Microsoft.Network/*/read",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/*/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="avere-operator"></a>Avere 연산자

Avere vFXT 클러스터에서 클러스터를 관리 하는 데 사용 됩니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Compute/virtualMachines/read | 가상 머신의 속성을 가져옵니다. |
> | Microsoft.Network/networkInterfaces/read | 네트워크 인터페이스 정의를 가져옵니다.  |
> | Microsoft.Network/networkInterfaces/write | 네트워크 인터페이스를 만들거나 기존 네트워크 인터페이스를 업데이트합니다.  |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/read | 가상 네트워크 서브넷 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | 가상 네트워크를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/networkSecurityGroups/join/action | 네트워크 보안 그룹을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | 컨테이너 삭제 결과를 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | 컨테이너 목록을 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Blob 컨테이너 넣기의 결과를 반환합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Blob 삭제 결과를 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Blob 또는 Blob 목록을 반환합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Blob 쓰기 결과 반환 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the Avere vFXT cluster to manage the cluster",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "name": "c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-contributor"></a>Backup 참가자

백업 서비스를 관리할 수 있지만 자격 증명 모음을 만들고 다른 사용자에 게 액세스 권한을 부여할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | 백업 관리에 대한 작업의 결과 관리 |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Recovery Services 자격 증명 모음의 백업 패브릭 내에서 백업 컨테이너 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | 컨테이너 목록을 새로 고칩니다. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | 백업 작업 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | 작업을 내보냅니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | 백업 관리 작업의 결과 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | 백업 정책 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | 백업할 수 있는 항목 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | 백업한 항목 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | 백업 항목을 보유하는 컨테이너 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Recovery Services의 보호된 항목 및 보호된 서버에 대한 요약을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Recovery Services 자격 증명 모음의 백업과 관련된 인증서 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | 자격 증명 모음과 관련된 확장 정보 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Recovery Services 자격 증명 모음에 대한 경고를 받습니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | 등록된 ID 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/usages/* | Recovery Services 자격 증명 모음 만들기 및 사용 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | 보호된 항목에 대한 작업의 유효성을 검사합니다. |
> | Microsoft.RecoveryServices/Vaults/write | 자격 증명 모음 만들기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 만듭니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Recovery Services 자격 증명 모음의 Backup 작업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | 자격 증명 모음에 등록된 모든 백업 관리 서버를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | 보호 가능한 컨테이너를 모두 가져옵니다. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Recovery Services 자격 증명 모음의 백업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | 기능의 유효성을 검사합니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | 경고를 해결합니다. |
> | Microsoft.RecoveryServices/operations/read | 작업에서 리소스 공급자에 대한 작업 목록을 반환합니다. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | 지정된 작업의 작업 상태를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | 모든 백업 보호 의도를 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup service,but can't create vaults and give access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e467623-bb1f-42f4-a55d-6e525e11384b",
  "name": "5e467623-bb1f-42f4-a55d-6e525e11384b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupSecurityPIN/*",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/*",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/Vaults/usages/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-operator"></a>Backup 운영자

백업 제거를 제외한 백업 서비스를 관리하고 자격 증명 모음 만들고 다른 사람에게 액세스 권한을 부여할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | 작업의 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | 보호 컨테이너에 대해 수행된 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | 보호 항목 Backup을 수행합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | 보호 항목에 대해 수행된 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | 보호 항목에 대해 수행된 작업의 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | 보호된 항목의 개체 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | 보호된 항목에 대한 빠른 항목 복구를 프로비전합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | 보호 항목의 복구 지점을 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | 보호 항목의 복구 지점을 복원합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | 보호된 항목에 대한 빠른 항목 복구를 취소합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | 백업 보호 항목을 만듭니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | 등록된 모든 컨테이너를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | 컨테이너 목록을 새로 고칩니다. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | 백업 작업 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | 작업을 내보냅니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | 백업 관리 작업의 결과 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | 정책 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | 모든 보호 정책을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | 백업할 수 있는 항목 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | 모든 보호 항목 목록을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | 구독에 속하는 컨테이너를 모두 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Recovery Services의 보호된 항목 및 보호된 서버에 대한 요약을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | 리소스 인증서 업데이트 작업은 리소스/저장소 자격 증명 인증서를 업데이트합니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | 확장 정보 가져오기 작업에서는 ‘자격 증명 모음’ 형식의 Azure 리소스를 나타내는 개체의 확장 정보를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | 확장 정보 가져오기 작업에서는 ‘자격 증명 모음’ 형식의 Azure 리소스를 나타내는 개체의 확장 정보를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Recovery Services 자격 증명 모음에 대한 경고를 받습니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | 작업 결과 가져오기 작업을 사용하여 비동기적으로 제출된 작업에 대한 작업 상태와 결과를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | 컨테이너 가져오기 작업을 사용하여 리소스에 대해 등록된 컨테이너를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | 서비스 컨테이너 등록 작업을 사용하여 복구 서비스와 함께 컨테이너를 등록할 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | 보호된 항목에 대한 작업의 유효성을 검사합니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Recovery Services 자격 증명 모음의 Backup 작업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | 정책 작업의 상태를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | 등록된 컨테이너를 만듭니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | 컨테이너 내의 워크로드를 조회합니다. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | 자격 증명 모음에 등록된 모든 백업 관리 서버를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | 백업 보호 의도 만들기 |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | 백업 보호 의도를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | 보호 가능한 컨테이너를 모두 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | 컨테이너의 모든 항목을 가져옵니다. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Recovery Services 자격 증명 모음의 백업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | 기능의 유효성을 검사합니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | 경고를 해결합니다. |
> | Microsoft.RecoveryServices/operations/read | 작업에서 리소스 공급자에 대한 작업 목록을 반환합니다. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | 지정된 작업의 작업 상태를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | 모든 백업 보호 의도를 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup services, except removal of backup, vault creation and giving access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00c29273-979b-4161-815c-10b084fb9324",
  "name": "00c29273-979b-4161-815c-10b084fb9324",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/write",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/write",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-reader"></a>Backup 읽기 권한자

백업 서비스를 볼 수 있지만 변경할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | 작업의 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | 보호 컨테이너에 대해 수행된 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | 보호 항목에 대해 수행된 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | 보호 항목에 대해 수행된 작업의 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | 보호된 항목의 개체 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | 보호 항목의 복구 지점을 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | 등록된 모든 컨테이너를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | 작업의 작업 결과를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | 모든 작업 개체를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | 작업을 내보냅니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Recovery Services 자격 증명 모음의 Backup 작업 결과를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | 정책 작업의 결과를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | 모든 보호 정책을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | 모든 보호 항목 목록을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | 구독에 속하는 컨테이너를 모두 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Recovery Services의 보호된 항목 및 보호된 서버에 대한 요약을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | 확장 정보 가져오기 작업에서는 ‘자격 증명 모음’ 형식의 Azure 리소스를 나타내는 개체의 확장 정보를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Recovery Services 자격 증명 모음에 대한 경고를 받습니다. |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | 작업 결과 가져오기 작업을 사용하여 비동기적으로 제출된 작업에 대한 작업 상태와 결과를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | 컨테이너 가져오기 작업을 사용하여 리소스에 대해 등록된 컨테이너를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Recovery Services 자격 증명 모음에 대한 스토리지 구성을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Recovery Services 자격 증명 모음에 구성을 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Recovery Services 자격 증명 모음의 Backup 작업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | 정책 작업의 상태를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | 자격 증명 모음에 등록된 모든 백업 관리 서버를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | 백업 보호 의도를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | 컨테이너의 모든 항목을 가져옵니다. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Recovery Services 자격 증명 모음의 백업 상태를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | 경고를 해결합니다. |
> | Microsoft.RecoveryServices/operations/read | 작업에서 리소스 공급자에 대한 작업 목록을 반환합니다. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | 지정된 작업의 작업 상태를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | 모든 백업 보호 의도를 나열합니다. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | 기능의 유효성을 검사합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view backup services, but can't make changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "name": "a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/read",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-contributor"></a>클래식 Storage 계정 참가자

클래식 Storage 계정을 관리할 수 있지만 여기에 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ClassicStorage/storageAccounts/* | 스토리지 계정 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic storage accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "name": "86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-key-operator-service-role"></a>클래식 스토리지 계정 키 운영자 서비스 역할

클래식 스토리지 계정 키 운영자가 클래식 스토리지 계정에서 키를 나열하고 다시 생성할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | 스토리지 계정의 액세스 키를 나열합니다. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | 스토리지 계정에 대한 기존 액세스 키를 다시 생성합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Classic Storage Account Key Operators are allowed to list and regenerate keys on Classic Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "name": "985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ClassicStorage/storageAccounts/listkeys/action",
        "Microsoft.ClassicStorage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-contributor"></a>Data Box 기여자

다른 사람에게 액세스 권한을 부여하는 것을 제외한 모든 항목을 Data Box 서비스에서 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything under Data Box Service except giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/add466c9-e687-43fc-8d98-dfcf8d720be5",
  "name": "add466c9-e687-43fc-8d98-dfcf8d720be5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Databox/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-reader"></a>Data Box 읽기 권한자

주문하기나 주문 세부 정보 편집 및 다른 사용자에게 액세스 권한 부여 외에 Data Box 서비스를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | 주문과 관련된 암호화되지 않은 자격 증명을 나열합니다. |
> | Microsoft.Databox/locations/availableSkus/action | 이 메서드는 사용할 수 있는 SKU 목록을 반환합니다. |
> | Databox/위치/validateInputs/작업 | 이 메서드는 모든 유형의 유효성 검사를 수행 합니다. |
> | Databox/위치/지역 구성/작업 | 이 메서드는 영역에 대 한 구성을 반환 합니다. |
> | Databox/위치/s i d/작업 | 배송 주소의 유효성을 검사하고, 있는 경우, 대체 주소를 제공합니다. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Data Box Service except creating order or editing order details and giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "name": "028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Databox/*/read",
        "Microsoft.Databox/jobs/listsecrets/action",
        "Microsoft.Databox/jobs/listcredentials/action",
        "Microsoft.Databox/locations/availableSkus/action",
        "Microsoft.Databox/locations/validateInputs/action",
        "Microsoft.Databox/locations/regionConfiguration/action",
        "Microsoft.Databox/locations/validateAddress/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-lake-analytics-developer"></a>Data Lake Analytics 개발자

사용자 자신의 작업을 제출, 모니터링 및 관리할 수 있지만 Data Lake Analytics 계정을 만들거나 삭제할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | DataLakeAnalytics 계정을 삭제합니다. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | 다른 사용자가 제출한 작업을 취소하는 권한을 부여합니다. |
> | Microsoft.DataLakeAnalytics/accounts/Write | DataLakeAnalytics 계정을 만들거나 업데이트합니다. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | DataLakeAnalytics 계정과 연결된 DataLakeStore 계정을 만들거나 업데이트합니다. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | DataLakeAnalytics 계정에서 DataLakeStore 계정을 연결 해제합니다. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | DataLakeAnalytics 계정과 연결된 Storage 계정을 만들거나 업데이트합니다. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | DataLakeAnalytics 계정에서 Storage 계정을 연결 해제합니다. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | 방화벽 규칙을 만들거나 업데이트합니다. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | 방화벽 규칙을 삭제합니다. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | 컴퓨팅 정책을 만들거나 업데이트합니다. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | 컴퓨팅 정책을 삭제합니다. |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you submit, monitor, and manage your own jobs but not create or delete Data Lake Analytics accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/47b7735b-770e-4598-a7da-8b91488b4c88",
  "name": "47b7735b-770e-4598-a7da-8b91488b4c88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BigAnalytics/accounts/*",
        "Microsoft.DataLakeAnalytics/accounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.BigAnalytics/accounts/Delete",
        "Microsoft.BigAnalytics/accounts/TakeOwnership/action",
        "Microsoft.BigAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action",
        "Microsoft.DataLakeAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Write",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Write",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Lake Analytics Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader-and-data-access"></a>읽기 권한자 및 데이터 액세스

모든 것을 볼 수 있지만, 스토리지 계정 또는 포함된 리소스를 삭제하거나 만들 수는 없습니다. 또한 스토리지 계정 키에 액세스하여 스토리지 계정에 포함된 모든 데이터를 읽고 쓸 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | Microsoft. Storage/storageAccounts/ListAccountSas/action | 지정된 스토리지 계정에 대한 계정 SAS 토큰을 반환합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything but will not let you delete or create a storage account or contained resource. It will also allow read/write access to all data contained in a storage account via access to storage account keys.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c12c1c16-33a1-487b-954d-41c89c60f349",
  "name": "c12c1c16-33a1-487b-954d-41c89c60f349",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/ListAccountSas/action",
        "Microsoft.Storage/storageAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader and Data Access",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-contributor"></a>Storage 계정 참가자

저장소 계정 관리를 허용 합니다. 공유 키 권한 부여를 통해 데이터에 액세스 하는 데 사용할 수 있는 계정 키에 대 한 액세스를 제공 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server에 대한 진단 설정 생성, 업데이트 및 읽기 |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | 스토리지 계정 또는 SQL 데이터베이스 같은 리소스를 서브넷에 조인합니다. 했어야 하지 않습니다. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/* | 스토리지 계정 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage storage accounts, including accessing storage account keys which provide full access to storage account data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "name": "17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-key-operator-service-role"></a>스토리지 계정 키 운영자 서비스 역할

저장소 계정 액세스 키를 나열 하 고 다시 생성 하도록 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | 지정된 스토리지 계정에 대한 액세스 키를 다시 생성합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Storage Account Key Operators are allowed to list and regenerate keys on Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/81a9662b-bebf-436f-a333-f67b29880f12",
  "name": "81a9662b-bebf-436f-a333-f67b29880f12",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-contributor"></a>Storage Blob 데이터 기여자

컨테이너 및 blob Azure Storage 읽고, 쓰고, 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | 컨테이너를 삭제 합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | 컨테이너 또는 컨테이너 목록을 반환 합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | 컨테이너의 메타 데이터 또는 속성을 수정 합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob service에 대 한 사용자 위임 키를 반환 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Blob을 삭제합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Blob 또는 blob 목록을 반환 합니다. |
> | Microsoft. Storage/storageAccounts/blobServices/컨테이너/b l o b/작업 | Blob을 한 경로에서 다른 경로로 이동 합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Blob에 씁니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "name": "ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-owner"></a>Storage Blob 데이터 소유자

POSIX 액세스 제어 할당을 포함 하 여 Azure Storage blob 컨테이너 및 데이터에 대 한 모든 권한을 제공 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | 컨테이너에 대 한 모든 권한 |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob service에 대 한 사용자 위임 키를 반환 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Blob에 대 한 모든 권한 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "name": "b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/*",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-reader"></a>Storage Blob 데이터 읽기 권한자

컨테이너 및 blob Azure Storage 읽고 나열 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | 컨테이너 또는 컨테이너 목록을 반환 합니다. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob service에 대 한 사용자 위임 키를 반환 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Blob 또는 blob 목록을 반환 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-delegator"></a>저장소 Blob 위임자

Azure AD 자격 증명으로 서명 된 컨테이너 또는 blob에 대 한 공유 액세스 서명을 만드는 데 사용할 수 있는 사용자 위임 키를 가져옵니다. 자세한 내용은 [사용자 위임 SAS 만들기](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas)를 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob service에 대 한 사용자 위임 키를 반환 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for generation of a user delegation key which can be used to sign SAS tokens",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "name": "db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Delegator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-contributor"></a>저장소 파일 데이터 SMB 공유 참가자

Azure 파일 공유의 파일/디렉터리에 대 한 읽기, 쓰기 및 삭제 액세스를 허용 합니다. 이 역할에는 Windows 파일 서버에 대 한 기본 제공 기능이 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/read | 파일/폴더 또는 파일/폴더 목록을 반환 합니다. |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/write | 파일을 쓰거나 폴더를 만든 결과를 반환 합니다. |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/delete | 파일/폴더를 삭제 한 결과를 반환 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "name": "0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-elevated-contributor"></a>저장소 파일 데이터 SMB 공유 승격 된 참가자

Azure 파일 공유에서 파일/디렉터리에 대 한 Acl을 읽고, 쓰고, 삭제 하 고, 수정할 수 있습니다. 이 역할은 Windows 파일 서버의 변경 내용에 대 한 파일 공유 ACL에 해당 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/read | 파일/폴더 또는 파일/폴더 목록을 반환 합니다. |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/write | 파일을 쓰거나 폴더를 만든 결과를 반환 합니다. |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/delete | 파일/폴더를 삭제 한 결과를 반환 합니다. |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/modifypermissions/action | 파일/폴더에 대 한 수정 권한이 있는 결과를 반환 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7264617-510b-434b-a828-9731dc254ea7",
  "name": "a7264617-510b-434b-a828-9731dc254ea7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Elevated Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-reader"></a>저장소 파일 데이터 SMB 공유 판독기

Azure 파일 공유의 파일/디렉터리에 대 한 읽기 액세스를 허용 합니다. 이 역할은 Windows 파일 서버에서 읽기의 파일 공유 ACL에 해당 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft. Storage/storageAccounts/fileServices/파일 공유/files/read | 파일/폴더 또는 파일/폴더 목록을 반환 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure File Share over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aba4ae5f-2193-4029-9191-0cb91df5e314",
  "name": "aba4ae5f-2193-4029-9191-0cb91df5e314",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-contributor"></a>저장소 큐 데이터 기여자

Azure Storage 큐 및 큐 메시지를 읽고, 쓰고, 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | 큐를 삭제 합니다. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | 큐 또는 큐 목록을 반환 합니다. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | 큐 메타 데이터 또는 속성을 수정 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | 큐에서 하나 이상의 메시지를 삭제 합니다. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | 큐에서 하나 이상의 메시지를 피킹 (peeking) 하거나 검색 합니다. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | 큐에 메시지를 추가 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "name": "974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-processor"></a>저장소 큐 데이터 메시지 처리기

Azure Storage 큐에서 메시지를 피킹 (peeking) 하 고 검색 하 고 삭제 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | 메시지를 미리 봅니다. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | 메시지를 검색 하 고 삭제 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for peek, receive, and delete access to Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "name": "8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Processor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-sender"></a>저장소 큐 데이터 메시지 보낸 사람

Azure Storage 큐에 메시지를 추가 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | 큐에 메시지를 추가 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for sending of Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "name": "c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-reader"></a>저장소 큐 데이터 판독기

Azure Storage 큐 및 큐 메시지를 읽고 나열 합니다. 지정 된 데이터 작업에 필요한 작업에 대 한 자세한 내용은 [blob 호출 및 큐 데이터 작업에 대 한 사용 권한](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)을 참조 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | 큐 또는 큐 목록을 반환합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | 큐에서 하나 이상의 메시지를 피킹 (peeking) 하거나 검색 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/19e7f393-937e-4f77-808e-94535e297925",
  "name": "19e7f393-937e-4f77-808e-94535e297925",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="web"></a>웹


### <a name="azure-maps-data-reader"></a>Azure Maps 데이터 판독기

Azure maps 계정에서 맵 관련 데이터를 읽을 수 있는 액세스 권한을 부여 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft Maps/계정/*/읽기 |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "name": "423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Search 서비스 참가자

Search 서비스를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Search/searchServices/* | 검색 서비스 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Search services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "name": "7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Search/searchServices/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Search Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="web-plan-contributor"></a>웹 계획 참가자

웹 사이트의 웹 계획을 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Web/serverFarms/* | 서버 팜 만들기 및 관리 |
> | Microsoft 웹/hostingEnvironments/조인/작업 | App Service Environment를 조인 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the web plans for websites, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "name": "2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/serverFarms/*",
        "Microsoft.Web/hostingEnvironments/Join/Action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Web Plan Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="website-contributor"></a>웹 사이트 참가자

웹 사이트(웹 계획은 제외)를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/components/* | Insights 구성 요소 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Web/certificates/* | 웹 사이트 인증서 만들기 및 관리 |
> | Microsoft.Web/listSitesAssignedToHostName/read | 호스트 이름에 할당된 사이트의 이름을 가져옵니다. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | App Service 계획의 속성을 가져옵니다. |
> | Microsoft.Web/sites/* | 웹 사이트 만들기 및 관리(사이트 만들기도 관련 App Service 계획에 대한 쓰기 권한이 필요) |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage websites (not web plans), but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/de139f84-1756-47ae-9be6-808fbbe84772",
  "name": "de139f84-1756-47ae-9be6-808fbbe84772",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/certificates/*",
        "Microsoft.Web/listSitesAssignedToHostName/read",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Website Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="containers"></a>컨테이너


### <a name="acrdelete"></a>AcrDelete

acr 삭제

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.containerregistry/레지스트리/아티팩트/삭제 | 컨테이너 레지스트리에서 아티팩트를 삭제 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr delete",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/artifacts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrDelete",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrimagesigner"></a>AcrImageSigner

acr 이미지 서명자

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | 컨테이너 레지스트리에 대한 콘텐츠 신뢰 메타데이터를 푸시/풀합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr image signer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6cef56e8-d556-48e5-a04f-b8e64114680f",
  "name": "6cef56e8-d556-48e5-a04f-b8e64114680f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/sign/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrImageSigner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpull"></a>AcrPull

acr pull

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | 컨테이너 레지스트리에서 이미지를 끌어오거나 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr pull",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "name": "7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPull",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpush"></a>AcrPush

acr push

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | 컨테이너 레지스트리에서 이미지를 끌어오거나 가져옵니다. |
> | Microsoft.ContainerRegistry/registries/push/write | 컨테이너 레지스트리에 이미지를 푸시하거나 작성합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr push",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8311e382-0749-4cb8-b61a-304f252e45ec",
  "name": "8311e382-0749-4cb8-b61a-304f252e45ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read",
        "Microsoft.ContainerRegistry/registries/push/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPush",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinereader"></a>AcrQuarantineReader

acr 격리 데이터 읽기 권한자

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.containerregistry/레지스트리/격리/읽기 | 컨테이너 레지스트리에서 격리된 이미지를 끌어오거나 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cdda3590-29a3-44f6-95f2-9f980659eb04",
  "name": "cdda3590-29a3-44f6-95f2-9f980659eb04",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineReader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinewriter"></a>AcrQuarantineWriter

acr 격리 데이터 작성자

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.containerregistry/레지스트리/격리/읽기 | 컨테이너 레지스트리에서 격리된 이미지를 끌어오거나 가져옵니다. |
> | Microsoft.containerregistry/레지스트리/격리/쓰기 | 격리된 이미지의 격리 상태를 작성/수정합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data writer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "name": "c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read",
        "Microsoft.ContainerRegistry/registries/quarantine/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineWriter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Azure Kubernetes Service 클러스터 관리자 역할

클러스터 관리자 자격 증명 작업을 나열합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | 관리되는 클러스터의 clusterAdmin 자격 증명을 나열합니다. |
> | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | 자격 증명 나열을 사용하여 역할 이름별로 관리되는 클러스터 액세스 프로필을 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster admin credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "name": "0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action",
        "Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster Admin Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes Service 클러스터 사용자 역할

클러스터 사용자 자격 증명 작업을 나열합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | 관리되는 클러스터의 clusterUser 자격 증명을 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "name": "4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="databases"></a>데이터베이스


### <a name="cosmos-db-account-reader-role"></a>Cosmos DB 계정 독자 역할

Azure Cosmos DB 계정 데이터를 읽을 수 있음. Azure Cosmos DB 계정 관리는 [DocumentDB 계정 참가자](#documentdb-account-contributor)를 참조하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.DocumentDB/*/read | 컬렉션 읽기 |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | 데이터베이스 계정 읽기 전용 키를 읽습니다. |
> | Microsoft.Insights/MetricDefinitions/read | 메트릭 정의 읽기 |
> | Microsoft.Insights/Metrics/read | 메트릭 읽기 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read Azure Cosmos DB Accounts data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "name": "fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDB/*/read",
        "Microsoft.DocumentDB/databaseAccounts/readonlykeys/action",
        "Microsoft.Insights/MetricDefinitions/read",
        "Microsoft.Insights/Metrics/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Account Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-operator"></a>Cosmos DB 연산자

에서는 Azure Cosmos DB 계정을 관리할 수 있지만 데이터에 액세스할 수는 없습니다. 계정 키 및 연결 문자열에 대 한 액세스를 금지 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.DocumentDb/databaseAccounts/* |  |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | 스토리지 계정 또는 SQL 데이터베이스 같은 리소스를 서브넷에 조인합니다. 했어야 하지 않습니다. |
> | **NotActions** |  |
> | Microsoft DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Cosmos DB accounts, but not access data in them. Prevents access to account keys and connection strings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/230815da-be43-4aae-9cb4-875f7bd000aa",
  "name": "230815da-be43-4aae-9cb4-875f7bd000aa",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [
        "Microsoft.DocumentDB/databaseAccounts/readonlyKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/regenerateKey/*",
        "Microsoft.DocumentDB/databaseAccounts/listKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosbackupoperator"></a>CosmosBackupOperator

Cosmos DB 데이터베이스 또는 계정의 컨테이너에 대한 복원 요청을 제출할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | 백업 구성하는 요청 제출 |
> | Microsoft.DocumentDB/databaseAccounts/restore/action | 복원 요청 제출 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can submit restore request for a Cosmos DB database or a container for an account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "name": "db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/databaseAccounts/backup/action",
        "Microsoft.DocumentDB/databaseAccounts/restore/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosBackupOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="documentdb-account-contributor"></a>DocumentDB 계정 참가자

Azure Cosmos DB 계정을 관리할 수 있습니다. Azure Cosmos DB는 이전의 DocumentDB입니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB 계정 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | 스토리지 계정 또는 SQL 데이터베이스 같은 리소스를 서브넷에 조인합니다. 했어야 하지 않습니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DocumentDB accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5bd9cd88-fe45-4216-938b-f97437e15450",
  "name": "5bd9cd88-fe45-4216-938b-f97437e15450",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DocumentDB Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="redis-cache-contributor"></a>Redis Cache 참가자

Redis Cache를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Cache/redis/* | Redis 캐시 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Redis caches, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e0f68234-74aa-48ed-b826-c38b57376e17",
  "name": "e0f68234-74aa-48ed-b826-c38b57376e17",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cache/redis/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Redis Cache Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-db-contributor"></a>SQL DB 참가자

SQL 데이터베이스를 관리할 수 있지만 액세스할 수는 없습니다. 또한 보안 관련 정책이나 부모 SQL 서버를 관리할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | SQL 데이터베이스 만들기 및 관리 |
> | Microsoft.Sql/servers/read | 서버 목록을 가져오거나 지정된 서버에 대한 속성을 가져옵니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Insights/metrics/read | 메트릭 읽기 |
> | Microsoft.Insights/metricDefinitions/read | 메트릭 정의 읽기 |
> | **NotActions** |  |
> | Microsoft .Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/스키마/tables/columns/sensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft .Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft .Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | 감사 정책 편집 |
> | Microsoft.Sql/servers/databases/auditingSettings/* | 감사 설정 편집 |
> | Microsoft.Sql/servers/databases/auditRecords/read | 데이터베이스 blob 감사 레코드를 검색합니다. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | 연결 정책 편집 |
> | Microsoft .Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | 데이터 마스킹 정책 편집 |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft .Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | 보안 경고 정책 편집 |
> | Microsoft.Sql/servers/databases/securityMetrics/* | 보안 메트릭 편집 |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL databases, but not access to them. Also, you can't manage their security-related policies or their parent SQL servers.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "name": "9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/databases/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL DB Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-managed-instance-contributor"></a>SQL Managed Instance 기여자

SQL 관리 되는 인스턴스 및 필요한 네트워크 구성을 관리할 수 있지만 다른 사용자에 게는 액세스할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft. Network/networkSecurityGroups/* |  |
> | Microsoft. Network/routeTables/* |  |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft .Sql/managedInstances/* |  |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft. Network/virtualNetworks/서브넷/* |  |
> | Microsoft. Network/virtualNetworks/* |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/metrics/read | 메트릭 읽기 |
> | Microsoft.Insights/metricDefinitions/read | 메트릭 정의 읽기 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL Managed Instances and required network configuration, but can't give access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "name": "4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Network/networkSecurityGroups/*",
        "Microsoft.Network/routeTables/*",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/managedInstances/*",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/*",
        "Microsoft.Network/virtualNetworks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Managed Instance Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-security-manager"></a>SQL 보안 관리자

SQL Server 및 데이터베이스의 보안과 관련된 정책을 관리할 수 있지만 여기에 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | 스토리지 계정 또는 SQL 데이터베이스 같은 리소스를 서브넷에 조인합니다. 했어야 하지 않습니다. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft .Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/스키마/tables/columns/sensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft .Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft .Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft .Sql/managedInstances/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL 서버 감사 정책 만들기 및 관리 |
> | Microsoft.Sql/servers/auditingSettings/* | SQL 서버 감사 설정 만들기 및 관리 |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | 지정된 서버에 구성된 확장 서버 Blob 감사 정책의 세부 정보를 검색합니다. |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL 서버 데이터베이스 감사 정책 만들기 및 관리 |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL 서버 데이터베이스 감사 설정 만들기 및 관리 |
> | Microsoft.Sql/servers/databases/auditRecords/read | 데이터베이스 blob 감사 레코드를 검색합니다. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL 서버 데이터베이스 연결 정책 만들기 및 관리 |
> | Microsoft .Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL 서버 데이터베이스 데이터 마스킹 정책 만들기 및 관리 |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | 지정된 데이터베이스에 구성된 확장 Blob 감사 정책의 세부 정보를 검색합니다. |
> | Microsoft.Sql/servers/databases/read | 데이터베이스 목록을 가져오거나 지정된 데이터베이스에 대한 속성을 가져옵니다. |
> | Microsoft .Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | 데이터베이스 스키마를 가져옵니다. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | 데이터베이스 열을 가져옵니다. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | 데이터베이스 테이블을 가져옵니다. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL 서버 데이터베이스 보안 경고 정책 만들기 및 관리 |
> | Microsoft.Sql/servers/databases/securityMetrics/* | SQL 서버 데이터베이스 보안 메트릭 만들기 및 관리 |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft .Sql/servers/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | 서버 목록을 가져오거나 지정된 서버에 대한 속성을 가져옵니다. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL 서버 보안 경고 정책 만들기 및 관리 |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the security-related policies of SQL servers and databases, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "name": "056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/transparentDataEncryption/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/read",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/read",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/transparentDataEncryption/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/firewallRules/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Security Manager",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-server-contributor"></a>SQL Server 참가자

SQL server 및 데이터베이스를 관리할 수 있지만 보안 관련 정책에는 액세스할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | SQL 서버 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Insights/metrics/read | 메트릭 읽기 |
> | Microsoft.Insights/metricDefinitions/read | 메트릭 정의 읽기 |
> | **NotActions** |  |
> | Microsoft .Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/스키마/tables/columns/sensitivityLabels/* |  |
> | Microsoft .Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft .Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft .Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL 서버 감사 정책 편집 |
> | Microsoft.Sql/servers/auditingSettings/* | SQL 서버 감사 설정 편집 |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL 서버 데이터베이스 감사 정책 편집 |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL 서버 데이터베이스 감사 설정 편집 |
> | Microsoft.Sql/servers/databases/auditRecords/read | 데이터베이스 blob 감사 레코드를 검색합니다. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL 서버 데이터베이스 연결 정책 편집 |
> | Microsoft .Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL 서버 데이터베이스 데이터 마스킹 정책 편집 |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft .Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL 서버 데이터베이스 보안 경고 정책 편집 |
> | Microsoft.Sql/servers/databases/securityMetrics/* | SQL 서버 데이터베이스 보안 메트릭 편집 |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL 서버 보안 경고 정책 편집 |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL servers and databases, but not access to them, and not their security -related policies.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "name": "6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/*",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="analytics"></a>분석


### <a name="azure-event-hubs-data-owner"></a>Azure Event Hubs 데이터 소유자

Azure Event Hubs 리소스에 대 한 모든 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft EventHub/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft EventHub/* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f526a384-b230-433a-b45c-95f59c4a2dec",
  "name": "f526a384-b230-433a-b45c-95f59c4a2dec",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-receiver"></a>Azure Event Hubs 데이터 수신기

Azure Event Hubs 리소스에 대 한 수신 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft EventHub/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft EventHub/*/receive/action |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows receive access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "name": "a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/consumergroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-sender"></a>Azure Event Hubs 데이터 발신자

Azure Event Hubs 리소스에 대 한 액세스를 보낼 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft EventHub/*/eventhubs/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft EventHub/*/send/action |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2b629674-e913-4c01-ae53-ef4638d8f975",
  "name": "2b629674-e913-4c01-ae53-ef4638d8f975",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-factory-contributor"></a>Data Factory 참가자

데이터 팩터리를 만들고 관리하며 해당 하위 리소스도 만들고 관리합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.DataFactory/dataFactories/* | 데이터 팩터리 및 그 안에 포함된 자식 리소스를 만들고 관리합니다. |
> | Microsoft.DataFactory/factories/* | 데이터 팩터리 및 그 안에 포함된 자식 리소스를 만들고 관리합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.EventGrid/eventSubscriptions/write | EventSubscription 만들기 또는 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and manage data factories, as well as child resources within them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/673868aa-7521-48a0-acc6-0f60742d39f5",
  "name": "673868aa-7521-48a0-acc6-0f60742d39f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DataFactory/dataFactories/*",
        "Microsoft.DataFactory/factories/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.EventGrid/eventSubscriptions/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Factory Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-purger"></a>데이터 제거자

분석 데이터를 제거할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Application Insights에서 데이터 삭제 |
> | Microsoft.OperationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | Microsoft.OperationalInsights/workspaces/purge/action | 작업 영역에서 지정된 데이터를 삭제합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can purge analytics data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "name": "150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/components/*/read",
        "Microsoft.Insights/components/purge/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/purge/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Purger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-cluster-operator"></a>HDInsight 클러스터 운영자

HDInsight 클러스터 구성을 읽고 수정할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft HDInsight/*/읽기 |  |
> | Microsoft HDInsight/클러스터/getGatewaySettings/작업 | HDInsight 클러스터에 대 한 게이트웨이 설정 가져오기 |
> | Microsoft HDInsight/클러스터/updateGatewaySettings/action | HDInsight 클러스터에 대 한 게이트웨이 설정 업데이트 |
> | Microsoft HDInsight/클러스터/구성/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/operations/read | 배포 작업을 가져오거나 나열합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and modify HDInsight cluster configurations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/61ed4efc-fab3-44fd-b111-e24485cc132a",
  "name": "61ed4efc-fab3-44fd-b111-e24485cc132a",
  "permissions": [
    {
      "actions": [
        "Microsoft.HDInsight/*/read",
        "Microsoft.HDInsight/clusters/getGatewaySettings/action",
        "Microsoft.HDInsight/clusters/updateGatewaySettings/action",
        "Microsoft.HDInsight/clusters/configurations/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Cluster Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-domain-services-contributor"></a>HDInsight 도메인 서비스 기여자

HDInsight Enterprise Security Package에 필요한 도메인 서비스 관련 작업을 읽고, 만들고, 수정하고, 삭제할 수 있음

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.AAD/*/read |  |
> | Microsoft.AAD/domainServices/*/read |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can Read, Create, Modify and Delete Domain Services related operations needed for HDInsight Enterprise Security Package",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "name": "8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "permissions": [
    {
      "actions": [
        "Microsoft.AAD/*/read",
        "Microsoft.AAD/domainServices/*/read",
        "Microsoft.AAD/domainServices/oucontainer/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Domain Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-contributor"></a>Log Analytics 참가자

Log Analytics 참가자는 모든 모니터링 데이터를 읽고 모니터링 설정을 편집할 수 있습니다. 모니터링 설정 편집에는 VM에 VM 확장 추가, Azure Storage에서 로그 컬렉션을 구성할 수 있는 스토리지 계정 키 읽기, Automation 계정 생성 및 구성, 솔루션 추가 및 모든 Azure 리소스에 대한 Azure 진단을 구성하는 기능도 포함되어 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | 스토리지 계정의 액세스 키를 나열합니다. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | HybridCompute/컴퓨터/확장/쓰기 | Azure Arc 확장을 설치 또는 업데이트 합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server에 대한 진단 설정 생성, 업데이트 및 읽기 |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Contributor can read all monitoring data and edit monitoring settings. Editing monitoring settings includes adding the VM extension to VMs; reading storage account keys to be able to configure collection of logs from Azure Storage; creating and configuring Automation accounts; adding solutions; and configuring Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "name": "92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Automation/automationAccounts/*",
        "Microsoft.ClassicCompute/virtualMachines/extensions/*",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.Compute/virtualMachines/extensions/*",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/*",
        "Microsoft.OperationsManagement/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-reader"></a>Log Analytics 독자

Log Analytics 독자는 모든 Azure 리소스에 대한 Azure 진단의 구성 보기를 비롯하여 모니터링 설정 보기 및 모든 모니터링 데이터를 보고 검색할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | 새 엔진을 사용하여 검색합니다. |
> | Microsoft.OperationalInsights/workspaces/search/action | 검색 쿼리를 실행합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | 작업 영역에 대한 공유 키를 검색합니다. 이러한 키는 Microsoft Operational Insights 에이전트를 작업 영역에 연결하는 데 사용됩니다. |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Reader can view and search all monitoring data as well as and view monitoring settings, including viewing the configuration of Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/73c42c96-874c-492b-b04d-ab87d138a893",
  "name": "73c42c96-874c-492b-b04d-ab87d138a893",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.OperationalInsights/workspaces/sharedKeys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="blockchain"></a>블록체인


### <a name="blockchain-member-node-access-preview"></a>Blockchain 구성원 노드 액세스 (미리 보기)

Blockchain 멤버 노드에 대 한 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft Blockchain/blockchainMembers/transactionNodes/read | 기존 Blockchain 구성원 트랜잭션 노드를 가져오거나 나열 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft Blockchain/blockchainMembers/transactionNodes/connect/action | Blockchain 구성원 트랜잭션 노드에 연결 합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for access to Blockchain Member nodes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "name": "31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Blockchain Member Node Access (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="ai--machine-learning"></a>AI + 기계 학습


### <a name="cognitive-services-contributor"></a>Cognitive Services 기여자

Cognitive Services의 키를 만들고, 읽고, 업데이트하고, 삭제 및 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Features/features/read | 구독 기능을 가져옵니다. |
> | Microsoft.Features/providers/features/read | 지정된 리소스 공급자에서 구독의 기능을 가져옵니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server에 대한 진단 설정 생성, 업데이트 및 읽기 |
> | Microsoft.Insights/logDefinitions/read | 로그 정의 읽기 |
> | Microsoft.Insights/metricdefinitions/read | 메트릭 정의 읽기 |
> | Microsoft.Insights/metrics/read | 메트릭 읽기 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/deployments/operations/read | 배포 작업을 가져오거나 나열합니다. |
> | Microsoft.Resources/subscriptions/operationresults/read | 구독 작업 결과를 가져옵니다. |
> | Microsoft.Resources/subscriptions/read | 구독 목록을 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, read, update, delete and manage keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "name": "25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.CognitiveServices/*",
        "Microsoft.Features/features/read",
        "Microsoft.Features/providers/features/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-data-reader-preview"></a>데이터 판독기 Cognitive Services (미리 보기)

Cognitive Services 데이터를 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read Cognitive Services data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "name": "b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-user"></a>Cognitive Services 사용자

Cognitive Services의 키를 읽고 나열할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | 키를 나열합니다. |
> | Microsoft.Insights/alertRules/read | 클래식 메트릭 경고를 읽습니다. |
> | Microsoft.Insights/diagnosticSettings/read | 리소스 진단 설정을 읽습니다. |
> | Microsoft.Insights/logDefinitions/read | 로그 정의 읽기 |
> | Microsoft.Insights/metricdefinitions/read | 메트릭 정의 읽기 |
> | Microsoft.Insights/metrics/read | 메트릭 읽기 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/operations/read | 배포 작업을 가져오거나 나열합니다. |
> | Microsoft.Resources/subscriptions/operationresults/read | 구독 작업 결과를 가져옵니다. |
> | Microsoft.Resources/subscriptions/read | 구독 목록을 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and list keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a97b65f3-24c7-4388-baec-2e87135dc908",
  "name": "a97b65f3-24c7-4388-baec-2e87135dc908",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.CognitiveServices/accounts/listkeys/action",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Insights/diagnosticSettings/read",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="mixed-reality"></a>혼합 현실


### <a name="spatial-anchors-account-contributor"></a>공간 앵커 계정 기여자

계정의 공간 앵커를 관리할 수 있지만 삭제할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | MixedReality/SpatialAnchorsAccounts/만들기/작업 | 공간 앵커 만들기 |
> | MixedReality/SpatialAnchorsAccounts/검색/읽기 | 주변 공간 앵커 검색 |
> | MixedReality/SpatialAnchorsAccounts/속성/읽기 | 공간 앵커의 속성을 가져옵니다. |
> | MixedReality/SpatialAnchorsAccounts/쿼리/읽기 | 공간 앵커 찾기 |
> | MixedReality/SpatialAnchorsAccounts/submitdiag/read | 진단 데이터를 제출 하 여 Azure 공간 앵커 서비스의 품질 향상 |
> | MixedReality/SpatialAnchorsAccounts/write | 공간 앵커 속성 업데이트 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, but not delete them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "name": "8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-owner"></a>공간 앵커 계정 소유자

삭제를 포함 하 여 계정의 공간 앵커를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | MixedReality/SpatialAnchorsAccounts/만들기/작업 | 공간 앵커 만들기 |
> | MixedReality/SpatialAnchorsAccounts/delete | 공간 앵커 삭제 |
> | MixedReality/SpatialAnchorsAccounts/검색/읽기 | 주변 공간 앵커 검색 |
> | MixedReality/SpatialAnchorsAccounts/속성/읽기 | 공간 앵커의 속성을 가져옵니다. |
> | MixedReality/SpatialAnchorsAccounts/쿼리/읽기 | 공간 앵커 찾기 |
> | MixedReality/SpatialAnchorsAccounts/submitdiag/read | 진단 데이터를 제출 하 여 Azure 공간 앵커 서비스의 품질 향상 |
> | MixedReality/SpatialAnchorsAccounts/write | 공간 앵커 속성 업데이트 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, including deleting them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/70bbe301-9835-447d-afdd-19eb3167307c",
  "name": "70bbe301-9835-447d-afdd-19eb3167307c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/delete",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-reader"></a>공간 앵커 계정 판독기

계정의 공간 앵커 속성을 찾고 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | MixedReality/SpatialAnchorsAccounts/검색/읽기 | 주변 공간 앵커 검색 |
> | MixedReality/SpatialAnchorsAccounts/속성/읽기 | 공간 앵커의 속성을 가져옵니다. |
> | MixedReality/SpatialAnchorsAccounts/쿼리/읽기 | 공간 앵커 찾기 |
> | MixedReality/SpatialAnchorsAccounts/submitdiag/read | 진단 데이터를 제출 하 여 Azure 공간 앵커 서비스의 품질 향상 |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you locate and read properties of spatial anchors in your account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "name": "5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="integration"></a>통합


### <a name="api-management-service-contributor"></a>API Management 서비스 참가자

서비스 및 API를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ApiManagement/service/* | API Management 서비스 만들기 및 관리 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service and the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c",
  "name": "312a565d-c81f-4fd8-895a-4e21e48d571c",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-operator-role"></a>API Management 서비스 운영자 역할

서비스를 관리할 수 있지만 API는 관리할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ApiManagement/service/*/read | API Management 서비스 인스턴스 읽기 |
> | Microsoft.ApiManagement/service/backup/action | 사용자가 제공한 스토리지 계정의 지정된 컨테이너로 API Management 서비스를 백업합니다. |
> | Microsoft.ApiManagement/service/delete | API Management 서비스 인스턴스를 삭제합니다. |
> | Microsoft.ApiManagement/service/managedeployments/action | SKU/단위를 변경하고 API Management 서비스의 지역별 배포를 추가 또는 제거합니다. |
> | Microsoft.ApiManagement/service/read | API Management 서비스 인스턴스에 대한 메타데이터 읽기 |
> | Microsoft.ApiManagement/service/restore/action | 사용자가 제공한 스토리지 계정의 지정된 컨테이너에서 API Management 서비스 복원 |
> | Microsoft.ApiManagement/service/updatecertificate/action | API Management 서비스에 대 한 TLS/SSL 인증서 업로드 |
> | Microsoft.ApiManagement/service/updatehostname/action | API Management 서비스에 대한 사용자 지정 도메인 이름 설정, 업데이트 또는 제거합니다. |
> | Microsoft.ApiManagement/service/write | API Management 서비스 인스턴스 만들기 또는 업데이트 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | 사용자와 연결 된 키 가져오기 |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service but not the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "name": "e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/backup/action",
        "Microsoft.ApiManagement/service/delete",
        "Microsoft.ApiManagement/service/managedeployments/action",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.ApiManagement/service/restore/action",
        "Microsoft.ApiManagement/service/updatecertificate/action",
        "Microsoft.ApiManagement/service/updatehostname/action",
        "Microsoft.ApiManagement/service/write",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-reader-role"></a>Azure API Management 읽기 권한자 역할

서비스 및 API에 대한 읽기 전용 액세스

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ApiManagement/service/*/read | API Management 서비스 인스턴스 읽기 |
> | Microsoft.ApiManagement/service/read | API Management 서비스 인스턴스에 대한 메타데이터 읽기 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | 사용자와 연결 된 키 가져오기 |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to service and APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/71522526-b88f-4d52-b57f-d31fc3546d0d",
  "name": "71522526-b88f-4d52-b57f-d31fc3546d0d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-owner"></a>앱 구성 데이터 소유자

앱 구성 데이터에 대 한 모든 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft AppConfiguration/configurationStores/*/sread |  |
> | Microsoft AppConfiguration/configurationStores/*/쓰기 |  |
> | Microsoft AppConfiguration/configurationStores/*/delete |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows full access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "name": "5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read",
        "Microsoft.AppConfiguration/configurationStores/*/write",
        "Microsoft.AppConfiguration/configurationStores/*/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-reader"></a>앱 구성 데이터 판독기

앱 구성 데이터에 대 한 읽기 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | *없음* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft AppConfiguration/configurationStores/*/sread |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-4d78-a4de-a74fb236a071",
  "name": "516239f1-63e1-4d78-a4de-a74fb236a071",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-owner"></a>Azure Service Bus 데이터 소유자

Azure Service Bus 리소스에 대 한 모든 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | ServiceBus/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | ServiceBus/* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/090c5cfd-751d-490a-894a-3ce6f1109419",
  "name": "090c5cfd-751d-490a-894a-3ce6f1109419",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-receiver"></a>Azure Service Bus 데이터 수신기

Azure Service Bus 리소스에 대 한 수신 액세스를 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | ServiceBus/*/queues/read |  |
> | ServiceBus/*/항목/읽기 |  |
> | ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | ServiceBus/*/receive/action |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for receive access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "name": "4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-sender"></a>Azure Service Bus 데이터 발신자

Azure Service Bus 리소스에 대 한 액세스를 보낼 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | ServiceBus/*/queues/read |  |
> | ServiceBus/*/항목/읽기 |  |
> | ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | ServiceBus/*/send/action |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "name": "69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-stack-registration-owner"></a>Azure Stack 등록 소유자

Azure Stack 등록을 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft AzureStack/등록/제품/*/action |  |
> | Microsoft.AzureStack/registrations/products/read | Azure Stack Marketplace 제품의 속성 가져오기 |
> | Microsoft.AzureStack/registrations/read | Azure Stack 등록의 속성 가져오기 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Stack registrations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "name": "6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureStack/registrations/products/*/action",
        "Microsoft.AzureStack/registrations/products/read",
        "Microsoft.AzureStack/registrations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Stack Registration Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription 기여자

EventGrid 이벤트 구독 작업을 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | 항목 유형별로 글로벌 이벤트 구독을 나열합니다. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | 지역 이벤트 구독을 나열합니다. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | 항목 유형별로 지역 이벤트 구독을 나열합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid event subscription operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "name": "428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/*",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription 읽기 권한자

EventGrid 이벤트 구독을 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.EventGrid/eventSubscriptions/read | EventSubscription 읽기 |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | 항목 유형별로 글로벌 이벤트 구독을 나열합니다. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | 지역 이벤트 구독을 나열합니다. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | 항목 유형별로 지역 이벤트 구독을 나열합니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read EventGrid event subscriptions.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2414bbcf-6497-4faf-8c65-045460748405",
  "name": "2414bbcf-6497-4faf-8c65-045460748405",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="intelligent-systems-account-contributor"></a>지능형 시스템 계정 참가자

인텔리전트 시스템 계정을 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.IntelligentSystems/accounts/* | 지능형 시스템 계정 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Intelligent Systems accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/03a6d094-3444-4b3d-88af-7477090a9e5e",
  "name": "03a6d094-3444-4b3d-88af-7477090a9e5e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.IntelligentSystems/accounts/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Intelligent Systems Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-contributor"></a>논리 앱 참가자

논리 앱을 관리할 수 있지만 해당 앱에 대 한 액세스 권한은 변경할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | 스토리지 계정의 액세스 키를 나열합니다. |
> | Microsoft.ClassicStorage/storageAccounts/read | 지정된 계정의 스토리지 계정을 반환합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | MetricAlerts/* |  |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server에 대한 진단 설정 생성, 업데이트 및 읽기 |
> | Microsoft.Insights/logdefinitions/* | 이 사용 권한은 포털을 통해 활동 로그에 액세스해야 하는 사용자에게 필요합니다. 활동 로그의 로그 범주를 나열합니다. |
> | Microsoft.Insights/metricDefinitions/* | 메트릭 정의(리소스에 사용 가능한 메트릭 형식 목록)를 읽습니다. |
> | Microsoft.Logic/* | Logic Apps 리소스를 관리합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/operationresults/read | 구독 작업 결과를 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/listkeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Web/connectionGateways/* | 연결 게이트웨이를 만들고 관리합니다. |
> | Microsoft.Web/connections/* | 연결을 만들고 관리합니다. |
> | Microsoft.Web/customApis/* | 사용자 지정 API를 만들고 관리합니다. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | App Service 계획의 속성을 가져옵니다. |
> | Microsoft.Web/sites/functions/listSecrets/action | 함수 암호를 나열 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage logic app, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "name": "87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logdefinitions/*",
        "Microsoft.Insights/metricDefinitions/*",
        "Microsoft.Logic/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*",
        "Microsoft.Web/connections/*",
        "Microsoft.Web/customApis/*",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/functions/listSecrets/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-operator"></a>논리 앱 운영자

논리 앱을 읽고 사용 하지 않도록 설정할 수 있지만 편집 하거나 업데이트할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/*/read | Insights 경고 규칙을 읽습니다. |
> | Microsoft Insights/metricAlerts/*/cread |  |
> | Microsoft.Insights/diagnosticSettings/*/read | Logic Apps에 대한 진단 설정을 가져옵니다. |
> | Microsoft.Insights/metricDefinitions/*/read | Logic Apps에 사용 가능한 메트릭을 가져옵니다. |
> | Microsoft.Logic/*/read | Logic Apps 리소스를 읽습니다. |
> | Microsoft.Logic/workflows/disable/action | 워크플로를 사용하지 않도록 설정합니다. |
> | Microsoft.Logic/workflows/enable/action | 워크플로를 사용하도록 설정합니다. |
> | Microsoft.Logic/workflows/validate/action | 워크플로의 유효성을 검사합니다. |
> | Microsoft.Resources/deployments/operations/read | 배포 작업을 가져오거나 나열합니다. |
> | Microsoft.Resources/subscriptions/operationresults/read | 구독 작업 결과를 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Web/connectionGateways/*/read | 연결 게이트웨이를 읽습니다. |
> | Microsoft.Web/connections/*/read | 연결을 읽습니다. |
> | Microsoft.Web/customApis/*/read | 사용자 지정 API를 읽습니다. |
> | Microsoft.Web/serverFarms/read | App Service 계획의 속성을 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read, enable and disable logic app.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "name": "515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*/read",
        "Microsoft.Insights/metricAlerts/*/read",
        "Microsoft.Insights/diagnosticSettings/*/read",
        "Microsoft.Insights/metricDefinitions/*/read",
        "Microsoft.Logic/*/read",
        "Microsoft.Logic/workflows/disable/action",
        "Microsoft.Logic/workflows/enable/action",
        "Microsoft.Logic/workflows/validate/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*/read",
        "Microsoft.Web/connections/*/read",
        "Microsoft.Web/customApis/*/read",
        "Microsoft.Web/serverFarms/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="identity"></a>ID


### <a name="managed-identity-contributor"></a>관리 ID 참가자

사용자 할당 ID를 만들고, 읽고, 업데이트하고, 삭제합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/read | 기존 사용자 할당 ID를 가져옵니다. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/write | 새로운 사용자 할당 ID를 만들거나 기존 사용자 할당 ID와 연결된 태그를 업데이트합니다. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/delete | 기존 사용자 할당 ID를 삭제합니다. |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "name": "e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/write",
        "Microsoft.ManagedIdentity/userAssignedIdentities/delete",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-identity-operator"></a>관리 ID 운영자

사용자 할당 ID를 읽고 할당합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and Assign User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f1a07417-d97a-45cb-824c-7a7467783830",
  "name": "f1a07417-d97a-45cb-824c-7a7467783830",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="security"></a>보안


### <a name="azure-sentinel-contributor"></a>Azure 센티널 기여자

Azure 센티널 기여자

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft SecurityInsights/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | 새 엔진을 사용하여 검색합니다. |
> | Microsoft.OperationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* |  |
> | Microsoft.OperationsManagement/solutions/read | 기존 OMS 솔루션을 가져옵니다. |
> | Microsoft.OperationalInsights/workspaces/query/read | 작업 영역의 데이터에서 쿼리를 실행 |
> | OperationalInsights/작업 영역/쿼리/*/읽기 |  |
> | OperationalInsights/작업 영역/데이터 원본/읽기 | 작업 영역의 데이터 원본을 가져옵니다. |
> | Microsoft Insights/통합 문서/* |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ab8e14d6-4a74-4a29-9ba8-549422addade",
  "name": "ab8e14d6-4a74-4a29-9ba8-549422addade",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-reader"></a>Azure 센티널 판독기

Azure 센티널 판독기

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft SecurityInsights/*/읽기 |  |
> | DataConnectorsCheckRequirements/작업 | 사용자 권한 부여 및 라이선스 확인 |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | 새 엔진을 사용하여 검색합니다. |
> | Microsoft.OperationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | OperationalInsights/작업 영역/Linkedservices.json 및 datasets.json/읽기 | 지정된 작업 영역에서 연결된 서비스를 가져옵니다. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | 저장된 검색 쿼리를 가져옵니다. |
> | Microsoft.OperationsManagement/solutions/read | 기존 OMS 솔루션을 가져옵니다. |
> | Microsoft.OperationalInsights/workspaces/query/read | 작업 영역의 데이터에서 쿼리를 실행 |
> | OperationalInsights/작업 영역/쿼리/*/읽기 |  |
> | OperationalInsights/작업 영역/데이터 원본/읽기 | 작업 영역의 데이터 원본을 가져옵니다. |
> | Microsoft Insights/통합 문서/읽기 | 통합 문서 읽기 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "name": "8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/LinkedServices/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-responder"></a>Azure 센티널 응답자

Azure 센티널 응답자

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft SecurityInsights/*/읽기 |  |
> | DataConnectorsCheckRequirements/작업 | 사용자 권한 부여 및 라이선스 확인 |
> | Microsoft SecurityInsights/case/* |  |
> | Microsoft SecurityInsights/인시던트/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | 새 엔진을 사용하여 검색합니다. |
> | Microsoft.OperationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | OperationalInsights/작업 영역/데이터 원본/읽기 | 작업 영역의 데이터 원본을 가져옵니다. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | 저장된 검색 쿼리를 가져옵니다. |
> | Microsoft.OperationsManagement/solutions/read | 기존 OMS 솔루션을 가져옵니다. |
> | Microsoft.OperationalInsights/workspaces/query/read | 작업 영역의 데이터에서 쿼리를 실행 |
> | OperationalInsights/작업 영역/쿼리/*/읽기 |  |
> | OperationalInsights/작업 영역/데이터 원본/읽기 | 작업 영역의 데이터 원본을 가져옵니다. |
> | Microsoft Insights/통합 문서/읽기 | 통합 문서 읽기 |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Responder",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "name": "3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/cases/*",
        "Microsoft.SecurityInsights/incidents/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Responder",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-contributor"></a>Key Vault 참가자

키 자격 증명 모음을 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | 일시 삭제된 Key Vault를 제거합니다. |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage key vaults, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f25e0fa2-a7c8-4377-a976-54943a77a395",
  "name": "f25e0fa2-a7c8-4377-a976-54943a77a395",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.KeyVault/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.KeyVault/locations/deletedVaults/purge/action",
        "Microsoft.KeyVault/hsmPools/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-admin"></a>보안 관리자

Security Center에 대 한 사용 권한을 확인 하 고 업데이트 합니다. 보안 읽기 권한자 역할과 동일한 권한 및 보안 정책을 업데이트 하 고 경고 및 권장 사항을 해제할 수도 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Authorization/policyAssignments/* | 정책 할당 만들기 및 관리 |
> | Microsoft.Authorization/policyDefinitions/* | 정책 정의 만들기 및 관리 |
> | Microsoft.Authorization/policySetDefinitions/* | 정책 집합 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Security/* | 보안 구성 요소 및 정책 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Admin Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "name": "fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Authorization/policyAssignments/*",
        "Microsoft.Authorization/policyDefinitions/*",
        "Microsoft.Authorization/policySetDefinitions/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-assessment-contributor"></a>보안 평가 참가자

Security Center에 대 한 평가를 푸시할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft. 보안/평가/작성 | 구독에서 보안 평가를 만들거나 업데이트 합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you push assessments to Security Center",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "name": "612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Security/assessments/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Assessment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-manager-legacy"></a>보안 관리자(레거시)

레거시 역할입니다. 대신 보안 관리자를 사용 하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.ClassicCompute/*/read | 클래식 가상 머신에 대한 구성 정보 읽기 |
> | Microsoft.ClassicCompute/virtualMachines/*/write | 클래식 가상 머신에 대한 구성 정보 쓰기 |
> | Microsoft.ClassicNetwork/*/read | 클래식 네트워크에 대한 구성 정보 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Security/* | 보안 구성 요소 및 정책 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "This is a legacy role. Please use Security Administrator instead",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "name": "e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/*/read",
        "Microsoft.ClassicCompute/virtualMachines/*/write",
        "Microsoft.ClassicNetwork/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Manager (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-reader"></a>보안 Reader

Security Center에 대 한 사용 권한을 봅니다. 권장 사항, 경고, 보안 정책 및 보안 상태를 볼 수 있지만 변경할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics 데이터 보기 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Security/*/read | 보안 구성 요소 및 정책 읽기 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "name": "39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*/read",
        "Microsoft.Support/*",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="devops"></a>DevOps


### <a name="devtest-labs-user"></a>DevTest Lab 사용자

Azure DevTest Labs의 가상 머신을 연결, 시작, 다시 시작 및 종료할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Compute/availabilitySets/read | 가용성 집합의 속성을 가져옵니다. |
> | Microsoft.Compute/virtualMachines/*/read | 가상 머신(VM 크기, 런타임 상태, VM 확장 등)의 속성 읽기 |
> | Microsoft.Compute/virtualMachines/deallocate/action | 가상 머신을 끄고 Compute 리소스를 해제합니다. |
> | Microsoft.Compute/virtualMachines/read | 가상 머신의 속성을 가져옵니다. |
> | Microsoft.Compute/virtualMachines/restart/action | 가상 머신을 다시 시작합니다. |
> | Microsoft.Compute/virtualMachines/start/action | 가상 머신 시작 |
> | Microsoft.DevTestLab/*/read | 랩의 속성 읽기 |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | 랩에서 임의 클레임 가능 가상 머신을 클레임합니다. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | 랩에 가상 머신을 만듭니다. |
> | Microsoft DevTestLab/labs/ensureCurrentUserProfile/action | 현재 사용자가 랩에서 유효한 프로필을 갖고 있는지 확인 합니다. |
> | Microsoft.DevTestLab/labs/formulas/delete | 수식을 삭제합니다. |
> | Microsoft.DevTestLab/labs/formulas/read | 수식을 읽습니다. |
> | Microsoft.DevTestLab/labs/formulas/write | 수식을 추가하거나 수정합니다. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | 랩 정책을 평가합니다. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | 기존 가상 머신의 소유권을 가져옵니다. |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | 해당 시작/중지 일정이 있는 경우 이를 나열합니다. |
> | Microsoft DevTestLab/labs/virtualMachines/getRdpFileContents/action | 가상 머신에 대한 RDP 파일의 콘텐츠를 나타내는 문자열을 가져옵니다. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | 부하 분산 장치 백 엔드 주소 풀을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | 부하 분산 장치 인바운드 nat 규칙을 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/networkInterfaces/*/read | 네트워크 인터페이스(예: 네트워크 인터페이스의 일부인 모든 부하 분산 장치)의 속성 읽기 |
> | Microsoft.Network/networkInterfaces/join/action | 가상 컴퓨터를 네트워크 인터페이스에 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/networkInterfaces/read | 네트워크 인터페이스 정의를 가져옵니다.  |
> | Microsoft.Network/networkInterfaces/write | 네트워크 인터페이스를 만들거나 기존 네트워크 인터페이스를 업데이트합니다.  |
> | Microsoft.Network/publicIPAddresses/*/read | 공용 IP 주소의 속성 읽기 |
> | Microsoft.Network/publicIPAddresses/join/action | 공용 ip 주소를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Network/publicIPAddresses/read | 공용 IP 주소 정의를 가져옵니다. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | 가상 네트워크를 조인 합니다. 했어야 하지 않습니다. |
> | Microsoft.Resources/deployments/operations/read | 배포 작업을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/read | 배포를 가져오거나 나열합니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/listKeys/action | 지정된 스토리지 계정에 대한 액세스 키를 반환합니다. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | 가상 머신이 업데이트될 수 있는 사용 가능한 크기를 나열합니다. |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76283e04-6283-4c54-8f91-bcf1374a3c64",
  "name": "76283e04-6283-4c54-8f91-bcf1374a3c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.DevTestLab/*/read",
        "Microsoft.DevTestLab/labs/claimAnyVm/action",
        "Microsoft.DevTestLab/labs/createEnvironment/action",
        "Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action",
        "Microsoft.DevTestLab/labs/formulas/delete",
        "Microsoft.DevTestLab/labs/formulas/read",
        "Microsoft.DevTestLab/labs/formulas/write",
        "Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action",
        "Microsoft.DevTestLab/labs/virtualMachines/claim/action",
        "Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action",
        "Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/networkInterfaces/join/action",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/publicIPAddresses/*/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listKeys/action"
      ],
      "notActions": [
        "Microsoft.Compute/virtualMachines/vmSizes/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DevTest Labs User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="lab-creator"></a>랩 작성자

Azure Lab 계정 하에서 관리 랩을 만들고, 관리하고, 삭제할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | 랩 계정에서 랩을 만듭니다. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | 랩 계정에서 구성된 각 크기 범주에 대한 지역별 가용성 정보를 가져옵니다. |
> | Microsoft. 서비스/랩 계정/getPricingAndAvailability/작업 | 랩 계정의 크기, 지역 및 운영 체제 조합에 대 한 가격 책정 및 가용성을 가져옵니다. |
> | Microsoft. 서비스/랩 계정/getRestrictionsAndUsage/작업 | 이 구독에 대 한 핵심 제한 및 사용 현황 가져오기 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, manage, delete your managed labs under your Azure Lab Accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "name": "b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.LabServices/labAccounts/*/read",
        "Microsoft.LabServices/labAccounts/createLab/action",
        "Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getPricingAndAvailability/action",
        "Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Lab Creator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="monitor"></a>모니터


### <a name="application-insights-component-contributor"></a>Application Insights 구성 요소 참가자

Application Insights 구성 요소를 관리할 수 있음

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 경고 규칙 만들기 및 관리 |
> | MetricAlerts/* | 새 경고 규칙 만들기 및 관리 |
> | Microsoft.Insights/components/* | Insights 구성 요소 만들기 및 관리 |
> | Microsoft.Insights/webtests/* | Insights 웹 테스트 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage Application Insights components",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e",
  "name": "ae349356-3a1b-4a5e-921d-050484c6347e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Component Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="application-insights-snapshot-debugger"></a>Application Insights 스냅샷 디버거

Application Insights 스냅샷 디버거를 사용하여 수집한 디버그 스냅샷을 보고 다운로드할 수 있는 사용자 권한을 제공합니다. 이러한 사용 권한은 [소유자](#owner) 또는 [기여자](#contributor) 역할에 포함되지 않습니다. 사용자에 게 Application Insights 스냅숏 디버거 역할을 부여 하는 경우 사용자에 게 직접 역할을 부여 해야 합니다. 역할은 사용자 지정 역할에 추가 될 때 인식 되지 않습니다. 

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives user permission to use Application Insights Snapshot Debugger features",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "name": "08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Snapshot Debugger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-contributor"></a>Monitoring Contributor

모든 모니터링 데이터를 읽고 모니터링 설정을 편집할 수 있음 [Azure Monitor에서의 역할, 권한 및 보안 시작](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)도 참조하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Insights/components/* | Insights 구성 요소 만들기 및 관리 |
> | Microsoft.Insights/DiagnosticSettings/* | Analysis Server에 대한 진단 설정 생성, 업데이트 및 읽기 |
> | Microsoft.Insights/eventtypes/* | 구독에서 활동 로그 이벤트(관리 이벤트)를 나열합니다. 이 권한은 활동 로그에 대한 프로그래밍 방식 및 포털 액세스 모두에 적용 가능합니다. |
> | Microsoft.Insights/LogDefinitions/* | 이 사용 권한은 포털을 통해 활동 로그에 액세스해야 하는 사용자에게 필요합니다. 활동 로그의 로그 범주를 나열합니다. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | 메트릭 정의(리소스에 사용 가능한 메트릭 형식 목록)를 읽습니다. |
> | Microsoft.Insights/Metrics/* | 리소스에 대한 메트릭을 읽습니다. |
> | Microsoft.Insights/Register/Action | Microsoft Insights 공급자를 등록합니다. |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Insights 웹 테스트 만들기 및 관리 |
> | Microsoft Insights/통합 문서/* |  |
> | PrivateLinkScopes/* |  |
> | PrivateLinkScopeOperationStatuses/* |  |
> | Microsoft.OperationalInsights/workspaces/write | 새 작업 영역을 만들거나 기존 작업 영역의 고객 ID를 제공하여 기존 작업 영역에 연결합니다. |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Log analytics 솔루션 팩을 읽고 쓰고 삭제 합니다. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Log analytics 저장 된 검색을 읽고 쓰고 삭제 합니다. |
> | Microsoft.OperationalInsights/workspaces/search/action | 검색 쿼리를 실행합니다. |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | 작업 영역에 대한 공유 키를 검색합니다. 이러한 키는 Microsoft Operational Insights 에이전트를 작업 영역에 연결하는 데 사용됩니다. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Log analytics 저장소 정보 구성을 읽고 쓰고 삭제 합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | AlertsManagement/smartDetectorAlertRules/* |  |
> | AlertsManagement/actionRules/* |  |
> | AlertsManagement/smartGroups/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data and update monitoring settings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "name": "749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.AlertsManagement/alerts/*",
        "Microsoft.AlertsManagement/alertsSummary/*",
        "Microsoft.Insights/actiongroups/*",
        "Microsoft.Insights/activityLogAlerts/*",
        "Microsoft.Insights/AlertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/DiagnosticSettings/*",
        "Microsoft.Insights/eventtypes/*",
        "Microsoft.Insights/LogDefinitions/*",
        "Microsoft.Insights/metricalerts/*",
        "Microsoft.Insights/MetricDefinitions/*",
        "Microsoft.Insights/Metrics/*",
        "Microsoft.Insights/Register/Action",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/privateLinkScopes/*",
        "Microsoft.Insights/privateLinkScopeOperationStatuses/*",
        "Microsoft.OperationalInsights/workspaces/write",
        "Microsoft.OperationalInsights/workspaces/intelligencepacks/*",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action",
        "Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*",
        "Microsoft.Support/*",
        "Microsoft.WorkloadMonitor/monitors/*",
        "Microsoft.WorkloadMonitor/notificationSettings/*",
        "Microsoft.AlertsManagement/smartDetectorAlertRules/*",
        "Microsoft.AlertsManagement/actionRules/*",
        "Microsoft.AlertsManagement/smartGroups/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-metrics-publisher"></a>모니터링 메트릭 게시자

Azure 리소스에 대한 게시 메트릭 사용

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Insights/Register/Action | Microsoft Insights 공급자를 등록합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | 메트릭을 작성합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Enables publishing metrics against Azure resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c390055eb",
  "name": "3913510d-42f4-4e42-8a64-420c390055eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/Register/Action",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Insights/Metrics/Write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Metrics Publisher",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-reader"></a>Monitoring Reader

모든 모니터링 데이터를 읽을 수 있음(메트릭, 로그 등) [Azure Monitor에서의 역할, 권한 및 보안 시작](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)도 참조하세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.OperationalInsights/workspaces/search/action | 검색 쿼리를 실행합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "name": "43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-contributor"></a>통합 문서 기여자

공유 통합 문서를 저장할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft Insights/통합 문서/쓰기 | 통합 문서 만들기 또는 업데이트 |
> | Microsoft Insights/통합 문서/삭제 | 통합 문서 삭제 |
> | Microsoft Insights/통합 문서/읽기 | 통합 문서 읽기 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can save shared workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "name": "e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/workbooks/write",
        "Microsoft.Insights/workbooks/delete",
        "Microsoft.Insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-reader"></a>통합 문서 판독기

통합 문서를 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | microsoft insights/통합 문서/읽기 | 통합 문서 읽기 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "name": "b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "permissions": [
    {
      "actions": [
        "microsoft.insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="management--governance"></a>관리 + 거 버 넌 스


### <a name="automation-job-operator"></a>Automation 작업 연산자

Automation Runbook을 사용하여 작업을 만들고 관리합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Hybrid Runbook Worker 리소스를 읽습니다. |
> | Microsoft.Automation/automationAccounts/jobs/read | Azure Automation 작업을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Azure Automation 작업을 계속합니다. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Azure Automation 작업을 중지합니다. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Azure Automation 작업 스트림을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Automation 작업을 일시 중단합니다. |
> | Microsoft.Automation/automationAccounts/jobs/write | Azure Automation 작업을 만듭니다. |
> | Microsoft.Automation/automationAccounts/jobs/output/read | 작업의 출력을 가져옵니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and Manage Jobs using Automation Runbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "name": "4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Job Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-operator"></a>Automation 운영자

자동화 연산자는 작업을 시작, 중지, 일시 중단 및 다시 시작할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Hybrid Runbook Worker 리소스를 읽습니다. |
> | Microsoft.Automation/automationAccounts/jobs/read | Azure Automation 작업을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Azure Automation 작업을 계속합니다. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Azure Automation 작업을 중지합니다. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Azure Automation 작업 스트림을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Automation 작업을 일시 중단합니다. |
> | Microsoft.Automation/automationAccounts/jobs/write | Azure Automation 작업을 만듭니다. |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Azure Automation 작업 일정을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Azure Automation 작업 일정을 만듭니다. |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | 자동화 계정에 연결된 작업 영역 가져오기 |
> | Microsoft.Automation/automationAccounts/read | Azure Automation 계정을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/runbooks/read | Azure Automation Runbook을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/schedules/read | Azure Automation 일정 자산을 가져옵니다. |
> | Microsoft.Automation/automationAccounts/schedules/write | Azure Automation 일정 자산을 만들거나 업데이트합니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Automation/automationAccounts/jobs/output/read | 작업의 출력을 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Automation Operators are able to start, stop, suspend, and resume jobs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d3881f73-407a-4167-8283-e981cbba0404",
  "name": "d3881f73-407a-4167-8283-e981cbba0404",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobSchedules/read",
        "Microsoft.Automation/automationAccounts/jobSchedules/write",
        "Microsoft.Automation/automationAccounts/linkedWorkspace/read",
        "Microsoft.Automation/automationAccounts/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Automation/automationAccounts/schedules/read",
        "Microsoft.Automation/automationAccounts/schedules/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-runbook-operator"></a>Automation Runbook 연산자

Runbook 작업을 만들려면 Runbook 속성을 읽어보세요.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Automation/automationAccounts/runbooks/read | Azure Automation Runbook을 가져옵니다. |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read Runbook properties - to be able to create Jobs of the runbook.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "name": "5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Runbook Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-onboarding"></a>Azure 연결 된 컴퓨터 온 보 딩

Azure 연결 된 컴퓨터를 등록할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | HybridCompute/컴퓨터/읽기 | 모든 Azure Arc 컴퓨터를 읽습니다. |
> | HybridCompute/컴퓨터/쓰기 | Azure Arc 컴퓨터를 작성 합니다. |
> | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | 게스트 구성 할당을 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "name": "b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.GuestConfiguration/guestConfigurationAssignments/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-resource-administrator"></a>Azure 연결 된 컴퓨터 리소스 관리자

Azure에 연결 된 컴퓨터를 읽고, 쓰고, 삭제 하 고, 다시 등록할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | HybridCompute/컴퓨터/읽기 | 모든 Azure Arc 컴퓨터를 읽습니다. |
> | HybridCompute/컴퓨터/쓰기 | Azure Arc 컴퓨터를 작성 합니다. |
> | HybridCompute/컴퓨터/삭제 | Azure Arc 컴퓨터를 삭제 합니다. |
> | HybridCompute/컴퓨터/다시 연결/작업 | Azure Arc 컴퓨터 다시 연결 |
> | HybridCompute/컴퓨터/확장/쓰기 | Azure Arc 확장을 설치 또는 업데이트 합니다. |
> | HybridCompute/*/읽기 |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read, write, delete and re-onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cd570a14-e51a-42ad-bac8-bafd67325302",
  "name": "cd570a14-e51a-42ad-bac8-bafd67325302",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/machines/delete",
        "Microsoft.HybridCompute/machines/reconnect/action",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.HybridCompute/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Resource Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="billing-reader"></a>청구 읽기 권한자

결제 데이터에 대해 읽기 권한 허용

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Billing/*/read | 대금 청구 정보 읽기 |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to billing data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "name": "fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Billing/*/read",
        "Microsoft.Commerce/*/read",
        "Microsoft.Consumption/*/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Billing Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-contributor"></a>청사진 기여자

청사진 정의를 관리할 수 있지만 할당할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft. 청사진/청사진/* | 청사진 정의 또는 청사진 아티팩트를 만들고 관리 합니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage blueprint definitions, but not assign them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/41077137-e803-4205-871c-5a86e6a753b4",
  "name": "41077137-e803-4205-871c-5a86e6a753b4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprints/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-operator"></a>청사진 연산자

는 게시 된 기존 청사진을 할당할 수 있지만 새 청사진을 만들 수는 없습니다. 사용자 할당 관리 id를 사용 하 여 할당을 수행 하는 경우에만이 기능이 작동 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft. 청사진/blueprintAssignments/* | 청사진 할당을 만들고 관리 합니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can assign existing published blueprints, but cannot create new blueprints. NOTE: this only works if the assignment is done with a user-assigned managed identity.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/437d2ced-4a38-4302-8479-ed2bcb43d090",
  "name": "437d2ced-4a38-4302-8479-ed2bcb43d090",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprintAssignments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-contributor"></a>Cost Management 기여자

비용을 확인하고 비용 구성(예: 예산, 내보내기)을 관리할 수 있음

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | 구독 목록을 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Advisor/configurations/read | 구성 가져오기 |
> | Microsoft.Advisor/recommendations/read | 권장 사항을 읽습니다. |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view costs and manage cost configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/434105ed-43f6-45c7-a02f-909b2ba83430",
  "name": "434105ed-43f6-45c7-a02f-909b2ba83430",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*",
        "Microsoft.CostManagement/*",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-reader"></a>Cost Management 읽기 권한자

비용 데이터 및 구성(예: 예산, 내보내기)을 확인할 수 있음

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | 구독 목록을 가져옵니다. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft.Advisor/configurations/read | 구성 가져오기 |
> | Microsoft.Advisor/recommendations/read | 권장 사항을 읽습니다. |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view cost data and configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/72fafb9e-0641-4937-9268-a91bfd8191a3",
  "name": "72fafb9e-0641-4937-9268-a91bfd8191a3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-contributor-role"></a>관리 되는 응용 프로그램 참가자 역할

관리 되는 응용 프로그램 리소스를 만들 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft 솔루션/응용 프로그램/* |  |
> | Microsoft.Solutions/register/action | 솔루션에 등록합니다. |
> | Microsoft .Resources/subscription/resourceGroups/* |  |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for creating managed application resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/641177b8-a67a-45b9-a033-47bc880bb21e",
  "name": "641177b8-a67a-45b9-a033-47bc880bb21e",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/*",
        "Microsoft.Solutions/register/action",
        "Microsoft.Resources/subscriptions/resourceGroups/*",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-operator-role"></a>관리되는 애플리케이션 운영자 역할

관리되는 애플리케이션 리소스에서 작업을 읽고 수행할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.Solutions/applications/read | 애플리케이션 목록을 검색합니다. |
> | Microsoft Solutions/*/action |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and perform actions on Managed Application resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "name": "c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/read",
        "Microsoft.Solutions/*/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-applications-reader"></a>Managed Applications 읽기 권한자

관리 앱 및 요청 JIT 액세스에서 리소스를 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read resources in a managed app and request JIT access.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "name": "b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Solutions/jitRequests/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Applications Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-services-registration-assignment-delete-role"></a>관리 되는 서비스 등록 할당 삭제 역할

관리 되는 서비스 등록 할당 삭제 역할을 통해 테 넌 트 사용자는 테 넌 트에 할당 된 등록 할당을 삭제할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft ManagedServices/registrationAssignments/read | 관리 되는 서비스 등록 할당 목록을 검색 합니다. |
> | Microsoft ManagedServices/registrationAssignments/delete | 관리 되는 서비스 등록 할당을 제거 합니다. |
> | Microsoft ManagedServices/operationStatuses/읽기 | 리소스의 작업 상태를 읽습니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Managed Services Registration Assignment Delete Role allows the managing tenant users to delete the registration assignment assigned to their tenant.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/91c1777a-f3dc-4fae-b103-61d183457e46",
  "name": "91c1777a-f3dc-4fae-b103-61d183457e46",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedServices/registrationAssignments/read",
        "Microsoft.ManagedServices/registrationAssignments/delete",
        "Microsoft.ManagedServices/operationStatuses/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Services Registration assignment Delete Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-contributor"></a>관리 그룹 참가자

관리 그룹 참가자 역할

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Management/managementGroups/delete | 관리 그룹을 삭제합니다. |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | Microsoft.Management/managementGroups/subscriptions/delete | 관리 그룹에서 구독의 연결을 해제합니다. |
> | Microsoft.Management/managementGroups/subscriptions/write | 기존 구독을 관리 그룹과 연결합니다. |
> | Microsoft.Management/managementGroups/write | 관리 그룹을 만들거나 업데이트합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Contributor Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "name": "5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/delete",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/delete",
        "Microsoft.Management/managementGroups/subscriptions/write",
        "Microsoft.Management/managementGroups/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-reader"></a>관리 그룹 읽기 권한자

관리 그룹 읽기 권한자 역할

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Management/managementGroups/read | 인증된 사용자의 관리 그룹을 나열합니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ac63b705-f282-497d-ac71-919bf39d939d",
  "name": "ac63b705-f282-497d-ac71-919bf39d939d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="new-relic-apm-account-contributor"></a>NewRelic APM 계정 참가자

New Relic Application Performance Management 계정 및 애플리케이션을 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage New Relic Application Performance Management accounts and applications, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d28c62d-5b37-4476-8438-e587778df237",
  "name": "5d28c62d-5b37-4476-8438-e587778df237",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "NewRelic.APM/accounts/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "New Relic APM Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="policy-insights-data-writer-preview"></a>Policy Insights 데이터 기록기 (미리 보기)

리소스 정책에 대 한 읽기 액세스를 허용 하 고 리소스 구성 요소 정책 이벤트에 대 한 쓰기 권한을 허용 합니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft. 권한 부여/policyassignments/읽기 | 정책 할당에 대한 정보를 가져옵니다. |
> | Microsoft. Authorization/policydefinitions/읽기 | 정책 정의에 대한 정보를 가져옵니다. |
> | Microsoft. Authorization/policysetdefinitions/읽기 | 정책 집합 정의에 대한 정보를 가져옵니다. |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | Microsoft. PolicyInsights/checkDataPolicyCompliance/action | 데이터 정책에 대해 지정 된 구성 요소의 준수 상태를 확인 합니다. |
> | Microsoft. PolicyInsights/Policyinsights/logDataEvents/action | 리소스 구성 요소 정책 이벤트를 로깅합니다. |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to resource policies and write access to resource component policy events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "name": "66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/policyassignments/read",
        "Microsoft.Authorization/policydefinitions/read",
        "Microsoft.Authorization/policysetdefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.PolicyInsights/checkDataPolicyCompliance/action",
        "Microsoft.PolicyInsights/policyEvents/logDataEvents/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Policy Insights Data Writer (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="resource-policy-contributor"></a>리소스 정책 참가자

리소스 정책을 만들거나 수정할 수 있는 권한이 있는 사용자, 지원 티켓을 만들고 리소스/계층 구조를 읽을 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | */read | 암호를 제외한 모든 유형의 리소스를 읽습니다. |
> | Microsoft.Authorization/policyassignments/* | 정책 할당 만들기 및 관리 |
> | Microsoft.Authorization/policydefinitions/* | 정책 정의 만들기 및 관리 |
> | Microsoft.Authorization/policysetdefinitions/* | 정책 집합 만들기 및 관리 |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Users with rights to create/modify resource policy, create support ticket and read resources/hierarchy.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/36243c78-bf99-498c-9df9-86d9f8d28608",
  "name": "36243c78-bf99-498c-9df9-86d9f8d28608",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/policyassignments/*",
        "Microsoft.Authorization/policydefinitions/*",
        "Microsoft.Authorization/policysetdefinitions/*",
        "Microsoft.PolicyInsights/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Resource Policy Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-contributor"></a>Site Recovery 참가자

자격 증명 모음 만들기 및 역할 할당을 제외한 Site Recovery 서비스를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | 리소스 인증서 업데이트 작업은 리소스/저장소 자격 증명 인증서를 업데이트합니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | 자격 증명 모음과 관련된 확장 정보 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | 등록된 ID 만들기 및 관리 |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | 복제 경고 설정 만들기 또는 업데이트 |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | 이벤트를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | 복제 패브릭 만들기 및 관리 |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | 복제 작업 만들기 및 관리 |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | 복제 정책 만들기 및 관리 |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | 복구 계획 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Recovery Services 자격 증명 모음의 스토리지 구성 만들기 및 관리 |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | 자격 증명 모음 토큰 작업을 사용하여 자격 증명 모음 수준의 백 엔드 작업에 대한 자격 증명 모음 토큰을 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Recovery Services 자격 증명 모음에 대한 경고 읽기 |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft RecoveryServices/자격 증명 모음/replicationOperationStatus/읽기 | 자격 증명 모음 복제 작업 상태를 읽습니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Site Recovery service except vault creation and role assignment",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "name": "6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/*",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/*",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/*",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/*",
        "Microsoft.RecoveryServices/Vaults/storageConfig/*",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/vaults/replicationOperationStatus/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-operator"></a>Site Recovery 운영자

장애 조치(failover) 및 장애 복구(failback)를 수행할 수 있지만 다른 Site Recovery 관리 작업은 수행할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Network/virtualNetworks/read | 가상 네트워크 정의를 가져옵니다. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | 확장 정보 가져오기 작업에서는 ‘자격 증명 모음’ 형식의 Azure 리소스를 나타내는 개체의 확장 정보를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | 작업 결과 가져오기 작업을 사용하여 비동기적으로 제출된 작업에 대한 작업 상태와 결과를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | 컨테이너 가져오기 작업을 사용하여 리소스에 대해 등록된 컨테이너를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | 경고 설정을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | 이벤트를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | 패브릭의 일관성을 검사합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | 패브릭을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | 게이트웨이를 다시 연결합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | 패브릭용 인증서 갱신 |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | 네트워크를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | 네트워크 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | 보호 컨테이너를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | 보호 가능한 항목을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | 복구 지점을 적용합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | 장애 조치(Failover) 커밋 |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | 계획된 장애 조치(Failover) |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | 보호된 항목을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | 복제 복구 지점을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | 복제를 복구합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | 보호된 항목을 다시 보호합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | 보호 컨테이너를 전환합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | 테스트 장애 조치(Failover) |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | 테스트 장애 조치(Failover) 정리 |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | 장애 조치  |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | 모바일 서비스를 업데이트합니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | 보호 컨테이너 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Recovery Services 공급자를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | 공급자를 새로 고칩니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | 스토리지 분류를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | 스토리지 분류 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | vCenter를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | 복제 작업 만들기 및 관리 |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | 정책을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | 장애 조치(Failover) 커밋 복구 계획 |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | 계획된 장애 조치(Failover) 복구 계획 |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | 복구 계획을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | 복구 계획을 다시 보호합니다. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | 테스트 장애 조치(failover) 복구 계획 |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | 테스트 장애 조치(failover) 정리 복구 계획 |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | 장애 조치(Failover) 복구 계획 |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Recovery Services 자격 증명 모음에 대한 경고 읽기 |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | 자격 증명 모음 토큰 작업을 사용하여 자격 증명 모음 수준의 백 엔드 작업에 대한 자격 증명 모음 토큰을 가져올 수 있습니다. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Storage/storageAccounts/read | 스토리지 계정의 목록을 반환하거나 지정된 스토리지 계정의 속성을 가져옵니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you failover and failback but not perform other Site Recovery management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494ae006-db33-4328-bf46-533a6560a3ca",
  "name": "494ae006-db33-4328-bf46-533a6560a3ca",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-reader"></a>Site Recovery 구독자

Site Recovery 상태를 볼 수 있지만 다른 관리 작업은 수행할 수 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp는 서비스에서 사용하는 내부 작업입니다. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | 확장 정보 가져오기 작업에서는 ‘자격 증명 모음’ 형식의 Azure 리소스를 나타내는 개체의 확장 정보를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Recovery Services 자격 증명 모음에 대한 경고를 받습니다. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | 자격 증명 모음 가져오기 작업에서는 '자격 증명 모음' 형식의 Azure 리소스를 나타내는 개체를 가져옵니다. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | 작업 결과 가져오기 작업을 사용하여 비동기적으로 제출된 작업에 대한 작업 상태와 결과를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | 컨테이너 가져오기 작업을 사용하여 리소스에 대해 등록된 컨테이너를 가져올 수 있습니다. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | 경고 설정을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | 이벤트를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | 패브릭을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | 네트워크를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | 네트워크 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | 보호 컨테이너를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | 보호 가능한 항목을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | 보호된 항목을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | 복제 복구 지점을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | 보호 컨테이너 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Recovery Services 공급자를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | 스토리지 분류를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | 스토리지 분류 매핑을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | vCenter를 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | 작업을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | 정책을 읽습니다. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | 복구 계획을 읽습니다. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Recovery Services 자격 증명 모음에 대한 사용 세부 정보를 반환합니다. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | 자격 증명 모음 토큰 작업을 사용하여 자격 증명 모음 수준의 백 엔드 작업에 대한 자격 증명 모음 토큰을 가져올 수 있습니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view Site Recovery status but not perform other management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "name": "dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/read",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="support-request-contributor"></a>지원 요청 참가자

지원 요청을 만들고 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create and manage Support requests",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "name": "cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Support Request Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="tag-contributor"></a>태그 참여자

엔터티 자체에 대 한 액세스를 제공 하지 않고 엔터티의 태그를 관리할 수 있습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft .Resources/subscription/resourceGroups/resources/read | 리소스 그룹에 대한 리소스를 가져옵니다. |
> | Microsoft.Resources/subscriptions/resources/read | 구독 리소스를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | Microsoft .Resources/tags/* |  |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage tags on entities, without providing access to the entities themselves.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "name": "4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
        "Microsoft.Resources/subscriptions/resources/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/tags/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Tag Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="other"></a>기타


### <a name="biztalk-contributor"></a>BizTalk 참가자

BizTalk Services를 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.BizTalkServices/BizTalk/* | BizTalk 서비스 만들기 및 관리 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage BizTalk services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "name": "5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BizTalkServices/BizTalk/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "BizTalk Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="scheduler-job-collections-contributor"></a>Scheduler 작업 컬렉션 참가자

Scheduler 작업 컬렉션을 관리할 수 있지만 액세스할 수는 없습니다.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **actions** |  |
> | Microsoft.Authorization/*/read | 역할 및 역할 할당 읽기 |
> | Microsoft.Insights/alertRules/* | 클래식 메트릭 경고 만들기 및 관리 |
> | Microsoft.ResourceHealth/availabilityStatuses/read | 지정된 범위의 모든 리소스에 대한 가용성 상태를 가져옵니다. |
> | Microsoft.Resources/deployments/* | 배포 만들기 및 관리 |
> | Microsoft.Resources/subscriptions/resourceGroups/read | 리소스 그룹을 가져오거나 나열합니다. |
> | Microsoft.Scheduler/jobcollections/* | 스케줄러 작업 컬렉션 만들기 및 관리 |
> | Microsoft.Support/* | 지원 티켓 만들기 및 업데이트 |
> | **NotActions** |  |
> | *없음* |  |
> | **DataActions** |  |
> | *없음* |  |
> | **NotDataActions** |  |
> | *없음* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Scheduler job collections, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "name": "188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Scheduler/jobcollections/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Scheduler Job Collections Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="next-steps"></a>다음 단계

- [리소스 공급자와 서비스 일치](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Azure 사용자 지정 역할](custom-roles.md)
- [Azure Security Center의 권한](../security-center/security-center-permissions.md)
