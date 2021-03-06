---
title: Azure Synapse Analytics의 공유 메타데이터 테이블
description: Azure Synapse Analytics는 Apache Spark에서 테이블을 만들면 데이터를 복제하지 않고도 SQL 주문형(미리 보기) 및 SQL 풀 엔진에서 액세스할 수 있는 공유 메타데이터 모델을 제공합니다.
services: sql-data-warehouse
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 7c1951c772dcd2f49f4f7c09021f69193af0a87e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420837"
---
# <a name="azure-synapse-analytics-shared-metadata-tables"></a>Azure Synapse Analytics 공유 메타데이터 테이블

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Azure Synapse Analytics를 사용하면 서로 다른 작업 영역 컴퓨팅 엔진에서 Apache Spark 풀(미리 보기), SQL 주문형(미리 보기) 엔진 및 SQL 풀 간에 데이터베이스와 Parquet 지원 테이블을 공유할 수 있습니다.

Spark 작업을 통해 데이터베이스가 만들어지면 Parquet를 스토리지 형식으로 사용하는 Spark를 사용하여 테이블을 이 데이터베이스에 만들 수 있습니다. 이러한 테이블은 모든 Azure Synapse 작업 영역 Spark 풀에서 쿼리하는 데 즉시 사용할 수 있게 됩니다. 권한이 있는 모든 Spark 작업에서도 사용할 수 있습니다.

또한 Spark에서 만든 관리형 및 외부 테이블은 SQL 주문형의 해당 동기화된 데이터베이스 및 해당 메타데이터 동기화를 사용하도록 설정된 SQL 풀의 해당 `$` 접두사가 있는 스키마에서 동일한 이름의 외부 테이블로 사용할 수 있습니다. [SQL에서 Spark 테이블을 공개](#exposing-a-spark-table-in-sql)하면 테이블 동기화에 대한 자세한 정보가 제공됩니다.

테이블이 SQL 주문형 및 SQL 풀에 비동기적으로 동기화되므로 해당 테이블이 표시될 때까지 지연됩니다.

테이블은 외부 테이블, 데이터 원본 및 파일 형식에 매핑됩니다.

## <a name="manage-a-spark-created-table"></a>Spark에서 만든 테이블 관리

Spark를 사용하여 Spark에서 만든 데이터베이스를 관리합니다. 예를 들어 Spark 풀 작업을 통해 삭제하고 Spark에서 테이블을 만듭니다.

SQL 주문형에서 이러한 데이터베이스의 개체를 만들거나 데이터베이스를 삭제하려고 하면 이 작업이 성공하지만 원본 Spark 데이터베이스는 변경되지 않습니다.

SQL 풀에서 동기화된 스키마를 삭제하거나 테이블을 이 풀에 만들려고 하면 Azure에서 오류를 반환합니다.

## <a name="exposing-a-spark-table-in-sql"></a>SQL에서 Spark 테이블 공개

### <a name="which-spark-tables-are-shared"></a>공유되는 Spark 테이블

Spark는 Azure Synapse를 통해 SQL에서 자동으로 공개되는 두 가지 유형의 테이블을 제공합니다.

- 관리형 테이블

  Spark는 TEXT, CSV, JSON, JDBC, PARQUET, ORC, HIVE, DELTA 및 LIBSVM과 같은 관리형 테이블에 데이터를 저장하는 방법에 대한 다양한 옵션을 제공합니다. 이러한 파일은 일반적으로 관리형 테이블 데이터가 저장된 `warehouse` 디렉터리에 저장됩니다.

- 외부 테이블

  Spark는 `LOCATION` 옵션을 제공하거나 Hive 형식을 사용하여 기존 데이터에 대한 외부 테이블을 만드는 방법도 제공합니다. 이러한 외부 테이블은 Parquet를 포함하여 다양한 데이터 형식일 수 있습니다.

Azure Synapse는 현재 데이터를 Parquet 형식으로 저장하는 관리형 테이블 및 외부 Spark 테이블만 SQL 엔진과 공유합니다. 다른 형식으로 지원되는 테이블은 자동으로 동기화되지 않습니다. SQL 엔진에서 테이블의 기본 형식을 지원하는 경우 이러한 테이블을 사용자의 SQL 데이터베이스에 있는 외부 테이블로 명시적으로 동기화할 수 있습니다.

### <a name="how-are-spark-tables-shared"></a>Spark 테이블을 공유하는 방법

공유 가능한 관리형 및 외부 Spark 테이블은 SQL 엔진에서 다음과 같은 속성이 있는 외부 테이블로 공개됩니다.

- SQL 외부 테이블의 데이터 원본은 Spark 테이블의 위치 폴더를 나타내는 데이터 원본입니다.
- SQL 외부 테이블의 파일 형식은 Parquet입니다.
- SQL 외부 테이블의 액세스 자격 증명은 통과입니다.

모든 Spark 테이블 이름은 유효한 SQL 테이블 이름이고 모든 Spark 열 이름은 유효한 SQL 열 이름이므로 Spark 테이블 및 열 이름은 SQL 외부 테이블에 사용됩니다.

Spark 테이블은 Synapse SQL 엔진과 다른 데이터 형식을 제공합니다. 다음 표에서는 Spark 테이블 데이터 형식을 SQL 형식에 매핑하고 있습니다.

| Spark 데이터 형식 | SQL 데이터 형식 | 주석 |
|---|---|---|
| `byte`      | `smallint`       ||
| `short`     | `smallint`       ||
| `integer`   |    `int`            ||
| `long`      |    `bigint`         ||
| `float`     | `real`           |<!-- need precision and scale-->|
| `double`    | `float`          |<!-- need precision and scale-->|
| `decimal`      | `decimal`        |<!-- need precision and scale-->|
| `timestamp` |    `datetime2`      |<!-- need precision and scale-->|
| `date`      | `date`           ||
| `string`    |    `varchar(max)`   | `Latin1_General_CP1_CI_AS_UTF8` 데이터 정렬 사용 |
| `binary`    |    `varbinary(max)` ||
| `boolean`   |    `bit`            ||
| `array`     |    `varchar(max)`   | `Latin1_General_CP1_CI_AS_UTF8` 데이터 정렬을 사용하여 JSON으로 직렬화 |
| `map`       |    `varchar(max)`   | `Latin1_General_CP1_CI_AS_UTF8` 데이터 정렬을 사용하여 JSON으로 직렬화 |
| `struct`    |    `varchar(max)`   | `Latin1_General_CP1_CI_AS_UTF8` 데이터 정렬을 사용하여 JSON으로 직렬화 |

<!-- TODO: Add precision and scale to the types mentioned above -->

## <a name="security-model"></a>보안 모델

Spark 데이터베이스와 테이블뿐만 아니라 SQL 엔진에서 동기화된 해당 표현도 기본 스토리지 수준에서 보호됩니다. 현재 개체 자체에 대한 권한이 없으므로 개체는 개체 탐색기에서 볼 수 있습니다.

관리형 테이블을 만드는 보안 주체는 해당 테이블의 소유자로 간주되며 기본 폴더 및 파일 외에도 테이블에 대한 모든 권한을 갖습니다. 또한 데이터베이스 소유자는 자동으로 테이블의 공동 소유자가 됩니다.

인증 통과를 사용하여 Spark 또는 SQL 외부 테이블을 만드는 경우 데이터는 폴더 및 파일 수준에서만 보호됩니다. 다른 사용자가 이 유형의 외부 테이블을 쿼리하면 쿼리 제출자의 보안 ID가 파일 시스템으로 전달되어 액세스 권한이 확인됩니다.

폴더 및 파일에 대한 권한을 설정하는 방법에 대한 자세한 내용은 [Synapse Analytics 공유 데이터베이스](database.md)를 참조하세요.

## <a name="examples"></a>예

### <a name="create-a-managed-table-backed-by-parquet-in-spark-and-query-from-sql-on-demand"></a>Spark에서 Parquet로 지원되는 관리형 테이블 만들기 및 SQL 주문형에서 쿼리

이 시나리오에는 `mytestdb`라는 Spark 데이터베이스가 있습니다. [Spark 데이터베이스 만들기 및 연결 - SQL 주문형](database.md#create--connect-to-spark-database---sql-on-demand)을 참조하세요.

다음 명령을 실행하여 SparkSQL을 통해 관리형 Spark 테이블을 만듭니다.

```sql
    CREATE TABLE mytestdb.myParquetTable(id int, name string, birthdate date) USING Parquet
```

그러면 `myParquetTable` 테이블이 `mytestdb` 데이터베이스에 만들어집니다. 잠시 지연된 후에 SQL 주문형에서 테이블을 볼 수 있습니다. 예를 들어 SQL 주문형에서 다음 명령문을 실행합니다.

```sql
    USE mytestdb;
    SELECT * FROM sys.tables;
```

결과에 `myParquetTable`이 포함되어 있는지 확인합니다.

>[!NOTE]
>Parquet를 해당 스토리지 형식으로 사용하지 않는 테이블은 동기화되지 않습니다.

다음으로, 몇 가지 값을 Spark의 테이블에 삽입합니다(예: C# Notebook에서 다음 C# Spark 문 사용).

```csharp
using Microsoft.Spark.Sql.Types;

var data = new List<GenericRow>();

data.Add(new GenericRow(new object[] { 1, "Alice", new Date(2010, 1, 1)}));
data.Add(new GenericRow(new object[] { 2, "Bob", new Date(1990, 1, 1)}));

var schema = new StructType
    (new List<StructField>()
        {
            new StructField("id", new IntegerType()),
            new StructField("name", new StringType()),
            new StructField("birthdate", new DateType())
        }
    );

var df = spark.CreateDataFrame(data, schema);
df.Write().Mode(SaveMode.Append).InsertInto("mytestdb.myParquetTable");
```

이제 다음과 같이 SQL 주문형에서 데이터를 읽을 수 있습니다.

```sql
SELECT * FROM mytestdb.dbo.myParquetTable WHERE name = 'Alice';
```

다음 행을 결과로 가져옵니다.

```
id | name | birthdate
---+-------+-----------
1 | Alice | 2010-01-01
```

### <a name="creating-an-external-table-backed-by-parquet-in-spark-and-querying-it-from-sql-on-demand"></a>Spark에서 Parquet로 지원되는 외부 테이블 만들기 및 SQL 주문형에서 쿼리

다음 예제에서는 관리형 테이블에 대해 이전 예제에서 만든 Parquet 데이터 파일에 외부 Spark 테이블을 만듭니다.

예를 들어 SparkSQL을 사용하여 다음을 실행합니다.

```sql
CREATE TABLE mytestdb.myExternalParquetTable
    USING Parquet
    LOCATION "abfss://<fs>@arcadialake.dfs.core.windows.net/synapse/workspaces/<synapse_ws>/warehouse/mytestdb.db/myparquettable/"
```

`<fs>` 자리 표시자를 작업 영역 기본 파일 시스템인 파일 시스템 이름으로 바꾸고, `<synapse_ws>` 자리 표시자를 이 예제를 실행하는 데 사용하는 Synapse 작업 영역의 이름으로 바꿉니다.

이전 예제에서는 `myExtneralParquetTable` 테이블을 `mytestdb` 데이터베이스에 만듭니다. 잠시 지연된 후에 SQL 주문형에서 테이블을 볼 수 있습니다. 예를 들어 SQL 주문형에서 다음 명령문을 실행합니다.

```sql
USE mytestdb;
SELECT * FROM sys.tables;
```

결과에 `myExternalParquetTable`이 포함되어 있는지 확인합니다.

이제 다음과 같이 SQL 주문형에서 데이터를 읽을 수 있습니다.

```sql
SELECT * FROM mytestdb.dbo.myExternalParquetTable WHERE name = 'Alice';
```

다음 행을 결과로 가져옵니다.

```
id | name | birthdate
---+-------+-----------
1 | Alice | 2010-01-01
```

### <a name="querying-spark-tables-in-a-sql-pool"></a>SQL 풀에서 Spark 테이블 쿼리

이전 예제에서 만든 테이블을 사용하는 경우 이제 메타데이터 동기화를 사용하도록 설정하는 `mysqlpool`이라는 SQL 풀을 작업 영역에 만들거나 [SQL 풀에서 Spark 데이터베이스 공개](database.md#exposing-a-spark-database-in-a-sql-pool)에서 이미 만든 풀을 사용합니다.

`mysqlpool` SQL 풀에 대해 다음 문을 실행합니다.

```sql
SELECT * FROM sys.tables;
```

`$mytestdb` 스키마에 `myParquetTable` 및 `myExternalParquetTable` 테이블이 표시되는지 확인합니다.

이제 다음과 같이 SQL 주문형에서 데이터를 읽을 수 있습니다.

```sql
SELECT * FROM [$mytestdb].myParquetTable WHERE name = 'Alice';
SELECT * FROM [$mytestdb].myExternalParquetTable WHERE name = 'Alice';
```

위의 SQL 주문형과 동일한 결과를 가져옵니다.

## <a name="next-steps"></a>다음 단계

- [Azure Synapse Analytics의 공유 메타데이터에 대한 자세한 정보](overview.md)
- [Azure Synapse Analytics의 공유 메타데이터 테이블에 대한 자세한 정보](table.md)


