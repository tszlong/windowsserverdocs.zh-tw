---
title: ftp mget
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b45f84622fcd6abf7a743f606fb514def584cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843351"
---
# <a name="ftp-mget"></a>ftp： mget

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。   
## <a name="syntax"></a>語法  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>參數  

|  參數   |                        描述                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | 指定要複製到本機電腦的遠端檔案。 |

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
使用目前的檔案傳輸類型，將遠端檔案 .exe 和**b.** 複製到本機電腦 **。**  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp： ascii](ftp-ascii.md)  
-   [ftp： binary](ftp-binary.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
