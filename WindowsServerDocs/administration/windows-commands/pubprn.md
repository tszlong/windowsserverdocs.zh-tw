---
title: pubprn
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831699"
---
# <a name="pubprn"></a>pubprn

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將印表機發佈到 active directory 網域服務中。

## <a name="syntax"></a>語法
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<ServerName>|指定裝載的印表機，您要發行的 Windows 伺服器的名稱。 如果您沒有指定的電腦，則會使用本機電腦。|
|\<UNCprinterpath>|您想要發行的共用印表機通用命名慣例 (UNC) 路徑。|
|"LDAP://CN=<Container>,DC=<Container>"|指定您想要用來公佈印表機的 active directory 網域服務中的容器路徑。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Pubprn**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 pubprn 檔案或將目錄變更為適當的資料夾完整路徑。 例如: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。

## <a name="BKMK_examples"></a>範例
要在發行的所有印表機\\\Server1 電腦 MyContainer 容器在 MyDomain.company.Com 網域中，輸入：
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
要在發行 Laserprinter1 印表機\\\Server1 server MyContainer 容器在 MyDomain.company.Com 網域中，類型：
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
