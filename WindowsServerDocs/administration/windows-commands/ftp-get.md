---
title: ftp get
description: 取得 ftp 的 Windows 命令主題
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 798317f3921cd0e5ff12b69b972e2ea423fa6b3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816729"
---
# <a name="ftp-get"></a>ftp： 取得

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將遠端檔案複製到本機電腦使用目前的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<remoteFile>|指定要複製的遠端檔案。|  
|[<LocalFile>]|指定要使用本機電腦上的檔案名稱。 如果*LocalFile*未指定，在指定的檔案*remoteFile*名稱。|  
## <a name="remarks"></a>備註  
**取得**命令等同於**接收**命令。  
## <a name="BKMK_Examples"></a>範例  
複製**test.txt**到本機電腦使用目前的檔案傳輸類型。  
```  
get test.txt  
```  
複製**test.txt**本機電腦**test1.txt**使用目前的檔案傳輸類型。  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
