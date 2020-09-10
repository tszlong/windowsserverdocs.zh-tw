---
title: list writers
description: 清單寫入器命令的參考文章，此命令會列出系統上的寫入器。
ms.topic: reference
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b6e8fd300b2592a0ffee317f7d325482250fcfc6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639551"
---
# <a name="list-writers"></a>list writers

列出系統上的寫入器。 如果使用時不含參數， **清單** 預設會顯示 **清單中繼資料** 的輸出。

## <a name="syntax"></a>語法

```
list writers [metadata | detailed | status]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 中繼資料 | 列出寫入器的身分識別和狀態，並顯示元件詳細資料和排除的檔案等中繼資料。 這是預設參數。 |
| 詳細 | 列出與 **中繼資料**相同的資訊，但也包含所有元件的完整檔案清單。 |
| status | 僅列出已註冊之寫入器的身分識別和狀態。 |

### <a name="examples"></a>範例

若只要列出寫入器的身分識別和狀態，請輸入：

```
list writers status
```

類似下列顯示的輸出：

```
Listing writer status ...
* WRITER System Writer
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER Shadow Copy Optimization Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
* WRITER Registry Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)