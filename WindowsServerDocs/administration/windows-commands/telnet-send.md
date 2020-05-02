---
title: telnet 傳送
description: Telnet send 的參考主題，它會將 telnet 命令傳送到 telnet 伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432401bbe2050a7954967a73b5ba8abeee5bb1d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721492"
---
# <a name="telnet-send"></a>telnet：傳送

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 telnet 命令傳送到 telnet 伺服器。   

## <a name="syntax"></a>語法  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
#### <a name="parameters"></a>參數  

| 參數 |                     描述                      |
|-----------|------------------------------------------------------|
|    ao     |       傳送 telnet 命令中止輸出。        |
|    ayt    |       傳送 telnet 命令給您。       |
|    brk    |            傳送 telnet 命令 brk。            |
|    esc    |      傳送目前的 telnet escape 字元。      |
|    ip     |     傳送 telnet 命令中斷進程。     |
|   synch   |           傳送 telnet 命令同步處理。           |
| <string>  | 將您輸入的任何字串傳送到 telnet 伺服器。 |
|     ?     |     顯示與此命令相關聯的說明。      |

## <a name="examples"></a>範例  
傳送到 telnet 伺服器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
