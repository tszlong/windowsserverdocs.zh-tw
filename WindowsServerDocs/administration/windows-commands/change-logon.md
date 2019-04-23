---
title: change logon
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a5668618417ec315a452d911dbe15c02c30eeb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861139"
---
>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="change-logon"></a>change logon
啟用或停用登入，從用戶端工作階段，或顯示目前的登入狀態。
此公用程式可用於系統維護。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
## <a name="syntax"></a>語法
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|顯示目前登入狀態，啟用或停用。|
|/enable|可讓用戶端工作階段，但不是從主控台的登入。|
|/disable|停用後續的登入，從用戶端工作階段，但不是從主控台。 不會影響目前登入的使用者。|
|/drain|停用登入，從新的用戶端工作階段，但是允許重新連線到現有的工作階段。|
|/drainuntilrestart|停用從新用戶端工作階段，直到電腦重新啟動，但是允許重新連線到現有的工作階段的登入。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   只有系統管理員可以使用**變更登入**命令。
-   當您重新啟動系統時，會重新啟用登入。 如果您從用戶端工作階段連線至遠端桌面工作階段主機 (rd 工作階段主機) 伺服器並停用登入，並接著登出然後再重新啟用 登入，您將無法重新連線到您的工作階段。 若要重新啟用用戶端工作階段的登入，請在主控台登入。
## <a name="BKMK_examples"></a>範例
-   若要顯示目前的登入狀態，請輸入：
    ```
    change logon /query
    ```
-   若要啟用從用戶端工作階段的登入，請輸入：
    ```
    change logon /enable
    ```
-   若要停用用戶端登入，請輸入：
    ```
    change logon /disable
    ```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[變更](change.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
