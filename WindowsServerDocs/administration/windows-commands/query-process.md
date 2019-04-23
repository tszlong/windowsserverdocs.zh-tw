---
title: 查詢處理序
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e15a41fc931d2cf04d60759a63e3b80392265175
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840719"
---
# <a name="query-process"></a>查詢處理序

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示遠端桌面工作階段主機 (rd 工作階段主機) 伺服器執行的處理序的相關資訊。
您可以使用此命令，以找出特定的使用者正在執行，哪些程式，以及哪些使用者也會執行特定程式。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
## <a name="syntax"></a>語法
```
query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|*|列出所有工作階段的處理程序。|
|<ProcessID>|指定用來識別您想要查詢的程序的數值識別碼。|
|<UserName>|指定您想要列出之處理程序所屬的使用者名稱。|
|<SessionName>|指定您想要列出之處理程序所屬之工作階段的名稱。|
|/id:<nn>|指定您想要列出之處理程序所屬之工作階段的識別碼。|
|<ProgramName>|指定您想要查詢之處理程序所屬的程式名稱。 需要.exe 副檔名。|
|/server:<ServerName>|指定您想要列出之處理程序所屬的 rd 工作階段主機伺服器。 如果未指定，則會使用其中您目前登入伺服器。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   系統管理員具有完整存取權**查詢處理程序**函式。
-   如果您未指定 <*使用者名稱*>，<*SessionName*>， **/id:**<*nn*>，<*ProgramName*>，或**\*** 參數**查詢程序**會顯示屬於目前使用者處理序。
-   如果指定的工作階段，則它必須識別作用中工作階段。
-   **查詢處理程序**傳回下列資訊：
    -   擁有此程序的使用者
    -   擁有處理序工作階段
    -   工作階段的識別碼
    -   處理序名稱
    -   處理序的識別碼
-   當**查詢處理程序**傳回資訊，大於 (>) 符號會顯示目前的工作階段所屬的每個處理序之前。
## <a name="BKMK_examples"></a>範例
-   若要顯示所有工作階段正在使用的處理程序的相關資訊，請輸入：
    ```
    query process *
    ```
-   若要顯示處理序正在使用工作階段識別碼為 2 的相關資訊，請輸入：
    ```
    query process /ID:2
    ```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[查詢](query.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
