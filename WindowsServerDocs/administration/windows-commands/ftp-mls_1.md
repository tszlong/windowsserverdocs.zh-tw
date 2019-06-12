---
title: ftp mls_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a379ead9c56af096e121048a8c0f596f6879bb0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438546"
---
# <a name="ftp-mls1"></a>ftp: mls_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

遠端目錄中顯示檔案和子目錄的簡短的清單。   
## <a name="syntax"></a>語法  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>參數  

|  參數   |                       描述                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | 指定您要查看清單的檔案。 |
| <LocalFile>  |  指定用來儲存清單中的本機檔案。  |

## <a name="remarks"></a>備註  
- 指定*remoteFiles*  
  輸入連字號 ( **-** ) 若要在遠端電腦上使用目前的工作目錄。  
- 指定*本機檔案*  
  輸入連字號 ( **-** ) 若要在螢幕上顯示的清單。  
  ## <a name="BKMK_Examples"></a>範例  
  顯示檔案和子目錄的簡短的清單**dir1**並**dir2**。  
  ```  
  mls dir1 dir2 -  
  ```  
  儲存檔案和子目錄的簡短的清單**dir1**並**dir2**本機檔案中**dirlist.txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
