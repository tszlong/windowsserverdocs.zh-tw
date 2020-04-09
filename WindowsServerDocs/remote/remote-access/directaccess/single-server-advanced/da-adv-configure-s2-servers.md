---
title: 步驟2設定 Advanced DirectAccess 伺服器
description: 本主題是使用 Windows Server 2016 部署單一 DirectAccess 伺服器與 Advanced Settings 指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: lizross
author: eross-msft
ms.openlocfilehash: bfcdd58da67f41e84ff0956e3f0a0d9b63fa4f75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854871"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>步驟2設定 Advanced DirectAccess 伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明如何針對在混合了 IPv4 和 IPv6 的環境中使用單一「遠端存取」伺服器的進階「遠端存取」部署，設定所需的用戶端和伺服器設定。 開始部署步驟之前，請確定您已完成[規劃先進的 DirectAccess 部署](Plan-an-Advanced-DirectAccess-Deployment.md)中所述的規劃步驟。  
  
|工作|描述|  
|----|--------|  
|2.1. 安裝遠端存取角色|安裝「遠端存取」角色。|  
|2.2. 設定部署類型|將部署類型設定為 DirectAccess 與 VPN、僅 DirectAccess 或僅 VPN。|  
|[規劃進階 DirectAccess 部署](Plan-an-Advanced-DirectAccess-Deployment.md)|在「遠端存取」伺服器設定包含 DirectAccess 用戶端的安全性群組。|  
|2.4. 設定遠端存取伺服器|設定遠端存取伺服器設定。|  
|2.5. 設定基礎結構伺服器|設定組織中所使用的基礎結構伺服器。|  
|2.6. 設定應用程式伺服器|設定應用程式伺服器，讓它們需要驗證和加密。|  
|2.7. 設定摘要和替代 GPO|檢視「遠端存取」設定摘要，並視需要修改 GPO。|  
|2.8. 如何使用 Windows PowerShell 來設定遠端存取伺服器|使用 Windows PowerShell 設定遠端存取。|  
  
> [!NOTE]  
> 本主題包含可讓您用以自動化文中所述部分程序的範例 Windows PowerShell 指令程式。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-install-the-remote-access-role"></a><a name="BKMK_Role"></a>2.1。 安裝遠端存取角色  
若要部署「遠端存取」，您必須在將組織中將做為「遠端存取」伺服器的伺服器上安裝「遠端存取」角色。  
  
#### <a name="to-install-the-remote-access-role"></a>安裝「遠端存取」角色  
  
1.  在 遠端存取 伺服器的 伺服器管理員主控台的 **儀表板** 中，按一下 **新增角色及功能**。  
  
2.  按 [下一步] 三次以進入 [選取伺服器角色] 畫面。  
  
3.  在 [選取伺服器角色] 頁面上，選取 [遠端存取]，按一下 [新增功能]，然後按 [下一步]。  
  
4.  按 [下一步] 五次。  
  
5.  在 [確認安裝選項] 頁面上，按一下 [安裝]。  
  
6.  在 [安裝進度] 頁面上，確認安裝成功，然後按一下 [關閉]。  
  
![安裝進度成功](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="22-configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>2.2。 設定部署類型  
可以使用「遠端存取管理主控台」以三種方式部署「遠端存取」：  
  
-   DirectAccess 與 VPN  
  
-   僅 DirectAccess  
  
-   僅 VPN  
  
本指南在範例程序中使用的是僅 DirectAccess 部署。  
  
#### <a name="to-configure-the-deployment-type"></a>設定部署類型  
  
1.  在遠端存取服務器上，開啟 [遠端存取管理] 主控台：在 [**開始**] 畫面上，輸入**ramgmtui.exe**，然後按 enter。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
2.  在 [遠端存取管理] 主控台的中央窗格中，按一下 [執行遠端存取安裝精靈]。  
  
3.  在 [設定遠端存取] 對話方塊方塊中，按一下要部署 [DirectAccess 與 VPN]、[僅 DirectAccess] 還是 [僅 VPN]。  
  
## <a name="23-configure-directaccess-clients"></a><a name="BKMK_Clients"></a>2.3。 設定 DirectAccess 用戶端  
若要佈建用戶端電腦以使用 DirectAccess，它必須屬於所選取的安全性群組。 設定 DirectAccess 之後，就會佈建安全性群組中的用戶端電腦來接收 DirectAccess「群組原則物件」(GPO)。 您也可以設定部署案例，這可讓您設定 DirectAccess 來用於用戶端存取和遠端管理，或僅用於遠端管理。  
  
#### <a name="to-configure-directaccess-clients"></a>設定 DirectAccess 用戶端  
  
1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 1 遠端用戶端] 區域中，按一下 [設定]。  
  
2.  在 [DirectAccess 用戶端安裝精靈] 的 [部署案例] 頁面上，按一下您想要在組織中使用的部署案例 ([完整 DirectAccess] 或 [僅用於遠端管理])，然後按 [下一步]。  
  
3.  在 [選取群組] 頁面中，按一下 [新增]。  
  
4.  在 [選取群組] 對話方塊中，選取包含 DirectAccess 用戶端電腦的安全性群組。  
  
    > [!NOTE]  
    > 如果安全性群組位於與「遠端存取」伺服器不同的樹系中，在您完成 [遠端存取設定] 嚮導之後，請按一下 **[工作] 窗格中的 [** 重新整理**管理伺服器**]，以探索新樹系中的網域控制站和 Configuration Manager 伺服器。  
  
5.  視需要選取 [僅針對攜帶型電腦啟用 DirectAccess] 核取方塊，以只允許行動電腦存取內部網路。  
  
6.  視需要選取 [使用強制通道] 核取方塊，以透過「遠端存取」伺服器路由傳送所有用戶端流量 (至內部網路和至網際網路)。  
  
7.  按 [下一步]。  
  
8.  在 [網路連線助理] 頁面上：  
  
    -   在表格中，新增將用來判斷內部網路連線能力的資源。 如果沒有設定任何其他資源，將會自動建立預設 Web 探查。  
  
        > [!CAUTION]  
        > 設定 Web 探查位置來判斷企業網路連線能力時，請確定您至少設定一個 HTTP 型探查。 僅設定 **ping** 探查並不足夠，這可能會導致連線能力狀態判斷不正確。 這是因為 **ping** 已自 IPsec 豁免，因此，它無法確保能夠正確建立 IPsec 通道。  
  
    -   新增「技術服務人員」電子郵件地址，以允許使用者在遇到連線問題時傳送資訊。  
  
    -   提供 DirectAccess 連線的易記名稱。 當使用者按一下通知區域中的網路圖示時，這個名稱就會顯示在網路清單中。  
  
    -   視需要選取 [允許 DirectAccess 用戶端使用本機名稱解析] 核取方塊。  
  
        > [!NOTE]  
        > 已啟用本機名稱解析時，執行 [網路連線助理] 的使用者可以選擇使用 DirectAccess 用戶端電腦上設定的 DNS 伺服器來解析名稱。  
  
9. 按一下 **[完成]** 。  
  
## <a name="24-configure-the-remote-access-server"></a><a name="BKMK_Server"></a>2.4。 設定遠端存取伺服器  
若要部署「遠端存取」，您需要在「遠端存取」伺服器設定正確的網路介面卡、用戶端電腦可連線之「遠端存取」伺服器的公用 URL (ConnectTo 位址)、主體符合 ConnectTo 位址的 IP-HTTPS 憑證、IPv6 設定，以及用戶端電腦驗證。  
  
#### <a name="to-configure-the-remote-access-server"></a>設定遠端存取伺服器  
  
1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 2 遠端存取伺服器] 區域中，按一下 [設定]。  
  
2.  在 [遠端存取伺服器安裝精靈] 的 [網路拓撲] 頁面上，按一下將在您組織中使用的部署拓撲。 在 [輸入用戶端用於連線至遠端存取伺服器的公用名稱或 IPv4 位址]、輸入部署的公用名稱 (此名稱符合 IP-HTTPS 憑證的主體名稱，例如，edge1.contoso.com)，然後按 [下一步]。  
  
3.  在 [網路介面卡] 頁面上，精靈會自動偵測部署中網路的網路介面卡。 如果精靈沒有偵測到正確的網路介面卡，請手動選取正確的介面卡。 精靈也會根據在其上一個步驟中設定的部署公用名稱，自動偵測 IP-HTTPS 憑證。 如果精靈沒有偵測到正確的 IP-HTTPS 憑證，請按一下 [瀏覽] 來手動選取正確的憑證，然後按 [下一步]。  
  
4.  在 首碼設定 頁面 (只有在內部網路中有部署 IPv6 時才會顯示) 上，精靈會自動偵測內部網路中使用的 IPv6 設定。 如果您的部署需要額外的首碼，請設定內部網路的 IPv6 首碼、要指派給 DirectAccess 用戶端電腦的 IPv6 首碼，以及要指派給 VPN 用戶端電腦的 IPv6 首碼。  
  
    > [!NOTE]  
    > 您可以使用分號分隔的清單方式指定多個內部 IPv6 首碼，例如 2001:db8:1::/48;2001:db8:2::/48。  
  
5.  在 [驗證] 頁面上：  
  
    -   在 [使用者驗證] 中，按一下 [Active Directory 認證]。 若要使用雙因素驗證來設定部署，請按一下 [雙因素驗證]。 如需詳細資訊，請參閱[使用 OTP 驗證部署「遠端存取」](https://technet.microsoft.com/library/hh831379.aspx)。  
  
    -   針對多站台和雙因素驗證部署，您必須使用電腦憑證驗證。 請選取 [使用電腦憑證] 核取方塊以使用電腦憑證驗證，並選取 IPsec 根憑證。  
  
    -   若要讓 Windows 7 用戶端電腦透過 DirectAccess 進行連線，請選取 [**啟用 windows 7 用戶端電腦透過 directaccess 進行連線]** 核取方塊。  
  
        > [!NOTE]  
        > 在這種部署中，您必須也使用電腦憑證驗證。  
  
6.  按一下 **[完成]** 。  
  
## <a name="25-configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>2.5。 設定基礎結構伺服器  
若要在「遠端存取」部署中設定基礎結構伺服器，您必須設定網路位置伺服器、DNS 設定 (包括 DNS 尾碼搜尋清單)，以及「遠端存取」不會自動偵測的管理伺服器。  
  
#### <a name="to-configure-the-infrastructure-servers"></a>設定基礎結構伺服器  
  
1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 3 基礎結構伺服器] 區域中，按一下 [設定]。  
  
2.  在 [基礎結構伺服器安裝精靈] 的 [網路位置伺服器] 頁面上，按一下與部署中網路位置伺服器的位置對應的選項。 如果網路位置伺服器位於遠端網頁伺服器上，請先輸入 URL 並按一下 [驗證]，然後再繼續。 如果網路位置伺服器位於「遠端存取」伺服器上，請按一下 [瀏覽] 來找出相關的憑證，然後按 [下一步]。  
  
3.  在 [DNS] 頁面的表格中，輸入將套用為「名稱解析原則表格」(NRPT) 豁免的任何其他名稱尾碼。 選取本機名稱解析選項，然後按 [下一步]。  
  
4.  在 [DNS 尾碼搜尋清單] 頁面上，「遠端存取」伺服器會自動偵測部署中的任何網域尾碼。 請使用 [新增] 和 [移除] 按鈕，從要使用的網域尾碼清單中新增和移除網域尾碼。 若要新增網域尾碼，請在 [新尾碼] 中輸入尾碼，然後按一下 [新增]。 按 [下一步]。  
  
5.  在 [管理] 頁面上，新增系統不會自動偵測的任何管理伺服器，然後按 [下一步]。 遠端存取會自動新增網域控制站和 Configuration Manager 伺服器。  
  
    > [!NOTE]  
    > 雖然會自動新增伺服器，但它們不會出現在清單中。 第一次套用設定之後，Configuration Manager 的伺服器就會出現在清單中。  
  
6.  按一下 **[完成]** 。  
  
## <a name="26-configure-application-servers"></a><a name="BKMK_App"></a>2.6。 設定應用程式伺服器  
在「遠端存取」部署中，設定應用程式伺服器是一項選擇性的工作。 「遠端存取」可讓您針對選取的應用程式伺服器要求進行驗證，若要這麼做，只要將這些伺服器包含在應用程式伺服器安全群組中即可。 根據預設，連到需要驗證之應用程式伺服器的流量也會經過加密；不過，您可以選擇不要加密連到應用程式伺服器的流量，而只使用驗證。  
  
> [!NOTE]  
> 只有在執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的應用程式伺服器上，才支援不加密的驗證。  
  
#### <a name="to-configure-application-servers"></a>設定應用程式伺服器  
  
1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 4 應用程式伺服器] 區域中，按一下 [設定]。  
  
2.  在 [DirectAccess 應用程式伺服器安裝精靈] 中，若要要求向選取的應用程式伺服器進行驗證，請按一下 [將驗證延伸到選取的應用程式伺服器]。 按一下 [新增] 來選取應用程式伺服器安全性群組。  
  
3.  若要限制只能存取應用程式伺服器安全群組中的伺服器，請選取 [僅允許存取安全性群組內含的伺服器] 核取方塊。  
  
4.  若要在沒有加密的情況下使用驗證，請選取 **不要加密流量。僅使用驗證** 核取方塊。  
  
5.  按一下 **[完成]** 。  
  
## <a name="27-configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>2.7。 設定摘要和替代 GPO  
當「遠端存取」設定完成時，會顯示 [遠端存取檢閱]。 您可以檢閱先前選取的所有設定，包括：  
  
1.  **GPO 設定**：列出 DirectAccess 伺服器 GPO 名稱與用戶端 GPO 名稱。 此外，您也可以按一下 [GPO 設定] 標題旁邊的 [變更] 連結來修改 GPO 設定。  
  
2.  **遠端用戶端**：會顯示 DirectAccess 用戶端設定，包括安全性群組、強制通道狀態、連線能力檢查器，以及 DirectAccess 連線名稱。  
  
3.  **遠端存取伺服器**：會顯示 DirectAccess 設定，包括公用名稱/位址、網路介面卡設定、憑證資訊，以及 OTP 資訊 (如果有設定的話)。  
  
4.  **基礎結構伺服器**：此清單包含網路位置伺服器 URL、DirectAccess 用戶端所使用的 DNS 尾碼，以及管理伺服器資訊。  
  
5.  **應用程式伺服器**：除了對特定應用程式伺服器的端對端驗證狀態之外，還會顯示 DirectAccess 遠端管理狀態。  
  
## <a name="28-how-to-configure-the-remote-access-server-by-using-windows-powershell"></a><a name="BKMK_PS"></a>2.8。 如何使用 Windows PowerShell 來設定遠端存取伺服器  
![Windows PowerShell](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**Windows powershell 對等命令**  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
若要在遠端存取的邊緣拓撲中，只在具有根**corp.contoso.com**的網域中執行完整安裝，並使用下列參數：伺服器 GPO： **DirectAccess 伺服器設定**、用戶端 GPO： DirectAccess 用戶端設定、內部網路介面卡：**公司**網路、外部網路介面卡： **Internet**、connectto address： **edge1.contoso.com**和 network location server： **nls.corp.contoso.com**：  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
若要設定「遠端存取」伺服器以使用電腦憑證驗證，並搭配名為 CORP-APP1-CA 的憑證授權單位所簽發的 IPsec 根憑證：  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
若要新增包含名為 **DirectAccessClients** 之 DirectAccess 用戶端的安全性群組，以及移除預設的「網域電腦」安全性群組：  
  
```  
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')  
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')  
```  
  
若要啟用所有電腦（不只是筆記本和膝上型電腦）的遠端存取，以及啟用 Windows 7 用戶端的遠端存取：  
  
```  
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'  
```  
  
若要設定 DirectAccess 用戶端體驗，包括易記的連線名稱和 Web 探查 URL：  
  
```  
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')  
```  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>上一個步驟  
  
-   [步驟1：設定 Advanced DirectAccess 基礎結構](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>後續步驟  
  
-   [步驟3：驗證部署](Step-3-Verify-the-Deployment.md)  
  


