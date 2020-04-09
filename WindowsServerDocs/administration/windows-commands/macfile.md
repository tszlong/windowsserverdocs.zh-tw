---
title: macfile
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0156be5a3209bf8cedf13b35ceab61ef38a0f49a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840311"
---
# <a name="macfile"></a>macfile

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理適用于 Macintosh 伺服器、磁片區、目錄和檔案的檔案伺服器。 您可以在批次檔中包含一系列命令，並以手動方式或在預先定義的時間啟動它們，來自動化管理工作。 
-   [修改 Macintosh 可存取磁片區中的目錄](#BKMK_Moddirs)
-   [加入 Macintosh 檔案的資料和資源分支](#BKMK_Joinforks)
-   [變更登入訊息和限制會話](#BKMK_LogonLimit)
-   [新增、變更或移除 Macintosh 可存取的磁片區](#BKMK_addvol)

## <a name="to-modify-directories-in-macintosh-accessible-volumes"></a><a name=BKMK_Moddirs></a>修改 Macintosh 可存取磁片區中的目錄

### <a name="syntax"></a>語法
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

#### <a name="parameters"></a>參數
-   /server：\\\\<computerName> 指定要變更目錄的伺服器。 如果省略，則會在本機電腦上執行此操作。
-   /path：<directory> 需要。 指定您要變更之目錄的路徑。 目錄必須存在。 **macfile 目錄**不會建立目錄。
-   /owner：<OwnerName> 變更目錄的擁有者。 如果省略，則擁有者會保持不變。
-   /group：<GroupName> 指定或變更與目錄相關聯的 Macintosh 主要群組。 如果省略，主要群組會保持不變。
-   /permissions：<Permissions> 為擁有者、主要群組和全球（everyone）設定目錄的許可權。 使用11位數的數位來設定許可權。 數位1會授與許可權，0則會撤銷許可權（例如，11111011000）。 如果省略，許可權會維持不變。
    數位的位置會決定要設定的許可權，如下表所述。

    |Position|設定許可權|
    |------|------------|
    |第一個|OwnerSeeFiles|
    |第二個|OwnerSeeFolders|
    |步驟 3|OwnerMakechanges|
    |步驟 4|GroupSeeFiles|
    |步驟 5|GroupSeeFolders|
    |步驟 6|GroupMakechanges|
    |第|WorldSeeFiles|
    |8|WorldSeeFolders|
    |隊員|WorldMakechanges|
    |十分之一|無法重新命名、移動或刪除此目錄。|
    |第|這些變更會套用至目前目錄和所有子目錄。|

-   /?
    在命令提示字元顯示說明。

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字（例如，* * * *<em>電腦名稱稱</em>* * * *）。
- 使用**macfiledirectory** ，將 macintosh 可存取磁片區中的現有目錄提供給 macintosh 使用者。 **Macfiledirectory**命令不會建立目錄。 使用 [檔案管理員]、[命令提示字元] 或 [ **macintosh 新增資料夾**] 命令，在使用**macfile 目錄**命令之前，先在 macintosh 可存取的磁片區中建立目錄。
  ### <a name="examples"></a><a name=BKMK_Examples></a>典型
  下列範例會在本機伺服器的 E 磁片磁碟機上，變更 [Macintosh 可存取磁片區] 統計資料中的子目錄許可權。 此範例會指派 [查看檔案]、[資料夾] 和 [變更擁有者的許可權] 和 [查看檔案]，並查看所有其他使用者的資料夾許可權，同時防止重新命名、移動或刪除目錄。
  ```
  macfile directory /path:e:\statistics\may sales /permissions:11111011000
  ```

## <a name="to-join-a-macintosh-files-data-and-resource-forks"></a><a name=BKMK_Joinforks></a>加入 Macintosh 檔案的資料和資源分支

### <a name="syntax"></a>語法
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

#### <a name="parameters"></a>參數

|         參數          |                                                                                                           描述                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /server：\\\\<computerName> |                                                            指定要聯結檔案的伺服器。 如果省略，則會在本機電腦上執行此操作。                                                            |
|   /creator：<CreatorName>   |                                      指定檔案的建立者。 Macintosh 搜尋工具會使用 **/creator**命令列選項來判斷建立檔案的應用程式。                                       |
|      /type：<typeName>      |                                 指定檔的類型。 Macintosh 搜尋工具會使用 **/type**命令列選項來判斷應用程式內建立檔案的檔案類型。                                 |
|    /datafork：<Filepath>    |                                                                   指定要聯結的資料分叉位置。 您可以指定遠端路徑。                                                                   |
|  /resourcefork：<Filepath>  |                                                                 指定要聯結的資源分支位置。 您可以指定遠端路徑。                                                                 |
|   /targetfile：<Filepath>   | 必要。 指定透過聯結資料分支和資源分支所建立之檔案的位置，或指定您要變更其類型或建立者之檔案的位置。 檔案必須在指定的伺服器上。 |
|             /?             |                                                                                               在命令提示字元顯示說明。                                                                                               |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字（例如，* * * *<em>電腦名稱稱</em>* * * *）。

### <a name="examples"></a>範例
若要在 [Macintosh 可存取磁片區] D:\Release 上建立 treeapp 檔案，請使用 [資源分支 C:\Cross\Mac\Appcode]，並將此新檔案顯示為 Macintosh 用戶端作為應用程式（Macintosh 應用程式會使用類型 APPL.EXE），並將建立者（簽章）設定為木蘭，請輸入：
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
若要將檔案建立者變更為 Microsoft Word 5.1，請針對目錄 D:\Word documents\Group 檔中的檔案單字，在伺服器 \\\SERverA 上，輸入：
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="to-change-the-logon-message-and-limit-sessions"></a><a name=BKMK_LogonLimit></a>變更登入訊息和限制會話
### <a name="syntax"></a>語法
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

#### <a name="parameters"></a>參數

|               參數                |                                                                                                                                                                           描述                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /server：\\\\<computerName>       |                                                                                                                        指定要在其上變更參數的伺服器。 如果省略，則會在本機電腦上執行此操作。                                                                                                                         |
| /maxsessions： {無&#124;限制數位} |                                                                                         指定可以同時使用 Macintosh 檔案和列印伺服器的最大使用者數目。 如果省略，伺服器的**maxsessions**設定會維持不變。                                                                                         |
|        /loginmessage：<Message>         | 變更 Macintosh 使用者登入 Macintosh 伺服器的檔案伺服器時所看到的訊息。 登入訊息的字元數上限為199。 如果省略，伺服器的**loginmessage**訊息會保持不變。 若要移除現有的登入訊息，請包含 **/loginmessage**參數，但將*訊息*變數保留空白。 |
|                   /?                   |                                                                                                                                                               在命令提示字元顯示說明。                                                                                                                                                               |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字（例如，* * * *<em>電腦名稱稱</em>* * * *）。

### <a name="examples"></a>範例
若要將本機伺服器上允許之 Macintosh 會話的檔案和列印伺服器數目，從目前的設定變更為五個會話，並在完成時，從 Server for Macintosh 新增登入訊息記錄檔。請輸入：
```
macfile server /maxsessions:5 /loginmessage:Log off from Server for Macintosh when you are finished.
```

## <a name="to-add-change-or-remove-macintosh-accessible-volumes"></a><a name=BKMK_addvol></a>新增、變更或移除 Macintosh 可存取的磁片區
### <a name="syntax"></a>語法
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

#### <a name="parameters"></a>參數

|              參數               |                                                                                                                                                                       描述                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/add &#124; /set}          |                                                                                                                      當您新增或變更 Macintosh 可存取的磁片區時，這是必要的。 新增或變更指定的磁片區。                                                                                                                       |
|      /server：\\\\<computerName>      |                                                                                                             指定要在其上新增、變更或移除磁片區的伺服器。 如果省略，則會在本機電腦上執行此操作。                                                                                                              |
|          /name：<volumeName>          |                                                                                                                                          必要。 指定要新增、變更或移除的磁片區名稱。                                                                                                                                           |
|          /path：<directory>           |                                                                                                                只有在新增磁片區時才需要和有效。 指定要新增之磁片區根目錄的路徑。                                                                                                                 |
|    /readonly： {true &#124; false}     | 指定使用者是否可以變更磁片區中的檔案。 輸入 true，指定使用者無法變更磁片區中的檔案。 輸入 false 以指定使用者可以變更磁片區中的檔案。 如果在新增磁片區時省略，則允許變更檔案。 如果在變更磁片區時省略，則磁片區的**readonly**設定會保持不變。 |
|  /guestsallowed： {true &#124; false}  |      指定以來賓身分登入的使用者是否可以使用該磁片區。 輸入 true，指定來賓可以使用磁片區。 輸入 false 以指定來賓無法使用磁片區。 如果在新增磁片區時省略，來賓可以使用磁片區。 如果在變更磁片區時省略，磁片區的**guestsallowed**設定會維持不變。       |
|         /password：<Password>         |                                                                               指定存取磁片區時所需的密碼。 新增磁片區時若省略此參數，則不會建立任何密碼。 如果在變更磁片區時省略，密碼會保持不變。                                                                               |
| /maxusers： {<Number>>&#124;無限制} |                                                 指定可以同時使用磁片區上之檔案的最大使用者數目。 如果在新增磁片區時省略，則不限數目的使用者可以使用該磁片區。 如果在變更磁片區時省略， **maxusers**值會維持不變。                                                 |
|               /remove                |                                                                                                                                當您移除 Macintosh 存取磁片區時，這是必要的。 移除指定的磁片區。                                                                                                                                |
|                  /?                  |                                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                                           |

### <a name="remarks"></a>備註
- 如果您提供的資訊包含空格或特殊字元，請使用引號括住文字（例如，* * * *<em>電腦名稱稱</em>* * * *）。

### <a name="examples"></a>範例
若要在本機伺服器上建立名為「美國行銷統計資料」的磁片區，並使用電子磁片磁碟機中的 Stats 目錄，並指定來賓無法存取該磁片區，請輸入：
```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```
若要將以上建立的磁片區變更為唯讀，並將最大使用者數設定為五個，請輸入：
```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```
若要新增名為 [橫向設計] 的磁片區，請在伺服器 \\\Magnolia 上，使用 E 磁片磁碟機中的樹狀目錄，並指定來賓可以存取該磁片區，請輸入：
```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```
若要移除本機伺服器上稱為「銷售報告」的磁片區，請輸入：
```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>其他參考資料
-   - [命令列語法關鍵](command-line-syntax-key.md)
