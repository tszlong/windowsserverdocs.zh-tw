---
title: pubprn
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372006"
---
# <a name="pubprn"></a>pubprn

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將印表機發佈到 active directory 網域服務。

## <a name="syntax"></a>語法
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<ServerName >|指定裝載您要發佈之印表機的 Windows server 名稱。 如果您未指定電腦，則會使用本機電腦。|
|\<UNCprinterpath >|您想要發佈之共用印表機的通用命名慣例（UNC）路徑。|
|"LDAP://CN = <Container>，DC = <Container>"|指定您要發佈印表機的 active directory 網域服務中容器的路徑。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Pubprn**命令是位於%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2 目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著 pubprn 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   如果您提供的資訊包含空格，請使用引號括住文字（例如，`"computer Name"`）。

## <a name="BKMK_examples"></a>典型
若要將 \\ \ Server1 電腦上的所有印表機發佈到 MyDomain.company.Com 網域中的 MyContainer 容器，請輸入：
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
若要將 \\ \ Server1 伺服器上的 Laserprinter1 印表機發佈到 MyDomain.company.Com 網域中的 MyContainer 容器，請輸入：
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[列印命令參考](print-command-reference.md)
