---
title: create partition efi
description: 建立磁碟分割 efi 的 Windows 命令主題，它會在 Itanium 型電腦上的 GUID 磁碟分割表格（gpt）磁片上建立可擴充固件介面（EFI）系統磁碟分割。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c61f103fc719c8144e942f64172e3be554a414
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847041"
---
# <a name="create-partition-efi"></a>create partition efi

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Itanium 電腦上，會在 GUID 磁碟分割表格（gpt）磁片上建立可延伸韌體介面（EFI）系統磁碟分割。

## <a name="syntax"></a>語法  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  大小\=<n>  |                         分割區的大小（以 mb 為單位） \(MB\)。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。                         |
| offset\=<n> |             建立資料分割的位移（以 kb 為單位） \(KB\)。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。              |
|    noerr    | 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟分割之後，焦點會自動移到新磁碟分割。  
  
-   必須選取 gpt 磁片，此操作才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要在選取的磁片上建立 1000 mb 的 EFI 磁碟分割，請輸入：  
  
```  
create partition efi size=1000  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

