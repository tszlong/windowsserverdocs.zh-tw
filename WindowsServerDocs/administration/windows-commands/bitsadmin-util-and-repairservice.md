---
title: bitsadmin util 和 repairservice
description: 適用于**bitsadmin util 和 repairservice**的 Windows 命令主題，可修正各種 BITS 服務版本中的已知問題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 164a402e7cbfc0a9223a97f4246eac84f0797aed
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122510"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

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

下列範例會修復 BITS 服務設定。

```
C:\>bitsadmin /util /repairservice
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)