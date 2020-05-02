---
title: ftp mls_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27f74d4c1d03cb4d9f665566f69485e80f8eccdc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725200"
---
# <a name="ftp-mls_1"></a>ftp： mls_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端目錄中的檔案和子目錄的縮寫清單。   
## <a name="syntax"></a>語法  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>參數  

|  參數   |                       描述                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | 指定您要查看其清單的檔案。 |
| <LocalFile>  |  指定用來儲存清單的本機檔案。  |

## <a name="remarks"></a>備註  
- 指定*remoteFiles*  
  輸入連字號（**-**），以使用遠端電腦上目前的工作目錄。  
- 指定*LocalFile*  
  輸入連字號（**-**）以在螢幕上顯示清單。  
  ## <a name="examples"></a>範例  
  顯示**dir1**和**dir2**的檔案和子目錄的縮寫清單。  
  ```  
  mls dir1 dir2 -  
  ```  
  在本機檔案**dirlist**中，儲存**dir1**和**dir2**的檔案和子目錄的縮寫清單  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>其他參考  
- - [命令列語法關鍵](command-line-syntax-key.md)  
