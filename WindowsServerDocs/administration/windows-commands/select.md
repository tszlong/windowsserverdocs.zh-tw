---
title: select
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6004d39e225b1ac4acd96b4108accff2ccfc485c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935936"
---
# <a name="select"></a>select



將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟（VHD）。

## <a name="syntax"></a>語法

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[選取磁片](select-disk.md)|將焦點移至磁片。|
|[選取資料分割](select-partition.md)|將焦點移至資料分割。|
|[選取磁片區](select-volume.md)|將焦點移至磁片區。|
|[選取 vdisk](select-vdisk.md)|將焦點移至 VHD。|

## <a name="remarks"></a>備註

-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。
-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

