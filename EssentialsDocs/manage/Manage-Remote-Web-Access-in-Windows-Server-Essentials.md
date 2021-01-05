---
title: 在 Windows Server Essentials 中管理遠端 Web 存取
description: 瞭解如何使用「設定隨處存取嚮導」來開啟遠端 Web 存取，然後瞭解如何設定您的路由器和功能變數名稱。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a9d83d12dc34b9b540f9f10ea677690726347766
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811295"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理遠端 Web 存取

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 Windows Server Essentials 中的遠端 Web 存取，或已安裝 Windows Server Essentials 體驗角色的 Windows Server 2012 R2 中的遠端，可提供簡化且便於使用的瀏覽器體驗，讓您幾乎可以使用任何裝置從任何地方存取應用程式和資料。 若要使用遠端 Web 存取功能，您必須先使用 [設定隨處存取精靈] 開啟該功能，然後設定您的路由器和網域名稱。

## <a name="in-this-topic"></a>本主題內容

-   [開啟和設定遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)

-   [設定路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)

-   [設定您的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)

-   [自訂遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)

-   [疑難排解遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)

##  <a name="turn-on-and-configure-remote-web-access"></a><a name="BKMK_1"></a> 開啟並設定遠端 Web 存取
 下列主題將協助您開啟和設定遠端 Web 存取：

-   [遠端 Web 存取概觀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)

-   [開啟遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)

-   [變更您的地區](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)

-   [管理遠端 Web 存取許可權](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)

-   [保護遠端 Web 存取的安全](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)

-   [管理遠端 Web 存取和 VPN 使用者](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)

###  <a name="remote-web-access-overview"></a><a name="BKMK_Overview"></a> 遠端 Web 存取總覽
 當您離開辦公室時，您可以開啟網頁瀏覽器，並從可存取網際網路的任何位置存取遠端 Web 存取。 在遠端 Web 存取中，您可以：

- 存取伺服器上的共用檔案和資料夾。

- 存取您在網路上的伺服器和電腦。 這表示您可以存取連線到網路之電腦的桌面，就好像您置身於辦公室中坐在電腦前一樣。

  遠端 Web 存取預設不會開啟。 當您執行 [設定隨處存取精靈] 時，精靈會嘗試設定您的路由器和網際網路連線能力。 開啟遠端 Web 存取之後，您可以設定伺服器的功能變數名稱，並自訂遠端 Web 存取。 如果您變更路由器，您也可以再設定一次路由器。

  當您新增使用者帳戶時，不會自動授與存取遠端 Web 存取的許可權。 當您新增使用者帳戶時，您可以選擇允許存取共用資料夾、媒體櫃、電腦、首頁連結，以及伺服器 [儀表板]。 您也可以指定不允許使用者使用遠端 Web 存取。

  在 [Windows Server Essentials 儀表板] 的 [ **使用者** ] 索引標籤上，會顯示每個使用者帳戶的 [遠端 Web 存取] 設定。 若要變更遠端 Web 存取設定，請在使用者帳戶上按一下滑鼠右鍵，然後按一下 **[查看帳戶屬性**]。

###  <a name="turn-on-remote-web-access"></a><a name="BKMK_TurnOnRWA"></a> 開啟遠端 Web 存取
 您可以從伺服器儀表板執行 [設定隨處存取精靈] 開啟遠端 Web 存取。

##### <a name="to-turn-on-remote-web-access"></a>開啟遠端 Web 存取

1.  開啟 [儀表板]。

2.  按一下 [設定]，然後按一下 [隨處存取] 索引標籤。

3.  按一下 [設定]  。 此時會出現 [設定隨處存取精靈]。

4.  在 [選擇要啟用的隨處存取功能] 頁面上，選取 [遠端 Web 存取] 核取方塊。

5.  遵循指示以完成精靈。

###  <a name="change-your-region"></a><a name="BKMK_Region"></a> 變更您的區域
 您必須是網路系統管理員，才能在 Windows Server Essentials 中變更地區設定。

##### <a name="to-change-the-region-setting"></a>變更地區設定

1.  在連線到 Windows Server Essentials 的電腦上，開啟 [儀表板]。

2.  按一下 [設定]。

3.  在 [一般] 索引標籤上，按一下 [伺服器的國家/地區位置] 區段中的下拉式清單。

4.  從下拉式清單中選取新的地區，然後按一下 [套用] 來接受新的地區設定。

###  <a name="manage-remote-web-access-permissions"></a><a name="BKMK_ManagePerms"></a> 管理遠端 Web 存取許可權
 當您在 Windows Server Essentials 中新增使用者帳戶時，預設會允許新的使用者使用「遠端 Web 存取」。 如果您選擇不允許使用者帳戶的遠端 Web 存取，然後發現使用者需要使用遠端 Web 存取，您可以更新使用者帳戶的屬性。

##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>管理使用者帳戶的遠端 Web 存取權限

1. 登入 [儀表板]，然後按一下 [使用者]。

2. 按一下您想要管理的使用者帳戶，然後按一下 [工作] 窗格中的 [檢視帳戶內容] 。

3. 在 [內容] 對話方塊中，按一下 [隨處存取] 索引標籤。

4. 在 [隨處存取] 索引標籤上，選取 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取] 核取方塊，以允許使用者使用「遠端 Web 存取」來連線到伺服器。

5. 按一下 [套用]，然後按一下 [確定]。

   如需詳細資訊，請參閱 [管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)。

###  <a name="secure-remote-web-access"></a><a name="BKMK_SecureRWA"></a> 安全的遠端 Web 存取
 Windows Server Essentials 使用安全性憑證來協助保護軟體與網頁瀏覽器之間交換的資訊。 當您在電腦上安裝連線程式軟體時，Windows Server Essentials 的安全性憑證會新增到您電腦上的受信任憑證清單中。 當使用者不在您辦公室時，讓他們存取「遠端 Web 存取」的最佳方式，就是使用已安裝連接程式軟體的可攜式電腦。

> [!WARNING]
>  使用者如果是從公共場所或其他不受信任的電腦使用「遠端 Web 存取」，應該確保在離開電腦或已完成其工作階段時登出網站。

###  <a name="manage-remote-web-access-and-vpn-users"></a><a name="BKMK_ManageRWAVPN"></a> 管理遠端 Web 存取和 VPN 使用者
 您可以使用 VPN 來連線到 Windows Server Essentials，並存取您所有儲存在伺服器上的資源。 當您有已設定網路帳戶的用戶端電腦且那些帳戶可透過 VPN 連線來連線到託管的 Windows Server Essentials 伺服器時，這會特別有用。 所有在託管的 Windows Server Essentials 伺服器上新建立的使用者帳戶，在第一次登入用戶端電腦時都必須使用 VPN。

##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>設定網路使用者的 VPN 和遠端 Web 存取權限

1.  開啟 [儀表板]。

2.  在瀏覽列上，按一下 [使用者]。

3.  在使用者帳戶清單中，選取您想要授與桌面遠端存取權限的使用者帳戶。

4.  在 [ **<使用者帳戶 \>** 工作] 窗格中 **，按一下 [** 內容]。

5.  在 **<使用者帳戶 \>** 內容] 中，按一下 [ **隨處存取** ] 索引標籤。

6.  在 [隨處存取] 索引標籤上，執行下列動作：

    1.  若要允許使用者使用 VPN 來連線到伺服器，請選取 [允許虛擬私人網路 (VPN)] 核取方塊。

    2.  若要允許使用者使用「遠端 Web 存取」來連線到伺服器，請選取 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取] 核取方塊。

7.  按一下 [套用]，然後按一下 [確定]。

##  <a name="set-up-your-router"></a><a name="BKMK_2"></a> 設定您的路由器
 當您設定伺服器來進行「遠端 Web 存取」時，[設定隨處存取精靈] 會嘗試設定路由器。 如果您變更路由器或變更路由器上的設定，必須重新執行 [設定您的路由器精靈]。 如需詳細資訊，請參閱下列主題：

-   [設定路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)

-   [更換路由器](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)

-   [定義的網路位置](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)

-   [啟用遠端桌面服務 ActiveX 控制項](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)

###  <a name="set-up-your-router"></a><a name="BKMK_SetUpRouter"></a> 設定您的路由器
 在這個步驟中， Windows Server Essentials 會使用 UPnP 命令嘗試自動設定您的路由器。 若要這樣做，路由器必須支援 UPnP 標準，而且必須在路由器上啟用 UPnP 設定。

> [!NOTE]
> 您的網路設定應該要符合 Windows Server Essentials 支援的網路需求。 您的網路上應該只有一個路由器。

 如果路由器不是透過 [設定您的網域名稱精靈] 進行設定，您必須手動轉送連接埠 443。 如需有關如何在路由器上設定埠轉送的詳細資訊，請參閱 [Small Business Server 論壇](/answers/topics/windows-small-business-server.html)。

###  <a name="replace-a-router"></a><a name="BKMK_ReplaceRouter"></a> 更換路由器
 根據製造商的指示來更換路由器，然後執行 [設定您的路由器]，以設定新的路由器。

##### <a name="to-set-up-your-new-router"></a>設定新路由器

1.  在 [Windows Server Essentials 儀表板] 上，按一下 [設定]。

2.  按一下 [隨處存取] 索引標籤，然後在 [路由器] 區段中，按一下 [設定]。 [設定您的路由器精靈] 隨即啟動。

3.  依照精靈中的指示來完成新路由器的設定。

###  <a name="network-location-defined"></a><a name="BKMK_NetworkLocation"></a> 定義的網路位置
 網路位置是當您連線到網路時，Windows 會套用的網路設定集合。 設定會依據您使用的網路類型而有所不同並且可以自訂。 網路位置的設定可決定是否要開啟或關閉特定功能 (例如檔案和印表機共用、網路探索，以及公用資料夾共用)。 當您需要連線到不同的網路時，網路位置相當有用。

 例如，您可能擁有一部在家及在工作上使用的膝上型電腦。 當您在辦公室時，您會連線到辦公室網路。 不過，當您回到家時，則會使用膝上型電腦來存取及播放儲存在家用伺服器上的影片和音樂。 當您連線到新網路並指定位置類型時，Windows 會指派一個為該類型的位置預設的網路設定檔。 下次您連線到該網路時，Windows 會認出該網路並自動指派正確的設定。 這會增添一層安全性來協助保護您的電腦上的資訊，並且只會開啟您就該位置所需的網路功能。

 網路位置有四種：

-   **家用網路** 如果是家用網路，或您認識並信任網路上的人員和裝置時，請選擇這種網路。 家用網路上的電腦可以屬於家用群組。 家用網路有開啟網路探索，這可讓您看見網路上的其他電腦和裝置，並讓其他網路使用者看見您的電腦。

-   **工作場所網路** 如果是小型辦公室或其他工作場所網路，請選擇這種網路。 預設會開啟網路探索，這可讓您看見網路上的其他電腦和裝置，並讓其他網路使用者看見您的電腦，但是您無法建立或加入家用群組。

-   **公用網路** 如果是公共場所 (例如咖啡館或機場)，請選擇這種網路。 這個位置的設計目的是要讓其他電腦無法看見您的電腦，並協助保護您的電腦免於受到來自網際網路的惡意軟體侵害。 公用網路上無法使用家用群組，並且已關閉網路探索。 如果您是直接連線到網際網路而沒有使用路由器，或是有行動式寬頻連線，則也應該選擇這個選項。

-   **網域** 如果是企業工作場所之類的網域，請選擇這種網路。 這種類型的網路位置是由您的網路系統管理員所控制，無法被選取或變更。

###  <a name="enable-remote-desktop-services-activex-controls"></a><a name="BKMK_ActiveX"></a> 啟用遠端桌面服務 ActiveX 控制項
 遠端桌面服務的 ActiveX 控制項可讓您使用遠端 Web 存取，從另一部電腦存取您的家用或公司電腦（透過網際網路）。

##### <a name="to-enable-remote-desktop-services-activex-controls"></a>啟用遠端桌面服務 ActiveX 控制項

1.  在 Internet Explorer 中，按一下 [工具]，然後按一下 [網際網路選項]。

2.  在 [安全性] 索引標籤上，按一下 [自訂層級]。

3.  在 [ActiveX 控制項與外掛程式] 區段中，執行下列動作：

    1.  在 [下載已簽署的 ActiveX 控制項] 底下，按一下 [提示]。

    2.  在 [執行 ActiveX 控制項與外掛程式] 底下，按一下 [啟用]。

4.  按一下 [確定] 兩次來接受變更並關閉對話方塊。

##  <a name="set-up-your-domain-name"></a><a name="BKMK_3"></a> 設定您的功能變數名稱
 開啟「遠端 Web 存取」之後，您便可以為執行 Windows Server Essentials 的伺服器設定網域名稱。 如果您打算從遠端電腦使用遠端 Web 存取，這是必要步驟。 如需詳細資訊，請參閱下列主題：

-   [網域名稱概觀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)

-   [了解 Microsoft 個人化的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)

-   [使用新的或現有的網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)

-   [設定網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)

-   [選擇網域名稱服務提供者](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)

-   [選擇網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)

-   [選擇網域名稱前置碼](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)

-   [選擇網域名稱延伸](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)

-   [更新或升級您的網域名稱服務](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)

-   [匯出或匯入您伺服器上的憑證](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)

-   [手動設定網域名稱](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)

-   [尋找您的網域名稱服務提供者](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)

###  <a name="domain-names-overview"></a><a name="BKMK_DNOverview"></a> 功能變數名稱總覽
 網域名稱可唯一識別您在網際網路上的伺服器。 網域名稱至少包含兩個部分：頂層網域名稱 (TLD) 和次層網域名稱。 例如，在 contoso.com 中，com 是 TLD，而 contoso 是第二層的功能變數名稱。

 當您不在辦公室時，您可以使用您的網域名稱來存取伺服器上的共用檔案或網路上的電腦。 您也可以在外出時管理您的伺服器。 例如，您可以為您的伺服器登錄 contoso.com。 當您不在辦公室時，您可以開啟您膝上型電腦上的網頁瀏覽器，然後在網址文字方塊中輸入 **contoso.com**，以連線到您在 Windows Server Essentials 上設定的「遠端 Web 存取」執行個體。

###  <a name="understand-microsoft-personalized-domain-names"></a><a name="BKMK_PersonalizedNames"></a> 瞭解 Microsoft 個人化功能變數名稱
 Microsoft 個人化網域名稱包含下列特色：

- 遠端 Web 存取的自訂功能變數名稱 (例如， *yourhostname*. remotewebaccess.com) 。 您的網域名稱會與公用 IP 位址關聯。

- DNS 動態更新通訊協定服務，如此一來，如果您的公用 IP 位址變更，使用您的功能變數名稱的遠端 Web 存取將不會中斷。 一般而言，網際網路服務提供者針對您組織的寬頻連線 (Isp) 提供可變更的動態公用 IP 位址。

- 一個與網域名稱關聯的受信任憑證。

  若要將 Microsoft 個人化網域名稱與您的伺服器整合，您需要有 Microsoft 帳戶 (先前稱為 Windows Live ID)。 如果您沒有 Microsoft 帳戶，您可以在 [Microsoft Hotmail](https://login.live.com/) 網站申請一個帳戶。

> [!IMPORTANT]
>  Windows Live 允許您在 Microsoft 帳戶密碼中使用伺服器不支援的特殊字元。 如果您使用 Microsoft 個人化網域，請確定您的 Microsoft 帳戶密碼只包含伺服器支援的字元。 伺服器不支援使用 $、/、' 及 % 字元。

###  <a name="use-a-new-or-existing-domain-name"></a><a name="BKMK_UseNewName"></a> 使用新的或現有的功能變數名稱
 若要在執行 Windows Server Essentials 的伺服器上自動設定您的網域名稱，您必須使用 [設定您的網域名稱精靈] 中所列的網域名稱服務提供者。 您可以選擇取得新網域名稱或使用現有的網域名稱。 執行下列其中一個動作：

-   如果您想要從精靈所列的其中一個網域名稱服務提供者取得新的網域名稱，請按一下 [我要設定新的網域名稱]。

-   如果您有向其中一個支援的網域名稱服務提供者購買的現有網域名稱，您可以使用 [設定您的網域名稱精靈] 來設定您伺服器的網域名稱。 請按一下 [我要使用我已有的網域名稱]，然後在 [設定您的網域名稱] 文字方塊中輸入網域名稱。 您必須提供您用來購買網域名稱的使用者名稱和密碼。

-   如果您有向 Windows Server Essentials 不支援的網域名稱服務提供者購買的現有網域名稱，而您想要使用 [設定您的網域名稱精靈] 來設定您伺服器的網域名稱，您可以將該網域名稱轉移給精靈所列的其中一個網域名稱服務提供者。 按一下 [ **我要使用我已有的功能變數名稱**]，在 [ **功能變數名稱** ] 文字方塊中輸入功能變數名稱，然後依照功能變數名稱服務提供者網站上的指示來傳送功能變數名稱。

###  <a name="set-up-a-domain-name"></a><a name="BKMK_SetUpName"></a> 設定功能變數名稱
 當您開啟「遠端 Web 存取」時，您可以選擇設定伺服器的網際網路網域名稱。

##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>設定或管理網際網路網域名稱

1.  開啟 [儀表板]。

2.  按一下 [伺服器設定]，然後按一下 [隨處存取] 索引標籤。

3.  在 [網域名稱] 區段中，按一下 [設定]。

4.  遵循指示以完成精靈。 如果您尚未擁有網域名稱和憑證，精靈會協助您尋找網域名稱提供者來購買網域名稱和憑證，或者您也可以取得個人化的 Microsoft 網域名稱。

###  <a name="choose-a-domain-name-service-provider"></a><a name="BKMK_ChooseProvider"></a> 選擇功能變數名稱服務提供者
 您應該選擇支援您想要使用之網域名稱延伸的網域名稱服務提供者。 [設定您的功能變數名稱] Wizard 包含一份合格的提供者清單，您可以將這些提供者與每個提供者網站的連結一起使用。 按一下每個提供者名稱旁的 [ **其他資訊** ] 連結，以取得提供者所提供之服務和價格的相關資訊。

> [!NOTE]
>  有些網域名稱服務提供者服務廣泛的國際地區，有些則服務較小的市場。 因此，有些提供者可能不提供轉譯成您慣用語言的網站。

 當您購買網域名稱時，您可能也會考慮向您的網域名稱服務提供者購買網域名稱系統 (DNS) 動態更新通訊協定服務。 DNS 動態更新通訊協定是一項服務，可讓網際網路上的任何人在即使網路 IP 位址不斷變更的情況下，仍能存取本機網路上的資源。 或者，您也可以向您的網際網路服務提供者 (ISP) 購買靜態的 IP 位址，以確保您的 IP 位址不會變更。

###  <a name="choose-a-domain-name"></a><a name="BKMK_ChooseDomainName"></a> 選擇功能變數名稱
 選擇可唯一識別您商用伺服器的名稱。 例如，如果您的公司名稱是 Contoso Ltd.，您可能會選擇 Contoso 來唯一識別您在網際網路上的家用或商用伺服器。 如果無法使用該網域名稱，請嘗試該名稱的另一種變異，或嘗試完全不同的名稱。

 您輸入的名稱可以包含下列內容：

-   最多 63 個字元

-   字母 (英文或您當地語系化的字元)、數字或連字號 (-)。 名稱的開頭和結尾必須是字母或數字。

    > [!NOTE]
    >  網域名稱並不區分大小寫。

###  <a name="choose-a-domain-name-prefix"></a><a name="BKMK_Prefixes"></a> 選擇功能變數名稱首碼
 網域名稱是由階層式標籤所組成。

 **頂層網域延伸** 是網域名稱中最右邊的標籤。 例如，在 www \. contoso.com 中，com 是最上層的功能變數名稱延伸。

 **次層網域名稱** 是頂層網域名稱延伸旁的標籤。 次層網域名稱通常是根據公司名稱、產品或服務來建立的。 例如，在 www \. contoso.com 中，contoso 是第二層的功能變數名稱，而且是針對公司名稱 Contoso 製藥選擇的。 次層網域有時也稱為主機名稱，它會有一個關聯的 IP 位址。

 **網域名稱前置碼** 可識別子網域。 子網域名稱可用來識別服務、裝置或地區。 例如，Contoso Pharmaceuticals 想要讓遠端使用者可以登入「遠端 Web 存取」，但是不想要將網站提供給大眾使用，因此它們會建立一個只允許具有適當權限之使用者存取網站的子網域。 Contoso Pharmaceuticals 會設定 remote.contoso.com 做為子網域，而 remote 就是網域名稱前置碼。

> [!TIP]
>  建議您使用預設的 **Remote** 做為您網域名稱的前置碼。

###  <a name="choose-a-domain-name-extension"></a><a name="BKMK_Extension"></a> 選擇功能變數名稱擴充功能
 當您為網際網路網站選擇網域名稱時，您也需要指定您想要使用的網域名稱延伸。 延伸是由任何網域名稱最後一個句點後面的字母來識別。  (延伸模組的正式詞彙是最上層的網域或 TLD。 ) 

 您可以使用的網域延伸主要有兩種類型：一般和國碼 (地區碼)。

#### <a name="generic-top-level-domains"></a>一般頂層網域
 一般網域延伸的長度為三個或更多個字母，而且通常是供特定類型的組織使用。

 **一般頂層網域的範例**

|網域延伸|描述|
|----------------------|-----------------|
|.com|通常是供商業組織使用，但也可供任何人使用。|
|.net|專為提供網路基礎結構服務的企業而設計。|
|.org|原先是供非營利機構及其他不屬於另一個一般頂層網域類別的企業使用。 可供任何人使用。|
|.edu|僅限教育性組織使用。|

#### <a name="country-code-top-level-domains"></a>國碼 (地區碼) 頂層網域
 這些網域延伸的長度為兩個字母。 它們是為了供與該代碼關聯之國家或地區中的組織使用而設計。 有些國碼 (地區碼) 頂層網域僅限該國家或地區的公民使用。 其他則可供任何人使用。

 **國碼 (地區碼) 頂層網域的範例**

|網域延伸|描述|
|----------------------|-----------------|
|.ca|供位於加拿大的網站使用|
|.cn|供位於中國的網站使用|
|.de|供位於德國的網站使用|
|.co.uk|供位於英國的網站使用|

 若要檢視頂層網域的完整清單，請參閱 [網際網路指定編號機構網站](https://go.microsoft.com/fwlink/?LinkId=117438)。

#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>如果 [設定網域名稱精靈] 中沒有某個網域延伸可供選取
 當您執行 [設定網域名稱精靈] 時，精靈會查看您的系統資訊來判斷您的國家或地區。 然後，精靈會只顯示您地區中的參與提供者支援的網域延伸。 如果您想要的網域延伸並未出現在清單中，您就必須選擇不同的網域延伸才能繼續。 從精靈所傳回的清單中選取一個延伸。

###  <a name="update-or-upgrade-your-domain-name-service"></a><a name="BKMK_UpdateService"></a> 更新或升級您的功能變數名稱服務
 如果您購買了網域名稱，但未購買憑證，您就可能需要更新或升級您的網域名稱服務。 您必須從網域名稱服務提供者取得您網域名稱的憑證。

> [!NOTE]
>  請與您的網域名稱服務提供者合作來判斷您所需的憑證類型。 憑證可以是所提供的其中一種便宜憑證。 不過，您應該檢閱較高層級安全性憑證的文件和功能，以判斷其是否較符合您的業務需求。

###  <a name="export-or-import-your-certificate-on-your-server"></a><a name="BKMK_ExportCert"></a> 在您的伺服器上匯出或匯入憑證
 如果您想要建立一份憑證備份，或在另一部伺服器上使用它，您就必須匯出憑證。 如需有關匯出憑證的資訊，請參閱 [匯出憑證](https://go.microsoft.com/fwlink/p/?LinkId=214362)。

###  <a name="set-up-a-domain-name-manually"></a><a name="BKMK_SetNameManually"></a> 手動設定功能變數名稱
 如果您選擇這個選項，伺服器不會監視或維護您的網域名稱，也不會警告您是否有設定的問題。 如果符合下列任一條件，您可能也會考慮這個選項：

- 未列出您國家或地區的合作網域名稱提供者。

- 所列的合作網域提供者不支援您的網域名稱延伸。

- 您有一個從目前並非合作夥伴之網域名稱提供者取得的現有網域名稱，而且您不想要將該網域名稱轉移給 Windows Server Essentials 支援的網域名稱提供者。

- 精靈未列出您想要使用的網域名稱延伸，但目前並非合作夥伴的網域名稱提供者可提供該延伸。

  如果您選擇手動設定功能變數名稱，請與您的功能變數名稱服務提供者合作，為您的網域建立 A 記錄。

##### <a name="to-create-an-a-record"></a>若要建立 A 記錄

1.  決定主機名稱，例如 remote。 這是網域名稱首碼。 功能變數名稱前置詞加上您的功能變數名稱會定義用來開啟遠端 Web 存取登入頁面的 URL;例如， **http://remote.contoso.com** 。

2.  在您的功能變數名稱服務提供者設定儀表板中 (通常是在其網頁) 上，針對您在步驟1中決定的主機名稱建立 A 記錄。 確定您在 A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A A)  (A 請參閱您的路由器文件，找出您的 WAN IP 位址。

3.  建議向您的網際網路服務提供者 (ISP) 購買網路的靜態 IP 位址。 這可確保 IP 位址不會變更，而且您的 DNS 項目不會過時。

     如果您沒有可從 ISP 取得靜態 IP 位址的選項，您也可以考慮向您的網域名稱服務提供者或其他服務提供者，購買網域名稱系統 (DNS) 動態更新通訊協定服務。 DNS 動態更新通訊協定是一項服務，可讓您網路的 WAN IP 位址保持在最新狀態，使得即使 IP 位址發生變更，也能將 IP 位址解析成您的網域名稱。

4.  當精靈提示時，匯入受信任的憑證。 如果您沒有受信任的憑證，可以從精靈所列的其中一個支援的網域名稱提供者取得憑證，或是向您選擇的受信任提供者購買憑證。 如需受信任的憑證的詳細資訊，請連絡您的網域名稱提供者。

###  <a name="find-your-domain-name-service-provider"></a><a name="BKMK_Find"></a> 尋找您的功能變數名稱服務提供者

##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>尋找您網域名稱的網域名稱服務提供者

1. 開啟網頁瀏覽器，然後在網址列中輸入 <strong>www.internic.com</strong> ，以移至 internic 首頁。

2. 在 InterNIC 首頁上，按一下 [ **Whois**]。

3. 在 [Whois] 方塊中，輸入您的網域名稱 (例如 contoso.com)。

4. 按一下 [Domain] 選項，然後按一下 [Submit]。

5. 在搜尋結果中，您網域名稱服務提供者的名稱會列在 [Registrar] 底下。

##  <a name="customize-remote-web-access"></a><a name="BKMK_4"></a> 自訂遠端 Web 存取
 您可以新增個人標誌或背景影像來自訂您的遠端 Web 存取站台。 您也可以在首頁新增連結，讓所有使用者看到這些資訊。 如需詳細資訊，請參閱下列主題：

-   [自訂遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)

-   [自訂背景和標誌影像](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)

-   [修復遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)

###  <a name="customize-remote-web-access"></a><a name="BKMK_CustomizeRWA"></a> 自訂遠端 Web 存取
 您可以透過變更網站標題、變更背景影像和標誌，以及在首頁上新增其他網站的連結，來自訂「遠端 Web 存取」。

##### <a name="to-customize-remote-web-access"></a>自訂遠端 Web 存取

1.  開啟 [儀表板]。

2.  按一下 [設定]，然後按一下 [隨處存取] 索引標籤。

3.  在 [網站設定] 區段中，按一下 [自訂]。

4.  完成「遠端 Web 存取」自訂之後，按一下 [確定]。 測試您在「遠端 Web 存取」上的變更。

###  <a name="customize-images-for-backgrounds-and-logos"></a><a name="BKMK_CustomizeImages"></a> 自訂背景和標誌的影像
 本節提供您可用來自訂「遠端 Web 存取」的影像相關資訊。

#### <a name="image-size"></a>映像大小
 **標誌影像**

 建議您使用 32x32 像素的標誌影像。 較大的影像會壓縮成 32x32，較小的影像則會伸展成 32x32，這可能會導致影像變形。

 **背景影像**

 背景影像並無大小限制，但為獲得最佳結果，建議使用大約 800x500 像素的影像。 背景影像會在登入頁面置中 (水平和垂直)。 為了讓登入頁面上的文字容易閱讀，背景影像的中央應該採用淺色。

#### <a name="image-file-types"></a>影像檔類型
 下列影像檔類型可用來取代預設背景和網站標誌：

-   Bitmap ( * .bmp、 \* .dib、 \* rle) 

-   GIF (*.gif)

-   PNG (*.png)

-   JPG (*.jpg)

###  <a name="repair-remote-web-access"></a><a name="BKMK_RepairRWA"></a> 修復遠端 Web 存取
 [修復精靈] 可以協助您偵測和解決路由器或網域名稱的問題。 有兩種方法可以找出「遠端 Web 存取」問題：

-   在 [儀表板] 上的 [伺服器設定] 中，於 [隨處存取] 索引標籤上，會顯示帶有紅色 X 及問題描述的圖示。

-   [警示檢視器] 中的警示。

> [!NOTE]
>  在您開啟「遠端 Web 存取」之後，才能使用 [修復精靈]。 如需有關開啟「遠端 Web 存取」的資訊，請參閱[開啟遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。

##### <a name="to-repair-remote-web-access"></a>修復遠端 Web 存取

1.  登入 [儀表板]。

2.  按一下 [設定]，然後按一下 [隨處存取] 索引標籤。

3.  按一下 [修復]。 [修復遠端 Web 存取] 精靈隨即啟動。

4.  按一下 [下一步] 。 精靈會分析「遠端 Web 存取」、識別問題所在，然後嘗試修復該問題。

5.  如果您在精靈完成時收到警示，您可以按一下 [重試] 來重新嘗試修復問題。 如果您持續收到警示，請查看警示以了解問題和疑難排解步驟的其他相關資訊。

##  <a name="troubleshoot-remote-web-access"></a><a name="BKMK_5"></a> 遠端 Web 存取疑難排解

-   [遠端 Web 存取連線問題疑難排解](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)

-   [疑難排解您的防火牆](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)

-   [疑難排解隨處存取](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)

## <a name="additional-references"></a>其他參考資料

-   [遠端桌面選項](Remote-desktop-options.md)

-   [使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [管理隨處存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)