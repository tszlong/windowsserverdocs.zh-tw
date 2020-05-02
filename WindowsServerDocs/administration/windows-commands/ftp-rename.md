---
title: ftp 重新命名
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc5006c82df8417a8652a9c0ba20f7f1a002e7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725100"
---
# <a name="ftp-rename"></a>ftp： rename

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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

## <a name="examples"></a>範例  
將遠端檔案**範例 .txt**重新命名為**example1 .txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
