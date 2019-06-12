---
title: ftp 重新命名
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 80d1a15f038017444c7654a44748bfd22be8e487
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438385"
---
# <a name="ftp-rename"></a>ftp： 重新命名

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

重新命名遠端檔案。   
## <a name="syntax"></a>語法  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>參數  

|   參數   |                 描述                 |
|---------------|---------------------------------------------|
|  <FileName>   | 指定您想要重新命名的檔案。 |
| <NewFileName> |        指定新的檔案名稱。         |

## <a name="BKMK_Examples"></a>範例  
遠端檔案重新命名**example.txt**到**example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
