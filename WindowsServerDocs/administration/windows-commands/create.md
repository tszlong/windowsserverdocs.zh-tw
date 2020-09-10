---
title: 建立
description: Create 命令的參考文章，此命令會在磁片上建立磁碟分割或陰影分割、一或多個磁片上的磁片區，或虛擬硬碟 (VHD) 。
ms.topic: reference
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8a836320f9fe699beac20990ad60ade5f06e37f6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629007"
---
# <a name="create"></a>建立

在磁片上建立磁碟分割或陰影、一或多個磁片上的磁片區，或 (VHD) 的虛擬硬碟。 如果您使用此命令在陰影磁片上建立磁片區，則陰影複製集中必須至少有一個磁片區。

## <a name="syntax"></a>語法

```
create partition
create volume
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [建立資料分割主要命令](create-partition-primary.md) | 在基本磁碟上建立具有焦點的主要磁碟分割。 |
| [建立資料分割 efi 命令](create-partition-efi.md) | 在 GUID 磁碟分割表格上建立可延伸的固件介面 (EFI) 系統磁碟分割 (以 Itanium 為基礎的電腦上 gpt) 磁片。 |
| [建立分割區擴充命令](create-partition-extended.md) | 在磁片上建立具有焦點的延伸磁碟分割。 |
| [建立資料分割邏輯命令](create-partition-logical.md) | 在現有的延伸磁碟分割中建立邏輯分割區。 |
| [建立資料分割 msr 命令](create-partition-msr.md) | 在 GUID 磁碟分割表格上建立 Microsoft Reserved (MSR) 磁碟分割 (gpt) 磁片。 |
| [建立 volume simple 命令](create-volume-simple.md) | 在指定的動態磁碟上建立簡單磁片區。 |
| [建立磁片區鏡像命令](create-volume-mirror.md) | 使用兩個指定的動態磁碟來建立磁片區鏡像。 |
| [建立磁片區 raid 命令](create-volume-raid.md) | 使用三個或多個指定的動態磁碟來建立 RAID-5 磁片區。 |
| [建立磁片區 stripe 命令](create-volume-stripe.md) | 使用兩個或多個指定的動態磁碟來建立等量磁片區。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
