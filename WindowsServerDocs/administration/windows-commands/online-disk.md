---
title: online disk
description: 線上磁片命令的參考文章，此命令會將離線磁片設為線上狀態。
ms.topic: reference
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a2c33a85a8217963a220495e75efebf0c6999309
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627322"
---
# <a name="online-disk"></a>online disk

將離線磁片設為線上狀態。 針對基本磁碟，此命令會嘗試將選取的磁片和磁片上的所有磁片區上線。 若為動態磁碟，此命令會嘗試將未標示為「外部」的所有磁片帶到本機電腦上。 它也會嘗試使動態磁碟集上的所有磁片區上線。

如果磁片群組中的動態磁碟已上線，而且是群組中唯一的磁片，則會重新建立原始群組，並將磁片移至該群組。 如果群組中有其他磁片且它們處於線上狀態，則會將磁片直接新增回群組中。 如果所選磁片的群組包含鏡像或 RAID-5 磁片區，此命令也會重新同步這些磁片區。

> [!NOTE]
> 必須選取磁片， **線上磁片** 命令才能成功。 使用 [ [選取磁片](select-disk.md) ] 命令來選取磁片，並將焦點移至該磁片。

> [!IMPORTANT]
> 如果在唯讀磁片上使用此命令，此命令將會失敗。

## <a name="syntax"></a>語法

```
online disk [noerr]
```

### <a name="parameters"></a>參數

如需使用此命令的指示，請參閱 [重新啟用遺失或離線的動態磁碟](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732026(v=ws.11))。

| 參數 | 描述 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要讓具有焦點的磁片上線，請輸入：

```
online disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
