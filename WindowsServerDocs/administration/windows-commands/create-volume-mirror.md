---
title: 建立磁碟區鏡像
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72d80fdf6eca1262a858cbe2a98ed8c9c421bff6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434086"
---
# <a name="create-volume-mirror"></a>建立磁碟區鏡像

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用兩個指定的動態磁碟，以建立磁碟區鏡像。  
  
> [!NOTE]  
> 此命令只適用於 Windows 7 和 Windows Server 2008 R2。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|         參數         |                                                                                                                                     描述                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |                 指定的磁碟空間量，以 mb 為單位\(MB\)的磁碟區佔據每個磁碟上。 如果未指定大小，新的磁碟區會佔用最小的磁碟和每個後續磁碟上等量的空間上剩餘的可用空間。                 |
| disk\=<n>,<n>\[,<n>,...\] |                       指定動態鏡像磁碟區建立所在的磁碟。 您需要建立鏡像磁碟區的兩個動態磁碟。 使用指定的大小相等的空間數量**大小**參數會配置每個磁碟上。                        |
|        align\=<n>         | 對齊最接近對齊界限的所有磁碟區範圍。 這個參數通常搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。 *n*是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。 |
|           noerr           |                                        用於僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，發生錯誤。                                         |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新的磁碟區。  
  
## <a name="BKMK_examples"></a>範例  
若要建立鏡像磁碟區的 1000 mb 的大小，在 磁碟 1 和 2 中，輸入：  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

