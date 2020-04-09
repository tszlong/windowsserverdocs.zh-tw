---
title: ftp mdir
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb024d063761f1e817f02fdd7301376dd167846
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842711"
---
# <a name="ftp-mdir"></a>ftp： mdir

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端目錄中檔案和子目錄的目錄清單。   
## <a name="syntax"></a>語法  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>參數  

|  參數   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   指定您要查看其清單的目錄或檔案。   |
| <LocalFile>  | 指定用來儲存清單的本機檔案。 此為必要參數。 |

## <a name="remarks"></a>備註  
- 您可以使用**mdir**來指定多個檔案。  
- 指定*remoteFile*  
  輸入連字號（ **-** ），以在遠端電腦上使用目前的工作目錄。  
- 指定*LocalFile*  
  輸入連字號（ **-** ）以在螢幕上顯示清單。  
  ## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  在畫面上顯示**dir1**和**dir2**的目錄清單  
  ```  
  mdir dir1 dir2 -  
  ```  
  將**dir1**和**dir2**的合併目錄清單儲存在名為 dirlist 的本機檔案中 **。**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- - [命令列語法關鍵](command-line-syntax-key.md)  
