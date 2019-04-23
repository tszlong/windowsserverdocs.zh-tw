---
title: telnet 取消設定
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853889"
---
# <a name="telnet-unset"></a>telnet: unset

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

關閉先前設定的選項。   
## <a name="syntax"></a>語法  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|bsasdel|傳送**退格鍵**作為**退格鍵**。|  
|crlf|傳送**Enter**金鑰為 CR。 也稱為換行字元模式。|  
|delasbs|傳送**刪除**作為**刪除**。|  
|逸出|移除逸出字元設定。|  
|localecho|關閉 localecho。|  
|logging|關閉記錄功能。|  
|Ntlm|關閉 NTLM 驗證。|  
|?|此命令會顯示說明。|  
## <a name="BKMK_Examples"></a>範例  
關閉記錄功能。  
```  
u logging  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
