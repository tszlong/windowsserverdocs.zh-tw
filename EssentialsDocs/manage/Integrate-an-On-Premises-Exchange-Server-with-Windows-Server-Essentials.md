---
title: "與 Windows Server Essentials 整合先 Exchange Server"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>與 Windows Server Essentials 整合先 Exchange Server

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本指南提供資訊和基本的指示來協助您整合 Exchange Server 執行執行的 Windows Server Essentials 的伺服器上場所伺服器並完成設定。  
  
 本指南應該先部署的 Windows Server Essentials 網路上執行 Exchange Server 先伺服器。  
  
> [!NOTE]
>  Exchange Server 2010 不支援執行 Windows Server 2012 的電腦上安裝。  
  
## <a name="prerequisites"></a>必要條件  
 安裝之前 Exchange Server Windows Server Essentials 網路上，請確定您完成的工作，本節所述。  
  
-   [執行 Windows Server Essentials 的伺服器設定](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [準備要安裝 Exchange Server 的第二個伺服器](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [設定您的網際網路網域名稱](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>執行 Windows Server Essentials 的伺服器設定  
 您必須已經設定執行 Windows Server Essentials 的伺服器。 這將會網域控制站伺服器 Exchange Server 執行。 如需有關如何設定 Windows Server Essentials 的資訊，請查看[安裝 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_SecondServer"></a>準備要安裝 Exchange Server 的第二個伺服器  
 您必須安裝 Exchange Server 上執行的版本的 Windows Server 作業系統的正式支援執行 Exchange Server 2010 第二部伺服器或 Exchange Server 2013。 然後您必須將第二部伺服器加入 Windows Server Essentials 網域。  
  
 了解如何加入 Windows Server Essentials 網域中的第二個伺服器的資訊，會看到網路中加入的第二個伺服器[連接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  
  
> [!NOTE]
>  Microsoft 不會支援執行 Windows Server Essentials 的伺服器上安裝 Exchange Server。  
  
###  <a name="BKMK_DomainNames"></a>設定您的網際網路網域名稱  
 若要整合執行 Exchange Server 與 Windows Server Essentials 先伺服器，您必須已經登記完畢為您的企業有效網際網路網域名稱 (例如*contoso.com*)。 您還必須使用您的網域名稱提供者來建立需要 Exchange Server 的 DNS 資源記錄。  
  
 例如，如果您的公司網際網路的網域名稱是 contoso.com，而您想要使用的完整的網域名稱 (FQDN) *mail.contoso.com*參考 Exchange server 您先伺服器，以搭配您的網域名稱提供者來建立 DNS 資源記錄下表中。  
  
|資源記錄名稱|使用碼表進行類型|使用碼表進行設定|描述|  
|--------------------------|-----------------|--------------------|-----------------|  
|電子郵件|主機 (A)|地址 =*指派給您的 ISP 公用 IP 位址*|Exchange Server 將會收到郵件傳送給 mail.contoso.com。<br /><br /> 在您的選取項目，您可以使用不同的名稱。|  
|MX|「 郵件 」 交換程式 (MX)|主機名稱 = @<br /><br /> Address=mail.contoso.com<br /><br /> 喜好設定 = 0|提供的電子郵件訊息路由適用於email@contoso.com以到達您先伺服器執行 Exchange Server。|  
|SPF|(TXT) 的文字|v = spf1 mx ~ 所有|資源記錄協助防止從您的伺服器為被視為垃圾郵件已傳送電子郵件。|  
|autodiscover._tcp|服務 (SRV)|服務： _autodiscover<br /><br /> 通訊協定： _tcp<br /><br /> 優先順序： 0<br /><br /> 減重： 0<br /><br /> 連接埠： 443<br /><br /> 目標主機： mail.contoso.com|可讓 Microsoft Office Outlook 和行動裝置版裝置自動探索 Exchange server 您先伺服器。<br /><br /> **注意：**您也可以設定自動探索 (A) 主機的資源記錄及記錄指向上場所 server 執行 Exchange Server 的公用 IP 位址。 不過，如果您執行此選項，您必須也提供支援 mail.contoso.com 和 autodiscover.contoso.com 網域名稱主題替代名稱 （舊） SSL 憑證。|  
  
> [!NOTE]
>  -   取代*contoso.com*在此範例中，您登記網際網路網域名稱。  
  
 您必須先伺服器執行 Exchange Server 的 FQDN 您使用執行 Windows Server Essentials 伺服器比選擇不同的 FQDN。 例如，您可以選擇使用*remote.contoso.com*以 FQDN 存取伺服器 Windows Server Essentials 執行從網際網路的電腦使用。 您可以使用*mail.contoso.com*以電子郵件傳送到您的先伺服器執行 Exchange Server 的 FQDN。  
  
## <a name="install-exchange-server"></a>安裝 Exchange Server  
 Exchange Server 整合的功能在 Windows Server Essentials 支援下列 Exchange Server 版本：  
  
-   Exchange Server 2013  
  
-   交換與 Service Pack 1 (SP1) Server 2010  
  
 您在第二個伺服器上安裝 Exchange Server 之前，您必須先將目前的管理員，若要**企業系統管理員**群組。  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>將目前的管理員新增至企業系統管理員群組  
  
1.  以系統管理員身分登入 Windows Server Essentials。  
  
2.  系統管理員身分執行 Windows PowerShell。  
  
3.  Windows PowerShell 命令提示字元中，輸入**新增-ADGroupMember ˜Enterprise 管理員 $env： 使用者名稱**，然後按 Enter 鍵。  
  
#### <a name="to-install-exchange-server"></a>若要安裝 Exchange Server  
  
1.  以系統管理員身分登入第二部伺服器。  
  
2.  打開網際網路瀏覽器，並瀏覽至[Exchange Server 部署小幫手]](https://go.microsoft.com/fwlink/p/?LinkID=249163)的網站。  
  
3.  按一下**上的場所只**。  
  
4.  按一下新的版本，您將會安裝 Exchange Server 安裝選項。  
  
    > [!NOTE]
    >  如果您從安裝 Windows 的小型企業伺服器移轉，您應該選取適當的升級選項，包括移轉步驟。  
  
5.  在下一個頁面，接受預設設定，，然後按一下**下一步**。  
  
    > [!NOTE]
    >  如果您打算使用公用資料夾中 Exchange Server 的全新安裝時，變更該設定**[是]**。  
  
6.  依照檢查清單中部署 Exchange Server 的逐步指示。  
  
     Exchange Server 部署小幫手也可讓您：  
  
    -   列印一份檢查清單。  
  
    -   傳送電子郵件收件者的一份檢查清單。  
  
    -   下載為 PDF 檔案的檢查清單。  
  
> [!NOTE]
>  -   您必須隨時 Exchange server 的伺服器上安裝管理工具。 在 Windows Server Essentials Exchange Server 整合的功能需要管理工具。  
> -   如果您需要設定目錄，我們建議也設定**InternalUrl**會以相同的 URL 屬性**ExternalUrl**的每個 virtual directory 屬性。 如需詳細資訊，請查看[管理 Client 存取伺服器目錄](https://go.microsoft.com/fwlink/p/?LinkId=251058)Exchange Server 2010 online 協助網站。  
> -   如果您想要存取 Outlook Web Access (OWA) 從網路存取網站上 Windows Server Essentials 中，您必須 OWA 設定的 URL 外部屬性。  
  
 如果您在全新的安裝程式安裝 Exchange Server 2010，您也可以使用下列的指令碼 Exchange Server 設定。  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>若要使用的指令碼設定 Exchange Server  
  
1.  打開 「 記事本 」，並下列指令碼貼入新的檔案：  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  將檔案儲存為**InstallDependencies.ps1**。  
  
3.  伺服器上的位置複製 Exchange SSL 憑證。  
  
4.  打開新的 「 記事本 」 檔案，並將下列文字複製檔案到：  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  設定的參數，以反映網路環境指令碼的開頭。  
  
6.  將檔案儲存為**ConfigureExchange.ps1**。  
  
7.  系統管理員身分執行 Windows PowerShell。  
  
8.  Windows PowerShell 命令提示字元中，輸入**設定為 ExecutionPolicy RemoteSigned**，然後按 Enter 鍵。  
  
9. 執行指令碼**InstallDependencies.ps1**。  
  
10. 將伺服器，然後執行的 Windows PowerShell 重新開機以系統管理員。  
  
11. Windows PowerShell 命令提示字元中，執行下列指令碼：  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  請務必在輸入正確的路徑 Exchange Server 安裝程式。  
  
12. Exchange Server 安裝完成後，請打開換貨管理殼層系統管理員的身分。  
  
13. 換貨管理殼層命令提示字元中，輸入**設定為 ExecutionPolicy RemoteSigned**，然後按 Enter 鍵。  
  
14. 執行指令碼**ConfigureExchange.ps1**。  
  
15. 重新開機伺服器。  
  
> [!NOTE]
>  如果您選擇使用 SSL 公開受信任的憑證，而不是自我發行憑證，您可以依照指示建立憑證要求，並將其傳送到您選取憑證授權單位節目表設定中。 您也可以使用 Exchange PowerShell cmdlet 來建立憑證要求。 範例如下。  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  自訂的指令碼參數，以反映您網路的環境。  
  
## <a name="post-installation-tasks"></a>安裝後工作  
 本章節描述您可能需要安裝後階段包含適用於設定 Windows Server Essentials 網路上執行 Exchange Server 先伺服器的資訊中完成的伺服器設定工作。  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>新增電子郵件公用網域並設定電子郵件地址原則  
  
> [!NOTE]
>  如果您正在執行全新安裝，這是必要的工作。 如果您從 Windows 小型企業伺服器移轉，請略過此步驟。  
  
 必須指定您的電子郵件網域將接受預設網域中，並設定電子郵件地址原則。  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>若要將您的電子郵件的網域新增為預設值接受網域  
  
1.  請依照下列指示 Exchange Server 文章中的[建立接受網域](https://go.microsoft.com/fwlink/p/?LinkId=249174)來新增接受的網域。  
  
2.  第二個伺服器以系統管理員身分登入，開放 Exchange 管理主控台中，瀏覽至**傳輸中樞**索引標籤的**的組織設定**。  
  
3.  換貨管理主控台工作窗格中，新接受的網域中，按一下滑鼠右鍵，然後按一下 [**設定為預設值**。  
  
4.  請依照下列指示 Exchange Server 文章中的[建立的電子郵件地址原則](https://go.microsoft.com/fwlink/p/?LinkId=249179)來建立新的電子郵件地址原則。 您可以接受電子郵件地址以外的預設值。 電子郵件地址，指定您的電子郵件公用網域。  
  
### <a name="create-smtp-send-and-receive-connectors"></a>建立 SMTP 傳送和接收連接器  
  
> [!NOTE]
>  這是必要的工作。  
  
 您必須設定 SMTP 傳送連接器及輸出日靠近花朵的電子郵件訊息傳輸 SMTP 接收的連接器。  
  
 若要建立 SMTP 傳送連接器，請依照下列 Exchange Server 文件中的指示[建立 SMTP 傳送連接器](https://technet.microsoft.com/library/aa997285.aspx)。  
  
 若要建立 SMTP 接收連接器，請依照下列 Exchange Server 文件中的指示[建立 SMTP 收到連接器](https://technet.microsoft.com/library/bb125159.aspx)。  
  
 選項，您可以參考稍早建立傳送的文件的指令碼，並使用 Exchange PowerShell cmdlet 接收連接器。  
  
### <a name="configure-the-network-router"></a>設定路由器網路  
  
> [!NOTE]
>  如果您正在執行全新安裝，這是必要的工作。 如果您的移轉 Windows 小型企業伺服器，查看[的伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)設定網路的相關指示。  
  
 最小，您必須在路由器上設定連接埠下列設定：  
  
|路由器連接埠|目的地 IP|目的地連接埠|注意|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|內部 Exchange server 先伺服器的 IP。|25||  
|80 (HTTP)|執行 Windows Server Essentials 的伺服器內部 IP|80||  
|443 (HTTPS)|執行 Windows Server Essentials 的伺服器內部 IP|443||  
  
 如果您支援 POP3 或 IMAP 訊息通訊協定，您網路上的，您還必須設定連接埠 forwardings 這些通訊協定。 相關資訊，會看到一節**Client 存取伺服器**主題中的[Exchange 網路連接埠參考](https://go.microsoft.com/fwlink/p/?LinkId=250773)中 Exchange Server TechNet Library。  
  
> [!NOTE]
>  -   我們建議您設定靜態 IP 位址執行的 Windows Server Essentials 的伺服器，並執行 Exchange Server 的第二部伺服器。 了解如何設定執行 Windows Server 2003 或 Windows Server 2008 R2 的電腦上的靜態 IP 位址的指示，請查看[設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx)在 Windows Server TechNet Library  
>   
>      **注意：**的 DNS 伺服器設定應永遠指向執行的 Windows Server Essentials 的伺服器的 IP 位址。  
> -   在路由器上，請確定執行的 Windows Server Essentials 的伺服器及 Exchange server 的伺服器的 IP 位址會保留，或超過 DHCP IP 位址範圍。  
> -   在本區段中路由器設定假設您已指派給透過網際網路服務提供者 (ISP) 只有一個公用 IP 位址。 如果您有多個公用 IP 位址，您可以執行的 Windows Server Essentials 的伺服器，並執行 Exchange Server 的伺服器指定不同的 IP 位址，然後再建立連接埠 forwardings 根據公用 IP 位址。  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>在 Windows Server Essentials 先 Exchange Server 整合  
  
> [!NOTE]
>  如果您要安裝 Windows 的小型企業伺服器移轉，我們建議您目前略過此步驟，並執行之後，您在解除安裝之前 Exchange Server 來源伺服器上安裝。  
  
 您安裝並將 Exchange server 的伺服器設定之後，您必須讓先 Exchange Server 整合執行的 Windows Server Essentials 的伺服器上。  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>若要從儀表板上場所 Exchange Server 整合  
  
1.  登入執行 Windows Server Essentials 的系統管理員的身分，以伺服器，然後打開儀表板。  
  
2.  在**Home**頁面上，按一下 [**連接至我的電子郵件服務**，然後按一下 [**整合 Exchange Server**。  
  
3.  在 [資訊] 窗格中，按一下**設定 Exchange Server 整合**。  
  
4.  請依照精靈中的指示進行。  
  
### <a name="configure-a-reverse-proxy"></a>設定反向 proxy  
  
> [!NOTE]
>  如果您有一個網際網路連接網際網路服務提供者，這是必要的工作。  
  
 Exchange Server 與 Windows Server Essentials 支援某些遠端存取案例中網路使用者。 例如您在任何地方存取關閉執行的 Windows Server Essentials 的伺服器上，如果您可以從遠端存取遠端 Web 存取網站或 virtual 私人網路 (VPN) 遠端連接到 Windows Server Essentials 網路。 若要從遠端存取的電子郵件訊息，您必須使用 Outlook 隨處、 Outlook Web Access (OWA) 或 ActiveSync。  
  
 如果 Windows Server Essentials 和執行 Exchange Server 伺服器相同路由器連接兩個和只有一個輸入網際網路連接網際網路服務提供者所提供的路由器，您必須使用反向 proxy 方案路由傳送從依據目的主機名稱網際網路遠端存取要求不同類型。 我們建議您使用 Microsoft 支援 IIS 應用程式要求路由 (ARR) 做為您反向 proxy 方案的擴充功能。 更多有關應用程式要求路由 IIS 的詳細資訊，請造訪[的應用程式要求路由網站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  
  
##### <a name="to-install-and-configure-application-request-routing"></a>安裝和設定路由應用程式要求  
  
1.  以系統管理員身分登入 Windows Server Essentials。  
  
2.  打開網際網路瀏覽器，並瀏覽至[的應用程式要求路由網站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。  
  
3.  ARR 在網站上，按一下 [**安裝**，然後依照指示安裝 ARR.  
  
    > [!NOTE]
    >  ARR 安裝期間，您必須選取 URL 重新寫入模組。  
    >   
    >  您可能會收到錯誤的 ARR 安裝未成功安裝 KB 2589179 ARR 2.5 的結尾。 您可以放心地忽略此錯誤。  
  
4.  重新開機 ARR 安裝完成時，**遠端桌面閘道**如果這不執行的服務。  
  
    > [!NOTE]
    >  在您安裝 ARR 後**遠端桌面閘道**服務可能會停止。 若要重新服務，請打開**服務**系統管理工具，並再重新開機**遠端桌面閘道**服務。  
  
5.  [下載適用於 ARR 2.5 KB2732764](https://go.microsoft.com/fwlink/?LinkID=258302)，並在執行 Windows Server Essentials 的伺服器上安裝的更新。  
  
6.  Exchange Server SSL 憑證檔案複製到執行 Windows Server Essentials 的伺服器。 憑證檔案必須包含私密金鑰，而且它必須使用 PFX 檔案格式。  
  
    > [!NOTE]
    >  如果您使用自我發行的憑證，請依照下列指示 Exchange Server 文章中的[匯出 Exchange 憑證](https://technet.microsoft.com/library/dd351274.aspx)若要匯出的憑證。  
  
7.  根據您執行 Windows Server Essentials 的版本，請執行下列其中一個動作：  
  
    -   在 Windows Server Essentials： 開放為系統管理員的身分在命令視窗中，然後打開 %ProgramFiles%\Windows Server\Bin directory  
  
    -   在 Windows Server Essentials： 開放為系統管理員的身分在命令視窗中，然後打開 %Windir%\System32\Essentials directory。  
  
8.  根據您的安裝案例，請依照下列步驟來設定 ARR 的其中一個：  
  
    -   如果您正在執行全新安裝，請執行下列命令：  
  
         * * ARRConfig 組態憑證 * **的憑證檔案的路徑** * 主機 * **主機 Exchange Server 的名稱* ****  
  
        > [!NOTE]
        >  例如;* * ARRConfig 組態憑證***c:\temp\certificate.pfx***主機 ***mail.contoso.com***  
        >   
        >  取代*mail.contoso.com*的憑證來保護您的網域名稱。  
  
    -   在您的移轉 Windows 小型企業伺服器，執行下列命令：  
  
         * * ARRConfig 組態憑證 * **的憑證檔案的路徑** * 主機 * **主機名稱 Exchange Server 的** * targetserver * **伺服器 Exchange Server 的完整名稱* ****  
  
         例如;* * ARRConfig 組態憑證***c:\temp\certificate.pfx***主機***mail.contoso.com*** targetserver * * * ExchangeSvr * * *  
  
         取代*mail.contoso.com*以您的網域名稱。 取代*ExchangeSvr*以 server 執行 Exchange Server 的完整名稱。  
  
9. 出現提示時，輸入憑證的密碼。  
  
> [!NOTE]
>  -   必須包含您所提供的主機名稱在您購買 Exchange Server SSL 憑證。  
> -   如果您有多個主機名稱，可用於 （，） 逗號分隔它們。  
  
 若要確認該設定的運作方式，請嘗試存取 OWA 網站伺服器執行 Exchange Server (https://mail。 *yourdomainname*.com 日 owa) 未加入網域的電腦。 若要疑難排解連接的問題，您也可以使用 online [Microsoft 遠端連接分析器](https://go.microsoft.com/fwlink/p/?LinkId=249455)工具。  
  
### <a name="configure-split-dns-for-exchange-server"></a>設定分割 DNS Exchange Server 的  
  
> [!NOTE]
>  這是建議的工作。  
  
 分割 DNS 可讓您的主機同名，根據起始 DNS 要求 DNS 設定不同的 IP 位址。 如果 client 電腦在企業網路，DNS 要求解析 IP 位址。 如果 client 電腦上，DNS 要求解析網際網路 IP 位址。 這是透明使用者。  
  
 我們建議您設定分割 DNS 的方式，讓使用者可以隨時存取 Exchange Server 使用主機相同名稱的服務，無論他們的位置。  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>若要設定分割 DNS Exchange Server 的  
  
1.  Windows Server Essentials 為系統管理員的身分登入，然後打開 DNS 管理員。  
  
2.  DNS Manager 主控台樹上，您的伺服器，以滑鼠右鍵按一下，然後按一下 [**新增區域]**。 **新增區精靈]**會顯示。  
  
3.  在**區域類型**頁面精靈的接受預設選項，然後按一下 [**下**。  
  
4.  在**Active Directory 區域複寫領域**頁面，接受預設選項，然後按一下 [**下**。  
  
5.  上**轉寄或反向對應區域**頁面上，請接受或選取**正向對應區域**，然後按一下 [**下一步**。  
  
6.  在**區域名稱**頁面上，輸入您的伺服器 （例如; 執行 Exchange Server 的 FQDN*mail.contoso.com*)，然後按**下**。  
  
7.  在**動態更新**頁面上，接受預設選項，按一下 [**下一步**，然後按一下 [**完成**。  
  
8.  在 DNS Manager 主控台新正向對應的區域，以滑鼠右鍵按一下，然後按一下**（A 或 AAAA） 的新主機**。  
  
9. 在**新主機**頁面中，保留**名稱**欄位空白，輸入您執行的 Exchange Server 的伺服器內部 IP 位址，然後按一下**新增主機**。  
  
    > [!NOTE]
    >  當您離開**名稱**欄位中空白，伺服器預設使用家長網域名稱。  
  
10. 在**新主機**頁面上，按**完成**。  
  
> [!NOTE]
>  如果您使用 ActiveSync，但無法同步處理電子郵件的一些信箱帳號，判斷是否那些帳號有一或多個例如網域系統管理員受保護的群組成員。 可協助您修正這個問題的相關的相關資訊，請查看[Exchange ActiveSync 傳回 HTTP 500 錯誤](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx)。  
  
## <a name="related-topics"></a>相關的主題  
 整合先 Exchange Server 的相關詳細資訊，會看到以下的各節。  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>萬一停用整合換貨？  
 停用與先 Exchange Server 整合，如果您不會再無法使用 Windows Server Essentials 儀表板檢視、 建立，或管理 Exchange Server 信箱。  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>我需要知道的相關電子郵件帳號項目？  
 設定電子郵件裝載的方案伺服器上。 從裝載的電子郵件提供者，例如 Microsoft Office 365、 方案可以提供網路使用者的個人電子郵件帳號。 當您新增使用者 Account 精靈在執行 Windows Server Essentials 建立帳號時，精靈會嘗試帳號加入提供裝載的電子郵件方案。 在此同時，精靈將電子郵件名稱 （別名） 指派給使用者，並設定信箱 （配額） 的最大值。 最大大小信箱會根據您所使用的電子郵件提供者而有所不同。 新增帳號之後, 您可以繼續從使用者的屬性頁面管理信箱別名和配額資訊。 適用於完整的帳號和裝載的電子郵件提供者管理，使用管理主控台裝載提供者。 根據您的提供者，您可以存取他們管理主控台從 web 架構入口網站，或伺服器儀表板中的索引標籤。  
  
 使用者別名建議名稱為您提供時，您可以執行 [新增使用者 Account 精靈別名傳送裝載的電子郵件提供者。 例如，如果使用者別名*FrankM*，可能會使用者 s 電子郵件地址* FrankM@Contoso.com *。  
  
 此外的密碼，您的使用者，在 [新增使用者 Account 精靈中的設定將會起始密碼方案裝載的電子郵件中的使用者。  
  
 最後，如果您使用 Delete 使用者 Account 精靈伺服器上 delete 使用者，精靈也會傳送要求裝載的電子郵件提供者來 delete 使用者從，以及他們的系統。 提供者可能 delete 帳號 s 和 account 相關聯的電子郵件。  
  
 有關使用者如何設定所需的電子郵件 client 軟體或如何存取電子郵件帳號，請參考裝載的電子郵件提供者所提供的協助文件。  
  
### <a name="what-is-a-mailbox-quota"></a>信箱配額為何？  
 信箱限額就是配置網路使用者 s 換貨信箱資料儲存空間量。  
  
 當您執行**設定 Exchange Server 整合**工作儀表板，精靈將頁面加入新增使用者 Account 精靈，可讓您選擇是否執行信箱配額，以及指定配額大小。 根據預設，**執行信箱配額**選取 [選項 （），以及使用者信箱指派 2 GB 的儲存空間。 換貨的系統管理員可以自訂符合企業需求的信箱配額設定。  
  
## <a name="see-also"></a>也了  
  
-   [Windows Server Essentials 的系統需求](../get-started/system-requirements.md)  
  
-   [管理電子郵件服務整合](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
