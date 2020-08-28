---
title: select
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fb6230c24f723e20b09449967b157dd86347202
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032220"
---
# <a name="select"></a>select



將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟 (VHD) 。

## <a name="syntax"></a>語法

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[選取磁片](select-disk.md)|將焦點移至磁片。|
|[選取資料分割](select-partition.md)|將焦點移至資料分割。|
|[選取磁片區](select-volume.md)|將焦點移至磁片區。|
|[選取 vdisk](select-vdisk.md)|將焦點移至 VHD。|

## <a name="remarks"></a>備註

-   如果選取的磁片區具有對應的磁碟分割，則會自動選取該磁碟分割。
-   如果選取磁碟分割與對應的磁片區，則會自動選取該磁片區。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

