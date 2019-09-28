---
title: 建立磁片區 raid
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378859"
---
# <a name="create-volume-raid"></a>建立磁片區 raid

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用三個或多個指定的動態磁碟，建立 RAID @ no__t-05 磁片區。  
  
> [!IMPORTANT]  
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|           參數           |                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           size @ no__t-0 @ no__t-1           | 磁片區將在每個磁片上佔用的磁碟空間量（以 mb 為單位） \(MB @ no__t-1。 如果沒有指定大小，將會建立最大可能的 RAID @ no__t-05 磁片區。 具有最小可用連續可用空間的磁片，會決定 RAID @ no__t-05 磁片區的大小，並從每個磁片配置相同的空間量。 在 RAID @ no__t-05 磁片區中，可用磁碟空間的實際量小於磁碟空間的總和，因為同位檢查需要一些磁碟空間。 |
| disk @ no__t-0 @ no__t-1，<n>，<n> @ no__t-4，<n>,... \] |                                                                                                                                               要在其上建立 RAID @ no__t-05 磁片區的動態磁碟。 您至少需要三個動態磁碟，才能建立 RAID @ no__t-05 磁片區。 在每個磁片上配置**大小等於 @ no__t-1 @ no__t-2**的空間量。                                                                                                                                                |
|          align @ no__t-0 @ no__t-1           |                                                                                                                   將所有磁片區範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 \(LUN @ no__t-1 陣列以改善效能。 *n*是從磁片開頭到最接近對齊界限的 \(kb @ no__t-2 kb 數。                                                                                                                   |
|             noerr             |                                                                                                                                                 僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                                                  |
  
## <a name="remarks"></a>備註  
  
-   建立磁片區之後，焦點會自動移至新的磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要使用磁片1、2和3，建立大小為 1000 mb 的 RAID @ no__t-05 磁片區，請輸入：  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

