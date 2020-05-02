---
title: ftp 接收
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ece259f2d48e18f6a789d51b1df7089490f2fa1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725122"
---
# <a name="ftp-recv"></a>ftp：接收

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
  ## <a name="examples"></a>範例  
  使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦。  
  ```  
  recv test.txt  
  ```  
  使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦做為**test1 .txt。**  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>其他參考  
- [ftp： ascii](ftp-ascii.md)  
- [ftp： binary](ftp-binary.md)  
- [ftp： get](ftp-get.md)  
- - [命令列語法關鍵](command-line-syntax-key.md)  
