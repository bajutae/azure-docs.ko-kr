---
title: 파일 포함
description: 포함 파일
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67182556"
---
Azure Batch에서 실행 중인 태스크는 실행 시 출력 데이터를 생성할 수 있습니다. 작업의 다른 태스크, 작업을 실행한 클라이언트 애플리케이션 또는 둘 다에서 검색할 수 있도록 태스크 출력 데이터는 종종 저장할 필요가 있습니다. 태스크는 Batch 컴퓨팅 노드의 파일 시스템에 출력 데이터를 쓰지만, 노드 이미지를 다시 만들거나 노드에서 풀을 종료할 때 해당 노드의 모든 데이터가 손실됩니다. 또한 태스크에는 파일 보존 기간이 있을 수 있으며, 그 후에는 해당 태스크에서 만든 파일이 삭제됩니다. 이러한 이유로 나중에 [Azure Storage](https://docs.microsoft.com/azure/storage/)와 같은 데이터 저장소에 필요한 태스크 출력을 유지하는 것이 중요합니다.

Batch의 스토리지 계정 옵션은 [Batch 기능 개요](../articles/batch/batch-api-basics.md#azure-storage-account)를 참조하세요.
