---
title: Azure Data Factory를 사용 하 여 PostgreSQL에서 데이터 복사
description: Azure Data Factory 파이프라인의 복사 작업을 사용하여 PostgreSQL에서 지원되는 싱크 데이터 저장소로 데이터를 복사하는 방법에 대해 알아봅니다.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: jingwang
ms.openlocfilehash: 6d10e7b9b24817eb738172bd0f2d2c3e7f8f2cbf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416758"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Azure Data Factory를 사용하여 PostgreSQL에서 데이터 복사
> [!div class="op_single_selector" title1="사용 중인 Data Factory 서비스 버전을 선택합니다."]
> * [버전 1](v1/data-factory-onprem-postgresql-connector.md)
> * [현재 버전](connector-postgresql.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


이 문서에서는 Azure Data Factory의 복사 작업을 사용하여 PostgreSQL 데이터베이스에서 데이터를 복사하는 방법을 설명합니다. 이 문서는 복사 작업에 대한 일반적인 개요를 제공하는 [복사 작업 개요](copy-activity-overview.md) 문서를 기반으로 합니다.

## <a name="supported-capabilities"></a>지원되는 기능

이 PostgreSQL 커넥터는 다음과 같은 작업에 대해 지원 됩니다.

- [지원 되는 원본/싱크 매트릭스](copy-activity-overview.md) 를 사용 하 여 [복사 작업](copy-activity-overview.md)
- [조회 작업](control-flow-lookup-activity.md)

PostgreSQL 데이터베이스에서 지원되는 모든 싱크 데이터 저장소로 데이터를 복사할 수 있습니다. 복사 작업의 원본/싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats) 표를 참조하세요.

특히 이 PostgreSQL 커넥터는 PostgreSQL **버전 7.4 이상**을 지원합니다.

## <a name="prerequisites"></a>전제 조건

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Integration Runtime은 버전 3.7부터 시작하는 기본 제공 PostgreSQL 드라이버를 제공하므로 수동으로 드라이버를 설치할 필요가 없습니다.

## <a name="getting-started"></a>시작

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 PostgreSQL 커넥터에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

PostgreSQL 연결된 서비스에 다음 속성이 지원됩니다.

| 속성 | Description | 필수 |
|:--- |:--- |:--- |
| type | 형식 속성은 **PostgreSql**로 설정해야 합니다. | 예 |
| connectionString | Azure Database for PostgreSQL에 연결하는 ODBC 연결 문자열입니다. <br/>Azure Key Vault에 암호를 넣고, 연결 문자열에서 `password` 구성을 끌어올 수도 있습니다. 자세한 내용은 다음 샘플 및 [Azure Key Vault에 자격 증명 저장](store-credentials-in-key-vault.md) 문서를 참조하세요. | 예 |
| connectVia | 데이터 저장소에 연결 하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md) 입니다. [전제 조건](#prerequisites) 섹션에서 자세히 알아보세요. 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. |아니요 |

일반적인 연결 문자열은 `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`입니다. 사례에 따라 다음과 같은 더 많은 속성을 설정할 수 있습니다.

| 속성 | Description | 옵션 | 필수 |
|:--- |:--- |:--- |:--- |
| EncryptionMethod(EM)| 드라이버와 데이터베이스 서버 간에 전송되는 데이터를 암호화하기 위해 드라이버에서 사용하는 메서드입니다. 예:`EncryptionMethod=<0/1/6>;`| 0(암호화 없음)**(기본값)** / 1(SSL) / 6(RequestSSL) | 아니요 |
| ValidateServerCertificate(VSC) | SSL 암호화를 사용할 때(암호화 메서드=1) 데이터베이스 서버에서 보내는 인증서의 유효성을 드라이버가 검사하는지 여부를 결정합니다. 예:`ValidateServerCertificate=<0/1>;`| 0(사용 안 함)**(기본값)** / 1(사용) | 아니요 |

**예제:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**예제: Azure Key Vault에 암호 저장**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

다음 페이로드에서 PostgreSQL 연결된 서비스를 사용하는 경우, 있는 그대로도 지원되지만 앞으로 새 옵션을 사용하는 것이 좋습니다.

**이전 페이로드:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 세트 속성

데이터 집합 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 집합](concepts-datasets-linked-services.md) 문서를 참조 하세요. 이 섹션에서는 PostgreSQL 데이터 세트에서 지원하는 속성의 목록을 제공합니다.

PostgreSQL에서 데이터를 복사 하기 위해 지원 되는 속성은 다음과 같습니다.

| 속성 | Description | 필수 |
|:--- |:--- |:--- |
| type | 데이터 집합의 type 속성은 **PostgreSqlTable** 로 설정 해야 합니다. | 예 |
| 스키마 | 스키마의 이름입니다. |아니요(작업 원본에서 "query"가 지정된 경우)  |
| 테이블 | 테이블 이름입니다. |아니요(작업 원본에서 "query"가 지정된 경우)  |
| tableName | 스키마가 있는 테이블의 이름입니다. 이 속성은 이전 버전과의 호환성을 위해 지원 됩니다. 새 `schema` 워크 `table` 로드에 및를 사용 합니다. | 아니요(작업 원본에서 "query"가 지정된 경우) |

**예제**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "PostgreSqlTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

형식화 된 데이터 집합 `RelationalTable` 을 사용 하는 경우에는 계속 해 서 새 항목을 사용 하는 것이 좋습니다.

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 PostgreSQL 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="postgresql-as-source"></a>PostgreSQL을 원본으로

PostgreSQL에서 데이터를 복사 하기 위해 복사 작업 **원본** 섹션에서 다음 속성이 지원 됩니다.

| 속성 | Description | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 type 속성은 **PostgreSqlSource** 로 설정 해야 합니다. | 예 |
| Query | 사용자 지정 SQL 쿼리를 사용하여 데이터를 읽습니다. 예: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""` | 아니요(데이터 세트의 "tableName"이 지정된 경우) |

> [!NOTE]
> 스키마 및 테이블 이름은 대/소문자를 구분합니다. 쿼리에서 `""`(큰따옴표)로 묶습니다.

**예제:**

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "PostgreSqlSource",
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

형식화 된 소스를 `RelationalSource` 사용 하는 경우에는 계속 해 서 새 항목을 사용 하는 것이 좋습니다.

## <a name="lookup-activity-properties"></a>조회 작업 속성

속성에 대 한 자세한 내용을 보려면 [조회 작업](control-flow-lookup-activity.md)을 확인 하세요.


## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats)를 참조하세요.
