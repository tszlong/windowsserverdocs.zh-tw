---
title: 步驟1設定 Advanced DirectAccess 基礎結構
description: 本主題是使用 Windows Server 2016 部署單一 DirectAccess 伺服器與 Advanced Settings 指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 74a38d16bba173fc91790fbdb03026c679929d56
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955275"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>步驟1設定 Advanced DirectAccess 基礎結構

>適用目標︰Windows Server 2012 R2、Windows Server 2012

本主題說明如何針對在混合了 IPv4 和 IPv6 的環境中使用單一 DirectAccess 伺服器的進階「遠端存取」部署，設定所需的基礎結構。 開始部署步驟之前，請確定您已完成[規劃先進的 DirectAccess 部署](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)中所述的規劃步驟。

| Task | 描述 |
|--|--|
| 1.1 設定伺服器網路設定 | 設定 DirectAccess 伺服器上的伺服器網路設定。 |
| 1.2 設定強制通道 | 設定強制通道。 |
| 1.3 設定公司網路中的路由 | 設定公司網路中的路由。 |
| 1.4 設定防火牆 | 如有必要，設定其他防火牆。 |
| 1.5 設定 CA 和憑證 | 如有必要，可設定憑證授權單位 (CA)，以及部署中所需的任何其他憑證範本。 |
| 1.6 設定 DNS 伺服器 | 設定 DirectAccess 伺服器的「網域名稱系統」(DNS) 設定。 |
| 1.7 設定 Active Directory | 將用戶端電腦和 DirectAccess 伺服器加入 Active Directory 網域。 |
| 1.8 設定 GPO | 視需要為部署設定 GPO。 |
| 1.9 設定安全性群組 | 設定將包含 DirectAccess 用戶端電腦的安全性群組，以及部署中所需的任何其他安全性群組。 |
| 1.10 設定網路位置伺服器 | 設定網路位置伺服器，包括安裝網路位置伺服器網站憑證。 |

> [!NOTE]
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="11-configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>1.1 設定伺服器網路設定
以下是在使用 IPv4 和 IPv6 的環境中進行單一伺服器部署時，所需的網路介面設定。 您可以使用 [Windows 網路和共用中心]**** 中的 [變更介面卡設定]**** 設定所有 IP 位址。

**邊緣拓撲**

-   兩個網際網路對向連續公用靜態 IPv4 或 IPv6 位址

    > [!NOTE]
    > Teredo 需要兩個公用位址。 如果您不是使用 Teredo，則可以設定單一公用靜態 IPv4 位址。

-   一個單一的內部靜態 IPv4 或 IPv6 位址

**在 NAT 裝置後面 (有兩張網路介面卡)**

-   一個單一的網際網路對向靜態 IPv4 或 IPv6 位址

-   一個單一的內部網路對向靜態 IPv4 或 IPv6 位址

**在 NAT 裝置後面 (有一張網路介面卡)**

-   一個單一的內部網路對向靜態 IPv4 或 IPv6 位址

> [!NOTE]
> 如果含兩張或更多張網路介面卡 (一張歸類於網域設定檔，另一張歸類於公用或私人設定檔) 的 DirectAccess 伺服器被設定了單一網路介面卡拓撲，我們有下列建議：
>
> -   確定第二張網路介面卡和任何額外的網路介面卡皆歸類於網域設定檔。
> -   如果無法為網域設定檔設定第二張網路介面卡，則在設定 DirectAccess 之後，必須使用下列 Windows PowerShell 命令，手動將 DirectAccess IPsec 原則的範圍設為所有設定檔：
>
>     ```
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any
>     Save-NetGPO "GPOSession $gposession
>     ```

## <a name="12-configure-force-tunneling"></a><a name="BKMK_forcetunnel"></a>1.2 設定強制通道
可以透過「遠端存取安裝精靈」設定強制通道。 它在「設定遠端用戶端精靈」中是顯示為核取方塊。 這個設定只會影響 DirectAccess 用戶端。 如果啟用了 VPN，VPN 用戶端預設便會使用強制通道。 系統管理員可以從用戶端設定檔變更 VPN 用戶端的設定。

選取強制通道的核取方塊會有下列作用：

-   在 DirectAccess 用戶端啟用強制通道

-   在「名稱解析原則表格」(NRPT) 中為 DirectAccess 用戶端新增 **Any** 項目，這表示所有 DNS 流量都會連到內部網路 DNS 伺服器

-   設定 DirectAccess 用戶端一律使用 IP-HTTPS 轉換技術

若要讓使用強制通道的 DirectAccess 用戶端能夠使用網際網路資源，您可以使用 Proxy 伺服器，此伺服器可接收 IPv6 型網際網路資源要求，並將它們轉譯成 IPv4 型網際網路資源要求。 若要為網際網路資源設定 Proxy 伺服器，您需要修改 NRPT 中的預設項目來新增 Proxy 伺服器。 您可以使用「遠端存取」PowerShell Cmdlet 或 DNS PowerShell Cmdlet 來達成這個目的。 例如，使用「遠端存取」PowerShell Cmdlet，如下所示：

```
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>
```

> [!NOTE]
> 如果在相同的伺服器上啟用 DirectAccess 和 VPN，而 VPN 處於強制通道模式，且伺服器部署在邊緣拓撲或在 NAT 後面拓撲 (有兩張網路介面卡，一張連線到網域，一張連線到私人網路) 中，則無法透過 DirectAccess 伺服器的外部介面轉送 VPN 網際網路流量。 若要啟用此案例，組織必須在單一網路介面卡拓撲中，於防火牆後面的伺服器上部署「遠端存取」。 組織也可以在內部網路中使用個別的 Proxy 伺服器，來轉送來自 VPN 用戶端的網際網路流量。

> [!NOTE]
> 如果組織使用 Web Proxy 讓 DirectAccess 用戶端存取網際網路資源，而公司 Proxy 無法處理內部網路資源，則 DirectAccess 用戶端若不在內部網路中，就會無法存取內部資源。 在這樣的案例中，若要讓 DirectAccess 用戶端存取內部資源，請使用基礎結構精靈的 DNS 頁面，手動為內部網路尾碼建立 NRPT 項目。 請勿在這些 NRPT 尾碼上套用 Proxy 設定。 尾碼應該被填入預設的 DNS 伺服器項目。

## <a name="13-configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>1.3 設定公司網路中的路由
設定公司網路中的路由，如下所示：

-   在組織中部署原生 IPv6 之後，新增一個路由，以便讓內部網路上的路由器透過 DirectAccess 伺服器將 IPv6 流量遞送回來。

-   在 DirectAccess 伺服器上手動設定組織的 IPv4 和 IPv6 路由。 新增一個已發佈的路由，以便將所有含有組織 (/48) IPv6 首碼的流量轉送到內部網路。 針對 IPv4 流量，新增明確的路由，以便將 IPv4 流量轉送到內部網路。

## <a name="14-configure-firewalls"></a><a name="ConfigFirewalls"></a>1.4 設定防火牆
當部署中使用額外的防火牆時，如果 DirectAccess 伺服器位於 IPv4 網際網路上，請為「遠端存取」流量套用下列網際網路對向防火牆例外：

-   Teredo 流量「使用者資料包協定 (UDP) 目的地埠3544輸入，以及 UDP 來源埠3544輸出。

-   6to4 流量「IP 通訊協定41輸入和輸出」。

-   IP-HTTPS 「傳輸控制通訊協定 (TCP) 目的地埠443，以及 TCP 來源埠443輸出。 當 DirectAccess 伺服器具有單一網路介面卡，而網路位置伺服器位於 DirectAccess 伺服器上時，則也需要 TCP 連接埠 62000。

    > [!NOTE]
    > 這個豁免必須設定在 DirectAccess 伺服器上，所有其他豁免則必須設定在邊緣防火牆上。

> [!NOTE]
> 針對 Teredo 和 6to4 流量，應該對 DirectAccess 伺服器上的兩個網際網路對向連續公用 IPv4 位址都套用這些例外。 若為 IP-HTTPS，這些例外只需套用到伺服器公用名稱解析的位址。

使用額外的防火牆時，如果 DirectAccess 伺服器位於 IPv6 網際網路上，請為「遠端存取」流量套用下列網際網路對向防火牆例外：

-   IP 通訊協定 50

-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。

-   適用于 IPv6 的網際網路控制訊息通訊協定 (ICMPv6) 流量輸入和輸出」僅適用于 Teredo 實現。

當使用額外防火牆時，請針對遠端存取流量套用下列內部網路防火牆例外：

-   ISATAP "通訊協定41輸入和輸出

-   適用於所有 IPv4/IPv6 流量的 TCP/UDP

-   適用於所有 IPv4/IPv6 流量的 ICMP

## <a name="15-configure-cas-and-certificates"></a><a name="ConfigCAs"></a>1.5 設定 CA 和憑證
Windows Server 2012 中的遠端存取可讓您選擇使用憑證來進行電腦驗證，或使用內建的 Kerberos proxy 來驗證使用使用者名稱和密碼。 您必須也在 DirectAccess 伺服器上設定 IP-HTTPS 憑證。

如需詳細資訊，請參閱[Active Directory 憑證服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10))。

### <a name="151-configure-ipsec-authentication"></a>1.5.1 設定 IPsec 驗證
DirectAccess 伺服器和所有 DirectAccess 用戶端上都必須有電腦憑證，才能使用 IPsec 驗證。 憑證必須由內部憑證授權單位 (CA) 簽發，而 DirectAccess 伺服器和 DirectAccess 用戶端必須信任簽發根憑證和中繼憑證的 CA 鏈結。

##### <a name="to-configure-ipsec-authentication"></a>設定 IPsec 驗證

1.  在內部 CA 中，決定您是要使用 [電腦]**** 憑證範本，還是要依[建立憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10))中所述建立新憑證範本。

    > [!NOTE]
    > 如果您建立新的範本，則必須設定它以進行「用戶端驗證」。

2.  視需要部署憑證範本。 如需詳細資訊，請參閱[部署憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10))。

3.  視需要設定評證範本以進行自動註冊。 如需詳細資訊，請參閱[設定憑證自動註冊](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731522(v=ws.11))。

### <a name="152-configure-certificate-templates"></a><a name="ConfigCertTemp"></a>1.5.2 設定憑證範本
使用內部 CA 來簽發憑證時，您必須為 IP-HTTPS 憑證和網路位置伺服器網站憑證設定憑證範本。

##### <a name="to-configure-a-certificate-template"></a>設定憑證範本

1.  在內部 CA 上，使用[建立憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10))中所述的方式建立憑證範本。

2.  使用 [部署憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10))中所述的方式部署憑證範本。

### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 設定 IP-HTTPS 憑證
「遠端存取」必須要有 IP-HTTPS 憑證，才能驗證連到 DirectAccess 伺服器的 IP-HTTPS 連線。 IP-HTTPS 驗證有三個可用的憑證選項：

**公開憑證**

公開憑證是由協力廠商提供。 如果憑證主體名稱不包含萬用字元，它必定是僅用於 DirectAccess 伺服器 IP-HTTPS 連線的可外部解析完整網域名稱 (FQDN) URL。

**私人憑證**

如果您使用私人憑證，則必須要有下列項目 (如果尚未備妥的話)：

-   用於 IP-HTTPS 驗證的網站憑證。 憑證主體必須是可從網際網路存取的外部可解析 FQDN。 憑證是以您依照1.5.2 設定憑證範本中的指示所建立的憑證範本為基礎。

-   可從公用可解析 FQDN 存取的憑證撤銷清單 (CRL) 發佈點。

**自我簽署憑證**

如果您使用自我簽署憑證，則必須要有下列項目 (如果尚未備妥的話)：

-   用於 IP-HTTPS 驗證的網站憑證。 憑證主體必須是可從網際網路存取的外部可解析 FQDN。

-   可從可公開解析的 FQDN 存取的 CRL 發佈點。

> [!NOTE]
> 自我簽署憑證無法在多站台部署中使用。

請確定用於 IP-HTTPS 驗證的網站憑證符合下列需求：

-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。

-   在 [主體]**** 欄位中，指定的 IP-HTTPS URL 的 FQDN。

-   在 [增強金鑰使用方法]**** 欄位中，使用伺服器驗證物件識別碼 (OID)。

-   在 [CRL 發佈點]**** 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。

-   IP-HTTPS 憑證必須具有私密金鑰。

-   IP-HTTPS 憑證必須直接匯入到個人存放區。

-   IP-HTTPS 憑證的名稱中可以包含萬用字元。

##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>從內部 CA 安裝 IP-HTTPS 憑證

1.  在 DirectAccess 伺服器上：在 [**開始**] 畫面上，輸入**mmc.exe**，然後按 enter。

2.  在 MMC 主控台的 [檔案] 功能表上，按一下 [新增/移除嵌入式管理單元]。

3.  在 [新增或移除嵌入式管理單元]**** 對話方塊中，依序按一下 [憑證]****、[新增]****、[電腦帳戶]****、[下一步]****、[本機電腦]****、[完成]****，然後按一下 [確定]****。

4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]****。

5.  在 [憑證]**** 上按一下滑鼠右鍵，指向 [所有工作]****，然後按一下 [要求新憑證]****。

6.  按兩次 [下一步]****。

7.  在 [**要求憑證**] 頁面上，選取您先前建立之憑證範本的核取方塊 (如需詳細資訊，請參閱1.5.2 設定憑證範本) 。 視需要按一下 [需要更多資訊才能註冊此憑證]****。

8.  在 [憑證內容]**** 對話方塊的 [主體]**** 索引標籤上，於 [主體名稱]**** 區域的 [類型]**** 中，選取 [一般名稱]****。

9. 在 [值]**** 中，指定 DirectAccess 伺服器之對外介面卡的 IPv4 位址，或 IP-HTTPS URL 的 FQDN，然後按一下 [新增]****。

10. 在 [別名]**** 區域的 [類型]**** 中，選取 [DNS]****。

11. 在 [值]**** 中，指定 DirectAccess 伺服器之對外介面卡的 IPv4 位址，或 IP-HTTPS URL 的 FQDN，然後按一下 [新增]****。

12. 在 [一般]**** 索引標籤的 [易記名稱]**** 中，您可以輸入可協助您識別憑證的名稱。

13. 在 [延伸]**** 索引標籤上，按一下 [擴充金鑰使用方法]**** 旁邊的箭號，確定 [伺服器驗證]**** 有出現在 [選取的選項]**** 清單中。

14. 依序按一下 [確定]****、[註冊]****，然後按一下 [完成]****。

15. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已註冊新憑證且「使用目的」為「伺服器驗證」。

## <a name="16-configure-the-dns-server"></a><a name="ConfigDNS"></a>1.6 設定 DNS 伺服器
您必須為部署中內部網路的網路位置伺服器網站手動設定 DNS 項目。

### <a name="to-create-the-network-location-server"></a><a name="NLS_DNS"></a>建立網路位置伺服器

1.  在內部網路 DNS 伺服器上：在 [**開始**] 畫面上，輸入**dnsmgmt.msc**，然後按 enter。

2.  在 [DNS 管理員]**** 主控台的左窗格中，展開您網域的正向對應區域。 在網域上按一下滑鼠右鍵，然後按一下 [新增主機 (A 或 AAAA)]****。

3.  在 [新增主機]**** 對話方塊的 [IP 位址]**** 方塊中：

    -   在 [名稱 (如果空白就使用父系網域名稱)]**** 方塊中，輸入網路位置伺服器網站的 DNS 名稱 (這是 DirectAccess 用戶端用來連線到網路位置伺服器的名稱)。

    -   輸入網路位置伺服器的 IPv4 或 IPv6 位址，然後按一下 [新增主機]****，再按一下 [確定]****。

4.  在 [新增主機]**** 對話方塊中：

    -   在 [名稱 (如果空白就使用父系網域名稱)]**** 方塊中，輸入 Web 探查的 DNS 名稱 (預設的 Web 探查名稱是 **directaccess-webprobehost**)。

    -   在 [IP 位址]**** 方塊中，輸入 Web 探查的 IPv4 或 IPv6 位址，然後按一下 [新增主機]****。

    -   針對 **directaccess-corpconnectivityhost** 和任何手動建立的連線能力檢查器重複此程序。

5.  在 [DNS]**** 對話方塊中，按一下 [確定]****，然後按一下 [完成]****。

![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>
```

您也必須設定下列各項的 DNS 項目：

-   **IP-HTTPS 伺服器**

    DirectAccess 用戶端必須要能夠從網際網路解析 DirectAccess 伺服器的 DNS 名稱。

-   **CRL 撤銷檢查**

    DirectAccess 會針對 DirectAccess 用戶端與 DirectAccess 伺服器之間的 IP-HTTPS 連線，以及 DirectAccess 用戶端與網路位置伺服器之間的 HTTPS 型連線，使用憑證撤銷檢查。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。

-   **ISATAP**

    「內部網站自動通道定址通訊協定」(ISATAP) 會使用通道讓 DirectAccess 用戶端透過 IPv4 網際網路 (將 IPv6 封包封裝在 IPv4 標頭內) 連線到 DirectAccess 伺服器。 「遠端存取」會使用它，透過內部網路提供對 ISATAP 主機的 IPv6 連線能力。 在非原生 IPv6 網路環境中，DirectAccess 伺服器會自動將自己設定為 ISATAP 路由器。 需要 ISATAP 名稱的解析支援。

## <a name="17-configure-active-directory"></a><a name="ConfigAD"></a>1.7 設定 Active Directory
DirectAccess 伺服器和所有 DirectAccess 用戶端電腦都必須加入 Active Directory 網域。 DirectAccess 用戶端電腦必須是下列其中一種網域類型的成員：

-   與 DirectAccess 伺服器屬於相同樹系的網域。

-   屬於與 DirectAccess 伺服器樹系具有雙向信任關係之樹系的網域。

-   與 DirectAccess 伺服器網域具有雙向網域信任關係的網域。

#### <a name="to-join-the-directaccess-server-to-a-domain"></a>將 DirectAccess 伺服器加入網域

1.  在 [伺服器管理員] 中，按一下 [本機伺服器]****。 在詳細資料窗格中，按一下 [電腦名稱]**** 旁邊的連結。

2.  在 [系統內容]**** 對話方塊中，按一下 [電腦名稱]**** 索引標籤，然後按一下 [變更]****。

3.  在 [電腦名稱]**** 中，如果您將伺服器加入網域時也要變更電腦名稱，請輸入該電腦名稱。 在 [隸屬於]**** 底下，按一下 [網域]****，然後輸入伺服器要加入的網域名稱 (例如 corp.contoso.com)，然後按一下 [確定]****。

4.  提示您輸入使用者名稱和密碼時，輸入有權將電腦加入至網域的使用者名稱及密碼，然後按一下 [確定]****。

5.  當您看見歡迎您加入網域的對話方塊時，按一下 [確定]****。

6.  當提示您必須重新啟動電腦時，按一下 [確定]****。

7.  按一下 [系統內容]**** 對話方塊中的 [關閉]****。

8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]****。

#### <a name="to-join-client-computers-to-the-domain"></a>將用戶端電腦加入網域

1.  在 [**開始**] 畫面上，輸入**explorer.exe**，然後按 enter。

2.  在 [電腦] 圖示上按一下滑鼠右鍵，然後按一下 [內容]****。

3.  在 [系統]**** 頁面上，按一下 [進階系統設定]****。

4.  在 [系統內容]**** 對話方塊中的 [電腦名稱]**** 索引標籤上，按一下 [變更]****。

5.  在 [電腦名稱]**** 中，如果您將伺服器加入網域時也要變更電腦名稱，請輸入該電腦名稱。 在 [隸屬於]**** 底下，按一下 [網域]****，然後輸入伺服器要加入的網域名稱 (例如 corp.contoso.com)，然後按一下 [確定]****。

6.  提示您輸入使用者名稱和密碼時，輸入有權將電腦加入至網域的使用者名稱及密碼，然後按一下 [確定]****。

7.  當您看見歡迎您加入網域的對話方塊時，按一下 [確定]****。

8.  當提示您必須重新啟動電腦時，按一下 [確定]****。

9. 按一下 [系統內容]**** 對話方塊中的 [關閉]****。

10. 當提示您重新啟動電腦時，請按一下 [立即重新啟動]****。

![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

> [!NOTE]
> 當您輸入下列 **Add-Computer** 命令時，您必須提供網域認證。

```
Add-Computer -DomainName <domain_name>
Restart-Computer
```

## <a name="18-configure-gpos"></a><a name="ConfigGPOs"></a>1.8 設定 GPO
若要部署遠端存取，至少需要兩個群組原則物件：

-   一個包含 DirectAccess 伺服器的設定

-   一個包含 DirectAccess 用戶端電腦的設定

當您設定「遠端存取」時，嚮導會自動建立必要的群組原則物件。 不過，如果您的組織有強制執行命名慣例，您可以在 [遠端存取管理主控台] 的 [GPO] 對話方塊中輸入名稱。 如需詳細資訊，請參閱 2.7. 設定摘要和替代 GPO。 如果您已經建立權限，GPO 也將被建立。 如果您沒有可建立 GPO 的必要權限，則必須先建立這些權限，再設定「遠端存取」。

若要建立群組原則物件，請參閱[建立和編輯群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11))。

> [!IMPORTANT]
> 系統管理員可以依照下列步驟，手動將 DirectAccess 群組原則物件連結到組織單位 (OU) ：
>
> 1.  設定 DirectAccess 之前，請先將已建立的 GPO 連結到個別的 OU。
> 2.  設定 DirectAccess 時，請指定用戶端電腦的安全性群組。
> 3.  「遠端存取」系統管理員不一定擁有將群組原則物件連結到網域的許可權。 在這兩種情況下，都會自動設定群組原則物件。 如果 GPO 已經連結到 OU，該連結將不會被移除，而 GPO 將不會被連結到網域。 以伺服器 GPO 來說，OU 必須包含伺服器電腦物件，否則 GPO 就會被連結到網域的根目錄。
> 4.  如果您在執行 DirectAccess Wizard 之前未連結到 OU，則在設定完成之後，網域系統管理員可以將 DirectAccess 群組原則物件連結到所需的 Ou。 您可以移除網域的連結。 如需詳細資訊，請參閱[連結群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11))。

> [!NOTE]
> 如果群組原則物件是以手動方式建立，則在 DirectAccess 設定期間，可能會無法使用群組原則物件。 群組原則物件可能尚未複寫到最接近管理電腦的網域控制站。 在此情況下，系統管理員可以等候複寫完成，或強制複寫。

### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 使用有限的權限設定遠端存取 GPO
在使用暫時與實際執行 GPO 的部署中，網域系統管理員應執行下列動作：

1.  從「遠端存取」系統管理員取得「遠端存取」部署所需的 GPO 清單。 如需詳細資訊，請參閱1.8 規劃群組原則物件。

2.  為「遠端存取」系統管理員要求的每一個 GPO ，建立一對具有不同名稱的 GPO。 第一個會用來做為暫存 GPO，第二個會用來做為實際執行 GPO。

    若要建立群組原則物件，請參閱[建立和編輯群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11))。

3.  若要連結實際執行 GPO，請參閱[連結群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11))。

4.  將所有暫存 GPO 的**編輯設定、刪除及修改安全性**權限授與「遠端存取」系統管理員。 如需詳細資訊，請參閱[委派群組原則物件中群組或使用者的權限](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754542(v=ws.11))。

5.  拒絕遠端存取系統管理員在所有網域中連結 Gpo 的許可權 (或確認「遠端存取」系統管理員沒有這類許可權) 。 如需詳細資訊，請參閱[委派連結群組原則物件的權限](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755086(v=ws.11))。

當「遠端存取」系統管理員設定「遠端存取」時，他們應該一律只指定暫存 GPO (而不是實際執行 GPO)。 這在「遠端存取」的初始設定中，以及在執行需要額外 GPO 的額外設定操作時都成立；例如，在多站台部署中新增進入點或在額外網域中啟用用戶端電腦的時候。

在「遠端存取」系統管理員完成對「遠端存取」設定的任何變更之後，網域系統管理員應該檢閱暫存 GPO 中的設定，並使用下列程序將設定複製到實際執行 GPO。

> [!TIP]
> 請在每次變更「遠端存取」設定之後，執行下列程序。

##### <a name="to-copy-settings-to-the-production-gpos"></a>將設定複製到實際執行 GPO

1.  確認「遠端存取」部署中的所有暫存 GPO 皆已複寫到網域中的所有網域控制站。 必須這麼做，才能確保將最新的設定匯入到實際執行 GPO。 如需詳細資訊，請參閱檢查群組原則基礎結構狀態。

2.  將「遠端存取」部署中的所有暫存 GPO 備份以匯出設定。 如需詳細資訊，請參閱備份群組原則物件。

3.  針對每個實際執行 GPO，變更安全性篩選以符合對應之暫存 GPO 的安全性篩選。 如需詳細資訊，請參閱使用安全性群組進行篩選。

    > [!NOTE]
    > 必須這麼做，因為 [匯入設定]**** 不會複製來源 GPO 的安全性篩選。

4.  針對每個實際執行 GPO，從對應之暫存 GPO 的備份匯入設定，如下所示：

    1.  在 [群組原則管理主控台 (GPMC) 中，展開樹系和網域中的 [群組原則物件] 節點，其中包含將匯入設定的生產群組原則物件。

    2.  在 GPO 上按一下滑鼠右鍵，然後按一下 [匯入設定]****。

    3.  在 [匯入設定精靈]**** 的 [歡迎使用]**** 頁面上，按 [下一步]****。

    4.  在 [備份 GPO]**** 頁面上，按一下 [備份]****。

    5.  在 [備份群組原則物件]**** 對話方塊的 [位置]**** 方塊中，輸入您要存放 GPO 備份的位置路徑，或按一下 [瀏覽]**** 來尋找資料夾。

    6.  在 [描述]**** 方塊中，輸入實際執行 GPO 的描述，然後按一下 [備份]****。

    7.  備份完成時，按一下 [確定]****，然後在 [備份 GPO]**** 頁面上，按 [下一步]****。

    8.  在 [備份位置]**** 頁面的 [備份資料夾]**** 方塊中，輸入在步驟 2 中儲存對應之暫存 GPO 備份的位置路徑，或按一下 [瀏覽]**** 來尋找資料夾，然後按 [下一步]****。

    9. 在 [來源 GPO]**** 頁面上，選取 [只顯示每個 GPO 最新的版本]**** 核取方塊來隱藏較舊的備份，然後選取對應的暫存 GPO。 按一下 [檢視設定]**** 以先檢閱「遠端存取」設定，再將它們套用到實際執行 GPO，然後按 [下一步]****。

    10. 在 [掃描備份]**** 頁面上，按 [下一步]****，然後按一下 [完成]****。

![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

-   若要將網域 "corp.contoso.com" 中的預備用戶端 GPO "DirectAccess Client Settings-預備" 備份到備份檔案夾 "C:\Backups \" ：

    ```
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'
    ```

-   若要查看網域 "corp.contoso.com" 中暫存用戶端 GPO 「DirectAccess 用戶端設定-預備」的安全性篩選：

    ```
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}
    ```

-   若要將安全性群組「Com\directaccess clients 用戶端」新增至網域 "corp.contoso.com" 中生產用戶端 GPO "DirectAccess Client Settings" 生產 "的安全性篩選：

    ```
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group
    ```

-   若要將設定從備份匯入網域 "corp.contoso.com" 中的實際執行用戶端 GPO "DirectAccess Client Settings" 生產 "：

    ```
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'
    ```

## <a name="19-configure-security-groups"></a><a name="ConfigSGs"></a>1.9 設定安全性群組
用戶端電腦中包含的 DirectAccess 設定群組原則物件，只會套用到您在設定遠端存取時所指定之安全性群組的成員電腦。 此外，如果您使用安全性群組來管理應用程式伺服器，請為這些伺服器建立安全性群組。

### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>為 DirectAccess 用戶端建立安全性群組

1.  在 [**開始**] 畫面上，輸入**dsa.msc**，然後按 enter。 在 [Active Directory 使用者和電腦]**** 主控台的左窗格中，展開將包含安全性群組的網域，在 [使用者]**** 上按一下滑鼠右鍵，指向 [新增]****，然後按一下 [群組]****。

2.  在 [新增物件 - 群組]**** 對話方塊的 [群組名稱]**** 底下，輸入安全性群組的名稱。

3.  在 [群組領域]**** 底下，按一下 [全域]****，在 [群組類型]**** 底下，按一下 [安全性]****，然後按一下 [確定]****。

4.  按兩下 DirectAccess 用戶端電腦安全性群組，然後在 [內容] 對話方塊中，按一下 [成員]**** 索引標籤。

5.  在 [成員]**** 索引標籤上，按一下 [新增]****。

6.  在 [選取使用者、連絡人、電腦或服務帳戶]**** 對話方塊方塊中，選取您要啟用 DirectAccess 的用戶端電腦，然後按一下 [確定]****。

![Windows PowerShell](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Windows powershell 對等命令**

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>
```

## <a name="110-configure-the-network-location-server"></a><a name="ConfigNLS"></a>1.10 設定網路位置伺服器
網路位置伺服器應該是具有高可用性的伺服器，而且應該要有 DirectAccess 用戶端信任的有效 SSL 憑證。 有兩種網路位置伺服器憑證的憑證選項：

-   **私人憑證**

    此憑證是以您遵循[1.5.2 設定憑證範本](#ConfigCertTemp)中的指示所建立的憑證範本為基礎。

-   **自我簽署憑證**

    > [!NOTE]
    > 自我簽署憑證無法在多站台部署中使用。

以下是這兩種憑證都需要的項目 (如果尚未備妥的話)：

-   用於網路位置伺服器的網站憑證。 憑證主體必須是網路位置伺服器的 URL。

-   對內部網路具有高可用性的 CRL 發佈點。

> [!NOTE]
> 如果網路位置伺服器網站位於 DirectAccess 伺服器上，則當您設定「遠端存取」時，會自動建立網站。 這個站台會繫結到您所提供的伺服器憑證。

#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>從內部 CA 安裝網路位置伺服器憑證

1.  在將裝載網路位置伺服器網站的伺服器上：在 [**開始**] 畫面上，輸入**mmc.exe**，然後按 enter。

2.  在 MMC 主控台的 [檔案] 功能表上，按一下 [新增/移除嵌入式管理單元]。

3.  在 [新增或移除嵌入式管理單元]**** 對話方塊中，依序按一下 [憑證]****、[新增]****、[電腦帳戶]****、[下一步]****、[本機電腦]****、[完成]****，然後按一下 [確定]****。

4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]****。

5.  在 [憑證]**** 上按一下滑鼠右鍵，指向 [所有工作]****，然後按一下 [要求新憑證]****。

6.  按兩次 [下一步]****。

7.  在 [**要求憑證**] 頁面上，選取您依照1.5.2 設定憑證範本中的指示所建立之憑證範本的核取方塊。 視需要按一下 [需要更多資訊才能註冊此憑證]****。

8.  在 [憑證內容]**** 對話方塊的 [主體]**** 索引標籤上，於 [主體名稱]**** 區域的 [類型]**** 中，選取 [一般名稱]****。

9. 在 [值]**** 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]****。

10. 在 [別名]**** 區域的 [類型]**** 中，選取 [DNS]****。

11. 在 [值]**** 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]****。

12. 在 [一般]**** 索引標籤的 [易記名稱]**** 中，您可以輸入可協助您識別憑證的名稱。

13. 依序按一下 [確定]****、[註冊]****，然後按一下 [完成]****。

14. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已註冊新憑證且「使用目的」為「伺服器驗證」。

#### <a name="to-configure-the-network-location-server"></a>設定網路位置伺服器

1.  在高可用性伺服器上設定網站。 這個網站不需要任何內容，但是進行測試時，您可以定義一個在用戶端連線時提供訊息的預設網頁。

    > [!NOTE]
    > 如果網路位置伺服器網站裝載在 DirectAccess 伺服器上，則不需要此步驟。

2.  將 HTTPS 伺服器憑證繫結到網站。 憑證的一般名稱應該與網路位置伺服器站台的名稱相符。 確定 DirectAccess 用戶端信任簽發 CA。

    > [!NOTE]
    > 如果網路位置伺服器網站裝載在 DirectAccess 伺服器上，則不需要此步驟。

3.  設定對內部網路具有高可用性的 CRL 站台。

    存取 CRL 發佈點時可以透過：

    -   網頁伺服器，方法是使用 HTTP 型 URL，例如：https://crl.corp.contoso.com/crld/corp-APP1-CA.crl

    -   透過通用命名慣例存取的檔案伺服器 (UNC) 路徑，例如 \\ \crl.corp.contoso.com\crld\corp-APP1-CA.crl

    如果只有透過 IPv6 才能連線到內部 CRL 發佈點，您就必須設定「具有進階安全性的 Windows 防火牆」連線安全性規則，以免除從內部網路 IPv6 位址到 CRL 發佈點 IPv6 位址的 IPsec 保護。

4.  確定內部網路上的 DirectAccess 用戶端可以解析網路位置伺服器的名稱。 確定網際網路上的 DirectAccess 用戶端無法解析該名稱。

## <a name="next-step"></a><a name="BKMK_Links"></a>後續步驟

-   [步驟 2：設定進階 DirectAccess 伺服器](da-adv-configure-s2-servers.md)

