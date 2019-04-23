---
title: 步驟 1 規劃基本 DirectAccess 基礎結構
description: 本主題是指南部署單一 DirectAccess 伺服器使用取得啟動精靈的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b86726fd6e9ee37dbfd43357d8040b43b8dfb200
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853339"
---
#<a name="step-1-plan-the-basic-directaccess-infrastructure"></a>步驟 1 規劃基本 DirectAccess 基礎結構
在單一伺服器上的基本 DirectAccess 部署的第一個步驟是執行部署所需的基礎結構規劃。 本主題描述基礎結構規劃步驟：  
  
|工作|描述|  
|----|--------|  
|規劃網路拓撲與設定|決定 DirectAccess 伺服器的放置的位置\(在邊緣，不然就是網路位址轉譯\(NAT\)裝置或防火牆\)，並規劃 IP 位址和路由。|  
|規劃防火牆需求|為允許 DirectAccess 通過邊緣防火牆做規劃。|  
|規劃憑證需求|DirectAccess 可以使用 Kerberos 或憑證來進行用戶端驗證。 在這個基本 DirectAccess 部署中，會自動設定 Kerberos Proxy，並使用 Active Directory 認證來完成驗證。|  
|規劃 DNS 需求|為 DirectAccess 伺服器、基礎結構伺服器及用戶端連線規劃 DNS 設定。|  
|規劃 Active Directory|規劃您的網域控制站和 Active Directory 需求。|  
|規劃群組原則物件|決定您組織中所需的 GPO，以及如何建立或編輯 GPO。|  
  
這些規劃工作不需要依特定的順序完成。  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>規劃網路拓樸和設定  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>規劃網路介面卡和 IP 位址  
  
1.  識別您想要使用的網路介面卡拓撲。 可以使用下列任一方式來設定 DirectAccess：  
  
    -   與兩個網路介面卡-可能是使用一個網路介面卡連線到網際網路，以及其他的內部網路，或 NAT 背後，在邊緣防火牆或路由器裝置，搭配一個網路介面卡連線到周邊網路，另一個用於內部網路。  
  
    -   NAT 裝置後面有一個網路介面卡-DirectAccess 伺服器安裝在 NAT 裝置後面，和單一網路介面卡連線到內部網路。  
  
2.  識別您的 IP 位址指定需求：  
  
    DirectAccess 使用 IPv6 搭配 IPsec 在 DirectAccess 用戶端電腦與公司內部網路之間建立安全連線。 不過，DirectAccess 不一定需要 IPv6 網際網路的連線能力，或內部網路的原生 IPv6 支援。 相反地，它會自動設定並且建立 IPv6 流量通道在 IPv4 網際網路上使用 IPv6 轉換技術\(6to4、teredo IP\-HTTPS\)和跨您 IPv4\-只有內部網路\(NAT64 或 ISATAP\)。 如需這些轉換技術的概觀，請參閱下列資源：  
  
    -   [IPv6 轉換技術](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [IP-HTTPS 通道通訊協定規格](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  根據下表設定所需的介面卡和位址指定。 在 NAT 裝置後面部署使用單一網路介面卡設定只會使用您的 IP 位址**內部網路介面卡**資料行。  
  
    ||外部網路介面卡|內部網路介面卡<sup>1</sup>|路由需求|  
    |-|--------------|--------------------|------------|  
    |IPv4 內部網路與 IPv4 網際網路|設定下列各項：<br /><br />-一個靜態公用 IPv4 位址搭配適當的子網路遮罩。<br />-預設閘道 IPv4 位址的網際網路防火牆或本機網際網路服務提供者\(ISP\)路由器。|設定下列各項：<br /><br />-IPv4 內部網路位址搭配適當的子網路遮罩。<br />-連接\-內部網路命名空間的特定 DNS 尾碼。 此外，也必須在內部介面上設定 DNS 伺服器。<br />-請勿在任何 intranet 上設定預設閘道。|若要設定 DirectAccess 伺服器連線到內部 IPv4 網路上的所有子網路，請執行下列動作：<br /><br />1.列出 Intranet 上所有位置的 IPv4 位址空間。<br />2.使用**路由新增\-p**或是**netsh interface ipv4 新增路由**命令來新增 IPv4 位址空間，做為 DirectAccess 伺服器之 IPv4 路由表中的靜態路由。|  
    |IPv6 網際網路與 IPv6 內部網路|設定下列各項：<br /><br />-使用您的 ISP 所提供的自動設定位址組態。<br />-使用**路由傳送列印**IPv6 路由表中是否存在指向 ISP 路由器的預設 IPv6 路由的命令。<br />-判斷 ISP 和內部網路路由器是否正在使用 RFC 4191 中所述，並使用較高的預設喜好設定比您本機內部網路路由器的預設路由器喜好設定。 如果這兩項都是肯定的，預設路由就不需要其他設定。 ISP 路由器的喜好設定等級較高時，可確保 DirectAccess 伺服器的作用中預設 IPv6 路由指向 IPv6 網際網路。<br /><br />由於 DirectAccess 伺服器是 IPv6 路由器，因此如果您有原生的 IPv6 基礎結構，網際網路介面也可以連線到內部網路上的網域控制站。 在此情況下，將封包篩選器新增至周邊網路，防止連線到網際網路的 IPv6 位址的網域控制站\-的 DirectAccess 伺服器的介面。|設定下列各項：<br /><br />-如果您未使用預設的喜好設定等級，來設定與內部網路介面**netsh 介面 ipv6 set InterfaceIndex ignoredefaultroutes\=啟用**命令。 此命令可確保指向內部網路路由器的其他預設路由不會新增至 IPv6 路由表。 您可以從 netsh interface show interface 命令的顯示畫面，取得內部網路介面的 InterfaceIndex。|當您有 IPv6 內部網路時，若要設定 DirectAccess 伺服器來連線到所有 IPv6 位置，請執行下列動作：<br /><br />1.列出內部網路上所有位置的 IPv6 位址空間。<br />2.使用 **netsh interface ipv6 add route** 命令來新增 IPv6 位址空間，做為 DirectAccess 伺服器之 IPv6 路由表中的靜態路由。|  
    |IPv4 網際網路和 IPv6 內部網路|DirectAccess 伺服器會使用 Microsoft 6to4 介面卡介面，將預設的 IPv6 路由流量轉送到 IPv4 網際網路上的 6to4 轉送。 您可以設定 DirectAccess 伺服器的 Microsoft 6to4 轉送的 IPv4 位址在 IPv4 網際網路上\(當公司網路中沒有部署原生的 IPv6 時使用\)使用下列命令： netsh 介面 ipv6 6to4 set relay名稱\=name=192.88.99.1 狀態\=啟用命令。|||  
  
    > [!NOTE]  
    > 請注意下列事項：  
    >   
    > 1.  如果 DirectAccess 用戶端已被指派公用的 IPv4 位址，它會使用 6to4 轉換技術連線到內部網路。 如果 DirectAccess 用戶端無法連線到 DirectAccess 伺服器使用 6to4，它會使用 IP\-HTTPS。  
    > 2.  原生 IPv6 用戶端電腦不需要任何轉換技術，即可透過原生 IPv6 連線到 DirectAccess 伺服器。  
  
### <a name="ConfigFirewalls"></a>規劃防火牆需求  
如果 DirectAccess 伺服器是在邊緣防火牆後面，當 DirectAccess 伺服器位於 IPv4 網際網路上時，必須為 DirectAccess 流量設定下列例外：  
  
-   6to4 流量-IP 通訊協定 41 輸入和輸出。  
  
-   IP\-HTTPS 傳輸控制通訊協定\(TCP\)目的地連接埠 443 和 TCP 來源連接埠 443 輸出。  
  
-   如果您是以單一網路介面卡部署 DirectAccess，並且將網路位置伺服器安裝在 DirectAccess 伺服器上，則也應該豁免 TCP 連接埠 62000。  
  
    > [!NOTE]  
    > 這個豁免是在 DirectAccess 伺服器上。 所有其他例外是在邊緣防火牆上。  
  
當 DirectAccess 伺服器位於 IPv6 網際網路上時，必須為 DirectAccess 流量設定下列例外：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
使用額外的防火牆時，請為 DirectAccess 流量套用下列內部網路防火牆例外：  
  
-   ISATAP-通訊協定 41 輸入和輸出  
  
-   TCP\/適用於所有 IPv4 UDP\/IPv6 流量  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>規劃憑證需求  
IPsec 的憑證需求包括 DirectAccess 用戶端電腦在用戶端與 DirectAccess 伺服器之間建立 IPsec 連線時所使用的電腦憑證，以及 DirectAccess 伺服器用來與 DirectAccess 用戶端建立 IPsec 連線的電腦憑證。 Windows Server 2012 R2 和 Windows Server 2012 中的 directaccess，您可能不一定要使用這些 IPsec 憑證。 「快速入門精靈」會設定 DirectAccess 伺服器做為 Kerberos Proxy 來執行 IPsec 驗證，而不需要憑證。
  
1.  **IP\-HTTPS 伺服器**。 當您設定 DirectAccess 時，DirectAccess 伺服器會自動設定做為 IP\-HTTPS 網頁接聽程式。 IP\-HTTPS 網站需要網站憑證，和用戶端電腦必須能夠連絡的憑證撤銷清單\(CRL\)憑證的站台。 「啟用 DirectAccess」精靈會嘗試使用 SSTP VPN 憑證。 如果未設定 SSTP，它會檢查如果憑證 IP\-HTTPS 存在於電腦的個人存放區中。 如果沒有，它會自動建立自我\-簽署憑證。
  
2.  **網路位置伺服器**。 網路位置伺服器是用來偵測用戶端電腦是否位於公司網路中的網站。 網路位置伺服器需要網站憑證。 DirectAccess 用戶端必須要能夠連線到 CRL 站台來查看該憑證是否在清單中。 啟用遠端存取精靈會檢查是否有電腦的個人存放區中的網路位置伺服器憑證。 如果不存在，它會自動建立自我\-簽署憑證。
  
下表摘要說明這當中每一個的憑證需求：  
  
|IPsec 驗證|IP\-HTTPS 伺服器|網路位置伺服器|  
|------------|----------|--------------|  
|內部 CA，才能使用電腦憑證簽發給 DirectAccess 伺服器和用戶端進行 IPsec 驗證，當您不使用 Kerberos proxy 進行驗證|公用 CA-建議使用公用 CA 來簽發 IP\-HTTPS 憑證，這可確保 CRL 發佈點使用的外部。|內部 CA-您可以使用內部 CA 來簽發網路位置伺服器網站憑證。 請確定 CRL 發佈點在內部網路具有高可用性。|  
||內部 CA-您可以使用內部 CA 來簽發 IP\-HTTPS 憑證; 不過，您必須確定 CRL 發佈點使用的外部。|自我\-簽署的憑證-您可以使用自我\-簽署網路位置伺服器網站憑證; 不過，您無法使用自我\-簽署在多站台部署中的憑證。|  
||自我\-簽署的憑證-您可以使用自我\-IP 的簽署憑證\-HTTPS 伺服器; 不過，您必須確定 CRL 發佈點使用的外部。 自我\-簽署的憑證不能在多站台部署。||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>規劃 IP 憑證\-HTTPS 和網路位置伺服器  
如果您想要佈建憑證來用於這些用途，請參閱[使用進階設定部署單一 DirectAccess 伺服器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 如果沒有憑證可供使用，「 快速入門精靈 」 會自動建立自我\-簽署憑證用於這些用途。
  
> [!NOTE]
> 如果您佈建憑證 ip\-HTTPS 和網路位置伺服器以手動的方式，請確定憑證有主體名稱。 如果憑證沒有主體名稱，但是有別名，DirectAccess 精靈將不會接受它。  
  
#### <a name="plan-dns-requirements"></a>規劃 DNS 需求  
在 DirectAccess 部署中，下列各項會需要 DNS：  
  
-   **DirectAccess 用戶端要求**。 DNS 會被用來解析來自不位於內部網路上的 DirectAccess 用戶端電腦的要求。 DirectAccess 用戶端會嘗試連線到 DirectAccess 網路位置伺服器，以判斷它們位於網際網路或公司網路上：如果連線成功，用戶端會被判斷為位於內部網路上，系統便不會使用 DirectAccess，而會使用在用戶端電腦的網路介面卡上設定的 DNS 伺服器來解析用戶端要求。 如果連線不成功，用戶端會被認為位於網際網路上。 DirectAccess 用戶端會使用名稱解析原則表格\(NRPT\)來決定解析名稱要求時要使用哪些 DNS 伺服器。 您可以指定用戶端應使用 DirectAccess DNS64 或替代的內部 DNS 伺服器來解析名稱。 執行名稱解析時，DirectAccess 用戶端會使用 NRPT 來識別如何處理要求。 用戶端會要求 FQDN 或單一\-標籤名稱，例如 http:\/\/內部。 如果單一\-標籤名稱的要求，會附加 DNS 尾碼來建立 FQDN。 如果 DNS 查詢與 NRPT 中的項目相符，而且已為該項目指定 DNS4 或內部網路 DNS 伺服器，系統就會將查詢傳送給指定的伺服器進行名稱解析。 如果有相符的項目存在，但是未指定任何 DNS 伺服器，這即表示有豁免規則，而將會套用一般名稱解析。  
  
    在 DirectAccess 管理主控台中將新尾碼新增到 NRPT 中時，只要按一下 [偵測] 按鈕，即可自動探索到該尾碼的預設 DNS 伺服器。 自動偵測的運作方式如下：  
  
    1.  如果公司網路是 IPv4\-為基礎，或 IPv4 與 IPv6，預設位址是 DirectAccess 伺服器上內部介面卡的 DNS64 位址。  
  
    2.  如果公司網路是 IPv6\-為基礎，預設位址就是在公司網路的 DNS 伺服器的 IPv6 位址。  
  
-   **基礎結構伺服器**
  
    1.  **網路位置伺服器**。 DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。 此外，當您設定 DirectAccess 時，系統會自動建立下列規則：  
  
        1.  DNS 尾碼規則 - 用於根網域或 DirectAccess 伺服器的網域名稱，以及與 DirectAccess 伺服器上設定的內部網路 DNS 伺服器對應的 IPv6 位址。 例如，如果 DirectAccess 伺服器是 corp.contoso.com 網域的成員，就會為 corp.contoso.com DNS 尾碼建立規則。  
  
        2.  網路位置伺服器之 FQDN 的豁免規則。 例如，如果網路位置伺服器 URL 是 https:\/\/nls.corp.contoso.com，為 FQDN nls.corp.contoso.com 建立豁免規則。  
  
        **IP\-HTTPS 伺服器**。 DirectAccess 伺服器做為 IP\-HTTPS 接聽程式，並使用其伺服器憑證來驗證 IP\-HTTPS 用戶端。 IP\-HTTPS 名稱必須是使用公用 DNS 伺服器的 DirectAccess 用戶端解析。  
  
        **連線能力檢查器**。 DirectAccess 會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 若要確保探查能夠如預期般運作，必須在 DNS 中手動登錄下列名稱：  
  
        1.  directaccess\-webprobehost-應該解析為 DirectAccess 伺服器的內部 IPv4 位址或 IPv6 中的 IPv6 位址解析\-只環境。  
  
        2.  directaccess\-corpconnectivityhost-應該解析為 localhost\(回送\)位址。 應該會建立 A 和 AAAA 資源記錄，A 記錄的值為 127.0.0.1，而 AAAA 記錄的值是從 NAT64 首碼建構且最後 32 位元為 127.0.0.1。 可擷取 NAT64 首碼，請執行 cmdlet 取得\-netnattransitionconfiguration。  
  
        您可以透過 HTTP 使用其他網址或使用 PING，來建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
#### <a name="dns-server-requirements"></a>DNS 伺服器需求  
  
-   為 DirectAccess 用戶端，您必須使用執行 Windows Server 2008、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016 中或任何支援 IPv6 的 DNS 伺服器的 DNS 伺服器。  
  
> [!NOTE]  
> 在部署 DirectAccess 時，建議您不要使用執行 Windows Server 2003 的 DNS 伺服器。 雖然 Windows Server 2003 DNS 伺服器支援 IPv6 記錄，但 Microsoft 不再支援 Windows Server 2003。 此外，如果您的網域控制站因為檔案複寫服務問題而執行 Windows Server 2003，則不應部署 DirectAccess。 如需詳細資訊，請參閱 [DirectAccess 不支援的設定](../DirectAccess-Unsupported-Configurations.md)。  
  
### <a name="bkmk_1_4_NLS"></a>規劃網路位置伺服器  
網路位置伺服器是一個用來偵測 DirectAccess 用戶端是否位於公司網路中的網站。 公司網路中的用戶端不會使用 DirectAccess 來連線到內部資源，而是會直接連線。  
  
「快速入門精靈」會自動在 DirectAccess 伺服器上設定網路位置伺服器，而當您部署 DirectAccess 時，便會自動建立網站。 這可讓您進行簡易安裝，而不需使用憑證基礎結構。
  
如果您想要部署 「 網路位置伺服器而不使用自我\-簽署的憑證，請參閱[部署單一 DirectAccess 伺服器使用進階設定](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。
  
### <a name="bkmk_1_6_AD"></a>規劃 Active Directory  
DirectAccess 會使用 Active Directory 和 Active Directory 群組原則物件如下所示：
  
-   **驗證**： 使用 Active Directory 進行驗證。 DirectAccess 通道會使用 Kerberos 驗證來讓使用者存取內部資源。
  
-   **群組原則物件**。 DirectAccess 會將組態設定收集到套用至 DirectAccess 伺服器和用戶端的群組原則物件中。
  
-   **安全性群組**。 DirectAccess 會使用安全性群組來收集並識別 DirectAccess 用戶端電腦和 DirectAccess 伺服器。 群組原則會套用到所需的安全性群組。

**Active Directory 需求**  
  
為 DirectAccess 部署規劃 Active Directory 時，必須具備下列條件：  
  
-   安裝 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 上的至少一個網域控制站。 
  
    如果網域控制站位於周邊網路\(因而可從網際網路\-DirectAccess 伺服器的網路介面卡的面向\)使其無法到達上新增封包篩選器以防止 DirectAccess 伺服器網域控制站，以避免連線到網際網路介面卡的 IP 位址。  
  
-   DirectAccess 伺服器必須是網域成員。  
  
-   DirectAccess 用戶端必須是網域成員。 用戶端可以屬於：  
  
    -   與 DirectAccess 伺服器位於相同樹系中的任何網域。  
  
    -   任何網域都有兩個\-與 DirectAccess 伺服器網域雙向信任。  
  
    -   有兩個樹系中的任何網域\-與 DirectAccess 網域所屬樹系信任的方式。  
  
> [!NOTE]
> - DirectAccess 伺服器不可以是網域控制站。  
> - 用於 DirectAccess 的 Active Directory 網域控制站不能從 DirectAccess 伺服器的外部網際網路介面卡連線到\(配接器不能在 Windows 防火牆的網域設定檔\)。  
  
### <a name="bkmk_1_7_GPOs"></a>規劃群組原則物件  
當您設定 DirectAccess 的 DirectAccess 設定會收集到群組原則物件\(GPO\)。 兩個不同的 GPO 會被填入 DirectAccess 設定，並依下列方式分配：  
  
-   **DirectAccess 用戶端 GPO**。 這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 項目，以及「具有進階安全性的 Windows 防火牆」連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。  
  
-   **DirectAccess 伺服器 GPO**。 這個 GPO 包含 DirectAccess 組態設定，這些設定會套用到在您的部署中被設定為 DirectAccess 伺服器的任何伺服器。 它也包含「具有進階安全性的 Windows 防火牆」連線安全性規則。  
  
設定 GPO 的方式有兩種：  
  
1.  **自動**。 您可以指定自動建立 GPO。 系統會為每個 GPO 指定一個預設名稱。 「快速入門精靈」會自動建立 GPO。
  
2.  **手動**。 您可以使用由 Active Directory 系統管理員預先定義的 GPO。
  
請注意，在設定 DirectAccess 使用特定的 GPO 之後，就無法再設定它使用不同的 GPO。
  
> [!IMPORTANT]
> 不論您使用自動還是手動設定的 GPO，只要您的用戶端會使用 3G，您就需要新增低速連結偵測原則。 群組原則路徑**原則：設定群組原則低速連結偵測**是：**電腦設定\/原則\/系統管理範本\/系統\/群組原則**。  
  
> [!CAUTION]  
> 執行 DirectAccess Cmdlet 之前，請先使用下列程序來備份所有 DirectAccess 群組原則物件：[備份和還原 DirectAccess 設定](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>自動\-建立的 Gpo  
請注意下列事項時自動使用\-建立的 Gpo:  
  
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
  
#### <a name="manually-created-gpos"></a>以手動方式\-建立的 Gpo  
以手動方式使用時，請注意下列\-建立的 Gpo:  
  
-   GPO 應該在執行「遠端存取快速入門精靈」之前就要存在。  
  
-   以手動方式使用時\-建立的 Gpo 來套用 DirectAccess 設定 DirectAccess 系統管理員需要完整的 GPO 權限\(編輯、 刪除、 修改安全性\)上手動\-建立的 Gpo。  
  
-   使用手動建立的 GPO 時，系統會搜尋整個網域來尋找 GPO 連結。 如果網域中未連結 GPO，則會在網域根目錄中自動建立連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
請注意，如果沒有正確的權限來連結 GPO，系統會發出警告。 DirectAccess 操作將會繼續，但是不會建立連結。 如果系統發出這個警告，則即使稍後新增權限，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>從已刪除的 GPO 復原  
如果 DirectAccess 伺服器、 用戶端或應用程式伺服器 GPO 已被意外刪除，而且沒有備份可用，您必須移除組態設定和重新\-設定一次。 如果有備份可用，您便可以從備份還原 GPO。  
  
**DirectAccess 管理**會顯示下列錯誤訊息：**GPO<GPO name>找不到**。 若要移除組態設定，請執行下列步驟：  
  
1.  執行 PowerShell cmdlet**解除安裝\-remoteaccess**。  
  
2.  Re\-開啟**DirectAccess 管理**。  
  
3.  您將會看到找不到 GPO 的錯誤訊息。 按一下 [移除組態設定]。 完成之後，伺服器將會還原取消\-設定狀態。  
  
### <a name="BKMK_Links"></a>下一個步驟  
  
-   [步驟 2：規劃基本 DirectAccess 部署](da-basic-plan-s2-deployment.md)  
  
