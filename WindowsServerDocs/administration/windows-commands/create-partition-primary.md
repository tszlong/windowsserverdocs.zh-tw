---
title: 建立主要磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881709"
---
# <a name="create-partition-primary"></a>建立主要磁碟分割

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在具有焦點的基本磁碟上建立主要磁碟分割。  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>語法  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|size\=<n>|指定資料分割的大小以 mb 為單位\(MB\)。 如果未指定大小，則磁碟分割會繼續直到沒有多餘的空間中目前的區域。|  
|offset\=<n>|以 kb 為單位的位移\(KB\)，在建立資料分割。 如果沒有指定位移，資料分割將會啟動夠大，足以容納它的最大磁碟範圍的開頭。|  
|align\=<n>|對齊最接近對齊界限的所有分割區範圍。 多半搭配硬體 RAID 邏輯單元編號\(LUN\)陣列來改善效能。 <n> 是的 kb 數\(KB\)從開始到最接近對齊界限的磁碟。|  
|id\={ <byte> &#124; <guid> }|指定磁碟分割類型。 這個參數供原始設備製造商\(OEM\)僅使用。 使用這個參數，可以指定任何磁碟分割類型位元組或 GUID。 DiskPart 不檢查磁碟分割類型，但若要確定它在十六進位格式或 GUID 是一個位元組的有效性。 **注意**：使用此參數建立資料分割，可能會導致您的電腦失敗或無法啟動。 除非您是 OEM 或是有 gpt 磁碟經驗的 IT 專業人員，請勿使用此參數的 gpt 磁碟上建立資料分割。 相反地，一律使用**建立資料分割 efi**命令來建立 EFI 系統磁碟分割，**建立 msr 磁碟分割**命令來建立 Microsoft Reserved 磁碟分割，而**建立主要的資料分割**命令\(不含**識別碼\={位元組&#124;guid}** 參數\)gpt 磁碟上建立主要磁碟分割。<br /><br />**主開機記錄磁碟**<br /><br />主開機記錄\(MBR\)磁碟，您可以指定磁碟分割類型位元組，十六進位格式，資料分割。 如果 MBR 磁碟不指定這個參數，命令就會建立型別的 0x06:sp，其指定未安裝 檔案系統磁碟分割。 範例包含：<br /><br />-LDM 資料磁碟分割：0x42<br />-修復磁碟分割：0x27<br />-可辨識的 OEM 磁碟分割：0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**GUID 磁碟分割表格磁碟**<br /><br />針對 GUID 磁碟分割表格\(gpt\)磁碟，您可以指定您想要建立資料分割的磁碟分割類型 GUID。 包含可辨識的 Guid:<br /><br />-   EFI system partition: c12a7328\-f81f\-11d2\-ba4b\-00a0c93ec93b<br />-   Microsoft Reserved partition: e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />-基本資料磁碟分割： ebd0a0a2\-b9e5\-4433\-87 c 0\-68b6b72699c7<br />在動態磁碟上的 LDM 中繼資料磁碟分割：5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />在動態磁碟上的 LDM 資料磁碟分割： af9b60a0\-1431年\-4f62\-bc68\-3311714a69ad<br />-修復磁碟分割： de94bba4\-06 d 1\-4d 40\-a16a\-bfd50179d6ac<br /><br />如果這個參數未指定 gpt 磁碟中，命令會建立基本資料磁碟分割。|  
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有 noerr 參數中，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   建立資料分割之後，焦點會自動移到新的磁碟分割。  
  
-   磁碟分割不接收磁碟機代號。 您必須使用**指派**指派磁碟機代號給磁碟分割的 DiskPart 命令。  
  
-   這項作業成功時，必須選取基本磁碟。 使用**選取磁碟**命令來選取基本磁碟，並將焦點移到它。  
  
## <a name="BKMK_examples"></a>範例  
若要建立主要磁碟分割的 1000 mb 的大小，請輸入：  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

