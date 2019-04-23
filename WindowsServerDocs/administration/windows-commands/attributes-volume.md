---
title: 屬性的磁碟區
description: 適用於 Windows 命令主題**屬性的磁碟區**-顯示、 集合或清除磁碟區的屬性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846579"
---
# <a name="attributes-volume"></a>屬性的磁碟區

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示、 設定，或清除磁碟區的屬性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|設定|設定具有焦點的磁碟區的指定的屬性。|  
|clear|清除指定的屬性具有焦點的磁碟區。|  
|唯讀|指定磁碟區讀取\-只。|  
|隱藏|指定的磁碟區為隱藏。|  
|nodefaultdriveletter|指定磁碟區不會預設接收磁碟機代號。|  
|陰影複製|指定磁碟區陰影複製磁碟區。|  
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   在基本主要開機記錄\(MBR\)磁碟**隱藏**， **readonly**，以及**nodefaultdriveletter**參數適用於所有的磁碟區上在磁碟中。  
  
-   在基本的 GUID 磁碟分割表格\(gpt\)磁碟和動態 MBR 和 gpt 磁碟上**隱藏**， **readonly**，和**nodefaultdriveletter**參數只適用於選取的磁碟區。  
  
-   必須選取一個磁碟區**屬性的磁碟區**命令才會成功。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要在選取的磁碟區上顯示目前的屬性，請輸入：  
  
```  
attributes volume  
```  
  
若要將選取的磁碟區設定為隱藏和讀取\-，輸入：  
  
```  
attributes volume set hidden readonly  
```  
  
若要移除的隱藏和讀取\-只在選取的磁碟區上的屬性型別：  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

