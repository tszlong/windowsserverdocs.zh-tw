---
title: 建立磁碟分割 efi
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d97129fd67345f23eee2fc7b300493a1632cc6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379015"
---
# <a name="create-partition-efi"></a>建立磁碟分割 efi

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Itanium @ no__t-0based 電腦上，會在 GUID 磁碟分割資料表上建立可擴充固件介面 \(EFI @ no__t-2 \(gpt @ no__t-4 磁片。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |                         分割區的大小（以 mb 為單位） \(MB @ no__t-1。 如果未指定大小，則磁碟分割會繼續，直到目前的區域中沒有更多可用空間為止。                         |
| offset @ no__t-0 @ no__t-1 |             建立資料分割的位移（以 kb 為單位） \(KB @ no__t-1。 如果沒有指定位移，磁碟分割就會放在夠大的第一個磁片範圍中以容納它。              |
|    noerr    | 僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
  
## <a name="remarks"></a>備註  
  
-   建立分割區之後，會將焦點提供給新的資料分割。  
  
-   必須選取 gpt 磁片，此操作才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。  
  
## <a name="BKMK_examples"></a>典型  
若要在選取的磁片上建立 1000 mb 的 EFI 磁碟分割，請輸入：  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

