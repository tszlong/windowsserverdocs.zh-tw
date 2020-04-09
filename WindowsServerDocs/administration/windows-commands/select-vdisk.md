---
title: 選取 vdisk
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65e186413bebbf467cd4c2033d274badd1fbea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834741"
---
# <a name="select-vdisk"></a>選取 vdisk

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

\(VHD\) 選取指定的虛擬硬碟，並將焦點移至該磁片。  
  
> [!NOTE]  
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。  
  
## <a name="syntax"></a>語法  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|檔案\=<full path>|指定現有 VHD 檔案的完整路徑和檔案名。|  
|noerr|僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要將焦點移至名為 Test .vhd 的 VHD，請輸入：  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [附加 vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [卸離 vdisk](detach-vdisk.md)  
  
-   [詳細資料 vdisk](detail-vdisk.md)  
  
-   [展開 vdisk](expand-vdisk.md)  
  
-   [合併 vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

