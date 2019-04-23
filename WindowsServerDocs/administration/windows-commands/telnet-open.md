---
title: 開啟 telnet
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a87c4bac000a63af806705e9371a79d7370a34c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838239"
---
# <a name="telnet-open"></a>telnet： 開啟

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

會連線到 telnet 伺服器。    
## <a name="syntax"></a>語法  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<hostname>|指定的電腦名稱或 IP 位址。|  
|[<Port>]|指定 telnet 伺服器正在接聽的 TCP 連接埠。 預設為 TCP 連接埠 23。|  
## <a name="BKMK_Examples"></a>範例  
連線到 telnet 伺服器在 telnet.microsoft.com。  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
