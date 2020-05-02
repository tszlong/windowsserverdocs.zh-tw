---
title: create volume stripe
description: 建立磁片區 stripe 的參考主題，它會使用兩個或多個指定的動態磁碟來建立等量磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98ca692d27135aa8ba4da0ff85ecd3074cdd73c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716872"
---
# <a name="create-volume-stripe"></a>create volume stripe

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用兩個或多個指定的動態磁碟建立等量磁片區。  
  
> [!IMPORTANT]  
> 對於 Windows Vista，此 DiskPart 命令僅適用于 Windows Vista 旗艦版、Windows Vista Enterprise 和 Windows Vista Business edition。

## <a name="syntax"></a>語法  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|         參數         |                                                                                                                            描述                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         容量\=<n>         |             磁片區將在每個磁片上\(佔用\)的磁碟空間量（以 mb 為單位）。 如果未指定大小，則新磁碟區會佔用最小磁碟上剩餘的可用空間，及每個後續磁碟上等量的空間。             |
| 磁片\=<n><n>、\[、<n>,.。。\] |                                  建立等量磁片區的動態磁碟。 您至少需要兩個動態磁碟來建立一個等量磁碟區。 每個磁片上會配置**與\= size**相等的空間量。                                   |
|        對齊\=<n>         | 將所有磁片區範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號\(LUN\)陣列搭配使用，以改善效能。 *n*是從磁片開頭到\(最\)接近對齊界限的 kb kb 數。 |
|           noerr           |                               僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新磁碟區。  
  
## <a name="examples"></a>範例  
若要建立大小為 1000 mb 的等量磁片區，請在磁片1和2上輸入：  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

