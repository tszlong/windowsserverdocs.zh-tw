---
title: telnet
description: Telnet 的參考文章，與執行 telnet 伺服器服務的電腦通訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5f588328deb51109ee9139b6e7dfaad8f0166dc
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934218"
---
# <a name="telnet"></a>telnet

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

與執行 telnet 伺服器服務的電腦通訊。

## <a name="syntax"></a>語法
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|/a|嘗試自動登入。 與/l 選項相同，但會使用目前登入的使用者名稱。|
|/e\<EscapeChar>|用來輸入 telnet 用戶端提示的 Escape 字元。|
|/f \<FileName>|用於用戶端記錄的檔案名。|
|/l\<UserName>|指定要在遠端電腦上登入的使用者名稱。|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|指定終端機類型。 支援的終端機類型為 vt100、vt52、ansi 和 vtnt。|
|\<Host> [\<Port>]|指定要連接之遠端電腦的主機名稱或 IP 位址，以及選擇性地使用的 TCP 埠（預設為 TCP 埠23）。|
|/?|在命令提示字元顯示說明。 或者，您也可以輸入/h。|

## <a name="remarks"></a>備註
-   您必須先安裝 telnet 用戶端軟體，才能執行此命令。 如需詳細資訊，請參閱[安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)。
-   您可以不使用參數來執行 telnet，以輸入 telnet 內容，如 telnet 提示（**Microsoft telnet>**）所示。 從 telnet 提示字元，您可以使用 telnet 命令來管理執行 telnet 用戶端的電腦。

## <a name="examples"></a>範例
使用 telnet 連線到執行 telnet 伺服器服務的電腦，網址為 telnet.microsoft.com。
```
telnet telnet.microsoft.com
```
使用 telnet 連線到執行 telnet 伺服器服務的電腦（位於 TCP 通訊埠44上的 telnet.microsoft.com），並在名為的本機檔案中記錄會話活動 telnetlog.txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他參考資料
-   [安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [telnet 技術參考](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
- [命令列語法關鍵](command-line-syntax-key.md)
