---
title: ftp mput_1
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376211"
---
# <a name="ftp-mput_1"></a>ftp： mput_1

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。   
## <a name="syntax"></a>語法  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>參數  

|  參數  |                       描述                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | 指定要複製到遠端電腦的本機檔案。 |

## <a name="BKMK_Examples"></a>典型  
使用目前的檔案傳輸類型，將**Program1**和**program2.c**複製到遠端電腦。  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp： binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
