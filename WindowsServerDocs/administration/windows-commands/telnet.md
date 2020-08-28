---
title: telnet
description: Telnet 的參考文章，與執行 telnet 伺服器服務的電腦通訊。
ms.topic: reference
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdf379a8d827ced295f1c36ac6c44ab5e167e66f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038287"
---
# <a name="telnet"></a>telnet

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

與執行 telnet 伺服器服務的電腦通訊。

## <a name="syntax"></a>語法
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/a|嘗試自動登入。 與/l 選項相同，但會使用目前登入的使用者名稱。|
|/e \<EscapeChar>|用來輸入 telnet 用戶端提示的 Escape 字元。|
|/f \<FileName>|用於用戶端記錄的檔案名。|
|/l \<UserName>|指定要在遠端電腦上用來登入的使用者名稱。|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|指定終端機類型。 支援的終端機類型為 vt100、vt52、ansi 和 vtnt。|
|\<Host> [\<Port>]|指定要連接之遠端電腦的主機名稱或 IP 位址，並選擇性地使用 (預設為 TCP 埠 23) 的 TCP 埠。|
|/?|在命令提示字元顯示說明。 或者，您可以輸入/h。|

## <a name="remarks"></a>備註
-   您必須先安裝 telnet 用戶端軟體，才能執行此命令。 如需詳細資訊，請參閱 [安裝 telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))。
-   您可以在沒有參數的情況下執行 telnet，以輸入 telnet 內容（由 telnet 提示字元指出 (**Microsoft telnet>**) 。 從 telnet 提示字元，您可以使用 telnet 命令來管理執行 telnet 用戶端的電腦。

## <a name="examples"></a>範例
使用 telnet 連接到執行 telnet 伺服器服務的電腦（位於 telnet.microsoft.com）。
```
telnet telnet.microsoft.com
```
使用 telnet 連接到執行 telnet 伺服器服務的電腦（位於 TCP 通訊埠44的 telnet.microsoft.com），並將會話活動記錄在名為 telnetlog.txt 的本機檔案中。
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他參考資料
-   [安裝 telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))
-   [telnet 技術參考](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754987(v=ws.10))
- [命令列語法關鍵](command-line-syntax-key.md)
