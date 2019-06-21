---
title: 在叢集中部署遠端存取
description: 本主題是部署在 Windows Server 2016 的叢集中的遠端存取快速入門的一部分。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 853788f20c452391c802f0681fa23978b4892c6a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281223"
---
# <a name="deploy-remote-access-in-a-cluster"></a>在叢集中部署遠端存取

>適用於：Windows Server （半年通道），Windows Server 2016

Windows Server 2016 和 Windows Server 2012 結合 DirectAccess 和遠端存取服務\(RAS\) VPN 成一個遠端存取角色。 您可以部署多個企業案例中的遠端存取。 本概觀介紹企業案例的部署多個遠端存取伺服器叢集負載平衡與 Windows 網路負載平衡\(NLB\)或使用外部負載平衡器\(ELB\)，例如 F5 巨量\-IP。  

## <a name="BKMK_OVER"></a>案例描述  
叢集部署會將多個遠端存取伺服器收集到單一單位，然後當作遠端用戶端電腦透過 DirectAccess 或 VPN 連線到內部公司網路使用的外部虛擬 IP連絡人的單一點\(VIP\)遠端存取叢集的位址。  叢集的流量進行負載平衡使用 Windows NLB 或外部負載平衡器\(例如 F5 巨量\-IP\)。  

## <a name="prerequisites"></a>必要條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  

-   預設透過 Windows NLB 的負載平衡。  

-   支援的外部負載平衡器。  

-   單點傳播模式是預設和建議使用的 NLB 模式。  

-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。  

-   使用 NLB 或外部負載平衡器時，將 IPHTTPS 前置詞不能變更為任何項目以外的其他\/59。  

-   負載平衡節點必須在相同的 IPv4 子網路中。  

-   在 ELB 部署中，如果管理需要向外，則 DirectAccess 用戶端無法使用&nbsp;Teredo。 只有 IPHTTPS 可以用於結束\-至\-結束通訊。  

-   請確定所有已知的 NLB\/ELB hotfix 的安裝。  

-   不支援公司網路中的 ISATAP。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。  

## <a name="in-this-scenario"></a>在這個案例中  
叢集部署案例包含一些步驟：  

1.  [部署 VPN 伺服器，使用進階選項永遠](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md)。 設定叢集部署之前，必須部署單一遠端存取伺服器使用進階設定。  

2.  [規劃遠端存取叢集部署](plan/Plan-a-Remote-Access-Cluster-Deployment.md)。 若要建立從單一伺服器部署在叢集的其他許多步驟則是必要項目，包括準備用於叢集部署的憑證項目。  

3.  [設定遠端存取叢集](configure/Configure-a-Remote-Access-Cluster.md)。 這包含數個設定步驟，包括準備 Windows NLB 或外部負載平衡器的單一伺服器、 準備要加入叢集，其他伺服器以及啟用負載平衡。  

## <a name="BKMK_APP"></a>實際的應用程式  
將多個伺服器納入叢集中，可以提供以下優點：  

-   延展性。 單一的遠端存取伺服器提供的伺服器可靠性與延展效能的程度有限。 將兩部或更多伺服器編列成單一叢集後，您就可以有更多能力接納更多使用者以及提供更高的輸送量。  

-   高可用性。 叢集提供高可用性，以 一律\-上存取。 如果叢集中有一部伺服器當機，遠端使用者可以透過叢集中的其他伺服器，繼續連接公司網路。 在叢集中的所有伺服器都具有同一組叢集虛擬 IP \(VIP\)位址，同時仍可保有唯一的專為每個伺服器的 IP 位址。  

-   輕鬆\-的\-管理。 叢集可讓管理多部伺服器做為單一實體。 共用的設定可以輕鬆設定到叢集中的每一部伺服器。 可以從任何叢集，或使用遠端伺服器管理工具，從遠端伺服器管理遠端存取設定\(RSAT\)。 此外，整個叢集可以從單一「遠端存取管理」主控台進行監視。  

## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出本案例必備的角色和功能：  

|角色\/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 它包含 DirectAccess，這是先前在 Windows Server 2008 R2，以及 「 路由及遠端存取服務的功能\(RRAS\)，這是先前的角色服務，在網路原則與存取服務\(NPAS\)伺服器角色。 遠端存取角色包含兩個元件：<br /><br />-永遠開啟 」 VPN 和路由及遠端存取服務\(RRAS\) VPN-管理 DirectAccess 和 VPN 一起在遠端存取管理主控台中。<br />-在舊版的路由及遠端存取主控台中管理 RRAS 路由-RRAS 路由功能。<br /><br />相依性如下所示：<br /><br />為 Internet Information Services \(IIS\) Web Serve-這項功能，才能設定網路位置伺服器和預設 web 探查。<br />-Windows 內部 Database-Used 的遠端存取伺服器上的本機計量。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它預設會安裝在遠端存取伺服器時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面。<br />-它可以選擇性地安裝在不執行 遠端存取伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI 和命令列工具<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />RAS 連線管理員系統管理組件\(CMAK\)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
|網路負載平衡|這個功能利用 Windows NLB，提供叢集的負載平衡功能。|  

## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  

-   至少兩部符合硬體需求適用於 Windows Server 2012 的電腦。  

-   外部負載平衡器案例中，需要專用的硬體已經\(例如 F5 BigIP\)。  

-   為了測試案例，您必須至少一個執行設定為一律開啟 」 VPN 用戶端的 Windows 10 的電腦。   

## <a name="BKMK_SOFT"></a>軟體需求  
本案例的一些需求：  

-   部署單一伺服器時的軟體需求。 如需詳細資訊，請參閱[部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 單一遠端存取）。  

-   除了在單一伺服器的軟體需求有幾個叢集的\-特定需求：  

    -   在每一部叢集伺服器的 IP\-HTTPS 憑證主體名稱必須與 ConnectTo 位址相符。 叢集部署支援混合使用萬用字元和非\-叢集伺服器上的萬用字元憑證。  

    -   如果網路位置伺服器安裝在遠端存取伺服器上，則在叢集中的每一部伺服器上，網路位置伺服器憑證必須要有相同的主體名稱。 此外，網路位置伺服器憑證的名稱不可以和 DirectAccess 部署中的任何伺服器名稱相同。  

    -   IP\-HTTPS 和網路位置伺服器憑證必須使用相同的方法與單一的伺服器憑證發行。 例如，如果單一伺服器使用公開憑證授權單位\(CA\)叢集中的所有伺服器都必須有公用 CA 所核發的憑證。 或者，如果單一伺服器使用自我\-ip 簽署憑證\-HTTPS 則叢集中的所有伺服器必須執行同樣的。  

    -   指派給伺服器叢集 DirectAccess 用戶端電腦的 IPv6 首碼必須是 59 位元。 如果開通 VPN，則 VPN 首碼也必須設定成 59 位元。  

## <a name="KnownIssues"></a>已知的問題  
以下是設定叢集案例的已知問題：  

-   Ipv4 設定 DirectAccess 之後\-只部署單一網路介面卡，並且在預設 DNS64 \(IPv6 位址包含": 3333::"\)會自動設定該網路介面卡，嘗試啟用負載\-平衡透過遠端存取管理主控台會導致提示使用者提供 IPv6 DIP。 如果提供 IPv6 DIP，設定會在按一下 [認可]  之後失敗，並發生錯誤：參數不正確。  

    解決此問題：  

    1.  從 [備份和還原遠端存取設定](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)下載備份和還原指令碼。  

    2.  備份遠端存取 Gpo 使用已下載指令碼備份\-RemoteAccess.ps1  

    3.  嘗試啟用負載平衡，直到它失敗所在的步驟。 在 [啟用負載平衡] 對話方塊中，展開 [詳細資料] 區域中，向右\-在 [詳細資料] 區域中，按一下，然後按一下**複製指令碼**。  

    4.  開啟 [記事本]，並貼上剪貼簿的內容。 例如:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  關閉任何開啟的 [遠端存取] 對話方塊，並關閉遠端存取管理主控台。  

    6.  編輯貼上的文字，並移除 IPv6 位址。 例如:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  在提升權限的 PowerShell 視窗中，執行前一步驟的命令。  

    8.  如果執行時，cmdlet 就會失敗\(並非因為不正確的輸入值\)，執行命令還原\-RemoteAccess.ps1 並遵循指示，來確定原始設定的完整性會受到保護.  

    9. 您現在可以重新開啟遠端存取管理主控台。  
