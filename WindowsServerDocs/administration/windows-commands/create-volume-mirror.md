---
title: 建立磁片區鏡像
description: 建立磁片區鏡像的參考主題，它會使用兩個指定的動態磁碟來建立磁片區鏡像。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eda0a6d799fc88c8382e128df1bd260b9e9426b7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716943"
---
# <a name="create-volume-mirror"></a>建立磁片區鏡像

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用兩個指定的動態磁碟來建立磁片區鏡像。  
  
> [!NOTE]  
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|         參數         |                                                                                                                                     描述                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         容量\=<n>         |                 指定磁片區將在每個磁片上\(佔用\)的磁碟空間量（以 mb 為單位）。 如果未指定大小，則新磁碟區會佔用最小磁碟上剩餘的可用空間，及每個後續磁碟上等量的空間。                 |
| 磁片\=<n><n>、\[、<n>,.。。\] |                       指定要在其上建立鏡像磁碟區的動態磁碟。 您需要兩個動態磁碟來建立鏡像磁碟區。 系統會在每個磁片上配置與**size**參數所指定大小相等的空間量。                        |
|        對齊\=<n>         | 將所有磁片區範圍對齊最接近的對齊界限。 此參數通常會與硬體 RAID 邏輯單元編號\(LUN\)陣列搭配使用，以改善效能。 *n*是從磁片開頭到\(最\)接近對齊界限的 kb kb 數。 |
|           noerr           |                                        僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束並出現錯誤。                                         |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新磁碟區。  
  
## <a name="examples"></a>範例  
若要建立大小為 1000 mb 的鏡像磁碟區，請在 [磁片 1] 和 [2] 上輸入：  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

