---
title: tsprof
description: Tsprof 的參考文章，可將遠端桌面服務使用者設定資訊從一位使用者複製到另一位使用者。
ms.topic: reference
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f676b1d11586d413e544d451043da242861083e1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023392"
---
# <a name="tsprof"></a>tsprof

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個使用者。
遠端桌面服務的使用者設定資訊會顯示在 [本機使用者和群組] 和 [active directory 使用者和電腦] 的遠端桌面服務擴充功能中。

**tsprof** 也可以設定使用者的設定檔路徑。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 Windows server TechNet Library 中的 [Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) 。

## <a name="syntax"></a>語法
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/update|更新網域 <*DomainName*中 <使用者*名稱*> 的設定檔路徑資訊> 以 <*Profilepath*>。|
|/domain\<DomainName>|指定要在其中套用作業之網域的名稱。|
|/local|只將作業套用至本機使用者帳戶。|
|/profile\<path>|指定 [本機使用者和群組] 和 [active directory 使用者和電腦] 中遠端桌面服務擴充功能所顯示的設定檔路徑。|
|\<UserName>|指定您要更新或查詢伺服器設定檔路徑之使用者的名稱。|
|/copy|從複製使用者設定資訊 \<*SourceUser*> \<*DestinationUser*> ，並將的設定檔路徑資訊更新為 \<*DestinationUser*> \<*Profilepath*> 。 \<*SourceUser*>和都 \<*DestinationUser*> 必須是本機或必須在網域中 \<*DomainName*> 。|
|\<Src_usr>|指定您要從中複製使用者設定資訊之使用者的名稱。|
|\<Dest_usr>|指定您要複製使用者設定資訊之使用者的名稱。|
|/q|顯示您要查詢伺服器設定檔路徑的使用者目前的設定檔路徑。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   只有當您在執行 windows Server 2008 的電腦上安裝終端機伺服器角色服務，或在執行 Windows Server 2008 R2 的電腦上 RD 工作階段主機角色服務時，才可使用 **tsprof** 命令。

## <a name="examples"></a>範例
-   若要將使用者設定資訊從 LocalUser1 複製到 LocalUser2，請輸入：
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   若要將 LocalUser1 的遠端桌面服務設定檔路徑設定為名為 c:\profiles 的目錄，請輸入：
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[遠端桌面服務 (終端機服務) 命令參考](remote-desktop-services-terminal-services-command-reference.md)
