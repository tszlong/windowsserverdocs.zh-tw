---
title: telnet 傳送
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36dc7f861e88cf991af57dda2f150107c6870f0f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441047"
---
# <a name="telnet-send"></a>telnet： 傳送

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將 telnet 命令傳送到 telnet 伺服器。   
## <a name="syntax"></a>語法  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>參數  

| 參數 |                     描述                      |
|-----------|------------------------------------------------------|
|    ao     |       傳送 telnet 命令中止的輸出。        |
|    ayt    |       傳送那里為您的 telnet 命令。       |
|    brk    |            傳送 telnet 命令 brk。            |
|    esc    |      傳送目前的 telnet 逸出字元。      |
|    ip     |     傳送 telnet 命令中斷處理序。     |
|   同步處理   |           傳送 telnet 命令同步處理。           |
| <string>  | 將您輸入任何字串傳送到 telnet 伺服器。 |
|     ?     |     顯示說明此命令相關聯。      |

## <a name="BKMK_Examples"></a>範例  
傳送可讓您有會到 telnet 伺服器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
