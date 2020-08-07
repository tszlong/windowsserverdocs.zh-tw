---
title: reg
description: Reg 命令的參考文章，其會對登錄子機碼資訊和登錄專案中的值執行作業。
ms.topic: article
ms.assetid: c97496b2-d1ff-4887-b5d2-6e1524be465a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18a9f243001758393597f6cc5803dd42cdc32dae
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883976"
---
# <a name="reg"></a>reg

針對登錄子機碼資訊和登錄專案中的值執行作業。

有些作業可讓您在本機電腦或遠端電腦上查看或設定登錄專案，有些則可讓您只設定本機電腦。 使用**reg**設定遠端電腦的登錄，會限制您可以在某些作業中使用的參數。 檢查每個作業的語法和參數，以確認它們可以在遠端電腦上使用。

> [!CAUTION]
> 請勿直接編輯登錄，除非您沒有替代方案。 登錄編輯程式會略過標準保護，允許降低效能、損毀您的系統，甚至要求您重新安裝 Windows 的設定。 您可以使用 [控制台] 中的程式或 Microsoft Management Console (MMC) ，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。

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
| [reg copy](reg-copy.md) | 將登錄專案複製到本機或遠端電腦上的指定位置。 |
| [reg delete](reg-delete.md) | 從登錄中刪除子機碼或專案。 |
| [reg export](reg-export.md) | 將本機電腦的指定子機碼、專案和值複製到檔案中，以傳輸到其他伺服器。 |
| [reg import](reg-import.md) | 將包含已匯出登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。 |
| [reg load](reg-load.md) | 將已儲存的子機碼和專案寫入登錄中的不同子機碼。 |
| [reg query](reg-query.md) | 傳回位於登錄中指定子機碼下的下一層子機碼和專案的清單。 |
| [reg restore](reg-restore.md) | 將已儲存的子機碼和專案寫入登錄。 |
| [reg save](reg-save.md) | 將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。 |
| [reg unload](reg-unload.md) | 移除使用**reg 載入**作業載入的登錄區段。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
