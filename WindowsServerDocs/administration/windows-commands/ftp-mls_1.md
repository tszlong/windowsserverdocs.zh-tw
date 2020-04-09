---
title: ftp mls_1
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca3b8e04dd4a152b2d1bf8ce1ca8006d70186116
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843291"
---
# <a name="ftp-mls_1"></a>ftp： mls_1

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
  輸入連字號（ **-** ），以在遠端電腦上使用目前的工作目錄。  
- 指定*LocalFile*  
  輸入連字號（ **-** ）以在螢幕上顯示清單。  
  ## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  顯示**dir1**和**dir2**的檔案和子目錄的縮寫清單。  
  ```  
  mls dir1 dir2 -  
  ```  
  在本機檔案**dirlist**中，儲存**dir1**和**dir2**的檔案和子目錄的縮寫清單  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- - [命令列語法關鍵](command-line-syntax-key.md)  
