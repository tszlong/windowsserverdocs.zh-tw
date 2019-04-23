---
title: ftp 附加
description: 'Ftp 的 Windows 命令主題附加 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23fa04b86d9c26fb30b74eebe8caef8498b90a12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879029"
---
# <a name="ftp-append"></a>ftp： 附加

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將本機檔案附加至使用目前的檔案類型設定的遠端電腦上的檔案。   
## <a name="syntax"></a>語法  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<LocalFile>|指定要新增的本機檔案。|  
|[remoteFile]|指定要在遠端電腦上的檔案<LocalFile>加入。|  
## <a name="remarks"></a>備註  
如果*remoteFile*省略，則*LocalFile*名稱來取代遠端檔案名稱。  
## <a name="BKMK_Examples"></a>範例  
附加 file1.txt file2.txt 遠端電腦上。  
```  
append file1.txt file2.txt  
```  
將本機 file1.txt 附加至名為 file1.txt 遠端電腦上的檔案中。  
```  
append file1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
