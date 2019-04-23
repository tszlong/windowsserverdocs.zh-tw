---
title: select disk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e8352b45cf3a46f14828c9c6796e4ec73499d5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858959"
---
# <a name="select-disk"></a>select disk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

選取指定的磁碟，並將焦點轉移到它。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> **<disk path>**， **system**，和**下一步**參數僅適用於 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|<n>|指定要接收焦點的磁碟數目。 您也可以使用電腦上檢視的所有磁碟的數字**清單中的磁碟**DiskPart 命令。 **注意：** 當有多個磁碟設定系統，請勿使用**選取磁碟\=0**指定系統磁碟。 當您重新啟動，而且不同的電腦具有相同的磁碟組態可以有不同的磁碟數字，則電腦可能會重新指派磁碟編號。|  
|<disk path>|指定要接收焦點，比方說，磁碟的位置**PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . 若要檢視磁碟的位置路徑，然後再輸入**詳細資料磁碟**。|  
|system|BIOS 的電腦上，指定該磁碟 0 收到焦點。 EFI 的電腦上，包含 EFI 系統磁碟分割的磁碟\(ESP\)用目前開機接收焦點。 EFI 的電腦上，如果沒有任何的 ESP，如果有一個以上的 ESP，或從 Windows 預先安裝環境開機電腦，將會失敗命令\(Windows PE\)。|  
|下一步|一旦選取一個磁碟之後，此命令會逐一磁碟清單中的所有磁碟。 當您執行此命令時，在清單中的下一個磁碟將會接收焦點。|  
  
## <a name="BKMK_examples"></a>範例  
若要將焦點移至磁碟 1，請輸入：  
  
```  
select disk=1  
```  
  
若要使用其位置路徑，以選取磁碟，請輸入：  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
若要將焦點移到系統磁碟，請輸入：  
  
```  
select disk=system  
```  
  
若要將焦點移到下一個磁碟的電腦上，輸入：  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  

