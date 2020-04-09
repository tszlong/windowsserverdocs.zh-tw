---
title: ftp 重新命名
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe159f2833ce52921b46e46881a1d7aed8c5df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843021"
---
# <a name="ftp-rename"></a>ftp： rename

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重新命名遠端檔案。   
## <a name="syntax"></a>語法  
```  
rename <FileName> <NewFileName>  
```  
#### <a name="parameters"></a>參數  

|   參數   |                 描述                 |
|---------------|---------------------------------------------|
|  <FileName>   | 指定您要重新命名的檔案。 |
| <NewFileName> |        指定新的檔案名。         |

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
將遠端檔案**範例 .txt**重新命名為**example1 .txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
