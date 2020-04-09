---
title: 步驟1規劃遠端存取基礎結構
description: 本主題是在 Windows Server 2016 中遠端系統管理 DirectAccess 用戶端指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2df4d64727de403264258e2f377e3df94b9b3d25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860581"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>步驟1規劃遠端存取基礎結構

>適用於：Windows Server (半年通道)、Windows Server 2016

> [!NOTE]
> Windows Server 2016 將 DirectAccess 與路由及遠端存取服務（RRAS）結合成一個遠端存取角色。  
  
本主題說明規劃基礎結構的步驟，您可以用來設定單一遠端存取服務器以進行 DirectAccess 用戶端的遠端系統管理。 下表列出這些步驟，但這些規劃工作不需要依特定循序執行。  
  
|工作|描述|  
|----|--------|  
|[規劃網路拓朴和伺服器設定](#plan-network-topology-and-settings)|決定要在何處放置遠端存取服務器（在邊緣或網路位址轉譯（NAT）裝置或防火牆後面），並規劃 IP 位址和路由。|  
|[規劃防火牆需求](#plan-firewall-requirements)|規劃透過邊緣防火牆允許遠端存取。|  
|[規劃憑證需求](#plan-certificate-requirements)|決定您是否要使用 Kerberos 通訊協定或憑證進行用戶端驗證，以及規劃您的網站憑證。<br/><br/>IP-HTTPS 是 DirectAccess 用戶端透過 IPv4 網路建立 IPv6 流量通道時所使用的轉換通訊協定。 使用憑證授權單位單位（CA）所發行的憑證，或使用由遠端存取服務器自動發出的自我簽署憑證，決定是否要驗證服務器的 IP-HTTPS。|  
|[規劃 DNS 需求](#plan-dns-requirements)|規劃遠端存取服務器、基礎結構伺服器、本機名稱解析選項和用戶端連線能力的網域名稱系統（DNS）設定。| 
|[規劃網路位置伺服器設定](#plan-the-network-location-server-configuration)|決定將網路位置伺服器網站放在您的組織中的位置（在遠端存取服務器或替代伺服器上），並在網路位置伺服器位於遠端存取服務器時，規劃憑證需求。 **注意：** DirectAccess 用戶端會使用網路位置伺服器來判斷它們是否位於內部網路上。|  
|[規劃管理伺服器的設定](#plan-management-servers-configuration)|計劃遠端管理用戶端時，會用到的管理伺服器 (例如更新伺服器)。 **注意：** 系統管理員可以使用網際網路，從遠端系統管理位於公司網路外部的 DirectAccess 用戶端電腦。|  
|[規劃 Active Directory 需求](#plan-active-directory-requirements)|規劃您的網域控制站、Active Directory 需求、用戶端驗證，以及多個網域結構。|  
|[規劃群組原則物件建立](#plan-group-policy-object-creation)|決定您的組織需要哪些 Gpo，以及如何建立和編輯 Gpo。|  
  
## <a name="plan-network-topology-and-settings"></a>規劃網路拓撲與設定  
當您規劃網路時，您必須考慮網路介面卡拓撲、IP 位址設定和 ISATAP 的需求。  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>規劃網路介面卡和 IP 位址  
  
1.  識別您想要使用的網路介面卡拓撲。 您可以使用下列任何拓撲來設定遠端存取：  
  
    -   使用兩張網路介面卡：遠端存取服務器安裝在邊緣，其中有一張網路介面卡連線到網際網路，另一個連接到內部網路。  
  
    -   使用兩張網路介面卡：遠端存取服務器安裝在 NAT 裝置、防火牆或路由器後方，其中一張網路介面卡連線到周邊網路，另一個連接到內部網路。  
  
    -   使用一個網路介面卡：遠端存取服務器安裝在 NAT 裝置後方，而單一網路介面卡連線到內部網路。  
  
2.  識別您的 IP 位址指定需求：  
  
    DirectAccess 使用 IPv6 搭配 IPsec 在 DirectAccess 用戶端電腦與公司內部網路之間建立安全連線。 不過，DirectAccess 不一定需要 IPv6 網際網路的連線能力，或內部網路的原生 IPv6 支援。 相反地，它會自動設定並使用 IPv6 轉換技術，透過 IPv4 網際網路（6to4、Teredo 或 IP-HTTPS）和在僅 IPv4 的內部網路（NAT64 或 ISATAP）建立 IPv6 流量通道。 如需這些轉換技術的概觀，請參閱下列資源：  
  
    -   [IPv6 轉換技術](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [IP-HTTPS 通道通訊協定規格](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  根據下表設定所需的介面卡和位址指定。 針對使用單一網路介面卡的 NAT 裝置後方的部署，請只使用 [**內部網路介面卡**] 資料行來設定您的 IP 位址。  
  
    ||外部網路介面卡|內部網路介面卡<sup>1，上方</sup>|路由需求|  
    |-|--------------|------------------------|------------|  
    |IPv4 網際網路和 IPv4 內部網路|設定下列項目：<br/><br/>-兩個具有適當子網路遮罩的靜態連續公用 IPv4 位址（僅限 Teredo 所需）。<br/>-網際網路防火牆或本機網際網路服務提供者（ISP）路由器的預設閘道 IPv4 位址。 **注意：** 遠端存取服務器需要兩個連續的公用 IPv4 位址，才能作為 Teredo 伺服器，而 Windows 型 Teredo 用戶端可以使用遠端存取服務器來偵測 NAT 裝置的類型。|設定下列項目：<br/><br/>-具有適當子網路遮罩的 IPv4 內部網路位址。<br/>-內部網路命名空間的連線特定 DNS 尾碼。 此外，也應該在內部介面上設定 DNS 伺服器。 **注意：** 請勿在任何內部網路介面上設定預設閘道。|若要將遠端存取服務器設定為連線到內部 IPv4 網路上的所有子網，請執行下列動作：<br/><br/>-列出內部網路上所有位置的 IPv4 位址空間。<br/>-使用 `route add -p` 或 `netsh interface ipv4 add route` 命令，將 IPv4 位址空間新增為遠端存取服務器之 IPv4 路由表中的靜態路由。|  
    |IPv6 網際網路與 IPv6 內部網路|設定下列項目：<br/><br/>-使用您 ISP 所提供的自動設定位址設定。<br/>-使用 `route print` 命令，以確保指向 ISP 路由器的預設 IPv6 路由存在於 IPv6 路由表中。<br/>-判斷 ISP 和內部網路路由器是否使用預設的路由器喜好設定（如 RFC 4191 中所述），以及其使用的預設喜好設定是否高於您的近端內部網路路由器。 如果這兩項都是肯定的，預設路由就不需要其他設定。 ISP 路由器較高的喜好設定可確保遠端存取伺服器使用中的預設 IPv6 路由指向 IPv6 網際網路。<br/><br/>因為遠端存取伺服器是 IPv6 路由器，如果您有原生的 IPv6 基礎結構，網際網路介面也可以連線到內部網路的網域控制站。 在此情況下，請將封包篩選器新增至周邊網路中的網域控制站，以防止連線到遠端存取服務器的網際網路介面的 IPv6 位址。|設定下列項目：<br/><br/>如果您不是使用預設的喜好設定層級，請使用 `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` 命令來設定內部網路介面。 這個命令可確保指向內部網路路由器的其他預設路由將不會新增到 IPv6 路由表。 您可以從 `netsh interface show interface` 命令的顯示中取得內部網路介面的 InterfaceIndex。|如果您有 IPv6 內部網路，要設定遠端存取伺服器以連線到所有 IPv6 位置，請執行下列動作：<br/><br/>-列出內部網路上所有位置的 IPv6 位址空間。<br/>-使用 `netsh interface ipv6 add route` 命令來新增 IPv6 位址空間，做為遠端存取服務器的 IPv6 路由表中的靜態路由。|  
    |IPv4 網際網路和 IPv6 內部網路|遠端存取服務器會使用 Microsoft 6to4 介面卡介面，將預設的 IPv6 路由流量轉送到 IPv4 網際網路上的6to4 轉送。 當公司網路中沒有部署原生 IPv6 時，您可以使用下列命令，為 IPv4 網際網路上的 Microsoft 6to4 轉送 IPv4 位址設定遠端存取服務器： `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`。|||  
  
    > [!NOTE]  
    > -   如果已將公用 IPv4 位址指派給 DirectAccess 用戶端，則會使用6to4 轉送技術來連線到內部網路。 如果已將私人 IPv4 位址指派給用戶端，則會使用 Teredo。 如果 DirectAccess 用戶端無法使用 6to4 或 Teredo 連線到 DirectAccess 伺服器，則會使用 IP-HTTPS。  
    > -   若要使用 Teredo，您必須在對外網路介面卡上設定兩個連續的 IP 位址。  
    > -   如果遠端存取服務器只有一張網路介面卡，您就無法使用 Teredo。  
    > -   原生 IPv6 用戶端電腦可以透過原生 IPv6 連線到遠端存取伺服器，不需要轉換技術。  
  
### <a name="plan-isatap-requirements"></a>規劃 ISATAP 需求  
DirectAccessclients 遠端系統管理需要 ISATAP，讓 DirectAccess 管理伺服器可以連線到位於網際網路上的 DirectAccess 用戶端。 不需要 ISATAP 就能支援 DirectAccess 用戶端電腦對公司網路上的 IPv4 資源所起始的連線。 針對這個目的，會使用 NAT64/DNS64。 如果您的部署需要 ISATAP，請使用下表來識別您的需求。  
  
|ISATAP 部署案例|需求|  
|---------------|--------|  
|現有的原生 IPv6 內部網路（不需要 ISATAP）|使用現有的原生 IPv6 基礎結構，您可以在遠端存取部署期間指定組織的首碼，而遠端存取服務器則不會將自己設定為 ISATAP 路由器。 請執行下列動作：<br/><br/>1. 若要確保可以從內部網路連線到 DirectAccess 用戶端，您必須修改 IPv6 路由，以便將預設路由流量轉送至遠端存取服務器。 如果您的內部網路 IPv6 位址空間使用單一48位 IPv6 位址首碼以外的位址，您必須在部署期間指定相關的組織 IPv6 首碼。<br/>2. 如果您目前已連線到 IPv6 網際網路，您必須設定預設路由流量，使其轉送至遠端存取服務器，然後在遠端存取服務器上設定適當的連線和路由，讓預設路由流量會轉送到連接到 IPv6 網際網路的裝置。|  
|現有的 ISATAP 部署|如果您有現有的 ISATAP 基礎結構，在部署期間，系統會提示您輸入組織的48位首碼，而遠端存取服務器不會將自己設定為 ISATAP 路由器。 若要確保可以從內部網路連線到 DirectAccess 用戶端，您必須修改 IPv6 路由基礎結構，以便將預設路由流量轉送至遠端存取服務器。 這種變更必須在內部網路用戶端必須已經轉送預設流量的現有 ISATAP 路由器上完成。|  
|沒有現有的 IPv6 連線能力|當「遠端存取」安裝程式偵測到伺服器沒有以原生或 ISATAP 為基礎的 IPv6 連線時，它會自動為內部網路衍生以6to4 為基礎的48位首碼，並將遠端存取服務器設定為 ISATAP 路由器以提供 IPv6內部網路上的 ISATAP 主機連線能力。 （只有當伺服器具有公用位址時，才會使用以6to4 為基礎的前置詞，否則會自動從唯一的本機位址範圍產生前置詞）。<br/><br/>若要使用 ISATAP，請執行下列動作：<br/><br/>1. 針對您要啟用 ISATAP 型連線的每個網域，在 DNS 伺服器上註冊 ISATAP 名稱，讓內部 DNS 伺服器可以將 ISATAP 名稱解析成遠端存取服務器的內部 IPv4 位址。<br/>2. 根據預設，使用全域查詢封鎖清單執行 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 解析 ISATAP 名稱的 DNS 伺服器。 若要啟用 ISATAP，您必須從封鎖清單中移除 ISATAP 名稱。 如需詳細資訊，請參閱[從 DNS 全域查詢封鎖清單中移除 ISATAP](https://go.microsoft.com/fwlink/p/?LinkId=168593)。<br/><br/>以 Windows 為基礎的 ISATAP 主機若可以解析 ISATAP 名稱，會自動以遠端存取服務器設定位址，如下所示：<br/><br/>1. ISATAP 通道介面上以 ISATAP 為基礎的 IPv6 位址<br/>2. 64 位路由，可提供與內部網路上其他 ISATAP 主機的連線能力<br/>3. 指向遠端存取服務器的預設 IPv6 路由。 預設路由可確保內部網路 ISATAP 主機可以連接到 DirectAccess 用戶端<br/><br/>當以 Windows 為基礎的 ISATAP 主機取得 ISATAP 型 IPv6 位址時，如果目的地也是 ISATAP 主機，它們就會開始使用 ISATAP 封裝的流量來進行通訊。 由於 ISATAP 會針對整個內部網路使用單一64位子網，因此您的通訊會從分割的 IPv4 通訊模型傳送到具有 IPv6 的單一子網通訊模型。 這可能會影響某些 Active Directory Domain Services （AD DS）和依賴您 Active Directory 網站和服務設定之應用程式的行為。 例如，如果您使用 [Active Directory 網站和服務] 嵌入式管理單元來設定網站、以 IPv4 為基礎的子網，以及用來將要求轉送到網站內伺服器的站對端傳輸，則 ISATAP 主機不會使用此設定。<br/><br/><ol><li>若要設定在 ISATAP 主機的網站內轉送 Active Directory 網站和服務，您必須針對每個 IPv4 子網物件設定對等的 IPv6 子網物件，其中子網的 IPv6 位址首碼會表示相同的 ISATAP 主機範圍作為 IPv4 子網的位址。 例如，對於 IPv4 子網 192.168.99.0/24 和64位 ISATAP 位址首碼2002：836b：1：8000：：/64，IPv6 子網物件的對等 IPv6 位址首碼為2002：836b：1：8000：0：5efe： 192.168.99.0/120。 針對任意的 IPv4 前置長度（在範例中設為24），您可以從公式 96 + IPv4PrefixLength 判斷對應的 IPv6 前置長度。</li><li>針對 DirectAccess 用戶端的 IPv6 位址，請新增下列內容：<br/><br/><ul><li>針對以 Teredo 為基礎的 DirectAccess 用戶端：範圍為2001：0： WWXX： YYZZ：：/64 的 IPv6 子網，其中 WWXX： YYZZ 是遠端存取服務器第一個網際網路對向 IPv4 位址的冒號-十六進位版本。 。</li><li>針對以 ip-HTTPs 為基礎的 DirectAccess 用戶端：適用于2002： WWXX： YYZZ：8100：：/56 範圍的 IPv6 子網，其中 WWXX： YYZZ 是遠端存取服務器第一個網際網路對應 IPv4 位址（w.x.y.z. x. z. z. z. z. z. z. z. y. z. z. z. z. z. z. z 。</li><li>對於6to4 型 DirectAccess 用戶端：一系列以2002開頭的6to4 型 IPv6 首碼，分別代表由網際網路指派的數位授權單位（IANA）和地區登錄所管理的地區、公用 IPv4 位址首碼。 公用 IPv4 位址首碼 w.x.y.z/n 的6to4 首碼是2002： WWXX： YYZZ：/[16 + n]，其中 WWXX： YYZZ 是冒號-十六進位版本的 w.x.y.z。<br/><br/>        例如，7.0.0.0/8 範圍是由美國登錄所管理，用於北美洲的網際網路號碼（ARIN）。 此公用 IPv6 位址範圍的對應6to4 首碼為2002:700::/24。 如需有關 IPv4 公用位址空間的詳細資訊，請參閱[IANA Ipv4 位址空間](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml)登錄。 。</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> 請確定您在 DirectAccess 伺服器的內部介面上沒有公用 IP 位址。 如果您在內部介面上有公用 IP 位址，透過 ISATAP 的連線可能會失敗。  
  
### <a name="plan-firewall-requirements"></a>規劃防火牆需求  
如果遠端存取伺服器位於邊緣防火牆後面，當遠端存取伺服器位於 IPv4 網際網路時，遠端存取流量就會需要下列例外狀況：  
  
-   針對 IP-HTTPS：傳輸控制通訊協定（TCP）目的地埠443，以及 TCP 來源埠443輸出。  
  
-   針對 Teredo 流量：使用者資料包協定（UDP）目的地埠3544輸入，以及 UDP 來源埠3544輸出。  
  
-   對於6to4 流量： IP 通訊協定41的輸入和輸出。  
  
    > [!NOTE]  
    > 若為 Teredo 和 6to4 流量，則必須針對遠端存取伺服器上的網際網路對向連續公用 IPv4 位址套用這些例外。  
    >   
    > 就 IP-HTTPS 而言，必須在公用 DNS 伺服器上註冊的位址上套用例外。  
  
-   如果您要以單一網路介面卡部署遠端存取，並在遠端存取服務器上安裝網路位置伺服器，請使用 TCP 通訊埠62000。  
  
    > [!NOTE]  
    > 這個豁免是在遠端存取服務器上，而先前的豁免則是在邊緣防火牆上。  
  
當遠端存取服務器位於 IPv6 網際網路上時，遠端存取流量需要下列例外狀況：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
-   ICMPv6 流量輸入和輸出（僅在使用 Teredo 時）。  
  
當您使用額外的防火牆時，請對遠端存取流量套用下列內部網路防火牆例外：  
  
-   針對 ISATAP：通訊協定41的輸入和輸出  
  
-   針對所有 IPv4/IPv6 流量： TCP/UD  
  
-   針對 Teredo：所有 IPv4/IPv6 流量的 ICMP  
  
### <a name="plan-certificate-requirements"></a>規劃憑證需求  
當您部署單一遠端存取服務器時，有三個需要憑證的案例。  
  
-   **Ipsec 驗證**： ipsec 的憑證需求包括 DirectAccess 用戶端電腦在建立與遠端存取服務器的 ipsec 連線時所使用的電腦憑證，以及遠端存取服務器用來與 DirectAccess 用戶端建立 ipsec 連線的電腦憑證。  
  
    對於 Windows Server 2012 中的 DirectAccess 而言，不一定要使用這些 IPsec 憑證。 或者，遠端存取服務器可以做為 Kerberos 驗證的 proxy，而不需要憑證。 如果使用 Kerberos 驗證，它會透過 SSL 運作，而 Kerberos 通訊協定會使用針對 IP-HTTPS 所設定的憑證。 某些企業案例（包括多網站部署和一次性密碼用戶端驗證）需要使用憑證驗證，而不是 Kerberos 驗證。  
  
-   **Ip-HTTPs 伺服器**：當您設定遠端存取時，遠端存取服務器會自動設定為作為 ip-HTTPs 網頁接聽程式。 IP-HTTPS 站台需要有網站憑證，而用戶端電腦必須要能夠連線到憑證撤銷清單 (CRL) 站台來查看該憑證是否在清單中。  
  
-   **網路位置伺服器**：網路位置伺服器是用來偵測用戶端電腦是否位於公司網路中的網站。 網路位置伺服器需要網站憑證。 DirectAccess 用戶端必須要能夠連線到 CRL 站台來查看該憑證是否在清單中。  
  
下表摘要說明每個案例的憑證授權單位單位（CA）需求。  
  
|IPsec 驗證|IP-HTTPS 伺服器|網路位置伺服器|  
|------------|----------|--------------|  
|當您不使用 Kerberos 通訊協定進行驗證時，需要內部 CA 將電腦憑證發行至遠端存取服務器和用戶端進行 IPsec 驗證。|內部 CA：您可以使用內部 CA 來發行 IP-HTTPS 憑證;不過，您必須確定 CRL 發佈點可供外部使用。|內部 CA：您可以使用內部 CA 來發行網路位置伺服器網站憑證。 請確定 CRL 發佈點在內部網路具有高可用性。|  
||自我簽署憑證：您可以使用 IP-HTTPS 伺服器的自我簽署憑證。 自我簽署憑證無法在多站台部署中使用。|自我簽署憑證：您可以使用網路位置伺服器網站的自我簽署憑證;不過，您無法在多網站部署中使用自我簽署憑證。|  
||公用 CA：建議您使用公用 CA 來簽發 IP-HTTPS 憑證，這可確保 CRL 發佈點可在外部使用。||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>規劃用於 IPsec 驗證的電腦憑證  
如果您使用以憑證為基礎的 IPsec 驗證，則需要遠端存取服務器和用戶端來取得電腦憑證。 安裝憑證最簡單的方式是使用群組原則來設定電腦憑證的自動註冊。 這可確保所有網域成員都會從企業 CA 取得憑證。 如果您的組織中並未設定企業 CA，請參閱 [Active Directory 憑證服務](https://technet.microsoft.com/library/cc770357.aspx)。  
  
這個憑證具有下列需求：  
  
-   憑證應該具有用戶端驗證擴充金鑰使用方法（EKU）。  
  
-   用戶端和伺服器憑證應該與相同的根憑證相關聯。 在 DirectAccess 組態設定中必須選取這個根憑證。  
  
#### <a name="plan-certificates-for-ip-https"></a>規劃 IP-HTTPS 的憑證  
遠端存取伺服器要做為 IP-HTTPS 接聽程式，而且您必須手動在伺服器上安裝 HTTPS 網站憑證。 規劃時，請考量下列各項：  
  
-   建議使用公用 CA，以便隨時可用 CRL。  
  
-   在 [主旨] 欄位中，指定遠端存取服務器的網際網路介面卡的 IPv4 位址，或 IP-HTTPS URL 的 FQDN （ConnectTo 位址）。 如果遠端存取伺服器位於 NAT 裝置後面，必須指定 NAT 裝置的公用名稱或位址。  
  
-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。  
  
-   在 [**增強金鑰使用**方法] 欄位中，使用伺服器驗證物件識別碼（OID）。  
  
-   在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。  
  
    > [!NOTE]  
    > 只有執行 Windows 7 的用戶端才需要此參數。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須直接匯入到個人存放區。  
  
-   IP-HTTPS 憑證的名稱中可以包含萬用字元。  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>規劃網路位置伺服器的網站憑證  
當您規劃網路位置伺服器網站時，請考慮下列事項：  
  
-   在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。  
  
-   在 [**增強金鑰使用**方法] 欄位中，使用伺服器驗證 OID。  
  
-   在 [ **CRL 發佈點**] 欄位中，使用連線到內部網路的 DirectAccess 用戶端可存取的 crl 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。  
  
> [!NOTE]  
> 確定 IP-HTTPS 和網路位置伺服器的憑證具有 [主體名稱]。 如果憑證使用替代名稱，則遠端存取嚮導將不會接受它。  
  
#### <a name="plan-dns-requirements"></a>規劃 DNS 需求  
本節說明遠端存取部署中用戶端和伺服器的 DNS 需求。  
  
##### <a name="directaccess-client-requests"></a>DirectAccess 用戶端要求  
DNS 會被用來解析來自不位於內部網路上的 DirectAccess 用戶端電腦的要求。 DirectAccess 用戶端會嘗試連線到 DirectAccess 網路位置伺服器，以判斷它們是否位於網際網路或公司網路上。  
  
-   如果連線成功，用戶端會被判斷為位於內部網路上，不會使用 DirectAccess，而且用戶端要求會使用在用戶端電腦的網路介面卡上設定的 DNS 伺服器來解決。  
  
-   如果連線不成功，用戶端會被認為位於網際網路上。 DirectAccess 用戶端會使用名稱解析原則表格 (NRPT) 來決定解析名稱要求時要使用的 DNS 伺服器。 您可以指定用戶端應使用 DirectAccess DNS64 或替代的內部 DNS 伺服器來解析名稱。  
  
執行名稱解析時，DirectAccess 用戶端會使用 NRPT 來識別如何處理要求。 用戶端會要求 FQDN 或單一標籤名稱，例如 <https://internal>。 如果要求的是單一標籤名稱，系統就會附加 DNS 尾碼來建立 FQDN。 如果 DNS 查詢符合 NRPT 和 DNS4 中的專案，或為該專案指定了內部網路 DNS 伺服器，則會使用指定的伺服器，傳送查詢進行名稱解析。 如果相符項存在，但是未指定任何 DNS 伺服器，則會套用豁免規則和一般名稱解析。  
  
在 [遠端存取管理] 主控台中將新的尾碼新增至 NRPT 時，按一下 [偵測] 按鈕，即可自動**探索到尾碼**的預設 DNS 伺服器。 自動偵測的運作方式如下：  
  
-   如果公司網路是以 IPv4 為基礎，或使用 IPv4 和 IPv6，預設位址就是遠端存取服務器上內部介面卡的 DNS64 位址。  
  
-   如果公司網路是 IPv6 型，預設位址就是公司網路中 DNS 伺服器的 IPv6 位址。  
  
##### <a name="infrastructure-servers"></a>基礎結構伺服器  
  
-   **網路位置伺服器**  
  
    DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，而且當它們位於網際網路上時，必須避免其解析名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。 此外，當您設定「遠端存取」時，系統會自動建立下列規則：  
  
    -   根域的 DNS 尾碼規則或遠端存取服務器的功能變數名稱，以及對應至遠端存取服務器上所設定之內部網路 DNS 伺服器的 IPv6 位址。 例如，如果遠端存取伺服器是 corp.contoso.com 網域的成員，就會為 corp.contoso.com DNS 尾碼建立規則。  
  
    -   網路位置伺服器之 FQDN 的豁免規則。 例如，如果網路位置伺服器 URL <https://nls.corp.contoso.com>，就會為 FQDN nls.corp.contoso.com 建立豁免規則。  
  
-   **IP-HTTPS 伺服器**  
  
    遠端存取服務器會做為 IP-HTTPS 接聽程式，並使用其伺服器憑證向 IP-HTTPS 用戶端進行驗證。 IP-HTTPS 名稱必須可由使用公用 DNS 伺服器的 DirectAccess 用戶端解析。  
  
##### <a name="connectivity-verifiers"></a>連線能力檢查器  
「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 若要確保探查能夠如預期般運作，必須在 DNS 中手動登錄下列名稱：  
  
-   **directaccess directaccess-webprobehost**應解析為遠端存取服務器的內部 IPv4 位址，或解析為僅 ipv6 環境中的 ipv6 位址。  
  
-   **directaccess-directaccess-corpconnectivityhost**應解析為本機主機（回送）位址。 您應該建立 A 和 AAAA 記錄。 A 記錄的值是127.0.0.1，而 AAAA 記錄的值則是由具有最後32位的 NAT64 前置詞，以127.0.0.1 的形式來構成。 您可以執行**netnatTransitionConfiguration** Windows PowerShell Cmdlet 來抓取 NAT64 前置詞。  
  
    > [!NOTE]  
    > 這只在僅 IPv4 的環境中有效。 在 IPv4 plus IPv6 或 IPv6 專用環境中，只建立具有回送 IP 位址：：1的 AAAA 記錄。  
  
您可以透過 HTTP 或 PING 使用其他網址來建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
##### <a name="dns-server-requirements"></a>DNS 伺服器需求  
  
-   針對 DirectAccess 用戶端，您必須使用執行 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 的 DNS 伺服器，或任何支援 IPv6 的 DNS 伺服器。  
  
-   您應該使用支援動態更新的 DNS 伺服器。 您可以使用不支援動態更新的 DNS 伺服器，但必須手動更新專案。  
  
-   您必須使用網際網路 DNS 伺服器來解析 CRL 發佈點的 FQDN。 例如，如果 URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> 位於遠端存取服務器 IP-HTTPS 憑證的 [ **CRL 發佈點**] 欄位中，您必須使用網際網路 DNS 伺服器確保 FQDN crld.contoso.com 可解析。  
  
#### <a name="plan-for-local-name-resolution"></a>規劃本機名稱解析  
當您規劃本機名稱解析時，請考慮下列事項：  
  
##### <a name="nrpt"></a>NRPT  
在下列情況下，您可能需要建立其他名稱解析原則表格（NRPT）規則：  
  
-   您需要為您的內部網路命名空間新增更多的 DNS 尾碼。  
  
-   如果 CRL 發佈點的 Fqdn 是以您的內部網路命名空間為基礎，您必須為 CRL 發佈點的 Fqdn 新增豁免規則。  
  
-   如果您有分裂式 DNS 環境，您必須針對您想要在網際網路上的 DirectAccess 用戶端存取網際網路版本的資源名稱新增豁免規則，而不是使用內部網路版本。  
  
-   如果您要透過內部網路 Web Proxy 伺服器將流量重新導向至外部網站，則外部網站僅適用于內部網路。 它會使用 Web Proxy 伺服器的位址來允許輸入要求。 在此情況下，請為外部網站的 FQDN 新增豁免規則，並指定規則使用內部網路 Web Proxy 伺服器，而不是內部網路 DNS 伺服器的 IPv6 位址。  
  
    例如，假設您要測試名為 test.contoso.com 的外部網站。 這個名稱無法透過網際網路 DNS 伺服器解析，但是 Contoso Web Proxy server 知道如何解析名稱，以及如何將網站的要求導向外部 web 伺服器。 為了防止不在 Contoso 內部網路的使用者存取站台，該外部網站只會允許來自 Contoso Web Proxy 之 IPv4 網際網路位址的要求。 因此，內部網路使用者可以存取網站，因為他們使用 Contoso Web Proxy，但 DirectAccess 使用者無法使用 Contoso Web Proxy。 藉由為使用 Contoso Web Proxy 的 test.contoso.com 設定 NRPT 豁免規則，即可透過 IPv4 網際網路將 test.contoso.com 的網頁要求路由傳送到內部網路 Web Proxy 伺服器。  
  
##### <a name="single-label-names"></a>單一標籤名稱  
單一標籤名稱（例如 <https://paycheck>）有時候會用於內部網路伺服器。 如果要求的是單一標籤名稱，而且已設定 DNS 尾碼搜尋清單，則清單中的 DNS 尾碼會附加至單一標籤名稱。 例如，當使用者在屬於 corp.contoso.com 網欄位型別成員的電腦上 <https://paycheck> 在網頁瀏覽器中時，視為名稱的 FQDN 是 paycheck.corp.contoso.com。 根據預設，附加的尾碼會以用戶端電腦的主要 DNS 尾碼為基礎。  
  
> [!NOTE]  
> 在脫離的命名空間案例中（其中一或多個網域電腦的 DNS 尾碼不符合電腦所屬的 Active Directory 網域），您應該確定搜尋清單已自訂為包含所有必要的尾碼。 根據預設，[遠端存取] 嚮導會將 Active Directory DNS 名稱設定為用戶端上的主要 DNS 尾碼。 請務必新增用戶端用來進行名稱解析的 DNS 尾碼。  
  
如果您的組織中已部署多個網域和 Windows 網際網路名稱服務（WINS），而且您是從遠端連線，則可依下列方式解析單一名稱：  
  
-   藉由在 DNS 中部署 WINS 正向對應區域。 嘗試解析 computername.dns.zone1.corp.contoso.com 時，會將要求導向僅使用電腦名稱稱的 WINS 伺服器。 用戶端會認為它正在發出一般的 DNS A 記錄要求，但實際上是 NetBIOS 要求。  
  
    如需詳細資訊，請參閱[管理正向對應區域](https://technet.microsoft.com/library/cc816891(WS.10).aspx)。  
  
-   藉由將 DNS 尾碼（例如，dns.zone1.corp.contoso.com）新增至預設網域 GPO。  
  
##### <a name="split-brain-dns"></a>拆分式 DNS  
分裂式 DNS 指的是針對網際網路和內部網路名稱解析使用相同的 DNS 網域。  
  
針對分裂式 DNS 部署，您必須列出在網際網路和內部網路上重複的 Fqdn，並決定 DirectAccess 用戶端應連線的資源-內部網路或網際網路版本。 當您想要 DirectAccess 用戶端連線到網際網路版本時，您必須將對應的 FQDN 當做豁免規則新增至每個資源的 NRPT。  
  
在分裂式 DNS 環境中，如果您想要使用這兩個版本的資源，請設定您的內部網路資源，其名稱不會與網際網路上使用的名稱重複。 然後指示使用者在存取內部網路上的資源時，使用替代名稱。 例如，針對 www\.contoso.com 的內部名稱設定 www\.internal.contoso.com。  
  
在非拆分式 DNS 環境中，網際網路命名空間會與內部網路命名空間不同。 例如，Contoso 公司在網際網路上使用 contoso.com，在內部網路上使用 corp.contoso.com。 由於所有內部網路資源都使用 corp.contoso.com DNS 尾碼，因此 corp.contoso.com 的 NRPT 規則會將內部網路資源的所有 DNS 名稱查詢都路由傳送到內部網路 DNS 伺服器。 具有 contoso.com 尾碼之名稱的 DNS 查詢不符合 NRPT 中的 corp.contoso.com 內部網路命名空間規則，而且會傳送至網際網路 DNS 伺服器。 使用非拆分式 DNS 部署時，由於內部網路和網際網路資源的 FQDN 不會重複，因此不需要為 NRPT 進行任何額外的設定。 DirectAccess 用戶端可以存取其組織的網際網路和內部網路資源。  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>規劃 DirectAccess 用戶端的本機名稱解析行為  
如果無法使用 DNS 解析名稱，Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 中的 DNS 用戶端服務可以使用本機名稱解析，並搭配連結本機多播名稱解析（LLMNR）和 NetBIOS over TCP/IP 通訊協定，以解析本機子網上的名稱。 當電腦位於私人網路 (例如單一子網路的家用網路) 時，通常需要本機名稱解析，才能進行對等連線。  
  
當 DNS 用戶端服務執行內部網路伺服器名稱的本機名稱解析，而電腦連線到網際網路上的共用子網時，惡意使用者可以透過 TCP/IP 訊息來捕捉 LLMNR 和 NetBIOS，以判斷內部網路伺服器名稱。 在 [基礎結構伺服器安裝程式] 的 [DNS] 頁面上，您可以根據從內部網路 DNS 伺服器收到的回應類型，設定本機名稱解析行為。 可用的選項如下：  
  
-   **如果名稱不存在於 DNS 中，請使用本機名稱解析**：此選項最安全，因為 DirectAccess 用戶端只會針對無法由內部網路 DNS 伺服器解析的伺服器名稱執行本機名稱解析。 如果可以連線到內部網路 DNS 伺服器，就會解析內部網路伺服器的名稱。 如果無法連線到內部網路 DNS 伺服器，或是有其他類型的 DNS 錯誤，也不會透過本機名稱解析將內部網路伺服器名稱遺漏到子網路。  
  
-   **如果用戶端電腦位於私人網路（建議）時無法連線到 dns 中的名稱或 dns 伺服器無法連線，請使用本機名稱解析**：建議使用此選項，因為只有在無法連線到內部網路 DNS 伺服器時，才允許在私人網路上使用本機名稱解析。  
  
-   **針對任何類型的 DNS 解析錯誤使用本機名稱解析（最不安全）** ：這是最不安全的選項，因為內部網路伺服器的名稱可以透過本機名稱解析洩漏給本機子網。  
  
#### <a name="plan-the-network-location-server-configuration"></a>規劃網路位置伺服器設定  
網路位置伺服器是一個用來偵測 DirectAccess 用戶端是否位於公司網路中的網站。 公司網路中的用戶端不會使用 DirectAccess 來觸及內部資源;相反地，它們會直接連接。  
  
網路位置伺服器網站可以裝載在遠端存取服務器或組織中的另一部伺服器上。 如果您將網路位置伺服器裝載在遠端存取服務器上，當您部署遠端存取時，就會自動建立網站。 如果您將網路位置伺服器裝載在執行 Windows 作業系統的另一部伺服器上，您必須確定該伺服器上已安裝 Internet Information Services （IIS），而且已建立網站。 [遠端存取] 不會設定網路位置伺服器上的設定。  
  
請確定網路位置伺服器網站符合下列需求：  
  
-   具有 HTTPS 伺服器憑證。  
  
-   對內部網路上的電腦具有高可用性。  
  
-   無法供網際網路上的 DirectAccess 用戶端電腦存取。  
  
-  
  
此外，當您設定網路位置伺服器網站時，請考慮下列用戶端需求：  
  
-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。  
  
-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器站台的名稱。  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>規劃網路位置伺服器的憑證  
當您取得要用於網路位置伺服器的網站憑證時，請考慮下列事項：  
  
-   在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。  
  
-   在 [**增強金鑰使用**方法] 欄位中，使用伺服器驗證 OID。  
  
-   網路位置伺服器憑證必須針對憑證撤銷清單（CRL）進行檢查。 在 [ **CRL 發佈點**] 欄位中，使用連線到內部網路的 DirectAccess 用戶端可存取的 crl 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。  
  
##### <a name="plan-dns-for-the-network-location-server"></a>規劃網路位置伺服器的 DNS  
DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。  
  
### <a name="plan-management-servers-configuration"></a>規劃管理伺服器的設定  
DirectAccess 用戶端會起始與提供服務（例如 Windows Update 和防病毒軟體更新）之管理伺服器的通訊。 DirectAccess 用戶端也會使用 Kerberos 通訊協定來驗證網域控制站，然後再存取內部網路。 在進行 DirectAccess 用戶端遠端管理時，管理伺服器會與用戶端電腦進行通訊來執行管理功能 (例如軟體或硬體清查評定)。 「遠端存取」可以自動探索某些管理伺服器，包括：  
  
-   網域控制站：針對包含用戶端電腦的網域，以及與遠端存取服務器位於相同樹系中的所有網域，執行網域控制站的自動探索。  
  
-   Microsoft 端點 Configuration Manager 伺服器  
  
第一次設定 DirectAccess 時，會自動偵測網域控制站和 Configuration Manager 伺服器。 偵測到的網域控制站不會顯示在主控台中，但是您可以使用 Windows PowerShell Cmdlet 來抓取設定。 如果已修改網域控制站或 Configuration Manager 伺服器，按一下主控台中的 [**更新管理伺服器**] 會重新整理管理伺服器清單。  
  
**管理伺服器需求**  
  
-   管理伺服器必須可透過基礎結構通道來存取。 設定「遠端存取」時，只要將伺服器新增到管理伺服器清單中，就會自動將它們設定為可透過這個通道存取。  
  
-   起始連線到 DirectAccess 用戶端的管理伺服器必須完全支援 IPv6，方法是使用原生 IPv6 位址或 ISATAP 所指派的位址。  
  
### <a name="plan-active-directory-requirements"></a>規劃 Active Directory 需求  
遠端存取會使用 Active Directory，如下所示：  
  
-   **驗證**：基礎結構通道會對連線到遠端存取服務器的電腦帳戶使用 NTLMv2 驗證，而且該帳戶必須位於 Active Directory 網域中。 內部網路通道會使用 Kerberos 驗證，讓使用者建立內部網路通道。  
  
-   **群組原則物件**：遠端存取會將設定設為群組原則物件（gpo），以套用至遠端存取服務器、用戶端以及內部應用程式伺服器。  
  
-   **安全性群組**：遠端存取使用安全性群組來收集和識別 DirectAccess 用戶端電腦。 Gpo 會套用到所需的安全性群組。  
  
當您規劃遠端存取部署的 Active Directory 環境時，請考慮下列需求：  
  
-   至少有一個網域控制站安裝在 Windows Server 2012、Windows Server 2008 R2 Windows Server 2008 或 Windows Server 2003 作業系統上。  
  
    如果網域控制站位於周邊網路（因而可從遠端存取服務器的網際網路對向網路介面卡連線），請防止遠端存取服務器到達它。 您需要在網域控制站上新增封包篩選器，以防止連線到網際網路介面卡的 IP 位址。  
  
-   遠端存取伺服器必須是網域成員。  
  
-   DirectAccess 用戶端必須是網域成員。 用戶端可以屬於：  
  
    -   與遠端存取伺服器位於相同樹系中的任何網域。  
  
    -   與遠端存取伺服器網域具有雙向信任關係的的任何網域。  
  
    -   樹系中的任何網域，與遠端存取服務器網域的樹系具有雙向信任關係。  
  
> [!NOTE]  
> -   遠端存取伺服器不能是網域控制站。  
> -   無法從遠端存取服務器的外部網際網路介面卡連線到用於遠端存取的 Active Directory 網域控制站（介面卡不得在 Windows 防火牆的網域設定檔中）。  
  
#### <a name="plan-client-authentication"></a>規劃用戶端驗證  
在 Windows Server 2012 的 [遠端存取] 中，您可以選擇使用內建的 Kerberos 驗證，其使用使用者名稱和密碼，或使用憑證進行 IPsec 電腦驗證。  
  
**Kerberos 驗證**：當您選擇使用 Active Directory 認證進行驗證時，DirectAccess 會先針對電腦使用 kerberos 驗證，然後對使用者使用 kerberos 驗證。 使用這種驗證模式時，DirectAccess 會使用單一的安全性通道，以提供 DNS 伺服器、網域控制站及內部網路上任何其他伺服器的存取權。  
  
**IPsec 驗證**：當您選擇使用雙因素驗證或網路存取保護時，DirectAccess 會使用兩個安全性通道。 [遠端存取設定] 嚮導會在 [具有 Advanced Security 的 Windows 防火牆] 中設定連線安全性規則。 這些規則會在將 IPsec 安全性協商至遠端存取服務器時，指定下列認證：  
  
-   基礎結構通道會使用電腦憑證認證來進行第二次驗證和使用者（NTLMv2）認證。 使用者認證會強制使用已驗證的網際網路通訊協定（AuthIP），並提供 DNS 伺服器和網域控制站的存取權，DirectAccess 用戶端才能使用適用于內部網路通道的 Kerberos 認證。  
  
-   內部網路通道會使用電腦憑證認證進行第一次驗證，使用者（Kerberos V5）認證用於第二次驗證。  
  
#### <a name="plan-multiple-domains"></a>規劃多個網域  
管理伺服器清單應該包括來自所有含安全性群組之網域的網域控制站，其中這些安全性群組皆包括 DirectAccess 用戶端電腦。 它應該包含所有網域，其中包含的使用者帳戶可能會使用設定為 DirectAccess 用戶端的電腦。 這可確保當使用者不是與所使用的用戶端電腦位於相同網域時，系統會以使用者網域中的網域控制站來驗證使用者。  
  
如果網域位於相同的樹系中，則會自動進行此驗證。 如果有一個安全性群組位於不同樹系中的用戶端電腦或應用程式伺服器，則不會自動偵測這些樹系的網域控制站。 也不會自動偵測到樹系。 您可以在 [**遠端存取管理**] 中執行工作**更新管理伺服器**，以偵測這些網域控制站。  
  
可能的話，在遠端存取部署期間，一般功能變數名稱尾碼應新增至 NRPT。 例如，如果您有 domain1.corp.contoso.com 和 domain2.corp.contoso.com 這兩個網域，您可以不用將兩個項目新增到 NRPT 中，而是新增一個通用的 DNS 尾碼項目 (其中網域名稱尾碼為 corp.contoso.com)。 這會針對相同根中的網域自動進行。 不在相同根中的網域必須以手動方式新增。  
  
### <a name="plan-group-policy-object-creation"></a>規劃群組原則物件建立  
當您設定「遠端存取」時，DirectAccess 設定會收集到群組原則物件（Gpo）中。 這兩個 Gpo 會填入 DirectAccess 設定，並以下列方式散發：  
  
-   **DirectAccess 用戶端 gpo**：此 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 專案，以及具有 Advanced Security 之 Windows 防火牆的連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。  
  
-   **Directaccess 伺服器 gpo**：此 GPO 包含 directaccess 設定，其適用于您在部署中設定為遠端存取服務器的任何伺服器。 其中也包含具有 Advanced Security 之 Windows 防火牆的連線安全性規則。  
  
> [!NOTE]  
> DirectAccess 用戶端的遠端系統管理不支援設定應用程式伺服器，因為用戶端無法存取應用程式伺服器所在之 DirectAccess 伺服器的內部網路。 [遠端存取安裝設定] 畫面中的步驟4無法用於這種類型的設定。  
  
您可以自動或手動設定 Gpo。  
  
**自動**：當您指定自動建立 gpo 時，會為每個 GPO 指定預設名稱。  
  
**手動**：您可以使用由 Active Directory 系統管理員預先定義的 gpo。  
  
當您設定 Gpo 時，請考慮下列警告：  
  
-   在設定 DirectAccess 使用特定的 GPO 之後，就無法再設定它使用不同的 GPO。  
  
-   執行 DirectAccess Cmdlet 之前，請使用下列程式來備份所有遠端存取群組原則物件：  
  
    [備份和還原遠端存取設定](https://go.microsoft.com/fwlink/?LinkID=257928)。  
  
-   無論您使用的是自動或手動設定的 Gpo，如果您的用戶端會使用3G，就需要新增低速連結偵測原則。 **原則：設定群組原則低速連結偵測**的路徑為：  
  
    **電腦設定/原則/系統管理範本/系統/群組原則**。  
  
-   如果連結 Gpo 的正確許可權不存在，就會發出警告。 遠端存取作業將會繼續，但不會進行連結。 如果發出這個警告，則即使稍後新增許可權，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
#### <a name="automatically-created-gpos"></a>自動建立的 Gpo  
使用自動建立的 Gpo 時，請考慮下列事項：  
  
自動建立的 GPO 會根據位置和連結目標套用，如下所示：  
  
-   對於 DirectAccess 伺服器 GPO 而言，位置和連結目標會指向包含遠端存取服務器的網域。  
  
-   建立用戶端和應用程式伺服器 Gpo 時，會將位置設定為單一網域。 系統會在每個網域中查閱 GPO 名稱，如果存在的話，則會使用 DirectAccess 設定填入網域。  
  
-   連結目標會設定為在其中建立 GPO 的網域根目錄。 系統會為每個包含用戶端電腦或應用程式伺服器的網域建立 GPO，而該 GPO 會連結到其個別網域的根目錄。  
  
使用自動建立的 Gpo 來套用 DirectAccess 設定時，遠端存取服務器管理員需要下列許可權：  
  
-   為每個網域建立 Gpo 的許可權。  
  
-   連結至所有選取之用戶端網域根目錄的許可權。  
  
-   連結到伺服器 GPO 網域根目錄的許可權。  
  
-   建立、編輯、刪除和修改 Gpo 的安全性許可權。  
  
-   每個所需網域的 GPO 讀取權限。 這不是必要的許可權，但建議使用，因為它會啟用遠端存取，以確認在建立 Gpo 時，名稱重複的 Gpo 並不存在。  
  
#### <a name="manually-created-gpos"></a>手動建立的 Gpo  
使用手動建立的 GPO 時，請考量下列各項：  
  
-   GPO 應該在執行「遠端存取安裝精靈」之前就要存在。  
  
-   若要套用 DirectAccess 設定，遠端存取服務器管理員需要完整的安全性許可權，才能建立、編輯、刪除及修改手動建立的 Gpo。  
  
-   針對整個網域中的 GPO 連結進行搜尋。 如果 GPO 在網域中並沒有被連結，系統就會在網域根目錄中自動建立一個連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>從已刪除的 GPO 復原  
如果遠端存取服務器、用戶端或應用程式伺服器上的 GPO 遭到意外刪除，則會出現下列錯誤訊息：**找不到 gpo （gpo 名稱）** 。  
  
如果有備份可用，您便可以從備份還原 GPO。 如果沒有可用的備份，您必須移除設定，然後重新設定它們。  
  
###### <a name="to-remove-configuration-settings"></a>移除設定  
  
1.  執行 Windows PowerShell Cmdlet**卸載-RemoteAccess**。  
  
2.  開啟 [**遠端存取管理**]。  
  
3.  您將會看到找不到 GPO 的錯誤訊息。 按一下 [移除組態設定]。 完成後，伺服器將會還原為未設定的狀態，您可以重新設定。  
  


