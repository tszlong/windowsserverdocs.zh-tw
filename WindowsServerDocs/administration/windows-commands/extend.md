---
title: extend
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11991f9fc338dca5201d8f9c9c598b9d7dcf239b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844781"
---
# <a name="extend"></a>extend

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將具有焦點的磁片區或磁碟分割，並將其檔案系統延伸到磁片上的可用 \(未配置\) 空間。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數  |                                                                                             描述                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 大小\=<n>  |      指定要新增至目前磁片區或磁碟分割的空間量（以 mb 為單位） \(MB\)。 如果未指定大小，則會使用磁片上可用的所有連續可用空間。       |
| 磁片\=<n>  |                          指定擴充磁片區或分割區的磁片。 如果未指定磁片，磁片區或磁碟分割會在目前的磁片上擴充。                          |
| 內 |                                   以焦點擴充磁片區的檔案系統。 僅適用于未使用磁片區擴充檔案系統的磁片。                                    |
|   noerr    | 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
  
## <a name="remarks"></a>備註  
  
-   在基本磁碟上，可用空間必須與具有焦點的磁片區或磁碟分割位於相同的磁片上。 它也必須立即追蹤具有焦點的磁片區或分割區 \(也就是必須從下一個磁區位移\)開始。  
  
-   在具有簡單或跨距磁片區的動態磁碟上，可以將磁片區延伸到任何動態磁碟上的任何可用空間。 使用此命令，您可以將簡單的動態磁碟區轉換成跨距動態磁碟區。 鏡像、RAID\-5 和等量磁片區無法擴充。  
  
-   如果先前已使用 NTFS 檔案系統格式化磁碟分割，檔案系統會自動擴充以填滿較大的磁碟分割，且不會遺失任何資料。  
  
-   如果磁碟分割先前是使用 NTFS 以外的檔案系統進行格式化，則此命令會失敗，且磁碟分割不會變更。  
  
-   如果先前未使用檔案系統格式化磁碟分割，則磁碟分割仍然會延伸。  
  
-   分割區必須具有相關聯的磁片區，才能進行擴充。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要將焦點延伸到 500 mb 的磁片區或磁碟分割，請在磁片3上輸入：  
  
```  
extend size=500 disk=3  
```  
  
若要擴充擴充之後磁片區的檔案系統，請輸入：  
  
```  
extend filesystem  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

