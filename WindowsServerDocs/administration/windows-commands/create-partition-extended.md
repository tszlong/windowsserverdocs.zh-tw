---
title: 建立擴充的磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0a1cca93a064cfb6e5c18f4a472ea837b922d07b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434189"
---
# <a name="create-partition-extended"></a>建立擴充的磁碟分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立延伸磁碟分割具有焦點的磁碟上。 您可以使用此命令只會在主開機記錄\(MBR\)磁碟。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                             描述                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                  指定資料分割的大小以 mb 為單位\(MB\)。 如果未指定大小，磁碟分割會繼續直到延伸磁碟分割沒有更多的可用空間。                                                  |
| offset\=<n> |                     指定的位移，以 kb 為單位\(KB\)，在建立資料分割。 如果沒有指定位移，資料分割將會啟動夠大，無法存放新的資料分割的磁碟上的可用空間的開頭。                      |
| align\=<n>  | 對齊最接近對齊界限的所有分割區範圍。 多半搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。 <n> 是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。 |
|    noerr    |                                 針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。                                 |
  
## <a name="remarks"></a>備註  
  
-   在建立資料分割之後，焦點會自動移到新的磁碟分割。  
  
-   每個磁碟，您可以建立只有一個延伸磁碟分割。  
  
-   如果您嘗試建立另一個延伸磁碟分割內的延伸磁碟分割，此命令將會失敗。  
  
-   您可以建立邏輯磁碟機之前，您必須建立延伸磁碟分割。  
  
-   這項作業成功時，必須選取一個基本 MBR 磁碟。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要建立延伸磁碟分割的 1000 mb 的大小，請輸入：  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

