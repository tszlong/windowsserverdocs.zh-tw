---
title: 糾正
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88293422519488405d94e32596c81dbe4a697dee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371535"
---
# <a name="repair"></a>糾正

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以指定的動態磁碟取代失敗的磁片區域，以修復具有焦點的 RAID @ no__t-05 磁片區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disk @ no__t-0 @ no__t-1  |                                                                 指定將取代失敗磁片區域的動態磁碟。                                                                 |
| align @ no__t-0 @ no__t-1 |          將所有磁片區或資料分割範圍對齊最接近的對齊界限。 *n*是從磁片開頭到最接近對齊界限的 \(kb @ no__t-2 kb 數。           |
|   noerr    | 僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
  
## <a name="remarks"></a>備註  
  
-   指定的動態磁碟必須要有大於或等於 RAID @ no__t-05 磁片區中失敗磁片區域大小總計的可用空間。  
  
-   必須選取 RAID @ no__t-05 陣列中的磁片區，此操作才會成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要將具有焦點的磁片區取代為動態磁碟4，請輸入：  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

