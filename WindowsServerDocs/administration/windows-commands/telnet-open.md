---
title: telnet 開啟
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4528b728c89bbdfc99de94c7fefebb18c8e1ad97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383652"
---
# <a name="telnet-open"></a>telnet：開啟

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到 telnet 伺服器。    
## <a name="syntax"></a>語法  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>參數  

| 參數  |                                        描述                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         指定電腦名稱稱或 IP 位址。                         |
|  [<Port>]  | 指定 telnet 伺服器正在接聽的 TCP 通訊埠。 預設值為 TCP 埠23。 |

## <a name="BKMK_Examples"></a>典型  
連接到 telnet 伺服器，網址為 telnet.microsoft.com。  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  
