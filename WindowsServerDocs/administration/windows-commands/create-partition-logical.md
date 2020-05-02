---
title: create partition logical
description: 建立資料分割邏輯的參考主題，它會在現有的擴展資料分割中建立邏輯分割區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43d1c26d785fcecbb81a99c7484a15b3ea8b58a2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719238"
---
# <a name="create-partition-logical"></a>create partition logical

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在現有的擴展資料分割中建立邏輯分割區。 您只能在主開機記錄（MBR）磁片上使用此命令。

## <a name="syntax"></a>語法  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                                                                                                                       描述                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  容量\=<n>  |                                                                                                              指定邏輯分割\(區的大小\)（mb），其必須小於延伸磁碟分割。 如果沒有指定大小，磁碟分割會繼續進行，直到延伸磁碟分割中沒有其他可用空間為止。                                                                                                               |
| 投影\=<n> | 指定分割區的建立\(位移\)，以 kb 為單位。 位移會向上舍入，以完全填滿使用的任何圓柱大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 資料分割至少是以位元組為單位所**指定\=的數位。** 如果您指定了邏輯分割區的大小，它必須小於延伸磁碟分割。 |
| 對齊\=<n>  |                                                                                     將所有磁片區或資料分割範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號\(LUN\)陣列搭配使用，以改善效能。  <n>這是從磁片開始\(到\)最接近對齊界限的 kb kb 數。                                                                                      |
|    noerr    |                                                                                                                           僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                           |
  
## <a name="remarks"></a>備註  
  
-   如果未指定**size**和**offset**參數，則會在延伸磁碟分割可用的最大磁片範圍內建立邏輯分割區。  
  
-   建立分割區之後，焦點會自動移至新的邏輯分割區。  
  
-   必須選取基本的 MBR 磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。  
  
## <a name="examples"></a>範例  
若要建立大小為 1000 mb 的邏輯分割區，請在所選磁片的延伸磁碟分割中，輸入：  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

