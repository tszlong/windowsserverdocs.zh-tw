---
title: ftp ls_1
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ff63272fe3c5e59965b04bef258a06fee2da0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843361"
---
# <a name="ftp-ls_1"></a>ftp： ls_1

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
> 
> 
> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從遠端電腦顯示檔案和子目錄的縮寫清單。   
## <a name="syntax"></a>語法  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>參數  

|      參數      |                                                                       描述                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | 指定您要查看其清單的目錄。 如果未指定任何目錄，則會使用遠端電腦上的目前工作目錄。 |
|    [<LocalFile>]    |               指定用來儲存清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。               |

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
從遠端電腦顯示檔案和子目錄的縮寫清單。  
```  
ls  
```  
取得遠端電腦上**dir1**的縮寫目錄清單，並將它儲存在名為 dirlist 的本機檔案中 **。**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
