---
title: select
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a41d240cfdcb15068d479fb96fce09880db7c1f9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882769"
---
# <a name="select"></a>select



將焦點移至磁片、分割區、磁片區或虛擬硬碟 (VHD) 。

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

-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。
-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

