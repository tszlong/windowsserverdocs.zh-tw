---
title: 步驟3規劃多網站部署
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 97705e3d6f5a4300c32ec98cc59e849862381607
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858321"
---
# <a name="step-3-plan-the-multisite-deployment"></a>步驟3規劃多網站部署

>適用於：Windows Server (半年通道)、Windows Server 2016

規劃多網站基礎結構之後，請規劃任何額外的憑證需求、用戶端電腦如何選取進入點，以及在您的部署中指派的 IPv6 位址。  

下列各節提供詳細的規劃資訊。
  
## <a name="31-plan-ip-https-certificates"></a><a name="bkmk_3_1_IPHTTPS"></a>3.1 規劃 IP-HTTPS 憑證  
當您設定進入點時，會使用特定的 ConnectTo 位址來設定每個進入點。 每個進入點的 IP-HTTPS 憑證必須符合 ConnectTo 位址。 取得憑證時，請注意下列事項：  
  
-   您無法在多站台部署中使用自我簽署憑證。  
  
-   建議使用公用 CA，以便隨時可用 CRL。  
  
-   在 [主旨] 欄位中，指定遠端存取服務器的外部介面卡的 IPv4 位址（如果已將 ConnectTo 位址指定為 IP 位址，而不是 DNS 名稱），或 IP-HTTPS URL 的 FQDN。  
  
-   憑證的一般名稱應該與 IP-HTTPS 網站的名稱相符。 也支援使用符合 ConnectTo DNS 名稱的萬用字元 URL。  
  
-   IP-HTTPS 憑證可以在主體名稱中使用萬用字元。 相同的萬用字元憑證可用於所有進入點。  
  
-   在「增強金鑰使用方法」欄位中使用伺服器驗證物件識別碼 (OID)。  
  
-   如果您在多網站部署中支援執行 Windows 7 的用戶端電腦，請在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。 執行 Windows 8 的用戶端並不需要這麼做（根據預設，這些用戶端上的 IP-HTTPS 已停用 CRL 撤銷檢查）。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須直接匯入到電腦的個人存放區，而不是使用者。  
  
## <a name="32-plan-the-network-location-server"></a><a name="bkmk_3_2_NLS"></a>3.2 規劃網路位置伺服器  
網路位置伺服器網站可以裝載在遠端存取服務器或組織中的另一部伺服器上。 如果您將網路位置伺服器裝載在遠端存取服務器上，當您部署遠端存取時，就會自動建立網站。 如果您在組織中執行 Windows 作業系統的另一部伺服器上裝載網路位置伺服器，您必須確定已安裝 Internet Information Services （IIS）以建立網站。  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>網路位置伺服器的3.2.1 憑證需求  
請確定網路位置伺服器網站符合下列憑證部署需求：  
  
-   它需要 HTTPS 伺服器憑證。  
  
-   如果網路位置伺服器位於遠端存取服務器上，而且您在部署單一遠端存取服務器時選擇使用自我簽署憑證，則必須重新設定單一伺服器部署，才能使用由內部 CA 所發行的憑證。  
  
-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。  
  
-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器網站的名稱。  
  
-   網路位置伺服器網站必須高可用性，才能供內部網路上的電腦使用。  
  
-   網路位置伺服器必須不可被網際網路上的 DirectAccess 用戶端電腦存取。  
  
-   伺服器憑證必須針對憑證撤銷清單（CRL）進行檢查。  
  
-   在遠端存取服務器上託管網路位置伺服器時，不支援萬用字元憑證。  
  
取得用於網路位置伺服器的網站憑證時，請注意下列事項：  
  
1.  在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或是網路位置 URL 的 FQDN。 請注意，如果網路位置伺服器裝載于遠端存取服務器上，則不應指定 IP 位址。 這是因為網路位置伺服器必須針對所有進入點使用相同的主體名稱，而且並非所有進入點都有相同的 IP 位址。  
  
2.  在 [增強金鑰使用方法] 欄位中使用伺服器驗證 OID。  
  
3.  在 [CRL 發佈點] 欄位中，使用連線到內部網路的 DirectAccess 用戶端可存取的 CRL 發佈點。  
  
### <a name="322dns-for-the-network-location-server"></a>網路位置伺服器的 3.2.2 DNS  
如果您將網路位置伺服器裝載在遠端存取服務器上，您必須為部署中的每個進入點新增網路位置伺服器網站的 DNS 專案。 請注意下列事項：  
  
-   在多網站部署中，第一個網路位置伺服器憑證的主體名稱會當做所有進入點的網路位置伺服器 URL 使用，因此，主體名稱和網路位置伺服器 URL 不能與電腦名稱稱相同。部署中的第一部遠端存取服務器。 它必須是專用於網路位置伺服器的 FQDN。  
  
-   網路位置伺服器流量提供的服務會使用 DNS 在進入點之間達到平衡，因此，每個進入點都應該有一個具有相同 URL 的 DNS 專案，並以進入點的內部 IP 位址進行設定。  
  
-   所有進入點都必須以具有相同主體名稱（符合網路位置伺服器 URL）的網路位置伺服器憑證進行設定。  
  
-   在新增進入點之前，必須先建立進入點的網路位置伺服器基礎結構（DNS 和憑證設定）。  
  
## <a name="33-plan-the-ipsec-root-certificate-for-all-remote-access-servers"></a><a name="bkmk_3_3_IPsec"></a>3.3 規劃所有遠端存取服務器的 IPsec 根憑證  
規劃多網站部署中的 IPsec 用戶端驗證時，請注意下列事項：  
  
1.  如果您在設定單一遠端存取服務器時選擇使用內建的 Kerberos proxy 進行電腦驗證，您必須將設定變更為 [使用由內部 CA 發行的電腦憑證]，因為多網站不支援 Kerberos proxy部署.  
  
2.  如果您使用自我簽署憑證，則必須重新設定單一伺服器部署，才能使用由內部 CA 所發行的憑證。  
  
3.  若要讓 IPsec 驗證在用戶端驗證期間成功，所有遠端存取服務器都必須具有 IPsec 根或中繼 CA 所發行的憑證，以及用於增強金鑰使用方式的用戶端驗證 OID。  
  
4.  您必須在多網站部署中的所有遠端存取服務器上安裝相同的 IPsec 根或中繼憑證。  
  
## <a name="34-plan-global-server-load-balancing"></a><a name="bkmk_3_4_GSLB"></a>3.4 規劃全域伺服器負載平衡  
在多網站部署中，您可以額外設定全域伺服器負載平衡器。 如果您的部署涵蓋大型地理分佈，則全域伺服器負載平衡器對您的組織來說可能會很有用，因為它可以分散進入點之間的流量負載。  全域伺服器負載平衡器可以設定為 DirectAccess 用戶端提供最接近進入點的進入點資訊。 此程式的運作方式如下：  
  
1.  執行 Windows 10 或 Windows 8 的用戶端電腦具有全域伺服器負載平衡器 IP 位址的清單，每個都與一個進入點相關聯。  
  
2.  Windows 10 或 Windows 8 用戶端電腦會嘗試將公用 DNS 中全域伺服器負載平衡器的 FQDN 解析為 IP 位址。 如果已解析的 IP 位址列為進入點的全域伺服器負載平衡器 IP 位址，則用戶端電腦會自動選取該進入點，並連接到其 IP-HTTPS URL （ConnectTo 位址）或其 Teredo 伺服器 IP 位址。 請注意，全域伺服器負載平衡器的 IP 位址不需要與 ConnectTo 位址或進入點的 Teredo 伺服器位址相同，因為用戶端電腦永遠不會嘗試連線到全域伺服器負載平衡器 IP 位址。  
  
3.  如果用戶端電腦位於 Web Proxy 後方（而且無法使用 DNS 解析），或全域伺服器負載平衡器 FQDN 未解析為任何已設定的全域伺服器負載平衡器 IP 位址，則會使用 HTTPS 探查自動選取進入點所有進入點的 ip-HTTPs Url。 用戶端將會連接到先回應的伺服器。  
  
如需支援遠端存取之全域伺服器負載平衡裝置的清單，請移至在[Microsoft 伺服器和雲端平臺](https://www.microsoft.com/server-cloud/)尋找合作夥伴頁面。  
  
## <a name="35-plan-directaccess-client-entry-point-selection"></a><a name="bkmk_3_5_EP_Selection"></a>3.5 規劃 DirectAccess 用戶端進入點選擇  
當您設定多網站部署時，根據預設，Windows 10 和 Windows 8 用戶端電腦會設定為連線至部署中所有進入點所需的資訊，並根據選取專案自動連線到單一進入點演算法. 您也可以將部署設定為允許 Windows 10 和 Windows 8 用戶端電腦手動選取要連線的進入點。 如果 Windows 10 或 Windows 8 用戶端電腦目前已連線到美國進入點，並已啟用自動進入點選取，則在幾分鐘後，用戶端電腦會嘗試連線透過歐洲進入點。 建議使用自動進入點選取;不過，允許手動進入點選擇可讓使用者根據目前的網路狀況，連接到不同的進入點。 例如，如果電腦連接到美國進入點，而與內部網路的連線速度會比預期慢很多。 在此情況下，終端使用者可以手動選取以連線到歐洲進入點，以改善內部網路的連線。  
  
> [!NOTE]  
> 使用者手動選取進入點之後，用戶端電腦將不會還原為自動進入點選取。 也就是說，如果手動選取的進入點變成無法連線，則使用者必須還原為自動進入點選取專案，或手動選取另一個進入點。  
  
 Windows 7 用戶端電腦會使用連線至多網站部署中單一進入點所需的資訊進行設定。 他們無法同時儲存多個進入點的資訊。 例如，您可以將 Windows 7 用戶端電腦設定為連線到美國的進入點，但不能連接到歐洲的進入點。 如果無法連線到美國的進入點，Windows 7 用戶端電腦將會失去與內部網路的連線，直到進入點可連線為止。 使用者無法進行任何變更以嘗試連接到歐洲進入點。  
  
## <a name="36-plan-prefixes-and-routing"></a><a name="bkmk_3_6_IPv6"></a>3.6 方案首碼和路由  
  
### <a name="internal-ipv6-prefix"></a>內部 IPv6 首碼  
在部署單一遠端存取服務器時，您規劃了內部網路 IPv6 首碼，請注意以下在多站部署中的下列各項：  
  
1.  如果您在設定單一伺服器遠端存取部署時包含所有 Active Directory 網站，則會在 [遠端存取管理] 主控台中定義內部網路 IPv6 首碼。  
  
2.  如果您為多網站部署建立額外的 Active Directory 網站，則必須為其他網站規劃新的 IPv6 首碼，並在遠端存取中加以定義。 請注意，IPv6 首碼只能使用 [遠端存取管理] 主控台或 PowerShell Cmdlet 來設定（如果 IPv6 是部署在公司內部網路中）。  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>DirectAccess 用戶端電腦的 IPv6 首碼（IP-HTTPS 首碼）  
  
1.  如果 IPv6 部署在內部的公司網路中，您必須規劃 IPv6 首碼，以在部署中的任何其他進入點指派給 DirectAccess 用戶端電腦。  
  
2.  確定要指派給每個進入點中 DirectAccess 用戶端電腦的 IPv6 首碼是相異的，而且 IPv6 首碼不會重迭。  
  
3.  如果未在公司網路中部署 IPv6，則新增進入點時，會自動選取每個進入點的 IP-HTTPS 首碼。  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>VPN 用戶端的 IPv6 首碼  
如果您已在單一遠端存取服務器上部署 VPN，請注意下列事項：  
  
1.  只有當您想要允許 VPN 用戶端 IPv6 連線到公司網路時，才需要將 IPv6 VPN 首碼新增到進入點。  
  
2.  只有在公司內部網路中部署 IPv6，且進入點上已啟用 VPN 時，才可以使用 [遠端存取管理] 主控台或 PowerShell Cmdlet 在進入點上設定 VPN 首碼。  
  
3.  VPN 首碼在每個進入點中應該是唯一的，而且不應該與其他 VPN 或 ip-HTTPs 首碼重迭。  
  
4.  如果未在公司網路中部署 IPv6，連線到進入點的 VPN 用戶端將不會被指派 IPv6 位址。  
  
### <a name="routing"></a>路由  
在多網站部署中，對稱路由會使用 Teredo 和 IP-HTTPS 來強制執行。 在公司網路中部署 IPv6 時，請注意下列事項：  
  
1. 每個進入點的 Teredo 和 IP-HTTPS 首碼必須可透過公司網路路由傳送到其相關聯的遠端存取服務器。  
  
2. 路由必須在公司網路路由基礎結構中設定。  
  
3. 針對每個進入點，內部網路中應該有一到三個路由：  
  
   1. IP-HTTPS 首碼-系統管理員會在 [新增進入點嚮導] 中選擇此首碼。  
  
   2. VPN IPv6 首碼（選擇性）。 為進入點啟用 VPN 之後，可以選擇此前置詞  
  
   3. Teredo 首碼（選擇性）。 只有在外部介面卡上使用兩個連續的公用 IPv4 位址設定遠端存取服務器時，這個前置詞才會相關。 前置詞是以位址配對的第一個公用 IPv4 位址為基礎。 例如，如果外部地址為：  
  
      1. www\.xxx. yyy. zzz  
  
      2. www\.xxx. zzz + 1  
  
      然後，要設定的 Teredo 首碼是2001：0： WWXX： YYZZ：：/64，其中 WWXX： YYZZ 是 IPv4 位址 www\.xxx. zzz 的十六進位標記法。  
  
      請注意，您可以使用下列腳本來計算 Teredo 首碼：  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. 所有上述路由都必須路由傳送至遠端存取服務器的內部介面卡上的 IPv6 位址（或負載平衡進入點的內部虛擬 IP （VIP）位址）。  
  
> [!NOTE]  
> 在公司網路中部署 IPv6，且遠端存取服務器管理是透過 DirectAccess 從遠端執行時，必須將所有其他進入點的 Teredo 和 IP-HTTPS 首碼的路由新增至每部遠端存取服務器，這樣流量才會是轉送到內部網路。  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory 網站特定 IPv6 首碼  
當執行 Windows 10 或 Windows 8 的用戶端電腦連線到進入點時，用戶端電腦會立即與進入點的 Active Directory 網站相關聯，並使用與該進入點相關聯的 IPv6 首碼進行設定。 喜好設定是讓用戶端電腦使用這些 IPv6 首碼來連線到資源，因為它們是在連接到進入點時，以較高的優先順序，在 IPv6 首碼原則表格中以動態方式設定。  
  
如果您的組織使用含有網站特定 IPv6 首碼的 Active Directory 拓撲（例如，內部資源 FQDN app.corp.com 同時裝載于北美洲和歐洲，且每個位置都有網站特定的 IP 位址），則不會設定此項預設會使用遠端存取主控台，而且不會針對每個進入點設定網站特定的 IPv6 首碼。 如果您想要啟用此選擇性案例，您必須使用特定的 IPv6 首碼來設定每個進入點，用戶端電腦應該將這些前置詞連接到特定的進入點。 執行此動作，如下所示：  
  
1.  針對 Windows 10 或 Windows 8 用戶端電腦所使用的每個 GPO，執行 DAEntryPointTableItem PowerShell Cmdlet  
  
2.  使用網站特定的 IPv6 首碼，設定 Cmdlet 的 EntryPointRange 參數。 例如，若要新增網站專屬的首碼2001： db8：1：1：：/64 和2001： db：1：2：：/64 到名為「歐洲」的進入點，請執行下列程式  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  修改 EntryPointRange 參數時，請確定您不會移除屬於 IPsec 通道端點和 DNS64 位址的現有128位首碼。  
  
## <a name="37-plan-the-transition-to-ipv6-when-multisite-remote-access-is-deployed"></a><a name="bkmk_3_7_TransitionIPv6"></a>3.7 規劃在部署多網站遠端存取時轉換為 IPv6  
許多組織會在公司網路上使用 IPv4 通訊協定。 在耗盡可用的 IPv4 首碼之後，許多組織都會從 IPv4 轉換成僅 IPv6 網路。  
  
這項轉換最有可能發生在兩個階段：  
  
1.  從僅 IPv4 到 IPv6 + IPv4 的公司網路。  
  
2.  從 IPv6 + IPv4 到 IPv6 專用的公司網路。  
  
在每個部分中，可能會分階段執行轉換。 在每個階段中，網路的一個子網可能會變更為新的網路設定。 因此，需要 DirectAccess 多網站部署才能支援混合式部署，例如，某些進入點屬於僅支援 IPv4 的子網，其他則屬於 IPv6 + IPv4 子網。 此外，轉換程式期間的設定變更不得透過 DirectAccess 中斷用戶端連線。  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6ipv4-corporate-network"></a><a name="TransitionIPv4toMixed"></a>從僅 IPv4 轉換至 IPv6 + IPv4 公司網路  
將 IPv6 位址新增到僅 IPv4 的公司網路時，您可能會想要將 IPv6 位址新增至已部署的 DirectAccess 伺服器。 此外，您可能會想要將進入點或節點新增至具有 IPv4 和 IPv6 位址的負載平衡叢集，以進行 DirectAccess 部署。  
  
遠端存取可讓您將具有 IPv4 和 IPv6 位址的伺服器，新增至原本只使用 IPv4 位址設定的部署。 這些伺服器會新增為僅 IPv4 伺服器，而 DirectAccess 會忽略其 IPv6 位址;因此，您的組織無法利用這些新伺服器上的原生 IPv6 連線優勢。  
  
若要將部署轉換成 IPv6 + IPv4 部署，並利用原生 IPv6 功能，您必須重新安裝 DirectAccess。 若要維護整個重新安裝的用戶端連線，請參閱使用雙重 DirectAccess 部署，從僅 IPv4 轉換為僅限 IPv6 的部署。  
  
> [!NOTE]  
> 如同 IPv4 專用的網路，在混合的 IPv4 + IPv6 網路中，用來解析用戶端 DNS 要求的 DNS 伺服器位址，必須使用部署于遠端存取服務器本身的 DNS64 來設定，而不是使用公司 DNS。  
  
### <a name="transition-from-an-ipv6ipv4-to-an-ipv6-only-corporate-network"></a><a name="TransitionMixedtoIPv6"></a>從 IPv6 + IPv4 轉換到僅 IPv6 的公司網路  
只有當部署中的第一部遠端存取服務器最初同時具有 IPv4 和 IPv6 位址，或只有 IPv6 位址時，DirectAccess 才可讓您新增 IPv6 專用的進入點。 也就是說，您無法在單一步驟中從僅 IPv4 網路轉換成 IPv6 私人網路絡，而不需要重新安裝 DirectAccess。 若要從僅 IPv4 網路直接轉換為僅限 IPv6 的網路，請參閱使用雙重 DirectAccess 部署從僅 IPv4 轉換為僅限 IPv6 的部署。  
  
當您完成從僅 IPv4 部署到 IPv6 + IPv4 部署的轉換之後，您可以轉換到 IPv6 私人網路絡。 在轉換期間和之後，請注意下列事項：  
  
-   如果有任何僅 IPv4 的後端伺服器保留在公司網路中，將無法連線到透過 IPv6 專用進入點連線的用戶端。  
  
-   將僅限 IPv6 的進入點新增至 IPv4 + IPv6 部署時，將不會在新的伺服器上啟用 DNS64 和 NAT64。 連接到這些進入點的用戶端將會自動設定為使用公司 DNS 伺服器。  
  
-   如果您需要從已部署的伺服器刪除 IPv4 位址，您必須從 DirectAccess 部署中移除該伺服器、移除其 IPv4 公司網路位址，然後再次將它新增至部署。  
  
若要支援用戶端連線到公司網路，您必須確定網路位置伺服器可以由公司 DNS 解析為其 IPv6 位址。 另一個 IPv4 位址也可以設定，但不是必要的。  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6-only-deployment-using-dual-directaccess-deployments"></a><a name="DualDeployment"></a>使用雙重 DirectAccess 部署，從僅 IPv4 轉換為僅限 IPv6 的部署  
不需要重新安裝 DirectAccess 部署，就能從僅 IPv4 轉換成僅 IPv6 的公司網路。 若要在轉換期間維護用戶端連線能力，您可以使用另一個 DirectAccess 部署。 當第一個轉換階段完成時（僅 IPv4 網路已升級至 IPv4 + IPv6），而且您想要準備未來轉換至僅 IPv6 的公司網路 orto 時，需要進行雙重部署，以利用原生 IPv6 連線能力的優勢。 雙重部署的說明請見下列一般步驟：  
  
1.  安裝第二個 DirectAccess 部署。 您可以在新的伺服器上安裝 DirectAccess，或從第一個部署中移除伺服器，並將它們用於第二個部署。  
  
    > [!NOTE]  
    > 當您同時安裝其他 DirectAccess 部署時，請確定沒有任何兩個進入點共用相同的用戶端前置詞。  
    >   
    > 如果您使用消費者入門 Wizard 或 Cmdlet `Install-RemoteAccess`來安裝 DirectAccess，遠端存取會自動將部署中第一個進入點的用戶端首碼設定為預設值 < IPv6 子網\_首碼 >：1000：：/64。 如有需要，您必須變更前置詞。  
  
2.  從第一個部署中移除選擇的用戶端安全性群組。  
  
3.  將用戶端安全性群組新增至第二個部署。  
  
    > [!IMPORTANT]  
    > 若要在整個過程中維持用戶端連線，您必須在從第一個部署中移除安全性群組之後，立即將它們新增至第二個部署。 這可確保不會使用兩個或零個 DirectAccess Gpo 來更新用戶端。 用戶端會在取得並更新其用戶端 GPO 之後，開始使用第二個部署。  
  
4.  選擇性：從第一個部署移除 DirectAccess 進入點，並在第二個中將這些伺服器新增為新的進入點。  
  
當您完成轉換時，可以卸載第一個 DirectAccess 部署。 卸載時，可能會發生下列問題：  
  
-   如果部署已設定為僅支援行動電腦上的用戶端，則會刪除 WMI 篩選器。 如果第二個部署的用戶端安全性群組包含桌上型電腦，DirectAccess 用戶端 GPO 將不會篩選桌上型電腦，而且可能會造成問題。 如果需要行動電腦篩選器，請遵循[建立 GPO 的 WMI 篩選器](https://technet.microsoft.com/library/cc947846.aspx)上的指示來加以重建。  
  
-   如果這兩個部署最初都是在相同的 Active Directory 網域上建立，則會刪除指向 localhost 的 DNS 探查專案，而且可能會導致用戶端連線問題。 例如，用戶端可以使用 IP-HTTPS 而不是 Teredo 來連線，或在 DirectAccess 多網站進入點之間切換。 在此情況下，您必須將下列 DNS 專案新增至公司 DNS：  
  
    -   區域：功能變數名稱  
  
    -   名稱： directaccess-Directaccess-corpconnectivityhost  
  
    -   IP 位址：：：1  
  
    -   類型： AAAA  
  
  
  


