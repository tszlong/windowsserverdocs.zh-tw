---
title: ftp 附加
description: Ftp 附加的參考主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b53228473b8ea16a0955c244d60fae77cf4f7d7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725407"
---
# <a name="ftp-append"></a>ftp： append

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。   
## <a name="syntax"></a>語法  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>參數  

|  參數   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     指定要加入的本機檔案。                     |
| [remoteFile] | 指定在新增的遠端電腦<LocalFile>上的檔案。 |

## <a name="remarks"></a>備註  
如果省略*remoteFile* ，則會使用*LocalFile*名稱來取代遠端檔案名。  
## <a name="examples"></a>範例  
將 file1 附加至遠端電腦上的 file2。  
```  
append file1.txt file2.txt  
```  
將本機 file1 附加至遠端電腦上名為 file1 .txt 的檔案。  
```  
append file1.txt  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
