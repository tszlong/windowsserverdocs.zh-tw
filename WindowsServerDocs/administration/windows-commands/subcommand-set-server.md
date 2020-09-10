---
title: 子命令集-伺服器
description: 子命令集伺服器的參考文章，設定 Windows 部署服務伺服器的設定。
ms.topic: reference
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 33ebdc4e411c7ff550b9686459e7440007d2ae63
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635475"
---
# <a name="subcommand-set-server"></a>子命令：設定伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Authorize： {Yes &#124; No}]|指定是否在 (DHCP) 的動態主機控制通訊協定中授權這部伺服器。|
|[/RogueDetection： {Yes &#124; No}]|啟用或停用 DHCP rogue 偵測。|
|[/AnswerClients： {All &#124; 已知 &#124; 無}]|指定這部伺服器將回應的用戶端。 如果您將此值設定為 [ **已知**]，必須在 Active Directory 網域服務中預先設置電腦 (AD DS) ，才能由 Windows 部署服務伺服器回答。|
|[/Responsedelay： <time in seconds> ]|在接聽開機用戶端之前，伺服器將等待的時間量。 此設定不適用於預先設置的電腦。|
|[/AllowN12forNewClients： {Yes &#124; No}]|若為 Windows Server 2008，指定不明用戶端不需要按 F12 鍵，即可起始網路開機。 已知的用戶端將會收到為電腦指定的開機程式，如果未指定，則會針對架構指定開機程式。<p>若為 Windows Server 2008 R2，此選項已取代為下列命令： wdsutil/Set-Server/PxepromptPolicy/New： Noprompt|
|[/ArchitectureDiscovery： {Yes &#124; No}]|啟用或停用架構探索。 這有助於探索未正確廣播其架構的 x64 型用戶端。|
|[/resetBootProgram： {Yes &#124; No}]|決定是否要清除剛剛開機之用戶端的開機路徑，而不需要按 F12 鍵。|
|[/DefaultX86X64Imagetype： {x86 &#124; x64 &#124; 兩者}]|控制哪些開機映射會顯示到 x64 型用戶端。|
|[/UseDhcpPorts： {Yes &#124; No}]|指定 PXE 伺服器是否應嘗試系結至 DHCP 埠（TCP 通訊埠67）。 如果 DHCP 和 Windows 部署服務是在同一部電腦上執行，您應該將此選項設為 [ **否** ]，讓 DHCP 伺服器利用該埠，並將 **/DhcpOption60** 參數設定為 **[是]**。 此值的預設設定為 **[是]**。|
|[/DhcpOption60： {Yes &#124; No}]|指定是否應針對 PXE 支援設定 DHCP 選項60。 如果 DHCP 和 Windows 部署服務是在相同的伺服器上執行，請將此選項設定為 **[是]** ，並將 **/UseDhcpPorts** 選項設定為 [ **否**]。 此值的預設設定為 [ **否**]。|
|[/RpcPort： <Port number> ]|指定用來服務用戶端要求的 TCP 通訊埠編號。|
|[/PxepromptPolicy]|設定已知 (預先設置) 和新用戶端如何起始 PXE 開機。 此選項只適用于 Windows Server 2008 R2。 您可以使用下列選項來設定設定：<p>-[/Known： {OptIn&#124;輸出&#124;Noprompt}]-為預先設置的用戶端設定原則。<br />-[/New： {OptIn&#124;輸出&#124;Noprompt}]-為新的用戶端設定原則。<p>**OptIn** 表示用戶端必須按下金鑰才能進行 PXE 開機，否則會切換回下一個開機裝置。<p>**Noprompt** 表示用戶端一律會進行 PXE 開機。<p>**輸出** 表示用戶端將 PXE 開機，除非按下 Esc 鍵。|
|[/BootProgram： <Relative path> ]/Architecture： {x86 &#124; ia64 &#124; x64}|指定 [remoteInstall] 資料夾中開機程式的相對路徑 (例如 **boot\x86\pxeboot.n12**) ，然後指定開機程式的架構。|
|[/N12BootProgram： <Relative path> ]/Architecture： {x86 &#124; ia64 &#124; x64}|指定開機程式的相對路徑，而不需要按 F12 鍵 (例如 **boot\x86\pxeboot.n12**) ，以及指定開機程式的架構。|
|[/BootImage： <Relative path> ]/Architecture： {x86 &#124; ia64 &#124; x64}|指定開機映射的相對路徑，以供開機用戶端接收，並指定開機映射的架構。 您可以為每個架構指定此項。|
|[/PreferredDC： <DC Name> ]|指定 Windows 部署服務應使用之網域控制站的名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/PreferredGC： <GC Name> ]|指定 Windows 部署服務應使用之通用類別目錄伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/PrestageUsingMAC： {Yes &#124; No}]|指定 Windows 部署服務在 AD DS 中建立電腦帳戶時，是否應使用 MAC 位址，而不是 GUID/UUID 來識別電腦。|
|[/NewMachineNamingPolicy： <Policy> ]|指定為用戶端產生電腦名稱稱時要使用的格式。 如需用於之格式的相關資訊 <policy> ，請在 mmc 嵌入式管理單元中的伺服器  **上按一下**滑鼠右鍵，按一下 [內容]，然後查看 [ **目錄服務** ] 索引標籤。例如， **/NewMachineNamingPolicy：% 61Username% #**。|
|[/NewMachineOU]|用來指定 AD DS 中將建立用戶端電腦帳戶的位置。 您可以使用下列選項來指定位置。<p>-[/type： Serverdomain &#124; Userdomain &#124; UserOU &#124; Custom] 指定位置的類型。 **Serverdomain** 會在與 Windows 部署服務伺服器相同的網域中建立帳戶。 **Userdomain** 會在執行安裝的使用者所在的相同網域中建立帳戶。 **UserOU** 會在執行安裝的使用者組織單位中建立帳戶。 **自訂** 可讓您指定自訂位置 (您也必須使用這個選項) 來指定 **/ou** 的值。<br />-[/OU： <Domain name of OU> ]-如果您針對 **/type**選項指定**Custom** ，此選項會指定應在其中建立電腦帳戶的組織單位。|
|[/DomainSearchOrder： {GCOnly &#124; DCFirst}]|指定在 AD DS (通用類別目錄或網域控制站) 中搜尋電腦帳戶的原則。|
|[/NewMachineDomainJoin： {Yes &#124; No}]|指定 AD DS 中尚未預先設置的電腦是否應該在安裝期間加入網域。 預設值為 [是]。|
|/WdsClientLogging|指定伺服器的記錄層級。<p>-[/Enabled： {Yes &#124; No}]-啟用或停用 Windows 部署服務用戶端動作的記錄。<br />-[/LoggingLevel： {None &#124; 錯誤 &#124; 警告 &#124; 資訊}-設定記錄層級。 **None** 相當於停用記錄。 **錯誤** 是最低的記錄層級，表示只會記錄錯誤。 **警告** 包括警告和錯誤。 **資訊** 是最高的記錄層級，並且包含錯誤、警告和資訊事件。|
|/WdsUnattend|這些設定會控制 Windows 部署服務用戶端的自動安裝行為。 您可以使用下列選項來設定設定：<p>-[/Policy： {Enabled &#124; Disabled}]-指定是否要使用自動安裝。<br />-[/CommandlinePrecedence： {Yes &#124; No}]-指定是否要在用戶端上使用 Autounattend.xml 檔案 (（如果用戶端) 或直接傳遞至 Windows 部署服務用戶端的自動安裝檔案），而不是在用戶端安裝期間使用的映射自動安裝檔案。 預設設定為 [ **否**]。<br />-[/File： <Relative path> /Architecture： {x86 &#124; ia64 &#124; x64}]-指定自動安裝檔案的檔案名、路徑和架構。|
|/AutoaddPolicy|這些設定會控制自動新增原則。 您可以使用下列選項來定義設定：<p>-[/Policy： {AdminApproval &#124; Disabled}]- **AdminApprove** 會將所有未知電腦新增到擱置佇列中，系統管理員可以在其中查看電腦清單，並視需要核准或拒絕每個要求。 **Disabled** 指出當未知電腦嘗試開機至伺服器時，不會採取其他動作。<br />-[/PollInterval： {time in seconds}]-指定網路開機程式應該輪詢 Windows 部署服務伺服器的間隔 (秒) 。<br />-[/MaxRetry： <Number> ]-指定網路開機程式應該輪詢 Windows 部署服務伺服器的次數。 此值連同 **/PollInterval**，規定網路開機程式等候系統管理員核准或拒絕電腦的時間長度，然後才會發生。例如， **MaxRetry** 值10和 **PollInterval** vlue 60 表示用戶端應該輪詢伺服器10次，並在嘗試之間等候60秒。 因此，用戶端會在10分鐘後超時 (10 x 60 秒 = 10 分鐘的) 。<br />-[/Message： <Message> ]-指定在網路開機程式對話方塊頁面上顯示給用戶端的訊息。<br />-[/RetentionPeriod]-指定電腦在自動清除之前可以處於擱置狀態的天數。<br />-[/Approved： <time in days> ]-指定已核准電腦的保留期限。 您必須使用此參數搭配 **/RetentionPeriod** 選項。<br />-[/Others： <time in days> ]-指定未核准的電腦 (拒絕或擱置) 的保留期限。 您必須使用此參數搭配 **/RetentionPeriod** 選項。|
|[/AutoaddSettings]|指定要套用至每部電腦的預設設定。 您可以使用下列選項來定義設定：<p>-/Architecture： {x86 &#124; ia64 &#124; x64}-指定架構。<br />-[/BootProgram： <Relative path> ]-指定傳送給已核准電腦的開機程式。 如果未指定任何開機程式，則會使用) 伺服器上指定之電腦架構 (的預設值。<br />-[/WdsClientUnattend： <Relative path> ]-設定核准用戶端應接收的自動安裝檔案的相對路徑。<br />-[/ReferralServer： <Server name> ]-指定用戶端將用來下載映射的 Windows 部署服務伺服器。<br />-[/BootImage： <Relative path> ]-指定已核准的用戶端將接收的開機映射。<br />-[/User： <Domain\User &#124; User@Domain>]-設定電腦帳戶物件的許可權，以授與指定的使用者將電腦加入網域的必要許可權。<br />-[JoinRights： {JoinOnly &#124; Full}]-指定要指派給使用者的許可權類型。 **JoinOnly** 需要系統管理員先重設電腦帳戶，使用者才能將電腦加入網域。 **Full** 提供使用者的完整存取權，包括將電腦加入網域的許可權。<br />-[/JoinDomain： {Yes &#124; No}]-指定是否要在 Windows 部署服務安裝期間，將電腦加入網域作為此電腦帳戶。 預設值為 [是]。|
|/BindPolicy|設定 PXE 提供者要接聽的網路介面。 您可以使用下列選項來定義原則：<p>-[/Policy： {Include &#124; Exclude}]-設定介面系結原則，以包含或排除介面清單上的位址。<br />-[/add]-將介面新增至清單。 您也必須指定/addresstype 和/address。<br />-[/remove]-從清單中移除介面。 您也必須指定/addresstype 和/address。<br />-/address： <IP or MAC address> -指定要新增或移除之介面的 IP 或 MAC 位址。<br />-/addresstype： {IP &#124; MAC}-指出 **/address** 選項中指定的網址類別型。|
|[/RefreshPeriod： <seconds> ]|指定伺服器重新整理其設定的頻率 (（秒) ）。|
|[/BannedGuidPolicy]|使用下列選項管理禁用 Guid 的清單：<p>-[/add]/Guid： <GUID> 將指定的 guid 加入至禁用的 guid 清單。 具有此 GUID 的任何用戶端都會改以其 MAC 位址識別。<br />-[/remove]/Guid： <GUID> -從禁用 guid 清單中移除指定的 guid。|
|/BcdRefreshPolicy|使用下列選項設定重新整理 Bcd 檔案的設定：<p>-[/Enabled： {Yes &#124; No}]-指定 Bcd 重新整理原則。 當 **/Enabled** 設定為 **[是]** 時，會在指定的時間間隔重新整理 Bcd 檔。<br />-[/RefreshPeriod： <time in minutes> ]-指定重新整理 Bcd 檔案的時間間隔。|
|/傳輸|設定下列選項：<p><ul><li>[/ObtainIpv4From： {Dhcp &#124; 範圍}]-指定 IPv4 位址的來源。<p><ul><li>[/start： <starting Ipv4 address> ]-指定 IP 位址範圍的開頭。 只有當 **/ObtainIpv4From**設定為**Range**時，才需要此選項且有效</li><li>[/End： <Ending Ipv4 address> ]-指定 IP 位址範圍的結尾。 只有當 **/ObtainIpv4From** 設定為 [ **範圍**] 時，這個選項才是必要的，而且是有效的。</li></ul></li><li>[/ObtainIpv6From：範圍][/start： <start IP address> ][/End： <End IP address> ] 指定 IPv6 位址的來源。 此選項只適用于 Windows Server 2008 R2，且唯一支援的值是 Range。</li><li>[/startPort： <starting port> ]-指定埠範圍的開頭。</li><li>[/EndPort： <Ending port> ]-指定埠範圍的結尾。</li><li>[/Profile： {10Mbps &#124; 100Mbps &#124; 1Gbps &#124; 自訂}]-指定要使用的網路設定檔。 只有執行 Windows Server 2008 的 forservers 才支援此選項。</li><li>[/MulticastSessionPolicy] 設定多播傳輸的傳輸設定。 此命令僅適用于 Windows Server 2008 R2。<p><ul><li>[/Policy： {None &#124; AutoDisconnect &#124; Multistream}]-決定如何處理緩慢的用戶端。 「無」表示將所有用戶端保持在一個會話的相同速度。 AutoDisconnect 表示低於指定/Threshold 的任何用戶端將會中斷連線。 Multistream 表示用戶端將依/StreamCount. 的指定分成多個會話</li><li>[/Threshold： <Speed in KBps> ]-對於/Policy： AutoDisconnect，此選項會設定最小傳輸速率（KBps）。 低於此費率的用戶端將會與多播傳輸中斷連線。</li><li>[/StreamCount： {2 &#124; 3}][/Fallback： {Yes &#124; No}]-針對/Policy： Multistream，這個選項會決定會話的數目。 2表示兩個會話 (fast 和慢) 3 表示三個會話 (慢、中、快速) 。</li><li>[/Fallback： {Yes&#124; No}]-判斷已中斷連線的用戶端是否會使用另一個方法來繼續傳輸 (如果用戶端) 支援的話）。 如果您使用 WDS 用戶端，電腦將會回復為單播。 Wdsmcast.exe 不支援 fallback 機制。 此選項也適用于不支援 Multistream 的用戶端。 在這種情況下，電腦會切換回另一種方法，而不是移至速度較慢的傳輸會話。</li></ul></li></ul>|
## <a name="examples"></a>範例
若要將伺服器設定為只回應已知的用戶端（回應延遲為4分鐘），請輸入：
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
若要設定開機前執行環境 (PXE) 伺服器嘗試系結至 TCP 埠67和60，請輸入：
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 enable-Server 命令](using-the-enable-server-command.md) 
[使用 get-Server 命令](using-the-get-server-command.md) 
[使用 Initialize-Server 命令](using-the-initialize-server-command.md) 
[子命令：啟動-伺服器](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md) 
解除[初始化伺服器選項](the-uninitialize-server-option.md)
