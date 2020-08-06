---
title: 整合內部部署 Exchange Server 與 Windows Server Essentials
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cef547570c58c405ac563a1c2215feda120350f4
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837877"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>整合內部部署 Exchange Server 與 Windows Server Essentials

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本指南提供資訊和基本指示，協助您設定和整合執行 Exchange Server 的內部部署伺服器與執行 Windows Server Essentials 的伺服器。

 嘗試在 Windows Server Essentials 網路部署執行 Exchange Server 的內部部署伺服器之前，請先閱讀本指南。

> [!NOTE]
>  Exchange Server 2010 不支援在執行 Windows Server 2012 的電腦上進行安裝。

## <a name="prerequisites"></a>先決條件
 在 Windows Server Essentials 網路上安裝 Exchange Server 之前，請確定您已完成本節所述的工作。

-   [設定執行 Windows Server Essentials 的伺服器](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)

-   [準備要安裝 Exchange Server 的第二部伺服器](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)

-   [設定您的網際網路網域名稱](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)

###  <a name="set-up-a-server-that-is-running-windows-server-essentials"></a><a name="BKMK_SetUpSBS8"></a>設定執行 Windows Server Essentials 的伺服器
 您必須設定好執行 Windows Server Essentials 的伺服器。 這會是執行 Exchange Server 之伺服器的網域控制站。 如需有關如何設定 Windows Server Essentials 的資訊，請參閱[安裝 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)。

###  <a name="prepare-a-second-server-on-which-to-install-exchange-server"></a><a name="BKMK_SecondServer"></a>準備要安裝 Exchange Server 的第二部伺服器
 您必須在執行 Windows Server 作業系統版本的第二部伺服器上安裝 Exchange Server，該作業系統版本正式支援執行 Exchange Server 2010 或 Exchange Server 2013。 接著，您必須將第二部伺服器加入 Windows Server Essentials 網域。

 如需如何將第二部伺服器加入 Windows Server Essentials 網域的相關資訊，請參閱在[取得連線](../use/Get-Connected-in-Windows-Server-Essentials.md)中將第二部伺服器加入網路。

> [!NOTE]
>  Microsoft 不支援在執行 Windows Server Essentials 的伺服器上安裝 Exchange Server。

###  <a name="configure-your-internet-domain-name"></a><a name="BKMK_DomainNames"></a>設定您的網際網路功能變數名稱
 若要整合執行 Exchange Server 的內部部署伺服器與 Windows Server Essentials，您必須為公司註冊有效的網際網路網域名稱 (例如 *contoso.com*)。 您還必須洽詢您的網域名稱提供者，以建立 Exchange Server 需要的 DNS 資源記錄。

 例如，如果您的公司網際網路網域名稱為 contoso.com，您想使用完整網域名稱 (FQDN) *mail.contoso.com* 來參照執行 Exchange Server 的內部部署伺服器，請洽詢您的網域名稱提供者以建立下表中的 DNS 資源記錄。


| 資源記錄名稱 |     記錄類型     |                                                                         記錄設定                                                                          |                                                                                                                                                                                                                                                              說明                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         mail         |      主機 (A)       |                                                        位址=*您的 ISP 指派的公用 IP 位址*                                                         |                                                                                                                                                                                                   Exchange Server 會接收傳送到 mail.contoso.com 的郵件。<br /><br /> 您可以使用自己選擇的其他名稱。                                                                                                                                                                                                    |
|          MX          | 郵件交換程式 (MX) |                                            主機名稱=@<br /><br /> 位址=mail.contoso.com<br /><br /> 喜好設定=0                                             |                                                                                                                                                                                                      提供的電子郵件訊息路由， email@contoso.com 以抵達執行 Exchange server 的內部部署伺服器。                                                                                                                                                                                                       |
|         SPF          |     文字 (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      協助防止將您伺服器傳送的電子郵件識別為垃圾郵件的資源記錄。                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    服務 (SRV)    | 服務：_autodiscover<br /><br /> 通訊協定：_tcp<br /><br /> 優先順序：0<br /><br /> 權數：0<br /><br /> 連接埠：443<br /><br /> 目標主機：mail.contoso.com | 讓 Microsoft Office Outlook 和行動裝置自動探索執行 Exchange Server 的內部部署伺服器。<br /><br /> **注意：** 您也可以 () 資源記錄設定自動探索主機，並將記錄指向執行 Exchange Server 之內部部署伺服器的公用 IP 位址。 不過，如果您實作這個選項，也必須提供同時支援 mail.contoso.com 和 autodiscover.contoso.com 網域名稱的主體別名 (SAN) SSL 憑證。 |

> [!NOTE]
>  -   用您註冊的網際網路網域名稱取代這個範例中的 *contoso.com* 項目。

 執行 Exchange Server 之內部部署伺服器的 FQDN 必須與執行 Windows Server Essentials 之伺服器使用的 FQDN 不同。 例如，您可以選擇使用 *remote.contoso.com* 做為電腦用來從網際網路存取執行 Windows Server Essentials 之伺服器的 FQDN。 您可以使用 *mail.contoso.com* 做為用來將電子郵件路由傳送到執行 Exchange Server 之內部部署伺服器的 FQDN。

## <a name="install-exchange-server"></a>安裝 Exchange Server
 Windows Server Essentials 的 Exchange Server 整合功能支援下列 Exchange Server 版本：

- Exchange Server 2013

- Exchange Server 2010 含 Service Pack 1 (SP1)

  在第二部伺服器上安裝 Exchange Server 之前，必須先將目前的系統管理員帳戶新增至 **Enterprise Admins** 群組。

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>將目前的系統管理員帳戶新增至 Enterprise Admins 群組

1.  以系統管理員身分登入 Windows Server Essentials。

2.  以系統管理理員身分執行 Windows PowerShell。

3.  在 Windows PowerShell 命令提示字元中，輸入**add-adgroupmember "Enterprise Admins" $env： username**，然後按 enter。

#### <a name="to-install-exchange-server"></a>安裝 Exchange Server

1.  以系統管理員身分登入第二部伺服器。

2.  開啟網際網路瀏覽器，然後巡覽至 [Exchange Server 部署助理](https://go.microsoft.com/fwlink/p/?LinkID=249163) 網站。

3.  按一下 [On-Premises Only]****。

4.  按一下您將安裝的 Exchange Server 版本的新安裝選項。

    > [!NOTE]
    >  如果您是從 Windows Small Business Server 安裝進行移轉，應該選取涵蓋移轉步驟的適當升級選項。

5.  在下一頁中，接受預設值，然後按 [下一頁]****。

    > [!NOTE]
    >  如果您計畫要全新安裝的 Exchange Server 中使用公用資料夾，請將該設定變更為 [是]****。

6.  依照檢查清單中的逐步指示部署 Exchange Server。

     Exchange Server 部署助理也可讓您：

    -   列印檢查清單。

    -   將檢查清單傳送給電子郵件收件者。

    -   將檢查清單下載為 PDF 檔。

> [!NOTE]
> - 您必須選擇將管理工具安裝在執行 Exchange Server 的伺服器上。 Windows Server Essentials 的 Exchange Server 整合功能需要管理工具。
>   -   如果您需要設定虛擬目錄，建議您將每個虛擬目錄的 [InternalUrl]**** 屬性與 [ExternalUrl]**** 屬性設定成相同的 URL。 如需詳細資訊，請參閱 Exchange Server 2010 線上說明網站的 [管理用戶端存取伺服器虛擬目錄](https://go.microsoft.com/fwlink/p/?LinkId=251058) 。
>   -   如果您要從 Windows Server Essentials 的遠端 Web 存取網站內存取 Outlook Web Access (OWA)，必須為 OWA 設定外部 URL 屬性。

 如果您以初始狀態安裝的方式安裝 Exchange Server 2010，也可以使用下列指令碼安裝 Exchange Server。

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>使用指令碼安裝 Exchange Server

1.  開啟記事本，將下列指令碼貼到新檔案中：

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. 將檔案另存為 **InstallDependencies.ps1**。

3. 將 Exchange SSL 憑證複製到伺服器上的某個位置。

4. 開啟新的記事本檔案，將下列文字複製到檔案：

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. 在指令碼開頭的位置設定參數，反映您的網路環境。

6. 將檔案儲存為 **ConfigureExchange.ps1**。

7. 以系統管理理員身分執行 Windows PowerShell。

8. 在 Windows PowerShell 命令提示字元中輸入 **Set-ExecutionPolicy RemoteSigned**，然後按 Enter。

9. 執行指令碼 **InstallDependencies.ps1**。

10. 重新啟動伺服器，然後以系統管理員身分執行 Windows PowerShell。

11. 在 Windows PowerShell 命令提示字元中，執行下列指令碼：

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`

   > [!NOTE]
   >  務必輸入 Exchange Server 安裝程式的正確路徑。

12. 當 Exchange Server 安裝完成後，以系統管理員身分開啟 Exchange 管理命令介面。

13. 在 Exchange 管理命令介面命令提示字元中，輸入 **Set-ExecutionPolicy RemoteSigned**，然後按 Enter 鍵。

14. 執行指令碼 **ConfigureExchange.ps1**。

15. 重新啟動伺服器。

> [!NOTE]
>  如果您決定使用公開信任的 SSL 憑證，而不是自行發行的憑證，您可以遵循安裝指南中的指示來建立憑證要求，並將它傳送至您選取的憑證授權單位單位。 您也可以使用 Exchange PowerShell Cmdlet 建立憑證要求。 範例如下。
>
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`
>
>  自訂指令碼參數以反映您的網路環境。

## <a name="post-installation-tasks"></a>後續安裝工作
 本節說明您可能需要在後續安裝階段完成的伺服器設定工作，其中包含設定 Windows Server Essentials 網路上執行 Exchange Server 之內部部署伺服器的特定資訊。

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>新增公用電子郵件網域和設定電子郵件地址原則

> [!NOTE]
>  這是執行初始狀態安裝的必要工作。 如果您是從 Windows Small Business Server 進行移轉，請略過這個步驟。

 您必須將電子郵件網域指定為預設公認的網域，然後設定電子郵件地址原則。

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>將您的電子郵件網域新增為預設公認的網域

1.  遵循 Exchange Server 文章 [建立公認的網域](https://go.microsoft.com/fwlink/p/?LinkId=249174) 中的指示，加入接受的網域。

2.  以系統管理員身分登入第二部伺服器，開啟 Exchange 管理主控台，然後瀏覽至 [組織組態]**** 的 [集線傳輸]**** 索引標籤。

3.  在 Exchange 管理主控台工作窗格中，在新的公認的網域上按一下滑鼠右鍵，然後按一下 [設為預設值]****。

4.  遵循 Exchange Server 文章 [建立電子郵件地址原則](https://go.microsoft.com/fwlink/p/?LinkId=249179) 中的指示，建立新的電子郵件地址原則。 您可以接受除了電子郵件地址之外的所有預設值。 電子郵件地址則指定您的公用電子郵件網域。

### <a name="create-smtp-send-and-receive-connectors"></a>建立 SMTP 傳送和接收連接器

> [!NOTE]
>  這是必要的工作。

 您必須為電子郵件訊息的外寄/內送傳輸設定 SMTP 傳送連接器和 SMTP 接收連接器。

 若要建立 SMTP 傳送連接器，請遵循 Exchange Server 文章 [建立 SMTP 傳送連接器](/previous-versions/office/exchange-server-2010/aa997285(v=exchg.141))中的指示。

 若要建立 SMTP 接收連接器，請遵循 Exchange Server 文章 [建立 SMTP 接收連接器](/previous-versions/office/exchange-server-2010/bb125159(v=exchg.141))中的指示。

 您可以選擇參考本文件前面的指令碼，利用 Exchange PowerShell Cmdlet 來建立傳送和接收連接器。

### <a name="configure-the-network-router"></a>設定網路路由器

> [!NOTE]
>  這是執行初始狀態安裝的必要工作。 如果您是從 Windows Small Business Server 進行移轉，請參閱[移轉伺服器資料到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)以瞭解有關如何設定網路的指示。

 您至少必須在路由器設定下列連接埠設定：

|路由器連接埠|目的地 IP|目的地連接埠|注意事項|
|-----------------|--------------------|----------------------|----------|
|25 (SMTP)|執行 Exchange Server 的內部部署伺服器的內部 IP。|25||
|80 (HTTP)|執行 Windows Server Essentials 的伺服器的內部 IP|80||
|443 (HTTPS)|執行 Windows Server Essentials 的伺服器的內部 IP|443||

 如果您的網路支援 POP3 或 IMAP 郵件通訊協定，還必須為這些通訊協定設定連接埠轉送。 如需相關資訊，請參閱 Exchange Server TechNet 文件庫 **Exchange 網路連接埠參照** 主題中的 [＜用戶端存取伺服器＞](https://go.microsoft.com/fwlink/p/?LinkId=250773) 一節。

> [!NOTE]
> - 我們建議您為執行 Windows Server Essentials 的伺服器和執行 Exchange Server 的第二部伺服器設定靜態 IP 位址。 如需如何在執行 Windows Server 2003 或 Windows Server 2008 R2 的電腦上設定靜態 IP 位址的指示，請參閱 Windows Server TechNet 文件庫中的 [設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) 。
>
>   **注意：** DNS 伺服器設定應該一律指向執行 Windows Server Essentials 之伺服器的 IP 位址。
>   -   在路由器上，確定執行 Windows Server Essentials 的伺服器和執行 Exchange Server 的伺服器的 IP 位址已經保留，還是超出 DHCP IP 位址範圍。
>   -   本節的路由器設定假設您只有一個由網際網路服務提供者 (ISP) 指派的公用 IP 位址。 如果您有多個公用 IP 位址，可以指派不同的 IP 位址給執行 Windows Server Essentials 的伺服器和執行 Exchange Server 的伺服器，然後根據公用 IP 位址建立連接埠轉送。

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>在 Windows Server Essentials 上啟用內部部署 Exchange Server 整合

> [!NOTE]
>  如果您是從 Windows Small Business Server 安裝進行移轉，建議您現在略過這個步驟，在來源伺服器上解除安裝之前安裝的 Exchange Server 之後再執行這個步驟。

 安裝並設定執行 Exchange Server 的伺服器之後，必須在執行 Windows Server Essentials 的伺服器上啟用內部部署 Exchange Server 整合。

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>從儀表板啟用內部部署 Exchange Server 整合

1.  以系統管理員身分登入執行 Windows Server Essentials 的伺服器，然後開啟儀表板。

2.  在 [首頁]**** 頁面上，按一下 [連線到我的電子郵件服務]****，然後按一下 [整合您的 Exchange Server]****。

3.  在資訊窗格中，按一下 [設定 Exchange Server 整合]****。

4.  遵循精靈的指示進行。

### <a name="configure-a-reverse-proxy"></a>設定反向 Proxy

> [!NOTE]
>  如果您只有一個網際網路服務提供者提供的網際網路連線，這是必要工作。

 Windows Server Essentials 和 Exchange Server 都支援網路使用者的一些遠端存取案例。 例如，如果您在執行 Windows Server Essentials 的伺服器上開啟「隨處存取」，可以從遠端存取「遠端 Web 存取」網站，或使用虛擬私人網路 (VPN) 遠端連線到 Windows Server Essentials 網路。 若要遠端存取電子郵件訊息，必須使用 Outlook 無所不在、Outlook Web Access (OWA) 或 ActiveSync。

 如果 Windows Server Essentials 和執行 Exchange Server 的伺服器連線到相同的路由器，而您的網際網路服務提供者只提供一個連線到路由器的連入網際網路連線，則您必須使用反向 Proxy 解決方案，根據目的地主機名稱，從網際網路路由傳送不同類型的遠端存取要求。 建議您使用 Microsoft 支援的 IIS 應用程式要求路由 (ARR) 延伸做為您的反向 Proxy 解決方案。 如需 IIS 應用程式要求路由的詳細資訊，請造訪 [應用程式要求路由網站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。

##### <a name="to-install-and-configure-application-request-routing"></a>安裝和設定應用程式要求路由

1. 以系統管理員身分登入 Windows Server Essentials。

2. 開啟網際網路瀏覽器，巡覽至 [應用程式要求路由網站](https://go.microsoft.com/fwlink/p/?LinkId=249181)。

3. 在 ARR 網站上，按一下 [安裝]****，然後依照指示安裝 ARR。

   > [!NOTE]
   >  您必須在 ARR 安裝時選取 URL Rewrite Module。
   >
   >  您可能會在 ARR 安裝最後收到錯誤，ARR 2.5 的 KB 2589179 未成功安裝。 您可以放心地忽略此錯誤。

4. ARR 安裝完成時，如果 [遠端桌面閘道]**** 服務未執行，請將它重新啟動。

   > [!NOTE]
   >  安裝 ARR 之後，[遠端桌面閘道]**** 服務可能會停止。 若要手動重新啟動服務，請開啟 [服務]**** 系統管理工具，然後重新啟動 [遠端桌面閘道]**** 服務。

5. [下載 ARR 2.5 的 KB2732764](https://go.microsoft.com/fwlink/?LinkID=258302)，然後在執行 Windows Server Essentials 的伺服器上安裝更新。

6. 將 Exchange Server 的 SSL 憑證檔案複製到執行 Windows Server Essentials 的伺服器上。 憑證檔案必須包含私密金鑰，而且必須使用 PFX 檔案格式。

   > [!NOTE]
   >  如果您使用自動發行憑證，請遵循 Exchange Server 文章 [匯出 Exchange 憑證](/previous-versions/office/exchange-server-2010/dd351274(v=exchg.141)) 中的指示匯出憑證。

7. 視您目前執行的 Windows Server Essentials 版本而定，執行下列其中一項：

   -   在 Windows Server Essentials 上：以系統管理員身分開啟命令視窗，然後開啟%ProgramFiles%\Windows Server\Bin 目錄。

   -   在 Windows Server Essentials 上：以系統管理員身分開啟命令視窗，然後開啟%Windir%\System32\Essentials 目錄。

8. 根據您的安裝情況，依照下列其中一個步驟來設定 ARR：

   - 如果您執行初始狀態安裝，請執行下列命令：

      **: Arrconfig 設定-** _憑證_檔案的憑證路徑 **-** _Exchange Server 的主機名稱_

     > [!NOTE]
     >  例如，**: Arrconfig config-cert** _c:\temp\certificate.pfx_ **-主機名稱** _mail.contoso.com_
     >
     >  用受憑證保護的網域名稱取代 *mail.contoso.com*。

   - 如果您是從 Windows Small Business Server 進行移轉，執行下列命令：

      **: Arrconfig 設定-** _憑證_檔案的憑證路徑 **-** _exchange server 的主機名稱主機名稱_ **-targetserver** _伺服器名稱的 exchange server_

      例如，**: Arrconfig config-cert** _c:\temp\certificate.pfx_ **-主機名稱** _mail.contoso.com_ **-targetserver** _ExchangeSvr_

      用您的網域名稱取代 *mail.contoso.com*。 用執行 Exchange Server 的伺服器名稱取代 *ExchangeSvr*。

9. 出現提示時，輸入憑證的密碼。

> [!NOTE]
> - 您提供的主機名稱必須包含在您為 Exchange Server 購買的 SSL 憑證中。
>   -   如果您有多個主機名稱，請使用逗號 (,) 加以分隔。

 若要確認設定是否正常運作，請嘗試存取執行 Exchange Server (之伺服器的 OWA 網站 https://mail 。 (https://mail.*您的網域名稱*.com/owa)。 若要疑難排解連線問題，您也可以使用線上 [Microsoft 遠端連線分析程式](https://go.microsoft.com/fwlink/p/?LinkId=249455) 工具。

### <a name="configure-split-dns-for-exchange-server"></a>為 Exchange Server 設定分割 DNS

> [!NOTE]
>  這是建議的工作。

 分割 DNS 可讓您根據 DNS 產生要求的位置，為相同主機名稱的 DNS 設定不同 IP 位址。 如果用戶端電腦位於內部網路，DNS 要求會解析為內部網路 IP 位址。 如果用戶端電腦位於網際網路，DNS 要求會解析為網際網路 IP 位址。 使用者並不會察覺。

 我們建議設定分割 DNS 的方式是：不論使用者的位置為何，一律使用相同主機名稱存取 Exchange Server 服務。

##### <a name="to-configure-split-dns-for-exchange-server"></a>為 Exchange Server 設定分割 DNS

1.  以系統管理員身分登入 Windows Server Essentials，然後開啟 DNS 管理員。

2.  在 DNS 管理員主控台樹狀目錄中，以滑鼠右鍵按一下您的伺服器，然後按一下 [新增區域]****。 [新增區域精靈]**** 隨即顯示。

3.  在精靈的 [區域類型]**** 頁面上，接受預設選項，然後按 [下一步]****。

4.  在 [Active Directory 區域複寫領域]**** 頁面上，接受預設選項，然後按 [下一步]****。

5.  在 [正向或反向對應區域]**** 頁面上，接受或選取 [正向對應區域]****，然後按 [下一步]****。

6.  在 [區域名稱]**** 頁面上，輸入執行 Exchange Server 之伺服器的 FQDN (例如，*mail.contoso.com*)，然後按 [下一步]****。

7.  在 [動態更新]**** 頁面上，接受預設選項，按 [下一步]****，然後按一下 [完成]****。

8.  在 DNS 管理員主控台樹狀目錄中，以滑鼠右鍵按一下新的正向對應區域，然後按一下 [新增主機 (A 或 AAAA)]****。

9. 在 [新增主機]**** 頁面上，將 [名稱]**** 欄位保留空白，輸入執行 Exchange Server 之伺服器的內部網路 IP 位址，然後按一下 [新增主機]****。

    > [!NOTE]
    >  將 [名稱]**** 欄位保留空白時，伺服器預設會使用父系網域名稱。

10. 在 [新增主機]**** 頁面上，按一下 [完成]****。

> [!NOTE]
>  如果您使用 ActiveSync 但無法同步處理部分信箱帳戶的電子郵件，請判斷這些帳戶是否為一或多個受保護群組 (例如 Domain Administrators 群組) 的成員。 如需可協助您解決此問題的相關資訊，請參閱 [Exchange ActiveSync 傳回 HTTP 500 錯誤](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx)。

## <a name="related-topics"></a>相關主題
 如需有關整合內部部署 Exchange Server 的詳細資訊，請參閱下列各節。

### <a name="what-happens-if-i-disable-exchange-integration"></a>如果停用 Exchange 整合，會發生什麼事？
 如果您停用與內部部署 Exchange Server 的整合，您將不能再使用 Windows Server Essentials 的儀表板檢視、建立或管理 Exchange Server 信箱。

### <a name="what-do-i-need-to-know-about-email-accounts"></a>我需要知道電子郵件帳戶的什麼資訊？
 託管的電子郵件解決方案是在您的伺服器設定。 來自主控電子郵件提供者的解決方案（例如 Microsoft Office 365）可以為網路使用者提供個別的電子郵件帳戶。 當您在 Windows Server Essentials 執行「新增使用者帳戶精靈 」建立使用者帳戶時，精靈會嘗試將使用者帳戶新增至可用的託管電子郵件解決方案。 在此同時，精靈會將電子郵件名稱 (別名) 指派給使用者，並設定信箱 (配額) 的最大大小。 信箱的最大大小會根據您使用的電子郵件提供者而有所不同。 新增使用者帳戶之後，您可以繼續從使用者的 [內容] 頁面管理信箱別名和配額資訊。 如果要完整管理您的使用者帳戶及託管的電子郵件提供者，請使用託管的提供者的管理主控台。 視您的提供者而定，您可以從網頁式的入口網站，或從伺服器儀表板中的索引標籤，存取其管理主控台。

 您在執行「新增使用者帳戶精靈 」時提供的別名會傳送至託管的電子郵件提供者，做為使用者別名的建議名稱。 例如，如果使用者別名是*FrankM*，則使用者的電子郵件地址可能是 <em>FrankM@Contoso.com</em> 。

 此外，您為使用者在「新增使用者帳戶精靈」中設定的密碼將會在託管的電子郵件解決方案中做為使用者的初始密碼。

 最後，如果您在伺服器上使用「刪除使用者帳戶精靈」刪除使用者，精靈也會將要求傳送至託管的電子郵件提供者，同時從他們的系統刪除使用者。 提供者可能會同時刪除使用者帳戶和與帳戶相關聯的電子郵件。

 如需有關如何設定必要電子郵件用戶端軟體或如何存取電子郵件帳戶的使用者資訊，請參閱託管的電子郵件提供者所提供的說明文件。

### <a name="what-is-a-mailbox-quota"></a>什麼是信箱配額？
 為網路使用者的 Exchange 信箱資料配置的儲存空間量，稱為信箱配額。

 當您執行儀表板上的 [設定 Exchange Server 整合]**** 工作時，精靈會將頁面新增至「新增使用者帳戶精靈」，讓您選擇是否強制執行信箱配額，並指定配額大小。 根據預設，[強制信箱配額]**** 選項為已選取 (開啟)，使用者信箱會被指派 2 GB 的儲存空間。 Exchange 系統管理員可以自訂信箱配額設定，以符合其商務需求。

## <a name="additional-references"></a>其他參考資料

-   [Windows Server Essentials 系統需求](../get-started/system-requirements.md)

-   [管理電子郵件服務整合](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)