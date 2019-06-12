---
title: 建立 efi 磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 99970fba41a747a6bb4b1ca6cc4b7f603c547790
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434166"
---
# <a name="create-partition-efi"></a>建立 efi 磁碟分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Itanium\-電腦，會建立可延伸韌體介面\(EFI\) GUID 磁碟分割資料表上的系統磁碟分割\(gpt\)磁碟。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                         以 mb 為單位的磁碟分割的大小\(MB\)。 如果未指定大小，磁碟分割會繼續，直到目前的區域中沒有更多的可用空間。                         |
| offset\=<n> |             以 kb 為單位的位移\(KB\)，在建立資料分割。 如果沒有指定位移，資料分割會置於足夠容納它的第一個磁碟範圍內。              |
|    noerr    | 針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。 |
  
## <a name="remarks"></a>備註  
  
-   在建立資料分割之後，新的資料分割提供焦點。  
  
-   這項作業成功時，必須選取 gpt 磁碟。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要在所選磁碟上建立 EFI 磁碟分割的 1000 mb，請輸入：  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

