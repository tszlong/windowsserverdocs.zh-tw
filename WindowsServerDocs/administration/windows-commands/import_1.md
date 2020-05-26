---
title: 匯入 diskpart
description: 匯入命令的參考主題，它會將外部磁片群組匯入到本機電腦的磁片群組。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6912aa9698d484501cad5f3cdfb5b19955bb4931
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818448"
---
# <a name="import-diskpart"></a>匯入（diskpart）

將外部磁片群組匯入本機電腦的磁片群組。 此命令會匯入與具有焦點的磁片位於相同群組中的每個磁片。

> 重大在您可以使用此命令之前，您必須使用 [[選取磁片] 命令](select-disk.md)來選取外部磁片群組中的動態磁碟，並將焦點移至它。

## <a name="syntax"></a>語法

```
import [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要匯入相同磁片群組中的每個磁片，並將焦點放入本機電腦的磁片群組，請輸入：

```
import
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskpart 命令](diskpart.md)
