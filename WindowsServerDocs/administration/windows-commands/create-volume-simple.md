---
title: create volume simple
description: 建立磁片區 simple 的參考主題，它會在指定的動態磁碟上建立簡單磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f92bd9cf92dea258c6a49dd4cf75c4aae357c88e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716928"
---
# <a name="create-volume-simple"></a>create volume simple

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在指定的動態磁碟上建立簡單磁片區。  
  
> [!IMPORTANT]  
> 對於 Windows Vista，此 DiskPart 命令僅適用于 Windows Vista 旗艦版、Windows Vista Enterprise 和 Windows Vista Business edition。
  
## <a name="syntax"></a>語法  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數  |                                                                                                                            描述                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 容量\=<n>  |                                                                  磁片區的大小（以 mb \(為\)單位）。 如果未指定大小，則新磁碟區會佔用磁碟上剩餘的空間。                                                                   |
| 硬碟\=<n>  |                                                                                要在其上建立磁片區的動態磁碟。 如果未指定任何磁片，則會使用目前的磁片。                                                                                |
| 對齊\=<n> | 將所有磁片區範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號\(LUN\)陣列搭配使用，以改善效能。 *n*是從磁片開頭到\(最\)接近對齊界限的 kb kb 數。 |
|   noerr    |                               僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新磁碟區。  
  
## <a name="examples"></a>範例  
若要建立大小為 1000 mb 的磁片區，請在 disk 1 上輸入：  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

