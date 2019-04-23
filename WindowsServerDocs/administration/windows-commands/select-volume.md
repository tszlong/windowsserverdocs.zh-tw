---
title: select volume
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0ebf9896621268c384ea8129d32c985028054d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890729"
---
# <a name="select-volume"></a>select volume

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

選取指定的磁碟區，並將焦點轉移到它。 此命令也可用來顯示目前的焦點在所選磁碟的磁碟區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|<n>|要接收焦點的磁碟區數目。 您可以檢視所有磁碟區的數字上使用目前選取的磁碟**列出磁碟區**DiskPart 命令。|  
|<d>|磁碟機代號或掛接點路徑要接收焦點的磁碟區。|  
  
## <a name="remarks"></a>備註  
  
-   如果未不指定任何磁碟區，則此命令會顯示目前的焦點在所選磁碟的磁碟區。  
  
-   在基本磁碟上，選取磁碟區也會將焦點置於對應的資料分割。  
  
-   如果有對應的資料分割選取磁碟區，則將會自動選取資料分割。  
  
-   如果已選取的分割區，與對應的磁碟區，將會自動選取磁碟區。  
  
## <a name="BKMK_examples"></a>範例  
若要將焦點移到 2 的磁碟區，輸入：  
  
```  
select volume=2  
```  
  
若要將焦點移到磁碟機 C，輸入：  
  
```  
select volume=c  
```  
  
若要將焦點移至裝載在名為"mountpath"的資料夾上的磁碟區，輸入：  
  
```  
select volume=c:\mountpath  
```  
  
若要顯示目前的焦點在所選磁碟的磁碟區，請輸入：  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

