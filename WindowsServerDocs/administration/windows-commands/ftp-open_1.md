---
title: ftp open_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a27d2f7512da352a0f4ddf02fa2511ffce7c1d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725189"
---
# <a name="ftp-open_1"></a>ftp： open_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到指定的 ftp 伺服器。   
## <a name="syntax"></a>語法  
```  
open <computer> [<Port>]  
```  
#### <a name="parameters"></a>參數  

| 參數  |                                           描述                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                指定您嘗試連接的遠端電腦。                 |
|  [<Port>]  | 指定要用來連接到 ftp 伺服器的 TCP 通訊埠編號。 預設會使用 TCP 埠21。 |

## <a name="remarks"></a>備註  
您可以使用 IP 位址或電腦名稱稱（在此情況下，必須要有 DNS 伺服器或 Hosts 檔案）來指定**電腦**。  
## <a name="examples"></a>範例  
連接到 ftp 伺服器，網址為**ftp.microsoft.com**。  
```  
Open ftp.microsoft.com  
```  
在接聽 TCP 通訊埠755的**ftp.microsoft.com**上，連接到 ftp 伺服器。  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
