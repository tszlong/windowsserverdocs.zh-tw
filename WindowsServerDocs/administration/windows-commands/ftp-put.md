---
title: ftp put
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3307ba71e7b3c8b4113f9ed29ab06660dafa5f6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868739"
---
# <a name="ftp-put"></a>ftp: put

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將本機檔案複製到遠端電腦使用目前的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<LocalFile>|指定要複製的本機檔案。|  
|[<remoteFile>]|指定要在遠端電腦上使用的名稱。|  
## <a name="remarks"></a>備註  
-   **放**命令等同於**傳送**命令。  
-   如果*remoteFile*未指定，在指定的檔案*LocalFile*名稱。  
## <a name="BKMK_Examples"></a>範例  
將本機檔案複製**test.txt**並將它命名**test1.txt**遠端電腦上。  
```  
put test.txt test1.txt  
```  
將本機檔案複製**program.exe**到遠端電腦。  
```  
put program.exe  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
