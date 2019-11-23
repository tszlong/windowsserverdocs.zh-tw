---
title: 卸離 vdisk
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4850f9f17218178f210820dd4c6ca96fd918accc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378695"
---
# <a name="detach-vdisk"></a>卸離 vdisk

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止選取的虛擬硬碟 \(VHD\) 不會顯示為主電腦上的本機硬碟。 卸離 VHD 之後，您可以將它複製到其他位置。  
  
> [!NOTE]  
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>語法  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Parameters  
  
|參數|描述|  
|-------|--------|  
|noerr|僅用於腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|  
  
## <a name="remarks"></a>備註  
  
-   必須選取並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。  
  
## <a name="BKMK_Examples"></a>典型  
若要卸離選取的 VHD，請輸入：  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [附加 vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [詳細資料 vdisk](detail-vdisk.md)  
  
-   [展開 vdisk](expand-vdisk.md)  
  
-   [合併 vdisk](merge-vdisk.md)  
  
-   [選取 vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

