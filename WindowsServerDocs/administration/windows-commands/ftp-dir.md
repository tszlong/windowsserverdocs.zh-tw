---
title: ftp 目錄
description: Ftp 目錄的 Windows 命令主題
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835249"
---
# <a name="ftp-dir"></a>ftp: dir

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示遠端電腦上的目錄檔案和子目錄的清單。   
## <a name="syntax"></a>語法  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<remotedirectory>]|指定您要查看清單的目錄。 如果未不指定任何目錄，則會使用目前的工作目錄，在遠端電腦上。|  
|[<LocalFile>]|指定用來儲存目錄清單中的本機檔案。 如果未指定本機檔案，結果會顯示在螢幕上。|  
## <a name="BKMK_Examples"></a>範例  
顯示的目錄清單**dir1**遠端電腦上。  
```  
dir dir1  
```  
將目前目錄的清單儲存在本機檔案中的遠端電腦上**dirlist.txt**。  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
