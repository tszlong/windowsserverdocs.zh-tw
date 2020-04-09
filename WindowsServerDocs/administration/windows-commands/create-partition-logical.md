---
title: create partition logical
description: 建立磁碟分割邏輯的 Windows 命令主題，它會在現有的擴展資料分割中建立邏輯分割區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b16c161bf12476eee9d3959e5f313fd844ff3519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847061"
---
# <a name="create-partition-logical"></a>create partition logical

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在現有的擴展資料分割中建立邏輯分割區。 您只能在主開機記錄（MBR）磁片上使用此命令。

## <a name="syntax"></a>語法  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                                                                                                                       描述                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  大小\=<n>  |                                                                                                              指定邏輯分割區的大小（以 mb 為單位） \(MB\)，其必須小於延伸磁碟分割。 如果沒有指定大小，磁碟分割會繼續進行，直到延伸磁碟分割中沒有其他可用空間為止。                                                                                                               |
| offset\=<n> | 指定建立分割區的位移（以 kb 為單位） \(KB\)。 位移會向上舍入，以完全填滿使用的任何圓柱大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 資料分割的長度至少為 [**大小]\=<n>** 所指定的數位。 如果您指定了邏輯分割區的大小，它必須小於延伸磁碟分割。 |
| \=對齊 <n>  |                                                                                     將所有磁片區或資料分割範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 \(LUN\) 陣列以改善效能。  <n> 是從磁片開頭到最接近之對齊界限的 kb \(KB\) 數。                                                                                      |
|    noerr    |                                                                                                                           僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                           |
  
## <a name="remarks"></a>備註  
  
-   如果未指定**size**和**offset**參數，則會在延伸磁碟分割可用的最大磁片範圍內建立邏輯分割區。  
  
-   建立分割區之後，焦點會自動移至新的邏輯分割區。  
  
-   必須選取基本的 MBR 磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要建立大小為 1000 mb 的邏輯分割區，請在所選磁片的延伸磁碟分割中，輸入：  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

