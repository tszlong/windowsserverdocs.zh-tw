---
title: ftp get
description: Ftp get 的 Windows 命令主題
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cc74b56fa849a25ed2f4e4a37d339b1da87c24f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376386"
---
# <a name="ftp-get"></a>ftp： get

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。   
## <a name="syntax"></a>語法  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>參數  

|   參數   |                                                              描述                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   指定要複製的遠端檔案。                                                   |
| [<LocalFile>] | 指定要在本機電腦上使用的檔案名。 如果未指定*LocalFile* ，則會為檔案提供*remoteFile*名稱。 |

## <a name="remarks"></a>備註  
**Get**命令與 [**接收**] 命令相同。  
## <a name="BKMK_Examples"></a>典型  
使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦。  
```  
get test.txt  
```  
使用目前的檔案傳輸類型，將**test.txt**複製到本機電腦做為**test1 .txt。**  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp： binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
