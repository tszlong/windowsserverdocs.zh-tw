---
title: 建立分割區擴充
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21620da46be0e1375f320172e7ccfe2edc338114
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378908"
---
# <a name="create-partition-extended"></a>建立分割區擴充

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在磁片上建立具有焦點的延伸磁碟分割。 您只能在主要開機記錄 \(MBR @ no__t-1 磁片上使用此命令。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                             描述                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |                                                  指定磁碟分割的大小（以 mb 為單位） \(MB @ no__t-1。 如果沒有指定大小，磁碟分割會繼續進行，直到延伸磁碟分割中沒有其他可用空間為止。                                                  |
| offset @ no__t-0 @ no__t-1 |                     指定分割區的位移（以 kb 為單位） \(KB @ no__t-1。 如果沒有指定位移，分割區會從磁片上可用空間的開頭開始，夠大而足以容納新的資料分割。                      |
| align @ no__t-0 @ no__t-1  | 將所有資料分割範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 \(LUN @ no__t-1 陣列以改善效能。 <n> 是從磁片開頭到最接近對齊界限的 kb 數 \(KB @ no__t-2。 |
|    noerr    |                                 僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                 |
  
## <a name="remarks"></a>備註  
  
-   建立分割區之後，焦點會自動移至新的分割區。  
  
-   每個磁片只能建立一個延伸磁碟分割。  
  
-   如果您嘗試在另一個延伸磁碟分割內建立延伸磁碟分割，則此命令會失敗。  
  
-   您必須先建立延伸磁碟分割，才能建立邏輯磁碟機。  
  
-   必須選取基本的 MBR 磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。  
  
## <a name="BKMK_examples"></a>典型  
若要建立大小為 1000 mb 的延伸磁碟分割，請輸入：  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

