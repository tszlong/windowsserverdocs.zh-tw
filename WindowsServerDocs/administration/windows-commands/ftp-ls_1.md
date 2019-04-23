---
title: ftp ls_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c45e26f6578510837f190ae20e3140e619dc59cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841759"
---
# <a name="ftp-ls1"></a>ftp: ls_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示檔案和子目錄，從遠端電腦的簡短的清單。   
## <a name="syntax"></a>語法  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<remotedirectory>]|指定您要查看清單的目錄。 如果未不指定任何目錄，則會使用目前的工作目錄，在遠端電腦上。|  
|[<LocalFile>]|指定用來儲存清單中的本機檔案。 如果未指定本機檔案，結果會顯示在螢幕上。|  
## <a name="BKMK_Examples"></a>範例  
顯示檔案和子目錄，從遠端電腦的簡短的清單。  
```  
ls  
```  
取得縮寫的目錄清單**dir1**遠端電腦上並儲存在本機的檔案稱為**dirlist.txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
