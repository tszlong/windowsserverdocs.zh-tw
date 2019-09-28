---
title: ftp mget
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376305"
---
# <a name="ftp-mget"></a>ftp： mget

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。   
## <a name="syntax"></a>語法  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>參數  

|  參數   |                        描述                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | 指定要複製到本機電腦的遠端檔案。 |

## <a name="BKMK_Examples"></a>典型  
使用目前的檔案傳輸類型，將遠端檔案 .exe 和**b.** 複製到本機電腦 **。**  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp： binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
