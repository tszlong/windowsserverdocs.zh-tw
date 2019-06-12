---
title: extend
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fdf070a733392d89bafe5bed5a1bf23d8e24d57
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439349"
---
# <a name="extend"></a>extend

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將具有焦點和其檔案系統磁碟分割的磁碟區延伸到免費\(未配置\)磁碟上的空間。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size\=<n>  |      以 mb 為單位指定的空間量\(MB\)来加入至目前的磁碟區或磁碟分割。 如果未指定大小，所有的可用磁碟的連續可用空間會使用它。       |
| disk\=<n>  |                          指定已擴充的磁碟區或磁碟分割的磁碟。 如果未不指定任何磁碟，磁碟區或磁碟分割會延伸目前的磁碟上。                          |
| 檔案系統 |                                   擴充的檔案系統，具有焦點的磁碟區。 只能在未與磁碟區擴充檔案系統的磁碟上使用。                                    |
|   noerr    | 針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。 |
  
## <a name="remarks"></a>備註  
  
-   基本磁碟上的可用空間必須是相同的磁碟與磁碟區或具有焦點的磁碟分割上。 它也會立即必須遵循的磁碟區或磁碟分割具有焦點\(也就是它必須啟動下一個磁區位移\)。  
  
-   使用簡單或跨距磁碟區的動態磁碟上的磁碟區可以擴充任何動態磁碟上任何可用空間。 使用這個命令，您可以將簡單的動態磁碟區轉換成跨距的動態磁碟區。 建立鏡像，RAID\-5 與等量磁碟區無法擴充。  
  
-   如果先前使用 NTFS 檔案系統格式化磁碟分割，檔案系統會自動延伸以填滿較大的磁碟分割，會發生資料遺失的狀況。  
  
-   如果先前以不同於 NTFS 檔案系統格式化磁碟分割，則命令會失敗且不會變更至磁碟分割。  
  
-   如果不使用檔案系統先前格式化磁碟分割，將仍會延伸磁碟分割。  
  
-   資料分割必須具有相關聯的磁碟區，然後可加以擴充。  
  
## <a name="BKMK_examples"></a>範例  
若要擴充 500 mb，3，磁碟上的磁碟區或具有焦點的磁碟分割中，輸入：  
  
```  
extend size=500 disk=3  
```  
  
若要延伸磁碟區的檔案系統之後它已擴充,，請輸入：  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

