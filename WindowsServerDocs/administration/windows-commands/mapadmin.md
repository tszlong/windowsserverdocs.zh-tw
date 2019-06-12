---
title: mapadmin
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ad9f55ba130014227326f4abe8540c78755f6c5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437371"
---
# <a name="mapadmin"></a>mapadmin



您可以使用**Mapadmin**來管理 Microsoft services for Network File System 的使用者名稱對應。

## <a name="syntax"></a>語法
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>描述
**Mapadmin**命令列公用程式執行 Microsoft Services for Network File System 的本機或遠端電腦上管理 使用者名稱對應。 如果您不需要系統管理認證的帳戶登入，您可以指定的使用者名稱和密碼的帳戶。

除了特定命令引數， **mapadmin**接受下列引數和選項：

&lt;電腦&gt;指定執行您想要管理的使用者名稱對應服務的遠端電腦。 您可以指定電腦使用 Windows 網際網路名稱服務 (WINS) 名稱或網域名稱系統 (DNS) 名稱，或透過網際網路通訊協定 (IP) 位址。

-u&lt;使用者&gt;指定所要使用的認證之使用者的使用者名稱。 可能需要將網域名稱新增至表單中的使用者名稱<em>網域</em> **\\** <em>使用者名</em>。

-p&lt;密碼&gt;指定之使用者的密碼。 如果您指定 **-u**選項，但是省略 **-p**選項，系統會提示輸入使用者的密碼。
特定的動作所**mapadmin**執行取決於您所指定的命令引數：

## <a name="parameters"></a>參數
### <a name="start"></a>start
啟動的使用者名稱對應的服務。

### <a name="stop"></a>stop
停止使用者名稱對應服務。

### <a name="config"></a>設定
指定使用者名稱對應的一般設定。 使用此命令的引數有下列選項： **-r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  -指定的重新整理間隔更新 Windows 和 NIS 資料庫天數、 小時和分鐘。 最小間隔為 5 分鐘。
**-i {yes | 沒有}** -開啟簡單對應 (**是**) 或 [關閉]\(**沒有**)。 根據預設，簡單的對應是在上。
**新增**-建立新的對應使用者或群組。 使用此命令的引數有下列選項：

|選項|定義|
|-----|-------|
|-wu &lt;name&gt;|指定正在建立新的對應之 Windows 使用者名稱。|
|-uu &lt;name&gt;|指定正在建立新對應的 UNIX 使用者名稱。|
|-wg &lt;group&gt;|指定正在建立新對應的 Windows 群組的名稱。|
|-ug&lt;群組&gt;|指定正在建立新的對應之 UNIX 群組的名稱。|
|-setprimary|指定新的對應是主要的對應。|

**setprimary** -指定對應是多個對應的 UNIX 使用者或群組的主要對應。 使用此命令的引數有下列選項：

|選項|定義|
|-----|-------|
|-wu &lt;name&gt;|指定 Windows 使用者的主要對應。 有多個使用者對應時，使用 **-uu**選項以指定的主要對應。|
|-uu &lt;name&gt;|指定主要對應的 UNIX 的使用者。|
|-wg &lt;group&gt;|指定 Windows 群組的主要對應。 如果一個以上的對應群組存在，請 **-ug**選項以指定的主要對應。|
|-ug&lt;群組&gt;|指定主要對應的 UNIX 的群組。|

**刪除**-移除使用者或群組的對應。 這個命令引數有下列選項：

|選項|定義|
|-----|-------|
|-wu &lt;user&gt;|Windows 使用者，將會刪除對應，指定為&lt; *WindowsDomain&gt;\\&lt;使用者名稱&gt;* 。 您必須指定 **-wu**或 **-uu**選項，或兩者。 如果您指定這兩個選項，將會刪除特定的兩個選項所識別的對應。 如果您只有指定 **-wu**選項，所有對應將會刪除指定的使用者。|
|-wg &lt;group&gt;|對應將被刪除，做為指定的 Windows 群組&lt;WindowsDomain&gt;\\&lt;groupname&gt;。 您必須指定 **-wg**或 **-ug**選項，或兩者。 如果您指定這兩個選項，將會刪除特定的兩個選項所識別的對應。 如果您只有指定 **-wg**選項，所有對應將會刪除指定的群組。|
|-uu &lt;user&gt;|UNIX 使用者的對應將被刪除，指定為&lt;使用者名&gt;。 您必須指定 **-wu**或 **-uu**選項，或兩者。 如果您指定這兩個選項，將會刪除特定的兩個選項所識別的對應。 如果您只有指定 **-uu**選項，所有對應將會刪除指定的使用者。|
|-ug&lt;群組&gt;|對應將被刪除，指定為 UNIX 群組&lt;groupname&gt;。 您必須指定 **-wg**或 **-ug**選項，或兩者。 如果您指定這兩個選項，將會刪除特定的兩個選項所識別的對應。 如果您只有指定 **-ug**選項，所有對應將會刪除指定的群組。|

**清單**-顯示使用者和群組對應的相關資訊。 使用此命令的引數有下列選項：

|選項|定義|
|-----|-------|
|-所有|列出使用者和群組的簡單與進階對應。|
|-simple|列出所有的簡單對應的使用者和群組。|
|-進階|列出所有進階的對應的使用者和群組。 對應會列在它們的評估的順序。 主要的對應，以星號 （*） 標示會先列出，後面接著第二個對應，會標有插入號 (^)。|
|-wu &lt;name&gt;|列出指定的 Windows 使用者的對應。|
|-wg &lt;group&gt;|列出 Windows 群組的對應。|
|-uu &lt;name&gt;|列出所對應的 UNIX 使用者。|
|-ug&lt;群組&gt;|列出 UNIX 群組的對應。|

**備份**-將使用者名稱對應的組態，並將資料對應至所指定的檔案&lt;filename&gt;。
**還原**-設定和對應的資料取代檔案中的資料 (所指定&lt;檔名&gt;)，建立使用**備份**命令引數。
**adddomainmap** -加入 Windows 網域和 NIS 網域或密碼及群組檔案之間的簡單對應。 這個命令引數有下列選項：

|選項|定義|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要對應的 Windows 網域。|
|-y &lt;NISdomain&gt;|指定要對應的 NIS 網域。&lt;b /&gt;&lt;b /&gt; **-n** &lt;nisServer&gt;指定與所指定的 NIS 網域的 NIS 伺服器 **-y**選項。|
|-f &lt;path&gt;|指定目錄，包含要對應密碼及群組檔案的完整的的路徑。 檔案必須位於電腦管理，而且您無法使用**mapadmin**來管理遠端電腦來設定密碼及群組的檔案為基礎的對應。|

**removedomainmap** -移除 Windows 網域和 NIS 網域之間的簡單對應。 下列選項和引數可供這個命令引數：

|選項|定義|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要移除之對應的 Windows 網域。|
|-y &lt;NISdomain&gt;|指定要移除之對應的 NIS 網域。|
|-所有|指定要移除所有 Windows 和 NIS 網域之間的簡單對應。 這也會移除任何 Windows 網域、 密碼及群組檔案之間的簡單地圖。|

**listdomainmaps** -列出會對應至 NIS 網域或密碼及群組檔案的 Windows 網域。

## <a name="notes"></a>附註
-   如果您未指定為命令引數， **mapadmin**會顯示目前設定的使用者名稱對應。
-   如需指定使用者或群組名稱的所有選項，可以使用下列格式：
-   Windows 使用者使用表單&lt;網域&gt;\\&lt;使用者名稱&gt;， \\ \\&lt;電腦&gt;\\&lt;使用者名稱&gt;， \\&lt;電腦&gt;\\&lt;使用者名稱&gt;，或&lt;電腦&gt;\\&lt;使用者名稱&gt;
-   為 Windows 群組，請使用 表單&lt;網域&gt;\\&lt;&lt;groupname&gt;&gt;， \\ \\&lt;電腦&gt;\\ &lt; &lt;groupname&gt;&gt;， \\&lt;電腦&gt;\\&lt;&lt;groupname&gt; &gt;，或&lt;電腦&gt;\\&lt;&lt;groupname&gt;&gt;
-   針對 UNIX 使用者使用表單&lt;NISdomain&gt;\\&lt;使用者名稱&gt;，&lt;使用者名稱&gt;@&lt;NISdomain&gt;，使用者&lt;名稱&gt;@PCNFS，或 PCNFS\\&lt;使用者名稱&gt;
-   針對 UNIX 群組中，使用表單&lt;NISdomain&gt;\\&lt;groupname&gt;， &lt;groupname&gt;@&lt;NISdomain&gt;， &lt;groupname&gt;@PCNFS，或 PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
