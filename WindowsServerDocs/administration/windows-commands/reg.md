---
title: reg
description: Reg 命令的參考文章，可針對登錄子機碼資訊和登錄專案中的值執行作業。
ms.topic: reference
ms.assetid: c97496b2-d1ff-4887-b5d2-6e1524be465a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a39fc22c2fe845d8cbbb64cf751455316bdc8747
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639497"
---
# <a name="reg"></a>reg

針對登錄子機碼資訊和登錄專案中的值執行作業。

某些作業可讓您在本機或遠端電腦上查看或設定登錄專案，而其他作業則可讓您只設定本機電腦。 使用 **reg** 來設定遠端電腦的登錄，會限制您可以在某些作業中使用的參數。 檢查每項作業的語法和參數，確認它們可以在遠端電腦上使用。

> [!CAUTION]
> 除非您沒有替代方案，否則請勿直接編輯登錄。 登錄編輯程式會略過標準的保護，允許可能降低效能、損毀系統或甚至需要重新安裝 Windows 的設定。 您可以使用主控台或 Microsoft Management Console (MMC) 中的程式，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。

## <a name="syntax"></a>語法

```
reg add
reg compare
reg copy
reg delete
reg export
reg import
reg load
reg query
reg restore
reg save
reg unload
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| [reg add](reg-add.md) | 將新的子機碼或專案新增至登錄。 |
| [reg compare](reg-compare.md) | 比較指定的登錄子機碼或專案。 |
| [reg copy](reg-copy.md) | 將登錄專案複製到本機或遠端電腦上指定的位置。 |
| [reg delete](reg-delete.md) | 從登錄中刪除子機碼或專案。 |
| [reg export](reg-export.md) | 將本機電腦的指定子機碼、專案和值複製到檔案中，以傳送到其他伺服器。 |
| [reg import](reg-import.md) | 將包含已匯出之登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。 |
| [reg load](reg-load.md) | 將已儲存的子機碼和專案寫入登錄中的不同子機碼。 |
| [reg query](reg-query.md) | 傳回登錄中指定的子機碼下的下一層子機碼和專案清單。 |
| [reg restore](reg-restore.md) | 將已儲存的子機碼和專案寫回登錄。 |
| [reg save](reg-save.md) | 將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。 |
| [reg unload](reg-unload.md) | 移除使用 **reg 載入** 作業載入的登錄區段。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
