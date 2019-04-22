---
title: Netsh 命令語法、內容，以及格式
description: 您可以使用本主題來了解如何輸入 netsh 內容和子內容，了解 netsh 語法和命令格式，以及如何在執行 Windows Server 2016 或 Windows 10 的本機和遠端電腦上執行的 netsh 命令。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb77d841ba4d69b0d36bc7f19d4707638530c97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823689"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令語法、內容，以及格式

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解如何輸入 netsh 內容與子內容，請了解 netsh 語法和命令格式，以及如何在本機和遠端電腦上執行的 netsh 命令。

Netsh 是電腦的命令列指令碼處理公用程式，可讓您顯示或修改目前正在執行的網路組態。 Netsh 命令可以執行在 netsh 提示字元中輸入命令，它們可以用批次檔或指令碼中。 可以使用 netsh 命令設定遠端電腦和本機電腦。

Netsh 也提供指令碼處理功能，讓您針對特定電腦以批次模式執行一組命令。 使用 netsh，您可以將設定指令碼儲存在文字檔中做為封存之用，或協助您設定其他電腦。

## <a name="netsh-contexts"></a>Netsh 內容

Netsh 與其他作業系統元件互動使用動態\-連結程式庫\(DLL\)檔案。 

每個 netsh 協助程式 DLL 提供一組廣泛的功能，稱為*內容*，這是一群特定網路的伺服器角色或功能的命令。 這些內容提供設定及監視一或多個服務、 公用程式或通訊協定的支援擴充 netsh 的功能。 比方說，Dhcpmon.dll 提供 netsh 內容與設定和管理 DHCP 伺服器所需的命令集。

### <a name="obtain-a-list-of-contexts"></a>取得內容的清單

您可以藉由執行 Windows Server 2016 或 Windows 10 的電腦上開啟命令提示字元] 或 [Windows PowerShell 來取得一份 netsh 內容。 輸入命令**netsh**按 ENTER 鍵。 型別 **/？**，然後按 ENTER 鍵。

以下是執行 Windows Server 2016 Datacenter 的電腦上的這些命令的範例輸出。

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>子內容

Netsh 內容可以包含命令和其他內容中，呼叫*子內容*。 比方說，在路由內容中，您可以變更至 IP 和 IPv6 的子內容。

若要顯示一份命令和子內容可供您在內容中，在 netsh 提示字元中，輸入內容名稱，然後再輸入 **/？** 或是**協助**。 例如，若要顯示的子內容以及您可以使用的命令清單在路由內容中，在 netsh 提示\(亦即**netsh&gt;**\)，輸入下列其中之一：

**路由 /？**

**路由的說明**

若要在另一個內容中執行工作，而不需要變更從您目前的內容，輸入您想要在 netsh 提示字元使用命令的內容路徑。 比方說，若要新增介面中 IGMP 內容名為"Local Area Connection"，而不需要第一個變更的 IGMP 內容，在 netsh 提示字元中，輸入：

**路由 ip igmp 新增介面"Local Area Connection"startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>執行 netsh 命令

若要執行的 netsh 命令，您必須啟動 netsh 命令提示字元中輸入**netsh**並按 enter。 接下來，您可以變更至包含您想要使用此的命令的內容。 可用的內容取決於您已安裝的網路元件。 例如，如果您鍵入**dhcp**在 netsh 提示字元按下 ENTER，netsh DHCP 伺服器內容的變更。 如果您沒有安裝的 DHCP，不過，會出現下列訊息：

**找不到下列命令： dhcp。**

## <a name="formatting-legend"></a>格式圖例

您可以使用下列格式的圖例來解譯，並使用正確的 netsh 命令語法，當您執行命令時在 netsh 提示或在批次檔或指令碼。

- 中的文字*斜體*是您輸入命令時，您必須提供的資訊。 例如，如果命令具有名為-的參數*UserName*，您必須輸入實際的使用者名稱。
- 中的文字**粗體**是完全依照顯示輸入命令時，您必須輸入的資訊。
- 文字後面接著省略符號\(...\)是可以在命令列中重複多次的參數。
- 方括號之間的文字 [&nbsp;] 是選擇性的項目。
- 大括號之間的文字 {&nbsp;} 以管線符號分隔的選項提供一組選擇從中您必須選取只有一個，例如`{enable|disable}`。
- 程式碼或程式的輸出以 Courier 字型格式化的文字。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>從命令提示字元或 Windows PowerShell 中執行的 Netsh 命令

若要啟動網路殼層並輸入 netsh 的命令提示字元，或在 Windows PowerShell 中，您可以使用下列命令。

### <a name="netsh"></a>netsh

Netsh 是電腦的命令列指令碼處理公用程式，可讓您在本機或遠端電腦上，顯示或修改目前正在執行的網路組態。 不含參數， **netsh** Netsh.exe 命令提示字元就會開啟\(亦即**netsh&gt;**\)。

#### <a name="syntax"></a>語法

**netsh** \[ **-a**&nbsp;*AliasFile* \] \[ **-c** &nbsp; *內容* \] \[ **-r**&nbsp;*遠端電腦*\] \[ **-u** \[ *DomainName\\*  \] *使用者名稱* \] \[ **-p** &nbsp;*密碼* |  \* \] \[{*NetshCommand* | **-f** &nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>參數

**`-a`**

選擇性。 指定您會回到**netsh**提示字元執行後*AliasFile*。

**`AliasFile`**

選擇性。 指定的文字檔案，其中包含一或多個名稱**netsh**命令。

**`-c`**

選擇性。 指定該 netsh 進入指定**netsh**內容。

**`Context`**

選擇性。 指定**netsh**您想要輸入的內容。 

**`-r`**

選擇性。 指定您想要在遠端電腦上執行的命令。

>[!IMPORTANT]
>當您使用某些的 netsh 命令從遠端使用的另一部電腦上**netsh – r**參數，必須在遠端電腦上執行遠端登錄服務。 如果未執行，Windows 會顯示 「 網路找不到路徑 」 錯誤訊息。

***`RemoteComputer`***

選擇性。 指定您想要設定的遠端電腦。

**`-u`**

選擇性。 指定您想要執行的使用者帳戶下的 netsh 命令。

***`DomainName\\`***

選擇性。 指定的使用者帳戶所在的網域。 預設值是本機網域，如果*DomainName\\* 未指定。

***`UserName`***

選擇性。 指定的使用者帳戶名稱。

**`-p`**

選擇性。 指定您想要提供的使用者帳戶的密碼。

***`Password`***

選擇性。 指定使用您指定的使用者帳戶的密碼 **-u** *UserName*。

***`NetshCommand`***

選擇性。 指定**netsh**您想要執行的命令。

**`-f`**

選擇性。 結束**netsh**執行您指定與指令碼之後*ScriptFile*。

***`ScriptFile`***

選擇性。 指定您想要執行的指令碼。

**`/?`**

選擇性。 在 netsh 提示中顯示說明。

>[!NOTE]
>如果您指定**`-r`** 後面接著另一個命令， **netsh**遠端電腦上執行命令，然後傳回給 Cmd.exe 命令提示字元。 如果您指定**`-r`** 而另一個命令，不**netsh**會在遠端模式中開啟。 此程序，類似於使用**組機器**的 Netsh 命令提示字元。 當您使用**`-r`**，設定目前的執行個體的目標電腦**netsh**只。 在您結束並重新輸入之後**netsh**，目標電腦會重設為 本機電腦。 您可以執行**netsh**藉由指定電腦的遠端電腦上的命令名稱在 WINS 中的預存、 UNC 名稱、 網際網路名稱解析的 DNS 伺服器或 IP 位址。

**輸入 netsh 命令的參數字串值**

在 Netsh 命令參考有包含的參數的字串值是必要的命令。

在其中的字串值包含空格字元，例如多個字組所組成的字串值之間的情況下會需要您在以引號括住的字串值。 例如，對於名為的參數**介面**字串值是**無線網路連線**，使用引號括住的字串值：

**`interface="Wireless Network Connection"`**

