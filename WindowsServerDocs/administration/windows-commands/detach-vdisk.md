---
title: 中斷連結 vdisk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b6a1ecd3d787506c89f120bed204cc30e6d68d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822729"
---
# <a name="detach-vdisk"></a>中斷連結 vdisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

停止選取的虛擬硬碟\(VHD\)為主機電腦上的本機硬碟磁碟機不會出現。 當已中斷連結 VHD 時，您可以將它複製到其他位置。  
  
> [!NOTE]  
> 此命令僅適用於 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>語法  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|noerr|用於僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|  
  
## <a name="remarks"></a>備註  
  
-   必須選取 VHD，並將其卸離此作業才會成功。 使用**選取 vdisk**命令來選取 VHD，並將焦點移到它。  
  
## <a name="BKMK_Examples"></a>範例  
若要卸離選取的 VHD，請輸入：  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [detail vdisk](detail-vdisk.md)  
  
-   [展開 vdisk](expand-vdisk.md)  
  
-   [合併 vdisk](merge-vdisk.md)  
  
-   [選取 vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

