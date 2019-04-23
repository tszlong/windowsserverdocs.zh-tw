---
title: 選取
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889899"
---
# <a name="select"></a>選取



將焦點轉移到磁碟、 磁碟分割、 磁碟區或虛擬硬碟 (VHD)。

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
|[選取磁碟](select-disk.md)|將焦點轉移到磁碟。|
|[選取資料分割](select-partition.md)|將焦點轉移到的磁碟分割。|
|[選取磁碟區](select-volume.md)|將焦點轉移到磁碟區。|
|[選取 vdisk](select-vdisk.md)|將焦點轉移到 VHD。|

## <a name="remarks"></a>備註

-   如果有對應的資料分割選取磁碟區，則將會自動選取資料分割。
-   如果已選取的分割區，與對應的磁碟區，將會自動選取磁碟區。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

