---
title: create volume raid
description: 建立磁片區 raid 的 Windows 命令主題，它會使用三個或更多指定的動態磁碟建立 RAID-5 磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 656656aa8f1783920097a270bee24aabeb3d005a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846921"
---
# <a name="create-volume-raid"></a>create volume raid

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用三個或多個指定的動態磁碟，建立 RAID-5 磁片區。  

> [!IMPORTANT]  
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|           參數           |                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           大小\=<n>           | 磁片區將在每個磁片上佔用的磁碟空間量（以 mb 為單位） \(MB\)。 如果沒有指定大小，將會建立最大可能的 RAID\-5 個磁片區。 具有最小可用連續可用空間的磁片會決定 RAID\-5 磁片區的大小，並從每個磁片配置相同的空間量。 RAID\-5 磁片區中實際可用的磁碟空間量小於磁碟空間的總和，因為同位檢查需要一些磁碟空間。 |
| 磁片\=<n>、<n>、<n>\[、<n>,...\] |                                                                                                                                               要在其上建立 RAID\-5 磁片區的動態磁碟。 您需要至少三個動態磁碟，才能建立 RAID\-5 磁片區。 每個磁片上配置的空間**大小等於\=<n>** 。                                                                                                                                                |
|          \=對齊 <n>           |                                                                                                                   將所有磁片區範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 \(LUN\) 陣列以改善效能。 *n*是從磁片開頭到最接近之對齊界限的 KB \(kb\) 數。                                                                                                                   |
|             noerr             |                                                                                                                                                 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                                                  |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新磁碟區。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要使用磁片1、2和3，建立大小為 1000 mb 的 RAID\-5 個容量，請輸入：  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

