---
title: bitsadmin util and repairservice
description: Bitsadmin util 和 repairservice 命令的參考文章，可修正各種 BITS 服務版本中的已知問題。
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d90e6328376f52e60b598d8c2324b59877415db
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880849"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util and repairservice

如果無法啟動 BITS，此交換器會嘗試解決與 Windows 服務 (（例如 LANManworkstation) 和網路目錄）上不正確的服務設定和相依性相關的錯誤。 此參數也會產生輸出，指出是否已解決問題。

> [!NOTE]
> BITS 1.5 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /force | 選擇性。 刪除並重新建立服務。|

> [!NOTE]
> 如果 BITS 再次建立服務，則即使是在當地語系化的系統中，服務描述字串可能也會設定為英文。

## <a name="examples"></a>範例

若要修復 BITS 服務設定：

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
