---
title: 릴리스 정보
description: Azure Synapse Analytics(작업 영역)에 대한 릴리스 정보
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 059e77c063d00ef850a171507ca2e06422ade426
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82191773"
---
# <a name="azure-synapse-analytics-preview-release-notes"></a>Azure Synapse Analytics(미리 보기) 릴리스 정보

이 문서에서는 Azure Synapse Analytics(작업 영역)의 제한 사항 및 문제에 대해 설명합니다. 관련 정보는 [Azure Synapse Analytics(작업 영역)란?](overview-what-is.md)을 참조하세요.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-synapse-workspaces"></a>Azure Synapse(작업 영역) 

### <a name="azure-synapse-cli"></a>Azure Synapse CLI

- 문제 및 고객에게 미치는 영향: SDK에서 만든 작업 영역은 Synapse Studio를 시작할 수 없습니다.

- 해결 방법: 다음 단계를 완료합니다. 
  1.    `az synapse workspace create`를 실행하여 작업 영역을 만듭니다.
  2.    `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")`를 실행하여 관리 ID를 추출합니다.
  3.    ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}`를 실행하여 스토리지 계정에 역할로 작업 영역을 추가합니다.
  4.    ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 `를 실행하여 방화벽 규칙을 추가합니다.

## <a name="next-steps"></a>다음 단계

* [작업 영역 만들기](quickstart-create-workspace.md)
* [Synapse Studio 사용](quickstart-synapse-studio.md)
* [SQL 풀 만들기](quickstart-create-sql-pool.md)
* [SQL 주문형 사용](quickstart-sql-on-demand.md)
* [Apache Spark 풀 만들기](quickstart-create-apache-spark-pool.md)