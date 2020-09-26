---
title: select volume
description: Select volume 命令的參考檔，它會選取指定的磁片區，並將焦點移至該磁片區。
ms.topic: reference
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 643db6484842e8a67462b0460a1ecdeac653e577
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389186"
---
# <a name="select-volume"></a>select volume

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁碟區並將焦點轉移到該磁碟區。 此命令也可以用來顯示目前在所選磁片中有焦點的磁片區。

## <a name="syntax"></a>語法

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<n>` | 要接收焦點的磁片區數目。 您可以使用 DiskPart 中的 **list volume** 命令，來查看目前所選磁片上所有磁片區的數目。 |
| `<d> `| 要接收焦點之磁片區的磁碟機號或掛接點路徑。 |

## <a name="remarks"></a>備註

- 如果未指定磁片區，此命令會顯示目前在所選磁片中具有焦點的磁片區。

- 在基本磁碟上，選取磁片區也會將焦點提供給對應的磁碟分割。

  - 如果選取的磁片區具有對應的磁碟分割，則會自動選取該磁碟分割。

  - 如果選取磁碟分割與對應的磁片區，則會自動選取該磁片區。

## <a name="examples"></a>範例

若要將焦點移至 *磁片區 2*，請輸入：

```
select volume=2
```

若要將焦點移到 *C 磁片磁碟機*，請輸入：

```
select volume=c
```

若要將焦點移至名為 *c:\mountpath*的資料夾上所裝載的磁片區，請輸入：

```
select volume=c:\mountpath
```

若要顯示目前在所選磁片中具有焦點的磁片區，請輸入：

```
select volume
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [新增磁片區命令](add-volume.md)

- [attributes volume 命令](attributes-volume.md)

- [建立磁片區鏡像命令](create-volume-mirror.md)

- [建立磁片區 raid 命令](create-volume-raid.md)

- [建立 volume simple 命令](create-volume-simple.md)

- [建立磁片區 stripe 命令](create-volume-stripe.md)

- [刪除磁片區命令](delete-volume.md)

- [detail volume 命令](detail-volume.md)

- [fsutil volume 命令](fsutil-volume.md)

- [列出磁片區命令](list-volume.md)

- [offline volume 命令](offline-volume.md)

- [onine volume 命令](online-volume.md)

- [選取磁片命令](select-disk.md)

- [選取資料分割命令](select-partition.md)

- [選取 vdisk 命令](select-vdisk.md)
