---
title: lpq
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 18ff1ff3ecbc2df0a437ec8a465dec9a12123ede
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437517"
---
# <a name="lpq"></a>lpq

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

執行線上印表機服務精靈 (LPD) 的電腦上顯示的列印佇列的狀態。  

## <a name="syntax"></a>語法  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>參數  

|    參數     |                                                                        描述                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | 電腦或印表機共用裝載的狀態，您想要顯示的 LPD 列印佇列的裝置，請指定 （依名稱或 IP 位址）。 必要。 |
| -P <printerName> |                           指定 （依名稱） 的印表機列印佇列，您想要顯示的狀態。 必要。                           |
|        -l        |                                      指定您想要顯示的列印佇列狀態的相關詳細資料。                                      |
|        /?        |                                                           在命令提示字元顯示說明。                                                            |

## <a name="remarks"></a>備註  
**-S**並 **-P**參數會區分大小寫，而且必須以大寫字母輸入。  
## <a name="BKMK_examples"></a>範例  
此範例會示範如何在 10.0.0.45 LPD 主機上顯示 Laserprinter1 印表機佇列的狀態：  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
[列印命令參考資料](print-command-reference.md)  
