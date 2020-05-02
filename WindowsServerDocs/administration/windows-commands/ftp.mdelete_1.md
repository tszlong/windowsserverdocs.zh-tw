---
title: ftp mdelete_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f578a50207439f9bfb21c2607f0aa60a20fad292
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725032"
---
# <a name="ftp-mdelete_1"></a>ftp： mdelete_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除遠端電腦上的檔案。   
## <a name="syntax"></a>語法  
```  
mdelete <remoteFile>[ ]  
```  
#### <a name="parameters"></a>參數  

|  參數   |             描述              |
|--------------|--------------------------------------|
| <remoteFile> | 指定要刪除的遠端檔案。 |

## <a name="examples"></a>範例  
刪除遠端檔案**a .exe**和**b.**.exe。  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
