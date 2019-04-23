---
title: ftp mdir
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ac1e7cd50fe4d9325c272f74a7b81971c8bb12a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878209"
---
# <a name="ftp-mdir"></a>ftp: mdir

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

遠端目錄中顯示檔案和子目錄的目錄清單。   
## <a name="syntax"></a>語法  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<remoteFile>|指定目錄或檔案的項目，您要查看清單。|  
|<LocalFile>|指定本機檔案以儲存清單。 此為必要參數。|  
## <a name="remarks"></a>備註  
-   您可以使用**mdir**來指定多個檔案。  
-   指定*remoteFile*  
    輸入連字號 (**-**) 若要在遠端電腦上使用目前的工作目錄。  
-   指定*本機檔案*  
    輸入連字號 (**-**) 若要在螢幕上顯示的清單。  
## <a name="BKMK_Examples"></a>範例  
顯示的目錄清單**dir1**並**dir2**畫面上  
```  
mdir dir1 dir2 -  
```  
儲存的合併的目錄清單**dir1**並**dir2**在本機的檔案稱為**dirlist.txt**  
```  
mdir dir1 dir2 dirlist.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
