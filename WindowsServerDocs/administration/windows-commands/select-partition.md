---
title: 選取資料分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79449bc74dd09246b380b3f892acc1b338650d20
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441507"
---
# <a name="select-partition"></a>選取資料分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

選取指定的資料分割，並將焦點轉移到它。 此命令也可用來顯示目前的焦點在所選磁碟的磁碟分割。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>參數  
  
|   參數    |                                                                                    描述                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| partition\=<n> | 要接收焦點的磁碟分割數目。 您可以檢視所有資料分割的數字上使用目前選取的磁碟**列出資料分割**DiskPart 命令。 |
  
## <a name="remarks"></a>備註  
  
-   您可以選取資料分割之前，您必須先選取磁碟，使用**選取磁碟**命令。  
  
-   如果未不指定任何資料分割編號，則此命令會顯示目前的焦點在所選磁碟的磁碟分割。  
  
-   如果有對應的資料分割選取磁碟區，則將會自動選取資料分割。  
  
-   如果已選取的分割區，與對應的磁碟區，將會自動選取磁碟區。  
  
## <a name="BKMK_examples"></a>範例  
若要將焦點移至 資料分割 3，輸入：  
  
```  
select partitition=3  
```  
  
若要顯示目前的焦點在所選磁碟的磁碟分割，請輸入：  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

