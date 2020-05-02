---
title: shadow
description: Shadow 的參考主題，可讓您從遠端控制遠端桌面工作階段主機伺服器上另一位使用者的使用中會話。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1330aef40a4bd5ce9fa6f565b92ade3f8c304895
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721821"
---
# <a name="shadow"></a>shadow

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您在遠端桌面工作階段主機伺服器上，遠端控制另一位使用者的使用中會話。



## <a name="syntax"></a>語法
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<SessionName>|指定您想要遠端控制的會話名稱。|
|\<SessionID>|指定您想要遠端控制之會話的識別碼。 使用 [**查詢使用者**] 顯示會話及其會話識別碼的清單。|
|/server：\<ServerName>|指定包含您要遠端控制之會話的 rd 工作階段主機伺服器。 根據預設，會使用目前的 rd 會話 Host4 伺服器。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您可以檢視或主動控制工作階段。 如果您選擇主動控制使用者的工作階段，即可將鍵盤和滑鼠動作輸入工作階段。
-   您可以隨時從遠端控制自己的會話（目前的會話除外），但您必須擁有 [完全控制] 許可權或 [遠端控制] 特殊存取權限，才能遠端控制另一個會話。
-   您也可以使用遠端桌面服務管理員來起始遠端控制。
-   除非停用警告，否則在開始監視之前，伺服器會警告使用者即將從遠端控制工作階段。 等待使用者的回應時，您的工作階段可能看似凍結數秒鐘。 若要設定使用者和會話的遠端控制，請使用 [遠端桌面服務設定工具] 或 [遠端桌面服務延伸模組給 [本機使用者和群組] 和 [active directory 使用者和電腦]。
-   您的工作階段必須能夠支援遠端控制的工作階段所使用的視訊解析度，否則操作就會失敗。
-   主控台會話不能從遠端控制另一個會話，也不能由另一個會話遠端控制。
-   當您想要結束遠端控制（遮蔽）時，請按\* CTRL + （ \*僅從數位鍵臺上使用）。

## <a name="examples"></a>範例
-   若要遮蔽會話93，請輸入：
    ```
    shadow 93
    ```
-   若要遮蔽會話 ACCTG01，請輸入：
    ```
    shadow ACCTG01
    ```

## <a name="additional-references"></a>其他參考
- [命令列語法金鑰](command-line-syntax-key.md)
[遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)
