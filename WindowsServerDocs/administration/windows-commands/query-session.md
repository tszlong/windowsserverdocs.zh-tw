---
title: 查詢工作階段
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b96f7e6a6252b40e5a32910d161b1fa0ff94e11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868889"
---
# <a name="query-session"></a>查詢工作階段

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的工作階段的相關資訊。
此清單包含使用中工作階段的相關不僅伺服器執行其他工作階段的相關資訊。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
## <a name="syntax"></a>語法
```
query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<SessionName>|指定您想要查詢之工作階段的名稱。|
|<UserName>|指定您想要查詢其工作階段的使用者名稱。|
|<SessionID>|指定您想要查詢的工作階段識別碼。|
|/server:<ServerName>|識別要查詢的 rd 工作階段主機伺服器。 預設為目前的伺服器。|
|/mode|會顯示目前行設定。|
|/flow|會顯示目前的流程控制設定。|
|/connect|顯示目前的連線設定。|
|/counter|顯示目前的計數器資訊，包括建立的工作階段總數中斷連接，並重新連線。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   使用者一律可以查詢的使用者目前登入工作階段。 若要查詢的其他工作階段，使用者必須具有查詢資訊 特殊存取權限。
-   如果您未使用指定的工作階段 <*SessionName*>，<*UserName*>，或 <*SessionID*>，**查詢工作階段**顯示系統中的所有作用中工作階段的相關資訊。
-   當**查詢工作階段**傳回資訊，大於 (>) 符號會顯示目前的工作階段之前。 以下是範例輸出**查詢工作階段**:
    ```
    C:\>query session
     SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
    >console        Administrator1  0 active wdcon
     rdp-tcp#1      User1           1 active wdtshare
     rdp-tcp                        2 listen wdtshare
                                    4 idle
                                    5 idle
    ```
    大於 (>) 符號會指出目前的工作階段。 工作階段名稱會指定指派給工作階段的名稱。 使用者名稱會指出使用者的連線工作階段的使用者名稱。 狀態會提供工作階段的目前狀態的相關資訊。 類型表示的工作階段型別。 裝置不存在 主控台 或 網路連線的工作階段，是指派給工作階段的裝置名稱。 下列工作階段資訊的註解是從工作階段設定檔。 在其中的初始狀態設定為已停用任何工作階段不會出現在**查詢工作階段**清單，直到它們啟用。
## <a name="BKMK_examples"></a>範例
-   若要在伺服器 SERver2 上顯示所有作用中的工作階段的相關資訊，請輸入：
    ```
    query session /server:SERver2
    ```
-   若要顯示使用中工作階段 modeM02 的相關資訊，請輸入：
    ```
    query session modeM02
    ```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[查詢](query.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
