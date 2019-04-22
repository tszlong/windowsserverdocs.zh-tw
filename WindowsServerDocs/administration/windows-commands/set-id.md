---
title: 集合識別碼
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f95490850acd263fb0b34007ac64a84c9a374865
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822049"
---
# <a name="set-id"></a>集合識別碼

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Diskpart 設定識別碼命令會變更具有焦點的磁碟分割的磁碟分割類型欄位。  
  
> [!IMPORTANT]  
> 此命令適用於由原始設備製造商\(Oem\)只。 變更使用此參數的資料分割類型欄位可能會導致您的電腦失敗或無法開機。 除非您是 OEM 或是有 gpt 磁碟經驗，否則您不應該使用此參數變更 gpt 磁碟上的磁碟分割類型欄位。 相反地，一律使用[建立資料分割 efi](create-partition-efi.md)命令來建立 EFI 系統磁碟分割，[建立 msr 磁碟分割](create-partition-msr.md)命令來建立 Microsoft Reserved 磁碟分割，而[建立主要磁碟分割](create-partition-primary.md)命令而不需要在 gpt 磁碟上建立主要磁碟分割的 ID 參數。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|<byte>|主開機記錄\(MBR\)磁碟、 分割區的十六進位格式指定 [類型] 欄位中，新值。 除了指定 LDM 磁碟分割的型別 0x42，這個參數可以指定任何磁碟分割類型位元組。 請注意，在指定十六進位的磁碟分割類型時，省略前置的 0x。|  
|<GUID>|針對 GUID 磁碟分割表格\(gpt\)磁碟中，指定分割區的 [類型] 欄位的新 GUID 值。 包含可辨識的 Guid:<br /><br />-   EFI system partition: c12a7328\-f81f\-11d2\-ba4b\-00a0c93ec93b<br />-基本資料磁碟分割： ebd0a0a2\-b9e5\-4433\-87 c 0\-68b6b72699c7<br /><br />可以指定任何磁碟分割類型 GUID，這個參數，但下列除外：<br /><br />-   Microsoft Reserved partition: e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />-LDM 中繼資料的磁碟分割上動態磁碟： 5808c8aa\-7e8f\-42e0\-85 d 2\-e1e90434cfb3<br />在動態磁碟上的 LDM 資料磁碟分割： af9b60a0\-1431年\-4f62\-bc68\-3311714a69ad<br />-叢集中繼資料的磁碟分割： db97dba9\-0840年\-4bae\-97f0\-ffb9a327c7e1|  
|覆寫|強制在變更磁碟分割類型之前卸載磁碟區上的檔案系統。 當您執行**集識別碼**命令時，DiskPart 會嘗試鎖定並解下磁碟區上的檔案系統。 如果**覆寫**未指定，以及鎖定的檔案系統呼叫失敗\(比方說，因為沒有開啟的控制代碼\)，則作業會失敗。 當**覆寫**指定時，DiskPart 會強制解下，即使鎖定的檔案系統呼叫失敗，且磁碟區任何開啟控制代碼會失效。<br /><br />此命令只適用於 Windows 7 和 Windows Server 2008 R2。|  
|noerr|用於僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   非先前所述的限制，DiskPart 不會檢查您所指定值的有效性\(但若要確定它在十六進位格式或 GUID 是一個位元組\)。  
  
-   此命令無法運作或 Microsoft Reserved 磁碟分割上的動態磁碟。  
  
## <a name="BKMK_examples"></a>範例  
若要設 0x07 中的 [類型] 欄位，而且強制卸載檔案系統，請輸入：  
  
```  
set id=0x07 override  
```  
  
若要設定為基本的資料分割區的 [類型] 欄位，請輸入：  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

