---
title: 建立 msr 磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3fa9ba46418c3ed3b7999a734b4c0df40dce5027
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434171"
---
# <a name="create-partition-msr"></a>建立 msr 磁碟分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立 Microsoft Reserved \(MSR\) GUID 磁碟分割表格上的磁碟分割\(gpt\)磁碟。  
  
> [!CAUTION]  
> 務必特別小心使用此命令。 因為 gpt 磁碟需要特定資料分割配置，建立 Microsoft Reserved 磁碟分割可能會造成磁碟變成無法存取。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                         描述                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |               以 mb 為單位的磁碟分割的大小\(MB\)。 指定的數字為至少長度以位元組為單位的磁碟分割<n>。 如果未指定大小，磁碟分割會繼續，直到目前的區域中沒有更多的可用空間。               |
| offset\=<n> | 指定的位移，以 kb 為單位\(KB\)，在建立資料分割。 位移會以完全填滿磁區的大小會無條件進位。 如果沒有指定位移，資料分割會置於足夠容納它的第一個磁碟範圍內。 |
|    noerr    |                            針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。                             |
  
## <a name="remarks"></a>備註  
  
-   用來啟動 Windows 作業系統，可延伸韌體介面的 gpt 磁碟上\(EFI\)系統磁碟分割是磁碟，後面接著 Microsoft Reserved 磁碟分割的第一個磁碟分割。 只能用於資料儲存體的 gpt 磁碟沒有 EFI 系統磁碟分割，大小寫的 Microsoft Reserved 磁碟分割的第一個資料分割。  
  
-   Windows 無法裝載 Microsoft Reserved 磁碟分割。 您無法在其上儲存資料，並無法予以刪除。  
  
-   每個 gpt 磁碟上需要 Microsoft Reserved 磁碟分割。 此資料分割的大小取決於 gpt 磁碟的總大小。 Gpt 磁碟的大小必須至少 32 MB，才能建立 Microsoft Reserved 磁碟分割。  
  
-   這項作業成功時，必須選取基本的 gpt 磁碟。 使用**選取磁碟**命令來選取基本的 gpt 磁碟，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要建立 Microsoft Reserved 磁碟分割的 1000 mb 的大小，請輸入：  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

