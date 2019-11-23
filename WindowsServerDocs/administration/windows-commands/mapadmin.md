---
title: mapadmin
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fc4b76c1989298ea83c480b9c838ce0fc18fef5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373764"
---
# <a name="mapadmin"></a>mapadmin



您可以使用**Mapadmin**來管理 Microsoft Services For Network File System 的消費者名稱對應。

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
**Mapadmin**命令列公用程式會在執行 Microsoft Services For Network File System 的本機或遠端電腦上管理消費者名稱對應。 如果您使用沒有系統管理認證的帳戶登入，則可以指定執行之帳戶的使用者名稱和密碼。

除了特定的命令引數之外， **mapadmin**也會接受下列引數和選項：

&lt;電腦&gt; 指定執行您要管理之消費者名稱對應服務的遠端電腦。 您可以使用 Windows 網際網路名稱服務（WINS）名稱或網域名稱系統（DNS）名稱或網際網路通訊協定（IP）位址來指定電腦。

-u &lt;使用者&gt; 指定要使用其認證之使用者的使用者名稱。 可能需要將功能變數名稱新增至格式為<em>domain</em> **\\** <em>使用者名稱</em>的使用者名稱。

-p &lt;密碼&gt; 指定使用者的密碼。 如果您指定 **-u**選項，但省略 **-p**選項，系統會提示您輸入使用者的密碼。
**Mapadmin**所執行的特定動作取決於您指定的命令引數：

## <a name="parameters"></a>Parameters
### <a name="start"></a>start
啟動消費者名稱對應服務。

### <a name="stop"></a>stop
停止消費者名稱對應服務。

### <a name="config"></a>web.config
指定消費者名稱對應的一般設定。 下列選項可用於此命令引數： **-r &lt;dddd&gt;：&lt;hh&gt;：&lt;mm&gt;** -指定從 WINDOWS 和 NIS 資料庫更新的重新整理間隔（以天、小時和分鐘為單位）。 最小間隔為5分鐘。
**-i {yes | no}** -開啟（**是**）或關閉的簡單對應（**no**）。 根據預設，簡單對應是 on。
**add** -為使用者或群組建立新的對應。 下列選項可用於此命令引數：

|選項|定義|
|-----|-------|
|-wu &lt;名稱&gt;|指定要建立新對應的 Windows 使用者名稱。|
|-uu &lt;名稱&gt;|指定要建立新對應的 UNIX 使用者名稱。|
|-wg &lt;群組&gt;|指定要建立新對應的 Windows 組名。|
|-ug &lt;群組&gt;|指定要建立新對應之 UNIX 群組的名稱。|
|-setprimary|指定新的對應是主要對應。|

**setprimary** -指定哪一個對應是 UNIX 使用者或具有多個對應之群組的主要對應。 下列選項可用於此命令引數：

|選項|定義|
|-----|-------|
|-wu &lt;名稱&gt;|指定主要對應的 Windows 使用者。 如果有一個以上的使用者對應存在，請使用 **-uu**選項來指定主要對應。|
|-uu &lt;名稱&gt;|指定主要對應的 UNIX 使用者。|
|-wg &lt;群組&gt;|指定主要對應的 Windows 群組。 如果群組有一個以上的對應存在，請使用 **-ug**選項來指定主要對應。|
|-ug &lt;群組&gt;|指定主要對應的 UNIX 群組。|

**delete** -移除使用者或群組的對應。 下列選項適用于此命令引數：

|選項|定義|
|-----|-------|
|-wu &lt;使用者&gt;|將刪除對應的 Windows 使用者，指定為 &lt;*WindowsDomain&gt;\\&lt;使用者名稱&gt;* 。 您必須指定 **-wu**或 **-uu**選項，或兩者。 如果您同時指定這兩個選項，則會刪除這兩個選項所識別的特定對應。 如果您只指定 **-wu**選項，將會刪除指定之使用者的所有對應。|
|-wg &lt;群組&gt;|將刪除對應的 Windows 群組，指定為 &lt;WindowsDomain&gt;\\&lt;組名&gt;。 您必須指定 **-wg**或 **-ug**選項，或兩者。 如果您同時指定這兩個選項，則會刪除這兩個選項所識別的特定對應。 如果您只指定 **-wg**選項，將會刪除指定群組的所有對應。|
|-uu &lt;使用者&gt;|將刪除對應的 UNIX 使用者，指定為 &lt;的使用者名稱&gt;。 您必須指定 **-wu**或 **-uu**選項，或兩者。 如果您同時指定這兩個選項，則會刪除這兩個選項所識別的特定對應。 如果您只指定 **-uu**選項，將會刪除指定之使用者的所有對應。|
|-ug &lt;群組&gt;|將刪除對應的 UNIX 群組，指定為 &lt;組名&gt;。 您必須指定 **-wg**或 **-ug**選項，或兩者。 如果您同時指定這兩個選項，則會刪除這兩個選項所識別的特定對應。 如果您只指定 **-ug**選項，將會刪除指定群組的所有對應。|

**清單**-顯示使用者和群組對應的相關資訊。 下列選項可用於此命令引數：

|選項|定義|
|-----|-------|
|-全部|同時列出使用者和群組的簡單和先進對應。|
|-簡單|列出所有簡單對應的使用者和群組。|
|-advanced|列出所有 advanced 對應的使用者和群組。 對應會依照其評估的順序列出。 主要對應（以星號（*）標示）會先列出，後面接著次要對應，並以克拉（^）標示。|
|-wu &lt;名稱&gt;|列出指定之 Windows 使用者的對應。|
|-wg &lt;群組&gt;|列出 Windows 群組的對應。|
|-uu &lt;名稱&gt;|列出 UNIX 使用者的對應。|
|-ug &lt;群組&gt;|列出 UNIX 群組的對應。|

**備份**-將消費者名稱對應設定和對應資料儲存至 &lt;filename&gt;所指定的檔案。
**restore** -使用**備份**命令引數所建立之檔案中的資料（由 &lt;filename&gt;）取代設定和對應資料。
**adddomainmap** -新增 Windows 網域和 NIS 網域或密碼與群組檔案之間的簡單對應。 下列選項適用于此命令引數：

|選項|定義|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要對應的 Windows 網域。|
|-y &lt;NISdomain&gt;|指定要對應的 NIS 網域。&lt;br/&gt;&lt;br/&gt; **-n** &lt;nisServer&gt; 會針對以 **-y**選項指定的 nis 網域指定 nis 伺服器。|
|-f &lt;路徑&gt;|指定目錄的完整路徑，其中包含要對應的密碼和群組檔案。 這些檔案必須位於受管理的電腦上，而且您無法使用**mapadmin**來管理遠端電腦，以根據密碼和群組檔案來設定對應。|

**removedomainmap** -移除 Windows 網域和 NIS 網域之間的簡單對應。 下列選項和引數適用于此命令引數：

|選項|定義|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要移除之對應的 Windows 網域。|
|-y &lt;NISdomain&gt;|指定要移除之對應的 NIS 網域。|
|-全部|指定要移除 Windows 和 NIS 網域之間的所有簡單對應。 這也會移除 Windows 網域和密碼與群組檔案之間的任何簡單對應。|

**listdomainmaps** -列出對應至 NIS 網域或密碼和群組檔案的 Windows 網域。

## <a name="notes"></a>附註
-   如果您未指定命令引數， **mapadmin**會顯示消費者名稱對應的目前設定。
-   針對指定使用者或組名的所有選項，可以使用下列格式：
-   對於 Windows 使用者，請使用 &lt;網域&gt;\\&lt;使用者名稱&gt;、\\\\&lt;電腦&gt;\\&lt;使用者名稱&gt;、\\&lt;電腦&gt;\\&lt;使用者名稱&gt;或 &lt;電腦&gt;\\使用者名稱 &lt;&gt;
-   針對 Windows 群組，請使用 &lt;網域&gt;\\&lt;&lt;組名&gt;&gt;，\\\\&lt;電腦&gt;\\&lt;&lt;組名&gt;&gt;、\\&lt;電腦&gt;\\&lt;&lt;組名&gt;&gt;，或 &lt;電腦&gt;\\&lt;組名 &lt;&gt;&gt;
-   針對 UNIX 使用者，請使用表單 &lt;NISdomain&gt;\\&lt;使用者名稱&gt;、&lt;使用者名稱&gt;@&lt;NISdomain&gt;、使用者 &lt;名稱&gt;@PCNFS或 PCNFS\\&lt;使用者名稱&gt;
-   針對 UNIX 群組，請使用 &lt;NISdomain&gt;\\&lt;組名&gt;、&lt;擁有群組&gt;@&lt;NISdomain&gt;、&lt;組名&gt;@PCNFS或 PCNFS\\組名組 &lt;&gt;

## <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)
