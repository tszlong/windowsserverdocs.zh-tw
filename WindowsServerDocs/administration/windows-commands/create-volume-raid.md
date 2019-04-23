---
title: 建立磁碟區的 raid
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432d661d8c0ce4cae6fe08a2671e8f9d613ce351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846779"
---
# <a name="create-volume-raid"></a>建立磁碟區的 raid

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立 RAID\-5 磁碟區使用三個或多個指定的動態磁碟。  
  
> [!IMPORTANT]  
> 無法在任何版本的 Windows Vista 中使用這個 DiskPart 命令。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|size\=<n>|以 mb 為單位的磁碟空間數量\(MB\)的磁碟區佔據每個磁碟上。 如果未指定大小，最大的可能 RAID\-將建立 5 個磁碟區。 具有最小可用連續剩餘空間的磁碟的 RAID，以決定大小\-5 磁碟區和相同的空間量就會從每個磁碟配置。 在 RAID 中的可用磁碟空間實際數量\-5 磁碟區是合併的磁碟空間量小於因為同位檢查需要部分磁碟空間。|  
|disk\=<n>,<n>,<n>\[,<n>,...\]|動態磁碟，在其上建立 RAID\-5 磁碟區。 您需要至少三個動態磁碟，才能建立 RAID\-5 磁碟區。 相等的空間量**大小\=<n>** 配置每個磁碟上。|  
|align\=<n>|對齊最接近對齊界限的所有磁碟區範圍。 多半搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。 *n*是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。|  
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新的磁碟區。  
  
## <a name="BKMK_examples"></a>範例  
若要建立 RAID\-5 磁碟區的大小，使用 1、 2 和 3，類型的磁碟的 1000 mb:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

