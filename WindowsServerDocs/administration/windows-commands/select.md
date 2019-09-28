---
title: 選取
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc3bc8775f971968f096ba4344348e77c112cfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384106"
---
# <a name="select"></a>選取



將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟（VHD）。

## <a name="syntax"></a>語法

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[選取磁片](select-disk.md)|將焦點移至磁片。|
|[選取資料分割](select-partition.md)|將焦點移至資料分割。|
|[選取磁片區](select-volume.md)|將焦點移至磁片區。|
|[選取 vdisk](select-vdisk.md)|將焦點移至 VHD。|

## <a name="remarks"></a>備註

-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。
-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

