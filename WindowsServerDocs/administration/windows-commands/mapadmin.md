---
title: mapadmin
description: Mapadmin 命令的參考文章，可管理 Microsoft Services for Network File System 的消費者名稱對應。
ms.topic: reference
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b62e31dbe53c5e2b16093bb222b8129d3cca087
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033946"
---
# <a name="mapadmin"></a>mapadmin

**Mapadmin**命令列公用程式會在執行 Microsoft Services For Network File System 的本機或遠端電腦上管理消費者名稱對應。 如果您使用沒有系統管理認證的帳戶登入，您可以指定執行之帳戶的使用者名稱和密碼。

## <a name="syntax"></a>語法

```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <windowsuser> -uu <UNIXuser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <windowsgroup> -ug <UNIXgroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <Windowsuser> [-uu <UNIXuser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <Windowsgroup> [-ug <UNIXgroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <Windowsdomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <Windowsdomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<computer>` | 指定執行您要管理之消費者名稱對應服務的遠端電腦。 您可以使用 Windows 網際網路名稱服務 (WINS) 名稱或網域名稱系統 (DNS) 名稱，或網際網路通訊協定 (IP) 位址來指定電腦。 |
| -u `<user>` | 指定要使用其認證之使用者的使用者名稱。 可能需要以 [網域 *\*使用者名稱] 的格式將功能變數名稱新增到使用者名稱。 |
| -p `<password>` | 指定使用者的密碼。 如果您指定 **-u** 選項，但省略 **-p** 選項，系統會提示您輸入使用者的密碼。 |
| `start | stop` | 啟動或停止消費者名稱對應服務。 |
| config | 指定消費者名稱對應的一般設定。 此參數可使用下列選項：<ul><li>**-r `<dddd>:<hh>:<mm>` ：** 指定從 Windows 和 NIS 資料庫更新的重新整理間隔（以天、小時和分鐘為單位）。 最小間隔為5分鐘。</li><li>**-i `{yes | no}` ：** 開啟 (**yes**) 或 off (**不**) 的簡單對應。 依預設，會開啟對應。</li></ul> |
| add | 為使用者或群組建立新的對應。 此參數可使用下列選項：<ul><li>**-wu `<name>` ：** 指定正在建立新對應的 Windows 使用者名稱。</li><li>**-uu `<name>` ：** 指定正在建立新對應的 UNIX 使用者名稱。</li><li>**-wg `<group>` ：** 指定正在建立新對應之 Windows 群組的名稱。</li><li>**-ug `<group>` ：** 指定正在建立新對應的 UNIX 組名。</li><li>**-setprimary：** 指定新的對應是主要對應。</li></ul> |
| setprimary | 針對具有多個對應的 UNIX 使用者或群組，指定哪個對應是主要對應。 此參數可使用下列選項：<ul><li>**-wu `<name>` ：** 指定主要對應的 Windows 使用者。 如果有一個以上的使用者對應存在，請使用 **-uu** 選項來指定主要對應。</li><li>**-uu `<name>` ：** 指定主要對應的 UNIX 使用者。</li><li>**-wg `<group>` ：** 指定主要對應的 Windows 群組。 如果群組有一個以上的對應存在，請使用 **-ug** 選項來指定主要對應。</li><li>**-ug `<group>` ：** 指定主要對應的 UNIX 群組。</li></ul> |
| delete | 移除使用者或群組的對應。 此參數可使用下列選項：<ul><li>**-wu `<user>` ：** 指定將刪除對應的 Windows 使用者，指定為 `<windowsdomain>\<username>` 。<p>您必須指定 **-wu** 或 **-uu** 選項，或兩者都是。 如果您同時指定這兩個選項，則會刪除兩個選項所識別的特定對應。 如果您只指定 **-wu** 選項，將會刪除指定使用者的所有對應。</li><li>**-uu `<user>` ：** 指定將刪除對應的 UNIX 使用者，指定為 `<username>` 。<p>您必須指定 **-wu** 或 **-uu** 選項，或兩者都是。 如果您同時指定這兩個選項，則會刪除兩個選項所識別的特定對應。 如果您只指定 **-uu** 選項，將會刪除指定之使用者的所有對應。</li><li>**-wg `<group>` ：** 指定將刪除對應的 Windows 群組，並指定為 `<windowsdomain>\<username>` 。<p>您必須指定 **-wg** 或 **-ug** 選項，或兩者都指定。 如果您同時指定這兩個選項，則會刪除兩個選項所識別的特定對應。 如果您只指定 **-wg** 選項，則會刪除指定群組的所有對應。</li><li>**-ug `<group>` ：** 指定要刪除對應的 UNIX 群組，並指定為 `<groupname>` 。<p>您必須指定 **-wg** 或 **-ug** 選項，或兩者都指定。 如果您同時指定這兩個選項，則會刪除兩個選項所識別的特定對應。 如果您只指定 **-ug** 選項，則會刪除指定群組的所有對應。</li></ul> |
| list | 顯示使用者和群組對應的相關資訊。 此參數可使用下列選項：<ul><li>**-all：** 列出使用者和群組的簡單和先進對應。</li><li>**-簡單：** 列出所有簡單的對應使用者和群組。</li><li>**-advanced：** 列出所有 advanced 對應的使用者和群組。 對應會依照其評估順序列出。 最先列出以星號 () 的主要地圖，*接著會列出以克拉標示的次要對應 `(^)` 。 </li> <li>* *-wu `<name>` ：** 列出指定之 Windows 使用者的對應。</li><li>**-wg `<group>` ：** 列出 Windows 群組的對應。</li><li>**-uu `<name>` ：** 列出 UNIX 使用者的對應。</li><li>**-ug `<group>` ：** 列出 UNIX 群組的對應。</li></ul> |
| 備份 (backup) | 將消費者名稱對應設定和對應資料儲存至所指定的檔案 `<filename>` 。 |
| 還原 | `<filename>`使用**備份**參數所建立) 所指定之檔案 (中的資料，取代設定和對應資料。 |
| adddomainmap | 在 Windows 網域與 NIS 網域或密碼和群組檔案之間新增簡單的對應。 此參數可使用下列選項：<ul><li>**-d `<windowsdomain>` ：** 指定要對應的 Windows 網域。</li><li>**-y `<NISdomain>` ：** 指定要對應的 NIS 網域。 您必須使用 **-n `<NISserver>` **參數，為 **-y**選項指定的 nis 網域指定 nis 伺服器。</li><li>**-f `<path>` ：** 指定目錄的完整路徑，其中包含要對應的密碼和群組檔案。 這些檔案必須位於受管理的電腦上，而且您無法使用 **mapadmin** 來管理遠端電腦，以根據密碼和群組檔案來設定對應。</li></ul> |
| removedomainmap | 移除 Windows 網域與 NIS 網域之間的簡單對應。 此參數可使用下列選項和引數：<ul><li>**-d `<windowsdomain>` ：** 指定要移除之對應的 Windows 網域。</li><li>**-y `<NISdomain>` ：** 指定要移除之對應的 NIS 網域。</li><li>**-all：** 指定要移除 Windows 和 NIS 網域之間的所有簡單對應。 這也會移除 Windows 網域和密碼與群組檔案之間的任何簡單對應。</li></ul> |
| listdomainmaps | 列出對應至 NIS 網域或密碼和群組檔案的 Windows 網域。 |

#### <a name="remarks"></a>備註

- 如果您未指定任何參數， **mapadmin** 命令會顯示消費者名稱對應的目前設定。

- 針對所有指定使用者或組名的選項，可以使用下列格式：

    - 若為 Windows 使用者，請使用下列格式： `<domain>\<username>` 、 `\\<computer>\<username>` 、 `\<computer>\<username>` 或。 `<computer>\<username>`

    - 若為 Windows 群組，請使用下列格式： `<domain>\<groupname>` 、 `\\<computer>\<groupname>` 、 `\<computer>\<groupname>` 或。 `<computer>\<groupname>`

    - 若為 UNIX 使用者，請使用下列格式： `<NISdomain>\<username>` 、 `<username>@<NISdomain>` 、 `<username>@PCNFS` 或 `PCNFS\<username>`

    - 若為 UNIX 群組，請使用下列格式： `<NISdomain>\<groupname>` 、 `<groupname>@<NISdomain>` 、 `<groupname>@PCNFS` 或 `PCNFS\<groupname>`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
