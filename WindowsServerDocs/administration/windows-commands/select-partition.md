---
title: select partition
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97145d73cbbe1bdc9b27e545b047b78fe89e4984
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834801"
---
# <a name="select-partition"></a>select partition

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁碟分割，並將焦點移至該分割區。 此命令也可以用來顯示目前在選取的磁片中有焦點的分割區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
select partition=<n>  
```  
  
### <a name="parameters"></a>參數  
  
|   參數    |                                                                                    描述                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 分割區\=<n> | 要接收焦點的資料分割編號。 您可以使用 DiskPart 中的 [**清單磁碟分割**] 命令，來查看目前選取之磁片上的所有資料分割編號。 |
  
## <a name="remarks"></a>備註  
  
-   您必須先使用 [**選取磁片**] 命令來選取磁片，才能選取資料分割。  
  
-   如果未指定分割區編號，此命令會顯示目前在所選磁片中具有焦點的分割區。  
  
-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。  
  
-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要將焦點移至資料分割3，請輸入：  
  
```  
select partitition=3  
```  
  
若要顯示目前在所選磁片中具有焦點的資料分割，請輸入：  
  
```  
select partition  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

