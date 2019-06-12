---
title: 建立簡單磁碟區
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a35d0de5110c0e1616c42921c8402ecc1aff8c41
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434048"
---
# <a name="create-volume-simple"></a>建立簡單磁碟區

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

指定的動態磁碟上建立簡單磁碟區。  
  
> [!IMPORTANT]  
> 適用於 Windows Vista，此 DiskPart 命令才會提供 Windows Vista Ultimate、 Windows Vista Enterprise 和 Windows Vista Business 版本。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數  |                                                                                                                            描述                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size\=<n>  |                                                                  以 mb 為單位的磁碟區的大小\(MB\)。 如果未指定大小，新的磁碟區會佔用磁碟上剩餘的可用空間。                                                                   |
| disk\=<n>  |                                                                                建立磁碟區所在之動態磁碟。 如果未不指定任何磁碟，則會使用目前的磁碟。                                                                                |
| align\=<n> | 對齊最接近對齊界限的所有磁碟區範圍。 多半搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。 *n*是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。 |
|   noerr    |                               針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。                                |
  
## <a name="remarks"></a>備註  
  
-   建立磁碟區之後，焦點會自動移到新的磁碟區。  
  
## <a name="BKMK_examples"></a>範例  
若要建立 1000 mb 的磁碟區的大小，在 磁碟 1 上輸入：  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

