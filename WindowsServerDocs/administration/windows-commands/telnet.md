---
title: telnet
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1621a10b9b445e87eb6bc16a8d7f52a64c146a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385149"
---
# <a name="telnet"></a>telnet

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

與執行 telnet 伺服器服務的電腦通訊。 
## <a name="syntax"></a>語法
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parameters
|參數|描述|
|-------|--------|
|/a|嘗試自動登入。 與/l 選項相同，但會使用目前登入的使用者名稱。|
|/e \<EscapeChar >|用來輸入 telnet 用戶端提示的 Escape 字元。|
|/f \<FileName >|用於用戶端記錄的檔案名。|
|/l \<使用者名稱 >|指定要在遠端電腦上登入的使用者名稱。|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|指定終端機類型。 支援的終端機類型為 vt100、vt52、ansi 和 vtnt。|
|\<主機 > [\<埠 >]|指定要連接之遠端電腦的主機名稱或 IP 位址，以及選擇性地使用的 TCP 埠（預設為 TCP 埠23）。|
|/?|在命令提示字元顯示說明。 或者，您也可以輸入/h。|

## <a name="remarks"></a>備註
-   您必須先安裝 telnet 用戶端軟體，才能執行此命令。 如需詳細資訊，請參閱[安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)。
-   您可以不使用參數來執行 telnet，以輸入 telnet 內容，如 telnet 提示（**Microsoft telnet >** ）所示。 從 telnet 提示字元，您可以使用 telnet 命令來管理執行 telnet 用戶端的電腦。

## <a name="BKMK_Examples"></a>典型
使用 telnet 連線到執行 telnet 伺服器服務的電腦，網址為 telnet.microsoft.com。
```
telnet telnet.microsoft.com
```
使用 telnet 連線到執行 telnet 伺服器服務的電腦（位於 TCP 通訊埠44上的 telnet.microsoft.com），並將會話活動記錄在名為 telnetlog 的本機檔案中。
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他參考
-   [安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [telnet 技術參考](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [命令列語法關鍵](command-line-syntax-key.md)
