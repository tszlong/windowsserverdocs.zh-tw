---
title: shadow
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb2fad4b0a553e736755f2dc56e5d88297a1fef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383956"
---
# <a name="shadow"></a>shadow

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您從遠端控制遠端桌面工作階段主機（rd 工作階段主機）伺服器上另一位使用者的使用中會話。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parameters
|參數|描述|
|-------|--------|
|\<SessionName >|指定您想要遠端控制的會話名稱。|
|\<SessionID >|指定您想要遠端控制之會話的識別碼。 使用 [**查詢使用者**] 顯示會話及其會話識別碼的清單。|
|/server：\<ServerName >|指定包含您要遠端控制之會話的 rd 工作階段主機伺服器。 根據預設，會使用目前的 rd 會話 Host4 伺服器。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您可以查看或主動控制會話。 如果您選擇主動控制使用者的會話，您就能夠在會話中輸入鍵盤和滑鼠動作。
-   您可以隨時從遠端控制自己的會話（目前的會話除外），但您必須擁有 [完全控制] 許可權或 [遠端控制] 特殊存取權限，才能遠端控制另一個會話。
-   您也可以使用遠端桌面服務管理員來起始遠端控制。
-   開始監視之前，除非停用此警告，否則伺服器會警告使用者即將從遠端控制會話。 您的會話可能會在等待使用者回應的幾秒鐘後出現凍結。 若要設定使用者和會話的遠端控制，請使用 遠端桌面服務設定工具 或 遠端桌面服務延伸模組給 本機使用者和群組 和 active directory 使用者和電腦。
-   您的會話必須能夠支援您遠端控制的會話所使用的影片解析，否則作業會失敗。
-   主控台會話不能從遠端控制另一個會話，也不能由另一個會話遠端控制。
-   當您想要結束遠端控制（遮蔽）時，請按 CTRL +\* （僅使用數位鍵臺上的 \*）。

## <a name="BKMK_examples"></a>典型
-   若要遮蔽會話93，請輸入：
    ```
    shadow 93
    ```
-   若要遮蔽會話 ACCTG01，請輸入：
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>其他參考

[ &#40;遠端桌面服務終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)[的命令列語法金鑰](command-line-syntax-key.md)
