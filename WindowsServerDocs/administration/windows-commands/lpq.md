---
title: lpq
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 051b1983fcc0fddd7b69e561c0a27a120f78d998
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840391"
---
# <a name="lpq"></a>lpq

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示執行線上印表機 Daemon （LPD）之電腦上的列印佇列狀態。  

## <a name="syntax"></a>語法  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>參數  

|    參數     |                                                                        描述                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | 指定以您要顯示的狀態裝載 LPD 列印佇列的電腦或印表機共用裝置（依名稱或 IP 位址）。 必要。 |
| -P <printerName> |                           以您想要顯示之狀態的列印佇列指定（依名稱）印表機。 必要。                           |
|        -l        |                                      指定您想要顯示有關列印佇列狀態的詳細資料。                                      |
|        /?        |                                                           在命令提示字元顯示說明。                                                            |

## <a name="remarks"></a>備註  
**-S**和 **-P**參數會區分大小寫，且必須以大寫字母形式輸入。  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
這個範例示範如何在10.0.0.45 的 LPD 主機上顯示 Laserprinter1 印表機佇列的狀態：  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
[列印命令參考](print-command-reference.md)  
