---
title: 步驟 1 中設定遠端存取基礎結構
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
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 95fc9cbef454c8f36b1921eb7f570138bf124256
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446938"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>步驟 1 中設定遠端存取基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 以及「路由及遠端存取服務」(RRAS) 合併成一個遠端存取角色。  
  
本主題描述如何設定基礎結構所需的進階 「 遠端存取部署在混合 IPv4 和 IPv6 的環境中使用單一的遠端存取伺服器。 開始部署步驟之前，請確定您已完成中所述的規劃步驟[步驟 1:規劃遠端存取基礎結構](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md)。  
  
|工作|描述|  
|----|--------|  
|設定伺服器網路設定|在遠端存取伺服器上設定伺服器網路設定。|  
|設定公司網路中的路由|設定公司網路中的路由，確保適當地路由流量。|  
|設定防火牆|如有必要，設定其他防火牆。|  
|設定 CA 和憑證|設定憑證授權單位 (CA)，如有需要，以及在部署中所需的任何其他憑證範本。|  
|設定 DNS 伺服器|設定遠端存取伺服器的 DNS 設定。|  
|設定 Active Directory|加入 Active Directory 網域的用戶端電腦和遠端存取伺服器。|  
|設定 GPO|如有必要，請針對部署中，設定群組原則物件 (Gpo)。|  
|設定安全性群組|設定將包含 DirectAccess 用戶端電腦的安全性群組，以及部署中所需的任何其他安全性群組。|  
|設定網路位置伺服器|設定網路位置伺服器，包括安裝網路位置伺服器網站憑證。|  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigNetworkSettings"></a>設定伺服器網路設定  
取決於您決定要放置遠端存取伺服器，在邊緣或在網路位址轉譯 (NAT) 裝置後面，下列的網路介面位址設定需要有 IPv4 和 IPv6 的環境中的單一伺服器部署。 您可以使用 [Windows 網路和共用中心]  中的 [變更介面卡設定]  設定所有 IP 位址。  
  
**邊緣拓撲**:  
  
需要下列項目：  
  
-   兩個網際網路對向連續公用靜態 IPv4 或 IPv6 位址。  
  
    > [!NOTE]  
    > Teredo 需要兩個連續的公用 IPv4 位址。 如果您不是使用 Teredo，則可以設定單一公用靜態 IPv4 位址。  
  
-   單一內部靜態 IPv4 或 IPv6 位址。  
  
**（兩個網路介面卡） 的 NAT 裝置後面**:  
  
需要有單一內部網路對向靜態 IPv4 或 IPv6 位址。  
  
**（一張網路介面卡） 的 NAT 裝置後面**:  
  
需要單一靜態 IPv4 或 IPv6 位址。  
  
如果遠端存取伺服器具有兩張網路介面卡 （一個用於網域設定檔，另一個則用於公用或私人設定檔），但您會使用單一網路介面卡拓撲，建議是，如下所示：  
  
1.  請確定第二個網路介面卡也會被歸類在網域設定檔。  
  
2.  如果第二個網路介面卡不能設定為網域設定檔，因為任何原因，將 DirectAccess IPsec 原則必須是以手動方式擴大到所有設定檔使用下列 Windows PowerShell 命令：  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    若要在這個命令中使用的 IPsec 原則的名稱是**Directaccess-daservertoinfra**並**Directaccess-daservertocorp**。  
  
## <a name="BKMK_ConfigRouting"></a>設定公司網路中的路由  
設定公司網路中的路由，如下所示：  
  
-   在組織中部署原生 IPv6 時，請新增路由，讓內部網路的路由器透過遠端存取伺服器將 IPv6 流量路由回來。  
  
-   在遠端存取伺服器上手動設定組織 IPv4 和 IPv6 的路由。 新增已發佈的路由，讓所有流量使用 （/ 48） IPv6 首碼會轉送至內部網路。 此外，如果是 IPv4 流量，則新增明確的路由，讓 IPv4 流量轉送到內部網路。  
  
## <a name="BKMK_ConfigFirewalls"></a>設定防火牆  
在您選擇取決於網路設定，當您在部署中使用額外的防火牆，適用於遠端存取 」 流量的下列防火牆例外：  
  
### <a name="remote-access-server-on-ipv4-internet"></a>IPv4 網際網路上的遠端存取伺服器  
當遠端存取伺服器位於 IPv4 網際網路上時，請套用下列網際網路對向防火牆例外，遠端存取流量：  
  
-   **Teredo 流量**  
  
    使用者資料包通訊協定 (UDP) 目的地連接埠 3544 輸入，及 UDP 來源連接埠 3544 輸出。 遠端存取伺服器上套用此豁免，這兩個網際網路對向連續公用 IPv4 位址。  
  
-   **6to4 流量**  
  
    IP 通訊協定 41 輸入和輸出。 遠端存取伺服器上套用此豁免，這兩個網際網路對向連續公用 IPv4 位址。  
  
-   **若為 IP-HTTPS 流量**  
  
    傳輸控制通訊協定 (TCP) 目的地連接埠 443 和 TCP 來源連接埠 443 輸出。 當遠端存取伺服器具有單一網路介面卡，且網路位置伺服器位於遠端存取伺服器上時，則還需要 TCP 連接埠 62000。 適用於這些豁免只針對至伺服器的外部名稱解析的位址。  
  
    > [!NOTE]  
    > 這個豁免是在伺服器上設定遠端存取。 所有其他豁免則會在邊緣防火牆上設定。  
  
### <a name="remote-access-server-on-ipv6-internet"></a>IPv6 網際網路上的遠端存取伺服器  
當遠端存取伺服器位於 IPv6 網際網路時，請套用下列網際網路對向防火牆例外，遠端存取流量：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
-   適用於 IPv6 的網際網路控制訊息通訊協定 (ICMPv6) 流量輸入和輸出-適用於只有 Teredo 部署。  
  
### <a name="remote-access-traffic"></a>遠端存取 」 流量  
套用下列內部網路防火牆例外，遠端存取流量：  
  
-   ISATAP:通訊協定 41 輸入和輸出  
  
-   適用於所有的 IPv4 或 IPv6 流量的 TCP/UDP  
  
-   適用於所有的 IPv4 或 IPv6 流量的 ICMP  
  
## <a name="BKMK_ConfigCAs"></a>設定 Ca 和憑證  
在 Windows Server 2012，您可以選擇使用憑證進行電腦驗證，或使用內建的 Kerberos 驗證使用使用者名稱和密碼的遠端存取。 您也必須在遠端存取伺服器上設定 IP-HTTPS 憑證。 本節說明如何設定這些憑證。  
  
如需公開金鑰基礎結構 (PKI) 設定的詳細資訊，請參閱[Active Directory 憑證服務](https://technet.microsoft.com/library/cc770357.aspx)。  
  
### <a name="BKMK_ConfigIPsec"></a>設定 IPsec 驗證  
遠端存取伺服器和所有 DirectAccess 用戶端上需要憑證，使其可以使用 IPsec 驗證。 憑證必須由內部憑證授權單位 (CA) 發行。 遠端存取伺服器和 DirectAccess 用戶端必須信任簽發根憑證和中繼憑證的 CA。  
  
##### <a name="to-configure-ipsec-authentication"></a>設定 IPsec 驗證  
  
1.  在內部 CA 上，決定如果您將使用預設的電腦憑證範本，或如果您將建立新的憑證範本中所述[建立憑證範本](https://technet.microsoft.com/library/cc731705.aspx)。  
  
    > [!NOTE]  
    > 如果您建立新的範本，它必須設定用戶端驗證。  
  
2.  如有必要，請部署憑證範本。 如需詳細資訊，請參閱[部署憑證範本](https://technet.microsoft.com/library/cc770794.aspx)。  
  
3.  如有必要，請設定自動註冊的範本。  
  
4.  如有必要，請設定憑證自動註冊。 如需詳細資訊，請參閱[設定憑證自動註冊](https://technet.microsoft.com/library/cc731522.aspx)。  
  
### <a name="BKMK_ConfigCertTemp"></a>設定憑證範本  
當您使用內部 CA 來簽發憑證時，您必須設定 IP-HTTPS 憑證和網路位置伺服器網站憑證的憑證範本。  
  
##### <a name="to-configure-a-certificate-template"></a>設定憑證範本  
  
1.  在內部 CA 上，使用 [建立憑證範本](https://technet.microsoft.com/library/cc731705.aspx)中所述的方式建立憑證範本。  
  
2.  使用 [部署憑證範本](https://technet.microsoft.com/library/cc770794.aspx)中所述的方式部署憑證範本。  
  
準備您的範本之後，您可以使用它們來設定憑證。 下列程序，如需詳細資訊，請參閱：  
  
-   [設定 IP-HTTPS 憑證](#BKMK_IPHTTPS)  
  
-   [設定網路位置伺服器](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>設定 IP-HTTPS 憑證  
遠端存取需要 IP-HTTPS 憑證來驗證遠端存取伺服器的 IP-HTTPS 連線。 有三種 IP-HTTPS 憑證的憑證選項：  
  
-   **Public**  
  
    由協力廠商所提供。  
  
-   **私用**  
  
    這個憑證根據您在中建立的憑證範本[設定憑證範本](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)。 它需要，可從可公開解析的 FQDN 存取憑證撤銷清單 (CRL) 發佈點。  
  
-   **自我簽署**  
  
    此憑證需要可從可公開解析的 FQDN 存取的 CRL 發佈點。  
  
    > [!NOTE]  
    > 自我簽署憑證無法在多站台部署中使用。  
  
請確定用於 IP-HTTPS 驗證的網站憑證符合下列需求：  
  
-   憑證主體名稱應該是外部可解析的完整的網域名稱 (FQDN) 僅用於遠端存取伺服器 IP-HTTPS 連線的 IP-HTTPS url （ConnectTo 位址）。  
  
-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。  
  
-   在 [主旨] 欄位中，指定遠端存取伺服器的外部介面卡或 IP-HTTPS URL 的 FQDN 的 IPv4 位址。  
  
-   針對**增強金鑰使用方法**欄位中，使用伺服器驗證物件識別碼 (OID)。  
  
-   在 [CRL 發佈點]  欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須直接匯入到個人存放區。  
  
-   IP-HTTPS 憑證的名稱中可以包含萬用字元。  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>從內部 CA 安裝 IP-HTTPS 憑證  
  
1.  在遠端存取伺服器上：在 **開始**畫面上，輸入**mmc.exe**，然後按 ENTER 鍵。  
  
2.  在 MMC 主控台中，按一下 [檔案]  功能表上的 [新增/移除嵌入式管理單元]  。  
  
3.  在 [新增或移除嵌入式管理單元]  對話方塊中，依序按一下 [憑證]  、[新增]  、[電腦帳戶]  、[下一步]  、[本機電腦]  、[完成]  ，然後按一下 [確定]  。  
  
4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]  。  
  
5.  以滑鼠右鍵按一下**憑證**，指向**所有工作**，按一下 [**要求新憑證**，然後按一下**下一步]** 兩次...  
  
6.  在 **要求憑證**頁面上，選取您設定憑證範本中所建立的憑證範本的核取方塊，然後視需要按一下**需要更多資訊才能註冊此憑證**。  
  
7.  在 [憑證內容]  對話方塊的 [主體]  索引標籤上，於 [主體名稱]  區域的 [類型]  中，選取 [一般名稱]  。  
  
8.  在 **值**，指定外部配接器的遠端存取伺服器或 IP-HTTPS URL 的 FQDN 的 IPv4 位址，然後按一下**新增**。  
  
9. 在 [別名]  區域的 [類型]  中，選取 [DNS]  。  
  
10. 在 **值**，指定外部配接器的遠端存取伺服器或 IP-HTTPS URL 的 FQDN 的 IPv4 位址，然後按一下**新增**。  
  
11. 在 [一般]  索引標籤的 [易記名稱]  中，您可以輸入可協助您識別憑證的名稱。  
  
12. 在 [擴充功能]  索引標籤的 [擴充金鑰使用方法]  旁邊，按一下箭號，確認伺服器驗證在 [選取的選項]  清單中。  
  
13. 依序按一下 [確定]  、[註冊]  ，然後按一下 [完成]  。  
  
14. 在 [憑證] 嵌入式管理單元的詳細資料窗格，確認新的憑證已向伺服器驗證的用途。  
  
## <a name="BKMK_ConfigDNS"></a>設定 DNS 伺服器  
您必須為部署中內部網路的網路位置伺服器網站手動設定 DNS 項目。  
  
### <a name="NLS_DNS"></a>若要新增網路位置伺服器與 web 探查  
  
1.  在內部網路 DNS 伺服器上：在 **開始**畫面上，輸入**dnsmgmt.msc**，然後按 ENTER 鍵。  
  
2.  在 [DNS 管理員]  主控台的左窗格中，展開您網域的正向對應區域。 以滑鼠右鍵按一下網域，然後按一下 **新的主機 （A 或 AAAA）** 。  
  
3.  在 **新主機**對話方塊的 **名稱 （使用父系網域名稱如果空白）** 方塊中，輸入網路位置伺服器網站的 DNS 名稱 (這是 DirectAccess 用戶端用來連接到的名稱網路位置伺服器 」）。 在  **IP 位址**，輸入網路位置伺服器的 IPv4 位址，然後按一下**新增主機**，然後按一下**確定**。  
  
4.  在 **新主機**對話方塊中，於**名稱 （使用父系網域名稱如果空白）** 方塊中，輸入 web 探查 （預設 web 探查的名稱是 directaccess-webprobehost） 的 DNS 名稱。 在 [IP 位址]  方塊中，輸入 Web 探查的 IPv4 位址，然後按一下 [新增主機]  。  
  
5.  為 directaccess-corpconnectivityhost 和任何手動建立的連線能力檢查器重複此程序。 在 [ **DNS** ] 對話方塊中，按一下**確定**。  
  
6.  按一下 [完成]  。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
您也必須設定下列各項的 DNS 項目：  
  
-   **IP-HTTPS 伺服器**  
  
    DirectAccess 用戶端必須能夠解析網際網路的遠端存取伺服器的 DNS 名稱。  
  
-   **CRL 撤銷檢查**  
  
    DirectAccess 會使用憑證撤銷檢查，IP-HTTPS 連線，DirectAccess 用戶端與遠端存取伺服器之間，以及 DirectAccess 用戶端與網路位置伺服器之間的 HTTPS 型連線。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。  
  
-   **ISATAP**  
  
    站台內自動通道定址通訊協定 (ISATAP) 會使用通道讓 DirectAccess 用戶端透過 IPv4 網際網路時，封裝 IPv6 封包在 IPv4 標頭內連線到遠端存取伺服器。 「遠端存取」會使用它，透過內部網路提供對 ISATAP 主機的 IPv6 連線能力。 在非原生 IPv6 網路環境中，遠端存取伺服器本身會自動設定為 ISATAP 路由器。 需要 ISATAP 名稱的解析支援。  
  
## <a name="BKMK_ConfigAD"></a>設定 Active Directory  
遠端存取伺服器和所有 DirectAccess 用戶端電腦都必須加入 Active Directory 網域。 DirectAccess 用戶端電腦必須是下列其中一種網域類型的成員：  
  
-   與遠端存取伺服器屬於相同樹系的網域。  
  
-   屬於與遠端存取伺服器樹系具有雙向信任關係之樹系的網域。  
  
-   與遠端存取伺服器網域具有雙向網域信任關係的的網域。  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>將遠端存取伺服器加入網域  
  
1.  在 [伺服器管理員] 中，按一下 [本機伺服器]  。 在詳細資料窗格中，按一下 [電腦名稱]  旁邊的連結。  
  
2.  在 [系統內容]  對話方塊中，按一下 [電腦名稱]  索引標籤，然後按一下 [變更]  。  
  
3.  在 **電腦名稱**方塊中，輸入電腦名稱，如果您也要加入網域的伺服器時變更電腦名稱。 底下**隸屬**，按一下**網域**，然後輸入您想要加入的伺服器 (例如 corp.contoso.com)，然後按一下 網域名稱**確定**。  
  
4.  當系統提示您輸入使用者名稱和密碼時，請輸入使用者名稱和密碼來將電腦加入網域，然後按一下 有權限的使用者**確定**。  
  
5.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]  。  
  
6.  當提示您必須重新啟動電腦時，按一下 [確定]  。  
  
7.  按一下 [系統內容]  對話方塊中的 [關閉]  。  
  
8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]  。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>將用戶端電腦加入網域  
  
1.  在 **開始**畫面上，輸入**explorer.exe**，然後按 ENTER 鍵。  
  
2.  在 [電腦] 圖示上按一下滑鼠右鍵，然後按一下 [內容]  。  
  
3.  在 [系統]  頁面上，按一下 [進階系統設定]  。  
  
4.  在 [系統內容]  對話方塊中的 [電腦名稱]  索引標籤上，按一下 [變更]  。  
  
5.  在 **電腦名稱**方塊中，輸入電腦名稱，如果您也要加入網域的伺服器時變更電腦名稱。 在 [隸屬於]  底下，按一下 [網域]  ，然後輸入伺服器要加入的網域名稱 (例如 corp.contoso.com)，然後按一下 [確定]  。  
  
6.  當系統提示您輸入使用者名稱和密碼時，請輸入使用者名稱和密碼來將電腦加入網域，然後按一下 有權限的使用者**確定**。  
  
7.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]  。  
  
8.  當提示您必須重新啟動電腦時，按一下 [確定]  。  
  
9. 在 **系統屬性** 對話方塊中，按一下 關閉。  
  
10. 出現提示時，按一下 [立即重新啟動]  。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
> [!NOTE]  
> 輸入下列命令後，您必須提供網域認證。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>設定 Gpo  
若要部署遠端存取，您需要至少兩個群組原則物件。 一個群組原則物件包含設定的遠端存取伺服器，以及一個包含 DirectAccess 用戶端電腦的設定。 當您設定遠端存取時，精靈會自動建立必要的群組原則物件。 不過，如果您的組織強制使用命名慣例，或您沒有建立或編輯群組原則物件的必要權限，則必須建立在之前設定的遠端存取。  
  
若要建立群組原則物件，請參閱[建立和編輯群組原則物件](https://technet.microsoft.com/library/cc754740.aspx)。  
  
系統管理員手動將 DirectAccess 群組原則物件連結到組織單位 (OU)。 請考慮下列事項：  
  
1.  設定 DirectAccess 之前，請建立的 Gpo 連結到個別的 Ou。  
  
2.  設定 DirectAccess 時，請指定用戶端電腦的安全性群組。  
  
3.  Gpo 會自動設定，不論的系統管理員必須將 Gpo 連結到網域的權限。  
  
4.  如果 Gpo 已經連結到 OU，該連結將不會被移除，但未連結到網域。  
  
5.  伺服器 Gpo，OU 必須包含伺服器電腦物件否則 GPO 會連結到網域的根目錄。  
  
6.  如果執行 DirectAccess 安裝精靈中，設定完成之後，沒有先前連結的 OU，系統管理員可以將 DirectAccess Gpo 連結到所需的 Ou，並移除網域的連結。  
  
    如需詳細資訊，請參閱[連結群組原則物件](https://technet.microsoft.com/library/cc732979.aspx)。  
  
> [!NOTE]  
> 如果手動建立群組原則物件，就可以在群組原則物件將不會提供在 DirectAccess 設定期間。 群組原則物件可能已複寫到最接近管理電腦的網域控制站。 系統管理員可以等候複寫完成，或強制執行複寫。  
  
## <a name="BKMK_ConfigSGs"></a>設定安全性群組  
用戶端電腦群組原則物件中所包含的 DirectAccess 設定只用於設定遠端存取時，您所指定之安全性群組的成員電腦。  
  
### <a name="Sec_Group"></a>為 DirectAccess 用戶端建立安全性群組  
  
1.  在 **開始**畫面上，輸入**dsa.msc**，然後按 ENTER 鍵。  
  
2.  在 [Active Directory 使用者和電腦]  主控台的左窗格中，展開將包含安全性群組的網域，在 [使用者]  上按一下滑鼠右鍵，指向 [新增]  ，然後按一下 [群組]  。  
  
3.  在 [新增物件 - 群組]  對話方塊的 [群組名稱]  底下，輸入安全性群組的名稱。  
  
4.  在 [群組領域]  底下，按一下 [全域]  ，在 [群組類型]  底下，按一下 [安全性]  ，然後按一下 [確定]  。  
  
5.  按兩下 DirectAccess 用戶端電腦安全性群組，然後在**屬性** 對話方塊中，按一下**成員** 索引標籤。  
  
6.  在 [成員]  索引標籤上，按一下 [新增]  。  
  
7.  在 [選取使用者、連絡人、電腦或服務帳戶]  對話方塊方塊中，選取您要啟用 DirectAccess 的用戶端電腦，然後按一下 [確定]  。  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**Windows PowerShell 對等的命令**  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>設定網路位置伺服器  
網路位置伺服器應該具有高可用性的伺服器上，而且需要有效的安全通訊端層 (SSL) 憑證，DirectAccess 用戶端信任。  
  
> [!NOTE]  
> 如果網路位置伺服器網站位於遠端存取伺服器上，網站將會自動建立時設定遠端存取與它繫結至您所提供的伺服器憑證。  
  
有兩種網路位置伺服器憑證的憑證選項：  
  
-   **私用**  
  
    > [!NOTE]  
    > 這個憑證根據您在中建立的憑證範本[設定憑證範本](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)。  
  
-   **自我簽署**  
  
    > [!NOTE]  
    > 自我簽署憑證無法在多站台部署中使用。  
  
不論您是使用私人憑證或自我簽署的憑證，它們需要下列項目：  
  
-   用於網路位置伺服器的網站憑證。 憑證主體必須是網路位置伺服器的 URL。  
  
-   內部網路上具有高可用性的 CRL 發佈點。  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>從內部 CA 安裝網路位置伺服器憑證  
  
1.  在將裝載網路位置伺服器網站的伺服器：在 **開始**畫面上，輸入**mmc.exe**，然後按 ENTER 鍵。  
  
2.  在 MMC 主控台中，按一下 [檔案]  功能表上的 [新增/移除嵌入式管理單元]  。  
  
3.  在 [新增或移除嵌入式管理單元]  對話方塊中，依序按一下 [憑證]  、[新增]  、[電腦帳戶]  、[下一步]  、[本機電腦]  、[完成]  ，然後按一下 [確定]  。  
  
4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]  。  
  
5.  以滑鼠右鍵按一下**憑證**，指向**所有工作**，按一下 [**要求新憑證**，然後按一下**下一步]** 兩次。  
  
6.  在 **要求憑證**頁面上，選取您設定憑證範本中所建立的憑證範本的核取方塊，然後視需要按一下**需要更多資訊才能註冊此憑證**。  
  
7.  在 [憑證內容]  對話方塊的 [主體]  索引標籤上，於 [主體名稱]  區域的 [類型]  中，選取 [一般名稱]  。  
  
8.  在 [值]  中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]  。  
  
9. 在 [別名]  區域的 [類型]  中，選取 [DNS]  。  
  
10. 在 [值]  中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]  。  
  
11. 在 [一般]  索引標籤的 [易記名稱]  中，您可以輸入可協助您識別憑證的名稱。  
  
12. 依序按一下 [確定]  、[註冊]  ，然後按一下 [完成]  。  
  
13. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，請確認該新的憑證已註冊的伺服器驗證用途使用。  
  
#### <a name="to-configure-the-network-location-server"></a>設定網路位置伺服器  
  
1.  在高可用性伺服器上設定網站。 這個網站不需要任何內容，但是進行測試時，您可以定義一個在用戶端連線時提供訊息的預設網頁。  
  
    如果網路位置伺服器網站裝載於遠端存取伺服器，不需要此步驟。  
  
2.  將 HTTPS 伺服器憑證繫結到網站。 憑證的一般名稱應該與網路位置伺服器站台的名稱相符。 確定 DirectAccess 用戶端信任簽發 CA。  
  
    如果網路位置伺服器網站裝載於遠端存取伺服器，不需要此步驟。  
  
3.  設定的 CRL 站台具有高可用性的內部網路上。  
  
    存取 CRL 發佈點時可以透過：  
  
    -   使用網頁伺服器的 HTTP 為基礎的 URL，例如： https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   檔案伺服器，並可透過通用的命名慣例 (UNC) 路徑，例如\\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    如果只有透過 IPv6 才能連線到內部 CRL 發佈點，您必須使用進階安全性連線安全性規則來設定 Windows 防火牆。 這會讓豁免 IPsec 的保護您的內部網路的 IPv6 位址空間從 IPv6 位址到 CRL 發佈點。  
  
4.  請確定內部網路上的 DirectAccess 用戶端可以解析網路位置伺服器的名稱，並在網際網路上的 DirectAccess 用戶端無法解析名稱。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [步驟 2：設定遠端存取伺服器](Step-2-Configure-the-Remote-Access-Server.md)

