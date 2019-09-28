---
title: ftp send_1
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9f658b4fbfa5f6c9fa9a58fb0c524ad53627bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375996"
---
# <a name="ftp-send_1"></a>ftp： send_1

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。   
## <a name="syntax"></a>語法  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>參數  

|  參數   |                    描述                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         指定要複製的本機檔案。         |
| <remoteFile> | 指定要在遠端電腦上使用的名稱。 |

## <a name="remarks"></a>備註  
- **Send**命令等同于**put**命令。  
- 如果未指定*remoteFile* ，則會為檔案提供*LocalFile*名稱。  
  ## <a name="BKMK_Examples"></a>典型  
  複製本機檔案**test.txt** ，並在遠端電腦上將它命名為**test1. .txt** 。  
  ```  
  send test.txt test1.txt  
  ```  
  將本機檔案**program**複製到遠端電腦。  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
