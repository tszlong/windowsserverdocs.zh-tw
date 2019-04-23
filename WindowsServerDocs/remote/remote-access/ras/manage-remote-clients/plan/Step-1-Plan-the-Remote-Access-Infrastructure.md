---
title: 步驟 1 規劃遠端存取基礎結構
description: 本主題是指南 Windows Server 2016 中，從遠端管理 DirectAccess 用戶端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f008dbdb49692e4901ebd03310710b2fbf4bd71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844419"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>步驟 1 規劃遠端存取基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2016 會將 DirectAccess 與路由及遠端存取服務 (RRAS) 合併成一個遠端存取角色。  
  
本主題說明規劃基礎結構可供您設定單一遠端存取伺服器的遠端管理 DirectAccess 用戶端的步驟。 下表列出的步驟中，但不是需要以特定順序完成這些規劃工作。  
  
|工作|描述|  
|----|--------|  
|[規劃網路拓樸和伺服器設定](#BKMK_Network)|決定在何處放置遠端存取伺服器 （在邊緣或在網路位址轉譯 (NAT) 裝置或防火牆後面），並規劃 IP 位址和路由。|  
|[規劃防火牆需求](#BKMK_Firewall)|規劃透過邊緣防火牆允許遠端存取。|  
|[規劃憑證需求](#bkmk_12CAsandcerts)|如果您將會使用 Kerberos 通訊協定或憑證進行用戶端驗證，並規劃您的網站憑證決定。<br /><br />IP-HTTPS 是 DirectAccess 用戶端透過 IPv4 網路建立 IPv6 流量通道時所使用的轉換通訊協定。 決定是否要使用發行的憑證授權單位 (CA)，或使用遠端存取伺服器自動發出自我簽署的憑證的憑證，進行 IP-HTTPS 驗證伺服器。|  
|[規劃 DNS 需求](#BKMK_DNS)|規劃遠端存取伺服器、 基礎結構伺服器、 本機名稱解析選項和用戶端連線的網域名稱系統 (DNS) 設定。| 
|[規劃網路位置伺服器設定](#BKMK_Location)|決定如何將網路位置伺服器網站放在您的組織 （在遠端存取伺服器或替代伺服器），以及規劃憑證需求，如果網路位置伺服器將會位於遠端存取伺服器。 **注意：** DirectAccess 用戶端會使用網路位置伺服器來判斷它們是否位於內部網路。|  
|[規劃管理伺服器的組態](#BKMK_Management)|計劃遠端管理用戶端時，會用到的管理伺服器 (例如更新伺服器)。 **注意：** 系統管理員可以使用網際網路，從遠端管理位於公司網路外部的 DirectAccess 用戶端電腦。|  
|[規劃 Active Directory 需求](#BKMK_ActiveDirectory)|規劃您的網域控制站、 Active Directory 需求、 用戶端驗證和多個網域結構。|  
|[規劃群組原則物件建立](#BKMK_GPOs)|決定 Gpo 為何需要在您的組織，以及如何建立和編輯 Gpo。|  
  
## <a name="BKMK_Network"></a>規劃網路拓樸和設定  
當您規劃您的網路時，您需要考量網路介面卡拓撲，設定 IP 位址和 ISATAP 的需求。  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>規劃網路介面卡和 IP 位址  
  
1.  找出您想要使用的網路介面卡拓撲。 遠端存取可以設定與任何下列拓撲：  
  
    -   有兩個的網路介面卡：遠端存取伺服器安裝在邊緣上，使用一個網路介面卡連線到網際網路，另一個用於內部網路。  
  
    -   有兩個的網路介面卡：NAT 裝置、 防火牆或路由器，後面安裝遠端存取伺服器使用一個網路介面卡連線到周邊網路，另一個用於內部網路。  
  
    -   使用一個網路介面卡：遠端存取伺服器安裝在 NAT 裝置後面，單一網路介面卡連線到內部網路。  
  
2.  識別您的 IP 位址指定需求：  
  
    DirectAccess 使用 IPv6 搭配 IPsec 在 DirectAccess 用戶端電腦與公司內部網路之間建立安全連線。 不過，DirectAccess 不一定需要 IPv6 網際網路的連線能力，或內部網路的原生 IPv6 支援。 相反地，它會自動設定，並且使用 IPv6 轉換技術，來建立通道的 IPv6 流量，經過 IPv4 網際網路 （6to4，Teredo 或 IP-HTTPS） 及透過僅支援 IPv4 的內部網路 （NAT64 或 ISATAP）。 如需這些轉換技術的概觀，請參閱下列資源：  
  
    -   [IPv6 轉換技術](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [IP-HTTPS 通道通訊協定規格](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  根據下表設定所需的介面卡和位址指定。 使用單一網路介面卡的 NAT 裝置後方部署只會使用設定您的 IP 位址**內部網路介面卡**資料行。  
  
    ||外部網路介面卡|內部網路介面卡<sup>1 上面</sup>|路由需求|  
    |-|--------------|------------------------|------------|  
    |IPv4 網際網路和 IPv4 內部網路|設定下列各項：<br /><br />-兩個靜態連續公用 IPv4 位址與適當的子網路遮罩 （Teredo 才需要）。<br />-預設閘道為您的網際網路防火牆或本機網際網路服務提供者 (ISP) 路由器的 IPv4 位址。 **注意：** 遠端存取伺服器需要兩個連續的公用 IPv4 位址，以便它可以做為 Teredo 伺服器和 Windows 型 Teredo 用戶端可以使用遠端存取伺服器，來偵測 NAT 裝置的類型。|設定下列各項：<br /><br />-IPv4 內部網路位址搭配適當的子網路遮罩。<br />-您的內部網路命名空間連線特有 DNS 尾碼。 此外，也應該在內部介面上設定 DNS 伺服器。 **注意**：不要在任何 Intranet 接口上配置預設的閘道。|若要設定遠端存取伺服器連線到內部 IPv4 網路上的所有子網路，請執行下列作業：<br /><br />-列出您內部網路上的所有位置而言的 IPv4 位址空間。<br />-使用`route add -p`或`netsh interface ipv4 add route`命令來新增 IPv4 位址空間，做為遠端存取伺服器的 IPv4 路由表中的靜態路由。|  
    |IPv6 網際網路與 IPv6 內部網路|設定下列各項：<br /><br />-使用您的 ISP 所提供的自動設定位址組態。<br />-使用`route print`IPv6 路由表中是否存在指向 ISP 路由器的預設 IPv6 路由的命令。<br />-判斷 ISP 和內部網路路由器會使用預設路由器喜好設定 RFC 4191 中所述，如果它們是使用較高的預設喜好設定比您本機內部網路路由器。 如果這兩項都是肯定的，預設路由就不需要其他設定。 ISP 路由器較高的喜好設定可確保遠端存取伺服器使用中的預設 IPv6 路由指向 IPv6 網際網路。<br /><br />因為遠端存取伺服器是 IPv6 路由器，如果您有原生的 IPv6 基礎結構，網際網路介面也可以連線到內部網路的網域控制站。 在此情況下，防止連線到遠端存取伺服器的網際網路介面的 IPv6 位址的周邊網路中的網域控制站新增封包篩選器。|設定下列各項：<br /><br />如果您不使用預設的喜好設定等級，來設定內部網路介面使用`netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled`命令。 這個命令可確保指向內部網路路由器的其他預設路由將不會新增到 IPv6 路由表。 您可以從顯示中取得您內部網路介面的 InterfaceIndex`netsh interface show interface`命令。|如果您有 IPv6 內部網路，要設定遠端存取伺服器以連線到所有 IPv6 位置，請執行下列動作：<br /><br />-列出您內部網路上的所有位置而言的 IPv6 位址空間。<br />-使用`netsh interface ipv6 add route`命令來新增 IPv6 位址空間，做為遠端存取伺服器的 IPv6 路由表中的靜態路由。|  
    |IPv4 網際網路和 IPv6 內部網路|遠端存取伺服器會將預設 IPv6 路由流量轉送到 IPv4 網際網路上使用 Microsoft 6to4 介面卡介面的 6to4 轉送。 當公司網路中沒有部署原生 IPv6 時，您可以使用下列命令來設定 IPv4 網際網路上的 Microsoft 6to4 轉送的 IPv4 位址的遠端存取伺服器： `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`。|||  
  
    > [!NOTE]  
    > -   如果 DirectAccess 用戶端已被指派公用 IPv4 位址，它會使用 6to4 轉送技術，來連接到內部網路。 如果用戶端會指派一個私人 IPv4 位址，它會使用 Teredo。 如果 DirectAccess 用戶端無法使用 6to4 或 Teredo 連線到 DirectAccess 伺服器，則會使用 IP-HTTPS。  
    > -   若要使用 Teredo，您必須在對外網路介面卡上設定兩個連續的 IP 位址。  
    > -   如果遠端存取伺服器只有一個網路介面卡，您無法使用 Teredo。  
    > -   原生 IPv6 用戶端電腦可以透過原生 IPv6 連線到遠端存取伺服器，不需要轉換技術。  
  
### <a name="plan-isatap-requirements"></a>規劃 ISATAP 需求  
ISATAP，才能進行遠端管理 DirectAccessclients，DirectAccess 管理伺服器可以連線到位於網際網路上的 DirectAccess 用戶端。 不支援由 DirectAccess 用戶端電腦到公司網路上的 IPv4 資源起始的連線需要 ISATAP。 針對這個目的，會使用 NAT64/DNS64。 如果您的部署需要 ISATAP，請使用下表來識別您的需求。  
  
|ISATAP 部署案例|需求|  
|---------------|--------|  
|現有的原生 IPv6 內部網路 （沒有 ISATAP 是必要的）|現有原生 IPv6 基礎結構，您指定遠端存取在部署期間，組織的前置詞與遠端存取伺服器不會設定本身為 ISATAP 路由器。 執行下列動作：<br /><br />1.若要確保來自內部網路連線到 DirectAccess 用戶端，您必須修改您 IPv6 路由，因此預設路由流量轉送到遠端存取伺服器。 如果您的內部網路 IPv6 位址空間會使用單一的 48 位元 IPv6 位址前置詞以外的位址，您就必須在部署期間指定相關的組織 IPv6 前置詞。<br />2.如果您目前已連線到 IPv6 網際網路，您必須設定您的預設路由流量，以便將它轉寄到遠端存取伺服器，然後設定適當的連接和路由的遠端存取伺服器上，使之預設路由傳送流量轉送到 IPv6 網際網路連線的裝置。|  
|現有的 ISATAP 部署|如果您有現有的 ISATAP 基礎結構，在部署期間會提示您輸入組織的 48 位元首碼與遠端存取伺服器不會設定本身為 ISATAP 路由器。 若要確保來自內部網路連線到 DirectAccess 用戶端，您必須修改 IPv6 路由基礎結構，使預設路由流量轉送到遠端存取伺服器。 這項變更需要在現有的內部網路用戶端必須已經是預設將流量轉送的 ISATAP 路由器上執行。|  
|沒有現有的 IPv6 連線能力|當遠端存取安裝精靈偵測到伺服器的原生或 ISATAP 基礎 IPv6 連線中斷時，它會自動衍生 6to4 型 48 位元前置詞的內部網路，並將遠端存取伺服器設定為 ISATAP 路由器提供 IPv6在您的內部網路的 ISATAP 主機的連線。 （伺服器有公用的位址時，才使用 6to4 為基礎的前置詞，否則前置詞自動產生的唯一本機位址範圍）。<br /><br />若要使用 ISATAP，請執行下列：<br /><br />1.登錄在其您想要啟用 ISATAP 為基礎的連線以便 ISATAP 名稱是由內部 DNS 伺服器為遠端存取伺服器的內部 IPv4 位址進行解析每個網域的 DNS 伺服器上的 ISATAP 名稱。<br />2.根據預設，執行 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的 DNS 伺服器會使用 「 全域查詢封鎖清單封鎖 ISATAP 名稱的解析。 若要啟用 ISATAP，您必須移除 ISATAP 名稱的區塊清單。 如需詳細資訊，請參閱 <<c0> [ 從 DNS 全域查詢封鎖清單移除的 ISATAP](https://go.microsoft.com/fwlink/p/?LinkId=168593)。<br /><br />以 Windows 為基礎 ISATAP 主機可以解析 ISATAP 名稱自動設定位址與遠端存取伺服器，如下所示：<br /><br />1.在 ISATAP 通道介面上的 ISATAP IPv6 位址<br />2.64 位元的路由，可提供連線到內部網路上的其他 ISATAP 主機<br />3.預設的 IPv6 路由指向遠端存取伺服器。 預設路由可讓您確保內部網路的 ISATAP 主機可以連線到 DirectAccess 用戶端<br /><br />當您以 Windows 為基礎的 ISATAP 主機取得 ISATAP IPv6 位址時，他們開始進行通訊的目的地也是 ISATAP 主機使用 ISATAP 封裝的流量。 由於 ISATAP 會使用單一的 64 位元子網路，整個內部網路，您的通訊會從分段 IPv4 模型的通訊，前往配置有 IPv6 的單一子網路的通訊模型。 這可能會影響某些 Active Directory 網域服務 (AD DS) 的行為和依賴您的 Active Directory 站台和服務組態的應用程式。 例如，如果您使用 Active Directory 站台及服務嵌入式管理單元來設定站台、 ipv4 子網路和站台間傳輸將要求轉寄到站台內的伺服器，此設定不是由 ISATAP 主機。<br /><br /><ol><li>若要設定 Active Directory 站台及服務的站台內的轉送，ISATAP 主機上，每個 IPv4 子網路物件，您必須設定對等的 IPv6 子網路物件，在其中的子網路的 IPv6 位址前置詞表示相同的範圍的 ISATAP 主機IPv4 子網路的位址。 例如，適用於 IPv4 子網路 192.168.99.0/24 和 64 位元 ISATAP 位址前置詞 2002:836b:1:8000:: / 64，對等的 IPv6 位址首碼的 IPv6 子網路物件是 2002:836b:1:8000:0:5efe:192.168.99.0/120。 適用於任意的 IPv4 首碼長度 （設為此範例中是 24 個），您可以判斷對應的 IPv6 首碼長度，從公式 96 + IPv4PrefixLength。</li><li>DirectAccess 用戶端的 IPv6 位址，新增下列內容：<br /><br /><ul><li>Teredo 為基礎的 DirectAccess 用戶端：範圍 2001:0:WWXX:YYZZ 的 IPv6 子網路:: / 64，在哪個 WWXX:YYZZ 是遠端存取伺服器的第一個網際網路對向的 IPv4 位址的冒號與十六進位版本。 .</li><li>為 IP HTTPS 型的 DirectAccess 用戶端：範圍 2002:WWXX:YYZZ:8100 的 IPv6 子網路:: 56，在哪個 WWXX:YYZZ 是遠端存取伺服器的第一個網際網路對向的 IPv4 位址 (w.x.y.z) 的冒號與十六進位版本。 .</li><li>6to4 DirectAccess 用戶端：一系列的 6to4 型 IPv6 首碼開頭 2002年： 代表的區域、 公用 IPv4 位址首碼由 Internet Assigned Numbers 授權單位 (IANA) 和區域的登錄。 6to4 型前置詞為公用的 IPv4 位址前置詞 w.x.y.z/n 是 2002:WWXX:YYZZ:: / [16 + n]，在哪個 WWXX:YYZZ 是冒號與十六進位版本 w.x.y.z。<br /><br />        比方說，7.0.0.0/8 範圍取決於美國登錄為網際網路數字 (ARIN) 為 North America。 此公用 IPv6 位址範圍的對應 6to4 型前置詞是 2002:700:: / 24。 IPv4 公用位址空間的相關資訊，請參閱[IANA IPv4 位址空間登錄](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml)。 .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> 確定，您不需要公用 IP 位址在 DirectAccess 伺服器的內部介面上。 如果您有內部介面上的公用 IP 位址，透過 ISATAP 的連線可能會失敗。  
  
### <a name="BKMK_Firewall"></a>規劃防火牆需求  
如果遠端存取伺服器位於邊緣防火牆後面，當遠端存取伺服器位於 IPv4 網際網路時，遠端存取流量就會需要下列例外狀況：  
  
-   為 ip-https:傳輸控制通訊協定 (TCP) 目的地連接埠 443 和 TCP 來源連接埠 443 輸出。  
  
-   Teredo 流量：使用者資料包通訊協定 (UDP) 目的地連接埠 3544 輸入，及 UDP 來源連接埠 3544 輸出。  
  
-   6to4 流量：IP 通訊協定 41 輸入和輸出。  
  
    > [!NOTE]  
    > 若為 Teredo 和 6to4 流量，則必須針對遠端存取伺服器上的網際網路對向連續公用 IPv4 位址套用這些例外。  
    >   
    > Ip-https 例外狀況必須登錄在公用 DNS 伺服器的位址上套用。  
  
-   如果您是使用單一網路介面卡部署遠端存取，並將網路位置伺服器安裝在遠端存取伺服器上，TCP 連接埠 62000。  
  
    > [!NOTE]  
    > 這個豁免是在遠端存取伺服器上，和先前的豁免則是在邊緣防火牆上。  
  
當遠端存取伺服器位於 IPv6 網際網路時，下列的例外狀況所需的 遠端存取 」 流量：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
-   ICMPv6 流量輸入和輸出 （僅使用 Teredo 時）。  
  
當您使用額外防火牆時，套用下列內部網路防火牆例外，遠端存取流量：  
  
-   針對 ISATAP:通訊協定 41 輸入和輸出  
  
-   適用於所有 IPv4/IPv6 流量：TCP/UD  
  
-   針對 Teredo:適用於所有 IPv4/IPv6 流量的 ICMP  
  
### <a name="bkmk_12CAsandcerts"></a>規劃憑證需求  
有三種情況需要憑證，當您部署單一遠端存取伺服器。  
  
-   **IPsec 驗證**:IPsec 的憑證需求包括電腦憑證，當他們建立 IPsec 連線與遠端存取伺服器，供 DirectAccess 用戶端電腦和遠端存取伺服器用來建立電腦憑證與 DirectAccess 用戶端的 IPsec 連線。  
  
    Windows Server 2012 中的 directaccess，您可能不一定要使用這些 IPsec 憑證。 另外，遠端存取伺服器可以做為 Kerberos 驗證的 proxy 而不需要憑證。 如果使用 Kerberos 驗證時，它透過 SSL 運作，以及 Kerberos 通訊協定會使用已設定 ip-https 的憑證。 某些企業案例 （包括多站台部署和單次密碼的用戶端驗證） 需要使用憑證驗證，並不是 Kerberos 驗證。  
  
-   **IP-HTTPS 伺服器**:當您設定遠端存取時，遠端存取伺服器會自動設定做為 IP-HTTPS 網頁接聽程式。 IP-HTTPS 站台需要有網站憑證，而用戶端電腦必須要能夠連線到憑證撤銷清單 (CRL) 站台來查看該憑證是否在清單中。  
  
-   **網路位置伺服器**:網路位置伺服器是一個用來偵測用戶端電腦是否位於公司網路中的網站。 網路位置伺服器需要網站憑證。 DirectAccess 用戶端必須要能夠連線到 CRL 站台來查看該憑證是否在清單中。  
  
下表摘要說明每個案例的憑證授權單位 (CA) 需求。  
  
|IPsec 驗證|IP-HTTPS 伺服器|網路位置伺服器|  
|------------|----------|--------------|  
|您不使用 Kerberos 通訊協定進行驗證時，才能發行電腦憑證給遠端存取伺服器和用戶端進行 IPsec 驗證的內部 CA。|內部 CA：您可以使用內部 CA 來簽發 IP-HTTPS 憑證；不過，您必須確定外部可以使用 CRL 發佈點。|內部 CA：您可以使用內部 CA 來簽發網路位置伺服器網站憑證。 請確定 CRL 發佈點在內部網路具有高可用性。|  
||自我簽署憑證：您可以使用自我簽署的憑證用於 IP-HTTPS 伺服器。 自我簽署憑證無法在多站台部署中使用。|自我簽署憑證：您可以使用自我簽署的憑證的網路位置伺服器網站;不過，您無法在多站台部署中，使用自我簽署的憑證。|  
||公用 CA：建議您使用公用 CA 來簽發 IP-HTTPS 憑證，這可確保 CRL 發佈點使用的外部。||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>規劃用於 IPsec 驗證的電腦憑證  
如果您使用憑證式 IPsec 驗證，遠端存取伺服器和用戶端，才能取得電腦憑證。 若要安裝憑證最簡單的方式是使用群組原則來設定電腦憑證自動註冊。 這可確保所有網域成員都會從企業 CA 取得憑證。 如果您的組織中並未設定企業 CA，請參閱 [Active Directory 憑證服務](https://technet.microsoft.com/library/cc770357.aspx)。  
  
這個憑證具有下列需求：  
  
-   此憑證應該具有用戶端驗證擴充金鑰使用方法 (EKU)。  
  
-   用戶端和伺服器憑證應該與相同的根憑證。 在 DirectAccess 組態設定中必須選取這個根憑證。  
  
#### <a name="plan-certificates-for-ip-https"></a>規劃 IP-HTTPS 的憑證  
遠端存取伺服器要做為 IP-HTTPS 接聽程式，而且您必須手動在伺服器上安裝 HTTPS 網站憑證。 規劃時，請考量下列各項：  
  
-   建議使用公用 CA，以便隨時可用 CRL。  
  
-   在 [主旨] 欄位中，指定遠端存取伺服器之網際網路介面卡或 IP-HTTPS URL （ConnectTo 位址） 的 FQDN 的 IPv4 位址。 如果遠端存取伺服器位於 NAT 裝置後面，必須指定 NAT 裝置的公用名稱或位址。  
  
-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。  
  
-   針對**增強金鑰使用方法**欄位中，使用伺服器驗證物件識別碼 (OID)。  
  
-   在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。  
  
    > [!NOTE]  
    > 這只是需要執行 Windows 7 的用戶端。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須直接匯入到個人存放區。  
  
-   IP-HTTPS 憑證的名稱中可以包含萬用字元。  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>規劃網路位置伺服器的網站憑證  
當您規劃網路位置伺服器網站時，請考慮下列：  
  
-   在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。  
  
-   針對**增強金鑰使用方法**欄位中，使用伺服器驗證 OID。  
  
-   針對**CRL 發佈點**欄位中，使用已連線到內部網路的 DirectAccess 用戶端存取的 CRL 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。  
  
> [!NOTE]  
> 請確定 IP-HTTPS 和網路位置伺服器憑證有主體名稱。 如果憑證使用的替代名稱，它不會接受由遠端存取精靈。  
  
#### <a name="BKMK_DNS"></a>規劃 DNS 需求  
本章節將說明用戶端的 DNS 需求及遠端存取部署中的伺服器。  
  
##### <a name="directaccess-client-requests"></a>DirectAccess 用戶端要求  
DNS 會被用來解析來自不位於內部網路上的 DirectAccess 用戶端電腦的要求。 DirectAccess 用戶端嘗試連線到 DirectAccess 網路位置伺服器，以判斷它們是否位於網際網路或公司網路上。  
  
-   如果連線成功，用戶端判斷內部網路上、 未使用 DirectAccess，和用戶端要求會使用用戶端電腦的網路介面卡設定的 DNS 伺服器解析。  
  
-   如果連線不成功，用戶端會被認為位於網際網路上。 DirectAccess 用戶端會使用名稱解析原則表格 (NRPT) 來決定解析名稱要求時要使用的 DNS 伺服器。 您可以指定用戶端應使用 DirectAccess DNS64 或替代的內部 DNS 伺服器來解析名稱。  
  
執行名稱解析時，DirectAccess 用戶端會使用 NRPT 來識別如何處理要求。 用戶端會要求 FQDN 或單一標籤名稱這類 https://internal。 如果要求的是單一標籤名稱，系統就會附加 DNS 尾碼來建立 FQDN。 如果 DNS 查詢符合 NRPT 中的項目，項目指定 DNS4 或內部網路 DNS 伺服器，查詢會使用指定的伺服器傳送進行名稱解析。 如果有相符項目，但未指定任何 DNS 伺服器，則豁免規則，而一般名稱解析將會套用。  
  
當新的尾碼新增到 NRPT 中遠端存取管理主控台時，可以按一下 [自動探索尾碼的預設 DNS 伺服器**偵測**] 按鈕。 自動偵測運作方式如下：  
  
-   如果公司網路是 IPv4 型，或使用 IPv4 與 IPv6，預設位址是遠端存取伺服器上內部介面卡的 DNS64 位址。  
  
-   如果公司網路是 IPv6 型，預設位址就是公司網路中 DNS 伺服器的 IPv6 位址。  
  
##### <a name="infrastructure-servers"></a>基礎結構伺服器  
  
-   **網路位置伺服器**  
  
    DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，並必須防止它們位於網際網路上時，將名稱解析。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。 此外，當您設定「遠端存取」時，系統會自動建立下列規則：  
  
    -   DNS 尾碼規則-用於根網域或網域名稱的遠端存取伺服器，以及對應至遠端存取伺服器設定的內部網路 DNS 伺服器的 IPv6 位址。 例如，如果遠端存取伺服器是 corp.contoso.com 網域的成員，就會為 corp.contoso.com DNS 尾碼建立規則。  
  
    -   網路位置伺服器之 FQDN 的豁免規則。 比方說，如果網路位置伺服器 URL 為 https://nls.corp.contoso.com，為 FQDN nls.corp.contoso.com 建立豁免規則。  
  
-   **IP-HTTPS 伺服器**  
  
    遠端存取伺服器做為 IP-HTTPS 接聽程式，並使用其伺服器憑證向 IP-HTTPS 用戶端。 IP-HTTPS 名稱必須是可解析使用公用 DNS 伺服器的 DirectAccess 用戶端。  
  
##### <a name="connectivity-verifiers"></a>連線能力檢查器  
「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 若要確保探查能夠如預期般運作，必須在 DNS 中手動登錄下列名稱：  
  
-   **directaccess webprobehost**應該解析為內部 IPv4 位址的遠端存取伺服器，或僅支援 IPv6 的環境中的 IPv6 位址。  
  
-   **directaccess corpconnectivityhost**應該解析為本機主機 （回送） 位址。 您應該建立 A 和 AAAA 記錄。 A 記錄的值為 127.0.0.1，和 AAAA 記錄的值從 NAT64 首碼的最後一個 32 位元為 127.0.0.1 建構。 可擷取 NAT64 首碼，請執行**Get-netnattransitionconfiguration** Windows PowerShell cmdlet。  
  
    > [!NOTE]  
    > 這是在僅 IPv4 的環境中才有效。 在 IPv4 加 IPv6 或僅支援 IPv6 的環境，建立只有一筆 AAAA 記錄的回送 IP 位址:: 1。  
  
您可以使用其他網址，透過 HTTP 或 PING，以建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
##### <a name="BKMK_DNSServer"></a>DNS 伺服器需求  
  
-   為 DirectAccess 用戶端，您必須使用執行 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003、 或任何支援 IPv6 的 DNS 伺服器的 DNS 伺服器。  
  
-   您應該使用支援動態更新 DNS 伺服器。 您可以使用 DNS 伺服器不支援動態更新，但則必須手動更新項目。  
  
-   CRL 發佈點的 FQDN 必須是可解析使用網際網路 DNS 伺服器。 例如，如果 URL https://crl.contoso.com/crld/corp-DC1-CA.crl處於**CRL 發佈點**欄位的 IP-HTTPS 憑證的遠端存取伺服器，您必須確定，解析 FQDN crld.contoso.com 使用網際網路 DNS 伺服器。  
  
#### <a name="BKMK_NameResolution"></a>本機名稱解析的的計劃  
當您打算為本機名稱解析時，請考慮下列：  
  
##### <a name="nrpt"></a>NRPT  
若要建立其他的名稱解析原則表格 (NRPT) 規則，在下列情況：  
  
-   您需要新增更多的 DNS 尾碼，針對您的內部網路命名空間。  
  
-   如果您的 CRL 發佈點的 Fqdn 根據您的內部網路命名空間，您必須在 CRL 發佈點的 fqdn 新增豁免規則。  
  
-   如果您有拆分式 DNS 環境，您必須新增豁免規則，您想要的 DirectAccess 用戶端位於網際網路，以存取網際網路版本而非內部網路版本資源的名稱。  
  
-   如果您將流量重新導向至外部網站透過內部網路 web proxy 伺服器，該外部網站就只能從內部網路。 它會使用您的 web proxy 伺服器的位址來允許輸入的要求。 在此情況下，新增豁免規則之 FQDN 的外部網站，並指定此規則會使用您的內部網路 web proxy 伺服器，而不是內部網路 DNS 伺服器的 IPv6 位址。  
  
    例如，假設您正在測試名為 test.contoso.com 的外部網站。 此名稱不能透過網際網路 DNS 伺服器解析，但是 Contoso web proxy 伺服器知道如何解析名稱，以及如何將導向至外部 web 伺服器網站的要求。 為了防止不在 Contoso 內部網路的使用者存取站台，該外部網站只會允許來自 Contoso Web Proxy 之 IPv4 網際網路位址的要求。 因此，內部網路使用者可以存取該網站，因為他們使用 Contoso web proxy，但是 DirectAccess 使用者無法，因為他們不使用 Contoso web proxy。 藉由為使用 Contoso Web Proxy 的 test.contoso.com 設定 NRPT 豁免規則，即可透過 IPv4 網際網路將 test.contoso.com 的網頁要求路由傳送到內部網路 Web Proxy 伺服器。  
  
##### <a name="single-label-names"></a>單一標籤名稱  
單一標籤名稱，例如 https://paycheck，有時會用於內部網路伺服器。 如果要求的單一標籤名稱，且設定 DNS 尾碼搜尋清單，清單中的 DNS 尾碼會附加至單一標籤名稱。 比方說，當電腦上的使用者，會為 corp.contoso.com 的網域類型的成員 https://paycheck在網頁瀏覽器中，會建構為名稱的 FQDN 會是 paycheck.corp.contoso.com。 根據預設，附加的尾碼根據用戶端電腦的主要 DNS 尾碼。  
  
> [!NOTE]  
> 在脫離的命名空間案例中 （一或多個網域電腦具有不符合要的電腦是成員的 Active Directory 網域的 DNS 尾碼），您應該確保搜尋清單已自訂為包含所有必要的尾碼。 根據預設，遠端存取精靈 會設定主要 DNS 尾碼用戶端上的 Active Directory DNS 名稱。 請務必新增用戶端用來進行名稱解析的 DNS 尾碼。  
  
如果您組織中部署多個網域和 Windows 網際網路名稱服務 (WINS) 和遠端連線，就可以如下所示解析單一名稱：  
  
-   藉由部署 WINS 正向對應區域在 DNS 中。 當嘗試解析 computername.dns.zone1.corp.contoso.com，要求會導向到只使用電腦名稱的 WINS 伺服器。 用戶端會認為它發出一般的 DNS A 記錄要求，但實際上是 NetBIOS 要求。  
  
    如需詳細資訊，請參閱 <<c0> [ 管理正向對應區域](https://technet.microsoft.com/library/cc816891(WS.10).aspx)。  
  
-   藉由將預設網域 GPO 中的 DNS 尾碼 (例如 dns.zone1.corp.contoso.com)。  
  
##### <a name="split-brain-dns"></a>拆分式 DNS  
拆分式 DNS 是指使用相同的 DNS 網域進行網際網路和內部網路的名稱解析。  
  
針對拆分式 DNS 部署，您必須列出重複的網際網路和內部網路，並決定哪些資源的 Fqdn，DirectAccess 用戶端應該觸達內部網路或網際網路版本。 當您想要連線到網際網路版本的 DirectAccess 用戶端時，您必須加入對應的 FQDN 做為豁免規則 NRPT 中，為每個資源。  
  
在拆分式 DNS 環境中，如果您想兩個版本的資源可供使用時，會設定您的內部網路資源不會用於網際網路的名稱重複的名稱。 然後指示使用者能夠存取內部網路上的資源時使用的替代名稱。 例如，設定 www.internal.contoso.com www.contoso.com 的內部名稱。  
  
在非拆分式 DNS 環境中，網際網路命名空間會與內部網路命名空間不同。 例如，Contoso 公司在網際網路上使用 contoso.com，在內部網路上使用 corp.contoso.com。 由於所有內部網路資源都使用 corp.contoso.com DNS 尾碼，因此 corp.contoso.com 的 NRPT 規則會將內部網路資源的所有 DNS 名稱查詢都路由傳送到內部網路 DNS 伺服器。 名稱尾碼為 contoso.com 的 DNS 查詢不相符的 NRPT 中 corp.contoso.com 內部網路命名空間的規則，並會在傳送到網際網路 DNS 伺服器。 使用非拆分式 DNS 部署時，由於內部網路和網際網路資源的 FQDN 不會重複，因此不需要為 NRPT 進行任何額外的設定。 DirectAccess 用戶端可以存取其組織的網際網路和內部網路資源。  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>規劃 DirectAccess 用戶端的本機名稱解析行為  
如果名稱不能解析使用 DNS 時，DNS 用戶端服務，在 Windows Server 2012、 Windows 8、 Windows Server 2008 R2 和 Windows 7 可以使用本機名稱解析，使用連結-本機多點傳送名稱解析 (LLMNR) 和 NetBIOS over TCP/IP 通訊協定，若要解決 本機子網路上的名稱。 當電腦位於私人網路 (例如單一子網路的家用網路) 時，通常需要本機名稱解析，才能進行對等連線。  
  
當 DNS 用戶端服務執行本機名稱解析內部網路伺服器名稱，並在電腦連線到網際網路上的共用子網路時，惡意使用者可以擷取 LLMNR 和 NetBIOS over TCP/IP 訊息來判斷內部網路伺服器名稱。 在基礎結構伺服器安裝精靈 的 DNS 頁面上，您可以設定從內部網路 DNS 伺服器收到的回應類型為基礎的本機名稱解析行為。 可用的選項如下：  
  
-   **如果名稱不存在於 DNS，請使用本機名稱解析**:這是最安全的選項，因為 DirectAccess 用戶端只會針對內部網路 DNS 伺服器無法解析的伺服器名稱執行本機名稱解析。 如果可以連線到內部網路 DNS 伺服器，就會解析內部網路伺服器的名稱。 如果無法連線到內部網路 DNS 伺服器，或是有其他類型的 DNS 錯誤，也不會透過本機名稱解析將內部網路伺服器名稱遺漏到子網路。  
  
-   **如果名稱不存在於 DNS，或當用戶端電腦位於私人網路 （建議選項） 時，無法連線到 DNS 伺服器會使用本機名稱解析**:這是建議使用的選項，因為它可允許只在無法連線到內部網路 DNS 伺服器時，才在私人網路上使用本機名稱解析。  
  
-   **使用本機名稱解析為任何種類的 （最不安全） 的 DNS 解析錯誤**:這是最不安全的選項，因為可能透過本機名稱解析將內部網路伺服器的名稱洩露到本機子網路。  
  
#### <a name="BKMK_Location"></a>規劃網路位置伺服器設定  
網路位置伺服器是一個用來偵測 DirectAccess 用戶端是否位於公司網路中的網站。 公司網路中的用戶端不會使用 DirectAccess 來連線到內部資源;但是請改為直接連接。  
  
遠端存取伺服器上或您組織中的另一部伺服器可以裝載網路位置伺服器網站。 如果您裝載網路位置伺服器遠端存取伺服器上的，網站會在您部署遠端存取時自動建立。 如果您裝載在執行 Windows 作業系統的另一部伺服器上的網路位置伺服器，您必須確定 Internet Information Services (IIS) 已安裝在該伺服器上，並已建立網站。 遠端存取不會設定網路位置伺服器上的設定。  
  
請確定網路位置伺服器網站符合下列需求：  
  
-   具有 HTTPS 伺服器憑證。  
  
-   對內部網路的電腦保持高可用性。  
  
-   無法存取網際網路上的 DirectAccess 用戶端電腦。  
  
-  
  
此外，當您設定您的網路位置伺服器網站，請考慮用戶端的下列需求：  
  
-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。  
  
-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器站台的名稱。  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>規劃網路位置伺服器憑證  
當您取得要用於網路位置伺服器網站憑證時，請考慮下列各項：  
  
-   在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或網路位置 URL 的 FQDN。  
  
-   針對**增強金鑰使用方法**欄位中，使用伺服器驗證 OID。  
  
-   網路位置伺服器憑證必須針對憑證撤銷清單 (CRL) 檢查。 針對**CRL 發佈點**欄位中，使用已連線到內部網路的 DirectAccess 用戶端存取的 CRL 發佈點。 這個 CRL 發佈點應該要無法從內部網路之外存取。  
  
##### <a name="plan-dns-for-the-network-location-server"></a>規劃網路位置伺服器的 DNS  
DirectAccess 用戶端會嘗試連線到網路位置伺服器，以判斷它們是否位於內部網路上。 內部網路上的用戶端必須能夠解析網路位置伺服器的名稱，但是當它們位於網際網路上時，則必須防止它們解析該伺服器的名稱。 為了確保這種情況，預設會將網路位置伺服器的 FQDN 新增到 NRPT 中做為豁免規則。  
  
### <a name="BKMK_Management"></a>規劃管理伺服器的設定  
DirectAccess 用戶端會起始與提供服務，例如 Windows Update 和防毒更新的管理伺服器通訊。 DirectAccess 用戶端也會使用 Kerberos 通訊協定向網域控制站，然後才存取內部網路。 在進行 DirectAccess 用戶端遠端管理時，管理伺服器會與用戶端電腦進行通訊來執行管理功能 (例如軟體或硬體清查評定)。 「遠端存取」可以自動探索某些管理伺服器，包括：  
  
-   網域控制站：自動探索的網域控制站被執行的網域來包含用戶端電腦和遠端存取伺服器相同的樹系中的所有網域。  
  
-   System Center Configuration Manager 伺服器  
  
設定網域控制站和 System Center Configuration Manager 伺服器會自動偵測 DirectAccess 在第一次。 偵測到的網域控制站不會顯示在主控台中，但可以使用 Windows PowerShell cmdlet 來擷取設定。 如果修改網域控制站或 System Center Configuration Manager 伺服器，按一下**更新管理伺服器**在主控台中重新整理管理伺服器清單。  
  
**管理伺服器需求**  
  
-   管理伺服器必須可透過基礎結構通道。 設定「遠端存取」時，只要將伺服器新增到管理伺服器清單中，就會自動將它們設定為可透過這個通道存取。  
  
-   起始 DirectAccess 用戶端連線的管理伺服器必須完全支援 IPv6，，透過原生的 IPv6 位址，或使用 ISATAP 所指派的位址。  
  
### <a name="BKMK_ActiveDirectory"></a>規劃 Active Directory 需求  
遠端存取會使用 Active Directory，如下所示：  
  
-   **驗證**:基礎結構通道會使用 NTLMv2 驗證進行連接到遠端存取伺服器的電腦帳戶，帳戶必須是 Active Directory 網域中。 內部網路通道會使用使用者的 Kerberos 驗證，來建立內部網路通道。  
  
-   **群組原則物件**:遠端存取組態設定收集到群組原則物件 (Gpo) 套用至遠端存取伺服器、 用戶端，以及內部應用程式伺服器。  
  
-   **安全性群組**:遠端存取會使用安全性群組來收集並識別 DirectAccess 用戶端電腦。 Gpo 會套用至所需的安全性群組。  
  
當您規劃 「 遠端存取 」 部署的 Active Directory 環境時，請考慮下列需求：  
  
-   至少一個網域控制站已安裝於 Windows Server 2012、 Windows Server 2008 R2 Windows Server 2008 或 Windows Server 2003 作業系統。  
  
    如果網域控制站位於周邊網路 （因而可從遠端存取伺服器的網際網路對向網路介面卡），可防止遠端存取伺服器它。 您需要以避免連線到網際網路介面卡的 IP 位址的網域控制站上新增封包篩選器。  
  
-   遠端存取伺服器必須是網域成員。  
  
-   DirectAccess 用戶端必須是網域成員。 用戶端可以屬於：  
  
    -   與遠端存取伺服器位於相同樹系中的任何網域。  
  
    -   與遠端存取伺服器網域具有雙向信任關係的的任何網域。  
  
    -   具有樹系內的遠端存取伺服器網域雙向信任的樹系中的任何網域。  
  
> [!NOTE]  
> -   遠端存取伺服器不能是網域控制站。  
> -   用於遠端存取的 Active Directory 網域控制站不能從遠端存取伺服器 （配接器不能在 Windows 防火牆的網域設定檔） 的外部網際網路介面卡可連線。  
  
#### <a name="plan-client-authentication"></a>規劃用戶端驗證  
Windows Server 2012 中的遠端存取，您可以選擇使用內建的 Kerberos 驗證，使用使用者名稱和密碼，或使用憑證進行 IPsec 電腦驗證。  
  
**Kerberos 驗證**:當您選擇使用 Active Directory 認證進行驗證時，DirectAccess 會先使用 Kerberos 驗證的電腦，並接著它會使用 Kerberos 驗證使用者。 使用此模式的驗證時，DirectAccess 會使用提供的 DNS 伺服器、 網域控制站，及任何其他伺服器存取內部網路的單一安全性通道  
  
**IPsec 驗證**:當您選擇使用雙因素驗證] 或 [網路存取保護時，DirectAccess 會使用兩個安全性通道。 遠端存取安裝精靈會在 具有進階安全性的 Windows 防火牆中設定連線安全性規則。 交涉 IPsec 安全性，遠端存取伺服器時，這些規則會指定下列認證：  
  
-   基礎結構通道會使用電腦憑證認證進行第一次驗證，而第二個驗證的使用者 (NTLMv2) 認證。 使用者認證會強制使用已驗證網際網路通訊協定 (AuthIP)，以及 DirectAccess 用戶端可以使用內部網路通道的 Kerberos 認證之前，它們會提供 DNS 伺服器和網域控制站的存取。  
  
-   內部網路通道會使用電腦憑證認證進行第一次驗證，而第二個驗證的使用者 (Kerberos V5) 認證。  
  
#### <a name="plan-multiple-domains"></a>規劃多個網域  
管理伺服器清單應該包括來自所有含安全性群組之網域的網域控制站，其中這些安全性群組皆包括 DirectAccess 用戶端電腦。 它應該包含所有的網域來包含可能會使用設定為 DirectAccess 用戶端電腦的使用者帳戶。 這可確保當使用者不是與所使用的用戶端電腦位於相同網域時，系統會以使用者網域中的網域控制站來驗證使用者。  
  
此驗證是自動，如果網域位於相同的樹系。 如果沒有與用戶端電腦或在不同的樹系的應用程式伺服器的安全性群組，都不會自動偵測這些樹系的網域控制站。 樹系會也不會自動偵測。 您可以執行工作**更新管理伺服器**中**遠端存取管理**來偵測這些網域控制站。  
  
可能的話，一般的網域名稱尾碼應該被新增到 NRPT 遠端存取部署期間。 例如，如果您有 domain1.corp.contoso.com 和 domain2.corp.contoso.com 這兩個網域，您可以不用將兩個項目新增到 NRPT 中，而是新增一個通用的 DNS 尾碼項目 (其中網域名稱尾碼為 corp.contoso.com)。 這會自動在相同的根網域。 必須以手動方式新增不在相同的根目錄中的網域。  
  
### <a name="BKMK_GPOs"></a>規劃群組原則物件建立  
當您設定遠端存取時，DirectAccess 設定會收集到群組原則物件 (Gpo)。 兩個 Gpo 會填入 DirectAccess 設定，並將其散發，如下所示：  
  
-   **DirectAccess 用戶端 GPO**:這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、 NRPT 項目和連線安全性規則具有進階安全性的 Windows 防火牆。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。  
  
-   **DirectAccess 伺服器 GPO**:這個 GPO 包含 DirectAccess 組態設定套用至您設定為遠端存取伺服器部署中的任何伺服器。 它也會包含具有進階安全性的 Windows 防火牆的連線安全性規則。  
  
> [!NOTE]  
> 應用程式伺服器的設定不會支援遠端管理 DirectAccess 用戶端中，因為用戶端無法存取內部網路應用程式伺服器所在的 DirectAccess 伺服器。 在 [遠端存取設定組態] 畫面中的步驟 4 不適用於這種組態。  
  
您可以在自動或手動設定的 Gpo。  
  
**自動**:當您指定會自動建立 Gpo 時，每個 GPO 指定預設名稱。  
  
**以手動方式**:您可以使用 Active Directory 系統管理員所預先定義的 Gpo。  
  
當您設定 Gpo 時，請考慮下列警告：  
  
-   在設定 DirectAccess 使用特定的 GPO 之後，就無法再設定它使用不同的 GPO。  
  
-   您可以使用下列程序來執行 DirectAccess cmdlet 之前，請先備份所有遠端存取群組原則物件：  
  
    [備份和還原遠端存取設定](https://go.microsoft.com/fwlink/?LinkID=257928)。  
  
-   不論您使用自動或手動設定的 Gpo 時，您需要新增低速連結偵測原則，如果您的用戶端會使用 3g。 路徑**原則：設定群組原則低速連結偵測**是：  
  
    **電腦設定/原則/系統管理範本/系統/群組原則**。  
  
-   如果沒有正確的權限來連結 Gpo，就會發出警告。 遠端存取作業會繼續，但不是會建立連結。 如果會發出這個警告，將不會自動建立連結，即使稍後新增權限。 系統管理員將必須改為手動建立連結。  
  
#### <a name="automatically-created-gpos"></a>自動建立的 Gpo  
請考慮下列會自動使用時建立的 Gpo:  
  
自動建立 GPO 會根據套用的位置和連結目標，如下所示：  
  
-   以 DirectAccess 伺服器 GPO，位置和連結目標指向包含遠端存取伺服器的網域。  
  
-   用戶端和應用程式伺服器 Gpo 建立時，位置會設定為單一網域中。 在每個網域中查詢 GPO 名稱和網域填入 DirectAccess 設定，若有的話。  
  
-   連結目標會設定為在其中建立 GPO 的網域根目錄。 系統會為每個包含用戶端電腦或應用程式伺服器的網域建立 GPO，而該 GPO 會連結到其個別網域的根目錄。  
  
當使用自動建立的 Gpo 來套用 DirectAccess 設定時，遠端存取伺服器系統管理員需要下列權限：  
  
-   針對每個網域建立 Gpo 的權限。  
  
-   連結到所有選取的用戶端網域根目錄的權限。  
  
-   連結到伺服器 GPO 網域根目錄的權限。  
  
-   安全性權限，來建立、 編輯、 刪除和修改 Gpo。  
  
-   GPO 的讀取權限的每個所需的網域。 此權限並非必要，但它建議，因為它可讓遠端存取 」 確認建立 Gpo 時有重複的名稱不會存在。  
  
#### <a name="manually-created-gpos"></a>手動建立的 Gpo  
使用手動建立的 GPO 時，請考量下列各項：  
  
-   GPO 應該在執行「遠端存取安裝精靈」之前就要存在。  
  
-   若要套用 DirectAccess 設定，遠端存取伺服器系統管理員會需要完整的安全性權限以建立、 編輯、 刪除和修改手動建立的 Gpo。  
  
-   搜尋會對整個網域 GPO 的連結。 如果 GPO 在網域中並沒有被連結，系統就會在網域根目錄中自動建立一個連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
#### <a name="recovering-from-a-deleted-gpo"></a>從已刪除的 GPO 復原  
如果不小心刪除遠端存取伺服器、 用戶端或應用程式伺服器上的 GPO，會出現下列錯誤訊息：**GPO<GPO name>找不到**。  
  
如果有備份可用，您便可以從備份還原 GPO。 如果沒有可用的備份，您必須移除組態設定，並再次設定它們。  
  
###### <a name="to-remove-configuration-settings"></a>若要移除組態設定  
  
1.  執行 Windows PowerShell cmdlet **Uninstall-remoteaccess**。  
  
2.  開啟**遠端存取管理**。  
  
3.  您將會看到找不到 GPO 的錯誤訊息。 按一下 [移除組態設定]。 完成之後，伺服器將會還原為未設定的狀態，以及您可以重新進行設定。  
  


