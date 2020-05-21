---
title: query session
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b0d122beac43abfd826cb406adac4aa277fc72e
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436273"
---
# <a name="query-session"></a>query session

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機（rd 工作階段主機）伺服器上的會話相關資訊。
此清單不僅包含作用中會話的資訊，還包括伺服器執行的其他會話。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>語法
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ### <a name="parameters"></a>參數
>
> |      參數       |                                                      描述                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               指定您想要查詢之會話的名稱。                               |
> |      <UserName>      |                           指定您想要查詢其會話的使用者名稱。                            |
> |     <SessionID>      |                                指定您想要查詢之會話的識別碼。                                |
> | /server:<ServerName> |                  識別要查詢的 rd 工作階段主機伺服器。 預設值為目前的伺服器。                   |
> |        /mode         |                                            顯示目前的線條設定。                                            |
> |        /flow         |                                        顯示目前的流程式控制制設定。                                        |
> |       /connect       |                                          顯示目前的連接設定。                                           |
> |       /counter       | 顯示目前的計數器資訊，包括建立、中斷連線和重新連線的會話總數。 |
> |          /?          |                                         在命令提示字元顯示說明。                                          |
>
>#### <a name="remarks"></a>備註
> - 使用者一律可以查詢使用者目前登入的會話。 若要查詢其他會話，使用者必須擁有「查詢資訊」特殊存取權限。
> - 如果您未使用 <*SessionName*>、<使用者*名稱*> 或 <*SessionID*> 指定會話，則**查詢會話**會顯示系統中所有使用中會話的相關資訊。
> - 當**查詢會話**傳回信息時，會在目前的會話之前顯示大於（>）符號。 以下是**查詢會話**的範例輸出：
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   大於（>）符號表示目前的會話。 SESSIONNAME 指定指派給會話的名稱。 使用者名稱表示連接到會話之使用者的使用者名稱。 狀態提供會話目前狀態的相關資訊。 類型表示會話類型。 裝置（不存在於主控台或網路連線的會話）是指派給會話的裝置名稱。 下列會話資訊的批註來自會話設定檔。 初始狀態設定為 [已停用] 的任何會話，都不會顯示在 [**查詢會話**] 清單中，直到它們啟用為止。
>   ## <a name="examples"></a>範例
> - 若要顯示伺服器 SERver2 上所有作用中會話的相關資訊，請輸入：
>   ```
>   query session /server:SERver2
>   ```
> - 若要顯示作用中會話 modeM02 的相關資訊，請輸入：
>   ```
>   query session modeM02
>   ```
>   ## <a name="additional-references"></a>其他參考
>   - [命令列語法索引鍵](command-line-syntax-key.md) 
>   [查詢](query.md) 
>   [遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)
