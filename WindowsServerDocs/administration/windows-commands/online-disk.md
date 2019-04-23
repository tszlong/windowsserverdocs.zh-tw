---
title: 線上磁碟
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858059"
---
# <a name="online-disk"></a>線上磁碟



將目前離線為線上狀態的磁碟。

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用此命令。

> [!IMPORTANT]
> 如果它是磁碟上使用唯讀模式，此命令會失敗。

如需如何使用此命令的相關資訊的指示，請參閱[重新啟用遺失或離線的動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207046)(https://go.microsoft.com/fwlink/?LinkId=207046)。

## <a name="syntax"></a>語法

```
online disk [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   當不使用 Windows Vista 中的參數，此命令會作磁碟群組。 若為基本磁碟，有絕不會是每個群組的多個磁碟。 若為動態磁碟，此群組包含所有非外部的動態磁碟。
-   若為基本磁碟，此命令會嘗試將上線所選的磁碟和所有的磁碟區磁碟上。
-   若為動態磁碟，此命令會嘗試將上線未標示為外部索引在本機電腦上的所有磁碟。 它也會嘗試將一組動態磁碟上的所有磁碟區上線。
-   如果動態磁碟的磁碟群組中處於線上，而且是群組中唯一的磁碟，然後重新建立原始的群組，並磁碟移至該群組。 如果群組中有其他磁碟，而它們處於線上狀態，然後只新增磁碟至群組。
-   如果所選磁碟的群組包含鏡像或 raid-5 磁碟區，此命令也來重新同步處理這些磁碟區。
-   磁碟必須選取這個命令才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要讓具有線上焦點的磁碟，請輸入：
```
online disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

