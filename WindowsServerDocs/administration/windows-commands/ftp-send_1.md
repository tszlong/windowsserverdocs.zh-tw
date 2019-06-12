---
title: ftp [send_1]
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 39423aff3c64f41c4fc0f8998484e6dcc38f822e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438447"
---
# <a name="ftp-send1"></a>ftp: send_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將本機檔案複製到遠端電腦使用目前的檔案傳輸類型。   
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
- **傳送**命令等同於**放**命令。  
- 如果*remoteFile*未指定，在指定的檔案*LocalFile*名稱。  
  ## <a name="BKMK_Examples"></a>範例  
  將本機檔案複製**test.txt**並將它命名**test1.txt**遠端電腦上。  
  ```  
  send test.txt test1.txt  
  ```  
  將本機檔案複製**program.exe**到遠端電腦。  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
