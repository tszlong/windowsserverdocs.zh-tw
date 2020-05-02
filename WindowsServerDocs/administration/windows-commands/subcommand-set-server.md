---
title: 子命令集-伺服器
description: 子命令集-伺服器的參考主題，其已設定 Windows 部署服務伺服器的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db21415e5545cd8a501360ae6afde6d30f879dc0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721687"
---
# <a name="subcommand-set-server"></a>子命令： set-Server

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定 Windows 部署服務伺服器的設定。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Authorize： {Yes &#124; No}]|指定是否要在動態主機控制通訊協定（DHCP）中授權此伺服器。|
|[/RogueDetection： {Yes &#124; No}]|啟用或停用 DHCP rogue 偵測。|
|[/AnswerClients： {All &#124; 已知 &#124; None}]|指定這部伺服器將回答的用戶端。 如果您將此值設定為 [**已知**]，則必須在 Active Directory 網域服務（AD DS）中預先設置電腦，才會由 Windows 部署服務伺服器進行回應。|
|[/Responsedelay：<time in seconds>]|伺服器在接聽開機用戶端之前會等待的時間量。 此設定不適用於預先設置的電腦。|
|[/AllowN12forNewClients： {Yes &#124; No}]|若為 Windows Server 2008，指定不明的用戶端不需要按 F12 鍵，就能起始網路開機。 已知的用戶端會收到針對該電腦指定的開機程式，如果未指定，則會針對架構指定開機程式。<p>針對 Windows Server 2008 R2，已使用下列命令取代此選項： wdsutil/Set-Server/PxepromptPolicy/New： Noprompt|
|[/ArchitectureDiscovery： {Yes &#124; No}]|啟用或停用架構探索。 這有助於探索不會正確廣播其架構的 x64 型用戶端。|
|[/resetBootProgram： {Yes &#124; No}]|判斷開機路徑是否會針對剛啟動的用戶端清除，而不需要按 F12 鍵。|
|[/DefaultX86X64Imagetype： {x86 &#124; x64 &#124; 兩個}]|控制要對 x64 型用戶端顯示哪些開機映射。|
|[/UseDhcpPorts： {Yes &#124; No}]|指定 PXE 伺服器是否應該嘗試系結至 DHCP 埠（TCP 埠67）。 如果 DHCP 和 Windows 部署服務在同一部電腦上執行，您應該將此選項設定為 [**否**]，讓 DHCP 伺服器使用該埠，並將 **/DhcpOption60**參數設定為 **[是]**。 此值的預設設定為 **[是]**。|
|[/DhcpOption60： {Yes &#124; No}]|指定是否應該為 PXE 支援設定 DHCP 選項60。 如果 DHCP 和 Windows 部署服務在同一部伺服器上執行，請將此選項設定為 **[是]** ，並將 **/UseDhcpPorts**選項設為 [**否**]。 此值的預設設定為 [**否**]。|
|[/RpcPort：<Port number>]|指定要用來服務用戶端要求的 TCP 通訊埠編號。|
|[/PxepromptPolicy]|設定已知（預先設置）和新的用戶端如何起始 PXE 開機。 此選項僅適用于 Windows Server 2008 R2。 您可以使用下列選項來設定設定：<p>-[/Known： {OptIn&#124;輸出&#124;Noprompt}]-設定預先設置之用戶端的原則。<br />-[/New： {OptIn&#124;輸出&#124;Noprompt}]-設定新用戶端的原則。<p>**OptIn**表示用戶端必須按下按鍵才能進行 PXE 開機，否則會回復到下一個開機裝置。<p>**Noprompt**表示用戶端一律會進行 PXE 開機。<p>**輸出**表示用戶端將會進行 PXE 開機，除非按下 Esc 鍵。|
|[/BootProgram：<Relative path>]/Architecture： {x86 &#124; ia64 &#124; x64}|指定 remoteInstall 資料夾中開機程式的相對路徑（例如， **boot\x86\pxeboot.n12**），並指定開機程式的架構。|
|[/N12BootProgram：<Relative path>]/Architecture： {x86 &#124; ia64 &#124; x64}|指定不需要按 F12 鍵的開機程式的相對路徑（例如， **boot\x86\pxeboot.n12**），並指定開機程式的架構。|
|[/BootImage：<Relative path>]/Architecture： {x86 &#124; ia64 &#124; x64}|指定開機映射的相對路徑，以啟動用戶端應接收，並指定開機映射的架構。 您可以為每個架構指定此項。|
|[/PreferredDC：<DC Name>]|指定 Windows 部署服務應該使用的網域控制站名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/PreferredGC：<GC Name>]|指定 Windows 部署服務應該使用之通用類別目錄伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/PrestageUsingMAC： {Yes &#124; No}]|指定 Windows 部署服務，在 AD DS 中建立電腦帳戶時，是否應使用 MAC 位址，而不是 GUID/UUID 來識別電腦。|
|[/NewMachineNamingPolicy：<Policy>]|指定為用戶端產生電腦名稱稱時所要使用的格式。 如需所要使用之格式的<policy>相關資訊，請在 mmc 嵌入式管理單元中的伺服器**上按一下滑鼠右鍵，按一下 [** 內容]，然後查看 [**目錄服務**] 索引標籤。例如， **/NewMachineNamingPolicy：% 61Username% #**。|
|[/NewMachineOU]|用來指定要在 AD DS 中建立用戶端電腦帳戶的位置。 您可以使用下列選項來指定位置。<p>-[/type： Serverdomain &#124; Userdomain &#124; UserOU &#124; Custom] 指定位置的類型。 **Serverdomain**會在 Windows 部署服務伺服器所在的相同網域中建立帳戶。 **Userdomain**會在執行安裝的使用者所在的相同網域中建立帳戶。 **UserOU**會在執行安裝之使用者的組織單位中建立帳戶。 [**自訂**] 可讓您指定自訂位置（您也必須使用此選項指定 **/ou**的值）。<br />-[/OU：<Domain name of OU>]-如果您針對 **/Type**選項指定**Custom** ，此選項會指定要在其中建立電腦帳戶的組織單位。|
|[/DomainSearchOrder： {GCOnly &#124; DCFirst}]|指定在 AD DS （通用類別目錄或網域控制站）中搜尋電腦帳戶的原則。|
|[/NewMachineDomainJoin： {Yes &#124; No}]|指定在安裝期間，是否應將尚未預先設置在 AD DS 中的電腦加入網域。 預設設定為 **[是]**。|
|/WdsClientLogging|指定伺服器的記錄層級。<p>-[/Enabled： {Yes &#124; No}]-啟用或停用 Windows 部署服務用戶端動作的記錄。<br />-[/LoggingLevel： {None &#124; Errors &#124; 警告 &#124; Info}-設定記錄層級。 **None**相當於停用記錄。 **錯誤**是最低的記錄層級，表示只會記錄錯誤。 **警告**同時包含警告和錯誤。 **Info**是最高層級的記錄，並包含錯誤、警告和資訊事件。|
|/WdsUnattend|這些設定會控制 Windows 部署服務用戶端的自動安裝行為。 您可以使用下列選項來設定設定：<p>-[/Policy： {Enabled &#124; Disabled}]-指定是否要使用自動安裝。<br />-[/CommandlinePrecedence： {Yes &#124; No}]-指定在用戶端安裝期間，將會使用以/Unattend 選項直接傳遞至 Windows 部署服務用戶端的 Autounattend.xml .xml 檔案或自動安裝檔案，而不是映射自動安裝檔案。 預設設定為 [**否**]。<br />-[/File：<Relative path> /Architecture： {x86 &#124; ia64 &#124; x64}]-指定自動安裝檔案的檔案名、路徑和架構。|
|[/AutoaddPolicy]|這些設定會控制自動新增原則。 您可以使用下列選項來定義設定：<p>-[/Policy： {AdminApproval &#124; Disabled}]- **AdminApprove**會將所有未知的電腦新增至擱置佇列，讓系統管理員可以在其中檢查電腦清單，並視需要核准或拒絕每個要求。 [**已停用**] 表示當未知電腦嘗試開機至伺服器時，不會採取任何其他動作。<br />-[/PollInterval： {time （秒）}]-指定網路開機程式應輪詢 Windows 部署服務伺服器的間隔時間（以秒為單位）。<br />-[/MaxRetry： <Number>]-指定網路開機程式應輪詢 Windows 部署服務伺服器的次數。 此值和 **/PollInterval**會指示網路開機程式在超時之前，會等待系統管理員核准或拒絕電腦的時間長度。例如， **MaxRetry**值為10，而**PollInterval** vlue 為60時，表示用戶端應輪詢伺服器10次，等待嘗試之間的60秒。 因此，用戶端會在10分鐘後完成時間（10 x 60 秒 = 10 分鐘）。<br />-[/Message： <Message>]-指定在 [網路開機程式] 對話方塊頁面上顯示給用戶端的訊息。<br />-[/RetentionPeriod]-指定電腦在自動清除之前可以處於擱置狀態的天數。<br />-[/Approved： <time in days>]-指定已核准電腦的保留期限。 您必須使用此參數搭配 **/RetentionPeriod**選項。<br />-[/Others： <time in days>]-指定未核准電腦的保留期限（已拒絕或擱置）。 您必須使用此參數搭配 **/RetentionPeriod**選項。|
|[/AutoaddSettings]|指定要套用至每部電腦的預設設定。 您可以使用下列選項來定義設定：<p>-/Architecture： {x86 &#124; ia64 &#124; x64}-指定架構。<br />-[/BootProgram： <Relative path>]-指定要傳送至已核准電腦的開機程式。 如果未指定開機程式，則會使用電腦架構的預設值（如伺服器上所指定）。<br />-[/WdsClientUnattend： <Relative path>]-設定核准的用戶端應接收的自動安裝檔案的相對路徑。<br />-[/ReferralServer： <Server name>]-指定用戶端將用來下載映射的 Windows 部署服務伺服器。<br />-[/BootImage： <Relative path>]-指定核准的用戶端將接收的開機映射。<br />-[/User： <Domain\User &#124; User@Domain>]-設定電腦帳戶物件的許可權，以授與指定的使用者將電腦加入網域所需的許可權。<br />-[JoinRights： {JoinOnly &#124; Full}]-指定要指派給使用者的許可權類型。 **JoinOnly**需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。 **Full**會提供使用者的完整存取權，包括將電腦加入網域的許可權。<br />-[/JoinDomain： {Yes &#124; No}]-指定在 Windows 部署服務安裝期間，是否應將電腦加入網域做為此電腦帳戶。 預設設定為 **[是]**。|
|/BindPolicy|設定 PXE 提供者接聽的網路介面。 您可以使用下列選項來定義原則：<p>-[/Policy： {Include &#124; Exclude}]-設定介面系結原則，以包含或排除介面清單上的位址。<br />-[/add]-將介面新增至清單。 您也必須指定/addresstype 和/address。<br />-[/remove]-從清單中移除介面。 您也必須指定/addresstype 和/address。<br />-/address：<IP or MAC address> -指定要新增或移除之介面的 IP 或 MAC 位址。<br />-/addresstype： {IP &#124; MAC}-表示 **/address**選項中指定的網址類別型。|
|[/RefreshPeriod： <seconds>]|指定伺服器將重新整理其設定的頻率（以秒為單位）。|
|[/BannedGuidPolicy]|使用下列選項管理禁止的 Guid 清單：<p>-[/add]/Guid：<GUID> -將指定的 guid 新增至禁用的 guid 清單。 具有此 GUID 的任何用戶端將會以其 MAC 位址來識別。<br />-[/remove]/Guid：<GUID> -從禁止的 guid 清單中移除指定的 guid。|
|[/BcdRefreshPolicy]|使用下列選項設定重新整理 Bcd 檔案的設定：<p>-[/Enabled： {Yes &#124; No}]-指定 Bcd 重新整理原則。 當 **/Enabled**設定為 **[是]** 時，會在指定的時間間隔重新整理 Bcd 檔案。<br />-[/RefreshPeriod：<time in minutes>]-指定重新整理 Bcd 檔案的時間間隔。|
|/傳輸|設定下列選項：<p><ul><li>[/ObtainIpv4From： {Dhcp &#124; 範圍}]-指定 IPv4 位址的來源。<p><ul><li>[/start： <starting Ipv4 address>]-指定 IP 位址範圍的開頭。 此選項是必要的，只有在 **/ObtainIpv4From**設定為**範圍**時才有效</li><li>[/End： <Ending Ipv4 address>]-指定 IP 位址範圍的結尾。 此選項是必要的，只有在 **/ObtainIpv4From**設定為**Range**時才有效。</li></ul></li><li>[/ObtainIpv6From： Range][/start：<start IP address>] [/End：<End IP address>] 指定 IPv6 位址的來源。 此選項僅適用于 Windows Server 2008 R2，而且唯一支援的值為 [範圍]。</li><li>[/startPort： <starting port>]-指定埠範圍的開頭。</li><li>[/EndPort： <Ending port>]-指定埠範圍的結尾。</li><li>[/Profile： {10Mbps &#124; 100Mbps &#124; 1Gbps &#124; Custom}]-指定要使用的網路設定檔。 只有執行 Windows Server 2008 的 forservers 支援此選項。</li><li>[/MulticastSessionPolicy] 設定多播傳輸的傳輸設定。 此命令僅適用于 Windows Server 2008 R2。<p><ul><li>[/Policy： {None &#124; AutoDisconnect &#124; Multistream}]-決定如何處理緩慢的用戶端。 None 表示將所有用戶端保持在同一個會話的速度。 AutoDisconnect 表示放置於指定/Threshold 下方的任何用戶端將會中斷連線。 Multistream 表示用戶端將會分成多個會話，如/StreamCount. 所指定</li><li>[/Threshold：<Speed in KBps>]-對於/Policy： AutoDisconnect，這個選項會設定以 KBps 為單位的最小傳輸速率。 低於此速率的用戶端將會與多播傳輸中斷連線。</li><li>[/StreamCount： {2 &#124; 3}][/Fallback： {Yes &#124; No}]-針對/Policy： Multistream，此選項會決定會話的數目。 2表示兩個會話（快速且緩慢）3表示三個會話（緩慢、中、快速）。</li><li>[/Fallback： {Yes&#124; No}]-判斷中斷連線的用戶端是否會使用另一種方法繼續傳輸（如果用戶端支援的話）。 如果您使用 WDS 用戶端，電腦會回到單播。 Wdsmcast.exe 不支援 fallback 機制。 此選項也適用于不支援 Multistream 的用戶端。 在這種情況下，電腦會切換回另一種方法，而不是移至較慢的傳輸會話。</li></ul></li></ul>|
## <a name="examples"></a>範例
若要將伺服器設定為只回答已知的用戶端，回應延遲為4分鐘，請輸入：
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
若要設定伺服器的開機程式和架構，請輸入：
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
若要在伺服器上啟用記錄，請輸入：
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
若要在伺服器上啟用自動安裝，以及架構和用戶端自動安裝檔案，請輸入：
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
若要將開機前執行環境（PXE）伺服器設定為嘗試系結至 TCP 埠67和60，請輸入：
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md)使用
[disable](using-the-disable-server-command.md) 
 [ ](subcommand-stop-server.md) 
 
 [ ](using-the-get-server-command.md) 
 [ ](using-the-initialize-server-command.md) 
 [ ](subcommand-start-server.md) [ ](using-the-enable-server-command.md) [ ](the-uninitialize-server-option.md) -server 命令的命令列語法索引鍵使用 [啟用-伺服器] 命令使用 [啟動-伺服器命令] 子命令： [起始-伺服器] 子命令： [停止初始化-伺服器] 選項

