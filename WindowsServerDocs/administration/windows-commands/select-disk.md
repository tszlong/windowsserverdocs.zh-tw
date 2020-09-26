---
title: select disk
description: 選取磁片命令的參考文章，此命令會選取指定的磁片，然後將焦點移至其中。
ms.topic: reference
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 194bd36305060c8adfec27435ef05bf87605b45c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389241"
---
# <a name="select-disk"></a>select disk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁碟並將焦點移至其上。

## <a name="syntax"></a>語法

```
select disk={<n>|<disk path>|system|next}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<n>` | 指定要接收焦點的磁片數目。 您可以使用 DiskPart 中的 [ **列出磁片** ] 命令來查看電腦上所有磁片的數目。<p>**注意**<br>設定具有多個磁片的系統時，請勿使用 **select disk = 0** 來指定系統磁片。 當您重新開機時，電腦可能會重新指派磁片編號，而且具有相同磁片設定的不同電腦可以有不同的磁片編號。 |
| `<disk path>` | 指定要接收焦點之磁片的位置，例如 `PCIROOT(0)#PCI(0F02)#atA(C00T00L00)` 。 若要查看磁片的位置路徑，請選取它，然後輸入 **詳細資料磁片**。 |
| 系統 | 在 BIOS 電腦上，此選項會指定磁片0接收焦點。 在 EFI 電腦上，包含 EFI 系統磁碟分割 (ESP) 的磁片（用於目前的開機）會接收焦點。 在 EFI 電腦上，如果沒有 ESP，則命令會失敗、如果有一個以上的 ESP，或電腦是從 Windows 預先安裝環境 (Windows PE) 進行開機。 |
| 下一步 | 選取磁片之後，此選項會逐一查看磁片清單中的所有磁片。 當您執行這個選項時，清單中的下一個磁片會獲得焦點。 |

## <a name="examples"></a>範例

若要將焦點移到磁片1，請輸入：

```
select disk=1
```

若要使用其位置路徑來選取磁片，請輸入：

```
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)
```

若要將焦點移至系統磁片，請輸入：

```
select disk=system
```

若要將焦點移到電腦上的下一個磁片，請輸入：

```
select disk=next
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

- [選取 vdisk 命令](select-vdisk.md)

- [選取磁片區命令](select-volume.md)
