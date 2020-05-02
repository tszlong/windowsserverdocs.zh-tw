---
title: ftp mput_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3fb654d5a2f44b9b63238abdbaee8d6a0294861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725222"
---
# <a name="ftp-mput_1"></a>ftp： mput_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。   
## <a name="syntax"></a>語法  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>參數  

|  參數  |                       描述                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | 指定要複製到遠端電腦的本機檔案。 |

## <a name="examples"></a>範例  
使用目前的檔案傳輸類型，將**Program1**和**program2.c**複製到遠端電腦。  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp： binary](ftp-binary.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
