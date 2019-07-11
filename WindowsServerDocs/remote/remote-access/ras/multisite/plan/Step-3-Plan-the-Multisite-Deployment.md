---
title: 步驟 3 計畫多站台部署
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 973ef70614f056adac1463918cc425d82b21ac62
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792306"
---
# <a name="step-3-plan-the-multisite-deployment"></a>步驟 3 計畫多站台部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

規劃之後的多站台的基礎結構，規劃任何額外的憑證需求，用戶端電腦的選取方式的進入點，並在您的部署中指派的 IPv6 位址。  

下列各節提供詳細的規劃資訊。
  
## <a name="bkmk_3_1_IPHTTPS"></a>3.1 規劃 IP-HTTPS 憑證  
當您設定的進入點時，您會使用特定的 ConnectTo 位址設定每個進入點。 每個進入點的 IP-HTTPS 憑證必須符合 ConnectTo 位址。 取得憑證時，請注意下列：  
  
-   您無法在多站台部署中使用自我簽署憑證。  
  
-   建議使用公用 CA，以便隨時可用 CRL。  
  
-   在 [主旨] 欄位中指定的遠端存取伺服器 （如果已指定 ConnectTo 位址為 IP 位址和 DNS 名稱） 的外部介面卡的 IPv4 位址或 IP-HTTPS URL 的 FQDN。  
  
-   憑證一般名稱應該符合 IP-HTTPS 網站的名稱。 也支援使用萬用字元 URL 符合 ConnectTo DNS 名稱。  
  
-   IP-HTTPS 憑證主體名稱中，可以使用萬用字元。 相同的萬用字元憑證可以用於所有進入點。  
  
-   在「增強金鑰使用方法」欄位中使用伺服器驗證物件識別碼 (OID)。  
  
-   如果您要支援在多站台部署中，執行 Windows 7 的用戶端電腦在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可以存取的 CRL 發佈點。 這不需要執行 （依預設，CRL 的撤銷檢查為 ip-https 停用這些用戶端） 的 Windows 8 的用戶端。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須匯入直接的電腦，而不是使用者的個人存放區。  
  
## <a name="bkmk_3_2_NLS"></a>3.2 規劃網路位置伺服器  
遠端存取伺服器上或您組織中的另一部伺服器可以裝載網路位置伺服器網站。 如果您裝載網路位置伺服器遠端存取伺服器上的，網站會在您部署遠端存取時自動建立。 如果您裝載網路位置伺服器，在您組織中執行 Windows 作業系統的另一部伺服器上，您必須確定 Internet Information Services (IIS) 已安裝以建立網站。  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 憑證的網路位置伺服器的需求  
請確定網路位置伺服器網站符合下列需求的憑證部署：  
  
-   它需要 HTTPS 伺服器憑證。  
  
-   如果網路位置伺服器位於遠端存取伺服器上，而且您選取要使用自我簽署的憑證，當您部署單一遠端存取伺服器時，您必須重新設定單一伺服器部署，以使用內部 CA 所核發的憑證。  
  
-   DirectAccess 用戶端電腦必須信任簽發伺服器憑證給網路位置伺服器網站的 CA。  
  
-   內部網路上的 DirectAccess 用戶端電腦必須能夠解析網路位置伺服器網站的名稱。  
  
-   網路位置伺服器網站必須是高度可用內部網路上的電腦。  
  
-   網路位置伺服器必須不可被網際網路上的 DirectAccess 用戶端電腦存取。  
  
-   必須針對憑證撤銷清單 (CRL) 檢查伺服器憑證。  
  
-   當網路位置伺服器裝載於遠端存取伺服器不支援萬用字元憑證。  
  
取得要用於網路位置伺服器網站憑證，請注意下列各項：  
  
1.  在 [主體] 欄位中，指定網路位置伺服器之內部網路介面的 IP 位址，或是網路位置 URL 的 FQDN。 請注意，您應該指定 IP 位址，是否網路位置伺服器裝載在遠端存取伺服器。 這是因為網路位置伺服器必須使用相同的主體名稱的所有進入點，而且並非所有的進入點有相同的 IP 位址。  
  
2.  在 [增強金鑰使用方法] 欄位中使用伺服器驗證 OID。  
  
3.  CRL 發佈點 欄位中，使用已連線到內部網路的 DirectAccess 用戶端存取的 CRL 發佈點。  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2DNS 網路位置伺服器  
如果您裝載網路位置伺服器遠端存取伺服器上的，您必須在您的部署中新增每個進入點的網路位置伺服器網站的 DNS 項目。 請注意下列事項：  
  
-   在多站台部署中第一次的網路位置伺服器憑證的主體名稱是網路位置伺服器 URL 的所有進入點，因此主體名稱和網路位置伺服器 URL 不能相同的電腦名稱在部署中的第一部遠端存取伺服器。 它必須是專用於網路位置伺服器的 FQDN。  
  
-   進入點使用 DNS 時，就會平衡的網路位置伺服器流量所提供的服務，因此應該會有與每個進入點，以進入點的內部 IP 位址設定的相同 URL 的 DNS 項目。  
  
-   所有進入點必須都設有相同的主體名稱 （也就符合網路位置伺服器 URL） 的網路位置伺服器憑證。  
  
-   新增的進入點之前，必須建立的網路位置伺服器基礎結構 （DNS 和憑證設定） 的進入點。  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 計劃的所有遠端存取伺服器的 IPsec 根憑證  
規劃用於 IPsec 在站台部署中的用戶端驗證時，請注意下列：  
  
1.  如果您選擇使用內建的 Kerberos proxy 進行電腦驗證，當您設定單一遠端存取伺服器時，您必須變更設定，才能使用 Kerberos proxy 不支援多站台之後，由內部 CA 中，發出電腦憑證部署。  
  
2.  如果您使用自我簽署的憑證，您必須重新設定單一伺服器部署，以使用內部 CA 所核發的憑證。  
  
3.  若要在用戶端驗證期間成功的 IPsec 驗證，所有遠端存取伺服器必須擁有憑證，簽發的 IPsec 根或中繼 CA，以及用戶端驗證 OID 的增強金鑰使用方法。  
  
4.  所有多站台部署遠端存取伺服器上必須安裝相同的 IPsec 根或中繼憑證。  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 計畫全域伺服器負載平衡  
在多站台部署中，您可另外設定全域伺服器負載平衡器。 如果您的部署涵蓋的大型的地理分佈，因為它可以將進入點之間的流量負載，可能會有用到您的組織全域伺服器負載平衡器。  全域伺服器負載平衡器可以設定為 DirectAccess 用戶端提供的最接近的進入點的項目點資訊。 此程序的運作方式，如下所示：  
  
1.  執行 Windows 10 或 Windows 8 的用戶端電腦有一份全域伺服器負載平衡器 IP 位址，每個相關聯的進入點。  
  
2.  Windows 10 或 Windows 8 用戶端電腦會嘗試在公用 DNS 中的全域伺服器負載平衡器將 FQDN 解析到 IP 位址。 如果解析的 IP 位址會列示為全域伺服器負載平衡器 IP 位址，進入點，用戶端電腦會自動選取該進入點，並連接至其 IP-HTTPS URL （ConnectTo 位址），或其 Teredo 伺服器 IP 位址。 請注意，全域伺服器負載平衡器的 IP 位址不需要為相同的 ConnectTo 位址，或到 Teredo 伺服器位址的進入點，因為用戶端電腦永遠不會嘗試連線到全域伺服器負載平衡器 IP 位址。  
  
3.  如果用戶端電腦位於 web proxy （且無法使用 DNS 解析），或如果全域伺服器負載平衡器 FQDN 無法解析為任何已設定全域伺服器負載平衡器 IP 位址，則將使用 HTTPS 探查，使其自動選取的進入點所有進入點的 IP-HTTPS Url。 用戶端會連線到第一次回應的伺服器。  
  
全域伺服器負載平衡支援遠端存取的裝置清單，請移至 尋找合作夥伴頁面[Microsoft 伺服器及雲端平台](https://www.microsoft.com/server-cloud/)。  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 規劃 DirectAccess 用戶端進入點選取範圍  
當您設定多站台部署中，根據預設，Windows 10 和 Windows 8 用戶端電腦設定資訊所需連線至部署中的所有進入點，並自動連接到單一進入點，根據選取項目演算法。 您也可以設定您的部署，以允許 Windows 10 和 Windows 8 用戶端電腦，以手動方式選取的項目點會連線。 如果 Windows 10 或 Windows 8 的用戶端電腦目前連線至美國境內的進入點，並啟用自動進入點選取時，如果美國進入點變成無法連線，在幾分鐘後用戶端電腦會嘗試連接透過歐洲的進入點。 建議您將使用自動進入點選取;不過，允許選取項目可讓使用者連線到不同進入點，根據目前網路狀況的手動進入點。 比方說，如果電腦連線的美國境內的進入點及連接到內部網路變得比預期慢。 在此情況下，使用者可以手動選取連接到歐洲的進入點，以改善內部網路的連線。  
  
> [!NOTE]  
> 終端使用者以手動方式選取的進入點之後，用戶端電腦將不會還原為自動進入點選取範圍。 也就是如果以手動方式選取的項目點無法連線到終端使用者必須還原成自動進入點選取範圍，或手動選取另一個進入點。  
  
 Windows 7 用戶端電腦已連線到多站台部署中的單一進入點所需的資訊。 它們不能同時儲存多個進入點的資訊。 比方說，Windows 7 用戶端電腦可以設定連接到美國的進入點，但不是歐洲的進入點。 如果美國進入點無法連線時，Windows 7 用戶端電腦將無法連線到內部網路之前的進入點可連線。 使用者無法進行任何變更，以嘗試連接到歐洲進入點。  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 規劃前置詞和路由  
  
### <a name="internal-ipv6-prefix"></a>內部 IPv6 首碼  
單一的遠端存取部署期間規劃內部網路 IPv6 首碼的伺服器，請注意多站台部署中的下列：  
  
1.  如果您包含所有您的 Active Directory 站台設定您的單一伺服器部署 「 遠端存取時，將已在遠端存取管理主控台中定義的內部網路 IPv6 首碼。  
  
2.  如果您建立的多站台部署的其他 Active Directory 站台時，您必須規劃的其他站台的新 IPv6 首碼，並在遠端存取中定義它們。 請注意，只能使用遠端存取管理主控台或 PowerShell cmdlet，如果 IPv6 已部署在公司內部網路設定 IPv6 首碼。  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>DirectAccess 用戶端電腦 （若為 IP-HTTPS 首碼） 的的 IPv6 首碼  
  
1.  如果 IPv6 已部署在公司內部網路中，您必須規劃 IPv6 首碼指派給 DirectAccess 用戶端電腦，在您的部署中的任何其他的進入點。  
  
2.  請確定將指派給每個進入點中的 DirectAccess 用戶端電腦的 IPv6 首碼相異，且沒有 IPv6 前置詞中的沒有重疊。  
  
3.  如果公司網路中沒有部署 IPv6，新增的進入點時，將會自動選取每個進入點的 IP-HTTPS 首碼。  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>VPN 用戶端的 IPv6 首碼  
如果您部署單一遠端存取伺服器上的 VPN，請注意下列各項：  
  
1.  新增的 Ipsec、ipv6 VPN 前置詞的進入點是只需要您如果想要允許 VPN 用戶端的 IPv6 連線到公司網路。  
  
2.  只能如果 IPv6 已部署在內部的公司網路中，而且如果進入點上啟用 VPN，而且使用遠端存取管理主控台或 PowerShell 指令程式的進入點上設定 VPN 的前置詞。  
  
3.  則 VPN 首碼應該是唯一在每個進入點，而且應該不會與其他 VPN 或 IP-HTTPS 首碼重疊。  
  
4.  如果公司網路中沒有部署 IPv6，連接到進入點的 VPN 用戶端將不會指派 IPv6 位址。  
  
### <a name="routing"></a>路由  
在站台部署對稱式路由是強制執行使用 Teredo 與 IP-HTTPS。 當公司網路中部署 IPv6 時請注意下列各項：  
  
1. 每個進入點的 Teredo 和 IP-HTTPS 首碼必須可路由傳送到其相關聯的遠端存取伺服器在公司網路上。  
  
2. 路由必須設定公司網路路由基礎結構中。  
  
3. 每個進入點應該有一到三個路由在內部網路中：  
  
   1. IP-HTTPS 首碼此前置詞是系統管理員選擇在 [新增進入點] 精靈。  
  
   2. VPN IPv6 前置詞 （選擇性）。 此前置詞可以選擇啟用 VPN，做為進入點之後  
  
   3. Teredo 首碼 （選擇性）。 此前置詞是兩個連續公用 IPv4 位址的外部介面卡上設定遠端存取伺服器時，才適用。 前置詞根據位址組的第一個公用 IPv4 位址。 如果外部位址的範例：  
  
      1. www\.xxx.yyy.zzz  
  
      2. www\.xxx.yyy.zzz+1  
  
      若要設定 Teredo 首碼則 2001:0:WWXX:YYZZ:: / 64，其中 WWXX:YYZZ 是 IPv4 位址 www 的十六進位表示法\.xxx.yyy.zzz。  
  
      請注意，您可以使用下列指令碼來計算 Teredo 首碼：  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. 所有上述的路由必須路由傳送到遠端存取伺服器的內部介面卡上的 IPv6 位址 （或內部的虛擬 IP (VIP) 位址的負載平衡的進入點）。  
  
> [!NOTE]  
> 透過 DirectAccess，Teredo 路由 IPv6 遠端存取伺服器的系統管理與公司網路中的部署時從遠端執行，而且的所有其他進入點的 IP-HTTPS 首碼必須新增至每個遠端存取伺服器，以便將可轉送到內部網路。  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory 站台特定的 IPv6 首碼  
執行 Windows 10 或 Windows 8 的用戶端電腦連線到進入點，用戶端電腦會立即進入點的 Active Directory 站台相關聯，且已設定的進入點相關聯的 IPv6 首碼。 喜好設定為用戶端電腦使用這些 IPv6 前置詞，因為它們會設定動態 IPv6 首碼原則資料表中具有較高優先順序的進入點連線時連線到資源。  
  
如果您的組織使用 Active Directory 拓樸與站台特定的 IPv6 首碼 （例如內部資源的 FQDN app.corp.com 裝載於北美洲與歐洲與站台特定的 IP 位址，每個位置中） 這是未設定預設會使用站台特定的 IPv6 首碼與遠端存取主控台中，未設定每個進入點。 如果您想要啟用此選擇性的案例，您需要使用特定應該連接到特定的進入點的用戶端電腦所慣用的 IPv6 前置詞設定每個進入點。 執行這項操作，如下所示：  
  
1.  Windows 10 或 Windows 8 用戶端電腦所使用的每個 gpo，執行 設定 DAEntryPointTableItem PowerShell cmdlet  
  
2.  設定 EntryPointRange 為 cmdlet 的參數使用的站台特定的 IPv6 首碼。 例如，若要新增站台特定前置詞 2001:db8:1:1:: / 64 和 2001:db:1:2:: / 64 可以用來呼叫歐洲的進入點執行下列命令  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  時修改 EntryPointRange 參數，請確定您沒有移除現有的 128 位元首碼屬於 IPsec 通道端點與 DNS64 位址。  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 在多站台的遠端存取部署時規劃轉換為 IPv6  
許多組織會在公司網路使用 IPv4 通訊協定。 可用的 IPv4 前置詞的耗盡，許多組織會讓從純 IPv4 轉換到僅支援 IPv6 的網路。  
  
這項轉換是最有可能會發生在兩個階段：  
  
1.  從僅 IPv4 的 IPv6 + IPv4 的公司網路。  
  
2.  從 IPv6 + IPv4 到 IPv6 專用的公司網路。  
  
在每個部分中，您可能會轉換執行階段中。 每個階段中只有一個子網路的網路可能會變更至新的網路組態。 因此，DirectAccess 多站台部署，才能支援混合式部署，比方說，某些項目點屬於僅支援 IPv4 的子網路和其他屬於 IPv6 + IPv4 子網路。 此外，在轉換程序中的組態變更必須不會中斷透過 DirectAccess 的用戶端連接性。  
  
### <a name="TransitionIPv4toMixed"></a>從僅 IPv4 轉換至 IPv6 + IPv4 公司網路  
將 IPv6 位址加入至僅 IPv4 的公司網路中，您可以將 IPv6 位址新增至已部署的 DirectAccess 伺服器。 此外，您可以將進入點或節點新增至負載平衡叢集中使用 IPv4 和 IPv6 位址的 DirectAccess 部署。  
  
遠端存取可讓您將 IPv4 和 IPv6 位址的伺服器新增到原本以僅 IPv4 位址設定的部署。 這些伺服器會新增為僅 IPv4 的伺服器和 DirectAccess; 會忽略其 IPv6 位址因此，您的組織無法利用原生 IPv6 連線能力的優點在這些新的伺服器上。  
  
若要轉換至 IPv6 + IPv4 部署部署，並利用原生的 IPv6 功能，您必須重新安裝 DirectAccess。 維護整個重新安裝用戶端連接性會看到轉換，從僅 IPv4 到 IPv6 專用部署使用雙重 DirectAccess 部署。  
  
> [!NOTE]  
> 在與僅支援 IPv4 的網路，在混合 IPv4 + IPv6 網路中，用來解析的 DNS 用戶端要求的 DNS 伺服器位址必須設定與 DNS64 部署於遠端存取伺服器本身，而非與公司的 DNS。  
  
### <a name="TransitionMixedtoIPv6"></a>從 IPv6 + IPv4 轉換到 IPv6 專用的公司網路  
DirectAccess 可讓您新增 IPv6 專用的進入點，只有當第一部遠端存取伺服器部署中的原本是這兩個 IPv4 和 IPv6 位址或僅 IPv6 位址。 也就是您無法從轉換的僅 IPv4 的網路到在單一步驟中僅支援 IPv6 的網路而不需要重新安裝 DirectAccess。 若要直接從僅 IPv4 的網路轉換到 IPv6 專用網路，請參閱 < 轉換從僅 IPv4 到 IPv6 專用部署使用雙重 DirectAccess 部署。  
  
您已完成從僅 IPv4 部署轉換至 IPv6 + IPv4 部署之後，您可以轉換到 IPv6 專用網路。 期間和轉換之後，請注意下列各項：  
  
-   如果任何僅支援 IPv4 的後端伺服器保持公司網路中，將無法透過僅支援 IPv6 的進入點連線的用戶端連線到它們。  
  
-   將僅支援 IPv6 的進入點新增至 IPv4 + IPv6 部署中，當 DNS64 和 NAT64 不會啟用新的伺服器上。 連接到這些進入點的用戶端會自動設定成使用公司的 DNS 伺服器。  
  
-   如果您要刪除已部署的伺服器上的 IPv4 位址，您必須從 DirectAccess 部署中移除伺服器、 移除其 IPv4 公司網路位址，並將它重新加入部署。  
  
若要支援用戶端連線到公司網路，您必須確定其 IPv6 位址的公司 DNS 可以解決網路位置伺服器。 其他的 IPv4 位址，可設定，但是不需要。  
  
### <a name="DualDeployment"></a>從僅 IPv4 轉換至僅支援 IPv6 的部署使用雙重 DirectAccess 部署  
無法完成從僅 IPv4 轉換到 IPv6 專用的公司網路，而不必重新安裝 DirectAccess 部署。 若要在轉換期間維護用戶端連接性，您可以使用另一個 DirectAccess 部署。 當第一個階段中轉換完成 （升級至 IPv4 + IPv6 僅支援 IPv4 的網路），而且您想要準備用於原生的 IPv6 連線優點未來轉換成僅支援 IPv6 的公司網路資料利用需要雙重的部署。 雙重部署所述的下列一般步驟：  
  
1.  安裝第二個的 DirectAccess 部署。 您可以安裝新的伺服器上的 DirectAccess 或從第一次的部署移除伺服器，和用於第二個部署。  
  
    > [!NOTE]  
    > 安裝時一起目前其他的 DirectAccess 部署，請確定沒有兩個進入點，共用相同的用戶端前置詞。  
    >   
    > 如果您安裝 DirectAccess 使用開始使用精靈或 cmdlet `Install-RemoteAccess`，預設值是部署在遠端存取會自動設定的第一個進入點的用戶端前置詞 < IPv6 子網路\_前置詞 >: 1000年:: /64。 如有需要您必須變更前置詞。  
  
2.  從第一次的部署移除所選的用戶端的安全性群組。  
  
3.  加入第二個部署中的用戶端的安全性群組。  
  
    > [!IMPORTANT]  
    > 為了維護整個程序的用戶端連接性，您必須將安全性群組加入第二個的部署移除第一個之後，立即。 這可確保用戶端使用兩個或零個 DirectAccess Gpo 將不會更新。 用戶端會開始使用第二個部署，一旦他們擷取並更新其用戶端 GPO。  
  
4.  選擇性：從第一次的部署移除 DirectAccess 進入點，並將這些伺服器新增為新進入點，在第二個。  
  
當您完成轉換之後時，您可以解除安裝第一次的 DirectAccess 部署。 解除安裝時，可能會發生下列問題：  
  
-   如果部署已設定為支援行動電腦上的只有用戶端，將會刪除 WMI 篩選器。 如果第二個部署的用戶端安全性群組包含桌面的電腦，DirectAccess 用戶端 GPO 將不會篩選桌面的電腦，並在其上可能會造成問題。 如果所需的攜帶型電腦篩選器 加以重新建立遵循上的指示[建立 gpo 的 WMI 篩選器](https://technet.microsoft.com/library/cc947846.aspx)。  
  
-   如果這兩個部署原先建立在相同的 Active Directory 網域中，DNS 探查 localhost 指向都會被刪除，而且可能會造成用戶端連線問題的項目。 比方說，用戶端可能使用 IP-HTTPS，而不是 Teredo 進行連線，或切換 DirectAccess 多站台進入點。 在此情況下，您必須在公司的 DNS 中新增下列的 DNS 項目：  
  
    -   區域： 網域名稱  
  
    -   名稱： directaccess corpConnectivityHost  
  
    -   IP 位址::: 1  
  
    -   輸入：AAAA  
  
  
  


