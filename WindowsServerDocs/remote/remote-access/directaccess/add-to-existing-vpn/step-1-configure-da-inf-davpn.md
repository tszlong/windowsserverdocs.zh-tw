---
title: 步驟1設定 DirectAccess 基礎結構
description: 本主題是將 DirectAccess 新增至 Windows Server 2016 的現有遠端存取（VPN）部署指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4437101c6cde25ebb370fe54a2f8ef821997f15d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388766"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>步驟1設定 DirectAccess 基礎結構

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明如何在現有 VPN 部署中設定啟用 DirectAccess 所需的基礎結構。 開始部署步驟之前，請確定您已完成[步驟1：規劃 DirectAccess 基礎結構](Step-1-Plan-DirectAccess-Infrastructure.md)中所述的規劃步驟。  
  
|工作|描述|  
|----|--------|  
|設定伺服器網路設定|在遠端存取伺服器上設定伺服器網路設定。|  
|設定公司網路中的路由|設定公司網路中的路由，確保適當地路由流量。|  
|設定防火牆|如有必要，設定其他防火牆。|  
|設定 CA 和憑證|啟用 DirectAccess 精靈會設定一種利用使用者名稱和密碼進行驗證的內建 Kerberos Proxy。 也會在遠端存取伺服器上設定 IP-HTTPS 憑證。|  
|設定 DNS 伺服器|設定遠端存取伺服器的 DNS 設定。|  
|設定 Active Directory|將用戶端電腦加入 Active Directory 網域。|  
|設定 GPO|視需要為部署設定 GPO。|  
|設定安全性群組|設定將包含 DirectAccess 用戶端電腦的安全性群組，以及部署所需的任何其他安全性群組。|  
|設定網路位置伺服器|啟用 DirectAccess 精靈會在 DirectAccess 伺服器上設定網路位置伺服器。|  
  
## <a name="ConfigNetworkSettings"></a>設定伺服器網路設定  
在 IPv4 和 IPv6 同時存在的環境中，單一伺服器部署需要下列網路介面設定。 您可以使用 [Windows 網路和共用中心] 中的 [變更介面卡設定] 設定所有 IP 位址。  
  
-   邊緣拓撲  
  
    -   一個網際網路對向的公用靜態 IPv4 或 IPv6 位址。  
  
    -   單一內部靜態 IPv4 或 IPv6 位址。  
  
-   位於 NAT 裝置後面 (兩張網路介面卡)  
  
    -   單一內部網路對向的靜態 IPv4 或 IPv6 位址。  
  
-   位於 NAT 裝置後面 (一張網路介面卡)  
  
    -   單一靜態 IPv4 或 IPv6 位址。  
  
> [!NOTE]  
> 如果遠端存取伺服器有兩張網路介面卡 (一張歸類在網域設定檔中，另一張在公用/私人設定檔中)，卻只使用單一 NIC 拓撲，則建議如下：  
>   
> 1.  確定第二張 NIC 也被歸類在網域設定檔中 - 建議選項。  
> 2.  如果第二張 NIC 因任何原因無法設定為網域設定檔，則必須使用下列 Windows PowerShell 命令手動將 DirectAccess IPsec 原則擴大到所有設定檔：  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="ConfigRouting"></a>設定公司網路中的路由  
設定公司網路中的路由，如下所示：  
  
-   在組織中部署原生 IPv6 時，請新增路由，讓內部網路的路由器透過遠端存取伺服器將 IPv6 流量路由回來。  
  
-   在遠端存取伺服器上手動設定組織 IPv4 和 IPv6 的路由。 新增一個已發佈的路由，以便將所有含有組織 (/48) IPv6 首碼的流量轉送到內部網路。 此外，如果是 IPv4 流量，則新增明確的路由，讓 IPv4 流量轉送到內部網路。  
  
## <a name="ConfigFirewalls"></a>設定防火牆  
當部署中使用額外防火牆時，如果遠端存取伺服器位於 IPv4 網際網路，請為遠端存取流量套用下列網際網路對向的防火牆例外：  
  
-   6to4 流量-IP 通訊協定41輸入和輸出。  
  
-   IP-HTTPS-傳輸控制通訊協定（TCP）目的地埠443，以及 TCP 來源埠443輸出。 當遠端存取伺服器具有單一網路介面卡，且網路位置伺服器位於遠端存取伺服器上時，則還需要 TCP 連接埠 62000。  
  
當使用額外防火牆時，如果遠端存取伺服器位於 IPv6 網際網路，請為遠端存取流量套用下列網際網路對向的防火牆例外：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
當使用額外防火牆時，請針對遠端存取流量套用下列內部網路防火牆例外：  
  
-   ISATAP-通訊協定41輸入和輸出  
  
-   適用於所有 IPv4/IPv6 流量的 TCP/UDP  
  
## <a name="ConfigCAs"></a>設定 Ca 和憑證  
啟用 DirectAccess 精靈會設定一種利用使用者名稱和密碼進行驗證的內建 Kerberos Proxy。 也會在遠端存取伺服器上設定 IP-HTTPS 憑證。  
  
### <a name="ConfigCertTemp"></a>設定憑證範本  
使用內部 CA 來簽發憑證時，您必須為 IP-HTTPS 憑證和網路位置伺服器網站憑證設定憑證範本。  
  
##### <a name="to-configure-a-certificate-template"></a>設定憑證範本  
  
1.  在內部 CA 上，使用 [建立憑證範本](https://technet.microsoft.com/library/cc731705.aspx)中所述的方式建立憑證範本。  
  
2.  使用 [部署憑證範本](https://technet.microsoft.com/library/cc770794.aspx)中所述的方式部署憑證範本。  
  
### <a name="configure-the-ip-https-certificate"></a>設定 IP-HTTPS 憑證  
遠端存取需要 IP-HTTPS 憑證來驗證遠端存取伺服器的 IP-HTTPS 連線。 有三種 IP-HTTPS 憑證的憑證選項：  
  
-   **公開**-由協力廠商提供。  
  
    用於 IP-HTTPS 驗證的憑證。 如果憑證主體名稱不是萬用字元，則它必須是只能用於遠端存取伺服器 IP-HTTPS 連線的外部可解析 FQDN URL。  
  
-   **私**用-下列為必要項（如果尚未存在）：  
  
    -   用於 IP-HTTPS 驗證的網站憑證。 憑證主體必須是可從網際網路存取，外部可解析的完整網域名稱 (FQDN)。  
  
    -   可從公用可解析 FQDN 存取的憑證撤銷清單 (CRL) 發佈點。  
  
-   **自我簽署**-下列為必要項（如果尚未存在）：  
  
    > [!NOTE]  
    > 自我簽署憑證無法在多站台部署中使用。  
  
    -   用於 IP-HTTPS 驗證的網站憑證。 憑證主體必須是可從網際網路存取，外部可解析的 FQDN。  
  
    -   可從可公開解析的完整網域名稱 (FQDN) 存取的 CRL 發佈點。  
  
請確定用於 IP-HTTPS 驗證的網站憑證符合下列需求：  
  
-   憑證的一般名稱必須符合 IP-HTTPS 站台的名稱。  
  
-   在 [主體] 欄位中指定遠端存取伺服器之外部對向介面卡的 IPv4 位址，或是 IP-HTTPS URL 的 FQDN。  
  
-   在「增強金鑰使用方法」欄位中使用伺服器驗證物件識別碼 (OID)。  
  
-   在 [CRL 發佈點] 欄位中，指定連線到網際網路的 DirectAccess 用戶端可存取的 CRL 發佈點。  
  
-   IP-HTTPS 憑證必須具有私密金鑰。  
  
-   IP-HTTPS 憑證必須直接匯入到個人存放區。  
  
-   IP-HTTPS 憑證的名稱可以包含萬用字元。  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>從內部 CA 安裝 IP-HTTPS 憑證  
  
1.  在 [遠端存取] 伺服器上：在 [**開始**] 畫面上，輸入**mmc.exe**，然後按 enter。  
  
2.  在 MMC 主控台中，按一下 [檔案] 功能表上的 [新增/移除嵌入式管理單元]。  
  
3.  在 [新增或移除嵌入式管理單元] 對話方塊中，依序按一下 [憑證]、[新增]、[電腦帳戶]、[下一步]、[本機電腦]、[完成]，然後按一下 [確定]。  
  
4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]。  
  
5.  在 [憑證] 上按一下滑鼠右鍵，指向 [所有工作]，然後按一下 [要求新憑證]。  
  
6.  按兩次 [下一步] 。  
  
7.  在 [**要求憑證**] 頁面上，選取憑證範本的核取方塊，如有需要，請按一下 [**需要更多資訊才能註冊此憑證**]。  
  
8.  在 [憑證內容] 對話方塊的 [主體] 索引標籤上，在 [主體名稱] 區域的 [類型] 中選取 [一般名稱]。  
  
9. 在 [值] 中，指定遠端存取伺服器的外部對向介面卡的 IPv4 位址或 IP-HTTPS URL 的 FQDN，然後按一下 [新增]。  
  
10. 在 [別名] 區域的 [類型] 中，選取 [DNS]。  
  
11. 在 [值] 中，指定遠端存取伺服器的外部對向介面卡的 IPv4 位址或 IP-HTTPS URL 的 FQDN，然後按一下 [新增]。  
  
12. 在 [一般] 索引標籤的 [易記名稱] 中，您可以輸入可協助您識別憑證的名稱。  
  
13. 在 [擴充功能] 索引標籤的 [擴充金鑰使用方法] 旁邊，按一下箭號，確認伺服器驗證在 [選取的選項] 清單中。  
  
14. 依序按一下 [確定]、[註冊]，然後按一下 [完成]。  
  
15. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已註冊新憑證且「使用目的」為「伺服器驗證」。  
  
## <a name="ConfigDNS"></a>設定 DNS 伺服器  
您必須為部署中內部網路的網路位置伺服器網站手動設定 DNS 項目。  
  
### <a name="NLS_DNS"></a>建立網路位置伺服器和 web 探查 DNS 記錄  
  
1.  在內部網路 DNS 伺服器上：在 [**開始**] 畫面上，輸入 * * dnsmgmt.msc * *，然後按 enter。  
  
2.  在 [DNS 管理員] 主控台的左窗格中，展開您網域的正向對應區域。 在網域上按一下滑鼠右鍵，按一下 [新增主機 (A 或 AAAA)]。  
  
3.  在 [新增主機] 對話方塊的 [名稱 (如果空白就使用父系網域名稱)] 方塊中，輸入網路位置伺服器網站的 DNS 名稱 (這是 DirectAccess 用戶端用來連線到網路位置伺服器的名稱)。 在 [IP 位址] 方塊中，輸入網路位置伺服器的 IPv4 位址，然後按一下 [新增主機]。 在 [DNS] 對話方塊中，按一下 [確定]。  
  
4.  在 [新增主機] 對話方塊的 [名稱 (如果空白就使用父系網域名稱)] 方塊中，輸入 Web 探查的 DNS 名稱 (預設 Web 探查的名稱是 directaccess-webprobehost)。 在 [IP 位址] 方塊中，輸入 Web 探查的 IPv4 位址，然後按一下 [新增主機]。 為 directaccess-corpconnectivityhost 和任何手動建立的連線能力檢查器重複此程序。 在 [DNS] 對話方塊中，按一下 [確定]。  
  
5.  按一下 \[完成\]。  

![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
您也必須設定下列各項的 DNS 項目：  
  
-   Ip-HTTPs**伺服器**-DirectAccess 用戶端必須能夠從網際網路解析遠端存取服務器的 DNS 名稱。  
  
-   **CRL 撤銷檢查**-directaccess 會針對 directaccess 用戶端與遠端存取服務器之間的 ip-HTTPs 連線，以及 directaccess 用戶端與網路位置伺服器之間的 HTTPS 連接，使用憑證撤銷檢查。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。  
  
## <a name="ConfigAD"></a>設定 Active Directory  
遠端存取伺服器和所有 DirectAccess 用戶端電腦都必須加入 Active Directory 網域。 DirectAccess 用戶端電腦必須是下列其中一種網域類型的成員：  
  
-   與遠端存取伺服器屬於相同樹系的網域。  
  
-   屬於與遠端存取伺服器樹系具有雙向信任關係之樹系的網域。  
  
-   與遠端存取伺服器網域具有雙向網域信任關係的的網域。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>將用戶端電腦加入網域  
  
1.  在 [**開始**] 畫面上，輸入**explorer .exe**，然後按 enter。  
  
2.  在 [電腦] 圖示上按一下滑鼠右鍵，然後按一下 [內容]。  
  
3.  在 [系統] 頁面上，按一下 [進階系統設定]。  
  
4.  在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
5.  在 [電腦名稱] 中，如果您將伺服器加入網域時也要變更電腦名稱，請輸入該電腦名稱。 在 [隸屬於]下面，按一下 [網域]，然後輸入要加入伺服器的網域名稱，例如，corp.contoso.com，然後按一下 [確定]。  
  
6.  提示您輸入使用者名稱和密碼時，輸入有權將電腦加入至網域的使用者名稱及密碼，然後按一下 [確定]。  
  
7.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]。  
  
8.  當提示您必須重新啟動電腦時，按一下 [確定]。  
  
9. 按一下 [系統內容] 對話方塊中的 [關閉]。 出現提示時，按一下 [立即重新啟動]。  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
請注意，在您輸入下方的 Add-Computer 命令後，必須提供網域認證。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>設定 Gpo  
若要部署遠端存取，您至少需要兩個群組原則物件：一個群組原則物件包含遠端存取服務器的設定，一個包含 DirectAccess 用戶端電腦的設定。 當您設定「遠端存取」時，嚮導會自動建立必要的群組原則物件。 不過，如果您的組織強制執行命名慣例，或您沒有建立或編輯群組原則物件的必要許可權，則必須在設定遠端存取之前先建立它們。  
  
若要建立群組原則物件，請參閱[建立和編輯群組原則物件](https://technet.microsoft.com/library/cc754740.aspx)。  
  
> [!IMPORTANT]  
> 系統管理員可以使用下列步驟，手動將 DirectAccess 群組原則物件連結到組織單位：  
>   
> 1.  設定 DirectAccess 之前，請先將建立的 GPO 連結到個別的組織單位。  
> 2.  為用戶端電腦指定安全性群組來設定 DirectAccess。  
> 3.  「遠端存取」系統管理員不一定擁有將群組原則物件連結到網域的許可權。 在這兩種情況下，都會自動設定群組原則物件。 如果 GPO 已經連結到 OU，該連結將不會被移除，而 GPO 將不會被連結到網域。 以伺服器 GPO 來說，OU 必須包含伺服器電腦物件，否則 GPO 就會被連結到網域的根目錄。  
> 4.  如果在執行 DirectAccess wizard 之前尚未連結到 OU，則在設定完成之後，網域系統管理員可以將 DirectAccess 群組原則物件連結到所需的組織單位。 您可以移除網域的連結。 在[這裡](https://technet.microsoft.com/library/cc732979.aspx)可以找到將群組原則物件連結到組織單位的步驟。  
  
> [!NOTE]  
> 如果群組原則物件是以手動方式建立，則在 DirectAccess 設定期間可能會無法使用群組原則物件。 群組原則物件可能尚未複寫到最接近管理電腦的網域控制站。 在此情況下，系統管理員可以等候複寫完成，或強制複寫。  
  
## <a name="ConfigSGs"></a>設定安全性群組  
用戶端電腦中包含的 DirectAccess 設定群組原則物件，只會套用到您在設定遠端存取時所指定之安全性群組的成員電腦。 此外，如果您使用安全性群組來管理應用程式伺服器，請為這些伺服器建立安全性群組。  
  
### <a name="Sec_Group"></a>建立 DirectAccess 用戶端的安全性群組  
  
1.  在 [**開始**] 畫面上，輸入**dsa.msc**，然後按 enter。 在 [Active Directory 使用者和電腦] 主控台的左窗格中，展開將包含安全性群組的網域，在 [使用者] 上按一下滑鼠右鍵，指向 [新增]，然後按一下 [群組]。  
  
2.  在 [新增物件 - 群組] 對話方塊中的 [群組名稱] 之下，輸入安全性群組的名稱。  
  
3.  在 [群組領域] 之下按一下 [全域]，在 [群組類型] 之下按一下 [安全性]，然後按一下 [確定]。  
  
4.  連按兩下 DirectAccess 用戶端電腦安全性群組，然後在 [內容] 對話方塊中，按一下 [成員] 索引標籤。  
  
5.  在 [成員] 索引標籤上，按一下 [新增]。  
  
6.  在 [選取使用者、連絡人、電腦或服務帳戶] 對話方塊中，選取您想要啟用 DirectAccess 的用戶端電腦，然後按一下 [確定]。  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**Windows powershell 對等命令**  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>設定網路位置伺服器  
網路位置伺服器所在的的伺服器必須具備高可用性，以及 DirectAccess 用戶端信任的有效 SSL 憑證。 有兩種網路位置伺服器憑證的憑證選項：  
  
-   **私**用-下列為必要項（如果尚未存在）：  
  
    -   用於網路位置伺服器的網站憑證。 憑證主體必須是網路位置伺服器的 URL。   
  
    -   可從內部網路高度可用的 CRL 發佈點。  
  
-   **自我簽署**-下列為必要項（如果尚未存在）：  
  
    > [!NOTE]  
    > 自我簽署憑證無法在多站台部署中使用。  
  
    -   用於網路位置伺服器的網站憑證。 憑證主體必須是網路位置伺服器的 URL。  
  
> [!NOTE]  
> 如果網路位置伺服器網站位於遠端存取伺服器，則會在設定遠端存取繫結至您所提供的伺服器憑證時建立網站。  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>從內部 CA 安裝網路位置伺服器憑證  
  
1.  在將裝載網路位置伺服器網站的伺服器上：在 [**開始**] 畫面上，輸入**mmc.exe**，然後按 enter。  
  
2.  在 MMC 主控台中，按一下 [檔案] 功能表上的 [新增/移除嵌入式管理單元]。  
  
3.  在 [新增或移除嵌入式管理單元] 對話方塊中，依序按一下 [憑證]、[新增]、[電腦帳戶]、[下一步]、[本機電腦]、[完成]，然後按一下 [確定]。  
  
4.  在 [憑證] 嵌入式管理單元的主控台樹狀目錄中，開啟 [憑證 (本機電腦)\個人\憑證]。  
  
5.  在 [憑證] 上按一下滑鼠右鍵，指向 [所有工作]，然後按一下 [要求新憑證]。  
  
6.  按兩次 [下一步] 。  
  
7.  在 [**要求憑證**] 頁面上，選取憑證範本的核取方塊，如有需要，請按一下 [**需要更多資訊才能註冊此憑證**]。  
  
8.  在 [憑證內容] 對話方塊的 [主體] 索引標籤上，在 [主體名稱] 區域的 [類型] 中選取 [一般名稱]。  
  
9. 在 [值] 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]。  
  
10. 在 [別名] 區域的 [類型] 中，選取 [DNS]。  
  
11. 在 [值] 中，輸入網路位置伺服器網站的 FQDN，然後按一下 [新增]。  
  
12. 在 [一般] 索引標籤的 [易記名稱] 中，您可以輸入可協助您識別憑證的名稱。  
  
13. 依序按一下 [確定]、[註冊]，然後按一下 [完成]。  
  
14. 在 [憑證] 嵌入式管理單元的詳細資料窗格中，確認已註冊新憑證且「使用目的」為「伺服器驗證」。  
  

  


