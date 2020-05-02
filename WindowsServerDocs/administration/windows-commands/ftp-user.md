---
title: ftp 使用者
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb4f0f47f23bec312c57266479c261c96535e8cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725070"
---
# <a name="ftp-user"></a>ftp：使用者

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定遠端電腦的使用者。   
## <a name="syntax"></a>語法  
```  
user <UserName> [<Password>] [<Account>]  
```  
#### <a name="parameters"></a>參數  

|  參數   |                                                                      描述                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          指定用來登入遠端電腦的使用者名稱。                                           |
| [<Password>] |               指定使用者*名稱*的密碼。 如果未指定密碼，但這是必要的， **ftp**會提示您輸入密碼。               |
| [<Account>]  | 指定用來登入遠端電腦的帳戶。 如果未指定*帳戶*，但為必要，則**ftp**會提示您輸入該帳戶。 |

## <a name="examples"></a>範例  
以 Password1 密碼指定 User1。  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
