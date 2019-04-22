---
title: 修復
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 940b0931671d5f3c2137fafe4ae73b7cecd0160e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821189"
---
# <a name="repair"></a>修復

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

修復 RAID\-具有焦點，以指定的動態磁碟取代失敗的磁碟區的 5 個磁碟區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|disk\=<n>|指定將會取代故障的磁碟區的動態磁碟。|  
|align\=<n>|對齊最接近對齊界限的所有磁碟區或分割區範圍。 *n*是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。|  
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   指定的動態磁碟的可用空間大於或等於在 RAID 中失敗的磁碟區的大小總計必須\-5 磁碟區。  
  
-   在 RAID 磁碟區\-5 陣列必須選取此作業才會成功。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要更換具有焦點的磁碟區，它取代成動態磁碟 4，請輸入：  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

