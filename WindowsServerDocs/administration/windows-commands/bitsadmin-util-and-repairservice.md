---
title: bitsadmin util and repairservice
description: Bitsadmin util and repairservice 命令的參考文章，可修正各種 BITS 服務版本的已知問題。
ms.topic: reference
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0a33030e6036eacdf39c29f7cd2e5e88775905
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034696"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util and repairservice

如果 BITS 無法啟動，此切換會嘗試解決與 Windows 服務 (（例如 LANManworkstation) 和網路目錄）不正確的服務設定和相依性相關的錯誤。 此參數也會產生輸出，以指出已解決的問題。

> [!NOTE]
> BITS 1.5 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /force | 選擇性。 刪除並重新建立服務。|

> [!NOTE]
> 如果 BITS 再次建立服務，即使在當地語系化的系統中，服務描述字串也可能設定為英文。

## <a name="examples"></a>範例

若要修復 BITS 服務設定：

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
