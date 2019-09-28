---
title: expand
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a88222ffe9ff374626a6406c330a6660d992f96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377317"
---
# <a name="expand"></a>expand

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

展開一或多個壓縮檔案。 您可以使用此命令從散發磁片取出壓縮檔案。  
## <a name="syntax"></a>語法  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>參數  

|  參數  |                                                                                                                                                                   描述                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             重新命名展開的檔案。                                                                                                                                                              |
|   來源    |                                                                              指定要展開的檔案。 *來源*可以包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。 您可以使用萬用字元（ **\\** \* 或 **？** ）。                                                                               |
| destination | 指定要展開檔案的位置。<br /><br />如果*來源*包含多個檔案，而您未指定 **/r**，則*destination*必須是目錄。<br /><br />*目的地*可以包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。<br /><br />目的地檔案&#124;路徑規格。 |
|     /i      |                                                                                                   重新命名已展開的檔案，但忽略目錄結構。<br /><br />此參數適用于：Windows Server 2008 R2 和 Windows 7。                                                                                                    |
|     /d      |                                                                                                                              顯示來源位置中的檔案清單。 不會展開或解壓縮檔案。                                                                                                                              |
|     /f:     |                                                                                                                 指定您想要展開之封包檔（.cab）中的檔案。 您可以使用萬用字元（ **\\** \* 或 **？** ）。                                                                                                                 |
|     /?      |                                                                                                                                                       在命令提示字元顯示說明。                                                                                                                                                       |

## <a name="remarks"></a>備註  
- 在修復主控台使用**expand**  
  您可以從 [修復主控台] 取得具有不同參數的 [**展開**] 命令。 如需有關修復主控台的詳細資訊，請參閱 Microsoft 知識庫中的[文章 314058](https://support.microsoft.com/kb/314058) 。  
  ## <a name="additional-references"></a>其他參考  
  [命令列語法關鍵](command-line-syntax-key.md)  
