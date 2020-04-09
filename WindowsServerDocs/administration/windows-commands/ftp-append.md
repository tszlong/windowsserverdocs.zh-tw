---
title: ftp 附加
description: Ftp 附加的 Windows 命令主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44ce1d6e7259dc8745da35ed462e6378f0fce8ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843821"
---
# <a name="ftp-append"></a>ftp： append

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。   
## <a name="syntax"></a>語法  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>參數  

|  參數   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     指定要加入的本機檔案。                     |
| [remoteFile] | 指定要在其中新增 <LocalFile> 的遠端電腦上的檔案。 |

## <a name="remarks"></a>備註  
如果省略*remoteFile* ，則會使用*LocalFile*名稱來取代遠端檔案名。  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
將 file1 附加至遠端電腦上的 file2。  
```  
append file1.txt file2.txt  
```  
將本機 file1 附加至遠端電腦上名為 file1 .txt 的檔案。  
```  
append file1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
