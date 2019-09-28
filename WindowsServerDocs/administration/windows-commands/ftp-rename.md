---
title: ftp 重新命名
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376020"
---
# <a name="ftp-rename"></a>ftp： rename

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重新命名遠端檔案。   
## <a name="syntax"></a>語法  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>參數  

|   參數   |                 描述                 |
|---------------|---------------------------------------------|
|  <FileName>   | 指定您要重新命名的檔案。 |
| <NewFileName> |        指定新的檔案名。         |

## <a name="BKMK_Examples"></a>典型  
將遠端檔案**範例 .txt**重新命名為**example1 .txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  
