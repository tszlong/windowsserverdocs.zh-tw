---
title: expand
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5df078af23c77f54ccb2da83b1057c5d7042593a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825959"
---
# <a name="expand"></a>expand

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

展開一或多個壓縮的檔案。 您可以使用此命令，從發佈磁碟擷取壓縮的檔案。  
## <a name="syntax"></a>語法  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/r|重新命名展開的檔案。|  
|來源|指定要展開的檔案。 *來源*可包含磁碟機代號與冒號、 目錄名稱、 檔案名稱或這些項目的組合。 您可以使用萬用字元 (**\*** 或是 **？**)。|  
|目的地|指定要展開的檔案的位置。<br /><br />如果*來源*包含多個檔案，且您未指定 **/r**，*目的地*必須是目錄。<br /><br />*目的地*可包含磁碟機代號與冒號、 目錄名稱、 檔案名稱或這些項目的組合。<br /><br />目的地檔案&#124;的路徑規格。|  
|/i|重新命名展開的檔案但略過的目錄結構。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|  
|/d|來源位置中，會顯示一份檔案。 不會展開或解壓縮檔案。|  
|/f:|在您想要擴充的封包 (.cab) 檔案中指定的檔案。 您可以使用萬用字元 (**\*** 或是 **？**)。|  
|/?|在命令提示字元顯示說明。|  
## <a name="remarks"></a>備註  
-   使用**展開**在修復主控台  
    **展開**命令，使用不同的參數，可以使用修復主控台。 如需有關復原主控台的詳細資訊，請參閱[文章 314058](https://support.microsoft.com/kb/314058) Microsoft 知識庫中。  
## <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
