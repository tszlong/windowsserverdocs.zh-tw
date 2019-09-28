---
title: 屬性磁片區
description: '**屬性磁片**區的 Windows 命令主題-顯示、設定或清除磁片區的屬性。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382578"
---
# <a name="attributes-volume"></a>屬性磁片區

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示、設定或清除磁片區的屬性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|設定|設定具有焦點之磁片區的指定屬性。|  
|clear|清除具有焦點之磁片區的指定屬性。|  
|唯讀|指定磁片區為讀取 @ no__t-0only。|  
|隱含|指定隱藏磁片區。|  
|nodefaultdriveletter|指定磁片區預設不會接收磁碟機號。|  
|拷貝|指定磁片區為陰影複製磁片區。|  
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|  
  
## <a name="remarks"></a>備註  
  
-   在基本的主要開機記錄 \(MBR @ no__t-1 磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數會套用到磁片上的所有磁片區。  
  
-   在基本 GUID 磁碟分割資料表上 \(gpt @ no__t-1 磁片，而在動態 MBR 和 gpt 磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數只適用于選取的磁片區。  
  
-   必須選取磁片區，**屬性 volume**命令才會成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要在選取的磁片區上顯示目前的屬性，請輸入：  
  
```  
attributes volume  
```  
  
若要將選取的磁片區設定為 hidden 和 read @ no__t-0only，請輸入：  
  
```  
attributes volume set hidden readonly  
```  
  
若要移除所選磁片區上的 hidden 和 read @ no__t-0only 屬性，請輸入：  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

