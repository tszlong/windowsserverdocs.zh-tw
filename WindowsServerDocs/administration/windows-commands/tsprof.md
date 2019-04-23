---
title: tsprof
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836069"
---
# <a name="tsprof"></a>tsprof

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

複製一位使用者的遠端桌面服務使用者的設定資訊儲存到另一個。
遠端桌面服務使用者的設定資訊會顯示在 遠端桌面服務延伸模組，以本機使用者和群組和 active directory 使用者和電腦。

**tsprof**也可以設定的使用者設定檔路徑。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/update|更新設定檔的路徑資訊 <*使用者名稱*> 在網域中 <*DomainName*> 到 <*設定檔路徑*>。|
|/domain:\<網域名稱 >|指定套用該作業的網域名稱。|
|本機 /|此作業僅適用於本機使用者帳戶。|
|/ 設定檔：\<路徑 >|指定的遠端桌面服務延伸模組，在 本機使用者和群組和 active directory 使用者和電腦中所顯示的設定檔路徑。|
|\<UserName>|指定您要更新或查詢伺服器設定檔路徑的使用者名稱。|
|/copy|中的使用者組態資訊複製\< *SourceUser*> 來\< *DestinationUser*> 和更新的設定檔的路徑資訊\< *使用 「*> 以\<*設定檔路徑*>。 兩者\< *SourceUser*> 並\< *DestinationUser*> 必須是本機或網域中必須是\< *DomainName*>.|
|\<Src_usr>|指定您要複製的使用者組態資訊的使用者名稱。|
|\<Dest_usr>|指定您要複製的使用者組態資訊的使用者名稱。|
|/q|顯示目前的設定檔路徑，您要查詢伺服器設定檔路徑的使用者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Tsprof**您已在執行 Windows Server 2008 R2 的電腦上執行 Windows Server 2008 或 RD 工作階段主機角色服務的電腦上安裝終端機伺服器角色服務時，才可用命令。

## <a name="BKMK_examples"></a>範例
-   若要複製 LocalUser1 的使用者設定資訊儲存到 LocalUser2，輸入：
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   若要設定的遠端桌面服務設定檔路徑 LocalUser1 到名為"c:\profiles"的目錄，輸入：
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
