---
title: 步驟3規劃多網站部署
description: 瞭解如何規劃任何其他憑證需求、用戶端電腦選取進入點的方式，以及在您的部署中指派的 IPv6 位址。
manager: brianlic
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 2758efb07b6dd601bfca8789ddf5ec430e418325
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941674"
---
# <a name="step-3-plan-the-multisite-deployment"></a>步驟3規劃多網站部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

規劃多網站基礎結構之後，請規劃任何其他憑證需求、用戶端電腦選取進入點的方式，以及部署中指派的 IPv6 位址。

下列各節提供詳細的規劃資訊。

## <a name="31-plan-ip-https-certificates"></a><a name="bkmk_3_1_IPHTTPS"></a>3.1 規劃 IP-HTTPS 憑證
當您設定進入點時，會使用特定的 ConnectTo 位址來設定每個進入點。 每個進入點的 IP-HTTPS 憑證必須符合 ConnectTo 位址。 取得憑證時，請注意下列事項：

-   您無法在多站台部署中使用自我簽署憑證。

-   建議使用公用 CA，以便隨時可用 CRL。

-   在 [主旨] 欄位中，指定遠端存取服務器之外部介面卡的 IPv4 位址 (如果已將 ConnectTo 位址指定為 IP 位址而非 DNS 名稱) 或 ip-HTTPs URL 的 FQDN。

-   憑證的一般名稱應該符合 IP-HTTPS 網站的名稱。 也支援使用符合 ConnectTo DNS 名稱的萬用字元 URL。

-   Ip-HTTPs 憑證可以在主體名稱中使用萬用字元。 相同的萬用字元憑證可用於所有進入點。

-   在「增強金鑰使用方法」欄位中使用伺服器驗證物件識別碼 (OID)。

-   如果您要支援在多網站部署中執行 Windows 7 的用戶端電腦，請在 [CRL 發佈點] 欄位中，指定已連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。 執行 Windows 8 的用戶端並不需要這麼做 (根據預設，在這些用戶端) 上，會停用對 ip-HTTPs 的 CRL 撤銷檢查。

-   IP-HTTPS 憑證必須具有私密金鑰。

-   Ip-HTTPs 憑證必須直接匯入電腦的個人存放區，而不是使用者。

## <a name="32-plan-the-network-location-server"></a><a name="bkmk_3_2_NLS"></a>3.2 規劃網路位置伺服器
網路位置伺服器網站可以裝載于遠端存取服務器或組織中的另一部伺服器上。 如果您在遠端存取服務器上裝載網路位置伺服器，則會在您部署遠端存取時自動建立網站。 如果您在組織中執行 Windows 作業系統的另一部伺服器上裝載網路位置伺服器，則必須確定已安裝 Internet Information Services (IIS) 以建立網站。

### <a name="321-certificate-requirements-for-the-network-location-server"></a>網路位置伺服器的3.2.1 憑證需求
請確定網路位置伺服器網站符合下列憑證部署需求：

-   它需要 HTTPS 伺服器憑證。

-   如果網路位置伺服器位於遠端存取服務器上，而且您在部署單一遠端存取服務器時選擇使用自我簽署憑證，則必須將單一伺服器部署重新設定為使用內部 CA 所發行的憑證。

-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。

-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器網站的名稱。

-   網路位置伺服器網站必須高度可供內部網路上的電腦使用。

-   網路位置伺服器必須不可被網際網路上的 DirectAccess 用戶端電腦存取。

-   您必須針對 (CRL) 的憑證撤銷清單檢查伺服器憑證。

-   在遠端存取服務器上託管網路位置伺服器時，不支援萬用字元憑證。

取得要用於網路位置伺服器的網站憑證時，請注意下列事項：

1.  在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或是網路位置 URL 的 FQDN。 請注意，如果網路位置伺服器是裝載在遠端存取服務器上，則不應指定 IP 位址。 這是因為網路位置伺服器必須針對所有進入點使用相同的主體名稱，而且並非所有的進入點都有相同的 IP 位址。

2.  在 [增強金鑰使用方法] 欄位中使用伺服器驗證 OID。

3.  針對 [CRL 發佈點] 欄位，使用已連線到內部網路的 DirectAccess 用戶端可存取的 CRL 發佈點。

### <a name="322dns-for-the-network-location-server"></a>網路位置伺服器的 3.2.2 DNS
如果您在遠端存取服務器上裝載網路位置伺服器，則必須針對部署中的每個進入點，新增網路位置伺服器網站的 DNS 專案。 請注意：

-   在多網站部署中，第一個網路位置伺服器憑證的主體名稱會當做所有進入點的網路位置伺服器 URL 使用，因此主體名稱和網路位置伺服器 URL 不能與部署中第一部遠端存取服務器的電腦名稱稱相同。 它必須是網路位置伺服器專用的 FQDN。

-   網路位置伺服器流量所提供的服務會在使用 DNS 的進入點之間達到平衡，因此每個進入點都應該有一個具有相同 URL 的 DNS 專案，並以進入點的內部 IP 位址設定。

-   所有進入點都必須使用具有相同主體名稱的網路位置伺服器憑證進行設定， (符合網路位置伺服器 URL) 。

-   在新增進入點之前，必須先建立網路位置伺服器基礎結構 (DNS 和憑證設定) 作為進入點。

## <a name="33-plan-the-ipsec-root-certificate-for-all-remote-access-servers"></a><a name="bkmk_3_3_IPsec"></a>3.3 規劃所有遠端存取服務器的 IPsec 根憑證
規劃多網站部署中的 IPsec 用戶端驗證時，請注意下列事項：

1.  如果您在設定單一遠端存取服務器時選擇使用內建的 Kerberos proxy 進行電腦驗證，則必須將此設定變更為使用由內部 CA 所發行的電腦憑證，因為多網站部署不支援 Kerberos proxy。

2.  如果您使用自我簽署憑證，則必須將單一伺服器部署重新設定為使用內部 CA 所發行的憑證。

3.  在用戶端驗證期間，若要讓 IPsec 驗證成功，所有遠端存取服務器都必須有 IPsec 根或中繼 CA 所發行的憑證，以及使用用戶端驗證 OID 來增強金鑰使用方法。

4.  您必須在多網站部署中的所有遠端存取服務器上安裝相同的 IPsec 根或中繼憑證。

## <a name="34-plan-global-server-load-balancing"></a><a name="bkmk_3_4_GSLB"></a>3.4 規劃全域伺服器負載平衡
在多網站部署中，您可以另外設定全域伺服器負載平衡器。 如果您的部署涵蓋大型地理分佈，則全域伺服器負載平衡器會對您的組織很有用，因為它可以在進入點之間分散流量負載。  您可以設定全域伺服器負載平衡器，為 DirectAccess 用戶端提供最接近進入點的進入點資訊。 流程的運作方式如下：

1.  執行 Windows 10 或 Windows 8 的用戶端電腦具有全域伺服器負載平衡器 IP 位址的清單，每個位址都與進入點相關聯。

2.  Windows 10 或 Windows 8 用戶端電腦會嘗試將公用 DNS 中全域伺服器負載平衡器的 FQDN 解析為 IP 位址。 如果已解析的 IP 位址列為進入點的全域伺服器負載平衡器 IP 位址，則用戶端電腦會自動選取該進入點，並連接到其 IP-HTTPS URL (ConnectTo address) 或其 Teredo 伺服器 IP 位址。 請注意，全域伺服器負載平衡器的 IP 位址不需要與 ConnectTo 位址或進入點的 Teredo 伺服器位址相同，因為用戶端電腦永遠不會嘗試連接到全域伺服器負載平衡器 IP 位址。

3.  如果用戶端電腦位於 web proxy 後方 (而且無法使用 DNS 解析) ，或如果全域伺服器負載平衡器 FQDN 無法解析為任何已設定的全域伺服器負載平衡器 IP 位址，則會使用 HTTPS 探查將輸入點自動選取至所有進入點的 ip-HTTPs Url。 用戶端將會連接到先回應的伺服器。

如需支援遠端存取的全域伺服器負載平衡裝置清單，請移至 [Microsoft 伺服器和雲端平臺](https://www.microsoft.com/server-cloud/)上的尋找合作夥伴頁面。

## <a name="35-plan-directaccess-client-entry-point-selection"></a><a name="bkmk_3_5_EP_Selection"></a>3.5 規劃 DirectAccess 用戶端進入點選取專案
當您設定多網站部署時，根據預設，Windows 10 和 Windows 8 用戶端電腦會使用連線到部署中所有進入點所需的資訊進行設定，並根據選取的演算法自動連接到單一進入點。 您也可以設定部署，以允許 Windows 10 和 Windows 8 用戶端電腦手動選取要連接的進入點。 若 Windows 10 或 Windows 8 用戶端電腦目前已連線到美國進入點，而且已啟用自動進入點選取，則在幾分鐘之後，用戶端電腦會嘗試透過歐洲進入點連線。 建議使用自動進入點選取專案;不過，允許手動進入點選取可讓使用者根據目前的網路狀況連接到不同的進入點。 例如，如果電腦連線到美國進入點，而與內部網路的連線變得比預期慢很多。 在這種情況下，使用者可以手動選取以連接到歐洲進入點，以改善內部網路的連接。

> [!NOTE]
> 當使用者手動選取進入點之後，用戶端電腦將不會還原為自動進入點選取。 亦即，如果手動選取的進入點無法連線，則使用者必須還原為自動進入點選取，或手動選取其他進入點。

 Windows 7 用戶端電腦會使用連接到多網站部署中單一進入點所需的資訊進行設定。 他們無法同時儲存多個進入點的資訊。 例如，您可以設定 Windows 7 用戶端電腦連接到美國進入點，但不能連線到歐洲進入點。 如果無法連線到美國的進入點，Windows 7 用戶端電腦將會失去與內部網路的連線，直到進入點可連線為止。 終端使用者無法進行任何變更，以嘗試連接到歐洲進入點。

## <a name="36-plan-prefixes-and-routing"></a><a name="bkmk_3_6_IPv6"></a>3.6 規劃首碼和路由

### <a name="internal-ipv6-prefix"></a>內部 IPv6 首碼
在部署單一遠端存取服務器的期間，您規劃了內部網路 IPv6 首碼，請注意多網站部署中的下列各項：

1.  如果您在設定單一伺服器遠端存取部署時包含所有的 Active Directory 網站，則會在遠端存取管理主控台中定義內部網路 IPv6 首碼。

2.  如果您為多網站部署建立額外的 Active Directory 網站，您必須為其他網站規劃新的 IPv6 首碼，並在遠端存取中加以定義。 請注意，IPv6 首碼只能使用遠端存取管理主控台或 PowerShell Cmdlet 來設定，如果 IPv6 是部署在內部公司網路中。

### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>DirectAccess 用戶端電腦的 IPv6 首碼 (IP-HTTPS 首碼) 

1.  如果 IPv6 部署在內部公司網路中，您必須規劃 IPv6 首碼，以指派給部署中任何額外進入點的 DirectAccess 用戶端電腦。

2.  確定在每個進入點中指派給 DirectAccess 用戶端電腦的 IPv6 首碼都不同，而且 IPv6 首碼中沒有重迭。

3.  如果未在公司網路中部署 IPv6，則在新增進入點時，將會自動選取每個進入點的 IP-HTTPS 首碼。

### <a name="ipv6-prefix-for-vpn-clients"></a>VPN 用戶端的 IPv6 首碼
如果您在單一遠端存取服務器上部署 VPN，請注意下列事項：

1.  只有當您想要允許 VPN 用戶端 IPv6 連線到公司網路時，才需要將 IPv6 VPN 首碼新增至進入點。

2.  只有在內部公司網路中已部署 IPv6，且進入點上已啟用 VPN 時，才能使用 [遠端存取管理主控台] 或 PowerShell Cmdlet，在進入點上設定 VPN 首碼。

3.  VPN 首碼在每個進入點都應該是唯一的，而且不應該與其他 VPN 或 ip-HTTPs 首碼重迭。

4.  如果未在公司網路中部署 IPv6，將不會指派 IPv6 位址給連接至進入點的 VPN 用戶端。

### <a name="routing"></a>路由
在多網站部署對稱式路由中，會使用 Teredo 和 IP-HTTPS 來強制執行。 在公司網路中部署 IPv6 時，請注意下列事項：

1. 每個進入點的 Teredo 和 IP-HTTPS 首碼必須可透過公司網路路由傳送到其相關聯的遠端存取服務器。

2. 路由必須設定在公司網路路由基礎結構中。

3. 針對每個進入點，內部網路中應該有一到三個路由：

   1. IP-HTTPS 首碼-在 [新增進入點] 嚮導中，系統管理員會選擇這個前置詞。

   2. VPN IPv6 首碼 (選擇性) 。 啟用進入點的 VPN 之後，可以選擇這個前置詞

   3. Teredo 首碼 (選擇性) 。 只有在遠端存取服務器是在外部介面卡上使用兩個連續的公用 IPv4 位址設定時，此首碼才會相關。 前置詞是以位址組的第一個公用 IPv4 位址為基礎。 例如，如果外部地址為：

      1. www \. xxx. zzz

      2. www \. xxx. zzz + 1

      接下來要設定的 Teredo 前置詞是2001：0： WWXX： YYZZ：：/64，其中 WWXX： YYZZ 是 IPv4 位址 www xxx 的十六進位標記法。 \.

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

   4. 上述所有路由都必須路由至遠端存取服務器內部介面卡上的 IPv6 位址 (或負載平衡進入點) 的內部虛擬 IP (VIP) 位址。

> [!NOTE]
> 當 IPv6 部署在公司網路中，且遠端存取服務器管理是透過 DirectAccess 從遠端執行，則必須將所有其他進入點的 Teredo 和 IP-HTTPS 首碼的路由新增至每部遠端存取服務器，以便將流量轉送到內部網路。

### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory 網站專用 IPv6 首碼
當執行 Windows 10 或 Windows 8 的用戶端電腦連線到進入點時，用戶端電腦會立即與進入點的 Active Directory 網站相關聯，並使用與進入點相關聯的 IPv6 首碼進行設定。 偏好設定是讓用戶端電腦使用這些 IPv6 首碼來連線到資源，因為它們是在 IPv6 首碼原則資料表中以動態方式設定，在連接到進入點時，優先順序較高。

如果您的組織使用 Active Directory 拓撲搭配網站專屬的 IPv6 首碼 (例如，內部資源 FQDN app.corp.com 會在每個位置中以網站專屬的 IP 位址裝載于北美洲和歐洲) 這項設定不會預設為使用遠端存取主控台，而且不會針對每個進入點設定網站特定 IPv6 首碼。 如果您想要啟用此選用案例，您必須使用特定 IPv6 首碼來設定每個進入點，而用戶端電腦應偏好這些前置詞，以連接到特定的進入點。 以下列方式來執行此動作：

1.  針對 Windows 10 或 Windows 8 用戶端電腦所使用的每個 GPO，請執行 Set-DAEntryPointTableItem PowerShell Cmdlet

2.  使用網站特定 IPv6 首碼來設定 Cmdlet 的 EntryPointRange 參數。 例如，若要將網站專屬的首碼2001： db8：1：1：：/64 和2001： db：1：2：：/64 新增至名為「歐洲」的進入點，請執行下列程式

    ```
    $entryPointName = "Europe"
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")
    $clientGpos = (Get-DAClient).GpoName
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}
    ```

3.  修改 EntryPointRange 參數時，請確定您不會移除屬於 IPsec 通道端點和 DNS64 位址的現有128位首碼。

## <a name="37-plan-the-transition-to-ipv6-when-multisite-remote-access-is-deployed"></a><a name="bkmk_3_7_TransitionIPv6"></a>3.7 規劃在部署多網站遠端存取時轉換為 IPv6
許多組織會在公司網路上使用 IPv4 通訊協定。 使用可用的 IPv4 首碼時，許多組織都會將從 IPv4 轉換為僅限 IPv6 的網路。

這項轉換最有可能發生在兩個階段：

1.  從 IPv4 到 IPv6 + IPv4 公司網路。

2.  從 IPv6 + IPv4 到僅限 IPv6 的公司網路。

在每個部分中，轉換可能會以階段執行。 在每個階段中，只有一個網路子網可能會變更為新的網路設定。 因此，需要 DirectAccess 多網站部署以支援混合式部署，例如，某些進入點屬於僅限 IPv4 的子網，其他則屬於 IPv6 + IPv4 子網。 此外，轉換程式期間的設定變更不能透過 DirectAccess 中斷用戶端連線。

### <a name="transition-from-an-ipv4-only-to-an-ipv6ipv4-corporate-network"></a><a name="TransitionIPv4toMixed"></a>從 IPv4 轉換至 IPv6 + IPv4 公司網路
將 IPv6 位址新增至僅限 IPv4 的公司網路時，您可能會想要將 IPv6 位址新增至已部署的 DirectAccess 伺服器。 此外，您可能會想要將進入點或節點新增至負載平衡叢集，並將 IPv4 和 IPv6 位址新增至 DirectAccess 部署。

遠端存取可讓您將具有 IPv4 和 IPv6 位址的伺服器新增至原本設定為僅使用 IPv4 位址的部署。 這些伺服器會新增為僅限 IPv4 的伺服器，而 DirectAccess 會忽略其 IPv6 位址;因此，您的組織無法利用這些新伺服器上原生 IPv6 連線的優點。

若要將部署轉換為 IPv6 + IPv4 部署並利用原生 IPv6 功能，您必須重新安裝 DirectAccess。 若要在整個重新安裝期間維持用戶端連線，請參閱使用雙重 DirectAccess 部署，從 IPv4 轉換為僅限 IPv6 的部署。

> [!NOTE]
> 如同純 IPv4 網路，在混合的 IPv4 + IPv6 網路中，用來解析用戶端 DNS 要求的 DNS 伺服器位址，必須使用部署于遠端存取服務器本身的 DNS64 來設定，而不是使用公司 DNS 來設定。

### <a name="transition-from-an-ipv6ipv4-to-an-ipv6-only-corporate-network"></a><a name="TransitionMixedtoIPv6"></a>從 IPv6 + IPv4 轉換至僅限 IPv6 的公司網路
只有當部署中的第一部遠端存取服務器最初具有 IPv4 和 IPv6 位址，或只有 IPv6 位址時，DirectAccess 才可讓您新增 IPv6 專用的進入點。 也就是說，您無法在單一步驟中將僅限 IPv4 的網路轉換成僅限 IPv6 的網路，而不需要重新安裝 DirectAccess。 若要直接從僅限 IPv4 的網路轉換至 IPv6 專用的網路，請參閱使用雙重 DirectAccess 部署從僅限 IPv4 轉換為僅限 IPv6 的部署。

完成從僅限 IPv4 部署到 IPv6 + IPv4 部署的轉換之後，您可以轉換為僅限 IPv6 的網路。 在轉換期間和之後，請注意下列事項：

-   如果任何僅 IPv4 的後端伺服器保留在公司網路中，則無法透過 IPv6 的進入點連接用戶端。

-   將僅 IPv6 的進入點新增至 IPv4 + IPv6 部署時，不會在新的伺服器上啟用 DNS64 和 NAT64。 連接到這些進入點的用戶端會自動設定為使用公司 DNS 伺服器。

-   如果您需要從已部署的伺服器中刪除 IPv4 位址，則必須從 DirectAccess 部署中移除該伺服器、移除其 IPv4 公司網路位址，然後再次將其新增至部署。

若要支援與公司網路的用戶端連線，您必須確定網路位置伺服器可由公司 DNS 解析為其 IPv6 位址。 您也可以設定額外的 IPv4 位址，但不是必要的。

### <a name="transition-from-an-ipv4-only-to-an-ipv6-only-deployment-using-dual-directaccess-deployments"></a><a name="DualDeployment"></a>使用雙重 DirectAccess 部署從 IPv4 轉換為僅限 IPv6 的部署
無法在不重新安裝 DirectAccess 部署的情況下，從 IPv4 轉換為僅限 IPv6 的公司網路。 若要在轉換期間維護用戶端連線能力，您可以使用另一個 DirectAccess 部署。 當第一個轉換階段完成時，需要雙重部署 (僅限 IPv4 的網路升級至 IPv4 + IPv6) ，而您想要準備未來轉換至僅限 IPv6 的公司網路 orto 利用原生 IPv6 連線能力優勢。 下列一般步驟會說明雙重部署：

1.  安裝第二個 DirectAccess 部署。 您可以在新的伺服器上安裝 DirectAccess，或從第一個部署中移除伺服器，並在第二個部署中使用它們。

    > [!NOTE]
    > 將其他 DirectAccess 部署與目前的部署一起安裝時，請確定沒有兩個進入點共用相同的用戶端首碼。
    >
    > 如果您使用消費者入門 Wizard 或 Cmdlet 來安裝 DirectAccess `Install-RemoteAccess` ，遠端存取會自動將部署中第一個進入點的用戶端前置詞設定為預設值 <IPv6 子網 \_ 首碼>：1000::/64。 如有需要，您必須變更前置詞。

2.  從第一個部署中移除所選的用戶端安全性群組。

3.  將用戶端安全性群組新增至第二個部署。

    > [!IMPORTANT]
    > 若要維護整個進程的用戶端連線，您必須在將安全性群組從第一個部署中移除之後，立即將安全性群組新增至第二個部署。 這可確保不會以兩個或零個 DirectAccess Gpo 更新用戶端。 用戶端會在取得並更新其用戶端 GPO 之後，開始使用第二個部署。

4.  選用：從第一個部署中移除 DirectAccess 進入點，然後將這些伺服器新增為第二個新的進入點。

當您完成轉換時，可以卸載第一個 DirectAccess 部署。 卸載時，可能會發生下列問題：

-   如果將部署設定為僅支援行動電腦上的用戶端，則會刪除 WMI 篩選器。 如果第二個部署的用戶端安全性群組包含桌上型電腦，則 DirectAccess 用戶端 GPO 不會篩選桌上型電腦，而且可能會造成問題。 如果需要行動電腦篩選器，請依照針對 [GPO 建立 WMI 篩選器](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc947846(v=ws.10))上的指示來重新建立。

-   如果這兩個部署最初是建立在相同的 Active Directory 網域上，則指向 localhost 的 DNS 探查專案將會遭到刪除，而且可能會導致用戶端連線問題。 例如，用戶端可以使用 IP-HTTPS （而不是 Teredo）進行連線，或在 DirectAccess 多網站進入點之間切換。 在此情況下，您必須將下列 DNS 專案新增至公司 DNS：

    -   區域：功能變數名稱

    -   名稱： directaccess-Directaccess-corpconnectivityhost

    -   IP 位址：：：1

    -   類型： AAAA



