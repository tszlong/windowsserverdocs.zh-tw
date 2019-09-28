---
title: 設定識別碼
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b48cc701716412c4a79cedddb4458c57ba25ad5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384070"
---
# <a name="set-id"></a>設定識別碼

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart Set ID 命令會變更具有焦點之分割區的資料分割類型欄位。  
  
> [!IMPORTANT]  
> 此命令僅供原始設備製造商使用，\(OEMs @ no__t-1。 使用此參數變更磁碟分割類型欄位可能會導致您的電腦失敗或無法開機。 除非您是 OEM 或有使用 gpt 磁片的經驗，否則您不應該使用此參數來變更 gpt 磁片上的磁碟分割類型欄位。 相反地，請一律使用[create partition efi](create-partition-efi.md)命令來建立 efi 系統磁碟分割、建立[磁碟分割 msr](create-partition-msr.md)命令以建立 Microsoft 保留的磁碟分割，以及建立不含 ID 參數的[partition primary](create-partition-primary.md)命令在 gpt 磁片上建立主要磁碟分割。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數 |                                                                                                                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       對於主開機記錄 \(MBR @ no__t-1 磁片，為數據分割指定類型欄位的新值（十六進位格式）。 除了指定 LDM 分割區的類型0x42 以外，您可以使用這個參數來指定任何資料分割類型 byte。 請注意，指定十六進位資料分割類型時，會省略前置的0x。                                                                                                                                                                                                       |
|  <GUID>   | 對於 GUID 磁碟分割資料表 \(gpt @ no__t-1 磁片，為數據分割的 [類型] 欄位指定新的 GUID 值。 可識別的 Guid 包括：<br /><br />-EFI 系統磁碟分割： c12a7328 @ no__t-0f81f @ no__t-111d2 @ no__t-2ba4b @ no__t-300a0c93ec93b<br />-基本資料分割： ebd0a0a2 @ no__t-0b9e5 @ no__t-14433 @ no__t-287c0 @ no__t-368b6b72699c7<br /><br />任何資料分割類型 GUID 都可以使用此參數來指定，但下列除外：<br /><br />-Microsoft Reserved partition： e3c9e316 @ no__t-00b5c @ no__t-14db8 @ no__t-2817d @ no__t-3f92df00215ae<br />-動態磁碟上的 LDM 中繼資料分割： 5808c8aa @ no__t-07e8f @ no__t-142e0 @ no__t-285d2 @ no__t-3e1e90434cfb3<br />-動態磁碟上的 LDM 資料磁碟分割： af9b60a0 @ no__t-01431 @ no__t-14f62 @ no__t-2bc68 @ no__t-33311714a69ad<br />-Cluster metadata partition： db97dba9 @ no__t-00840 @ no__t-14bae @ no__t-297f0 @ no__t-3ffb9a327c7e1 |
| 覆寫  |                                                                在變更磁碟分割類型之前，強制卸載磁片區上的檔案系統。 當您執行**Set id**命令時，DiskPart 會嘗試鎖定並卸載磁片區上的檔案系統。 如果未指定**override** ，而且鎖定檔案系統的呼叫失敗 @no__t 1for 範例中，因為有一個開啟控制碼 @ no__t-2，所以作業將會失敗。 當指定**override**時，DiskPart 會強制卸載，即使鎖定檔案系統的呼叫失敗，而且磁片區的任何開啟控制碼也會變成無效。<br /><br />此命令僅適用于 Windows 7 和 Windows Server 2008 R2。                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    僅用於腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>備註  
  
-   除了先前所述的限制以外，DiskPart 不會檢查您指定的值是否有效 @no__t 0except，以確保它是十六進位格式或 GUID @ no__t-1 的位元組。  
  
-   此命令不適用於動態磁碟或 Microsoft 保留的分割區。  
  
## <a name="BKMK_examples"></a>典型  
若要將 [類型] 欄位設定為0x07，並強制卸載檔案系統，請輸入：  
  
```  
set id=0x07 override  
```  
  
若要將類型欄位設定為基本資料分割，請輸入：  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

