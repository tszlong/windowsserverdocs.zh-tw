---
title: 步驟1設定遠端存取基礎結構
description: 本主題是《在 Windows Server 2016 中遠端系統管理 DirectAccess 用戶端》指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: df58a68da0eedebe0b21fd1b0a4651f342c12434
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947714"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>步驟1設定遠端存取基礎結構

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。

本主題說明如何使用混合的 IPv4 和 IPv6 環境中的單一遠端存取服務器，設定 advanced Remote Access 部署所需的基礎結構。 開始部署步驟之前，請確定您已完成 [步驟1：規劃遠端存取基礎結構](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md)中所述的規劃步驟。

|Task|描述|
|----|--------|
|設定伺服器網路設定|在遠端存取伺服器上設定伺服器網路設定。|
|設定公司網路中的路由|設定公司網路中的路由，確保適當地路由流量。|
|設定防火牆|如有必要，設定其他防火牆。|
|設定 CA 和憑證|設定憑證授權單位單位 (CA) （如有必要），以及部署所需的任何其他憑證範本。|
|設定 DNS 伺服器|設定遠端存取伺服器的 DNS 設定。|
|設定 Active Directory|將用戶端電腦和遠端存取服務器加入 Active Directory 網域。|
|設定 GPO|如有需要，請為部署 (Gpo) 設定群組原則物件。|
|設定安全性群組|設定將包含 DirectAccess 用戶端電腦的安全性群組，以及部署中所需的任何其他安全性群組。|
|設定網路位置伺服器|設定網路位置伺服器，包括安裝網路位置伺服器網站憑證。|

> [!NOTE]
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="configure-server-network-settings"></a><a name="BKMK_ConfigNetworkSettings"></a>設定伺服器網路設定
根據您決定將遠端存取服務器放置在邊緣或 (NAT) 裝置的網路位址轉譯後方，在具有 IPv4 和 IPv6 的環境中，單一伺服器部署需要下列網路介面位址設定。 您可以使用 [Windows 網路和共用中心] 中的 [變更介面卡設定] 設定所有 IP 位址。

**Edge 拓撲**：

需要下列各項：

-   兩個網際網路面向的連續公用靜態 IPv4 或 IPv6 位址。

    > [!NOTE]
    > Teredo 需要兩個連續的公用 IPv4 位址。 如果您不是使用 Teredo，則可以設定單一公用靜態 IPv4 位址。

-   單一內部靜態 IPv4 或 IPv6 位址。

在 **NAT 裝置後方 (兩個網路介面卡)**：

需要單一內部網路面向的靜態 IPv4 或 IPv6 位址。

在 **NAT 裝置後方 (一個網路介面卡)**：

需要單一靜態 IPv4 或 IPv6 位址。

如果遠端存取服務器有兩張網路介面卡 (一個用於網域設定檔，另一個用於公用或私人設定檔) ，但您使用的是單一網路介面卡拓撲，則建議如下所示：

1.  確定第二張網路介面卡也分類在網域設定檔中。

2.  如果因為任何原因而無法為網域設定檔設定第二張網路介面卡，則必須使用下列 Windows PowerShell 命令，手動將 DirectAccess IPsec 原則的範圍設定為所有設定檔：

    ```
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any
    Save-NetGPO -GPOSession $gposession
    ```

    要在此命令中使用的 IPsec 原則名稱為 **directaccess-DaServerToInfra** 和 **directaccess-DaServerToCorp**。

## <a name="configure-routing-in-the-corporate-network"></a><a name="BKMK_ConfigRouting"></a>設定公司網路中的路由
設定公司網路中的路由，如下所示：

-   在組織中部署原生 IPv6 時，請新增路由，讓內部網路的路由器透過遠端存取伺服器將 IPv6 流量路由回來。

-   在遠端存取伺服器上手動設定組織 IPv4 和 IPv6 的路由。 新增已發佈的路由，以便將具有 (/48) IPv6 首碼的所有流量轉送到內部網路。 此外，如果是 IPv4 流量，則新增明確的路由，讓 IPv4 流量轉送到內部網路。

## <a name="configure-firewalls"></a><a name="BKMK_ConfigFirewalls"></a>設定防火牆
視您所選擇的網路設定而定，當您在部署中使用其他防火牆時，請針對遠端存取流量套用下列防火牆例外：

### <a name="remote-access-server-on-ipv4-internet"></a>IPv4 網際網路上的遠端存取服務器
當遠端存取服務器是在 IPv4 網際網路上時，請針對遠端存取流量套用下列網際網路對應的防火牆例外：

-   **Teredo 流量**

    使用者資料包協定 (UDP) 目的地埠3544輸入和 UDP 來源埠3544輸出。 針對遠端存取服務器上的兩個網際網路對應連續公用 IPv4 位址套用此豁免。

-   **6to4 流量**

    IP 通訊協定41輸入和輸出。 針對遠端存取服務器上的兩個網際網路對應連續公用 IPv4 位址套用此豁免。

-   **Ip-HTTPs 流量**

    傳輸控制通訊協定 (TCP) 目的地埠443和 TCP 來源埠443輸出。 當遠端存取伺服器具有單一網路介面卡，且網路位置伺服器位於遠端存取伺服器上時，則還需要 TCP 連接埠 62000。 請僅針對伺服器外部名稱解析的位址套用這些豁免。

    > [!NOTE]
    > 這項豁免是在遠端存取服務器上設定的。 所有其他豁免都是在邊緣防火牆上設定的。

### <a name="remote-access-server-on-ipv6-internet"></a>IPv6 網際網路上的遠端存取服務器
當遠端存取服務器是在 IPv6 網際網路上時，請針對遠端存取流量套用下列網際網路對應的防火牆例外：

-   IP 通訊協定 50

-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。

-   適用于 IPv6 的網際網路控制訊息通訊協定 (ICMPv6) 流量輸入和輸出-僅適用于 Teredo 的實施。

### <a name="remote-access-traffic"></a>遠端存取流量
將下列內部網路防火牆例外套用至遠端存取流量：

-   ISATAP： Protocol 41 輸入和輸出

-   適用于所有 IPv4 或 IPv6 流量的 TCP/UDP

-   所有 IPv4 或 IPv6 流量的 ICMP

## <a name="configure-cas-and-certificates"></a><a name="BKMK_ConfigCAs"></a>設定 CA 和憑證
利用 Windows Server 2012 中的遠端存取，您可以選擇使用憑證進行電腦驗證，或使用內建的 Kerberos 驗證來使用使用者名稱和密碼。 您也必須在遠端存取服務器上設定 IP-HTTPS 憑證。 本節說明如何設定這些憑證。

如需 (PKI) 設定公開金鑰基礎結構的詳細資訊，請參閱 [Active Directory 憑證服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10))。

### <a name="configure-ipsec-authentication"></a><a name="BKMK_ConfigIPsec"></a>設定 IPsec 驗證
遠端存取服務器和所有 DirectAccess 用戶端都需要憑證，才能使用 IPsec 驗證。 憑證必須由內部憑證授權單位單位發行 (CA) 。 遠端存取服務器和 DirectAccess 用戶端必須信任發出根和中繼憑證的 CA。

##### <a name="to-configure-ipsec-authentication"></a>設定 IPsec 驗證

1.  在內部 CA 上，決定您將使用預設的電腦憑證範本，還是要建立新的憑證範本，如 [建立憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10))中所述。

    > [!NOTE]
    > 如果您建立新的範本，則必須設定它以進行用戶端驗證。

2.  視需要部署憑證範本。 如需詳細資訊，請參閱[部署憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10))。

3.  視需要設定自動註冊的範本。

4.  視需要設定憑證自動註冊。 如需詳細資訊，請參閱[設定憑證自動註冊](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731522(v=ws.11))。

### <a name="configure-certificate-templates"></a><a name="BKMK_ConfigCertTemp"></a>設定憑證範本
當您使用內部 CA 來發行憑證時，您必須為 IP-HTTPS 憑證和網路位置伺服器網站憑證設定憑證範本。

##### <a name="to-configure-a-certificate-template"></a>設定憑證範本

1.  在內部 CA 上，使用 [建立憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10))中所述的方式建立憑證範本。

2.  使用 [部署憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10))中所述的方式部署憑證範本。

準備範本之後，您可以使用它們來設定憑證。 請參閱下列程式以取得詳細資料：

-   [設定 IP-HTTPS 憑證](#BKMK_IPHTTPS)

-   [設定網路位置伺服器](#BKMK_ConfigNLS)

### <a name="configure-the-ip-https-certificate"></a><a name="BKMK_IPHTTPS"></a>設定 IP-HTTPS 憑證
遠端存取需要 IP-HTTPS 憑證來驗證遠端存取伺服器的 IP-HTTPS 連線。 有三種 IP-HTTPS 憑證的憑證選項：

-   **公用**

    由協力廠商提供。

-   **私人**

    憑證是以您在設定 [憑證範本](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)中建立的憑證範本為基礎。 它需要憑證撤銷清單， (CRL) 可從可公開解析的 FQDN 存取的發佈點。

-   **自我簽署**

    此憑證需要可從可公開解析的 FQDN 存取的 CRL 發佈點。

    > [!NOTE]
    > 自我簽署憑證無法在多站台部署中使用。

請確定用於 IP-HTTPS 驗證的網站憑證符合下列需求：

-   憑證主體名稱應該是可從外部解析的完整功能變數名稱 (FQDN) 的 IP-HTTPS URL (ConnectTo 位址) ，此位址僅適用于遠端存取服務器的 IP-HTTPS 連接。

-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。

-   在 [主體] 欄位中，指定遠端存取服務器之對外介面卡的 IPv4 位址，或 IP-HTTPS URL 的 FQDN。

-   針對 [ **增強金鑰使用** 方法] 欄位，使用 (OID) 的伺服器驗證物件識別碼。

-   在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。

-   IP-HTTPS 憑證必須具有私密金鑰。

-   IP-HTTPS 憑證必須直接匯入到個人存放區。

-   IP-HTTPS 憑證的名稱中可以包含萬用字元。

##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>從內部 CA 安裝 IP-HTTPS 憑證

1.  在 [遠端存取服務器] 上：在 [ **開始** ] 畫面上，輸入 **mmc.exe**，然後按 enter。

2.  在 MMC 主控台的 [檔案] 功能表上，按一下 [新增/移除嵌入式管理單元]。

3.  在 [新增或移除嵌入式管理單元] 對話方塊中，依序按一下 [憑證]、[新增]、[電腦帳戶]、[下一步]、[本機電腦]、[完成]，然後按一下 [確定]。

4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]。

5.  以滑鼠右鍵按一下 [ **憑證**]，指向 [ **所有** 工作]，再按一下 [ **要求新憑證**]，然後按兩次 **[下一步]** 。

6.  在 [ **要求憑證** ] 頁面上，選取您在設定憑證範本中建立之憑證範本的核取方塊，如果需要，請按一下 [ **需要更多資訊才能註冊此憑證**]。

7.  在 [憑證內容] 對話方塊的 [主體] 索引標籤上，於 [主體名稱] 區域的 [類型] 中，選取 [一般名稱]。

8.  在 [ **值**] 中，指定遠端存取服務器之對外介面卡的 IPv4 位址或 ip-HTTPs URL 的 FQDN，然後按一下 [ **新增**]。

9. 在 [別名] 區域的 [類型] 中，選取 [DNS]。

10. 在 [ **值**] 中，指定遠端存取服務器之對外介面卡的 IPv4 位址或 ip-HTTPs URL 的 FQDN，然後按一下 [ **新增**]。

11. 在 [一般] 索引標籤的 [易記名稱] 中，您可以輸入可協助您識別憑證的名稱。

12. 在 [擴充功能] 索引標籤的 [擴充金鑰使用方法] 旁邊，按一下箭號，確認伺服器驗證在 [選取的選項] 清單中。

13. 依序按一下 [確定]、[註冊]，然後按一下 [完成]。

14. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已向伺服器驗證的預期用途註冊新的憑證。

## <a name="configure-the-dns-server"></a><a name="BKMK_ConfigDNS"></a>設定 DNS 伺服器
您必須為部署中內部網路的網路位置伺服器網站手動設定 DNS 項目。

### <a name="to-add-the-network-location-server-and-web-probe"></a><a name="NLS_DNS"></a>新增網路位置伺服器和 web 探查

1.  在內部網路 DNS 伺服器上：在 [ **開始** ] 畫面上，輸入 **dnsmgmt.msc**，然後按 enter。

2.  在 [DNS 管理員] 主控台的左窗格中，展開您網域的正向對應區域。 在網域上按一下滑鼠右鍵，然後按一下 [ **新增主機] (A 或 AAAA)**。

3.  在 [ **新增主機** ] 對話方塊的 [ **名稱 (使用父系網域名稱空白)** ] 方塊中，輸入網路位置伺服器網站的 DNS 名稱 (這是 DirectAccess 用戶端用來連線到網路位置伺服器) 的名稱。 在 [ **IP 位址** ] 方塊中，輸入網路位置伺服器的 IPv4 位址，然後按一下 [ **新增主機**]，再按一下 **[確定]**。

4.  在 [ **新增主機** ] 對話方塊的 [ **名稱 (使用父系網域名稱空白)** ] 方塊中，輸入 web 探查的 DNS 名稱 (預設 web 探查的名稱是 directaccess directaccess-webprobehost) 。 在 [IP 位址] 方塊中，輸入 Web 探查的 IPv4 位址，然後按一下 [新增主機]。

5.  為 directaccess-corpconnectivityhost 和任何手動建立的連線能力檢查器重複此程序。 在 [ **DNS** ] 對話方塊中，按一下 **[確定]**。

6.  按一下 [完成]。

![Windows PowerShell ](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif) * *_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>
```

您也必須設定下列各項的 DNS 項目：

-   _ *Ip-HTTPs 伺服器**

    DirectAccess 用戶端必須能夠從網際網路解析遠端存取服務器的 DNS 名稱。

-   **CRL 撤銷檢查**

    DirectAccess 會針對 DirectAccess 用戶端與遠端存取服務器之間的 ip-HTTPs 連線，以及 DirectAccess 用戶端與網路位置伺服器之間以 HTTPS 為基礎的連線，使用憑證撤銷檢查。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。

-   **ISATAP**

    站對站自動通道定址通訊協定 (ISATAP) 使用通道讓 DirectAccess 用戶端透過 IPv4 網際網路連線到遠端存取服務器，將 IPv6 封包封裝在 IPv4 標頭內。 「遠端存取」會使用它，透過內部網路提供對 ISATAP 主機的 IPv6 連線能力。 在非原生 IPv6 網路環境中，遠端存取服務器會自動將本身設定為 ISATAP 路由器。 需要 ISATAP 名稱的解析支援。

## <a name="configure-active-directory"></a><a name="BKMK_ConfigAD"></a>設定 Active Directory
遠端存取伺服器和所有 DirectAccess 用戶端電腦都必須加入 Active Directory 網域。 DirectAccess 用戶端電腦必須是下列其中一種網域類型的成員：

-   與遠端存取伺服器屬於相同樹系的網域。

-   屬於與遠端存取伺服器樹系具有雙向信任關係之樹系的網域。

-   與遠端存取伺服器網域具有雙向網域信任關係的的網域。

#### <a name="to-join-the-remote-access-server-to-a-domain"></a>將遠端存取伺服器加入網域

1.  在 [伺服器管理員] 中，按一下 [本機伺服器]。 在詳細資料窗格中，按一下 [電腦名稱] 旁邊的連結。

2.  在 [系統內容] 對話方塊中，按一下 [電腦名稱] 索引標籤，然後按一下 [變更]。

3.  如果您在將伺服器加入網域時也要變更電腦名稱稱，請在 [ **電腦名稱稱** ] 方塊中輸入電腦的名稱。 在 [ **成員隸屬**] 下，按一下 [ **網域**]，然後輸入您要加入伺服器的功能變數名稱， (例如 corp.contoso.com) ]，然後按一下 **[確定]**。

4.  當系統提示您輸入使用者名稱和密碼時，請輸入有權將電腦加入網域之使用者的使用者名稱和密碼，然後按一下 **[確定]**。

5.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]。

6.  當提示您必須重新啟動電腦時，按一下 [確定]。

7.  按一下 [系統內容] 對話方塊中的 [關閉]。

8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]。

#### <a name="to-join-client-computers-to-the-domain"></a>將用戶端電腦加入網域

1.  在 [ **開始** ] 畫面上，輸入 **explorer.exe**，然後按 enter。

2.  在 [電腦] 圖示上按一下滑鼠右鍵，然後按一下 [內容]。

3.  在 [系統] 頁面上，按一下 [進階系統設定]。

4.  在 [系統內容] 對話方塊中的 [電腦名稱] 索引標籤上，按一下 [變更]。

5.  如果您在將伺服器加入網域時也要變更電腦名稱稱，請在 [ **電腦名稱稱** ] 方塊中輸入電腦的名稱。 在 [隸屬於] 底下，按一下 [網域]，然後輸入伺服器要加入的網域名稱 (例如 corp.contoso.com)，然後按一下 [確定]。

6.  當系統提示您輸入使用者名稱和密碼時，請輸入有權將電腦加入網域之使用者的使用者名稱和密碼，然後按一下 **[確定]**。

7.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]。

8.  當提示您必須重新啟動電腦時，按一下 [確定]。

9. 在 [ **系統屬性** ] 對話方塊中，按一下 [關閉]。

10. 出現提示時，按一下 [立即重新啟動]。

![Windows PowerShell ](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif) * *_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

> [!NOTE]
> 輸入下列命令之後，您必須提供網域認證。

```
Add-Computer -DomainName <domain_name>
Restart-Computer
```

## <a name="configure-gpos"></a><a name="BKMK_ConfigGPOs"></a>設定 GPO
若要部署遠端存取，您需要至少兩個群組原則物件。 其中一個群組原則物件包含遠端存取服務器的設定，另一個則包含 DirectAccess 用戶端電腦的設定。 當您設定遠端存取時，嚮導會自動建立所需的群組原則物件。 但是，如果您的組織強制執行命名慣例，或您沒有建立或編輯群組原則物件的必要許可權，則必須在設定遠端存取之前先建立它們。

若要建立群組原則物件，請參閱 [建立和編輯群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11))。

系統管理員可以將 DirectAccess 群組原則物件手動連結到組織單位 (OU) 。 請考慮下列事項：

1.  設定 DirectAccess 之前，請先將建立的 Gpo 連結到個別的 Ou。

2.  設定 DirectAccess 時，請指定用戶端電腦的安全性群組。

3.  無論系統管理員是否有許可權可將 Gpo 連結至網域，都會自動設定 Gpo。

4.  如果 Gpo 已經連結到 OU，則不會移除連結，但不會連結到網域。

5.  若為伺服器 Gpo，則 OU 必須包含伺服器電腦物件，否則 GPO 將會連結到網域的根目錄。

6.  如果先前未連結到 OU，則在設定完成之後，系統管理員可以將 DirectAccess Gpo 連結到所需的 Ou，並移除該網域的連結。

    如需詳細資訊，請參閱[連結群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11))。

> [!NOTE]
> 如果群組原則物件是以手動方式建立，則在 DirectAccess 設定期間可能無法使用群組原則物件。 群組原則物件可能尚未複寫到最接近管理電腦的網域控制站。 系統管理員可以等候複寫完成或強制複寫。

## <a name="configure-security-groups"></a><a name="BKMK_ConfigSGs"></a>設定安全性群組
用戶端電腦群組原則物件中包含的 DirectAccess 設定只會套用到您在設定遠端存取時所指定之安全性群組成員的電腦。

### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>為 DirectAccess 用戶端建立安全性群組

1.  在 _ [*開始*] 畫面上輸入 **dsa.msc**，然後按 enter。

2.  在 [Active Directory 使用者和電腦] 主控台的左窗格中，展開將包含安全性群組的網域，在 [使用者] 上按一下滑鼠右鍵，指向 [新增]，然後按一下 [群組]。

3.  在 [新增物件 - 群組] 對話方塊的 [群組名稱] 底下，輸入安全性群組的名稱。

4.  在 [群組領域] 底下，按一下 [全域]，在 [群組類型] 底下，按一下 [安全性]，然後按一下 [確定]。

5.  按兩下 [DirectAccess 用戶端電腦] 安全性群組，然後在 [內容 **] 對話方塊中** ，按一下 [ **成員** ] 索引標籤。

6.  在 [成員] 索引標籤上，按一下 [新增]。

7.  在 [選取使用者、連絡人、電腦或服務帳戶] 對話方塊方塊中，選取您要啟用 DirectAccess 的用戶端電腦，然後按一下 [確定]。

![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**Windows PowerShell 對等命令**

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>
```

## <a name="configure-the-network-location-server"></a><a name="BKMK_ConfigNLS"></a>設定網路位置伺服器
網路位置伺服器應位於高可用性的伺服器上，而且需要有效的安全通訊端層 (DirectAccess 用戶端所信任的 SSL) 憑證。

> [!NOTE]
> 如果網路位置伺服器網站位於遠端存取服務器上，當您設定遠端存取時，系統會自動建立一個網站，並將其系結至您提供的伺服器憑證。

有兩種網路位置伺服器憑證的憑證選項：

-   **私人**

    > [!NOTE]
    > 憑證是以您在設定 [憑證範本](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp)中建立的憑證範本為基礎。

-   **自我簽署**

    > [!NOTE]
    > 自我簽署憑證無法在多站台部署中使用。

無論您使用的是私用憑證或自我簽署憑證，都需要下列各項：

-   用於網路位置伺服器的網站憑證。 憑證主體必須是網路位置伺服器的 URL。

-   在內部網路上具有高可用性的 CRL 發佈點。

#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>從內部 CA 安裝網路位置伺服器憑證

1.  在將裝載網路位置伺服器網站的伺服器上：在 [ **開始** ] 畫面上，輸入 **mmc.exe**，然後按 enter。

2.  在 MMC 主控台的 [檔案] 功能表上，按一下 [新增/移除嵌入式管理單元]。

3.  在 [新增或移除嵌入式管理單元] 對話方塊中，依序按一下 [憑證]、[新增]、[電腦帳戶]、[下一步]、[本機電腦]、[完成]，然後按一下 [確定]。

4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]。

5.  以滑鼠右鍵按一下 [ **憑證**]，指向 [ **所有** 工作]，按一下 [ **要求新憑證**]，然後按兩次 **[下一步]** 。

6.  在 [ **要求憑證** ] 頁面上，選取您在設定憑證範本中建立之憑證範本的核取方塊，如果需要，請按一下 [ **需要更多資訊才能註冊此憑證**]。

7.  在 [憑證內容] 對話方塊的 [主體] 索引標籤上，於 [主體名稱] 區域的 [類型] 中，選取 [一般名稱]。

8.  在 [值] 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]。

9. 在 [別名] 區域的 [類型] 中，選取 [DNS]。

10. 在 [值] 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]。

11. 在 [一般] 索引標籤的 [易記名稱] 中，您可以輸入可協助您識別憑證的名稱。

12. 依序按一下 [確定]、[註冊]，然後按一下 [完成]。

13. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已向伺服器驗證的預期用途註冊新的憑證。

#### <a name="to-configure-the-network-location-server"></a>設定網路位置伺服器

1.  在高可用性伺服器上設定網站。 這個網站不需要任何內容，但是進行測試時，您可以定義一個在用戶端連線時提供訊息的預設網頁。

    如果網路位置伺服器網站裝載于遠端存取服務器上，則不需要執行此步驟。

2.  將 HTTPS 伺服器憑證繫結到網站。 憑證的一般名稱應該與網路位置伺服器站台的名稱相符。 確定 DirectAccess 用戶端信任簽發 CA。

    如果網路位置伺服器網站裝載于遠端存取服務器上，則不需要執行此步驟。

3.  設定在內部網路上具有高可用性的 CRL 網站。

    存取 CRL 發佈點時可以透過：

    -   使用以 HTTP 為基礎的 URL 的 Web 服務器，例如： https://crl.corp.contoso.com/crld/corp-APP1-CA.crl

    -   透過通用命名慣例存取的檔案伺服器 (UNC) 路徑，例如 \\ \crl.corp.contoso.com\crld\corp-APP1-CA.crl

    如果內部 CRL 發佈點只能透過 IPv6 連接，您必須設定具有 Advanced Security 連線安全性規則的 Windows 防火牆。 這會豁免 IPsec 保護，從內部網路的 IPv6 位址空間到 CRL 發佈點的 IPv6 位址。

4.  確定內部網路上的 DirectAccess 用戶端可以解析網路位置伺服器的名稱，而且網際網路上的 DirectAccess 用戶端無法解析此名稱。

## <a name="see-also"></a><a name="BKMK_Links"></a>請參閱

-   [步驟 2：設定遠端存取伺服器](Step-2-Configure-the-Remote-Access-Server.md)
