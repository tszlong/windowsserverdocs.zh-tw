---
title: ftp 目錄
description: Ftp 目錄的參考主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f2b9c610abe50bf662439a84d9bcbcb17b1a5bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725309"
---
# <a name="ftp-dir"></a>ftp： dir

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端電腦上的目錄檔案和子目錄清單。   
## <a name="syntax"></a>語法  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<remotedirectory>]|指定您要查看其清單的目錄。 如果未指定任何目錄，則會使用遠端電腦上的目前工作目錄。|  
|[<LocalFile>]|指定用來儲存目錄清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。|  
## <a name="examples"></a>範例  
顯示遠端電腦上**dir1**的目錄清單。  
```  
dir dir1  
```  
將遠端電腦上目前目錄的清單儲存在本機檔案**dirlist**中。  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
