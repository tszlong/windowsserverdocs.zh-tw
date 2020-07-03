---
title: netsh
description: Netsh 命令的參考文章，這是一種命令列腳本公用程式，可讓您在本機或遠端，顯示或修改目前執行中電腦的網路設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 96fc069d-53c0-4d0a-9f7f-f9f3d49a02bd carmonmills
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d39b891b9a8b6e313121756aa8266c79b5bfaca
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934778"
---
# <a name="netsh"></a>netsh

> 適用於：Windows Server (半年度管道)、Windows Server 2016

網路 Shell 命令列腳本公用程式，可讓您在本機或遠端，顯示或修改目前執行中電腦的網路設定。 您可以在命令提示字元或 Windows PowerShell 中啟動此公用程式。

## <a name="syntax"></a>語法

```
netsh [-a <Aliasfile>][-c <Context>][-r <Remotecomputer>][-u [<domainname>\<username>][-p <Password> | [{<NetshCommand> | -f <scriptfile>}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -a`<Aliasfile>` | 指定在執行 Aliasfile 之後，傳回至 netsh 提示字元，以及包含一或多個 netsh 命令的文字檔名稱。 |
| -c`<Context>` | 指定 netsh 輸入指定的 netsh 內容和要輸入的 netsh 內容。 |
| -r`<Remotecomputer>` | 指定要設定的遠端電腦。<p>**重要事項：** 如果您使用此參數，則必須確定遠端電腦上正在執行遠端登入服務。 如果未執行，Windows 會顯示「找不到網路路徑」錯誤訊息。 |
| -u`<domainname>\<username>` | 指定在使用者帳戶下執行 netsh 命令時，所要使用的網域和使用者帳戶名稱。 如果您省略網域，預設會使用本機網域。 |
| -p`<Password>` | 指定參數所指定之使用者帳戶的密碼 `-u <username>` 。 |
| `<NetshCommand>` | 指定要執行的 netsh 命令。 |
| -f`<scriptfile>` | 執行指定的腳本檔案之後，結束 netsh 命令。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您指定 **-r**後接另一個命令，netsh 會在遠端電腦上執行命令，然後回到 Cmd.exe 命令提示字元。 如果您指定 **-r**而不需要另一個命令，netsh 會在遠端模式中開啟。 此程式類似於在 Netsh 命令提示字元使用 **set machine**。 當您使用 **-r**時，您只會為目前的 netsh 實例設定目的電腦。 在您結束並重新鍵入 netsh 後，目標電腦會重設為本機電腦。 您可藉由指定存放在 WINS 中的電腦名稱、UNC 名稱、DNS 伺服器所解析的網際網路名稱或 IP 位址，在遠端電腦上執行 netsh 命令。

- 如果您的字串值包含字元之間的空格，您必須將字串值括在引號中。 例如， `-r "contoso remote device"`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
