---
title: Netsh 命令語法、內容，以及格式
description: 您可以使用本主題來瞭解如何輸入 netsh 內容和子上下文、瞭解 netsh 語法和命令格式，以及如何在執行 Windows Server 2016 或 Windows 10 的本機和遠端電腦上執行 netsh 命令。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405580"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令語法、內容，以及格式

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何輸入 netsh 內容和子上下文、瞭解 netsh 語法和命令格式，以及如何在本機和遠端電腦上執行 netsh 命令。

Netsh 是命令列腳本公用程式，可讓您顯示或修改目前正在執行之電腦的網路設定。 Netsh 命令可以在 netsh 提示字元中輸入命令來執行，而且可以在批次檔或腳本中使用。 您可以使用 netsh 命令來設定遠端電腦和本機電腦。

Netsh 也提供指令碼處理功能，讓您針對特定電腦以批次模式執行一組命令。 使用 netsh，您可以將設定指令碼儲存在文字檔中做為封存之用，或協助您設定其他電腦。

## <a name="netsh-contexts"></a>Netsh 內容

Netsh 會使用動態\-程式庫 \(DLL\) 檔案，與其他作業系統元件互動。 

每個 netsh helper DLL 會提供一組豐富的功能，稱為*內容*，這是一組網路伺服器角色或功能專屬的命令。 這些內容會藉由提供一或多個服務、公用程式或通訊協定的設定和監視支援，來擴充 netsh 的功能。 例如，Dhcpmon 會提供 netsh，其中包含設定和管理 DHCP 伺服器所需的內容和命令集。

### <a name="obtain-a-list-of-contexts"></a>取得內容清單

您可以在執行 Windows Server 2016 或 Windows 10 的電腦上開啟 [命令提示字元] 或 [Windows PowerShell]，以取得 netsh 內容清單。 輸入**netsh**命令，然後按 enter。 輸入 **/？** ，然後按 enter。

以下是執行 Windows Server 2016 Datacenter 之電腦上這些命令的範例輸出。

>    ```
>   PS C:\Windows\system32> netsh
>   netsh>/?
>    
>    The following commands are available:
>    
>    Commands in this context:
>    ..            - Goes up one context level.
>    ?             - Displays a list of commands.
>    abort         - Discards changes made while in offline mode.
>    add           - Adds a configuration entry to a list of entries.
>    advfirewall   - Changes to the `netsh advfirewall' context.
>    alias         - Adds an alias.
>    branchcache   - Changes to the `netsh branchcache' context.
>    bridge        - Changes to the `netsh bridge' context.
>    bye           - Exits the program.
>    commit        - Commits changes made while in offline mode.
>    delete        - Deletes a configuration entry from a list of entries.
>    dhcpclient    - Changes to the `netsh dhcpclient' context.
>    dnsclient     - Changes to the `netsh dnsclient' context.
>    dump          - Displays a configuration script.
>    exec          - Runs a script file.
>    exit          - Exits the program.
>    firewall      - Changes to the `netsh firewall' context.
>    help          - Displays a list of commands.
>    http          - Changes to the `netsh http' context.
>    interface     - Changes to the `netsh interface' context.
>    ipsec         - Changes to the `netsh ipsec' context.
>    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
>    lan           - Changes to the `netsh lan' context.
>    namespace     - Changes to the `netsh namespace' context.
>    netio         - Changes to the `netsh netio' context.
>    offline       - Sets the current mode to offline.
>    online        - Sets the current mode to online.
>    popd          - Pops a context from the stack.
>    pushd         - Pushes current context on stack.
>    quit          - Exits the program.
>    ras           - Changes to the `netsh ras' context.
>    rpc           - Changes to the `netsh rpc' context.
>    set           - Updates configuration settings.
>    show          - Displays information.
>    trace         - Changes to the `netsh trace' context.
>    unalias       - Deletes an alias.
>    wfp           - Changes to the `netsh wfp' context.
>    winhttp       - Changes to the `netsh winhttp' context.
>    winsock       - Changes to the `netsh winsock' context.
>    
>    The following sub-contexts are available:
>     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
>    
>    To view help for a command, type the command, followed by a space, and then type ?.
>    ```

### <a name="subcontexts"></a>子

Netsh 內容可以包含命令和其他內容，稱為子*上下文*。 例如，在路由內容中，您可以變更為 IP 和 IPv6 子上下文。

若要顯示您可以在內容中使用的命令和子上下文清單，請在 netsh 提示字元中輸入內容名稱，然後輸入 **/？** 或 **[** 說明]。 例如，若要顯示您可以在路由內容中使用的子類別和命令清單，請在 netsh 提示字元 \(也就是**netsh&gt;** \)，輸入下列其中一項：

**路由/？**

**路由說明**

若要執行另一個內容中的工作，而不需要從目前的內容變更，請輸入您要在 netsh 提示字元中使用之命令的內容路徑。 例如，若要在 IGMP 內容中新增名為「本機區域連線」的介面，而不先變更為 IGMP 內容，請在 netsh 提示字元中輸入：

**路由 ip igmp 新增介面「區域連線」 startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>執行 netsh 命令

若要執行 netsh 命令，您必須在命令提示字元中輸入**netsh** ，然後按 enter 鍵，以啟動 netsh。 接下來，您可以將變更為包含您要使用之命令的內容。 可用的內容取決於您已安裝的網路元件。 例如，如果您在 netsh 命令提示字元中輸入**dhcp** ，然後按 enter，則 netsh 會變更為 dhcp 伺服器內容。 不過，如果您沒有安裝 DHCP，則會出現下列訊息：

**找不到下列命令： dhcp。**

## <a name="formatting-legend"></a>格式化圖例

當您在 netsh 提示字元或在批次檔或腳本中執行命令時，可以使用下列格式化圖例來解讀和使用正確的 netsh 命令語法。

- [*斜體*] 中的文字是您在輸入命令時必須提供的資訊。 例如，如果命令具有名為-*UserName*的參數，您就必須輸入實際的使用者名稱。
- 以**粗體**顯示的文字是您輸入命令時，必須完全依照所示輸入的資訊。
- 文字後面接著省略號 \(...\) 是可以在命令列中重複多次的參數。
- 在括弧 [&nbsp;] 之間的文字是選擇性的專案。
- 在大括弧 {&nbsp;} 之間的文字與以管道分隔的選擇，會提供一組您必須只選取一個選項，例如 `{enable|disable}`。
- 使用中字型格式化的文字為 [程式碼] 或 [程式輸出]。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>從命令提示字元或 Windows PowerShell 執行 Netsh 命令

若要啟動 Network Shell 並在命令提示字元或 Windows PowerShell 中輸入 netsh，您可以使用下列命令。

### <a name="netsh"></a>netsh

Netsh 是命令列腳本公用程式，可讓您在本機或遠端顯示或修改目前執行中電腦的網路設定。 使用不含參數的情況下， **netsh**會開啟 netsh 命令提示字元 \(也就是**netsh&gt;** \)。

#### <a name="syntax"></a>語法

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*內容*\] \[ **-r**&nbsp;*遠端電腦*\] \[ **-u** \[ *DomainName\\* \]*使用者名稱*\] \[ **-p**&nbsp;*密碼* | \*\] \[{*NetshCommand* |  **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parameters

**`-a`**

選用。 指定在執行*AliasFile*之後，您會回到**netsh**提示字元。

**`AliasFile`**

選用。 指定包含一或多個**netsh**命令之文字檔的名稱。

**`-c`**

選用。 指定 netsh 進入指定的**netsh**內容。

**`Context`**

選用。 指定您想要輸入的**netsh**內容。 

**`-r`**

選用。 指定您想要在遠端電腦上執行命令。

> [!IMPORTANT]
> 當您在具有**netsh – r**參數的另一部電腦上遠端使用一些 netsh 命令時，遠端登入服務必須在遠端電腦上執行。 如果未執行，Windows 會顯示「找不到網路路徑」錯誤訊息。

***`RemoteComputer`***

選用。 指定您要設定的遠端電腦。

**`-u`**

選用。 指定您想要在使用者帳戶下執行 netsh 命令。

***`DomainName\\`***

選用。 指定使用者帳戶所在的網域。 如果未指定*DomainName\\* ，預設值為本機網域。

***`UserName`***

選用。 指定使用者帳戶名稱。

**`-p`**

選用。 指定您想要提供使用者帳戶的密碼。

***`Password`***

選用。 指定以 **-u** *UserName*指定之使用者帳戶的密碼。

***`NetshCommand`***

選用。 指定您想要執行的**netsh**命令。

**`-f`**

選用。 執行您以*ScriptFile*指定的腳本後，結束**netsh** 。

***`ScriptFile`***

選用。 指定您想要執行的腳本。

**`/?`**

選用。 在 netsh 提示字元中顯示說明。

> [!NOTE]
> 如果您指定 **`-r`** 後面接著另一個命令， **netsh**會在遠端電腦上執行命令，然後返回 cmd.exe 命令提示字元。 如果您指定 **`-r`** 而沒有另一個命令， **netsh**會在遠端模式中開啟。 此程式類似于在 Netsh 命令提示字元中使用**set machine** 。 當您使用 **`-r`** 時，只會為目前的**netsh**實例設定目的電腦。 在您結束並重新輸入**netsh**之後，目的電腦會重設為本機電腦。 您可以在遠端電腦上執行**netsh**命令，方法是指定存放在 WINS 中的電腦名稱稱、UNC 名稱、DNS 伺服器要解析的網際網路名稱，或 IP 位址。

**輸入 netsh 命令的參數字串值**

在 Netsh 命令參考中，有一些命令包含需要字串值的參數。

如果字串值包含字元之間的空格（例如由一個以上的單字組成的字串值），則需要將字串值括在引號中。 例如，針對名稱為**interface**且字串值為**無線網路**連線的參數，請使用引號括住字串值：

**`interface="Wireless Network Connection"`**

