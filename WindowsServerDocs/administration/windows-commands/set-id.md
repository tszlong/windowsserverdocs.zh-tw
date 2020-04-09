---
title: 設定識別碼
description: 適用于 Diskpart 集合識別碼的 Windows 命令主題，會變更具有焦點之分割區的資料分割類型欄位。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c1a7eabac68ab2615b66c51c509e32c8d246096
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834531"
---
# <a name="set-id"></a>設定識別碼

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart Set ID 命令會變更具有焦點之分割區的資料分割類型欄位。  
  
> [!IMPORTANT]  
> 此命令僅供原始設備製造商使用，\(Oem\)。 使用此參數變更磁碟分割類型欄位可能會導致您的電腦失敗或無法開機。 除非您是 OEM 或有使用 gpt 磁片的經驗，否則您不應該使用此參數來變更 gpt 磁片上的磁碟分割類型欄位。 相反地，請一律使用[create partition efi](create-partition-efi.md)命令來建立 efi 系統磁碟分割、建立[磁碟分割 msr](create-partition-msr.md)命令以建立 Microsoft 保留的磁碟分割，以及建立不含 ID 參數的[create partition primary](create-partition-primary.md)命令，以在 gpt 磁片上建立主要磁碟分割。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數 |                                                                                                                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       對於主開機記錄 \(MBR\) 磁片，請針對資料分割指定類型欄位的新值（十六進位格式）。 除了指定 LDM 分割區的類型0x42 以外，您可以使用這個參數來指定任何資料分割類型 byte。 請注意，指定十六進位資料分割類型時，會省略前置的0x。                                                                                                                                                                                                       |
|  <GUID>   | 對於 GUID 磁碟分割表格 \(gpt\) 磁片，會為分割區的 [類型] 欄位指定新的 GUID 值。 可識別的 Guid 包括：<p>-EFI 系統磁碟分割： c12a7328\-f81f\-11d2\-ba4b\-00a0c93ec93b<br />-基本資料分割： ebd0a0a2\-b9e5\-4433\-87c0\-68b6b72699c7<p>任何資料分割類型 GUID 都可以使用此參數來指定，但下列除外：<p>-Microsoft Reserved partition： e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />-動態磁碟上的 LDM 中繼資料分割： 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-動態磁碟上的 LDM 資料磁碟分割： af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-叢集中繼資料分割： db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1 |
| 覆寫  |                                                                在變更磁碟分割類型之前，強制卸載磁片區上的檔案系統。 當您執行**Set id**命令時，DiskPart 會嘗試鎖定並卸載磁片區上的檔案系統。 如果未指定**override** ，而且鎖定檔案系統的呼叫失敗 \(例如，因為有開啟的控制碼\)，作業將會失敗。 當指定**override**時，DiskPart 會強制卸載，即使鎖定檔案系統的呼叫失敗，而且磁片區的任何開啟控制碼也會變成無效。<p>此命令僅適用于 Windows 7 和 Windows Server 2008 R2。                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>備註  
  
-   除了先前提到的限制以外，DiskPart 不會檢查您所 \(指定的值是否有效，除非您要確保它是十六進位格式或 GUID\)的位元組。  
  
-   此命令不適用於動態磁碟或 Microsoft 保留的分割區。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要將 [類型] 欄位設定為0x07，並強制卸載檔案系統，請輸入：  
  
```  
set id=0x07 override  
```  
  
若要將類型欄位設定為基本資料分割，請輸入：  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  

