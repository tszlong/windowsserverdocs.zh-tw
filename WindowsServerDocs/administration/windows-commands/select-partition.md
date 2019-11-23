---
title: 選取資料分割
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a186e2678fde64396a8b4b57a2d14e4b0b7bf26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371066"
---
# <a name="select-partition"></a>選取資料分割

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁碟分割，並將焦點移至該分割區。 此命令也可以用來顯示目前在選取的磁片中有焦點的分割區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parameters  
  
|   參數    |                                                                                    描述                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 分割區\=<n> | 要接收焦點的資料分割編號。 您可以使用 DiskPart 中的 [**清單磁碟分割**] 命令，來查看目前選取之磁片上的所有資料分割編號。 |
  
## <a name="remarks"></a>備註  
  
-   您必須先使用 [**選取磁片**] 命令來選取磁片，才能選取資料分割。  
  
-   如果未指定分割區編號，此命令會顯示目前在所選磁片中具有焦點的分割區。  
  
-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。  
  
-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。  
  
## <a name="BKMK_examples"></a>典型  
若要將焦點移至資料分割3，請輸入：  
  
```  
select partitition=3  
```  
  
若要顯示目前在所選磁片中具有焦點的資料分割，請輸入：  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

