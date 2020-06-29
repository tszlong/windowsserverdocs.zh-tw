---
title: pubprn
description: Pubprn 命令的參考主題，它會將印表機發佈到 Active Directory Domain Services。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d45291b22978dd3fe2781699eaf616b9d08a4bf
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472143"
---
# <a name="pubprn"></a>pubprn

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將印表機發佈到 Active Directory Domain Services。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 pubprn 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如： `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn` 。

## <a name="syntax"></a>語法

```
cscript pubprn {<servername> | <UNCprinterpath>} LDAP://CN=<container>,DC=<container>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<servername>` | 指定裝載您要發佈之印表機的 Windows server 名稱。 如果您未指定電腦，則會使用本機電腦。 |
| `<UNCprinterpath>` | 您想要發佈之共用印表機的通用命名慣例（UNC）路徑。 |
| `LDAP://CN=<Container>,DC=<Container>` | 指定您要在其中發行印表機 Active Directory Domain Services 中容器的路徑。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字（例如「電腦名稱稱」）。

### <a name="examples"></a>範例

若要將 Server1 電腦上的所有印表機發佈 \\ 到 MyDomain.company.com 網域中的 MyContainer 容器，請輸入：

```
cscript pubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

若要將 \Server1 伺服器上的 Laserprinter1 印表機發佈 \\ 到 MyDomain.company.com 網域中的 MyContainer 容器，請輸入：

```
cscript pubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
