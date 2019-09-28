---
title: 建立磁片區簡單
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1afb97c5bdb167eaf6ecfcd34ca3607b7b5a4c71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378876"
---
# <a name="create-volume-simple"></a>建立磁片區簡單

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在指定的動態磁碟上建立簡單磁片區。  
  
> [!IMPORTANT]  
> 對於 Windows Vista，此 DiskPart 命令僅適用于 Windows Vista 旗艦版、Windows Vista Enterprise 和 Windows Vista Business edition。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數  |                                                                                                                            描述                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size @ no__t-0 @ no__t-1  |                                                                  磁片區的大小（以 mb 為單位） \(MB @ no__t-1。 如果未指定大小，則新磁片區會佔用磁片上剩餘的可用空間。                                                                   |
| disk @ no__t-0 @ no__t-1  |                                                                                要在其上建立磁片區的動態磁碟。 如果未指定任何磁片，則會使用目前的磁片。                                                                                |
| align @ no__t-0 @ no__t-1 | 將所有磁片區範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 \(LUN @ no__t-1 陣列以改善效能。 *n*是從磁片開頭到最接近對齊界限的 \(kb @ no__t-2 kb 數。 |
|   noerr    |                               僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                |
  
## <a name="remarks"></a>備註  
  
-   建立磁片區之後，焦點會自動移至新的磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要建立大小為 1000 mb 的磁片區，請在 disk 1 上輸入：  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

