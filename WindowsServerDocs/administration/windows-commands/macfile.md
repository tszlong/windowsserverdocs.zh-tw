---
title: macfile
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7646ada96cb02ae434d4ba846da7a9c4dca51b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437469"
---
# <a name="macfile"></a>macfile

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

管理檔案伺服器的 Macintosh 伺服器、 磁碟區、 目錄和檔案。 您可以自動化管理工作，包括批次檔中的一系列的命令，然後手動啟動它們，或在預定的時間。 
-   [若要修改 Macintosh 可存取的磁碟區中的目錄](#BKMK_Moddirs)
-   [加入 Macintosh 檔案的資料與資源的分支](#BKMK_Joinforks)
-   [若要變更登入訊息，並限制工作階段](#BKMK_LogonLimit)
-   [若要新增、 變更或移除 Macintosh 可存取的磁碟區](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>若要修改 Macintosh 可存取的磁碟區中的目錄

### <a name="syntax"></a>語法
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>參數
-   /server:\\\\<computerName>指定要變更的目錄伺服器。 如果省略，會為作業在本機電腦上。
-   /path:<directory>必要。 指定您想要變更目錄的路徑。 目錄必須存在。 **macfile 目錄**不會建立目錄。
-   /owner:<OwnerName>變更目錄的擁有者。 如果省略，擁有者就會維持不變。
-   /group:<GroupName>會指定或變更與目錄相關聯的 Macintosh 主要群組。 如果省略，則主要群組會保持不變。
-   /permissions:<Permissions>設定擁有者、 主要群組，以及世界 (everyone) 的目錄的權限。 11 位數的數字用來設定權限。 數字 1 會授與權限，0 則會撤銷權限 (例如 11111011000)。 如果省略，則權限會維持不變。
    數字的位置會決定哪些權限設定時下, 表中所述。

    |位置|設定權限|
    |------|------------|
    |第一個|OwnerSeeFiles|
    |第二個|OwnerSeeFolders|
    |第三個|OwnerMakechanges|
    |第四個|GroupSeeFiles|
    |第五個|GroupSeeFolders|
    |第六個|GroupMakechanges|
    |第七個|WorldSeeFiles|
    |第八個|WorldSeeFolders|
    |第九個|WorldMakechanges|
    |第十個|無法重新命名、 移動或刪除目錄。|
    |第十一個|變更套用至目前的目錄和所有子目錄。|

-   /?
    在命令提示字元顯示說明。

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住的文字 (例如 **」** <em>電腦名稱</em> **"** )。
- 使用**macfiledirectory**讓 Macintosh 使用者可用的 Macintosh 可存取的磁碟區中現有的目錄。 **Macfiledirectory**命令不會建立目錄。 使用檔案管理員 中，命令提示字元中，或**macintosh 新資料夾**Macintosh 可存取磁碟區中建立目錄，才能使用命令**macfile 目錄**命令。
  ### <a name="BKMK_Examples"></a>範例
  下列範例會變更本機伺服器的 E 磁碟機上的 Macintosh 可存取磁碟區統計資料中的子目錄月銷售額的權限。 此範例將看到檔案，請參閱資料夾和進行變更的權限同時防止目錄被重新命名、 移動或刪除指派給擁有者及所有其他使用者，請參閱檔案和查看資料夾權限。
  ```
  macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
  ```

## <a name="BKMK_Joinforks"></a>加入 Macintosh 檔案的資料與資源的分支

### <a name="syntax"></a>語法
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>參數

|         參數          |                                                                                                           描述                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /server:\\\\<computerName> |                                                            指定要加入的檔案伺服器。 如果省略，會為作業在本機電腦上。                                                            |
|   /creator:<CreatorName>   |                                      指定檔案的建立者。 使用 Macintosh finder **/creator**命令列選項，以決定建立檔案的應用程式。                                       |
|      / 類型：<typeName>      |                                 指定檔案的類型。 使用 Macintosh finder **/type**命令列選項，以決定建立檔案的應用程式內的檔案類型。                                 |
|    /datafork:<Filepath>    |                                                                   指定要加入資料分岔的位置。 您可以指定遠端路徑。                                                                   |
|  /resourcefork:<Filepath>  |                                                                 指定要加入資源分岔的位置。 您可以指定遠端路徑。                                                                 |
|   /targetfile:<Filepath>   | 必要。 指定所建立的聯結資料分叉和資源分叉，檔案的位置，或指定檔案的位置，其類型或您要變更建立者。 檔案必須是指定的伺服器上。 |
|             /?             |                                                                                               在命令提示字元顯示說明。                                                                                               |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住的文字 (例如 **」** <em>電腦名稱</em> **"** )。

### <a name="examples"></a>範例
若要 D:\Release，使用資源分支 C:\Cross\Mac\Appcode，在 Macintosh 可存取磁碟區上建立檔案 treeapp 並讓這個新的檔案似乎 Macintosh 用戶端，做為應用程式 （Macintosh 應用程式會使用應用程式的型別） 與建立者 （簽章) 設定為木蘭、 型別：
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
若要變更檔案建立者為 Microsoft Word 5.1，WOrd.txt 中目錄 D:\Word documents\Group 檔案伺服器上的檔案\\\SERverA 中，輸入：
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>若要變更登入訊息，並限制工作階段
### <a name="syntax"></a>語法
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>參數

|               參數                |                                                                                                                                                                           描述                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /server:\\\\<computerName>       |                                                                                                                        指定的伺服器上變更參數。 如果省略，會為作業在本機電腦上。                                                                                                                         |
| /maxsessions: {數目&#124;無限} |                                                                                         指定使用者可同時使用檔案及列印伺服器的 Macintosh 的數目上限。 如果省略， **maxsessions**設定伺服器會維持不變。                                                                                         |
|        /loginmessage:<Message>         | 變更登入適用於 Macintosh server 檔案伺服器時，請參閱 < 訊息 Macintosh 使用者。 登入訊息的字元數目上限是 199。 如果省略， **loginmessage**郵件，為伺服器會維持不變。 若要移除現有的登入訊息，包括 **/loginmessage**參數，但保留*訊息*空白的變數。 |
|                   /?                   |                                                                                                                                                               在命令提示字元顯示說明。                                                                                                                                                               |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住的文字 (例如 **」** <em>電腦名稱</em> **"** )。

### <a name="examples"></a>範例
若要變更的檔案數目和列印伺服器，允許使用的 Macintosh 工作階段在本機伺服器上從目前的設定五個工作階段，並將新增登入訊息 「 登出 Server for Macintosh 當您完成時。 」 中，輸入：
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>若要新增、 變更或移除 Macintosh 可存取的磁碟區
### <a name="syntax"></a>語法
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>參數

|              參數               |                                                                                                                                                                       描述                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/add &#124; /set}          |                                                                                                                      當您新增或變更的 Macintosh 可存取的磁碟區時，會需要它。 加入或變更指定的磁碟區。                                                                                                                       |
|      /server:\\\\<computerName>      |                                                                                                             指定要新增、 變更或移除磁碟區的伺服器。 如果省略，會為作業在本機電腦上。                                                                                                              |
|          /name:<volumeName>          |                                                                                                                                          必要。 指定要加入、 變更或移除的磁碟區名稱。                                                                                                                                           |
|          /path:<directory>           |                                                                                                                必要和您要新增磁碟區時才有效。 指定要加入的磁碟區根目錄的路徑。                                                                                                                 |
|    /readonly:{true &#124; false}     | 指定使用者是否可以變更磁碟區中的檔案。 輸入 true 會指定使用者無法變更磁碟區中的檔案。 輸入 [false] 以指定使用者可以變更磁碟區中的檔案。 如果省略新增磁碟區時，，允許對檔案進行變更。 如果時變更磁碟區，省略**readonly**設定磁碟區會保持不變。 |
|  /guestsallowed: {true &#124; ，則為 false}  |      指定登入為來賓使用者是否可以使用磁碟區。 輸入 true 會指定來賓可以使用的磁碟區。 輸入 [false] 以指定來賓無法使用的磁碟區。 如果省略新增磁碟區時，客體可使用的磁碟區。 如果時變更磁碟區，省略**guestsallowed**設定磁碟區會保持不變。       |
|         /password:<Password>         |                                                                               指定的密碼，才能存取磁碟區。 如果省略新增磁碟區時，會建立沒有密碼。 如果省略變更磁碟區時，密碼會維持不變。                                                                               |
| /maxusers:{<Number>>&#124;unlimited} |                                                 指定使用者都能同時使用磁碟區上的檔案的數目上限。 如果在新增磁碟區時，便會省略它，無限的數量的使用者可以使用磁碟區。 如果時變更磁碟區，省略**maxusers**值會維持不變。                                                 |
|               /remove                |                                                                                                                                當您要移除的 Macintosh 存取磁碟區時，會需要它。 移除指定的磁碟區。                                                                                                                                |
|                  /?                  |                                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                                           |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住的文字 (例如 **」** <em>電腦名稱</em> **"** )。

### <a name="examples"></a>範例
若要建立磁碟區在本機伺服器上，使用的統計資料目錄中的 E 磁碟機中，名為美國行銷統計資料，並以指定來賓無法存取磁碟區，輸入：
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
若要變更磁碟區建立上述處於唯讀模式需要密碼，以及設定五個類型的最大使用者數目：
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
若要新增稱為架構設計，在伺服器上的磁碟區\\\Magnolia，使用樹狀目錄中的 E 磁碟機，並指定來賓，型別可以存取磁碟區：
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
若要移除磁碟區，稱為本機伺服器上的銷售報表，請輸入：
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
