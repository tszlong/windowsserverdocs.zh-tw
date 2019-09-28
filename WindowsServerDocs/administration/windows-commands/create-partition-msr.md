---
title: 建立磁碟分割 msr
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45cc215b097ce048b15f0e907f95f976e4941e28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378905"
---
# <a name="create-partition-msr"></a>建立磁碟分割 msr

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 GUID 磁碟分割資料表上建立 Microsoft Reserved \(MSR @ no__t 分割區，\(gpt @ no__t-3 磁片。  
  
> [!CAUTION]  
> 使用此命令時，請務必小心。 因為 gpt 磁片需要特定的磁碟分割配置，所以建立 Microsoft 保留的磁碟分割可能會導致磁片變成無法讀取。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                         描述                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |               分割區的大小（以 mb 為單位） \(MB @ no__t-1。 資料分割至少是以位元組為單位，<n> 所指定的數位。 如果未指定大小，則磁碟分割會繼續，直到目前的區域中沒有更多可用空間為止。               |
| offset @ no__t-0 @ no__t-1 | 指定分割區的位移（以 kb 為單位） \(KB @ no__t-1。 位移會無條件進位，以完全填滿所使用的任何磁區大小。 如果沒有指定位移，磁碟分割就會放在夠大的第一個磁片範圍中以容納它。 |
|    noerr    |                            僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                             |
  
## <a name="remarks"></a>備註  
  
-   在用來啟動 Windows 作業系統的 gpt 磁片上，可延伸韌體介面 \(EFI @ no__t-1 系統磁碟分割是磁片上的第一個磁碟分割，後面接著 Microsoft 保留的磁碟分割。 僅用於資料儲存體的 gpt 磁片沒有 EFI 系統磁碟分割，在此情況下，Microsoft 保留的磁碟分割為第一個磁碟分割。  
  
-   Windows 不會掛接 Microsoft 保留的磁碟分割。 您無法在其上儲存資料，也無法將其刪除。  
  
-   每個 gpt 磁片上都必須有 Microsoft 保留的磁碟分割。 此分割區的大小取決於 gpt 磁片的總大小。 Gpt 磁片的大小至少必須為 32 MB，才能建立 Microsoft 保留的磁碟分割。  
  
-   必須選取基本 gpt 磁片，此操作才能成功。 使用 [**選取磁片**] 命令來選取基本的 gpt 磁片，並將焦點移至其上。  
  
## <a name="BKMK_examples"></a>典型  
若要建立大小為 1000 mb 的 Microsoft 保留磁碟分割，請輸入：  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

