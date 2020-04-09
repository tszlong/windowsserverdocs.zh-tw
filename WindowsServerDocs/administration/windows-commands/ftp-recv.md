---
title: ftp 接收
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fec409741e00bb3e6f61808630e5141ce4ec78f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842961"
---
# <a name="ftp-recv"></a>ftp：接收

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。   
## <a name="syntax"></a>語法  
```  
recv <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>參數  

|   參數   |                   描述                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        指定要複製的遠端檔案。        |
| [<LocalFile>] | 指定要在本機電腦上使用的名稱。 |

## <a name="remarks"></a>備註  
- [**接收**] 命令等同于**get**命令。  
- 如果未指定*LocalFile* ，則會為檔案提供*remoteFile*名稱。  
  ## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦。  
  ```  
  recv test.txt  
  ```  
  使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦做為**test1 .txt。**  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- [ftp： ascii](ftp-ascii.md)  
- [ftp： binary](ftp-binary.md)  
- [ftp： get](ftp-get.md)  
- - [命令列語法關鍵](command-line-syntax-key.md)  
