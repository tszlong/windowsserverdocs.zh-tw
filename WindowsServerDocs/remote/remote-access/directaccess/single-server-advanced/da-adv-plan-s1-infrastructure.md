---
title: 步驟1規劃 Advanced DirectAccess 基礎結構
description: 本主題是使用 Windows Server 2016 部署單一 DirectAccess 伺服器與 Advanced Settings 指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 759caf09531b2c09034c715fa6cc479fea6d6c07
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518134"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>步驟1規劃 Advanced DirectAccess 基礎結構

>適用於：Windows Server (半年度管道)、Windows Server 2016

為單一伺服器上的進階 DirectAccess 部署做規劃的第一步，就是規劃部署所需的基礎結構。 本主題描述基礎結構規劃步驟。 這些規劃工作不需要依特定的順序完成。

|Task|說明|
|----|--------|
|[1.1 規劃網路拓撲和設定](#11-plan-network-topology-and-settings)|決定 DirectAccess 伺服器的放置位置 (在邊緣，或在「網路位址轉譯」(NAT) 裝置或防火牆後面)，並規劃 IP 位址指定、路由及強制通道。|
|[1.2 規劃防火牆需求](#12-plan-firewall-requirements)|為允許 DirectAccess 流量通過邊緣防火牆做規劃。|
|[1.3 規劃憑證需求](#13-plan-certificate-requirements)|決定是否要使用 Kerberos 或憑證進行用戶端驗證，以及規劃您的網站憑證。 IP-HTTPS 是 DirectAccess 用戶端透過 IPv4 網路建立 IPv6 流量通道時所使用的轉換通訊協定。 決定是要使用憑證授權單位 (CA) 簽發的憑證，還是使用 DirectAccess 伺服器自動簽發的自我簽署憑證，向 IP-HTTPS 伺服器進行驗證。|
|[1.4 規劃 DNS 需求](#14-plan-dns-requirements)|規劃 DirectAccess 伺服器、基礎結構伺服器、本機名稱解析選項及用戶端連線的「網域名稱系統」(DNS) 設定。|
|[1.5 規劃網路位置伺服器](#15-plan-the-network-location-server)|DirectAccess 用戶端會使用網路位置伺服器來判斷它們是否位於內部網路。 決定要將網路位置伺服器網站放在您組織中的哪個位置 (DirectAccess 伺服器上或替代伺服器上)，如果網路位置伺服器位於 DirectAccess 伺服器上，則規劃憑證需求。|
|[1.6 規劃管理伺服器](#16-plan-management-servers)|您可以從遠端管理位於公司網路外部網際網路上的 DirectAccess 用戶端電腦。 計劃遠端管理用戶端時，會用到的管理伺服器 (例如更新伺服器)。|
|[1.7 規劃 Active Directory 網域服務](#17-plan-active-directory-domain-services)|為您的網域控制站、Active Directory 需求、用戶端驗證及多個網域做規劃。|
|[1.8 規劃群組原則物件](#18-plan-group-policy-objects)|決定您組織中所需的 GPO，以及如何建立或編輯 GPO。|

## <a name="11-plan-network-topology-and-settings"></a>1.1 規劃網路拓撲和設定

本節說明如何為您的網路做規劃，包括：

- [1.1.1 規劃網路介面卡和 IP 位址指定](#111-plan-network-adapters-and-ip-addressing)

- [1.1.2 規劃 IPv6 內部網路連線](#112-plan-ipv6-intranet-connectivity)

- [1.1.3 為強制通道做規劃](#113-plan-for-force-tunneling)

### <a name="111-plan-network-adapters-and-ip-addressing"></a>1.1.1 規劃網路介面卡和 IP 位址指定

1. 識別您想要使用的網路介面卡拓撲。 可以使用下列任一拓撲來設定 DirectAccess：

    - **兩張網路介面卡**。 DirectAccess 伺服器可以安裝在邊緣，其中一張網路介面卡連線到網際網路，另一張連線到內部網路；或是安裝在 NAT、防火牆或路由器裝置後面，其中一張網路介面卡連線到周邊網路，另一張連線到內部網路。

    - **一張網路介面卡**。 DirectAccess 伺服器安裝在 NAT 裝置後面，並且單一網路介面卡連線到內部網路。

2. 識別您的 IP 位址指定需求：

    DirectAccess 使用 IPv6 搭配 IPsec 在 DirectAccess 用戶端電腦與公司內部網路之間建立安全連線。 不過，DirectAccess 不一定需要 IPv6 網際網路的連線能力，或內部網路的原生 IPv6 支援。 取而代之的是，它會自動設定並使用 IPv6 轉換技術，透過 IPv4 網際網路 (藉由使用 6to4、Teredo 或 IP-HTTPS) 及透過僅支援 IPv4 的內部網路 (藉由使用 NAT64 或 ISATAP) 建立 IPv6 流量通道。 如需這些轉換技術的概觀，請參閱下列資源：

    - [IPv6 轉換技術](/previous-versions//bb726951(v=technet.10))

    - [IP-HTTPS 通道通訊協定規格](/previous-versions//bb726951(v=technet.10))

3. 根據下表設定所需的介面卡和位址。 針對使用單一網路介面卡並設定在 NAT 裝置後面的部署，請只使用**內部網路介面卡**欄來設定您的 IP 位址。

    | 說明 | 外部網路介面卡 | 內部網路介面卡 | 路由需求 |
    |--|--|--|--|
    | IPv4 網際網路和 IPv4 內部網路 | 請設定兩個靜態連續公用 IPv4 位址搭配適當的子網路遮罩 (只有 Teredo 才需要)。<br/><br/>另外，也請設定網際網路防火牆或本機網際網路服務提供者 (ISP) 路由器的預設閘道 IPv4 位址。 **注意：** DirectAccess 伺服器需要兩個連續的公用 IPv4 位址，使其可作為 Teredo 伺服器，而 Windows 型用戶端可以使用 DirectAccess 伺服器來偵測其背後的 NAT 裝置類型。 | 設定下列各項：<br/><br/>-具有適當子網路遮罩的 IPv4 內部網路位址。<br/>-內部網路命名空間的連線特定 DNS 尾碼。 此外，也應該在內部介面上設定 DNS 伺服器。 **注意：** 請勿在任何內部網路介面上設定預設閘道。 | 若要設定 DirectAccess 伺服器連線到內部 IPv4 網路上的所有子網路，請執行下列動作：<br/><br/>-列出內部網路上所有位置的 IPv4 位址空間。<br/>-使用**route add-p**或**netsh interface ipv4 add route**命令來新增 ipv4 位址空間，做為 DirectAccess 伺服器之 ipv4 路由表中的靜態路由。 |
    | IPv6 網際網路與 IPv6 內部網路 | 設定下列各項：<br/><br/>-使用 ISP 所提供的位址設定。<br/>-使用**Route Print**命令來確保預設的 ipv6 路由存在，並且指向 ipv6 路由表中的 ISP 路由器。<br/>-判斷 ISP 和內部網路路由器是否使用 RFC 4191 中所述的預設路由器喜好設定，以及使用高於您近端內部網路路由器的預設喜好設定。<br/>    如果這兩項都是肯定的，預設路由就不需要其他設定。 ISP 路由器的喜好設定等級較高時，可確保 DirectAccess 伺服器的作用中預設 IPv6 路由指向 IPv6 網際網路。<br/><br/>由於 DirectAccess 伺服器是 IPv6 路由器，因此如果您有原生的 IPv6 基礎結構，網際網路介面也可以連線到內部網路上的網域控制站。 在此情況下，請將封包篩選器新增到周邊網路中的網域控制站，以防止連線到 DirectAccess 伺服器之網際網路對向介面的 IPv6 位址。 | 設定下列各項：<br/><br/>-如果您不是使用預設的喜好設定層級，您可以使用下列命令：**netsh interface ipv6 Set InterfaceIndex ignoredefaultroutes = enabled**來設定內部網路介面。<br/>    這個命令可確保指向內部網路路由器的其他預設路由將不會新增到 IPv6 路由表。 您可以使用下列命令來取得您內部網路介面的介面索引：**netsh interface ipv6 show interface**。 | 當您有 IPv6 內部網路時，若要設定 DirectAccess 伺服器來連線到所有 IPv6 位置，請執行下列動作：<br/><br/>-列出內部網路上所有位置的 IPv6 位址空間。<br/>-使用**netsh interface ipv6 add route**命令來新增 ipv6 位址空間，做為 DirectAccess 伺服器之 ipv6 路由表中的靜態路由。 |
    | IPv4 網際網路和 IPv6 內部網路 | DirectAccess 伺服器會透過 Microsoft 6to4 介面卡，將預設的 IPv6 路由流量轉送到 IPv4 網際網路上的 6to4 轉送。 您可以使用下列命令為 Microsoft 6to4 介面卡的 IPv4 位址設定 DirectAccess 伺服器： `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled` 。 | | |

    > [!NOTE]
    > - 如果 DirectAccess 用戶端已被指派公用的 IPv4 位址，它會使用 6to4 轉換技術連線到內部網路。 如果它被指派私人 IPv4 位址，則會使用 Teredo。 如果 DirectAccess 用戶端無法使用 6to4 或 Teredo 連線到 DirectAccess 伺服器，則會使用 IP-HTTPS。
    > - 若要使用 Teredo，您必須在對外網路介面卡上設定兩個連續的 IP 位址。
    > - 如果 DirectAccess 伺服器只有一張網路介面卡，您就無法使用 Teredo。
    > - 原生 IPv6 用戶端電腦不需要任何轉換技術，即可透過原生 IPv6 連線到 DirectAccess 伺服器。

### <a name="112-plan-ipv6-intranet-connectivity"></a>1.1.2 規劃 IPv6 內部網路連線

若要管理遠端 DirectAccess 用戶端，就必須使用 IPv6。 IPv6 可讓 DirectAccess 管理伺服器連線到位於網際網路上的 DirectAccess 用戶端來進行遠端管理。

> [!NOTE]
> - 若要支援 DirectAccess 用戶端電腦對您組織網路上的 IPv4 資源起始的連線，並不需要在您的網路上使用 IPv6。 針對這個目的，會使用 NAT64/DNS64。
> - 如果您並不管理遠端 DirectAccess 用戶端，就不需要部署 IPv6。
> - 在 DirectAccess 部署中不支援內部網站自動通道定址通訊協定 (ISATAP)。
> - 使用 IPv6 時，您可以使用下列 Windows PowerShell 命令啟用 DNS64 的 IPv6 主機 (AAAA) 資源記錄查詢：**Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**。

### <a name="113-plan-for-force-tunneling"></a>1.1.3 為強制通道做規劃

使用 IPv6 和「名稱解析原則表格」(NRPT) 時，DirectAccess 用戶端預設會分隔其內部網路和網際網路流量，如下所示：

- DNS 名稱會查詢內部網路完整網域名稱 (FQDN)，並透過在 DirectAccess 伺服器建立的通道，或直接與內部網路伺服器，交換所有內部網路流量。 來自 DirectAccess 用戶端的內部網路流量是 IPv6 流量。

- DNS 名稱會查詢與豁免規則對應或不符合內部網路命名空間的 FQDN，並透過連線到網際網路的實體介面，交換所有連到網際網路伺服器的流量。 來自 DirectAccess 用戶端的內部網路流量通常是 IPv4 流量。

相對地，根據預設，某些遠端存取虛擬私人網路 (VPN) 實作 (包括 VPN 用戶端) 會透過遠端存取 VPN 連線，傳送所有內部網路和網際網路流量。 VPN 伺服器會將網際網路繫結流量路由傳送到內部網路 IPv4 Web Proxy 伺服器，以存取 IPv4 網際網路資源。 使用分割通道來分隔遠端存取 VPN 用戶端的內部網路和網際網路流量是可行的。 這牽涉到在 VPN 用戶端上設定「網際網路通訊協定」(IP) 路由表，以便透過 VPN 連線傳送連到內部網路位置的流量，以及使用連線到網際網路的實體介面傳送連到所有其他位置的流量。

您可以使用強制通道設定 DirectAccess 用戶端透過通往 DirectAccess 伺服器的通道傳送所有流量。 已設定強制通道時，DirectAccess 用戶端會偵測到它們是在網際網路上，然後將其 IPv4 預設路由移除。 除了本機子網路流量以外，DirectAccess 用戶端所傳送的所有流量都是透過通道前往 DirectAccess 伺服器的 IPv6 流量。

> [!IMPORTANT]
> 如果您打算使用強制通道，或可能在未來新增它，就應該部署雙通道設定。 基於安全性考量，在單一通道設定中不支援強制通道。

啟用強制通道的後果如下：

- DirectAccess 用戶端只會使用「透過安全超文字傳輸通訊協定的網際網路通訊協定」(IP-HTTPS)，來透過 IPv4 網際網路取得對 DirectAccess 伺服器的 IPv6 連線能力。

- DirectAccess 用戶端預設可以透過 IPv4 流量連線的唯一位置就是其本機子網路上的位置。 在 DirectAccess 用戶端上執行的應用程式和服務所傳送的所有其他流量都是 IPv6 流量，這些流量皆透過 DirectAccess 連線傳送。 因此，DirectAccess 用戶端上只支援 IPv4 的應用程式無法用來連線到網際網路資源 (除了本機子網路上的資源之外)。

> [!IMPORTANT]
> 針對透過 DNS64 和 NAT64 的通道強制，必須實作 IPv6 網際網路連線。 其中一個可達成此目的的方法就是將 IP-HTTPS 首碼設定為可全域路由傳送，以便能夠透過 IPv6 連線到 ipv6.msftncsi.com，以及透過 DirectAccess 伺服器傳回網際網路伺服器對 IP-HTTPS 用戶端的回應。
>
> 由於這在大多數情況下並不可行，因此最好的選擇是在企業網路內建立虛擬 NCSI 伺服器，如下所示：
>
> 1. 為 ipv6.msftncsi.com 新增一個 NRPT 項目，並依據 DNS64 將它解析為內部網站 (可以是 IPv4 網站)。
> 2. 為 dns.msftncsi.com 新增一個 NRPT 項目，並在公司 DNS 伺服器解析它，以傳回 IPv6 主機 (AAAA) 資源記錄 fd3e:4f5a:5b81::1。 (只使用 DNS64 來傳送這個 FQDN 的主機 (A) 資源記錄的查詢可能沒有作用，因為它是在僅支援 IPv4 的部署中設定的，因此您應該將它設定為直接在公司 DNS 解析)。

## <a name="12-plan-firewall-requirements"></a>1.2 規劃防火牆需求

如果 DirectAccess 伺服器是在邊緣防火牆後面，當 DirectAccess 伺服器位於 IPv4 網際網路上時，必須為「遠端存取」流量設定下列例外：

- Teredo 流量-使用者資料包協定（UDP）目的地埠3544輸入，以及 UDP 來源埠3544輸出。

- 6to4 流量-IP 通訊協定41輸入和輸出。

- IP-HTTPS-傳輸控制通訊協定（TCP）目的地埠443，以及 TCP 來源埠443輸出。

- 如果您是以單一網路介面卡部署「遠端存取」，並且將網路位置伺服器安裝在 DirectAccess 伺服器上，則也應該豁免 TCP 連接埠 62000。

    > [!NOTE]
    > 這個豁免是在 DirectAccess 伺服器上，而所有其他豁免則是在邊緣防火牆上。

針對 Teredo 和 6to4 流量，應該對 DirectAccess 伺服器上的兩個網際網路對向連續公用 IPv4 位址都套用這些例外。 針對 IP-HTTPS，則必須在已在公用 DNS 伺服器上登錄的位址上套用例外。

當 DirectAccess 伺服器位於 IPv6 網際網路上時，必須為「遠端存取」流量設定下列例外：

- IP 通訊協定識別碼 50

- UDP 目的地連接埠 500 輸入和 UDP 來源連接埠 500 輸出。

- ICMPv6 流量輸入和輸出 (僅使用 Teredo 時)

使用額外的防火牆時，請為「遠端存取」流量套用下列內部網路防火牆例外：

- ISATAP-通訊協定41輸入和輸出

- 所有 IPv4 和 IPv6 流量的 TCP/UDP

- 所有 IPv4 和 IPv6 流量的 ICMP (僅使用 Teredo 時)

## <a name="13-plan-certificate-requirements"></a>1.3 規劃憑證需求

部署單一 DirectAccess 伺服器時，有三種情況需要憑證：

- [1.3.1 規劃用於 IPsec 驗證的電腦憑證](#131-plan-computer-certificates-for-ipsec-authentication)

    IPsec 的憑證需求包括 DirectAccess 用戶端電腦在用戶端與 DirectAccess 伺服器之間建立 IPsec 連線時所使用的電腦憑證，以及 DirectAccess 伺服器用來與 DirectAccess 用戶端建立 IPsec 連線的電腦憑證。

    對於 Windows Server 2012 中的 DirectAccess，使用這些 IPsec 憑證並非必要。 DirectAccess 伺服器也可以做為 Kerberos Proxy 來執行 IPsec 驗證，而不需要憑證。 如果使用 Kerberos 通訊協定，它會透過 SSL 運作，而 Kerberos Proxy 會使用針對此目的為 IP-HTTPS 設定的憑證。 某些企業案例 (包括多站台部署和單次密碼 (OTP) 用戶端驗證) 需要使用的是憑證驗證，而不是 Kerberos 通訊協定。

-   [1.3.2 規劃 IP-HTTPS 的憑證](#132-plan-certificates-for-ip-https)

    當您設定「遠端存取」時，DirectAccess 伺服器會自動設定為做為 IP-HTTPS 接聽程式。 IP-HTTPS 站台需要有網站憑證，而用戶端電腦必須要能夠連線到憑證撤銷清單 (CRL) 站台來查看該憑證是否在清單中。

-   [1.3.3 規劃網路位置伺服器的網站憑證](#133-plan-website-certificates-for-the-network-location-server)

    網路位置伺服器是一個用來偵測用戶端電腦是否位於公司網路中的網站。 網路位置伺服器需要網站憑證。 DirectAccess 用戶端必須要能夠連線到 CRL 站台來查看該憑證是否在清單中。

下表摘要說明每個案例的憑證授權單位 (CA) 需求。

|IPsec 驗證|IP-HTTPS 伺服器|網路位置伺服器|
|------------|----------|--------------|
|當您不使用 Kerberos proxy 進行驗證時，需要內部 CA 將電腦憑證發行至 DirectAccess 伺服器和用戶端進行 IPsec 驗證|內部 CA：<br/><br/>您可以使用內部 CA 來簽發 IP-HTTPS 憑證；不過，您必須確定外部可以使用 CRL 發佈點。|內部 CA：<br/><br/>您可以使用內部 CA 來簽發網路位置伺服器網站憑證。 請確定 CRL 發佈點在內部網路具有高可用性。|
||自我簽署憑證：<br/><br/>您可以將自我簽署憑證用於 IP-HTTPS 伺服器；不過，您必須確定外部可以使用 CRL 發佈點。<br/><br/>自我簽署憑證無法在多站台部署中使用。|自我簽署憑證：<br/><br/>您可以將自我簽署憑證用於網路位置伺服器網站。<br/><br/>自我簽署憑證無法在多站台部署中使用。|
||**建議需求**<br/><br/>公用 CA：<br/><br/>建議使用公用 CA 來簽發 IP-HTTPS 憑證。 這可確保外部可以使用 CRL 發佈點。|

### <a name="131-plan-computer-certificates-for-ipsec-authentication"></a>1.3.1 規劃用於 IPsec 驗證的電腦憑證
如果您使用憑證式 IPsec 驗證，DirectAccess 伺服器和用戶端都必須取得電腦憑證。 安裝憑證的最簡單方式就是為電腦憑證設定群組原則型自動註冊。 這可確保所有網域成員都會從企業 CA 取得憑證。 如果您的組織中並未設定企業 CA，請參閱 [Active Directory 憑證服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10))。

這個憑證具有下列需求：

-   憑證應該具有「用戶端驗證」擴充金鑰使用方法 (EKU)。

-   用戶端憑證和伺服器憑證應該鏈結到相同的根憑證。 在 DirectAccess 組態設定中必須選取這個根憑證。

### <a name="132-plan-certificates-for-ip-https"></a>1.3.2 規劃 IP-HTTPS 的憑證
DirectAccess 伺服器會做為 IP-HTTPS 接聽程式，而您必須手動在伺服器上安裝 HTTPS 網站憑證。 規劃時，請考量下列各項：

-   建議使用公用 CA，以便讓憑證撤銷清單 (CRL) 立即可用。

-   在 [主體]**** 欄位中，指定 DirectAccess 伺服器之網際網路介面卡的 IPv4 位址，或 IP-HTTPS URL (ConnectTo 位址) 的 FQDN。 如果 DirectAccess 伺服器位於 NAT 裝置後面，應該指定 NAT 裝置的公用名稱或位址。

-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。

-   在 [增強金鑰使用方法]**** 欄位中，使用伺服器驗證物件識別碼 (OID)。

-   在 [CRL 發佈點]**** 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。

-   IP-HTTPS 憑證必須具有私密金鑰。

-   IP-HTTPS 憑證必須直接匯入到個人存放區。

-   IP-HTTPS 憑證的名稱中可以包含萬用字元。

如果您打算在非標準連接埠上使用 IP-HTTPS，請在 DirectAccess 伺服器上執行下列步驟：

1.  移除 0.0.0.0:443 的現有憑證繫結，然後以您所選連接埠的憑證繫結取代它。 針對這個範例的目的，使用了連接埠 44500。 在刪除憑證繫結之前，請先顯示和複製 **appid**。

    1.  若要刪除憑證繫結，請輸入：

        ```
        netsh http delete ssl ipport=0.0.0.0:443
        ```

    2.  若要新增憑證繫結，請輸入：

        ```
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>
        ```

2.  若要修改伺服器上的 IP-HTTPS URL，請輸入：

    ```
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS
    ```

    ```
    Net stop iphlpsvc & net start iphlpsvc
    ```

3.  變更 kdcproxy 的 URL 保留項目。

    1.  若要刪除現有的 URL 保留項目，請輸入：

        ```
        netsh http del urlacl url=https://+:443/KdcProxy/
        ```

    2.  若要新增 URL 保留項目，請輸入：

        ```
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)
        ```

4.  新增可讓 kppsvc 在非標準連接埠進行接聽的設定。 若要新增登錄項目，請輸入：

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f
    ```

5.  若要在網域控制站上重新啟動 kdcproxy 服務，請輸入：

    ```
    net stop kpssvc & net start kpssvc
    ```

若要在非標準連接埠上使用 IP-HTTPS，請在網域控制站上執行下列步驟：

1.  修改用戶端 GPO 中的 IP-HTTPS 設定。

    1.  開啟 [群組原則編輯器] 。

    2.  瀏覽至 [電腦設定] => [原則] => [系統管理範本] => [網路] => [TCPIP 設定值] => [IPv6 轉換技術]。

    3.  開啟 IP-HTTPS 狀態設定，並將 URL 變更為 **https://<DirectAccess 伺服器名稱 (例如 server.contoso.com)>: 44500/IPHTTPS**。

    4.  按一下 [套用]。

2.  修改用戶端 GPO 中的 Kerberos Proxy 用戶端設定。

    1.  在 [群組原則編輯器] 中，瀏覽至 [電腦設定] => [原則] => [系統管理範本] => [系統] => [Kerberos] = > 指定 Kerberos 用戶端的 KDC Proxy 伺服器。

    2.  開啟 IPHTTPS 狀態設定，並將 URL 變更為 **https://<DirectAccess 伺服器名稱 (例如 server.contoso.com)>: 44500/IPHTTPS**。

    3.  按一下 [套用]。

3.  修改用戶端 IPsec 原則設定以使用 ComputerKerb 和 UserKerb。

    1.  在 [群組原則編輯器] 中，瀏覽至 [電腦設定] => [原則] => [Windows 設定] => [安全性設定] => [具有進階安全性的 Windows 防火牆]。

    2.  按一下 [連線安全性規則]****，然後按兩下 [IPsec 規則]****。

    3.  在 [驗證]**** 索引標籤中，按一下 [進階]****。

    4.  針對 Auth1：移除現有的驗證方法，然後以 ComputerKerb 取代它。 對於 Auth2：移除現有的驗證方法，然後以 UserKerb 取代它。

    5.  按一下 [套用]****，然後按一下 [確定]****。

若要完成使用 IP-HTTPS 非標準連接埠的手動程序，請在用戶端電腦和 DirectAccess 伺服器上執行 **gpupdate /force**。

### <a name="133-plan-website-certificates-for-the-network-location-server"></a>1.3.3 規劃網路位置伺服器的網站憑證
為網路位置伺服器網站做規劃時，請考量下列各項：

-   在 [主體]**** 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。

-   在 [增強金鑰使用方法]**** 欄位中，使用「伺服器驗證 OID」。

-   在 [CRL 發佈點]**** 欄位中，使用連線到內部網路的 DirectAccess 用戶端可存取的 CRL 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。

-   如果您稍後打算要設定多站台或叢集部署，憑證名稱就不應該與任何將新增到部署中的 DirectAccess 伺服器的內部名稱相符。

    > [!NOTE]
    > 請確定 IP-HTTPS 和網路位置伺服器的憑證具有**主體名稱**。 如果憑證沒有**主體名稱**，但是有**別名**，「遠端存取精靈」將不會接受它。

## <a name="14-plan-dns-requirements"></a>1.4 規劃 DNS 需求
本節說明「遠端存取」部署中 DirectAccess 用戶端要求和基礎結構伺服器的 DNS 需求。 它包含以下各小節：

-   [1.4.1 為 DNS 伺服器需求做規劃](#141-plan-for-dns-server-requirements)

-   [1.4.2 為本機名稱解析做規劃](#142-plan-for-local-name-resolution)

**DirectAccess 用戶端要求**

DNS 會被用來解析來自不位於內部 (或公司) 網路上的 DirectAccess 用戶端電腦的要求。 DirectAccess 用戶端會嘗試連線到 DirectAccess 網路位置伺服器，以判斷它們是位於網際網路上或是內部網路上。

-   如果連線成功，用戶端會被識別為位於內部網路上，系統便不會使用 DirectAccess，而會使用在用戶端電腦的網路介面卡上設定的 DNS 伺服器來解析用戶端要求。

-   如果連線不成功，用戶端就會被視為位於網際網路上，而 DirectAccess 用戶端將會使用名稱解析原則表格 (NRPT) 來判斷要使用哪一個 DNS 伺服器來解析名稱要求。

您可以指定用戶端使用 DirectAccess DNS64 或替代的內部 DNS 伺服器來解析名稱。 執行名稱解析時，DirectAccess 用戶端會使用 NRPT 來識別如何處理要求。 用戶端會要求 FQDN 或單一標籤名稱，例如 <https://internal> 。 如果要求的是單一標籤名稱，系統就會附加 DNS 尾碼來建立 FQDN。 如果 DNS 查詢與 NRPT 中的項目相符，而且已為該項目指定 DNS64 或內部網路上的 DNS 伺服器，系統就會將查詢傳送給指定的伺服器進行名稱解析。 如果有相符的項目存在，但是未指定任何 DNS 伺服器，這即表示有豁免規則，而將會套用一般名稱解析。

> [!NOTE]
> 請注意，在「遠端存取管理主控台」中將新尾碼新增到 NRPT 中時，只要按一下 [偵測]****，即可自動探索到該尾碼的預設 DNS 伺服器。

自動偵測的運作方式如下：

-   如果公司網路是 IPv4 型，或是使用 IPv4 與 IPv6，預設位址就是 DirectAccess 伺服器上內部介面卡的 DNS64 位址。

-   如果公司網路是 IPv6 型，預設位址就是公司網路中 DNS 伺服器的 IPv6 位址。

**基礎結構伺服器**

-   **網路位置伺服器**

    DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。 此外，當您設定「遠端存取」時，系統會自動建立下列規則：

    -   DNS 尾碼規則 - 用於根網域或 DirectAccess 伺服器的網域名稱，以及與 DNS64 位址對應的 IPv6 位址。 在僅支援 IPv6 的公司網路中，是在 DirectAccess 伺服器上設定內部網路 DNS 伺服器。 例如，如果 DirectAccess 伺服器是 corp.contoso.com 網域的成員，就會為 corp.contoso.com DNS 尾碼建立規則。

    -   網路位置伺服器之 FQDN 的豁免規則。 例如，如果網路位置伺服器 URL 是 <https://nls.corp.contoso.com> ，就會為 FQDN nls.corp.contoso.com 建立豁免規則。

-   **IP-HTTPS 伺服器**

    DirectAccess 伺服器會做為 IP-HTTPS 接聽程式，並使用其伺服器憑證向 IP-HTTPS 用戶端進行驗證。 IP-HTTPS 名稱必須能夠被使用公用 DNS 伺服器的 DirectAccess 用戶端解析。

-   **CRL 撤銷檢查**

    DirectAccess 會針對 DirectAccess 用戶端與 DirectAccess 伺服器之間的 IP-HTTPS 連線，以及 DirectAccess 用戶端與網路位置伺服器之間的 HTTPS 型連線，使用憑證撤銷檢查。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。

-   **ISATAP**

    ISATAP 可讓公司電腦取得 IPv6 位址，並且會將 IPv6 封包封裝在 IPv4 標頭內。 DirectAccess 伺服器會使用它，透過內部網路提供對 ISATAP 主機的 IPv6 連線能力。 在非原生 IPv6 網路環境中，DirectAccess 伺服器會自動將自己設定為 ISATAP 路由器。

    由於 DirectAccess 已不再支援 ISATAP，因此您必須確定您的 DNS 伺服器已設定為不回應 ISATAP 查詢。 DNS 伺服器服務預設會透過 DNS「全域查詢封鎖清單」封鎖 ISATAP 名稱的名稱解析。 請勿將 ISATAP 名稱從「全域查詢封鎖清單」中移除 。

-   **連線能力檢查器**

    「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 若要確保探查能夠如預期般運作，必須在 DNS 中手動登錄下列名稱：

    -   **directaccess-directaccess-webprobehost**-應解析為 directaccess 伺服器的內部 IPv4 位址，或解析為僅 ipv6 環境中的 ipv6 位址。

    -   **directaccess-directaccess-corpconnectivityhost**-應解析為本機主機（回送）位址。 應該會建立下列主機 (A) 和 (AAAA) 資源記錄：一個值為 127.0.0.1 的主機 (A) 資源記錄，以及一個值從 NAT64 首碼建構且最後 32 位元為 127.0.0.1 的主機 (AAAA) 資源記錄。 執行下列 Windows PowerShell 命令即可抓取 NAT64 首碼：**get-netnattransitionconfiguration**。

        > [!NOTE]
        > 這只適用於僅支援 IPv4 的環境。 在 IPv4 加 IPv6 或僅支援 IPv6 的環境中，只有主機 (AAAA) 資源記錄在建立時應該加上回送 IP 位址 ::1。

    您可以透過 HTTP 使用其他網址或使用 **ping**，來建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。

### <a name="141-plan-for-dns-server-requirements"></a>1.4.1 為 DNS 伺服器需求做規劃
以下是部署 DirectAccess 時的 DNS 需求。

-   針對 DirectAccess 用戶端，您必須使用執行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 或任何其他支援 IPv6 的 DNS 伺服器的 DNS 伺服器。

    > [!NOTE]
    > 在部署 DirectAccess 時，建議您不要使用執行 Windows Server 2003 的 DNS 伺服器。 雖然 Windows Server 2003 DNS 伺服器支援 IPv6 記錄，但 Microsoft 不再支援 Windows Server 2003。 此外，如果您的網域控制站因為檔案複寫服務問題而執行 Windows Server 2003，則不應部署 DirectAccess。 如需詳細資訊，請參閱 [DirectAccess 不支援的設定](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn464274(v=ws.11))。

-   使用支援動態更新的 DNS 伺服器。 您可以使用不支援動態更新的 DNS 伺服器，但是您必須手動更新這些伺服器上的項目。

-   必須可以使用 DNS 伺服器來解析可透過網際網路存取之 CRL 發佈點的 FQDN。 例如，如果 URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> 位於 DirectAccess 伺服器之 ip-HTTPs 憑證的 [ **CRL 發佈點**] 欄位中，您必須使用網際網路 DNS 伺服器確保 FQDN crld.contoso.com 可解析。

### <a name="142-plan-for-local-name-resolution"></a>1.4.2 為本機名稱解析做規劃
為本機名稱解析做規劃時，請考量下列問題：

**NRPT**

在下列情況下，您可能需要建立額外的 NRPT 規則：

-   如果您需要為內部命名空間新增更多 DNS 尾碼。

-   如果 CRL 發佈點的完整網域名稱 (FQDN) 是根據內部網路命名空間，您就必須為 CRL 發佈點的 FQDN 新增豁免規則。

-   如果您有拆分式 DNS 環境，針對您想要讓位於網際網路的 DirectAccess 用戶端存取網際網路版本而非內部網路版本的資源，您必須為其名稱新增豁免規則。

-   如果您是透過內部網路 Proxy 伺服器將流量重新導向到外部網站，該外部網站就只能從內部網路使用，而且它會使用 Web Proxy 伺服器的位址來允許輸入要求。 在此情況下，請為該外部網站的 FQDN 新增豁免規則，並且指定該規則使用內部網路 Web Proxy 伺服器，而不是使用內部網路 DNS 伺服器的 IPv6 位址。

    例如，如果您要測試名為 test.contoso.com 的外部網站，透過網際網路 DNS 伺服器並無法解析這個名稱，但是 Contoso Web Proxy 伺服器知道如何解析這個名稱，以及如何將網站的要求導向到外部網頁伺服器。 為了防止不在 Contoso 內部網路的使用者存取站台，該外部網站只會允許來自 Contoso Web Proxy 之 IPv4 網際網路位址的要求。 因此，內部網路使用者可以存取該網站，因為他們使用 Contoso Web Proxy，但是 DirectAccess 使用者無法存取，因為他們不是使用 Contoso Web Proxy。 藉由為使用 Contoso Web Proxy 的 test.contoso.com 設定 NRPT 豁免規則，即可透過 IPv4 網際網路將 test.contoso.com 的網頁要求路由傳送到內部網路 Web Proxy 伺服器。

**單一標籤名稱**

單一標籤名稱（例如 <https://paycheck> ）有時候會用於內部網路伺服器。 如果要求的是單一標籤名稱，並且已設定 DNS 尾碼搜尋清單，系統就會將清單中的 DNS 尾碼附加到單一標籤名稱。 例如，當使用者在網頁瀏覽器中屬於 corp.contoso.com 網欄位型別成員的電腦上時， <https://paycheck> 視為名稱的 FQDN 是 paycheck.corp.contoso.com。 附加的尾碼預設會根據用戶端電腦的主要 DNS 尾碼。

> [!NOTE]
> 在不相鄰的名稱空間案例中 (其中一或多部網域電腦有不符合電腦所屬 Active Directory 網域的 DNS 尾碼)，您應該確保搜尋清單已自訂為包含所有必要的尾碼。 「遠端存取精靈」預設會將 Active Directory DNS 名稱設定為用戶端上的主要 DNS 尾碼。 請務必新增用戶端用來進行名稱解析的 DNS 尾碼。

如果您組織中部署了多個網域和「Windows 網際網路名稱服務」(WINS)，而您是透過遠端連線，便可依照下列方式解析單一名稱：

-   在 DNS 中部署 WINS 正向對應區域。 在嘗試解析 computername.dns.zone1.corp.contoso.com 時，系統會將要求導向到只使用電腦名稱的 WINS 伺服器。 用戶端會認為它發出一般的 DNS 主機 (A) 資源記錄，但實際上是 NetBIOS 要求。 如需詳細資訊，請參閱＜管理正向對應區域＞。

-   將 DNS 尾碼 (例如 dns.zone1.corp.contoso.com) 新增到預設網域原則 GPO。

**拆分式 DNS**

拆分式 DNS 係指使用相同的 DNS 網域進行網際網路和內部網路名稱解析。

針對分裂式 DNS 部署，您必須列出在網際網路和內部網路上重複的 Fqdn，並決定 DirectAccess 用戶端應連線的資源-內部網路或網際網路版本。 以您想要讓 DirectAccess 用戶端連線到網際網路版本的資源來說，針對與該資源對應的每個名稱，您必須為 DirectAccess 用戶端將對應的 FQDN 新增到 NRPT 中做為豁免規則。

在拆分式 DNS 環境中，如果您想要讓資源的兩個版本都可供使用，請為您的內部網路資源設定替代名稱 (此名稱不可與網際網路上使用的名稱重複)，並指示使用者在內部網路上使用這個替代名稱。 例如，為內部名稱 www.contoso.com 設定 www.internal.contoso.com 替代名稱。

在非拆分式 DNS 環境中，網際網路命名空間會與內部網路命名空間不同。 例如，Contoso 公司在網際網路上使用 contoso.com，在內部網路上使用 corp.contoso.com。 由於所有內部網路資源都使用 corp.contoso.com DNS 尾碼，因此 corp.contoso.com 的 NRPT 規則會將內部網路資源的所有 DNS 名稱查詢都路由傳送到內部網路 DNS 伺服器。 尾碼為 contoso.com 之名稱的 DNS 名稱查詢與 NRPT 中 corp.contoso.com 內部網路命名空間規則不相符，因此會被傳送到網際網路 DNS 伺服器。 使用非拆分式 DNS 部署時，由於內部網路和網際網路資源的 FQDN 不會重複，因此不需要為 NRPT 進行任何額外的設定。 DirectAccess 用戶端既可存取組織的網際網路資源，也可存取其內部網路資源。

**DirectAccess 用戶端的本機名稱解析行為**

如果無法使用 DNS 解析名稱，若要解析本機子網上的名稱，Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows 8 和 Windows 7 中的 DNS 用戶端服務可以使用本機名稱解析，並搭配連結本機多播名稱解析（LLMNR）和 NetBIOS over TCP/IP 通訊協定。

當電腦位於私人網路 (例如單一子網路的家用網路) 時，通常需要本機名稱解析，才能進行對等連線。 當 DNS 用戶端服務執行內部網路伺服器名稱的本機名稱解析，並且電腦連線到網際網路上的共用子網路時，惡意使用者將可以擷取 LLMNR 和 NetBIOS over TCP/IP 訊息來判斷內部網路伺服器名稱。 在 [基礎結構伺服器安裝精靈] 的 [DNS] 頁面上，您可以根據從內部網路 DNS 伺服器收到的回應類型，設定本機名稱解析行為。 有下列選項可供使用：

-   **如果名稱不存在於 DNS，則使用本機名稱解析**。 這是最安全的選項，因為 DirectAccess 用戶端只會針對內部網路 DNS 伺服器無法解析的伺服器名稱執行本機名稱解析。 如果可以連線到內部網路 DNS 伺服器，就會解析內部網路伺服器的名稱。 如果無法連線到內部網路 DNS 伺服器，或是有其他類型的 DNS 錯誤，也不會透過本機名稱解析將內部網路伺服器名稱遺漏到子網路。

-   **若名稱不存在於 DNS 或無法連絡 DNS 伺服器 (當用戶端電腦位於私人網路)，則使用本機名稱解析 (建議選項)**。 這是建議使用的選項，因為它可允許只在無法連線到內部網路 DNS 伺服器時，才在私人網路上使用本機名稱解析。

-   **對於任何類型的 DNS 解析錯誤，都使用本機名稱解析 (最不安全)**。 這是最不安全的選項，因為可能透過本機名稱解析將內部網路伺服器的名稱洩露到本機子網路。

## <a name="15-plan-the-network-location-server"></a>1.5 規劃網路位置伺服器
網路位置伺服器是一個用來偵測 DirectAccess 用戶端是否位於公司網路中的網站。 公司網路中的用戶端不會使用 DirectAccess 來連線到內部資源，而是會直接連線。

網路位置伺服器網站可以裝載在 DirectAccess 伺服器上，或是組織中的另一部伺服器上。 如果您將網路位置伺服器裝載在 DirectAccess 伺服器上，當您安裝「遠端存取」伺服器角色時，系統就會自動建立該網站。 如果您將網路位置伺服器裝載在組織中執行 Windows 作業系統的另一部伺服器上，您必須確定該伺服器上已安裝 Internet Information Services (IIS)，並且已建立該網站。 DirectAccess 不會在遠端網路位置伺服器上設定設定值。

請確保網路位置伺服器網站符合下列需求：

-   它是具有 HTTPS 伺服器憑證的網站。

-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。

-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器站台的名稱。

-   網路位置伺服器站台必須對內部網路上的電腦保持高可用性。

-   網路位置伺服器必須不可被網際網路上的 DirectAccess 用戶端電腦存取。

-   必須依據 CRL 檢查伺服器憑證。

### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 規劃網路位置伺服器的憑證
當您在取得要用於網路位置伺服器的網站憑證時，請考量下列各項：

1.  在 [主體]**** 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。

2.  在 [增強金鑰使用方法]**** 欄位中，使用「伺服器驗證 OID」。

3.  在 [CRL 發佈點]**** 欄位中，使用連線到內部網路的 DirectAccess 用戶端可存取的 CRL 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。

### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 規劃網路位置伺服器的 DNS
DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。

## <a name="16-plan-management-servers"></a>1.6 規劃管理伺服器
DirectAccess 用戶端會起始與提供服務 (例如 Windows Update 和防毒更新) 之管理伺服器的通訊。 DirectAccess 用戶端也會在存取內部網路之前，先使用 Kerberos 通訊協定連線到網域控制站來進行驗證。 在進行 DirectAccess 用戶端遠端管理時，管理伺服器會與用戶端電腦進行通訊來執行管理功能 (例如軟體或硬體清查評定)。 「遠端存取」可以自動探索某些管理伺服器，包括：

-   網域控制站-會針對與 DirectAccess 伺服器和用戶端電腦位於相同樹系中的所有網域，執行網域控制站的自動探索。

-   Microsoft 端點 Configuration Manager 伺服器-針對與 DirectAccess 伺服器和用戶端電腦相同樹系中的所有網域，執行 Configuration Manager 伺服器的自動探索。

第一次設定 DirectAccess 時，會自動偵測網域控制站和 Configuration Manager 伺服器。 偵測到的網域控制站不會顯示在主控台中，但是您可以使用 Windows PowerShell Cmdlet **DAMgmtServer-Type All**來抓取設定。 如果網域控制站或 Configuration Manager 伺服器遭到修改，按一下 [遠端存取管理] 主控台中的 [重新整理**管理伺服器**]，會重新整理管理伺服器清單。

**管理伺服器需求**

-   管理伺服器必需是可透過第一個 (基礎結構) 通道存取的伺服器。 設定「遠端存取」時，只要將伺服器新增到管理伺服器清單中，就會自動將它們設定為可透過這個通道存取。

-   對 DirectAccess 用戶端起始連線的管理伺服器必須完全支援 IPv6 (藉由原生 IPv6 位址或使用 ISATAP 所指派的位址)。

## <a name="17-plan-active-directory-domain-services"></a>1.7 規劃 Active Directory 網域服務
本節說明 DirectAccess 如何使用 Active Directory 網域服務 (AD DS)，其中包括下列各小節：

-   [1.7.1 規劃用戶端驗證](#171-plan-client-authentication)

-   [1.7.2 規劃多個網域](#172-plan-multiple-domains)

DirectAccess 會使用 AD DS 並 Active Directory 群組原則物件（Gpo），如下所示：

-   **驗證**

    AD DS 會用於驗證。 基礎結構通道針對連線到 DirectAccess 伺服器的電腦帳戶會使用 NTLMv2 驗證，而該帳戶必須列在 Active Directory 網域中。 內部網路通道會使用 Kerberos 驗證來讓使用者建立第二個通道。

-   **群組原則物件**

    DirectAccess 會將組態設定收集到套用至 DirectAccess 伺服器、用戶端及內部應用程式伺服器的 GPO 中。

-   **安全性群組**

    DirectAccess 會使用安全性群組來收集和識別 DirectAccess 用戶端電腦。 GPO 會套用到所需的安全性群組。

-   **延伸的 IPsec 原則**

    DirectAccess 可以在用戶端與 DirectAccess 伺服器之間使用 IPsec 驗證和加密。 您可以將 IPsec 驗證和加密從用戶端延伸到指定的內部應用程式伺服器。 若要這麼做，請將所需的應用程式伺服器新增到安全性群組。

**AD DS 需求**

為 DirectAccess 部署規劃 AD DS 時，請考量下列需求：

-   至少須有一個網域控制站安裝在 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 作業系統上。

    如果網域控制站位於周邊網路 (因而可從 DirectAccess 伺服器的網際網路對向網路介面卡連線到它)，您就必須在網域控制站上新增封包篩選器來防止對網際網路介面卡 IP 位址的連線，以防止 DirectAccess 伺服器連線到它。

-   DirectAccess 伺服器必須是網域成員。

-   DirectAccess 用戶端必須是網域成員。 用戶端可以屬於：

    -   與 DirectAccess 伺服器位於相同樹系中的任何網域。

    -   與 DirectAccess 伺服器網域具有雙向信任關係的任何網域。

    -   與 DirectAccess 網域所屬樹系具有雙向信任關係之樹系中的任何網域。

> [!NOTE]
> -   DirectAccess 伺服器不可以是網域控制站。
> -   用於 DirectAccess 的 AD DS 網域控制站不可以是能從 DirectAccess 伺服器的外部網際網路介面卡連到的網域控制站 (也就是說，介面卡不得在 Windows 防火牆的網域設定檔中)。

### <a name="171-plan-client-authentication"></a>1.7.1 規劃用戶端驗證
DirectAccess 可讓您選擇使用憑證來進行 IPsec 電腦驗證，或使用內建的 Kerberos Proxy 以使用者名稱和密碼來進行驗證。

選擇使用 AD DS 認證來進行驗證時，DirectAccess 會使用一個利用「電腦 Kerberos」進行第一次驗證和「使用者 Kerberos」進行第二次驗證的安全性通道。 使用此模式來進行驗證時，DirectAccess 會使用一個可存取 DNS 伺服器、網域控制站及內部網路上其他伺服器的單一安全性通道。

選擇使用雙因素驗證或「網路存取保護」時，DirectAccess 會使用兩個安全性通道。 「遠端存取安裝精靈」會設定「具有進階安全性的 Windows 防火牆」連線安全性規則，這些規則會指定當為連到 DirectAccess 伺服器的通道交涉 IPsec 安全性關聯時，使用下列類型的認證：

-   基礎結構通道會使用「電腦 Kerberos」認證進行第一次驗證，並使用「使用者 Kerberos」進行第二次驗證。

-   內部網路通道會使用電腦憑證認證進行第一次驗證，並使用「使用者 Kerberos」進行第二次驗證。

當 DirectAccess 選擇允許存取執行 Windows 7 或在多網站部署中的用戶端時，它會使用兩個安全性通道。 「遠端存取安裝精靈」會設定「具有進階安全性的 Windows 防火牆」連線安全性規則，這些規則會指定當為連到 DirectAccess 伺服器的通道交涉 IPsec 安全性關聯時，使用下列類型的認證：

-   基礎結構通道會使用電腦憑證認證進行第一次驗證，並使用 NTLMv2 進行第二次驗證。 NTLMv2 認證會強制使用「已驗證網際網路通訊協定」(AuthIP)，並且會先提供對 DNS 伺服器和網域控制站的存取權，這樣 DirectAccess 用戶端才能將 Kerberos 認證用於內部網路通道。

-   內部網路通道會使用電腦憑證認證進行第一次驗證，並使用「使用者 Kerberos」進行第二次驗證。

### <a name="172-plan-multiple-domains"></a>1.7.2 規劃多個網域
管理伺服器清單應該包括來自所有含安全性群組之網域的網域控制站，其中這些安全性群組皆包括 DirectAccess 用戶端電腦。 它應該包含所有含使用者帳戶的網域，其中這些使用者帳戶皆可能使用設定為 DirectAccess 用戶端的電腦。 這可確保當使用者不是與所使用的用戶端電腦位於相同網域時，系統會以使用者網域中的網域控制站來驗證使用者。 如果網域位於相同的樹系中，便會自動執行此動作。

> [!NOTE]
> 如果有安全性群組中的電腦被用來做為不同樹系中的用戶端電腦或應用程式伺服器，系統並不會自動偵測這些樹系的網域控制站。 您可以在 [遠端存取管理主控台] 中執行 [重新整理管理伺服器]**** 工作來偵測這些網域控制站。

進行「遠端存取部署」時，可能的話，應該將通用的網域名稱尾碼新增到「名稱解析原則表格」(NRPT) 中。 例如，如果您有 domain1.corp.contoso.com 和 domain2.corp.contoso.com 這兩個網域，您可以不用將兩個項目新增到 NRPT 中，而是新增一個通用的 DNS 尾碼項目 (其中網域名稱尾碼為 corp.contoso.com)。 如果網域位於相同的根目錄，這會自動新增，但如果網域不是位於相同的根目錄，則必須手動新增。

如果「Windows 網際網路名稱服務」(WINS) 是部署在多網域環境中，您就必須在 DNS 中部署 WINS 正向對應區域。 如需詳細資訊，請參閱本檔前面的「[本機名稱解析的1.4.2 計畫](#142-plan-for-local-name-resolution)」一節中的**單一標籤名稱**。

## <a name="18-plan-group-policy-objects"></a>1.8 規劃群組原則物件
本節說明「群組原則物件」(GPO) 在您「遠端存取」基礎結構中扮演的角色，其中包括下列各小節：

-   [1.8.1 設定自動建立的 GPO](#181-configure-automatically-created-gpos)

-   [1.8.2 設定手動建立的 GPO](#182-configure-manually-created-gpos)

-   [1.8.3 在多網域控制站環境中管理 GPO](#183-manage-gpos-in-a-multi-domain-controller-environment)

-   [1.8.4 使用有限的權限管理遠端存取 GPO](#184-manage-remote-access-gpos-with-limited-permissions)

-   [1.8.5 從已刪除的 GPO 復原](#185-recover-from-a-deleted-gpo)

您設定「遠端存取」時所設定的 DirectAccess 設定會被收集到 GPO 中。 下列類型的 GPO 會被填入 DirectAccess 設定，並依下列方式分配：

-   **DirectAccess 用戶端 GPO**

    這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 項目，以及「具有進階安全性的 Windows 防火牆」連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。

-   **DirectAccess 伺服器 GPO**

    這個 GPO 包含 DirectAccess 組態設定，這些設定會套用到在您的部署中被設定為 DirectAccess 伺服器的任何伺服器。 它也包含「具有進階安全性的 Windows 防火牆」連線安全性規則。

-   **應用程式伺服器 GPO**

    這個 GPO 包含所選應用程式伺服器的設定，這些伺服器是您選擇從 DirectAccess 用戶端延伸驗證和加密的伺服器。 如果沒有延伸驗證和加密，就不會使用這個 GPO。

設定 GPO 的方式有兩種：

-   **自動**-您可以指定自動建立它們。 系統會為每個 GPO 指定一個預設名稱。

-   **手動**-您可以使用由 Active Directory 系統管理員預先定義的 gpo。

> [!NOTE]
> 在設定 DirectAccess 使用特定的 GPO 之後，就無法再設定它使用不同的 GPO。

不論您使用自動還是手動設定的 GPO，只要您的用戶端會使用 3G 網路，您就需要新增低速連結偵測原則。 **原則：設定群組原則低速連接偵測**的路徑為：**Computer configuration/Polices/Administrative Templates/System/Group Policy**。

> [!CAUTION]
> 在您執行 DirectAccess Cmdlet 之前，請使用下列程序來備份所有「遠端存取群組原則物件 (GPO)」：[備份並還原遠端存取設定](https://go.microsoft.com/fwlink/?LinkID=257928)。

如果沒有正確的權限 (列在接下來的小節中) 來連結 GPO，系統會發出警告。 「遠端存取」操作將會繼續，但是不會建立連結。 如果系統發出這個警告，則即使稍後新增權限，也不會自動建立連結。 系統管理員將必須改為手動建立連結。

### <a name="181-configure-automatically-created-gpos"></a>1.8.1 設定自動建立的 GPO
使用自動建立的 GPO 時，請考量下列各項。

自動建立的 GPO 會根據位置和連結目標參數進行套用，如下所示：

-   以 DirectAccess 伺服器 GPO 來說， 位置參數和連結參數會指向包含 DirectAccess 伺服器的網域。

-   當用戶端和應用程式伺服器 GPO 建立時，位置會設定為將建立 GPO 的單一網域。 系統會在每個網域中查詢 GPO 名稱，如果存在的話，就會將 DirectAccess 設定填入它。 連結目標會設定為在其中建立 GPO 的網域根目錄。 系統會為每個包含用戶端電腦或應用程式伺服器的網域建立 GPO，而該 GPO 會連結到其個別網域的根目錄。

使用自動建立的 GPO 來套用 DirectAccess 設定時，「遠端存取」系統管理員需要具有下列權限：

-   每個網域的 GPO 建立權限

-   所有選取之用戶端網域根目錄的連結權限

-   伺服器 GPO 網域根目錄的連結權限

此外，還需要下列權限：

-   GPO 所需的「建立」、「編輯」、「刪除」及「修改」安全性權限。

-   我們建議「遠端存取」系統管理員要具備每個所需網域的 GPO「讀取」權限。 這可讓「遠端存取」確認建立 GPO 時不會有名稱重複的 GPO 存在。

### <a name="182-configure-manually-created-gpos"></a>1.8.2 設定手動建立的 GPO
使用手動建立的 GPO 時，請考量下列各項：

-   GPO 應該在執行「遠端存取安裝精靈」之前就要存在。

-   若要套用 DirectAccess 設定，「遠端存取」系統管理員必須對手動建立的 GPO 具有完整的 GPO 權限 (「編輯」、「刪除」、「修改」安全性權限)。

-   系統會搜尋整個網域來尋找 GPO 連結。 如果 GPO 在網域中並沒有被連結，系統就會在網域根目錄中自動建立一個連結。 如果沒有可建立連結的必要權限，系統就會發出警告。

### <a name="183-manage-gpos-in-a-multi-domain-controller-environment"></a>1.8.3 在多網域控制站環境中管理 GPO
每個 GPO 都是由特定的網域控制站管理，如下所示：

-   伺服器 GPO 會由與該伺服器關聯之 Active Directory 站台中的其中一個網域控制站管理。 如果該站台中的網域控制站是唯讀的，伺服器 GPO 便會由最接近 DirectAccess 伺服器的可寫入網域控制站管理。

-   用戶端和應用程式伺服器 GPO 會由以網域主控站 (PDC) 身分執行的網域控制站管理。

如果您想要手動修改 GPO 設定，請考量下列各項：

-   以伺服器 GPO 來說，若要識別哪一個網域控制站與 DirectAccess 伺服器關聯，請從 DirectAccess 伺服器上已提升權限的命令提示字元中執行 **nltest /dsgetdc: /writable**。

-   根據預設，當您使用網路功能 Windows PowerShell Cmdlet 進行變更，或是從「群組原則管理主控台」進行變更時，都會使用做為 PDC 的網域控制站。

此外，如果您修改與 DirectAccess 伺服器 (針對伺服器 GPO) 或 PDC (針對用戶端和應用程式伺服器 GPO) 沒有關聯之網域控制站上的設定，請考量下列各項：

-   修改設定之前，請確定已將最新的 GPO 複寫到網域控制站，並且備份您的 GPO 設定。 如需詳細資訊，請參閱[備份和還原遠端存取設定](https://go.microsoft.com/fwlink/?LinkID=257928)。 如果沒有更新 GPO，複寫時可能會發生合併衝突，這可能會導致「遠端存取」設定損毀。

-   修改設定之後，您必須等候系統將變更複寫到與 GPO 關聯的網域控制站。 在複寫完成之前，請勿使用「遠端存取管理主控台」或「遠端存取」PowerShell Cmdlet 來進行其他變更。 如果在複寫完成前在兩個網域控制站上編輯 GPO，可能會發生合併衝突，這可能會導致「遠端存取」設定損毀。

您也可以使用 [群組原則管理主控台] 中的 [變更網域控制站]**** 對話方塊，或使用 Windows PowerShell Cmdlet **Open-NetGPO**，來變更預設設定，讓所做的變更使用您指定的網域控制站。

-   若要在 [群組原則管理主控台] 中這麼做，請在網域或站台容器上按一下滑鼠右鍵，然後按一下 [變更網域控制站]****。

-   若要在 Windows PowerShell 中這麼做，請為 **Open-NetGPO** Cmdlet 指定 **DomainController** 參數 。 例如，若要使用名為 europe-dc.corp.contoso.com 的網域控制站，在名為 domain1\DA_Server_GPO _Europe 的 GPO 上啟用 Windows 防火牆中的私人和公用設定檔，請輸入：

    ```powershell
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True
    Save-NetGPO -GpoSession $gpoSession
    ```

### <a name="184-manage-remote-access-gpos-with-limited-permissions"></a>1.8.4 使用有限的權限管理遠端存取 GPO
若要管理「遠端存取」部署，「遠端存取」系統管理員必須對部署中使用的 GPO 具有完整的 GPO 權限 (「讀取」、「編輯」、「刪除」及「修改」安全性權限)。 這是因為「遠端存取管理主控台」和「遠端存取」PowerShell 模組會從「遠端存取 GPO」(也就是用戶端、伺服器及應用程式伺服器 GPO) 讀取設定及將設定寫入這些 GPO。

在許多組織中，負責 GPO 操作的網域系統管理員與負責「遠端存取」設定的「遠端存取」系統管理員不是同一人。 這些組織可能有限制「遠端存取」系統管理員對網域中的 GPO 擁有完整權限的政策。 網域系統管理員可能也必須先檢閱原則設定，才能將原則設定套用到網域中的任何電腦。

為了符合這些需求，網域系統管理員應該為每個 GPO 建立兩個複本：暫存和實際執行。 「遠端存取」系統管理員會被賦予暫存 GPO 的完整權限。 「遠端存取」系統管理員可在「遠端存取管理主控台」和 Windows PowerShell Cmdlet 中指定暫存 GPO 做為用於「遠端存取」部署的 GPO。 這可讓「遠端存取」系統管理員視需要讀取和修改「遠端存取」設定。

網域系統管理員必須確定暫存 GPO 未連結到網域中的任何管理領域，而且「遠端存取」系統管理員在網域中沒有 GPO 連結權限。 這可確保「遠端存取」系統管理員對暫存 GPO 所做的變更不會對網域中的電腦有任何影響。

網域系統管理員會將實際執行 GPO 連結到所需的管理領域，並套用適當的安全性篩選。 這可確保對這些 GPO 所做的變更會套用到網域中的電腦 (用戶端電腦、DirectAccess 伺服器及應用程式伺服器)。 「遠端存取」系統管理員對實際執行 GPO 沒有權限。

當暫存 GPO 被變更時，網域系統管理員可以檢閱這些 GPO 中的原則設定，以確定它符合組織中的安全性需求。 接著，網域系統管理員會使用備份功能從暫存 GPO 匯出設定，然後再將設定匯入到對應的實際執行 GPO，而這些 GPO 將會套用到網域中的電腦。

下圖顯示此設定。

![管理遠端存取 Gpo](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)

### <a name="185-recover-from-a-deleted-gpo"></a>1.8.5 從已刪除的 GPO 復原
如果不小心刪除了某個用戶端、DirectAccess 伺服器或應用程式伺服器 GPO，而且沒有備份可用，您就必須移除組態設定，然後重新設定。 如果有備份可用，您便可以從備份還原 GPO。

[遠端存取管理] 主控台會顯示下列錯誤訊息：**找不到 gpo （gpo 名稱）**。 若要移除組態設定，請執行下列步驟：

1.  執行 Windows PowerShell Cmdlet **Uninstall-remoteaccess**。

2.  開啟 [遠端存取管理主控台]。

3.  您將會看到找不到 GPO 的錯誤訊息。 按一下 [移除組態設定]****。 完成之後，伺服器將會還原到未設定的狀態。

## <a name="next-steps"></a>後續步驟

-   [步驟2：規劃 DirectAccess 部署](da-adv-plan-s2-deployments.md)

