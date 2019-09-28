---
title: 選取 vdisk
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bfa6450d1704cde1e5ff2a50a8e3b61a30d0766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384186"
---
# <a name="select-vdisk"></a>選取 vdisk

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的虛擬硬碟 \(VHD @ no__t-1，並將焦點轉移到其上。  
  
> [!NOTE]  
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>語法  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|file @ no__t-0 @ no__t-1|指定現有 VHD 檔案的完整路徑和檔案名。|  
|noerr|僅用於腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|  
  
## <a name="BKMK_examples"></a>典型  
若要將焦點移至名為 Test .vhd 的 VHD，請輸入：  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [附加 vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [卸離 vdisk](detach-vdisk.md)  
  
-   [詳細資料 vdisk](detail-vdisk.md)  
  
-   [展開 vdisk](expand-vdisk.md)  
  
-   [合併 vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

