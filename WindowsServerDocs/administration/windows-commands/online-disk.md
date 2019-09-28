---
title: 線上磁片
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3d798bf34ec2f9d2f01b5470c4ec52f936674135
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372506"
---
# <a name="online-disk"></a>線上磁片



將目前離線的磁片帶入線上狀態。

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此命令。

> [!IMPORTANT]
> 如果用於唯讀磁片，此命令將會失敗。

如需有關如何使用此命令的指示，請參閱[重新啟用遺失或離線的動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207046)（ https://go.microsoft.com/fwlink/?LinkId=207046) 。

## <a name="syntax"></a>語法

```
online disk [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   在 Windows Vista 中沒有參數使用時，此命令會在磁片群組上運作。 針對基本磁碟，每個群組永遠不會有一個以上的磁片。 若為動態磁碟，群組會包含所有非外部動態磁碟。
-   若為基本磁碟，此命令會嘗試讓選取的磁片和該磁片上的所有磁片區上線。
-   若為動態磁碟，此命令會嘗試使本機電腦上未標示為外部的所有磁片上線。 它也會嘗試將一組動態磁碟上的所有磁片區上線。
-   如果磁片群組中的動態磁碟已上線，而且它是群組中的唯一磁片，則會重新建立原始群組，並將磁片移至該群組。 如果群組中有其他磁片，而且它們已上線，則磁片會直接新增回群組。
-   如果所選磁片的群組包含鏡像或 RAID-5 磁片區，則此命令也會重新同步這些磁片區。
-   必須選取磁片，此命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="BKMK_examples"></a>典型

若要讓磁片上線，請輸入：
```
online disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

