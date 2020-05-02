---
title: bitsadmin util and repairservice
description: Bitsadmin util 和 repairservice 命令的參考主題，可修正各種 BITS 服務版本中的已知問題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0104a3f2ace972821151bf5083f9b0795e427ff1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707648"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util and repairservice

如果無法啟動 BITS，此交換器會嘗試解決與 Windows 服務（例如 LANManworkstation）和網路目錄不正確的服務設定和相依性相關的錯誤。 此參數也會產生輸出，指出是否已解決問題。

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
