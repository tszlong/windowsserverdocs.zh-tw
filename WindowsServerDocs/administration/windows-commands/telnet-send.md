---
title: telnet 傳送
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 958e0e507e5a0ae836da98de8d677a116dbb38bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383628"
---
# <a name="telnet-send"></a>telnet：傳送

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 telnet 命令傳送到 telnet 伺服器。   
## <a name="syntax"></a>語法  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>參數  

| 參數 |                     描述                      |
|-----------|------------------------------------------------------|
|    ao     |       傳送 telnet 命令中止輸出。        |
|    ayt    |       傳送 telnet 命令給您。       |
|    brk    |            傳送 telnet 命令 brk。            |
|    Esc    |      傳送目前的 telnet escape 字元。      |
|    ip     |     傳送 telnet 命令中斷進程。     |
|   同步   |           傳送 telnet 命令同步處理。           |
| <string>  | 將您輸入的任何字串傳送到 telnet 伺服器。 |
|     ?     |     顯示與此命令相關聯的說明。      |

## <a name="BKMK_Examples"></a>典型  
傳送到 telnet 伺服器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  
