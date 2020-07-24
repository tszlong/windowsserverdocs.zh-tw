---
title: online disk
description: 線上磁片命令的參考文章，它會將離線磁片設為線上狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b8b5dc2ec454cd98eb4b03952a400815f4e7ea3b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956700"
---
# <a name="online-disk"></a>online disk

使離線磁片處於線上狀態。 若為基本磁碟，此命令會嘗試讓選取的磁片和該磁片上的所有磁片區上線。 若為動態磁碟，此命令會嘗試使本機電腦上未標示為「外部」的所有磁片上線。 它也會嘗試讓動態磁碟集上的所有磁片區上線。

如果磁片群組中的動態磁碟已上線，而它是群組中的唯一磁片，則會重新建立原始群組，並將磁片移至該群組。 如果群組中有其他磁片，且它們在線上，則磁片會直接新增回群組。 如果所選磁片的群組包含鏡像或 RAID-5 磁片區，則此命令也會重新同步這些磁片區。

> [!NOTE]
> 必須選取磁片，**線上磁片**命令才會成功。 使用 [[選取磁片](select-disk.md)] 命令來選取磁片，並將焦點移至它。

> [!IMPORTANT]
> 如果用於唯讀磁片，此命令將會失敗。

## <a name="syntax"></a>語法

```
online disk [noerr]
```

### <a name="parameters"></a>參數

如需使用此命令的指示，請參閱[重新啟用遺失或離線的動態磁碟](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732026(v=ws.11))。

| 參數 | 描述 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要讓磁片上線，請輸入：

```
online disk
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
