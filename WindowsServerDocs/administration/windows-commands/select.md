---
title: 選取命令
description: Select 命令的參考文章，會將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟 (VHD) 。
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b322fc7bf9355e64fbe14a0823c85dddadd6171
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389176"
---
# <a name="select-commands"></a>選取命令

將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟 (VHD) 。

## <a name="syntax"></a>語法

```
select disk
select partition
select vdisk
select volume
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [選取磁片](select-disk.md) | 將焦點移至磁片。 |
| [選取資料分割](select-partition.md) | 將焦點移至資料分割。 |
| [選取 vdisk](select-vdisk.md) | 將焦點移至 VHD。 |
| [選取磁片區](select-volume.md) | 將焦點移至磁片區。 |

#### <a name="remarks"></a>備註

- 如果選取的磁片區具有對應的磁碟分割，則會自動選取該磁碟分割。

- 如果選取磁碟分割與對應的磁片區，則會自動選取該磁片區。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
