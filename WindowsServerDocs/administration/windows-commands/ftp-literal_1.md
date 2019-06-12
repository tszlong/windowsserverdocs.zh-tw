---
title: ftp literal_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 693916507488a9a480315a8e9299baa93a223b8a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438665"
---
# <a name="ftp-literal1"></a>ftp: literal_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 會傳送到遠端 ftp 伺服器的逐字引數。 會傳回單一的 ftp 回覆程式碼。   

## <a name="syntax"></a>語法  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>參數  

| 參數  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定的引數傳送至 ftp 伺服器。 |

## <a name="remarks"></a>備註  
**常值**命令等同於**報價**命令。  
## <a name="BKMK_Examples"></a>範例  
傳送**結束**命令至遠端 ftp 伺服器。  
```  
literal quit  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: quote](ftp-quote.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
