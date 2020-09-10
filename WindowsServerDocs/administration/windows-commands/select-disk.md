---
title: select disk
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b9f6461d22fa6ffcc9914e8edc6644a4b9364ece
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635489"
---
# <a name="select-disk"></a>select disk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁片並將焦點移至該磁片。



## <a name="syntax"></a>語法

```
select disk={ <n> | <disk path> | system | next }
```

> [!NOTE]
> **<disk path>**、**系統**和**下一個**參數只能在 Windows 7 和 windows Server 2008 R2 中使用。

### <a name="parameters"></a>參數

|  參數  |                                                                                                                                                                                                            描述                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | 指定要接收焦點的磁片數目。 您可以使用 DiskPart 中的 [ **列出磁片** ] 命令來查看電腦上所有磁片的數目。 **注意：** 設定具有多個磁片的系統時，請勿使用 [ **選取磁片 \= 0** ] 來指定系統磁片。 當您重新開機時，電腦可能會重新指派磁片編號，而且具有相同磁片設定的不同電腦可以有不同的磁片編號。 |
| <disk path> |                                                                                                                 指定要接收焦點之磁片的位置，例如**PCIROOT \( 0 \) \# PCI \( 0F02 \) \# atA \( C00T00L00 \) **。 若要查看磁片的位置路徑，請選取它，然後輸入 **詳細資料磁片**。                                                                                                                  |
|   系統    |                                 在 BIOS 電腦上，指定磁片0接收焦點。 在 EFI 電腦上，包含用於目前開機之 EFI 系統磁碟分割 ESP 的磁片 \( \) 會收到焦點。 在 EFI 電腦上，如果沒有 ESP，則命令會失敗，如果有一個以上的 ESP，或電腦是從 Windows 預先安裝環境 Windows PE 啟動的 \( \) 。                                  |
|    下一步     |                                                                                                                                     選取磁片之後，此命令會逐一查看磁片清單中的所有磁片。 當您執行此命令時，清單中的下一個磁片將會獲得焦點。                                                                                                                                      |

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

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)




