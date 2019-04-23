---
title: ftp 接收
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4979010c13d78c89c9a3e4965b567f7eef1f2ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841099"
---
# <a name="ftp-recv"></a>ftp: recv

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將遠端檔案複製到本機電腦使用目前的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<remoteFile>|指定要複製的遠端檔案。|  
|[<LocalFile>]|指定要在本機電腦上使用的名稱。|  
## <a name="remarks"></a>備註  
-   **接收**命令等同於**取得**命令。  
-   如果*LocalFile*未指定，在指定的檔案*remoteFile*名稱。  
## <a name="BKMK_Examples"></a>範例  
複製**test.txt**到本機電腦使用目前的檔案傳輸類型。  
```  
recv test.txt  
```  
複製**test.txt**本機電腦**test1.txt**使用目前的檔案傳輸類型。  
```  
recv test.txt test1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [ftp: get](ftp-get.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
