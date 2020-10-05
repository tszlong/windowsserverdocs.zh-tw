---
title: telnet
description: Telnet 命令的參考文章，此命令會與執行 telnet 伺服器服務的電腦通訊。
ms.topic: reference
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cf4fa5754aec18662800f4536afd16a427ca0952
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717925"
---
# <a name="telnet"></a>telnet

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

與執行 telnet 伺服器服務的電腦通訊。 執行此命令而不使用任何參數，可讓您輸入 telnet 內容，如 telnet 提示字元所示 (**Microsoft telnet>**) 。 從 telnet 提示字元，您可以使用 telnet 命令來管理執行 telnet 用戶端的電腦。

> [!IMPORTANT]
> 您必須先安裝 telnet 用戶端軟體，才能執行此命令。 如需詳細資訊，請參閱 [安裝 telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))。

## <a name="syntax"></a>語法

```
telnet [/a] [/e <escapechar>] [/f <filename>] [/l <username>] [/t {vt100 | vt52 | ansi | vtnt}] [<host> [<port>]] [/?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /a | 嘗試自動登入。 與 **/l** 選項相同，不同之處在于它會使用目前登入的使用者名稱。 |
| /e `<escapechar>` | 指定用來輸入 telnet 用戶端提示的 escape 字元。 |
| /f `<filename>` | 指定用於用戶端記錄的檔案名。 |
| /l `<username>` | 指定要在遠端電腦上用來登入的使用者名稱。 |
| 一起 `{vt100 | vt52 | ansi | vtnt}` | 指定終端機類型。 支援的終端機類型為 **vt100**、 **vt52**、 **ansi**和 **vtnt**。 |
| `<host> [<port>]` | 指定要連接之遠端電腦的主機名稱或 IP 位址，並選擇性地使用 (預設為 TCP 埠 23) 的 TCP 埠。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要在 *telnet.microsoft.com*上使用 telnet 連接到執行 Telnet 伺服器服務的電腦，請輸入：

```
telnet telnet.microsoft.com
```

若要使用 telnet 連接到執行 telnet 伺服器服務的電腦（位於 *TCP 通訊埠44上的 telnet.microsoft.com* ），並將會話活動記錄在稱為 *telnetlog.txt*的本機檔案中，請輸入：

```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [安裝 telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))

- [telnet 技術參考](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754987(v=ws.10))
