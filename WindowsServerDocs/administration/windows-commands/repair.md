---
title: 修復
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89c09608e64b408db9c7c79269046195e005c187
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722386"
---
# <a name="repair"></a>修復

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以指定的\-動態磁碟取代失敗的磁片區域，以修復具有焦點的 RAID 5 磁片區。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 硬碟\=<n>  |                                                                 指定將取代失敗磁片區域的動態磁碟。                                                                 |
| 對齊\=<n> |          將所有磁片區或資料分割範圍對齊最接近的對齊界限。 *n*是從磁片開頭到\(最\)接近對齊界限的 kb kb 數。           |
|   noerr    | 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
  
## <a name="remarks"></a>備註  
  
-   指定的動態磁碟的可用空間必須大於或等於 RAID\-5 磁片區中失敗磁片區域的總大小。  
  
-   必須選取 RAID\-5 陣列中的磁片區，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。  
  
## <a name="examples"></a>範例  
若要將具有焦點的磁片區取代為動態磁碟4，請輸入：  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

