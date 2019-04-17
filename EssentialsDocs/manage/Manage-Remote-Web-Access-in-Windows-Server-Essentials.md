---
title: "管理 Windows Server Essentials 遠端存取"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>管理 Windows Server Essentials 遠端存取

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 遠端 Web Access 與 Windows Server Essentials 體驗角色安裝 Windows Server 2012 R2 或 Windows Server Essentials 中, 提供精簡、方便觸控的瀏覽器體驗的應用程式和從幾乎任何地方資料，您可以連上網際網路存取並使用幾乎所有裝置。 若要使用遠端網站存取的功能，您必須先將它使用設定任何地方存取精靈，並再設定您的路由器和網域名稱。  
  
## <a name="in-this-topic"></a>本主題  
  
-   [關閉並設定遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [設定您的路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [設定您的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [自訂遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [疑難排解遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>關閉並設定遠端 Web 存取  
 下列主題可協助您將並設定遠端 Web 存取：  
  
-   [遠端存取 Web 概觀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [關閉遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [變更您的地區](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [管理網路存取權限](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [安全網路遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [管理使用者遠端 Web Access 與 VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>遠端存取 Web 概觀  
 當您離開您的 office 時，您可以開放網頁瀏覽器，並從任何地方存取網際網路存取遠端 Web 存取。 在遠端網路存取，您可以：  
  
-   存取共用的檔案和資料夾的伺服器上。  
  
-   存取您的伺服器或網路上的電腦。 這表示您可以如同您坐在前面它在您的 office 存取網路電腦的桌面。  
  
  遠端 Web 存取不打開預設。 當您執行任何地方存取精靈設定時，該精靈將會設定您的路由器和連接網際網路。 遠端 Web 存取您電腦之後，您可以設定您的伺服器的網域名稱及自訂遠端 Web 存取。 您也可以設定路由器再試一次您變更路由器如果。  
  
 存取網路上的存取權限未自動授與當您新增新的使用者 account。 當您新增帳號時，您可以選擇允許存取共用的資料夾、[媒體櫃、電腦、首頁連結，並伺服器儀表板。 您也可以指定不允許使用遠端網站存取的使用者。  
  
 存取網路設定為每個使用者帳號顯示在**使用者**索引標籤上的 Windows Server Essentials 儀表板。 若要變更遠端 Web 存取設定，帳號，以滑鼠右鍵按一下，然後按一下**檢視 account 屬性**。  
  
###  <a name="BKMK_TurnOnRWA"></a>關閉遠端存取  
 您可以關閉遠端網站存取的任何地方存取精靈設定伺服器儀表板執行。  
  
##### <a name="to-turn-on-remote-web-access"></a>若要在網頁上的存取  
  
1.  打開儀表板。  
  
2.  按一下**設定**，然後按一下 [**隨處存取**索引標籤。  
  
3.  按一下**設定**。 設定任何地方存取精靈會出現。  
  
4.  在**選擇隨處存取功能，可讓**頁面上，選取**遠端 Web 存取**核取方塊。  
  
5.  請依照下列指示完成精靈。  
  
###  <a name="BKMK_Region"></a>變更您的地區  
 您必須變更地區設定，在 Windows Server Essentials 的網路系統管理員。  
  
##### <a name="to-change-the-region-setting"></a>若要變更的地區設定  
  
1.  在電腦上已連接到 Windows Server Essentials，開放儀表板。  
  
2.  按一下**設定**。  
  
3.  在**一般**索引標籤上，按一下 [下拉式清單中的**伺服器的國家/地區位置**一節。  
  
4.  從下拉式清單中，選取新的區域，然後按一下**套用]**來將接受新的地區設定。  
  
###  <a name="BKMK_ManagePerms"></a>管理網路存取權限  
 當您在 Windows Server Essentials 新增帳號時，新的使用者允許預設來使用遠端存取的網頁。 如果您選擇不要允許遠端網站存取的使用者帳號，並找出使用者必須使用遠端網路存取，您可以更新使用者 account s 屬性。  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>若要管理使用者 account 遠端網路存取權限  
  
1.  儀表板，登入，然後按一下**使用者**。  
  
2.  按一下您想要管理，然後按一下 [使用者 account**檢視 account 屬性**在**工作**窗格。  
  
3.  在**屬性**對話方塊中，按**隨處存取**索引標籤。  
  
4.  在**隨處存取**索引標籤，選取**可讓遠端 Web Access 與網路服務] 應用程式存取**核取方塊，允許使用者連接到使用遠端 Web 存取伺服器。  
  
5.  按一下**套用**，然後按**[確定]**。  
  
 如需詳細資訊，請查看[管理使用者帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_SecureRWA"></a>安全網路遠端存取  
 Windows Server Essentials 使用安全性憑證來協助保護交換軟體與網頁瀏覽器資訊的安全。 當您在電腦上安裝的連接器軟體時，Windows Server Essentials 的安全性憑證已新增至您的電腦上受信任的憑證清單。 當他們會離開您的 office 存取遠端 Web 存取的使用者的最佳方式是使用筆記型電腦已安裝的連接器軟體。  
  
> [!WARNING]
>  使用公用位置從遠端 Web 存取或受信任的其他電腦的使用者應該確定離開自動電腦之前，請先登入時網站，或在用完他們的工作階段。  
  
###  <a name="BKMK_ManageRWAVPN"></a>管理使用者遠端 Web Access 與 VPN  
 您可以連接到 Windows Server Essentials 並存取所有資源是儲存在伺服器上使用 VPN。 這是您的電腦與網路帳號，可以用來連接的 VPN 連接到 Windows Server Essentials 伺服器裝載設定使用 client 的尤其是實用。 Windows Server Essentials 裝載伺服器上的所有新建立的使用者帳號必須使用 VPN 第一次登入 client 的電腦。  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>若要設定網路使用者的 VPN 和遠端網路存取權限  
  
1.  打開儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要從遠端存取桌面的權限授與帳號。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，按一下 [**隨處存取**索引標籤。  
  
6.  在**隨處存取**索引標籤上，執行下列動作：  
  
    1.  若要讓使用者使用 VPN 伺服器連接，請選取 [**允許 Virtual 私人網路 (VPN)**核取方塊。  
  
    2.  若要讓使用者透過遠端 Web 存取伺服器連接，請選取 [**允許遠端 Web Access 與網路服務] 應用程式存取**核取方塊。  
  
7.  按一下**套用**，然後按**[確定]**。  
  
##  <a name="BKMK_2"></a>設定您的路由器  
 當您設定遠端 Web 存取您的伺服器時，請設定任何地方存取精靈會嘗試在路由器設定。 如果您變更路由器或是變更路由器上的設定，您必須重新執行設定您的路由器精靈。 如需詳細資訊，下列主題：  
  
-   [設定您的路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [取代路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [定義的網路位置](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [可讓遠端桌面服務 ActiveX 控制項](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>設定您的路由器  
 在此步驟，Windows Server Essentials 會嘗試使用的命令 UPnP 自動設定您的路由器。 若要這樣做，您的路由器必須支援 UPnP 標準，而且 UPnP 設定必須會在您的路由器支援。  
  
> [!NOTE]
>  您的網路設定與 Windows Server Essentials 的網路支援的需求依循。 在您的網路應該只是其中一個路由器。  
  
 如果來設定您網域名稱精靈未設定路由器，您必須手動轉送 443 連接埠。 了解如何設定您的路由器上的連接埠轉送資訊，請查看[路由器設定](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx)。  
  
###  <a name="BKMK_ReplaceRouter"></a>取代路由器  
 取代 s 製造商的指示，請依據路由器，並執行設定您的路由器精靈設定新路由器。  
  
##### <a name="to-set-up-your-new-router"></a>若要將您新路由器設定  
  
1.  Windows Server Essentials 儀表板，請按一下**設定**。  
  
2.  按一下**隨處存取**索引標籤，然後在**路由器**區段中，按一下 [**設定**。 設定您的路由器精靈開始。  
  
3.  請依照精靈中的指示來完成您的新路由器設定。  
  
###  <a name="BKMK_NetworkLocation"></a>定義的網路位置  
 網路位置是當您連上網路時，適用於 Windows 的網路設定的集合。 設定而有所不同，您可以根據您所使用的網路類型自訂。 網路位置設定會判斷是否某些功能（例如檔案和印表機共用、網路探索與公用資料夾共用）已關閉。 當您需要連接至不同的網路，網路位置非常有用。  
  
 例如，您可能會擁有您使用工作和家庭膝上型電腦。 當您在 office，您連接到 office 網路。 不過，當您進入首頁、您使用您的膝上型電腦存取和播放影片和音樂的已儲存在家用伺服器上。 當您連接到新的網路，並在指定的位置輸入時，Windows 會將指派網路設定檔，會預設為該位置的類型。 下次當您連上網路，Windows 會辨識網路，並自動將指派的正確設定。 這會將層級的安全性，以協助保護您的電腦上的資訊，並只網路功能，您需要的該位置將會亮。  
  
 有四種網路位置：  
  
-   **家用網路**選擇此網路家用網路或當您知道，並信任的人員和網路上的裝置。 家用網路上的電腦可屬於家用群組。 網路探索已家用網路，讓您可以看到網路上的其他電腦和裝置，並讓其他網路使用者來查看您的電腦。  
  
-   **工作的網路**選擇小 office 的此網路或其他地點的網路。 網路探索，可讓您在網路上查看其他電腦和裝置，並讓其他網路使用者來查看您的電腦，在根據預設，但您無法建立或加入家用群組。  
  
-   **公用網路**選擇此網路公用地點（例如咖啡館或機場）。 保留您的電腦會看到到其他電腦，並協助保護您的電腦免於惡意軟體與網際網路的設計目的是此位置。 家用群組並不適用於公用網路，網路探索已關閉。 如果您正在直接連接到網際網路不用路由器，或如果您有連接行動式寬頻，您也應該選擇此選項。  
  
-   **網域**選擇這個網域這些企業工作，例如網路。 控制您的網路系統管理員，這種類型的網路位置，並且無法選取或變更。  
  
###  <a name="BKMK_ActiveX"></a>可讓遠端桌面服務 ActiveX 控制項  
 遠端桌面服務 ActiveX 控制項，可讓您存取您的家用或企業電腦，透過網際網路，從使用遠端 Web 存取其他電腦。  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>若要讓遠端桌面服務 ActiveX 控制項  
  
1.  在 Internet Explorer 中，按一下 [**工具**，然後按**網際網路選項]**。  
  
2.  在**安全性**索引標籤上，按**自訂層級**。  
  
3.  在**ActiveX 控制項以及插**區段中，執行下列動作：  
  
    1.  在**下載簽署 ActiveX 控制項**，按一下 [**提示**。  
  
    2.  在**執行 ActiveX 控制項及插**，按一下 [**支援**。  
  
4.  按一下**[確定]**若接受變更並關閉對話方塊要兩次。  
  
##  <a name="BKMK_3"></a>設定您的網域名稱  
 遠端 Web 存取您電腦之後，您可以設定為您執行的 Windows Server Essentials 的伺服器的網域名稱。 如果您打算使用遠端 Web 存取從遠端電腦，這是必要的步驟。 如需詳細資訊，下列主題：  
  
-   [網域名稱概觀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [了解 Microsoft 的個人化的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [使用新的或現有的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [設定的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [選擇網域名稱服務提供者](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [選擇網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [選擇網域名稱前置詞](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [選擇的網域名稱延伸模組](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [更新或升級您的網域名稱服務](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [匯出或匯入您伺服器上的憑證](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [手動設定的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [尋找您的網域名稱服務提供者](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>網域名稱概觀  
 網域名稱辨識您的伺服器上。 在至少兩個組件包含網域名稱：頂端層級的網域名稱 (TLD) 及第二個層級的網域名稱。 例如，contoso.com，在 com TLD，以 contoso 且的第二個層級的網域名稱。  
  
 離開您的 office 時，您可以使用您的網域名稱存取伺服器上的共用的檔案或網路上的電腦。 當您離開，您也可以管理您的伺服器。 例如，您進行登記 contoso.com 您的伺服器。 當您離開您的 office 時，您可以開放網頁瀏覽器在您的膝上型電腦和輸入**contoso.com**來連接遠端 Web 存取您在 Windows Server Essentials 設定的執行個體的地址文字方塊中。  
  
###  <a name="BKMK_PersonalizedNames"></a>了解 Microsoft 的個人化的網域名稱  
 Microsoft 的個人化的網域名稱包含下列功能：  
  
-   遠端 Web 存取自訂的網域名稱 (例如，*yourhostname*。remotewebaccess.com)。 您的網域名稱是與您的公用 IP 位址。  
  
-   DNS 動態更新服務通訊協定，讓您公用 IP 位址變更時，將不會中斷遠端 Web 存取使用您的網域名稱。 通常，為您的組織 s 寬頻連接網際網路服務提供者 (Isp) 提供動態可變更的公用 IP 位址。  
  
-   信任的憑證相關聯的網域名稱。  
  
 將 Microsoft 的個人化的網域名稱整合與您的伺服器，您必須 Microsoft account（先前稱為 [Windows Live ID）。 如果您不 Microsoft account，您可以申請在[Microsoft Hotmail](https://login.live.com/)的網站。  
  
> [!IMPORTANT]
>  Windows Live 允許特殊字元的伺服器不支援 Microsoft account 密碼。 如果您使用 Microsoft 個人化的網域，請確定您的 Microsoft account 密碼包含伺服器支援的字元。 伺服器不支援使用字元 $，/、'，以及 %。  
  
###  <a name="BKMK_UseNewName"></a>使用新的或現有的網域名稱  
 若要自動設定您在執行 Windows Server Essentials 的伺服器上的網域名稱，您必須使用設定您網域名稱精靈中列出的網域名稱服務提供者。 您可以選擇要取得新的網域名稱，或使用現有的網域名稱。 執行下列其中一個動作：  
  
-   如果您想要取得新的網域名稱的其中一個網域名稱服務提供者所列的精靈中，按一下**我想要設定新的網域名稱**。  
  
-   如果您現有的網域名稱您購買的其中一個支援的網域名稱服務提供者，您可以使用 [設定您網域名稱精靈設定伺服器的網域名稱。 按一下**我想要使用已經擁有的網域名稱**，然後輸入的網域名稱和**設定好您的網域名稱**文字方塊。 您必須提供的使用者名稱和密碼，您用來購買的網域名稱。  
  
-   如果您有您購買的網域名稱服務提供者，不支援 Windows Server Essentials，從現有的網域名稱，並想要設定您的伺服器的網域名稱使用設定您網域名稱精靈，您可以在其中一個列出精靈中的網域名稱服務提供者傳輸的網域名稱。 按一下**我想要使用已經擁有的網域名稱**，輸入的網域名稱**的網域名稱**文字方塊中，並依照指示傳輸的網域名稱網域名稱服務提供者的網站上。  
  
###  <a name="BKMK_SetUpName"></a>設定的網域名稱  
 當您將在網頁上的存取時，您可以選擇設定伺服器的網際網路網域名稱。  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>若要設定，或管理網際網路網域名稱  
  
1.  打開儀表板。  
  
2.  按一下**伺服器設定**，然後按一下 [**隨處存取**索引標籤。  
  
3.  在**的網域名稱**區段中，按**設定**。  
  
4.  請依照下列指示完成精靈。 如果您不會已經擁有的網域名稱和認證，精靈可協助您尋找的網域名稱提供者購買的網域名稱和認證，或您可以取得個人化的 Microsoft 網域名稱。  
  
###  <a name="BKMK_ChooseProvider"></a>選擇網域名稱服務提供者  
 您應該選擇您想要使用的網域名稱擴充功能的支援的網域名稱服務提供者。 設定您網域名稱精靈會包含對符合資格，您可以使用每個 s 提供者的網站連結提供者的清單。 按一下**其他資訊]**以取得相關的服務和價格提供者所提供資訊的每個 s 提供者名稱旁邊的連結。  
  
> [!NOTE]
>  有些網域名稱服務提供者會提供 broad 國際地區和其他提供較小的市場。 因此，部分的提供者可能不會提供的網站時，會被翻譯成您的喜好設定的語言。  
  
 當您購買了您的網域名稱時，您也可以考慮從您的網域名稱服務提供者購買的網域名稱系統」(DNS) 動態更新通訊協定服務。 DNS 動態更新通訊協定是一種服務，可讓任何人在網際網路上獲得資源區域網路上的存取權時，該網路的 IP 位址會不斷。 或您可以購買靜態 IP 位址從網際網路服務提供者 (ISP) 來確保不會變更您的 IP 位址。  
  
###  <a name="BKMK_ChooseDomainName"></a>選擇網域名稱  
 選擇辨識您的業務伺服器的名稱。 例如，如果您的公司名稱 Contoso Ltd，您可以選擇 Contoso 唯一找出您的家用或企業伺服器上。 如果無法使用的網域名稱，請嘗試另一個名稱，或可能東西完全不同的變化。  
  
 輸入的名稱可以包含下列動作：  
  
-   最大 63 個字元  
  
-   （英文或您當地語系化的字元）的字母、數字或連字號（-）。 名稱必須開始與結束字母與數字。  
  
    > [!NOTE]
    >  並非區分大小寫網域名稱。  
  
###  <a name="BKMK_Prefixes"></a>選擇網域名稱前置詞  
 網域名稱包含階層標籤。  
  
 **最上層網域擴充功能**的最右邊的標籤，網域名稱。 例如，在 www.contoso.com，com 是最上層的網域名稱擴充功能。  
  
 **第二層級網域名稱**的最上層的網域名稱副檔名旁的標籤。 通常建立根據公司名稱，你或服務的第二層級網域名稱。 例如，在 www.contoso.com，contoso 是第二層級網域名稱，獲選公司名稱 Contoso 醫藥。 第二層級網域有時稱為主機，關聯的 IP 位址。  
  
 **網域名稱前置詞**辨識子。 找出服務、裝置或地區都可子網域名稱。 例如 Contoso 醫藥想要讓遠端使用者來登入遠端網路存取，但不是想讓網站將在公開發行，可讓他們建立只允許使用者的適當權限存取網站子。 將 remote.contoso.com 子，以 Contoso 醫藥設定，而且遠端的網域名稱前置詞。  
  
> [!TIP]
>  我們建議使用預設的**遠端**為您的網域名稱。  
  
###  <a name="BKMK_Extension"></a>選擇的網域名稱延伸模組  
 當您選擇您的網際網路網站的網域名稱時，您也需要指定您要使用的網域名稱擴充功能。 依照任何網域名稱句點字母，可擴充功能。 （正式字詞的擴充功能是 TLD 的最上層網域）。  
  
 有兩個主要網域擴充功能可供您的類型：一般和國家代碼。  
  
#### <a name="generic-top-level-domains"></a>一般的最上層網域  
 一般網域擴充功能是三個或更多的字母長度，且通常會由特定類型的組織。  
  
 **一般的最上層網域的範例**  
  
|網域擴充功能|描述|  
|----------------------|-----------------|  
|.com|通常會由 commercial 組織，但它可以使用的任何人。|  
|.net|針對公司所提供的網路基礎結構服務。|  
|.org|原本是非營利代理商與其他公司落不到另一個一般的最上層網域分類所使用。 任何人都可以使用。|  
|.edu|使用限制教育組織。|  
  
#### <a name="country-code-top-level-domains"></a>國家代碼最上層的網域  
 這些網域擴充功能有兩個字母的長度。 它們是設計用來的國家或地區使用該代碼的相關聯的組織使用。 部分國家代碼最上層的網域的使用，該國家 / 地區的限制。 有些則可供使用的任何人。  
  
 **最上層的國家代碼網域範例**  
  
|網域擴充功能|描述|  
|----------------------|-----------------|  
|編碼可能為.ca|使用的網站，加拿大|  
|.cn|適用於中國的網站來使用|  
|.de|使用的網站，德國|  
|。co.uk|英國的網站來使用|  
  
 若要檢視的最上層的網域的完整清單，請查看[網站網際網路受指派的數字授權單位](https://go.microsoft.com/fwlink/?LinkId=117438)。  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>如果無法使用以選取 [設定網域名稱精靈中的網域延伸模組  
 當您執行設定網域名稱精靈時，精靈會檢查您的系統資訊，以判斷您的國家或地區。 精靈會再顯示當地的參與提供者支援網域擴充。 如果您想要的網域擴充功能不會出現在清單中，您必須選擇不同的網域擴充功能，以繼續。 精靈會傳回清單中選取擴充功能。  
  
###  <a name="BKMK_UpdateService"></a>更新或升級您的網域名稱服務  
 您可能需要更新或升級您的網域名稱服務，如果您購買的網域名稱，但不是購買的憑證。 您必須憑證您的網域名稱從您的網域名稱服務提供者。  
  
> [!NOTE]
>  使用您網域名稱服務提供者以判斷您需要的憑證類型。 憑證可以提供便宜的憑證。 不過，您應該檢視的文件和若要判斷是否變得更好符合您的企業需要更高版本層級的安全性憑證中的功能。  
  
###  <a name="BKMK_ExportCert"></a>匯出或匯入您伺服器上的憑證  
 如果您想要建立的備份的憑證，或使用它在另一部伺服器，您必須匯出憑證。 如需有關匯出憑證的資訊，請查看[匯出憑證](https://go.microsoft.com/fwlink/p/?LinkId=214362)。  
  
###  <a name="BKMK_SetNameManually"></a>手動設定的網域名稱  
 如果您選擇此選項，伺服器不監視或維持您的網域名稱和它不會不會通知您是否有設定的問題。 您也可以考慮此選項如果下列其中一項：  
  
-   不合作夥伴網域名稱提供者會列出您的國家或地區。  
  
-   中列出的網域協力廠商提供者不支援您的網域名稱擴充功能。  
  
-   您的網域名稱提供者目前不是合作夥伴，從現有的網域名稱，並不想要傳送的網域名稱與 Windows Server Essentials 支援的網域名稱提供者。  
  
-   精靈未列出您想要使用的網域名稱擴充功能，但擴充功能可從目前不是合作夥伴的網域名稱提供者。  
  
 如果您選擇手動設定您的網域名稱，用來建立您的網域 A 記錄您網域名稱服務提供者。  
  
##### <a name="to-create-an-a-record"></a>若要建立 A 記錄  
  
1.  選擇主應用程式名稱，例如遠端。 這是網域名稱前置詞。 網域名稱前置詞加上您的網域名稱將會定義 URL 打開遠端 Web 存取登入頁面。例如，**http://remote.contoso.com**。  
  
2.  網域名稱服務提供者組態儀表板（通常是在其網頁），在建立 A 記錄主機，而且在步驟 1 名稱。 請確定您 A 記錄指定 IP 位址 WAN 側邊（網際網路面對側邊）您的路由器上的 IP 位址。 請洽詢您的路由器的文件以尋找您的 WAN IP 位址。  
  
3.  建議您連絡網際網路服務提供者 (ISP) 購買靜態 IP 位址，您的網路。 這樣可確保的 IP 位址不會變更，以及您 DNS 項目不會變成過時。  
  
     如果您無法從您的 ISP 取得靜態 IP 位址] 選項，您也可以考慮網域名稱系統」(DNS) 動態更新通訊協定服務購買從您的網域名稱服務提供者或其他服務提供者。 DNS 動態更新通訊協定是一種服務，可以保留您的網路 WAN IP 位址最新狀態，即使您的 IP 位址變更的 IP 位址可以是您的網域名稱解析。  
  
4.  匯入精靈會提示您信任的憑證。 如果您不信任的憑證，您可以取得，其中一個支援的網域名稱提供者精靈中列出的其中一個或購買來自信任您所選擇的提供者。 如需受信任的憑證的詳細資訊，請連絡您的網域名稱提供者。  
  
###  <a name="BKMK_Find"></a>尋找您的網域名稱服務提供者  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>若要尋找您的網域名稱網域名稱服務提供者  
  
1.  打開網頁瀏覽器，然後輸入**www.internic.com**來移至網際網路首頁的網址列中。  
  
2.  在首頁網際網路上，按一下 [ **Whois**。  
  
3.  在**Whois**方塊中，輸入您的網域名稱 (例如 contoso.com)。  
  
4.  按一下**網域**選項，然後按一下 [**提交**。  
  
5.  在搜尋結果中，您的網域名稱服務提供者的名稱會列在**登錄器**。  
  
##  <a name="BKMK_4"></a>自訂遠端存取  
 您可以自訂遠端 Web 存取網站加個人商標或背景影像。 您也可以新增 [首頁] 頁面上的連結，這項資訊會提供給所有使用者的。 如需詳細資訊，下列主題：  
  
-   [自訂遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [自訂背景和標誌映像](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [修復遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>自訂遠端存取  
 您可以自訂遠端 Web 存取藉由變更網站的標題、變更背景影像和商標，並新增至其他網站上的 [首頁] 頁面的連結。  
  
##### <a name="to-customize-remote-web-access"></a>若要自訂遠端 Web 存取  
  
1.  打開儀表板。  
  
2.  按一下**設定**，然後按一下 [**隨處存取**索引標籤。  
  
3.  在**網站設定**區段中，按**自訂]**。  
  
4.  當您自訂遠端網路存取權，請按一下**[確定]**。 測試您的變更，在網頁上的存取。  
  
###  <a name="BKMK_CustomizeImages"></a>自訂背景和標誌映像  
 本節映像，您可以使用來自訂遠端網站存取的資訊。  
  
#### <a name="image-size"></a>影像大小  
 **商標映像**  
  
 建議您使用的是 32 x 32 個像素商標映像。 大型影像的壓縮到 32 x 32 和延伸 32 x 32，可能會變形的小型影像。  
  
 **背景影像**  
  
 雖然不的大小上限背景影像，最好建議使用約 800 x 500 像素的影像。 背景影像位於登入頁面的中心（水平和垂直）。 為了讓登入頁面上的文字朗讀輕鬆的背景影像中心應該彩色燈號。  
  
#### <a name="image-file-types"></a>影像檔案類型  
 下列影像檔案類型，可以用於取代預設背景和網站商標：  
  
-   點陣圖 *.bmp、\*.dib（\*.rle）  
  
-   GIF（位置）  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a>修復遠端存取  
 修復精靈可協助您偵測及修正的相關問題與您的路由器或網域名稱。 有兩種遠端 Web Access 與找出問題：  
  
-   中設定伺服器儀表板上的任何地方存取索引標籤，以及問題的描述紅色 x 會顯示的圖示。  
  
-   通知的警示檢視器中。  
  
> [!NOTE]
>  您將在網頁上的存取，才可以使用修復精靈。 打開遠端網站存取的相關資訊，請查看[上遠端 Web 存取關閉](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。  
  
##### <a name="to-repair-remote-web-access"></a>若要修復遠端 Web 存取  
  
1.  登入儀表板。  
  
2.  按一下**設定**，然後按一下 [**隨處存取**索引標籤。  
  
3.  按一下**修復**。 **修復遠端 Web 存取**精靈開始。  
  
4.  按一下**下一步**。 精靈會分析網頁上的存取、辨識的問題，並嘗試修復該問題。  
  
5.  如果您收到通知精靈完成時，您可以按一下**再試一次**來再試一次修復此問題。 如果您繼續接收的警示，查看其他資訊的問題和疑難排解步驟的警示。  
  
##  <a name="BKMK_5"></a>疑難排解遠端存取  
  
-   [疑難排解遠端 Web 存取連接](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [疑難排解防火牆](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [疑難排解隨時隨地存取](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>也了  
  
-   [遠端桌面選項](Remote-desktop-options.md)  
  
-   [使用遠端存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理隨時隨地存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
