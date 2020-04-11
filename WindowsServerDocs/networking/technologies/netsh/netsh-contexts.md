---
title: Netsh 命令語法、內容與格式
description: 您可以使用本主題來了解如何輸入 netsh 內容和子內容、了解 netsh 語法和命令格式，以及如何在執行 Windows Server 2016 或 Windows 10 的本機和遠端電腦上執行 netsh 命令。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 061d7252d5a7bbe09d3dca245d9b77ed20a4dedf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854761"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令語法、內容與格式

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來了解如何輸入 netsh 內容和子內容、了解 netsh 語法和命令格式，以及如何在本機和遠端電腦上執行 netsh 命令。

Netsh 是命令列指令碼處理公用程式，讓您顯示或修改目前執行中電腦的網路設定。 Netsh 命令可經由在 netsh 提示字元鍵入命令來執行，而且可以在批次檔或指令碼中使用。 您可以使用 netsh 命令來設定遠端電腦和本機電腦。

Netsh 也提供指令碼處理功能，讓您針對特定電腦以批次模式執行一組命令。 使用 netsh，您可以將設定指令碼儲存在文字檔中做為封存之用，或協助您設定其他電腦。

## <a name="netsh-contexts"></a>Netsh 內容

Netsh 會使用動態連結程式庫 \(DLL\) 檔案來與其他作業系統元件互動。 

每個 netsh 協助程式 DLL 都會提供一組延伸功能，稱之為「內容」  ，這是一組網路伺服器角色或功能專屬的命令。 這些內容會提供一或多個服務、公用程式或通訊協定的設定和監視支援，藉此擴充 netsh 的功能。 例如，Dhcpmon.dll 提供的 netsh 包含設定及管理 DHCP 伺服器所需的內容和命令集。

### <a name="obtain-a-list-of-contexts"></a>取得內容清單

您可在執行 Windows Server 2016 或 Windows 10 的電腦上開啟命令提示字元或 Windows PowerShell，以取得 netsh 內容清單。 鍵入 **netsh**命令，然後按 ENTER 鍵。 鍵入 **/?** ，然後按 ENTER 鍵。

以下是這些命令在執行 Windows Server 2016 Datacenter 的電腦上的輸出範例。

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

### <a name="subcontexts"></a>子內容

Netsh 內容可以包含命令和其他內容，稱之為「子內容」  。 例如，在 Routing 內容中，您可以變更為 IP 和 IPv6 子內容。

若要顯示您可以在內容中使用的命令和子內容清單，請在 netsh 提示字元鍵入內容名稱，然後鍵入 **/?** 或 **help**。 例如，若要顯示您可以在 Routing 內容中使用的子內容和命令清單，請在 netsh 提示字元 \(也就是 **netsh&gt;** \)，鍵入下列其中一項：

**routing /?**

**routing help**

若要在另一個內容中執行工作，而不需從目前的內容變更，請鍵入您要在 netsh 提示字元使用的命令內容路徑。 例如，若要在 IGMP 內容中新增名為「區域連線」的介面，而不需先變更為 IGMP 內容，請在 netsh 提示字元鍵入：

**routing ip igmp add interface "Local Area Connection" startupqueryinterval=21**

## <a name="running-netsh-commands"></a>執行 netsh 命令

若要執行 netsh 命令，您必須在命令提示字元鍵入 **netsh**，然後按 ENTER 鍵來啟動 netsh。 接著，您可以變更為包含所要使用命令的內容。 可用的內容取決於您已安裝的網路元件。 例如，如果您在 netsh 提示字元鍵入 **dhcp**，然後按 ENTER 鍵，則 netsh 會變更為 DHCP 伺服器內容。 不過，如果您尚未安裝 DHCP，則會出現下列訊息：

**找不到下列命令：dhcp。**

## <a name="formatting-legend"></a>格式圖例

當您在 netsh 提示字元或在批次檔或指令碼中執行命令時，可以使用下列格式圖例來解譯和使用正確的 netsh 命令語法。

- *斜體* 的文字是您在鍵入命令時必須提供的資訊。 例如，如果命令具有名為 -*UserName* 的參數，您就必須鍵入實際的使用者名稱。
- **粗體**的文字是您在鍵入命令時，必須完全依照所示鍵入的資訊。
- 後面接著 \(...\)的文字是可以在命令列中重複數次的參數。
- 中括弧 [&nbsp;] 之間的文字是選擇性項目。
- 大括弧 {&nbsp;} 內以直線號分隔選擇項的文字會提供一組選擇，您必須只能從中選取一個選項，例如 `{enable|disable}`。
- 使用 Courier 字型格式化的文字為程式碼或程式輸出。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>從命令提示字元或 Windows PowerShell 執行 Netsh 命令

若要啟動 Network Shell 並在命令提示字元或 Windows PowerShell 中輸入 netsh，您可以使用下列命令。

### <a name="netsh"></a>netsh

Netsh 是命令列指令碼處理公用程式，可讓您在本機或從遠端顯示或修改目前執行中電腦的網路設定。 若未搭配參數使用，**netsh** 會開啟 Netsh.exe 命令提示字元 \(也就是 **netsh&gt;** \)。

#### <a name="syntax"></a>語法

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[ **-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* |  **-f**&nbsp;*ScriptFile*}\]

##### <a name="parameters"></a>參數

**`-a`**

選擇性。 指定您會在執行 *AliasFile* 後回到 **netsh** 提示字元。

**`AliasFile`**

選擇性。 指定包含一或多個 **netsh** 命令的文字檔名稱。

**`-c`**

選擇性。 指定 netsh 輸入指定的 **netsh** 內容。

**`Context`**

選擇性。 指定您要輸入的 **netsh** 內容。 

**`-r`**

選擇性。 指定您想要在遠端電腦上執行命令。

> [!IMPORTANT]
> 當您從遠端在另一部電腦上使用某些 netsh 命令搭配 **netsh –r** 參數時，遠端登錄服務必須在遠端電腦上執行。 如果該服務並未執行，Windows 會顯示「找不到網路路徑」錯誤訊息。

***`RemoteComputer`***

選擇性。 指定您要設定的遠端電腦。

**`-u`**

選擇性。 指定您要在使用者帳戶下執行 netsh 命令。

***`DomainName\\`***

選擇性。 指定使用者帳戶所屬的網域。 如果未指定 *DomainName\\* ，預設值為區域網域。

***`UserName`***

選擇性。 指定使用者帳戶名稱。

**`-p`**

選擇性。 指定您想要提供使用者帳戶的密碼。

***`Password`***

選擇性。 針對您以 **-u** *UserName* 指定的使用者名稱指定密碼。

***`NetshCommand`***

選擇性。 指定您要執行的 **netsh** 命令。

**`-f`**

選擇性。 在執行您以 *ScriptFile* 指定的指令碼之後，結束 **netsh**。

***`ScriptFile`***

選擇性。 指定您想要執行的指令碼。

**`/?`**

選擇性。 在 netsh 命令提示字元顯示 help。

> [!NOTE]
> 如果您指定後面接著另一個命令的 **`-r`** ，則 **netsh** 會在遠端電腦上執行命令，然後回到 Cmd.exe 命令提示字元。 如果您指定不含另一個命令的 **`-r`** ，則 **netsh** 會在遠端模式中開啟。 此程式類似於在 Netsh 命令提示字元使用 **set machine**。 當您使用 **`-r`** 時，您只會針對 **netsh** 的目前執行個體設定目標電腦。 在您結束並重新鍵入 **netsh** 後，目標電腦會重設為本機電腦。 您可藉由指定存放在 WINS 中的電腦名稱、UNC 名稱、DNS 伺服器所解析的網際網路名稱或 IP 位址，在遠端電腦上執行 **netsh** 命令。

**鍵入 netsh 命令的參數字串值**

在 Netsh 命令參考中，有一些命令包含需要字串值的參數。

如果字串值的字元間含有空格 (例如由一個以上單字組成的字串值)，則需要將字串值括在引號中。 例如，對於名為 **interface** 且字串值為 **Wireless Network Connection** 的參數，請使用引號括住字串值：

**`interface="Wireless Network Connection"`**

