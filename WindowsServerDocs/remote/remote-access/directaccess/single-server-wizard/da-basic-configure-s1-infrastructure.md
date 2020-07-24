---
title: 步驟1設定基本 DirectAccess 基礎結構
description: 本主題是使用適用于 Windows Server 2016 的消費者入門 Wizard 部署單一 DirectAccess 伺服器指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: ba4de2a4-f237-4b14-a8a7-0b06bfcd89ad
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a1312834c42a168111e544f9a1ef4cf119ea5a8a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953590"
---
# <a name="step-1-configure-the-basic-directaccess-infrastructure"></a>步驟1設定基本 DirectAccess 基礎結構

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何針對在混合了 IPv4 和 IPv6 的環境中使用單一 DirectAccess 伺服器的基本 DirectAccess 部署，設定所需的基礎結構。 開始部署步驟之前，請確定您已完成[規劃基本 DirectAccess 部署](../../../remote-access/directaccess/single-server-wizard/Plan-a-Basic-DirectAccess-Deployment.md)中所述的規劃步驟。  
  
|工作|描述|  
|----|--------|  
|設定伺服器網路設定|設定 DirectAccess 伺服器上的伺服器網路設定。|  
|設定公司網路中的路由|設定公司網路中的路由，確保適當地路由流量。|  
|設定防火牆|如有必要，設定其他防火牆。|  
|設定 DNS 伺服器|設定 DirectAccess 伺服器的 DNS 設定。|  
|設定 Active Directory|將用戶端電腦和 DirectAccess 伺服器加入 Active Directory 網域。|  
|設定 GPO|視需要為部署設定 GPO。|  
|設定安全性群組|設定將包含 DirectAccess 用戶端電腦的安全性群組，以及部署所需的任何其他安全性群組。|  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>設定伺服器網路設定  
在 IPv4 和 IPv6 同時存在的環境中，單一伺服器部署需要下列網路介面設定。 您可以使用 [Windows 網路和共用中心]**** 中的 [變更介面卡設定]**** 設定所有 IP 位址。  
  
-   邊緣拓撲  
  
    -   一個網際網路對向的公用靜態 IPv4 或 IPv6 位址。  
  
        > [!NOTE]  
        > Teredo 需要兩個連續的公用 IPv4 位址。 如果您不是使用 Teredo，則可以設定單一公用靜態 IPv4 位址。  
  
    -   單一內部靜態 IPv4 或 IPv6 位址。  
  
-   位於 NAT 裝置後面 (兩張網路介面卡)  
  
    -   單一內部網路對向的靜態 IPv4 或 IPv6 位址。  
  
    -   單一內部部署網路對向的靜態 IPv4 或 IPv6 位址。  
  
-   位於 NAT 裝置後面 (一張網路介面卡)  
  
    -   單一靜態 IPv4 或 IPv6 位址。  
  
> [!NOTE]  
> 如果 DirectAccess 伺服器有兩張或更多網路介面卡 (一張歸類在網域設定檔中，另一張在公用/私人設定檔中)，但您想要使用單一 NIC 拓撲，則建議如下：  
>   
> 1.  確定第二張 NIC 和任何額外的 NIC 皆歸類於網域設定檔。  
> 2.  如果第二張 NIC 因任何原因無法設定為網域設定檔，則必須使用下列 Windows PowerShell 命令手動將 DirectAccess IPsec 原則擴大到所有設定檔：  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
>   
>     IPsec 原則的名稱是 DirectAccess-DaServerToInfra 和 DirectAccess-DaServerToCorp。  
  
## <a name="configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>設定公司網路中的路由  
設定公司網路中的路由，如下所示：  
  
-   在組織中部署原生 IPv6 時，請新增路由，讓內部網路的路由器透過遠端存取伺服器將 IPv6 流量路由回來。  
  
-   在遠端存取伺服器上手動設定組織 IPv4 和 IPv6 的路由。 新增一個已發佈的路由，以便將所有含有組織 (/48) IPv6 首碼的流量轉送到內部網路。 此外，如果是 IPv4 流量，則新增明確的路由，讓 IPv4 流量轉送到內部網路。  
  
## <a name="configure-firewalls"></a><a name="ConfigFirewalls"></a>設定防火牆  
當部署中使用額外防火牆時，如果遠端存取伺服器位於 IPv4 網際網路，請為遠端存取流量套用下列網際網路對向的防火牆例外：  
  
-   6to4 流量-IP 通訊協定41輸入和輸出。  
  
-   IP-HTTPS-傳輸控制通訊協定（TCP）目的地埠443，以及 TCP 來源埠443輸出。 當遠端存取伺服器具有單一網路介面卡，且網路位置伺服器位於遠端存取伺服器上時，則還需要 TCP 連接埠 62000。  
  
    > [!NOTE]  
    > 這個豁免必須在遠端存取伺服器上設定。 所有其他豁免則必須設定在邊緣防火牆上。  
  
> [!NOTE]  
> 若為 Teredo 和 6to4 流量，則必須針對遠端存取伺服器上的網際網路對向連續公用 IPv4 位址套用這些例外。 若為 IP-HTTPS，這些例外只需套用到伺服器外部名稱解析的位址。  
  
當使用額外防火牆時，如果遠端存取伺服器位於 IPv6 網際網路，請為遠端存取流量套用下列網際網路對向的防火牆例外：  
  
-   IP 通訊協定 50  
  
-   UDP 目的地連接埠 500 輸入，及 UDP 來源連接埠 500 輸出。  
  
當使用額外防火牆時，請針對遠端存取流量套用下列內部網路防火牆例外：  
  
-   ISATAP-通訊協定41輸入和輸出  
  
-   適用於所有 IPv4/IPv6 流量的 TCP/UDP  
  
## <a name="configure-the-dns-server"></a><a name="ConfigDNS"></a>設定 DNS 伺服器  
您必須為部署中內部網路的網路位置伺服器網站手動設定 DNS 項目。  
  
### <a name="to-create-the-network-location-server-and-ncsi-probe-dns-records"></a><a name="NLS_DNS"></a>建立網路位置伺服器與 NCSI 探查 DNS 記錄  
  
1.  在內部網路 DNS 伺服器上，執行**dnsmgmt.msc** ，然後按 enter。  
  
2.  在 [DNS 管理員]**** 主控台的左窗格中，展開您網域的正向對應區域。 在網域上按一下滑鼠右鍵，按一下 [新增主機 (A 或 AAAA)]****。  
  
3.  在 [新增主機]**** 對話方塊的 [名稱 (如果空白就使用父系網域名稱)]**** 方塊中，輸入網路位置伺服器網站的 DNS 名稱 (這是 DirectAccess 用戶端用來連線到網路位置伺服器的名稱)。 在 [IP 位址]**** 方塊中，輸入網路位置伺服器的 IPv4 位址，然後按一下 [新增主機]****。 在 [DNS]**** 對話方塊中，按一下 [確定]****。  
  
4.  在 [新增主機]**** 對話方塊的 [名稱 (如果空白就使用父系網域名稱)]**** 方塊中，輸入 Web 探查的 DNS 名稱 (預設 Web 探查的名稱是 directaccess-webprobehost)。 在 [IP 位址]**** 方塊中，輸入 Web 探查的 IPv4 位址，然後按一下 [新增主機]****。 為 directaccess-corpconnectivityhost 和任何手動建立的連線能力檢查器重複此程序。 在 [DNS]**** 對話方塊中，按一下 [確定]****。  
  
5.  按一下 [完成] 。  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
您也必須設定下列各項的 DNS 項目：  
  
-   Ip-HTTPs**伺服器**-DirectAccess 用戶端必須能夠從網際網路解析遠端存取服務器的 DNS 名稱。  
  
-   **CRL 撤銷檢查**-directaccess 會針對 directaccess 用戶端與遠端存取服務器之間的 ip-HTTPs 連線，以及 directaccess 用戶端與網路位置伺服器之間的 HTTPS 連接，使用憑證撤銷檢查。 在這兩種情況下，DirectAccess 用戶端都必須要能夠解析和存取 CRL 發佈點位置。  
  
## <a name="configure-active-directory"></a><a name="ConfigAD"></a>設定 Active Directory  
遠端存取伺服器和所有 DirectAccess 用戶端電腦都必須加入 Active Directory 網域。 DirectAccess 用戶端電腦必須是下列其中一種網域類型的成員：  
  
-   與遠端存取伺服器屬於相同樹系的網域。  
  
-   屬於與遠端存取伺服器樹系具有雙向信任關係之樹系的網域。  
  
-   與遠端存取伺服器網域具有雙向網域信任關係的的網域。  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>將遠端存取伺服器加入網域  
  
1.  在 [伺服器管理員] 中，按一下 [本機伺服器]****。 在詳細資料窗格中，按一下 [電腦名稱]**** 旁邊的連結。  
  
2.  在 [**系統**內容] 對話方塊中，按一下 [**電腦名稱稱**] 索引標籤。在 [**電腦名稱稱**] 索引標籤上，按一下 [**變更**]。  
  
3.  在 [電腦名稱]**** 中，如果您將伺服器加入網域時也要變更電腦名稱，請輸入該電腦名稱。 在 [隸屬於]**** 下面，按一下 [網域]****，然後輸入要加入伺服器的網域名稱，例如，corp.contoso.com，然後按一下 [確定]****。  
  
4.  提示您輸入使用者名稱和密碼時，輸入有權將電腦加入至網域的使用者名稱及密碼，然後按一下 [確定]****。  
  
5.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]****。  
  
6.  當提示您必須重新啟動電腦時，按一下 [確定]****。  
  
7.  按一下 [系統內容]**** 對話方塊中的 [關閉]****。  
  
8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]****。  
  
#### <a name="to-join-client-computers-to-the-domain"></a>將用戶端電腦加入網域  
  
1.  執行**explorer.exe**。  
  
2.  在 [電腦] 圖示上按一下滑鼠右鍵，然後按一下 [內容]****。  
  
3.  在 [系統]**** 頁面上，按一下 [進階系統設定]****。  
  
4.  在 [系統內容]**** 的 [電腦名稱]**** 索引標籤上，按一下 [變更]****。  
  
5.  在 [電腦名稱]**** 中，如果您將伺服器加入網域時也要變更電腦名稱，請輸入該電腦名稱。 在 [隸屬於]**** 下面，按一下 [網域]****，然後輸入要加入伺服器的網域名稱，例如，corp.contoso.com，然後按一下 [確定]****。  
  
6.  提示您輸入使用者名稱和密碼時，輸入有權將電腦加入至網域的使用者名稱及密碼，然後按一下 [確定]****。  
  
7.  在出現對話方塊並顯示您的網域的歡迎頁面時，按一下 [確定]****。  
  
8.  當提示您必須重新啟動電腦時，按一下 [確定]****。  
  
9. 按一下 [系統內容]**** 對話方塊中的 [關閉]。 出現提示時，按一下 [立即重新啟動]****。  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
請注意，在您輸入下方的 Add-Computer 命令後，必須提供網域認證。  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="configure-gpos"></a><a name="ConfigGPOs"></a>設定 GPO  
若要部署遠端存取，您至少需要兩個群組原則物件：一個群組原則物件包含遠端存取服務器的設定，一個包含 DirectAccess 用戶端電腦的設定。 當您設定遠端存取時，嚮導會自動建立必要的群組原則物件。 不過，如果您的組織強制執行命名慣例，或您沒有建立或編輯群組原則物件的必要許可權，則必須在設定遠端存取之前先建立它們。  
  
若要建立群組原則物件，請參閱[建立和編輯群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11))。  
  
> [!IMPORTANT]  
> 系統管理員可以使用下列步驟，手動將 DirectAccess 群組原則物件連結到組織單位：  
>   
> 1.  設定 DirectAccess 之前，請先將建立的 GPO 連結到個別的組織單位。  
> 2.  為用戶端電腦指定安全性群組來設定 DirectAccess。  
> 3.  系統管理員可能有、也可能不會有將群組原則物件連結到網域的權限。 在這兩種情況下，都會自動設定群組原則物件。 如果 GPO 已經連結到 OU，該連結將不會被移除。 GPO 將不會被連結到網域。 以伺服器 GPO 來說，OU 必須包含伺服器電腦物件，否則 GPO 就會被連結到網域的根目錄。  
> 4.  如果在執行 DirectAccess 精靈之前尚未連結到 OU，則設定完成之後，系統管理員可以將 DirectAccess 群組原則物件連結到所需的組織單位。 您可以移除網域的連結。 在[這裡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11))可以找到將群組原則物件連結到組織單位的步驟  
  
> [!NOTE]  
> 如果手動建立群組原則物件，則在 DirectAccess 設定期間可能會無法使用群組原則物件。 群組原則物件可能尚未複寫到最接近管理電腦的網域控制站。 在此情況下，系統管理員可以等候複寫完成，或強制複寫。

> [!Warning]
> 不支援使用 DirectAccess 設定向導以外的任何方式來設定 DirectAccess，例如直接修改 DirectAccess 群組原則物件，或手動修改伺服器或用戶端上的預設原則設定。
  
## <a name="configure-security-groups"></a><a name="ConfigSGs"></a>設定安全性群組  
用戶端電腦群組策略物件中包含的 DirectAccess 設定只會套用到您在設定「遠端存取」時所指定之安全性群組的成員電腦。  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>為 DirectAccess 用戶端建立安全性群組  
  
1.  執行**dsa.msc**。 在 [Active Directory 使用者和電腦]**** 主控台的左窗格中，展開將包含安全性群組的網域，在 [使用者]**** 上按一下滑鼠右鍵，指向 [新增]****，然後按一下 [群組]****。  
  
2.  在 [新增物件 - 群組]**** 對話方塊中的 [群組名稱]**** 之下，輸入安全性群組的名稱。  
  
3.  在 [群組領域]**** 之下按一下 [全域]****，在 [群組類型]**** 之下按一下 [安全性]****，然後按一下 [確定]****。  
  
4.  連按兩下 DirectAccess 用戶端電腦安全性群組，然後在 [內容] 對話方塊中，按一下 [成員]**** 索引標籤。  
  
5.  在 [成員]**** 索引標籤上，按一下 [新增]****。  
  
6.  在 [選取使用者、連絡人、電腦或服務帳戶]**** 對話方塊中，選取您想要啟用 DirectAccess 的用戶端電腦，然後按一下 [確定]****。  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Windows powershell 對等命令**  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="next-step"></a><a name="BKMK_Links"></a>下一步  
  
-   [步驟 2：設定基本 DirectAccess 伺服器](da-basic-configure-s2-server.md)  
  
