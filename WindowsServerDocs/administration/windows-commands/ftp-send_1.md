---
title: ftp send_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04617246c05edde127db01ce1a0fe692eb0aceb1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725099"
---
# <a name="ftp-send_1"></a>ftp： send_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。   
## <a name="syntax"></a>語法  
```  
send <LocalFile> [<remoteFile>]  
```  
#### <a name="parameters"></a>參數  

|  參數   |                    描述                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         指定要複製的本機檔案。         |
| <remoteFile> | 指定要在遠端電腦上使用的名稱。 |

## <a name="remarks"></a>備註  
- **Send**命令等同于**put**命令。  
- 如果未指定*remoteFile* ，則會為檔案提供*LocalFile*名稱。  
  ## <a name="examples"></a>範例  
  複製本機檔案**test.txt** ，並在遠端電腦上將它命名為**test1. .txt** 。  
  ```  
  send test.txt test1.txt  
  ```  
  將本機檔案**program**複製到遠端電腦。  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>其他參考  
- - [命令列語法關鍵](command-line-syntax-key.md)  
