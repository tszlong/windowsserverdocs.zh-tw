---
title: tsprof
description: Tsprof 的參考文章，會將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3abaf2413348edd723962ad99a19be5aa435a495
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954870"
---
# <a name="tsprof"></a>tsprof

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個。
[本機使用者和群組] 和 [active directory 使用者和電腦] 的 [遠端桌面服務延伸] 中會顯示遠端桌面服務的使用者設定資訊。

**tsprof**也可以設定使用者的設定檔路徑。



> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11))。

## <a name="syntax"></a>語法
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/update|更新網域 <*DomainName*> 中 <*UserName*> 的設定檔路徑資訊，以 <*Profilepath*>。|
|/domain\<DomainName>|指定套用作業的網功能變數名稱稱。|
|/local|僅將作業套用至本機使用者帳戶。|
|/profile\<path>|指定 [本機使用者和群組] 和 [active directory 使用者和電腦] 的 [遠端桌面服務延伸模組] 中所顯示的設定檔路徑。|
|\<UserName>|指定您想要更新或查詢伺服器設定檔路徑的使用者名稱。|
|/copy|將使用者設定資訊從複製 \<*SourceUser*> 到 \<*DestinationUser*> ，並將的設定檔路徑資訊更新為 \<*DestinationUser*> \<*Profilepath*> 。 \<*SourceUser*>和都 \<*DestinationUser*> 必須是本機或必須在網域中 \<*DomainName*> 。|
|\<Src_usr>|指定您要從中複製使用者設定資訊的使用者名稱。|
|\<Dest_usr>|指定您要將使用者設定資訊複製到其中的使用者名稱。|
|/q|顯示您要查詢伺服器設定檔路徑的使用者目前的設定檔路徑。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   只有當您已在執行 Windows Server 2008 的電腦上安裝終端機伺服器角色服務，或在執行 Windows Server 2008 R2 的電腦上 RD 工作階段主機角色服務時，才可以使用**tsprof**命令。

## <a name="examples"></a>範例
-   若要將使用者設定資訊從 LocalUser1 複製到 LocalUser2，請輸入：
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   若要將 LocalUser1 的遠端桌面服務設定檔路徑設定為名為 c:\profiles 的目錄，請輸入：
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md) 
[遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)
