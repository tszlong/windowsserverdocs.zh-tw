---
title: delete disk
description: 刪除磁片的 Windows 命令主題，這會從磁片清單刪除遺失的動態磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a767e0689d5fbabb193df37528a0909ab63a1ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846651"
---
# <a name="delete-disk"></a>delete disk

從磁片清單刪除遺失的動態磁碟。

如需有關如何使用此命令的指示，請參閱[移除遺失的動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207055)（ https://go.microsoft.com/fwlink/?LinkId=207055)。

## <a name="syntax"></a>語法

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|
|覆寫|啟用 DiskPart 來刪除磁碟上的全部簡單磁碟區。 如果磁碟包含鏡像磁碟區的一半，則會刪除磁碟上的一半鏡像。 如果磁碟屬於 RAID-5 磁碟區，則 delete disk override 命令失敗。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要從磁片清單中刪除遺失的動態磁碟，請輸入：
```
delete disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

