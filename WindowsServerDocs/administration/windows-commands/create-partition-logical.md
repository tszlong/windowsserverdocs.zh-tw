---
title: 建立邏輯磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3af60aed6c8305e410c6ebfba3cf2e006034ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434148"
---
# <a name="create-partition-logical"></a>建立邏輯磁碟分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在現有的延伸磁碟分割中建立的邏輯磁碟分割。 您只可以使用此命令在主開機記錄\(MBR\)磁碟。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                                                                                                                       描述                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                                                                              指定邏輯分割區的大小以 mb 為單位\(MB\)，該值必須小於延伸磁碟分割。 如果未指定大小，磁碟分割會繼續直到延伸磁碟分割沒有更多的可用空間。                                                                                                               |
| offset\=<n> | 指定的位移，以 kb 為單位\(KB\)，在建立資料分割。 位移會以完全填滿磁柱大小會無條件進位。 如果沒有指定位移，資料分割會置於足夠容納它的第一個磁碟範圍內。 指定的數字為至少長度以位元組為單位的磁碟分割**大小\=<n>** 。 如果您指定邏輯分割區的大小，它必須小於延伸磁碟分割。 |
| align\=<n>  |                                                                                     對齊最接近對齊界限的所有磁碟區或分割區範圍。 多半搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。  <n> 是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。                                                                                      |
|    noerr    |                                                                                                                           針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。                                                                                                                           |
  
## <a name="remarks"></a>備註  
  
-   如果**大小**並**位移**參數沒有指定，則延伸磁碟分割中的最大磁碟範圍內建立邏輯分割區。  
  
-   在建立資料分割之後，焦點會自動移到新的邏輯磁碟分割。  
  
-   這項作業成功時，必須選取一個基本 MBR 磁碟。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要建立邏輯分割區 1000 mb 為單位的大小，在 延伸磁碟分割的所選的磁碟，輸入：  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

