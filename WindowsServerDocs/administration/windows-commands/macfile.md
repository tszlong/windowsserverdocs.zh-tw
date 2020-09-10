---
title: macfile
description: 用於管理 File Server for Macintosh 伺服器、磁片區、目錄和檔案的 macfile 命令的參考文章。
ms.topic: reference
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ed5af42108d56016a4b5793993cf19c80a87ff3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633831"
---
# <a name="macfile"></a>macfile

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理 File Server for Macintosh 伺服器、磁片區、目錄和檔案。 您可以將一連串的命令包含在批次檔中，並以手動方式啟動它們或在預先定義的時間啟動，以將管理工作自動化。

## <a name="modify-directories-in-macintosh-accessible-volumes"></a>修改 Macintosh 可存取磁片區中的目錄

變更 Macintosh 可存取磁片區的目錄名稱、位置、擁有者、群組和許可權。

### <a name="syntax"></a>語法

```
macfile directory[/server:\\<computername>] /path:<directory> [/owner:<ownername>] [/group:<groupname>] [/permissions:<permissions>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /server:`\\<computername>` | 指定要變更目錄的伺服器。 如果省略，則會在本機電腦上執行操作。 |
| /path`<directory>` | 指定您想要變更之目錄的路徑。 此為必要參數。 **注意：** 目錄必須存在，使用 **macfile 目錄** 將不會建立目錄。 |
| /owner`<ownername>` | 變更目錄的擁有者。 如果省略，則擁有者名稱將不會變更。 |
| 工作組`<groupname>` | 指定或變更與目錄相關聯的 Macintosh 主要群組。 如果省略，則主要群組會維持不變。 |
| 授權`<permissions>` | 設定擁有者、主要群組和世界 (所有) 之目錄的許可權。 這必須是11位數的數位，其中數位1授與許可權，而0撤銷許可權 (例如 11111011000) 。 如果省略此參數，許可權會維持不變。 |
| /? | 在命令提示字元顯示說明。 |

##### <a name="position-of-permissions-digit"></a>許可權位數的位置

許可權位數的位置決定所設定的許可權，包括：

| 位置 | 設定許可權 |
| -------- | --------------- |
| First | OwnerSeeFiles |
| Second | OwnerSeeFolders |
| Third | OwnerMakechanges |
| 第四個 | GroupSeeFiles |
| 步驟 5 | GroupSeeFolders |
| 步驟 6 | GroupMakechanges |
| 七 | WorldSeeFiles |
| 八 | WorldSeeFolders |
| 九 | WorldMakechanges |
| 十 | 無法重新命名、移動或刪除目錄。 |
| 十一 | 這些變更會套用至目前的目錄和所有子目錄。 |

##### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字 (例如 " `<computer name>` " ) 。

- 使用 **macfile 目錄** ，將 macintosh 可存取磁片區中的現有目錄設為可供 macintosh 使用者使用。 **Macfile directory**命令不會建立目錄。

- 使用 [檔案管理員]、[命令提示字元] 或 [ **macintosh 新增資料夾** ] 命令，在使用 **macfile directory** 命令之前，在可存取 macintosh 的磁片區中建立目錄。

#### <a name="examples"></a>範例

若要指派 [ *查看*檔案]、[ *資料夾*] 和 [對擁有者 *進行變更* ] 許可權，若要設定 [查看所有其他使用者的 *資料夾* 許可權]，以及防止重新命名、移動或刪除目錄，請輸入：

```
macfile directory /path:e:\statistics\may sales /permissions:11111011000
```

在 E:\ 上，位於 Macintosh 可存取磁片區*統計資料*中的子目錄*可能是銷售量*本機伺服器的磁片磁碟機。

## <a name="join-a-macintosh-files-data-and-resource-forks"></a>加入 Macintosh 檔案的資料和資源分支

指定要加入檔案的伺服器、建立檔案的物件、資料分支所在的檔案類型、資源分支所在位置，以及輸出檔案的所在位置。

### <a name="syntax"></a>語法

```
macfile forkize[/server:\\<computername>] [/creator:<creatorname>] [/type:<typename>]  [/datafork:<filepath>] [/resourcefork:<filepath>] /targetfile:<filepath>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /server:`\\<computername>` | 指定要加入檔案的伺服器。 如果省略，則會在本機電腦上執行操作。 |
| application`<creatorname>` | 指定檔案的建立者。 Macintosh 搜尋工具會使用 **/creator** 命令列選項來判斷建立檔案的應用程式。 |
| /type`<typename>` | 指定檔案類型。 Macintosh 搜尋工具會使用 **/type** 命令列選項，來決定建立檔案的應用程式中的檔案類型。 |
| /datafork:`<filepath>` | 指定要加入之資料分支的位置。 您可以指定遠端路徑。 |
| /resourcefork:`<filepath>` | 指定要聯結之資源分支的位置。 您可以指定遠端路徑。 |
| targetfile`<filepath>` | 指定藉由聯結資料派生和資源分支所建立之檔案的位置，或指定您要變更其類型或建立者之檔案的位置。 檔案必須在指定的伺服器上。 此為必要參數。 |
| /? | 在命令提示字元顯示說明。 |

##### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字 (例如 " `<computer name>` " ) 。

#### <a name="examples"></a>範例

若要在 Macintosh 可存取的磁片區*D:\Release*上建立檔案*tree_app* 、使用資源分支*C:\Cross\Mac\Appcode*，以及將這個新檔案顯示為 macintosh 用戶端的應用程式 (macintosh 應用程式使用*appl.exe*) 搭配 creator (signature) 設定為*木蘭*，請輸入：

```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\tree_app
```

若要將檔案建立者變更為*Microsoft Word 5.1* *Word.txt* ，請在 [伺服器* \\ ServerA*] 的 [ *D:\Word*] 資料夾中，于 [伺服器] 上輸入：

```
macfile forkize /server:\\ServerA /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="change-the-sign-in-message-and-limit-sessions"></a>變更登入訊息和限制會話

變更當使用者登入 File Server for Macintosh 伺服器時所顯示的登入訊息，以及限制可同時使用 Macintosh 檔案和列印伺服器的使用者數目。

### <a name="syntax"></a>語法

```
macfile server [/server:\\<computername>] [/maxsessions:{number | unlimited}] [/loginmessage:<message>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| /server:`\\<computername>` | 指定要變更參數的伺服器。 如果省略，則會在本機電腦上執行操作。 |
| maxsessions`{number | unlimited}` | 指定可以同時使用 Macintosh 檔案和列印伺服器的最大使用者數目。 如果省略，伺服器的 **maxsessions** 設定會維持不變。 |
| /loginmessage:`<message>` | 變更 Macintosh 使用者登入 File Server for Macintosh 伺服器時所看到的訊息。 登入訊息的字元數上限為199。 如果省略，伺服器的 **loginmessage** 訊息會維持不變。 若要移除現有的登入訊息，請包含 **/loginmessage** 參數，但請將 *訊息* 變數保留空白。 |
| /? | 在命令提示字元顯示說明。 |

##### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字 (例如 " `<computer name>` " ) 。

#### <a name="examples"></a>範例

若要將本機伺服器上 Macintosh 會話的允許檔案和列印伺服器數目變更為五個會話，並在完成時新增登入訊息「登出伺服器 for Macintosh」，請輸入：

```
macfile server /maxsessions:5 /loginmessage:Sign off from Server for Macintosh when you are finished
```

## <a name="add-change-or-remove-macintosh-accessible-volumes"></a>新增、變更或移除 Macintosh 可存取的磁片區

新增、變更或移除 Macintosh 可存取的磁片區。

### <a name="syntax"></a>語法

```
macfile volume {/add|/set} [/server:\\<computername>] /name:<volumename>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<password>] [/maxusers:{<number>>|unlimited}]
macfile volume /remove[/server:\\<computername>] /name:<volumename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{/add | /set}` | 新增或變更 Macintosh 可存取磁片區時的必要元件。 新增或變更指定的磁片區。 |
| /server:`\\<computername>` | 指定要在其中新增、變更或移除磁片區的伺服器。 如果省略，則會在本機電腦上執行操作。 |
| /name`<volumename>` | 必要。 指定要新增、變更或移除的磁片區名稱。 |
| /path`<directory>` | 只有當您新增磁片區時，才需要與有效。 指定要新增之磁片區根目錄的路徑。 |
| readonly`{true | false}` | 指定使用者是否可以變更磁片區中的檔案。 使用 **True** 指定使用者無法變更磁片區中的檔案。 使用 **False** 來指定使用者可以變更磁片區中的檔案。 如果在新增磁片區時省略，則允許對檔案進行變更。 如果在變更磁片區時省略，則磁片區的 **readonly** 設定會維持不變。 |
| /guestsallowed:`{true | false}` | 指定以來賓身分登入的使用者是否可以使用該磁片區。 使用 **True** 來指定來賓可以使用該磁片區。 使用 **False** 來指定來賓無法使用該磁片區。 如果在新增磁片區時省略，來賓可以使用磁片區。 如果在變更磁片區時省略，則磁片區的 **guestsallowed** 設定會維持不變。 |
| /password`<password>` | 指定存取磁片區所需的密碼。 如果在新增磁片區時省略，則不會建立任何密碼。 如果在變更磁片區時省略，則密碼會維持不變。 |
| /maxusers:`{<number>> | unlimited}` | 指定可以同時使用磁片區上之檔案的使用者數目上限。 如果在新增磁片區時省略，則不限數量的使用者可以使用該磁片區。 如果在變更磁片區時省略，則 **maxusers** 值會維持不變。                                                 |
| /remove | 當您移除 Macintosh 可存取的磁片區時，這是必要的。 移除指定的磁片區。 |
| /? | 在命令提示字元顯示說明。 |

##### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字 (例如 " `<computer name>` " ) 。

#### <a name="examples"></a>範例

若要在本機伺服器上建立稱為 *美國行銷統計資料* 的磁片區，並使用 E 磁片磁碟機中的 *Stats* 目錄，並指定來賓無法存取該磁片區，請輸入：

```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```

若要將上面建立的磁片區變更為唯讀、需要密碼，以及將最大使用者數目設定為5，請輸入：

```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```

若要新增名為 [*橫向設計*] 的磁片區，請在 [伺服器] * \\ 木蘭*上，使用 E 磁片磁碟機中的*樹狀*目錄，並指定來賓可以存取該磁片區，請輸入：

```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```

若要在本機伺服器上移除稱為 *Sales Reports* 的磁片區，請輸入：

```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
