---
title: SQL 주문형(미리 보기) 스토리지 계정 액세스 제어
description: SQL 주문형(미리 보기)에서 Azure Storage에 액세스하는 방법과 Azure Synapse Analytics에서 SQL 주문형 스토리지 액세스를 제어하는 방법을 설명합니다.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 2d5d508afe81975cbeda448b497a098e8a3bbcf3
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589281"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview"></a>SQL 주문형(미리 보기) 스토리지 계정 액세스 제어

SQL 주문형 쿼리는 Azure Storage에서 직접 파일을 읽습니다. Azure Storage의 파일에 액세스할 수 있는 권한은 두 가지 수준으로 제어됩니다.
- **스토리지 수준** - 사용자에게 기본 스토리지 파일에 액세스할 수 있는 권한이 있어야 합니다. 스토리지 관리자가 Azure AD 보안 주체에게 파일 읽기/쓰기를 허용하거나 스토리지에 액세스하는 데 사용할 SAS 키를 생성해야 합니다.
- **SQL 서비스 수준** - 사용자에게 [외부 테이블](develop-tables-external-tables.md)의 데이터를 읽을 수 있는 `SELECT` 권한이나 `OPENROWSET`를 실행할 수 있는 `ADMINISTER BULK ADMIN` 권한이 있고 스토리지에 액세스하는 데 사용할 자격 증명을 사용할 권한도 있어야 합니다.

이 문서에서는 사용할 수 있는 자격 증명의 유형과 SQL 및 Azure AD 사용자에 대한 자격 증명 조회가 적용되는 방법에 대해 설명합니다.

## <a name="supported-storage-authorization-types"></a>지원되는 스토리지 권한 부여 유형

SQL 주문형 리소스에 로그인한 사용자는 Azure Storage 파일을 공개적으로 사용할 수 없는 경우 해당 파일에 액세스하고 쿼리할 수 있는 권한이 있어야 합니다. 지원되는 세 가지 권한 부여 유형은 다음과 같습니다.

- [공유 액세스 서명](?tabs=shared-access-signature)
- [사용자 ID](?tabs=user-identity)
- [관리 ID](?tabs=managed-identity)

> [!NOTE]
> [Azure AD 통과](#force-azure-ad-pass-through)는 작업 영역을 만들 때의 기본 동작입니다. 이를 사용하는 경우 Azure AD 로그인을 사용하여 액세스하는 각 스토리지 계정에 대한 자격 증명을 만들 필요가 없습니다. [이 동작을 사용하지 않도록 설정](#disable-forcing-azure-ad-pass-through)할 수 있습니다.

### <a name="shared-access-signature"></a>[공유 액세스 서명](#tab/shared-access-signature)

**SAS(공유 액세스 서명)** 는 스토리지 계정의 리소스에 대한 위임된 액세스를 제공합니다. SAS를 사용하면 고객이 계정 키를 공유하지 않고 스토리지 계정의 리소스에 대한 액세스 권한을 클라이언트에 부여할 수 있습니다. SAS는 유효성 간격, 부여된 권한, 허용되는 IP 주소 범위 및 허용되는 프로토콜(https/http)을 포함하여 SAS를 사용하는 클라이언트에 부여하는 액세스 유형에 대한 세부적인 제어를 제공합니다.

**Azure Portal > 스토리지 계정 > 공유 액세스 서명 > 권한 구성 > SAS 및 연결 문자열 생성**으로 차례로 이동하여 SAS 토큰을 가져올 수 있습니다.

> [!IMPORTANT]
> SAS 토큰이 생성되면 토큰의 시작 부분에 물음표('?')가 포함됩니다. SQL 주문형에서 토큰을 사용하려면 자격 증명을 만들 때 물음표('')를 제거해야 합니다. 다음은 그 예입니다.
>
> SAS token: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D

SAS 토큰을 사용한 액세스가 가능하려면 데이터베이스 범위 또는 서버 범위 자격 증명을 만들어야 합니다.

### <a name="user-identity"></a>[사용자 ID](#tab/user-identity)

**사용자 ID**("통과"라고도 함)는 주문형 SQL에 로그인한 Azure AD 사용자의 ID를 사용하여 데이터 액세스 권한을 부여하는 권한 부여 유형입니다. 데이터에 액세스하기 전에 Azure Storage 관리자가 Azure AD 사용자에게 권한을 부여해야 합니다. 위의 표에서 설명한 대로 SQL 사용자 유형에는 지원되지 않습니다.

> [!IMPORTANT]
> 이 ID를 사용하여 데이터에 액세스하려면 Storage Blob 데이터 소유자/기여자/읽기 권한자 역할이 있어야 합니다.
> 스토리지 계정의 소유자인 경우에도 Storage Blob 데이터 역할 중 하나에 자신을 추가해야 합니다.
>
> Azure Data Lake Store Gen2의 액세스 제어에 대한 자세한 내용은 [Azure Data Lake Storage Gen2의 액세스 제어](../../storage/blobs/data-lake-storage-access-control.md) 문서를 검토하세요.
>

Azure AD 사용자가 자신의 ID를 사용하여 스토리지에 액세스할 수 있도록 Azure AD 통과 인증을 명시적으로 활성화해야 합니다.

#### <a name="force-azure-ad-pass-through"></a>Azure AD 통과 강제 적용

Azure AD 통과를 강제 적용하는 것은 Azure Synapse 작업 영역을 프로비저닝하는 중에 자동으로 만들어지는 특수 CREDENTIAL NAME인 `UserIdentity`를 통해 수행되는 기본 동작입니다. 다른 자격 증명이 있음에도 불구하고 모든 Azure AD 로그인의 각 쿼리에 Azure AD 통과를 사용하도록 강제합니다.

> [!NOTE]
> Azure AD 통과는 기본 동작입니다. 따라서 AD 로그인을 통해 액세스하는 각 스토리지 계정에 대한 자격 증명을 만들 필요가 없습니다.

[Azure AD 통과 강제 적용을 각 쿼리에 사용하지 않도록 설정](#disable-forcing-azure-ad-pass-through)한 후에 이를 사용하도록 다시 설정하려면 다음을 실행합니다.

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Azure AD 통과를 특정 사용자에 강제 적용하도록 설정하려면 해당 특정 사용자에게 `UserIdentity` 자격 증명에 대한 REFERENCE 권한을 부여할 수 있습니다. 다음 예제에서는 Azure AD 통과를 user_name에 강제 적용하도록 설정합니다.

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

#### <a name="disable-forcing-azure-ad-pass-through"></a>Azure AD 통과 강제 적용 사용 안 함

[Azure AD 통과를 각 쿼리에 강제 적용](#force-azure-ad-pass-through)하지 않도록 설정할 수 있습니다. 사용하지 않도록 설정하려면 다음을 사용하여 `Userdentity` 자격 증명을 삭제합니다.

```sql
DROP CREDENTIAL [UserIdentity];
```

사용하도록 다시 설정하려면 [Azure AD 통과 강제 적용](#force-azure-ad-pass-through) 섹션을 참조하세요.

### <a name="managed-identity"></a>[관리 ID](#tab/managed-identity)

**관리 ID**는 MSI라고도 합니다. Azure 서비스를 SQL 주문형에 제공하는 Azure AD(Azure Active Directory)의 기능입니다. 또한 Azure AD에서 관리 ID를 자동으로 배포합니다. 이 ID는 Azure Storage에서 데이터 액세스에 대한 요청을 승인하는 데 사용될 수 있습니다.

데이터에 액세스하기 전에 Azure Storage 관리자가 데이터에 액세스할 수 있는 권한을 관리 ID에 부여해야 합니다. 관리 ID에 대한 권한 부여는 다른 Azure AD 사용자에게 권한을 부여하는 것과 동일한 방식으로 수행됩니다.

### <a name="anonymous-access"></a>[익명 액세스](#tab/public-access)

[익명 액세스를 허용](/azure/storage/blobs/storage-manage-access-to-resources.md)하는 Azure 스토리지 계정에 있는 공개적으로 사용 가능한 파일에 액세스할 수 있습니다.

---

### <a name="supported-authorization-types-for-databases-users"></a>데이터베이스 사용자에 대해 지원되는 권한 부여 유형

아래 표에서 사용 가능한 권한 부여 유형을 확인할 수 있습니다.

| 권한 부여 유형                    | *SQL 사용자*    | *Azure AD 사용자*     |
| ------------------------------------- | ------------- | -----------    |
| [사용자 ID](?tabs=user-identity#supported-storage-authorization-types)       | 지원되지 않음 | 지원됨      |
| [SAS](?tabs=shared-access-signature#supported-storage-authorization-types)       | 지원됨     | 지원됨      |
| [관리 ID](?tabs=managed-identity#supported-storage-authorization-types) | 지원되지 않음 | 지원됨      |

### <a name="supported-storages-and-authorization-types"></a>지원되는 스토리지 및 권한 부여 유형

사용할 수 있는 권한 부여 및 Azure Storage 유형의 조합은 다음과 같습니다.

|                     | Blob Storage   | ADLS Gen1        | ADLS Gen2     |
| ------------------- | ------------   | --------------   | -----------   |
| *SAS*               | 지원됨      | 지원되지 않음   | 지원됨     |
| *관리 ID* | 지원됨      | 지원됨        | 지원됨     |
| *사용자 ID*    | 지원됨      | 지원됨        | 지원됨     |

## <a name="credentials"></a>자격 증명

Azure Storage에 있는 파일을 쿼리하려면 SQL 주문형 엔드포인트에 인증 정보가 포함된 자격 증명이 필요합니다. 두 가지 유형의 자격 증명이 사용됩니다.
- 서버 수준 자격 증명은 `OPENROWSET` 함수를 사용하여 실행되는 임시 쿼리에 사용됩니다. 자격 증명 이름은 스토리지 URL과 일치해야 합니다.
- 데이터베이스 범위 자격 증명은 외부 테이블에 사용됩니다. 외부 테이블은 스토리지에 액세스하는 데 사용해야 하는 자격 증명으로 `DATA SOURCE`를 참조합니다.

사용자가 자격 증명을 만들거나 삭제할 수 있도록 관리자는 사용자에게 GRANT/DENY ALTER ANY CREDENTIAL 권한을 부여할 수 있습니다.

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

외부 스토리지에 액세스하는 데이터베이스 사용자는 자격 증명을 사용할 수 있는 권한이 있어야 합니다.

### <a name="grant-permissions-to-use-credential"></a>자격 증명을 사용할 수 있는 권한 부여

자격 증명을 사용하려면 사용자에게 특정 자격 증명에 대한 `REFERENCES` 권한이 있어야 합니다. specific_user의 storage_credential에 대한 `REFERENCES` 권한을 부여하려면 다음을 실행합니다.

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

원활한 Azure AD 통과 환경을 보장하기 위해 기본적으로 모든 사용자에게 `UserIdentity` 자격 증명을 사용할 수 있는 권한이 있습니다. 이 작업은 Azure Synapse 작업 영역을 프로비저닝할 때 다음 명령문을 자동으로 실행하여 수행됩니다.

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="server-scoped-credential"></a>서버 범위 자격 증명

서버 범위 자격 증명은 SQL 로그인이 `DATA_SOURCE` 없이 `OPENROWSET` 함수를 호출하여 일부 스토리지 계정의 파일을 읽을 때 사용됩니다. 서버 범위 자격 증명의 이름은 Azure 스토리지의 URL과 **반드시** 일치해야 합니다. 자격 증명은 [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)을 실행하여 추가됩니다. CREDENTIAL NAME 인수를 제공해야 합니다. Storage의 데이터에 대한 경로의 일부 또는 전체 경로와 일치해야 합니다(아래 참조).

> [!NOTE]
> FOR CRYPTOGRAPHIC PROVIDER 인수는 지원되지 않습니다.

서버 수준의 자격 증명 이름은 스토리지 계정의 전체 경로(및 선택적으로 컨테이너)와 일치해야 하며, 다음과 같은 형식이어야 합니다. `<prefix>://<storage_account_path>/<storage_path>`. 스토리지 계정 경로는 다음 표에 설명되어 있습니다.

| 외부 데이터 원본       | 접두사 | 스토리지 계정 경로                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Storage         | https  | <storage_account>.blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <storage_account>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <storage_account>.dfs.core.windows.net              |

> [!NOTE]
> [Azure AD 통과를 강제 적용](?tabs=user-identity#force-azure-ad-pass-through)하는 특별한 서버 수준 CREDENTIAL `UserIdentity`가 있습니다.

서버 범위 자격 증명을 사용하면 다음 인증 유형을 사용하여 Azure 스토리지에 액세스할 수 있습니다.

### <a name="shared-access-signature"></a>[공유 액세스 서명](#tab/shared-access-signature)

다음 스크립트는 `OPENROWSET` 함수가 SAS 토큰을 사용하여 Azure 스토리지의 파일에 액세스하는 데 사용할 수 있는 서버 수준 자격 증명을 만듭니다. 이 자격 증명을 만들면 `OPENROWSET` 함수를 실행하는 SQL 보안 주체가 자격 증명 이름의 URL과 일치하는 Azure 스토리지에서 SAS 키로 보호된 파일을 읽을 수 있습니다.

<*mystorageaccountname*>을 실제 스토리지 계정 이름으로 교환하고, <*mystorageaccountcontainername*>을 실제 컨테이너 이름으로 교환합니다.

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="user-identity"></a>[사용자 ID](#tab/user-identity)

다음 스크립트는 사용자가 Azure AD ID를 사용하여 가장할 수 있는 서버 수준 자격 증명을 만듭니다.

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

### <a name="managed-identity"></a>[관리 ID](#tab/managed-identity)

다음 스크립트는 `OPENROWSET` 함수가 작업 영역 관리 ID를 사용하여 Azure 스토리지의 파일에 액세스하는 데 사용할 수 있는 서버 수준 자격 증명을 만듭니다.

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='Managed Identity'
```

### <a name="public-access"></a>[공용 액세스](#tab/public-access)

다음 스크립트는 `OPENROWSET` 함수가 공개적으로 사용 가능한 Azure 스토리지의 파일에 액세스하는 데 사용할 수 있는 서버 수준 자격 증명을 만듭니다. 이 자격 증명을 만들면 `OPENROWSET` 함수를 실행하는 SQL 보안 주체가 자격 증명 이름의 URL과 일치하는 Azure 스토리지에서 공개적으로 사용 가능한 파일을 읽을 수 있습니다.

<*mystorageaccountname*>을 실제 스토리지 계정 이름으로 교환하고, <*mystorageaccountcontainername*>을 실제 컨테이너 이름으로 교환해야 합니다.

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = '';
GO
```
---

## <a name="database-scoped-credential"></a>데이터베이스 범위 자격 증명

데이터베이스 범위 자격 증명은 보안 주체가 `DATA_SOURCE`를 사용하여 `OPENROWSET` 함수를 호출하거나 공용 파일에 액세스하지 않는 [외부 테이블](develop-tables-external-tables.md)에서 데이터를 선택하는 경우에 사용됩니다. 데이터베이스 범위 자격 증명은 스토리지 위치를 정의하는 DATA SOURCE에서 명시적으로 사용되기 때문에 스토리지 계정 이름과 일치하지 않아도 됩니다.

데이터베이스 범위 자격 증명을 사용하면 다음 인증 유형을 사용하여 Azure 스토리지에 액세스할 수 있습니다.

### <a name="shared-access-signature"></a>[공유 액세스 서명](#tab/shared-access-signature)

다음 스크립트는 자격 증명에 지정된 SAS 토큰을 사용하여 스토리지의 파일에 액세스하는 데 사용되는 자격 증명을 만듭니다.

```sql
CREATE DATABASE SCOPED CREDENTIAL [SasToken]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="azure-ad-identity"></a>[Azure AD ID](#tab/user-identity)

다음 스크립트는 [외부 테이블](develop-tables-external-tables.md)에 사용되는 데이터베이스 범위 자격 증명 및 자체 Azure AD ID를 사용하여 스토리지 파일에 액세스하도록 자격 증명과 데이터 소스를 사용하는 `OPENROWSET` 함수를 만듭니다.

```sql
CREATE DATABASE SCOPED CREDENTIAL [AzureAD]
WITH IDENTITY = 'User Identity';
GO
```

### <a name="managed-identity"></a>[관리 ID](#tab/managed-identity)

다음 스크립트는 현재 Azure AD 사용자를 서비스의 관리 ID로 가장하는 데 사용할 수 있는 데이터베이스 범위 자격 증명을 만듭니다. 

```sql
CREATE DATABASE SCOPED CREDENTIAL [SynapseIdentity]
WITH IDENTITY = 'Managed Identity';
GO
```

데이터베이스 범위 자격 증명은 스토리지 위치를 정의하는 DATA SOURCE에서 명시적으로 사용되기 때문에 스토리지 계정 이름과 일치하지 않아도 됩니다.

### <a name="public-access"></a>[공용 액세스](#tab/public-access)

데이터베이스 범위 자격 증명은 공개적으로 사용 가능한 파일에 대한 액세스를 허용하는 데 필요하지 않습니다. Azure Storage에서 공개적으로 사용 가능한 파일에 액세스하려면 [데이터베이스 범위 자격 증명 없이 데이터 소스](develop-tables-external-tables.md?tabs=sql-ondemand#example-for-create-external-data-source)를 만듭니다.

---

데이터베이스 범위 자격 증명은 외부 데이터 소스에서 이 스토리지에 액세스하는 데 사용할 인증 방법을 지정하는 데 사용됩니다.

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples',
          CREDENTIAL = <name of database scoped credential> 
)
```

## <a name="examples"></a>예

**공개적으로 사용 가능한 데이터 소스에 액세스**

다음 스크립트를 사용하여 공개적으로 사용 가능한 데이터 소스에 액세스하는 테이블을 만듭니다.

```sql
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO
CREATE EXTERNAL DATA SOURCE publicData
WITH (    LOCATION   = 'https://****.blob.core.windows.net/public-access' )
GO

CREATE EXTERNAL TABLE dbo.userPublicData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [publicData], FILE_FORMAT = [SynapseParquetFormat] )
```

데이터베이스 사용자는 데이터 소스를 참조하는 [OPENROWSET](develop-openrowset.md) 함수나 외부 테이블을 사용하여 데이터 소스에서 파일의 콘텐츠를 읽을 수 있습니다.

```sql
SELECT TOP 10 * FROM dbo.userPublicData;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

**자격 증명을 사용하여 데이터 소스에 액세스**

다음 스크립트를 수정하여 SAS 토큰, 사용자의 Azure AD ID 또는 작업 영역의 관리 ID를 사용하여 Azure 스토리지에 액세스하는 외부 테이블을 만듭니다.

```sql
-- Create master key in databases with some password (one-off per database)
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Y*********0'
GO

-- Create databases scoped credential that use User Identity, Managed Identity, or SAS. User needs to create only database-scoped credentials that should be used to access data source:

CREATE DATABASE SCOPED CREDENTIAL MyIdentity WITH IDENTITY = 'User Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL WorkspaceIdentity WITH IDENTITY = 'Managed Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL SasCredential WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2019-10-1********ZVsTOL0ltEGhf54N8KhDCRfLRI%3D'

-- Create data source that one of the credentials above, external file format, and external tables that reference this data source and file format:

CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO

CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples'
-- Uncomment one of these options depending on authentication method that you want to use to access data source:
--,CREDENTIAL = MyIdentity 
--,CREDENTIAL = WorkspaceIdentity 
--,CREDENTIAL = SasCredential 
)

CREATE EXTERNAL TABLE dbo.userData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FILE_FORMAT = [SynapseParquetFormat] )

```

데이터베이스 사용자는 데이터 소스를 참조하는 [OPENROWSET](develop-openrowset.md) 함수나 [외부 테이블](develop-tables-external-tables.md)을 사용하여 데이터 소스에서 파일의 콘텐츠를 읽을 수 있습니다.

```sql
SELECT TOP 10 * FROM dbo.userdata;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

## <a name="next-steps"></a>다음 단계

아래 나열된 문서를 통해 다른 폴더 유형, 파일 형식을 쿼리하고, 보기를 만들고 사용하는 방법을 알아볼 수 있습니다.

- [단일 CSV 파일 쿼리](query-single-csv-file.md)
- [폴더 및 여러 CSV 파일 쿼리](query-folders-multiple-csv-files.md)
- [특정 파일 쿼리](query-specific-files.md)
- [Parquet 파일 쿼리](query-parquet-files.md)
- [보기 만들기 및 사용](create-use-views.md)
- [JSON 파일 쿼리](query-json-files.md)
- [Parquet 중첩 형식 쿼리](query-parquet-nested-types.md)
