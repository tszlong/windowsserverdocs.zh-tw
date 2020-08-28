---
title: 匯入 diskpart
description: 匯入命令的參考文章，此命令會將外部磁片群組匯入本機電腦的磁片群組。
ms.topic: reference
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a6bbd2a2adb6017f67667e7e2e71b0f6302b517
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037986"
---
# <a name="import-diskpart"></a>import (diskpart)

將外部磁片群組匯入本機電腦的磁片群組。 此命令會匯入具有焦點磁片的相同群組中的每個磁片。

> 須知使用此命令之前，您必須使用 [ [選取磁片] 命令](select-disk.md) 來選取外部磁片群組中的動態磁碟，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
import [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要將相同磁片群組中的每個磁片匯入至本機電腦的磁片群組，請輸入：

```
import
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskpart 命令](diskpart.md)
