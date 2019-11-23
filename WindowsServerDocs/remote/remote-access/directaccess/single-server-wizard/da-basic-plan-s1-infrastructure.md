---
title: 步驟1規劃基本 DirectAccess 基礎結構
description: 本主題是使用適用于 Windows Server 2016 的消費者入門 Wizard 部署單一 DirectAccess 伺服器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f4c727dc8f7905502d47119bd0e911537e827aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404872"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>步驟1規劃基本 DirectAccess 基礎結構
在單一伺服器上進行基本 DirectAccess 部署的第一個步驟，是針對部署所需的基礎結構進行規劃。 本主題描述基礎結構規劃步驟：  
  
|工作|描述|  
|----|--------|  
|規劃網路拓撲與設定|決定 \(在邊緣放置 DirectAccess 伺服器的位置，或在網路位址轉譯後面 \(NAT\) 裝置或防火牆\)，並規劃 IP 位址和路由。|  
|規劃防火牆需求|為允許 DirectAccess 通過邊緣防火牆做規劃。|  
|規劃憑證需求|DirectAccess 可以使用 Kerberos 或憑證來進行用戶端驗證。 在這個基本 DirectAccess 部署中，會自動設定 Kerberos Proxy，並使用 Active Directory 認證來完成驗證。|  
|規劃 DNS 需求|為 DirectAccess 伺服器、基礎結構伺服器及用戶端連線規劃 DNS 設定。|  
|規劃 Active Directory|規劃您的網域控制站和 Active Directory 需求。|  
|規劃群組原則物件|決定您組織中所需的 GPO，以及如何建立或編輯 GPO。|  
  
這些規劃工作不需要依特定的順序完成。  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>規劃網路拓朴和設定  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>規劃網路介面卡和 IP 位址  
  
1.  識別您想要使用的網路介面卡拓撲。 可以使用下列任一方式來設定 DirectAccess：  
  
    -   使用兩張網路介面卡-在邊緣上，有一張網路介面卡連線到網際網路，另一個連接到內部網路，或位於 NAT、防火牆或路由器裝置後面，其中一張網路介面卡連線到周邊網路，另一個則連接到內部網路.  
  
    -   在具有一張網路介面卡的 NAT 裝置後方-DirectAccess 伺服器會安裝在 NAT 裝置後方，而單一網路介面卡會連線到內部網路。  
  
2.  識別您的 IP 位址指定需求：  
  
    DirectAccess 使用 IPv6 搭配 IPsec 在 DirectAccess 用戶端電腦與公司內部網路之間建立安全連線。 不過，DirectAccess 不一定需要 IPv6 網際網路的連線能力，或內部網路的原生 IPv6 支援。 相反地，它會自動設定並使用 IPv6 轉換技術，透過 IPv4 網際網路 \(6to4、Teredo、IP\-HTTPS\)，以及您的 IPv4\-只有內部網路 \(NAT64 或 ISATAP\)的 IPv6 流量。 如需這些轉換技術的概觀，請參閱下列資源：  
  
    -   [IPv6 轉換技術](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [IP-HTTPS 通道通訊協定規格](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  根據下表設定所需的介面卡和位址指定。 針對使用單一網路介面卡的 NAT 裝置後方部署，請只使用 [**內部網路介面卡**] 資料行來設定您的 IP 位址。  
  
    ||外部網路介面卡|內部網路介面卡<sup>1</sup>|路由需求|  
    |-|--------------|--------------------|------------|  
    |IPv4 內部網路與 IPv4 網際網路|設定下列各項：<br /><br />-一個靜態公用 IPv4 位址，具有適當的子網路遮罩。<br />-網際網路防火牆或本機網際網路服務提供者 \(ISP\) 路由器的預設閘道 IPv4 位址。|設定下列各項：<br /><br />-具有適當子網路遮罩的 IPv4 內部網路位址。<br />-連線\-內部網路命名空間的特定 DNS 尾碼。 此外，也必須在內部介面上設定 DNS 伺服器。<br />-請勿在任何內部網路介面上設定預設閘道。|若要設定 DirectAccess 伺服器連線到內部 IPv4 網路上的所有子網路，請執行下列動作：<br /><br />1. 列出內部網路上所有位置的 IPv4 位址空間。<br />2. 使用**route add \-p**或**netsh interface ipv4 add route**命令來新增 ipv4 位址空間，做為 DirectAccess 伺服器之 ipv4 路由表中的靜態路由。|  
    |IPv6 網際網路與 IPv6 內部網路|設定下列各項：<br /><br />-使用您 ISP 所提供的自動設定位址設定。<br />-使用 [**路由列印**] 命令，以確保指向 ISP 路由器的預設 ipv6 路由存在於 IPv6 路由表中。<br />-判斷 ISP 和內部網路路由器是否使用 RFC 4191 中所述的預設路由器喜好設定，以及使用高於您近端內部網路路由器的預設喜好設定。 如果這兩項都是肯定的，預設路由就不需要其他設定。 ISP 路由器的喜好設定等級較高時，可確保 DirectAccess 伺服器的作用中預設 IPv6 路由指向 IPv6 網際網路。<br /><br />由於 DirectAccess 伺服器是 IPv6 路由器，因此如果您有原生的 IPv6 基礎結構，網際網路介面也可以連線到內部網路上的網域控制站。 在此情況下，請將封包篩選器新增至周邊網路中的網域控制站，以防止連線到 DirectAccess 伺服器\-面向介面的 IPv6 位址。|設定下列各項：<br /><br />-如果您不是使用預設的喜好設定層級，請使用**netsh interface ipv6 Set InterfaceIndex ignoredefaultroutes\=enabled 命令設定**內部網路介面。 此命令可確保指向內部網路路由器的其他預設路由不會新增至 IPv6 路由表。 您可以從 netsh interface show interface 命令的顯示畫面，取得內部網路介面的 InterfaceIndex。|當您有 IPv6 內部網路時，若要設定 DirectAccess 伺服器來連線到所有 IPv6 位置，請執行下列動作：<br /><br />1. 列出內部網路上所有位置的 IPv6 位址空間。<br />2. 使用**netsh interface ipv6 add route**命令來新增 ipv6 位址空間，做為 DirectAccess 伺服器之 ipv6 路由表中的靜態路由。|  
    |IPv4 網際網路和 IPv6 內部網路|DirectAccess 伺服器會使用 Microsoft 6to4 介面卡介面，將預設的 IPv6 路由流量轉送到 IPv4 網際網路上的 6to4 轉送。 您可以使用下列命令，針對 IPv4 網際網路上的 Microsoft 6to4 轉送的 IPv4 位址設定 DirectAccess 伺服器 \(在公司網路\) 中未部署原生 IPv6 時使用： netsh interface IPv6 6to4 set 轉送 name\=192.88.99.1 state\=enabled 命令。|||  
  
    > [!NOTE]  
    > 請注意下列事項：  
    >   
    > 1.  如果 DirectAccess 用戶端已被指派公用的 IPv4 位址，它會使用 6to4 轉換技術連線到內部網路。 如果 DirectAccess 用戶端無法使用6to4 連接到 DirectAccess 伺服器，則會使用 IP\-HTTPS。  
    > 2.  原生 IPv6 用戶端電腦不需要任何轉換技術，即可透過原生 IPv6 連線到 DirectAccess 伺服器。  
  
### <a name="ConfigFirewalls"></a>規劃防火牆需求  
如果 DirectAccess 伺服器是在邊緣防火牆後面，當 DirectAccess 伺服器位於 IPv4 網際網路上時，必須為 DirectAccess 流量設定下列例外：  
  
-   6to4 流量-IP 通訊協定41輸入和輸出。  
  
-   IP\-HTTPS-傳輸控制通訊協定 \(TCP\) 目的地埠443，以及 TCP 來源埠443輸出。  
  
-   如果您是以單一網路介面卡部署 DirectAccess，並且將網路位置伺服器安裝在 DirectAccess 伺服器上，則也應該豁免 TCP 連接埠 62000。  
  
    > [!NOTE]  
    > 這個豁免是在 DirectAccess 伺服器上。 所有其他例外是在邊緣防火牆上。  
  
當 DirectAccess 伺服器位於 IPv6 網際網路上時，必須為 DirectAccess 流量設定下列例外：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
使用額外的防火牆時，請為 DirectAccess 流量套用下列內部網路防火牆例外：  
  
-   ISATAP-通訊協定41輸入和輸出  
  
-   所有 IPv4\/IPv6 流量的 TCP\/UDP  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>規劃憑證需求  
IPsec 的憑證需求包括 DirectAccess 用戶端電腦在用戶端與 DirectAccess 伺服器之間建立 IPsec 連線時所使用的電腦憑證，以及 DirectAccess 伺服器用來與 DirectAccess 用戶端建立 IPsec 連線的電腦憑證。 針對 Windows Server 2012 R2 和 Windows Server 2012 中的 DirectAccess，不一定要使用這些 IPsec 憑證。 「快速入門精靈」會設定 DirectAccess 伺服器做為 Kerberos Proxy 來執行 IPsec 驗證，而不需要憑證。
  
1.  **IP\-HTTPS 伺服器**。 當您設定 DirectAccess 時，DirectAccess 伺服器會自動設定為作為 IP\-HTTPS 網頁接聽程式。 IP\-HTTPS 網站需要網站憑證，而且用戶端電腦必須能夠與憑證的憑證撤銷清單 \(CRL\) 網站。 「啟用 DirectAccess」精靈會嘗試使用 SSTP VPN 憑證。 如果未設定 SSTP，它會檢查電腦個人存放區中是否有 IP\-HTTPS 的憑證。 如果沒有可用的，它會自動建立自我\-簽署的憑證。
  
2.  **網路位置伺服器**。 網路位置伺服器是用來偵測用戶端電腦是否位於公司網路中的網站。 網路位置伺服器需要網站憑證。 DirectAccess 用戶端必須要能夠連線到 CRL 站台來查看該憑證是否在清單中。 [啟用遠端存取] 嚮導會檢查電腦個人存放區中是否有網路位置伺服器的憑證。 如果不存在，則會自動建立自我\-簽署的憑證。
  
下表摘要說明這當中每一個的憑證需求：  
  
|IPsec 驗證|IP\-HTTPS 伺服器|網路位置伺服器|  
|------------|----------|--------------|  
|當您不使用 Kerberos proxy 進行驗證時，需要內部 CA 將電腦憑證發行至 DirectAccess 伺服器和用戶端進行 IPsec 驗證|公用 CA-建議使用公用 CA 來發行 IP\-HTTPS 憑證，這可確保可在外部使用 CRL 發佈點。|內部 CA-您可以使用內部 CA 來發行網路位置伺服器網站憑證。 請確定 CRL 發佈點在內部網路具有高可用性。|  
||內部 CA-您可以使用內部 CA 來發行 IP\-HTTPS 憑證;不過，您必須確定 CRL 發佈點可供外部使用。|自我\-簽署的憑證-您可以使用網路位置伺服器網站的自我\-簽署憑證;不過，您無法在多網站部署中使用自我\-簽署的憑證。|  
||自我\-簽署的憑證-您可以針對 IP\-HTTPS 伺服器使用自我\-簽署的憑證;不過，您必須確定 CRL 發佈點可供外部使用。 自我\-簽署的憑證無法用於多網站部署。||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>規劃 IP\-HTTPS 和網路位置伺服器的憑證  
如果您想要佈建憑證來用於這些用途，請參閱[使用進階設定部署單一 DirectAccess 伺服器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 如果沒有可用的憑證，消費者入門 wizard 會針對這些用途自動建立自我\-簽署的憑證。
  
> [!NOTE]
> 如果您以手動方式布建 IP\-HTTPS 和網路位置伺服器的憑證，請確定憑證具有主體名稱。 如果憑證沒有主體名稱，但是有別名，DirectAccess 精靈將不會接受它。  
  
#### <a name="plan-dns-requirements"></a>規劃 DNS 需求  
在 DirectAccess 部署中，下列各項會需要 DNS：  
  
-   **DirectAccess 用戶端要求**。 DNS 會被用來解析來自不位於內部網路上的 DirectAccess 用戶端電腦的要求。 DirectAccess 用戶端會嘗試連線到 DirectAccess 網路位置伺服器，以判斷它們是否位於網際網路或公司網路上：如果連線成功，用戶端就會被判斷為位於內部網路上，而不使用 DirectAccess，而且用戶端要求是使用在用戶端電腦的網路介面卡上設定的 DNS 伺服器來解決。 如果連線不成功，用戶端會被認為位於網際網路上。 DirectAccess 用戶端會使用名稱解析原則表格 \(NRPT\) 來決定解析名稱要求時所要使用的 DNS 伺服器。 您可以指定用戶端應使用 DirectAccess DNS64 或替代的內部 DNS 伺服器來解析名稱。 執行名稱解析時，DirectAccess 用戶端會使用 NRPT 來識別如何處理要求。 用戶端會要求 FQDN 或單一\-標籤名稱，例如 HTTP：\/\/內部。 如果單一\-標籤名稱是要求，則會附加 DNS 尾碼來建立 FQDN。 如果 DNS 查詢與 NRPT 中的項目相符，而且已為該項目指定 DNS4 或內部網路 DNS 伺服器，系統就會將查詢傳送給指定的伺服器進行名稱解析。 如果有相符的項目存在，但是未指定任何 DNS 伺服器，這即表示有豁免規則，而將會套用一般名稱解析。  
  
    在 DirectAccess 管理主控台中將新尾碼新增到 NRPT 中時，只要按一下 [偵測] 按鈕，即可自動探索到該尾碼的預設 DNS 伺服器。 自動偵測的運作方式如下：  
  
    1.  如果公司網路是 IPv4\-型或 IPv4 和 IPv6，預設位址就是 DirectAccess 伺服器上內部介面卡的 DNS64 位址。  
  
    2.  如果公司網路是以 IPv6\-為基礎，則預設位址為公司網路中 DNS 伺服器的 IPv6 位址。  
  
-   **基礎結構伺服器**
  
    1.  **網路位置伺服器**。 DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。 此外，當您設定 DirectAccess 時，系統會自動建立下列規則：  
  
        1.  DNS 尾碼規則 - 用於根網域或 DirectAccess 伺服器的網域名稱，以及與 DirectAccess 伺服器上設定的內部網路 DNS 伺服器對應的 IPv6 位址。 例如，如果 DirectAccess 伺服器是 corp.contoso.com 網域的成員，就會為 corp.contoso.com DNS 尾碼建立規則。  
  
        2.  網路位置伺服器之 FQDN 的豁免規則。 例如，如果網路位置伺服器 URL 為 HTTPs：\/\/nls.corp.contoso.com，則會為 FQDN nls.corp.contoso.com 建立豁免規則。  
  
        **IP\-HTTPS 伺服器**。 DirectAccess 伺服器會作為 IP\-HTTPS 接聽程式，並使用其伺服器憑證向 IP\-HTTPS 用戶端進行驗證。 使用公用 DNS 伺服器的 DirectAccess 用戶端必須能夠解析 IP\-HTTPS 名稱。  
  
        **連線能力檢查器**。 DirectAccess 會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 若要確保探查能夠如預期般運作，必須在 DNS 中手動登錄下列名稱：  
  
        1.  directaccess\-directaccess-webprobehost-應解析為 DirectAccess 伺服器的內部 IPv4 位址，或僅限於 IPv6\-環境中的 IPv6 位址。  
  
        2.  directaccess\-directaccess-corpconnectivityhost-應解析為 localhost \(回送\) 位址。 應該會建立 A 和 AAAA 資源記錄，A 記錄的值為 127.0.0.1，而 AAAA 記錄的值是從 NAT64 首碼建構且最後 32 位元為 127.0.0.1。 您可以執行 Cmdlet get\-netnattransitionconfiguration 來抓取 NAT64 前置詞。  
  
        您可以透過 HTTP 使用其他網址或使用 PING，來建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
#### <a name="dns-server-requirements"></a>DNS 伺服器需求  
  
-   針對 DirectAccess 用戶端，您必須使用執行 Windows Server 2008、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2、Windows Server 2016 或任何支援 IPv6 的 DNS 伺服器的 DNS 伺服器。  
  
> [!NOTE]  
> 在部署 DirectAccess 時，建議您不要使用執行 Windows Server 2003 的 DNS 伺服器。 雖然 Windows Server 2003 DNS 伺服器支援 IPv6 記錄，但 Microsoft 不再支援 Windows Server 2003。 此外，如果您的網域控制站因為檔案複寫服務問題而執行 Windows Server 2003，則不應部署 DirectAccess。 如需詳細資訊，請參閱 [DirectAccess 不支援的設定](../DirectAccess-Unsupported-Configurations.md)。  
  
### <a name="bkmk_1_4_NLS"></a>規劃網路位置伺服器  
網路位置伺服器是一個用來偵測 DirectAccess 用戶端是否位於公司網路中的網站。 公司網路中的用戶端不會使用 DirectAccess 來連線到內部資源，而是會直接連線。  
  
「快速入門精靈」會自動在 DirectAccess 伺服器上設定網路位置伺服器，而當您部署 DirectAccess 時，便會自動建立網站。 這可讓您進行簡易安裝，而不需使用憑證基礎結構。
  
如果您想要部署網路位置伺服器，而不使用自我\-簽署的憑證，請參閱[使用 Advanced Settings 部署單一 DirectAccess 伺服器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。
  
### <a name="bkmk_1_6_AD"></a>規劃 Active Directory  
DirectAccess 會使用 Active Directory 並 Active Directory 群組原則物件，如下所示：
  
-   **驗證**： 使用 Active Directory 進行驗證。 DirectAccess 通道會使用 Kerberos 驗證來讓使用者存取內部資源。
  
-   **群組原則物件**。 DirectAccess 會將組態設定收集到套用至 DirectAccess 伺服器和用戶端的群組原則物件中。
  
-   **安全性群組**。 DirectAccess 會使用安全性群組來收集並識別 DirectAccess 用戶端電腦和 DirectAccess 伺服器。 群組原則會套用到所需的安全性群組。

**Active Directory 需求**  
  
為 DirectAccess 部署規劃 Active Directory 時，必須具備下列條件：  
  
-   至少有一個網域控制站安裝在 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 上。 
  
    如果網域控制站位於周邊網路上 \(且因此可從網際網路\-網路介面卡（DirectAccess 伺服器）連線，\) 讓 DirectAccess 伺服器無法藉由在網域控制站上新增封包篩選器來達到此限制，以防止連線到網際網路介面卡的 IP 位址。  
  
-   DirectAccess 伺服器必須是網域成員。  
  
-   DirectAccess 用戶端必須是網域成員。 用戶端可以屬於：  
  
    -   與 DirectAccess 伺服器位於相同樹系中的任何網域。  
  
    -   具有兩個\-方式的任何網域，都會與 DirectAccess 伺服器網域信任。  
  
    -   樹系中有兩個\-方式的任何網域，都會與 DirectAccess 網域所屬的樹系信任。  
  
> [!NOTE]
> - DirectAccess 伺服器不可以是網域控制站。  
> - 無法從 DirectAccess 伺服器的外部網際網路介面卡連線到 DirectAccess 所使用的 Active Directory 網域控制站，\(介面卡不得位於 Windows 防火牆\)的網域設定檔中。  
  
### <a name="bkmk_1_7_GPOs"></a>規劃群組原則物件  
當您設定 DirectAccess 時所設定的 DirectAccess 設定會收集到 \(GPO\)的群組原則物件中。 兩個不同的 GPO 會被填入 DirectAccess 設定，並依下列方式分配：  
  
-   **DirectAccess 用戶端 GPO**。 這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 項目，以及「具有進階安全性的 Windows 防火牆」連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。  
  
-   **DirectAccess 伺服器 GPO**。 這個 GPO 包含 DirectAccess 組態設定，這些設定會套用到在您的部署中被設定為 DirectAccess 伺服器的任何伺服器。 它也包含「具有進階安全性的 Windows 防火牆」連線安全性規則。  
  
設定 GPO 的方式有兩種：  
  
1.  **自動**。 您可以指定自動建立 GPO。 系統會為每個 GPO 指定一個預設名稱。 「快速入門精靈」會自動建立 GPO。
  
2.  **手動**。 您可以使用由 Active Directory 系統管理員預先定義的 GPO。
  
請注意，在設定 DirectAccess 使用特定的 GPO 之後，就無法再設定它使用不同的 GPO。
  
> [!IMPORTANT]
> 不論您使用自動還是手動設定的 GPO，只要您的用戶端會使用 3G，您就需要新增低速連結偵測原則。 原則的群組原則路徑 **： Configure 群組原則低速連結偵測**是：**電腦設定 \/ 原則 \/ 系統管理範本 \/ System \/ 群組原則**。  
  
> [!CAUTION]  
> 執行 DirectAccess Cmdlet 之前，請使用下列程式來備份所有 DirectAccess 群組原則物件：[備份和還原 directaccess](https://go.microsoft.com/fwlink/?LinkID=257928)設定  
  
#### <a name="automatically-created-gpos"></a>自動\-建立的 Gpo  
使用自動\-建立的 Gpo 時，請注意下列事項：  
  
自動建立的 GPO 會根據位置和連結目標參數進行套用，如下所示：  
  
-   以 DirectAccess 伺服器 GPO 來說，位置和連結參數都會指向包含 DirectAccess 伺服器的網域。  
  
-   建立用戶端 GPO 時，位置會設定為將在其中建立 GPO 的單一網域。 系統會在每個網域中查詢 GPO 名稱，如果存在的話，就會將 DirectAccess 設定填入它。 連結目標會設定為在其中建立 GPO 的網域根目錄。 系統會為每個包含用戶端電腦的網域建立 GPO，而該 GPO 會連結到其個別網域的根目錄。  
  
使用自動建立的 GPO 來套用 DirectAccess 設定時，DirectAccess 伺服器系統管理員需要具有下列權限：  
  
-   每個網域的 GPO 建立權限。  
  
-   所有選取之用戶端網域根目錄的連結權限。  
  
-   伺服器 GPO 網域根目錄的連結權限。  
  
-   GPO 所需的「建立」、「編輯」、「刪除」及「修改」安全性權限。  
  
-   建議 DirectAccess 系統管理員要具備每個所需網域的 GPO 讀取權限。 這可讓 DirectAccess 確認建立 GPO 時不會有名稱重複的 GPO 存在。  
  
請注意，如果沒有正確的權限來連結 GPO，系統會發出警告。 DirectAccess 操作將會繼續，但是不會建立連結。 如果系統發出這個警告，則即使稍後在新增權限之後，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
#### <a name="manually-created-gpos"></a>手動\-建立的 Gpo  
手動使用\-建立的 Gpo 時，請注意下列事項：  
  
-   GPO 應該在執行「遠端存取快速入門精靈」之前就要存在。  
  
-   手動使用\-建立的 Gpo 時，若要套用 DirectAccess 設定，DirectAccess 系統管理員需要完整的 GPO 許可權，\(手動\-建立的 Gpo 上的 [編輯]、[刪除]、[修改安全性\)]。  
  
-   使用手動建立的 GPO 時，系統會搜尋整個網域來尋找 GPO 連結。 如果網域中未連結 GPO，則會在網域根目錄中自動建立連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
請注意，如果沒有正確的權限來連結 GPO，系統會發出警告。 DirectAccess 操作將會繼續，但是不會建立連結。 如果系統發出這個警告，則即使稍後新增權限，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>從已刪除的 GPO 復原  
如果已意外刪除 DirectAccess 伺服器、用戶端或應用程式伺服器 GPO，而且沒有可用的備份，您必須移除設定，然後重新\-設定。 如果有備份可用，您便可以從備份還原 GPO。  
  
**DirectAccess 管理**會顯示下列錯誤訊息：**找不到 GPO <GPO name>** 。 若要移除組態設定，請執行下列步驟：  
  
1.  執行 PowerShell Cmdlet**卸載\-remoteaccess**。  
  
2.  重新\-開啟 [ **DirectAccess 管理**]。  
  
3.  您將會看到找不到 GPO 的錯誤訊息。 按一下 [移除組態設定]。 完成後，伺服器將會還原到未設定的\-狀態。  
  
### <a name="BKMK_Links"></a>下一步  
  
-   [步驟2：規劃基本 DirectAccess 部署](da-basic-plan-s2-deployment.md)  
  
