---
title: lpq
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3755c010c9bb4549deed08f26b5a0670fe7318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374200"
---
# <a name="lpq"></a>lpq

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示執行線上印表機 Daemon （LPD）之電腦上的列印佇列狀態。  

## <a name="syntax"></a>語法  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>參數  

|    參數     |                                                                        描述                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | 指定以您要顯示的狀態裝載 LPD 列印佇列的電腦或印表機共用裝置（依名稱或 IP 位址）。 必要。 |
| -P <printerName> |                           以您想要顯示之狀態的列印佇列指定（依名稱）印表機。 必要。                           |
|        -l        |                                      指定您想要顯示有關列印佇列狀態的詳細資料。                                      |
|        /?        |                                                           在命令提示字元顯示說明。                                                            |

## <a name="remarks"></a>備註  
**-S**和 **-P**參數會區分大小寫，且必須以大寫字母形式輸入。  
## <a name="BKMK_examples"></a>典型  
這個範例示範如何在10.0.0.45 的 LPD 主機上顯示 Laserprinter1 印表機佇列的狀態：  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
[列印命令參考](print-command-reference.md)  
