---
title: telnet
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858559"
---
# <a name="telnet"></a>telnet

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

與執行 telnet 伺服器服務的電腦進行通訊。 
## <a name="syntax"></a>語法
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/a|嘗試自動登入。 相同 /l 選項除了會使用目前登入 s 使用者名稱。|
|/e \<EscapeChar>|逸出字元，用來輸入 telnet 用戶端的命令提示字元。|
|/f \<FileName>|用於用戶端記錄的檔案名稱。|
|/l\<使用者名稱 >|指定要使用登入遠端電腦上的使用者名稱。|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|指定的終端機類型。 支援的終端機類型為 vt100、 vt52、 ansi 和 vtnt。|
|\<Host> [\<Port>]|指定的主機名稱或 IP 位址連接至遠端電腦和 （選擇性） 若要使用的 TCP 連接埠 （預設為 TCP 連接埠 23）。|
|/?|在命令提示字元顯示說明。 或者，您可以輸入 /h。|

## <a name="remarks"></a>備註
-   您可以執行此命令之前，您必須安裝 telnet 用戶端軟體。 如需詳細資訊，請參閱 <<c0> [ 安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)。
-   您可以執行 telnet，而不需要輸入 telnet 內容中，由 telnet 命令提示字元參數 (**Microsoft telnet >**)。 從 telnet 命令提示字元中，您可以使用 telnet 命令來管理電腦執行 telnet 用戶端。

## <a name="BKMK_Examples"></a>範例
使用 telnet 來連接到在 telnet.microsoft.com 執行 telnet 伺服器服務的電腦。
```
telnet telnet.microsoft.com
```
使用 telnet 連線到執行 telnet 伺服器服務在 telnet.microsoft.com，TCP 連接埠 44 上的電腦，並在本機檔案，稱為 telnetlog.txt 登入工作階段活動
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他參考資料
-   [安裝 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [telnet 技術參考](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [命令列語法關鍵](command-line-syntax-key.md)
