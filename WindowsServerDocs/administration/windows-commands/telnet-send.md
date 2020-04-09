---
title: telnet 傳送
description: Telnet send 的 Windows 命令主題，它會將 telnet 命令傳送到 telnet 伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fef48ca04a3817f58d063bc8b23f5c11c4ea197
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833281"
---
# <a name="telnet-send"></a>telnet：傳送

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
傳送到 telnet 伺服器。  
```  
sen ayt  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
