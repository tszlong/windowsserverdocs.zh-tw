---
title: 建立磁片區鏡像
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378874"
---
# <a name="create-volume-mirror"></a>建立磁片區鏡像

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用兩個指定的動態磁碟來建立磁片區鏡像。  
  
> [!NOTE]  
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|         參數         |                                                                                                                                     描述                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         大小\=<n>         |                 指定磁片區將在每個磁片上佔用的磁碟空間量（以 mb 為單位） \(MB\)。 如果未指定大小，則新磁片區會佔用最小磁片上的剩餘可用空間，以及每個後續磁片上的相同空間量。                 |
| 磁片\=<n>、<n>\[、<n>,...\] |                       指定要在其上建立鏡像磁碟區的動態磁碟。 您需要兩個動態磁碟來建立鏡像磁碟區。 系統會在每個磁片上配置與**size**參數所指定大小相等的空間量。                        |
|        \=對齊 <n>         | 將所有磁片區範圍對齊最接近的對齊界限。 此參數通常用於硬體 RAID 邏輯單元編號 \(LUN\) 陣列，以改善效能。 *n*是從磁片開頭到最接近之對齊界限的 KB \(kb\) 數。 |
|           noerr           |                                        僅用於腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束並出現錯誤。                                         |
  
## <a name="remarks"></a>備註  
  
-   建立磁片區之後，焦點會自動移至新的磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要建立大小為 1000 mb 的鏡像磁碟區，請在 [磁片 1] 和 [2] 上輸入：  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

