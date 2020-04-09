---
title: ftp 使用者
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29081bd8c5d537e1f060e4c848b720a60b4c8aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842851"
---
# <a name="ftp-user"></a>ftp：使用者

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
以 Password1 密碼指定 User1。  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
