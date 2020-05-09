---
title: 建立
description: Create 命令的參考主題，它會在磁片上建立分割區或陰影分割區、在一或多個磁片上的磁片區，或虛擬硬碟（VHD）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f371bee591e29a45b1488b2c36112b55ed54d3f8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993186"
---
# <a name="create"></a>建立

在磁片上、一或多個磁片上的磁片區或虛擬硬碟（VHD）上建立分割區或陰影。 如果您使用此命令在陰影磁片上建立磁片區，則陰影複製組中必須至少有一個磁片區。

## <a name="syntax"></a>語法

```
create partition
create volume
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [建立分割區主要命令](create-partition-primary.md) | 在基本磁碟上建立具有焦點的主要磁碟分割。 |
| [建立資料分割 efi 命令](create-partition-efi.md) | 在 Itanium 型電腦上的 GUID 磁碟分割表格（gpt）磁片上建立可延伸韌體介面（EFI）系統磁碟分割。 |
| [建立分割區擴充命令](create-partition-extended.md) | 在磁片上建立具有焦點的延伸磁碟分割。 |
| [建立磁碟分割邏輯命令](create-partition-logical.md) | 在現有的擴展資料分割中建立邏輯分割區。 |
| [建立資料分割 msr 命令](create-partition-msr.md) | 在 GUID 磁碟分割表格（gpt）磁片上建立 Microsoft Reserved （MSR）分割區。 |
| [建立磁片區簡單命令](create-volume-simple.md) | 在指定的動態磁碟上建立簡單磁片區。 |
| [建立磁片區鏡像命令](create-volume-mirror.md) | 使用兩個指定的動態磁碟來建立磁片區鏡像。 |
| [建立磁片區 raid 命令](create-volume-raid.md) | 使用三個或多個指定的動態磁碟，建立 RAID-5 磁片區。 |
| [建立磁片區 stripe 命令](create-volume-stripe.md) | 使用兩個或多個指定的動態磁碟建立等量磁片區。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
