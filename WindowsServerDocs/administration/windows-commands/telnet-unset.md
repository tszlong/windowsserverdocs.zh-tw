---
title: telnet 未設定
description: 取消設定 telnet 的 Windows 命令主題，這會關閉先前設定的選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34d52eedb2a5547ad0e3f2912dbe2a250eaf8fc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833141"
---
# <a name="telnet-unset"></a>telnet：未設定

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

關閉先前設定的選項。   

## <a name="syntax"></a>語法  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|bsasdel|將**倒退鍵**傳送為**退**格鍵。|  
|crlf|以 CR 的形式傳送**Enter**鍵。 也稱為換行模式。|  
|delasbs|傳送**刪除**做為**刪除**。|  
|逸出|移除 escape 字元設定。|  
|localecho|關閉 localecho。|  
|記錄|關閉記錄功能。|  
|ntlm|關閉 NTLM 驗證。|  
|?|顯示此命令的說明。|  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
關閉記錄功能。  
```  
u logging  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
