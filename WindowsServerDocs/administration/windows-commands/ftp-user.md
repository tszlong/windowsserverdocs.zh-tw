---
title: ftp 使用者
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ef3b943491a90078dab453aaf3a037bd4ccf1825
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887489"
---
# <a name="ftp-user"></a>ftp： 使用者

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

指定遠端電腦的使用者。   
## <a name="syntax"></a>語法  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<UserName>|指定用來登入遠端電腦的使用者名稱。|  
|[<Password>]|指定的密碼*UserName*。 如果未指定密碼，但為必要項， **ftp**會提示輸入密碼。|  
|[<Account>]|指定用來登入遠端電腦帳戶。 如果*帳號*未指定，但需要**ftp**會提示您輸入的帳戶。|  
## <a name="BKMK_Examples"></a>範例  
指定密碼 Password1 User1。  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
