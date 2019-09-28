---
title: 清單寫入器
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d00eebe4e562764e97794b3eb1b76ea96c2dc47c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374724"
---
# <a name="list-writers"></a>清單寫入器



列出系統上的寫入器。 如果使用時不含參數，[**清單**] 預設會顯示**清單中繼資料**的輸出。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
list writers [metadata | detailed | status]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|中繼資料|列出寫入器的身分識別和狀態，並顯示中繼資料，例如元件詳細資料和排除的檔案。 這是預設參數。|
|詳見|會列出與**中繼資料**相同的資訊，但**詳細**說明包含所有元件的完整檔案清單。|
|status|只會列出已註冊的寫入器的身分識別與狀態。|

## <a name="BKMK_examples"></a>典型

若只要列出寫入器的身分識別和狀態，請輸入：
```
list writers status
```
類似下列顯示的輸出：
```
Listing writer status ...
* WRITER "System Writer"
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER "Shadow Copy Optimization Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
...
...
...
* WRITER "Registry Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed. 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)