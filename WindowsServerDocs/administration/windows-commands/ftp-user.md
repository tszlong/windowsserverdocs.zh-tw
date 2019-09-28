---
title: ftp 使用者
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63281a0ffdd646d3652eb3a442a8edd9acec9cce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375875"
---
# <a name="ftp-user"></a>ftp：使用者

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定遠端電腦的使用者。   
## <a name="syntax"></a>語法  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>參數  

|  參數   |                                                                      描述                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          指定用來登入遠端電腦的使用者名稱。                                           |
| [<Password>] |               指定使用者*名稱*的密碼。 如果未指定密碼，但這是必要的， **ftp**會提示您輸入密碼。               |
| [<Account>]  | 指定用來登入遠端電腦的帳戶。 如果未指定*帳戶*，但為必要，則**ftp**會提示您輸入該帳戶。 |

## <a name="BKMK_Examples"></a>典型  
以 Password1 密碼指定 User1。  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  
