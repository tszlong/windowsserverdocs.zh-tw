---
title: select partition
description: Select partition 命令的參考檔，它會選取指定的資料分割，並將焦點移至其中。
ms.topic: reference
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6418ff1a08bdc12355d2b2dc75bbb7151539ae02
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389231"
---
# <a name="select-partition"></a>select partition

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的資料分割，並將焦點移至該分割區。 此命令也可以用來顯示目前在所選磁片中有焦點的磁碟分割。

## <a name="syntax"></a>語法

```
select partition=<n>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 分割區 =`<n>` | 要接收焦點的資料分割編號。 您可以使用 DiskPart 中的 **清單磁碟分割** 命令，來查看目前所選磁片上所有磁碟分割的數目。 |

#### <a name="remarks"></a>備註

- 選取磁碟分割之前，您必須先使用 [ **選取磁片** ] 命令來選取磁片。

  - 如果未指定資料分割編號，此選項會顯示目前在所選磁片中具有焦點的磁碟分割。

  - 如果選取的磁片區具有對應的磁碟分割，則會自動選取該分割區。

  - 如果選取磁碟分割與對應的磁片區，則會自動選取磁片區。

## <a name="examples"></a>範例

若要將焦點移至資料 *分割 3*，請輸入：

```
select partitition=3
```

若要顯示目前在所選磁片中具有焦點的磁碟分割，請輸入：

```
select partition
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立資料分割 efi 命令](create-partition-efi.md)

- [建立分割區擴充命令](create-partition-extended.md)

- [建立資料分割邏輯命令](create-partition-logical.md)

- [建立資料分割 msr 命令](create-partition-msr.md)

- [建立資料分割主要命令](create-partition-primary.md)

- [刪除資料分割命令](delete-partition.md)

- [detail partition 命令](detail-partition.md)

- [選取磁片命令](select-disk.md)

- [選取 vdisk 命令](select-vdisk.md)

- [選取磁片區命令](select-volume.md)
