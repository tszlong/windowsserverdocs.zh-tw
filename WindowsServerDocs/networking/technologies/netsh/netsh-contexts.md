---
title: Netsh 命令語法、內容，以及格式
description: 若要了解如何輸入 netsh 內容和子，了解 netsh 語法和命令格式，以及如何執行 netsh 命令本機和遠端電腦是執行 Windows Server 2016 或 Windows 10 上，您可以使用此主題。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令語法、內容，以及格式

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要了解如何輸入 netsh 內容和子，了解 netsh 語法和命令格式，以及如何執行 netsh 命令本機和遠端電腦上，您可以使用此主題。

Netsh 是電腦的一個命令列指令碼的公用程式，可讓您顯示或修改網路設定目前執行的是電腦的。 Netsh 命令可以 netsh 命令提示字元中，輸入命令執行，並在「批次檔案或指令碼所使用。 使用 netsh 命令可以設定遠端電腦與本機電腦。

Netsh 指令碼，可讓您在「批次模式之電腦執行的命令。 使用 netsh，您可以在文字檔案保存用途，或為協助您設定的其他電腦中儲存設定指令碼。

## <a name="netsh-contexts"></a>Netsh 內容

Netsh 與其他作業系統元件使用 dynamic\ 連結庫 \(DLL\) 檔案。 

每個 netsh 授與協助者 DLL 提供一組擴充功能稱為*操作*，這是一組命令特定網路伺服器角色或功能。 這些內容擴充功能的 netsh 提供設定及監視一或多個服務、公用程式或通訊協定的支援。 例如，Dhcpmon.dll 提供 netsh 操作與設定的設定及管理 DHCP 伺服器所需的命令。

### <a name="obtain-a-list-of-contexts"></a>取得內容清單

您可以取得 netsh 內容清單打開 Windows PowerShell 或命令提示字元中執行 Windows Server 2016 或 Windows 10 電腦上。 輸入命令**netsh**按下 ENTER。 輸入**日嗎？**，然後按 ENTER 鍵。

以下是這些命令執行 Windows Server 2016 Datacenter 的電腦上的範例輸出。

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


### <a name="subcontexts"></a>子

Netsh 內容可以包含命令和其他內容，稱為*子*。 例如中路由，您可以變更 IP 和 IPv6 子。

顯示命令清單，您可以使用 netsh 命令提示字元中，於操作，在子輸入操作名稱，然後鍵入任一個**日嗎？** 或**協助**。 例如，若要路由操作，在 netsh 提示中顯示的子，您可以使用的命令清單 \ (也就是**netsh&gt;**\)，輸入下列其中一個動作：

**路由日嗎？**

**尚協助**

另一個操作中執行工作，而無須更動您目前操作從，輸入您想要使用 netsh 命令提示字元中的命令操作路徑。 例如，若要新增介面「本機區域連接」中 IGMP 操作而不會先變更 netsh 命令提示字元中，於 IGMP 操作，以輸入：

**路由 ip igmp 新增介面「本機區域連接」startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Netsh 命令的執行

若要執行 netsh 命令，必須開始 netsh 命令提示字元中輸入**netsh**，然後按 ENTER 鍵。 接下來，您可以變更操作包含您想要使用的命令。 您已安裝的網路元件可內容而定。 例如，如果您輸入**dhcp**在 netsh 命令提示字元中，按下 ENTER netsh 變更 DHCP 伺服器操作。 如果您不需要安裝 DHCP，不過，會顯示以下訊息：

**找不到下列命令：dhcp。**

## <a name="formatting-legend"></a>格式設定的圖例

您可以使用下列格式圖例上尚未取得共識，並使用正確 netsh 命令語法 netsh 命令提示字元中，或在「批次檔案或指令碼執行的命令。

- 中的文字*斜體*是您輸入命令時，您必須提供的資訊。 例如，如果命令有一個名為-參數*的使用者名稱*，您必須輸入實際的使用者名稱。
- 中的文字**粗體**會完全如您輸入命令時，您必須輸入的資訊。
- 文字後面省略符號 \(...\) 是參數，可在命令列重複數次。
- 文字括弧之間的 [&nbsp;] 是選擇性的項目。
- 大括弧之間的文字 {&nbsp;} 的選項，以管道提供一組選擇，您必須選取只有一個，例如`{enable|disable}`。
- Courier 字型格式化的文字是代碼或程式輸出。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>從 Windows PowerShell 或命令提示字元中執行 Netsh 命令

[開始] 的網路殼層，並輸入 netsh Windows PowerShell 或命令提示字元中，您可以使用下列命令。

### <a name="netsh"></a>netsh

Netsh 是一個命令列指令碼的公用程式，可讓您在本機或遠端電腦上，顯示器或修改電腦目前執行的網路設定。 通常，若不使用**netsh**開啟 Netsh.exe 命令提示字元 \ (也就是**netsh&gt;**\)。

#### <a name="syntax"></a>語法

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>參數

**`-a`**

選用。 指定您會回到**netsh**之後執行提示*AliasFile*。

**`AliasFile`**

選用。 指定名稱包含下列一或多個文字檔案的**netsh**的命令。

**`-c`**

選用。 指定指定該 netsh，輸入**netsh**操作。

**`Context`**

選用。 指定**netsh**，輸入您想要的操作。 

**`-r`**

選用。 指定您想要在遠端電腦上執行的命令。

>[!IMPORTANT]
>當您使用一些 netsh 命令從遠端使用另一部電腦上**netsh – r**參數，必須在遠端電腦上執行遠端登錄服務。 如果這不執行，Windows 就會顯示「網路路徑找不到「的錯誤訊息。

***`RemoteComputer`***

選用。 指定您想要設定遠端電腦。

**`-u`**

選用。 指定您要在帳號 netsh 命令的執行。

***`DomainName\\`***

選用。 指定帳號所在位置的網域。 預設值是本機的網域如果*DomainName\\*未指定。

***`UserName`***

選用。 指定的使用者 account 名稱。

**`-p`**

選用。 指定您要帳號提供的密碼。

***`Password`***

選用。 指定您與指定的使用者 account 密碼**-u***的使用者名稱*。

***`NetshCommand`***

選用。 指定**netsh**您想要執行的命令。

**`-f`**

選用。 結束**netsh**之後您與指定的指令碼執行的*指令碼檔案*。

***`ScriptFile`***

選用。 指定您要執行的指令碼。

**`/?`**

選用。 顯示協助在 netsh 提示。

>[!NOTE]
>若您指定**`-r`**後面另一個命令，**netsh**遠端電腦上執行的命令，然後返回 Cmd.exe 命令提示字元。 若您指定**`-r`**另一個命令，而**netsh**開啟遠端模式。 此程序很類似，使用**設定電腦**在 Netsh 命令提示字元。 當您使用**`-r`**，您可以設定目標電腦目前執行個體的**netsh**只。 在您結束，並重新輸入後**netsh**的目標電腦重設為 [本機電腦。 您可以在執行**netsh**命令所指定電腦遠端電腦上的儲存 wins UNC 名稱、網際網路名稱解析 DNS 伺服器，或 IP 位址的名稱。

**輸入 netsh 命令的參數字串值**

整個 Netsh 命令參考有命令包含字串值是必要的參數。

在如此字串值，其中包含字元，例如，包含數個文字，字串值之間的空間很需要引號住字串值。 例如參數名為**介面**字串值為**無線網路連接**，使用引號字串值：

**`interface="Wireless Network Connection"`**

