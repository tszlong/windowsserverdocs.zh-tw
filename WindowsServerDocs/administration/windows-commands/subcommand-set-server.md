---
title: 子命令設定伺服器
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abc3fe23558f077e0ba9ac69f2641e3b8c9cde4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858379"
---
# <a name="subcommand-set-server"></a>子命令： 設定伺服器

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|[授權 /: {[是] &#124; No}]|指定是否授權此伺服器中動態主機控制通訊協定 (DHCP)。|
|[/RogueDetection:{Yes &#124; No}]|啟用或停用 DHCP rogue 偵測。|
|[/ AnswerClients: {所有&#124;已知&#124;None}]|指定此伺服器將會回答哪些用戶端。 如果您將此值設定為**已知**，必須在 active directory 網域服務 (AD DS) 中預先設置的電腦將會回應由 Windows 部署服務伺服器之前。|
|[/ Responsedelay:<time in seconds>]|伺服器會接聽開機的用戶端之前先等候的時間量。 此設定不適用於預先設置的電腦。|
|[/AllowN12forNewClients:{Yes &#124; No}]|對於 Windows Server 2008，請指定未知的用戶端將不需要按 F12 鍵以啟動網路開機。 已知的用戶端會收到指定電腦的開機程式，或如果未指定，為指定的開機程式架構。<br /><br />Windows Server 2008 r2，此選項已被取代下列命令： r2:wdsutil /Set-Server /PxepromptPolicy /New:Noprompt|
|[/ ArchitectureDiscovery: {[是] &#124; No}]|啟用或停用架構探索。 這有助於進行探索的不正確地廣播其架構的 x64 型用戶端。|
|[/resetBootProgram:{Yes &#124; No}]|判斷開機路徑只是開機而不需要按 F12 按鍵的用戶端，是否將被清除。|
|[/ DefaultX86X64Imagetype: {x86 &#124; x64&#124;兩者}]|開機映像的控制項將會顯示 x64 型用戶端。|
|[/UseDhcpPorts:{Yes &#124; No}]|指定是否在 PXE 伺服器應該嘗試繫結 DHCP 連接埠，TCP 連接埠 67。 如果在同一部電腦上執行 DHCP 和 Windows 部署服務，您應該將此選項設定為**否**若要啟用使用連接埠，並設定 DHCP 伺服器 **/DhcpOption60**參數**是**。 此值的預設設定**是**。|
|[/DhcpOption60:{Yes &#124; No}]|指定的 PXE 支援，是否應該設定 DHCP 選項 60。 如果 DHCP 和 Windows 部署服務相同的伺服器上執行，此選項設定為 **[是]** 並設定 **/UseDhcpPorts**選項設定為**No**。 此值的預設設定**No**。|
|[/RpcPort:<Port number>]|指定用來服務用戶端要求的 TCP 連接埠號碼。|
|[/PxepromptPolicy]|設定如何已知 （預先設置） 和新的用戶端會起始 PXE 開機。 此選項僅適用於 Windows Server 2008 R2。 您將使用下列選項的設定：<br /><br />-[/ 已知: {OptIn&#124;輸出&#124;Noprompt}]-針對預先設置的用戶端設定的原則。<br />-[/ [新增: {OptIn&#124;輸出&#124;Noprompt}]-新的用戶端設定的原則。<br /><br />**OptIn**表示用戶端必須按任一鍵以進行 PXE 開機順序，否則它會切換回下一個開機裝置。<br /><br />**Noprompt**表示用戶端一律會 PXE 開機。<br /><br />**OptOut**表示用戶端將 PXE 開機，除非按下 Esc 鍵。|
|[/ BootProgram:<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|指定 remoteInstall 資料夾的相對路徑的開機程式 (例如**boot\x86\pxeboot.n12**)，並指定開機程式的架構。|
|[/ N12BootProgram:<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|指定相對路徑，不需要按 F12 鍵的開機程式 (例如**boot\x86\pxeboot.n12**)，並指定開機程式的架構。|
|[/ BootImage:<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|指定開機映像，開機用戶端應接收，並指定開機映像架構的相對路徑。 您可以將它指定為每個架構。|
|[/PreferredDC:<DC Name>]|指定應該使用 Windows 部署服務的網域控制站的名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/PreferredGC:<GC Name>]|指定應該使用 Windows 部署服務的通用類別目錄伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。|
|[/ PrestageUsingMAC: {[是] &#124; No}]|指定 Windows 部署服務，在 AD DS 建立電腦帳戶時是否應使用的 MAC 位址，而不是 GUID/UUID 來識別電腦。|
|[/NewMachineNamingPolicy:<Policy>]|指定要產生用戶端的電腦名稱時所使用的格式。 如需有關要使用的格式資訊<policy>，以滑鼠右鍵按一下 mmc 嵌入式管理單元中的伺服器，再按**屬性**，並檢視**目錄服務** 索引標籤。例如， **/NewMachineNamingPolicy: %61username%#**。|
|[/NewMachineOU]|用戶端電腦帳戶將會建立用來在 AD DS 中指定的位置。 您指定的位置，使用下列選項。<br /><br />-[類型：Serverdomain &#124; Userdomain &#124; UserOU&#124;自訂] 指定之位置的類型。 **Serverdomain**會建立在 Windows 部署服務伺服器相同網域中的帳戶。 **Userdomain**會建立在執行安裝的使用者相同的網域中的帳戶。 **UserOU**執行安裝的使用者組織單位中建立帳戶。 **自訂**可讓您指定自訂的位置 (您也必須指定的值 **/OU**使用此選項)。<br />-[/ OU:<Domain name of OU>]-如果您指定**自訂**如 **/type**選項時，這個選項會指定應該在其中建立電腦帳戶的組織單位。|
|[/DomainSearchOrder:{GCOnly &#124; DCFirst}]|指定搜尋 AD DS （通用類別目錄 」 或 「 網域控制站） 中的電腦帳戶的原則。|
|[/NewMachineDomainJoin:{Yes &#124; No}]|指定尚未已預先設置在 AD DS 中的電腦應該已加入之網域在安裝期間。 預設值是**是**。|
|[/WdsClientLogging]|指定伺服器的記錄層級。<br /><br />-[/Enabled: {[是] &#124; No}]-啟用或停用的 Windows 部署服務用戶端動作的記錄。<br />-[/ LoggingLevel: {無&#124;錯誤&#124;警告&#124;資訊}-設定記錄層級。 **無**就相當於停用記錄。 **錯誤**是記錄的最低層級，並指出僅將錯誤將會記錄。 **警告**包含警告和錯誤。 **資訊**是最高層級的記錄，包含錯誤、 警告和資訊事件。|
|[/WdsUnattend]|這些設定會控制 Windows 部署服務用戶端的自動的安裝行為。 您將使用下列選項的設定：<br /><br />-[/ 原則: {已啟用&#124;已停用}]-指定是否要使用自動的安裝。<br />-[/ CommandlinePrecedence: {[是] &#124; No}]-指定是否可使用 Autounattend.xml 檔案 （如果有在用戶端上） 或已直接傳遞至 /Unattend 選項的 Windows 部署服務用戶端自動的安裝檔案而不是用戶端安裝期間將映像自動安裝檔案。 預設值是**No**。<br />-[/File:<Relative path> /Architecture: {x86 &#124; ia64 &#124; x64}]-指定檔案名稱、 路徑和架構的自動安裝檔案。|
|[/AutoaddPolicy]|這些設定可控制自動新增原則。 您定義使用下列選項的設定：<br /><br />-[/ 原則: {AdminApproval&#124;已停用}]- **AdminApprove**會導致要加入至擱置佇列，其中系統管理員可以接著檢閱電腦清單以及核准或拒絕每個要求，為所有未知的電腦適當的。 **已停用**表示未知的電腦嘗試開機到伺服器時，就會採取任何額外的動作。<br />-[/ PollInterval: {time 以秒為單位}]-指定的網路開機程式應該輪詢的 Windows 部署服務伺服器的間隔 （以秒為單位）。<br />-[/ MaxRetry: <Number>]-指定的網路開機程式應該輪詢的 Windows 部署服務伺服器的次數。 此值，以及 **/PollInterval**，規定網路開機程式將等候的時間來核准或拒絕逾時之前的電腦系統管理員。例如， **MaxRetry**值為 10， **PollInterval** vlue 60 的表示，用戶端應輪詢伺服器 10 次，等候 60 秒嘗試之間。 因此，用戶端會在 10 分鐘 （10 x 60 秒數 = 10 分鐘） 後，會逾時。<br />-[/ 訊息： <Message>]-指定會顯示網路開機程式] 對話方塊頁面上的用戶端的訊息。<br />-[/RetentionPeriod]-指定的電腦可以是處於擱置狀態，自動在清除之前的天數。<br />-[/ 核准︰ <time in days>]-指定已核准的電腦的保留期限。 您必須使用此參數與 **/RetentionPeriod**選項。<br />-[/ 其他人： <time in days>]-指定未核准的電腦的保留期限 （拒絕或擱置中）。 您必須使用此參數與 **/RetentionPeriod**選項。|
|[/AutoaddSettings]|指定要套用至每一部電腦的預設設定。 您定義使用下列選項的設定：<br /><br />-/Architecture: {x86 &#124; ia64 &#124; x64}-指定架構。<br />-[/ BootProgram: <Relative path>]-指定在傳送至已核准電腦的開機程式。 如果未不指定任何的開機程式，則會使用 （如 在伺服器上所指定） 的電腦架構的預設值。<br />-[/ WdsClientUnattend: <Relative path>]-設定應該會收到核准的用戶端自動安裝檔案的相對路徑。<br />-[/ ReferralServer: <Server name>]-指定用戶端將用來下載映像的 Windows 部署服務伺服器。<br />-[/ BootImage: <Relative path>]-指定已核准的用戶端將收到的開機映像。<br />-[/ 使用者： < 網域 \ 使用者&#124; User@Domain>]-若要將電腦加入網域的必要權限授與指定的使用者的電腦帳戶物件上設定權限。<br />-[JoinRights: {JoinOnly&#124;完整}]-指定要指派給使用者的權限類型。 **JoinOnly**需要系統管理員使用者可以將電腦加入網域之前，重設電腦帳戶。 **完整**可完整存取權提供給使用者，包括將電腦加入網域的權限。<br />-[/ JoinDomain: {[是] &#124; No}]-指定是否電腦應加入網域成為這個電腦帳戶在 Windows 部署服務安裝期間。 預設值是**是**。|
|[/BindPolicy]|設定要接聽的 PXE 提供者的網路介面。 您定義的原則使用下列選項：<br /><br />-[/ 原則: {Include&#124;排除}]-設定要包含或排除的介面清單上的位址的介面繫結原則。<br />-[/ 新增]-將介面加入至清單。 您也必須指定 /addresstype 和 /address。<br />-[/remove]-從清單中移除介面。 您也必須指定 /addresstype 和 /address。<br />-/ 位址：<IP or MAC address> -指定要新增或移除介面的 IP 或 MAC 位址。<br />-/addresstype: {IP &#124; MAC}-指出位址中指定的型別 **/位址**選項。|
|[/ RefreshPeriod: <seconds>]|指定頻率 （以秒為單位），伺服器會重新整理設定。|
|[/BannedGuidPolicy]|管理遭到禁用的 Guid，使用下列選項的清單：<br /><br />-[/ 新增] /Guid:<GUID> -遭到禁用的 Guid 清單中加入指定的 GUID。 任何用戶端具有此 GUID 將改為識別其 MAC 位址。<br />-[/remove] /Guid:<GUID> -從遭到禁用的 Guid 的清單中移除指定的 GUID。|
|[/BcdRefreshPolicy]|設定重新整理 Bcd 檔案使用下列選項：<br /><br />-[/Enabled: {[是] &#124; No}]-指定 Bcd 重新整理原則。 當 **/enabled**設為**是**，Bcd 檔案會在指定的時間間隔重新整理。<br />-[/ RefreshPeriod:<time in minutes>]-指定在哪個 Bcd 檔案會重新整理的時間間隔。|
|[/Transport]|設定下列選項：<br /><br /><ul><li>[/ ObtainIpv4From: {Dhcp&#124;範圍}]-指定 IPv4 位址的來源。<br /><br /><ul><li>[] / [開始： <starting Ipv4 address>]-指定的 IP 位址範圍的開頭。 這是必要選項，有效才 **/ObtainIpv4From**設定為**範圍**</li><li>[結束： <Ending Ipv4 address>]-指定的 IP 位址範圍的結尾。 這是必要選項，有效才 **/ObtainIpv4From**設為**範圍**。</li></ul></li><li>[/ ObtainIpv6From:Range][] / [開始：<start IP address>] [/ 結束：<End IP address>] 指定 IPv6 位址的來源。 此選項僅適用於 Windows Server 2008 R2，唯一支援的值範圍。</li><li>[/ startPort: <starting port>]-指定的連接埠範圍的開頭。</li><li>[/ EndPort: <Ending port>]-指定的連接埠範圍的結尾。</li><li>[/ 設定檔: {10Mbps &#124; 100Mbps &#124; 1Gbps&#124;自訂}]-指定要使用的網路設定檔。 只支援的 forservers 執行 Windows Server 2008 時，此選項。</li><li>[/MulticastSessionPolicy] 設定多點傳送傳輸的傳輸設定。 此命令只適用於 Windows Server 2008 R2。<br /><br /><ul><li>[/ 原則: {無&#124;自動中斷連線&#124;多資料流}]-決定如何處理緩慢的用戶端。 [無] 表示要保留在一個工作階段中的所有用戶端相同的速度。 自動中斷連線表示如下指定 /Threshold 卸除任何用戶端將會中斷連線。 多資料流表示用戶端會分成 /StreamCount 所指定的多個工作階段。</li><li>[/ 閾值：<Speed in KBps>]-針對 /Policy:AutoDisconnect，這個選項會設定最小傳輸速率以 kbps 為單位。 放在此速率下的用戶端將會中斷從多點傳送傳輸。</li><li>[/ StreamCount: {2 &#124; 3}][/ 後援: {[是] &#124; No}]-/Policy:Multistream，針對這個選項會決定工作階段的數目。 2 表示兩個工作階段 （快速和低速） 3 表示三個工作階段 (緩慢、 中、 fast)。</li><li>[/ 後援: {[是]&#124; No}]-決定是否都已中斷連線的用戶端會繼續使用其他方法 （如果支援用戶端） 傳送。 如果您使用的 WDS 用戶端，電腦會將改為單點傳播。 Wdsmcast.exe 不支援的後援機制。 此選項也適用於不支援多資料流的用戶端。 在此情況下，電腦會切換回另一種方法，而不需要移動的速度較慢的傳輸工作階段。</li></ul></li></ul>|
## <a name="BKMK_examples"></a>範例
若要設定讓伺服器只回應已知用戶端，以回應延遲 4 分鐘，請輸入：
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
若要設定伺服器的開機程式和架構，請輸入：
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
若要啟用登入伺服器，請輸入：
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
若要啟用自動安裝在伺服器上，以及架構和用戶端自動安裝檔案類型：
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
若要設定開機前執行環境 (PXE) 伺服器，以嘗試繫結至 TCP 連接埠 67 和 60，請輸入：
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用伺服器命令](using-the-disable-server-command.md)
[使用 啟用伺服器命令](using-the-enable-server-command.md)
[使用取得伺服器的命令](using-the-get-server-command.md)
[使用初始化伺服器命令](using-the-initialize-server-command.md)
[子命令： 啟動 Server](subcommand-start-server.md) 
 [子命令： 停止伺服器](subcommand-stop-server.md)
[取消初始化伺服器選項](the-uninitialize-server-option.md)
