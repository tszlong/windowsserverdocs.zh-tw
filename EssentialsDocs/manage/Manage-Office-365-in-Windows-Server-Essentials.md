---
title: 在 Windows Server Essentials 中管理 Office 365
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: bd7787b853395bf461165802251de8b2be5d5a39
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980264"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理 Office 365

>適用於：Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

當您將 Windows Server Essentials 伺服器與 Microsoft Office 365 整合時, 您可以從 Windows Server Essentials 儀表板管理您的 Office 365 服務和線上帳戶, 以及內部部署資源。 在本主題中, 您將瞭解如何整合您的伺服器與 Office 365、如何進行, 以及如何管理和疑難排解您的 Office 365 整合。  
  
  
  
> [!IMPORTANT]
>   只有單一網域控制站的環境才支援 Office 365 整合。 此外, Office 365 整合嚮導必須在網域控制站上執行。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [為什麼我應該將 Office 365 與「我的伺服器」整合？](#BKMK_IntegrationOverview)  
  
-   [設定 Office 365 整合](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [管理 Office 365 整合](#BKMK_ManageIntegration)  
  
-   [針對 Office 365 整合進行疑難排解](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>為什麼我應該將 Office 365 與「我的伺服器」整合？  
 整合 Office 365 與您的 Windows Server Essentials 伺服器有許多很好的理由。 如果您管理某些內部資源, 但使用 Office 365 做為其他服務, 您可以從儀表板管理您的 Office 365 服務和資源, 以及您的內部部署資源, 而不是在兩個地方工作。  
  
- 管理線上帳戶, 讓您的使用者能夠存取 Office 365 和您的使用者帳戶:  
  
  -   在單一步驟中為您的使用者建立 Microsoft Online Services 帳戶，或是在伺服器上為您現有的線上帳戶建立使用者帳戶。 您也可以將線上帳戶新增到新的或現有的使用者帳戶。  
  
       當您從儀表板建立線上帳戶時, 使用者會使用他們在伺服器上所使用的相同密碼來登入 Office 365。 如果他們變更了使用者帳戶的密碼，線上密碼也會變更。 這樣您就知道他們的線上帳戶密碼，永遠符合您為使用者帳戶設定的安全性需求。  
  
  -   在整個使用者帳戶生命週期裡管理線上帳戶以及使用者帳戶。 如果您停用使用者帳戶，線上帳戶也會停用。 如果您移除使用者帳戶，線上帳戶也會移除。  
  
  -   在 Windows Server Essentials 伺服器上, 也可以管理 Exchange Online 通訊群組的電子郵件。  
  
- 藉由將自訂的網際網路網域連結至 Office 365 訂閱, 從您組織的網際網路網域 (例如, contoso.com) 傳送和接收電子郵件。  
  
- 從儀表板管理您的訂用帳戶和 Office 365 整合。  
  
- 如果您的訂用帳戶包含 SharePoint Online 文件庫, Office 365 與 Windows Server Essentials 伺服器的整合可讓您:  
  
  - 從儀表板建立和管理您的 SharePoint Online 文件庫。  
  
    > [!NOTE]
    >  您也可以使用 My Server 2012 R2 應用程式, 從您的膝上型電腦、行動裝置或 Windows 手機處理 SharePoint Online 文件庫中的檔。 這項功能僅適用于 Windows Server Essentials。 如需詳細資訊, 請參閱[使用 My Server 應用程式](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。  
  
  - 從儀表板變更 SharePoint Online 小組網站的許可權, 或從儀表板開啟小組網站, 以進行其他變更。  
  
    如需詳細資訊, 請參閱[管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。  
  
- 如果您訂閱 Exchange Online, Office 365 與 Windows Server Essentials 伺服器的整合可讓您管理使用者用來連線到公司電子郵件伺服器的行動裝置:  
  
  -   當行動裝置連線到公司的電子郵件伺服器時，需要密碼保護。 設定最小密碼長度、可允許登入嘗試失敗次數，以及登入嘗試之間所需的最小時間。  
  
  -   如果您知道有安全性問題的裝置型號，可封鎖該行動裝置與 Exchange Online 的連線 。  
  
  -   如果行動裝置遺失或遭竊，在下一次開啟時，抹除此裝置來刪除任何公司機密資料。  
  
- Office 365 整合提供您連接到 Office 365 服務和資源的新方式:  
  
  -   從 Windows Server Essentials 啟動列開啟 Office 365 服務。 如需相關資訊, 請參閱[使用 Microsoft Office 365 的快速入門手冊](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。  
  
  -   使用 My Server 2012 R2 應用程式, 從您的膝上型電腦、行動裝置或 Windows phone 處理 SharePoint Online 文件庫中的檔。 如需相關資訊, 請參閱[使用 My Server 應用程式](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 這項功能僅適用于 Windows Server Essentials。  
  
##  <a name="BKMK_Configure"></a>設定 Office 365 整合  
 完成伺服器安裝之後, 您可以隨時將伺服器與 Office 365 整合。 如果您還沒有 Office 365 訂用帳戶, 您可以購買一個或註冊免費試用版訂用帳戶。  
  
 您將執行下列工作：  
  
-   [步驟 1：確認 Office 365 整合需求](#BKMK_StepOne_VERIFY)  
  
-   [步驟 2：整合伺服器與 Microsoft Office 365](#BKMK_StepTwo)  
  
-   [步驟 3：將您組織的網際網路功能變數名稱連結到 Office 365 (選擇性)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>步驟 1:確認 Office 365 整合需求  
 在開始之前，請確定伺服器符合這些需求：  
  
-   伺服器具備下列作業系統之一：Windows Server Essentials、Windows Server Essentials, 或已安裝 Windows Server Essentials 體驗角色的 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 作業系統。  
  
-   環境只能有一個網域控制站, 而且您必須在網域控制站上執行 Office 365 整合。  
  
-   您必須能從伺服器連線到網際網路。  
  
-   在開始 Office 365 整合之前, 您應該先在伺服器上安裝所有重大和重要更新。  
  
-   如果您想要在電子郵件地址和 SharePoint Online 資源中使用貴組織的網際網路網域, 您必須註冊該功能變數名稱, 才能在整合期間將網域連結到 Office 365。 如需詳細資訊，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
> [!NOTE]
>  您不需要事先訂閱 Office 365。 您將能夠購買訂用帳戶, 或在 Office 365 整合期間註冊免費試用版。 如果您想要查看 Office 365 的計畫和定價, 請[比較企業的 office 365 計畫](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。  
  
###  <a name="BKMK_StepTwo"></a>步驟 2:整合伺服器與 Microsoft Office 365  
 請在網域控制站上執行下列程式, 將您的 Windows Server Essentials 伺服器與 Office 365 整合。  
  
> [!NOTE]
>  無論您是否有 Windows Server Essentials 或 Windows Server Essentials, 程式都相同, 但您會從首頁的不同位置開始。 在 Windows Server Essentials 中, 您會在 [**服務**] 索引標籤上整合伺服器與 Office 365 和其他 Microsoft 線上服務。在 Windows Server Essentials 中, Office 365 整合是在 [**電子郵件**] 索引標籤上執行。  
  
##### <a name="to-integrate-the-server-with-office-365"></a>將伺服器與 Office 365 整合  
  
1. 以系統管理員身分登入伺服器, 然後開啟 [Windows Server Essentials 儀表板]。  
  
2. 在首頁上, 按一下 [**服務**] (在 [Windows Server Essentials] 中, 按一下 [**電子郵件**]), 按一下 [**與 Microsoft Office 365 整合**], 然後按一下 [**設定 Microsoft Office 365 整合**]。  
  
    [與 Microsoft Office 365 整合精靈] 會隨即出現。  
  
3. 在 [開始使用] 頁面上，執行下列動作之一：  
  
   -   如果您沒有 Office 365 的訂閱, 請按 **[下一步]** , 然後依照指示訂閱 office 365 或註冊試用版訂用帳戶。  
  
        您必須先登入 Office 365, 才能返回嚮導。 但是您不需要在 Office 365 入口網站的 [**從這裡開始**] 區段中執行任何工作。  
  
   -   如果您已經有想要與伺服器整合的 Office 365 訂閱, 請選取 [**我已經有 office 365 的訂閱**], 然後按 **[下一步]** 。  
  
4. 遵循指示以完成精靈。  
  
   在嚮導順利完成之後, 您會注意到儀表板的下列變更:  
  
-   有一個新的**office 365**頁面, 用來管理整合和您的 Office 365 訂閱。  
  
-   在 [**使用者**] 頁面上, 您會看到 [**線上帳戶**] 索引標籤, 您可以在此建立和管理 Microsoft Online Services 帳戶, 讓您的使用者存取 Office 365。 如果您使用 Exchange Online 且有 Windows Server Essentials 伺服器, 您也會看到 [**通訊群組**] 索引標籤。  
  
-   Windows Server Essentials 伺服器上的 [**儲存體**] 頁面具有 [ **sharepoint**程式庫] 索引標籤, 可用於管理 sharepoint Online 文件庫和變更小組網站的許可權。 Office 365 的每個商務計畫都包含這些基本的 SharePoint Online 功能。  
  
###  <a name="BKMK_StepThree"></a>步驟 3:將您組織的網際網路功能變數名稱連結到 Office 365 (選擇性)  
 如果您想要在寄給組織的電子郵件和 SharePoint Online 資源的 Url 中使用您自己的網際網路網域, 您可以將自訂網域連結到您的 Office 365 訂閱。 如果您將 Windows Server Essentials 伺服器與 Office 365 整合, 您可以從儀表板執行此動作。  
  
 在為您的使用者建立線上帳戶之前, 最好先執行此動作, 如此一來, 您就可以在大量建立線上帳戶時使用網域。  
  
 當您註冊 Office 365 時, 會取得功能變數名稱, 例如*contoso*. onmicrosoft.com。 如果您想要使用不同的功能變數名稱嗎？比方說, 只是 contoso.com 嗎？您可以。 如果您還沒有功能變數名稱, 您必須購買一個功能變數名稱, 並變更一些 DNS 記錄。  
  
 設定要與 Office 365 搭配使用的自訂網域包含四個工作:  
  
1. **購買網域名稱。** 也就是向網域註冊機構或 DNS 主機服務提供者註冊網域。  
  
   -   挑選可與 Office 365 搭配使用的功能變數名稱。 您可以使用第二層功能變數名稱 (例如 buycontoso.com？, 而不是第三層功能變數名稱)？例如, marketing.contoso.com。 如需在 Office 365 中選擇要使用之網域的詳細資訊, 請參閱[網域](https://technet.microsoft.com/library/office-365-domains.aspx)。  
  
   -   向網域註冊機構購買, 允許 Office 365 所需的功能變數名稱伺服器 (DNS) 記錄。 若要找出哪些網域註冊機構允許所需的 DNS 記錄，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果您已經向不同的註冊機構註冊您的網域, 別擔心;當您將網域連結到 Office 365 時, 可以將網域轉移給不同的註冊機構。  
  
2. **設定 DNS 紀錄以允許 Office 365 服務使用網域名稱。** 最簡單的方式是在您于步驟3將網域連結到 Office 365 訂閱時, 讓 wizard 為您設定 DNS 記錄。 如果您想要自行做, 請參閱[如何手動設定 Office 365 整合的 DNS 記錄](#BKMK_ManuallyConfigureDNS)。  
  
3. **將您的自訂網際網路網域連結到 Office 365 訂閱。** 您將使用 [將**網域連結到 Office 365** ] 動作。  
  
4. **確認您的 Office 365 服務使用新的功能變數名稱。**  
  
   如果您在使用 [將**網域連結到 office 365** ] 工作之前, 先完成步驟1和 2, 則嚮導可以將功能變數名稱連結到 office 365。 或者，您可以讓精靈協助您進行部分或全部的步驟 1 和 2。  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>將貴組織的網際網路網域連結至 Office 365  
  
1.  在儀表板上，開啟 [Office 365] 頁面，然後按一下 [將網域連結到 Office 365]。  
  
2.  遵循指示以完成精靈。  
  
     此 wizard 可以協助您進行註冊、設定及連結新的或現有的網際網路功能變數名稱, 以在 Office 365 中使用的部分或所有步驟。  
  
     按一下精靈頁面上的說明連結，以取得完成工作所需的資訊。 或者, 請參閱[管理 Windows Server Essentials 中的遠端 Web 存取中](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx)的設定功能變數名稱一節, 以取得處理常式總覽和需求。  
  
    > [!NOTE]
    >  若要使用精靈註冊新的網域名稱，您必須使用與 Microsoft 有合作關係的網域名稱服務提供者，才能與精靈密切整合。 若要尋找網域名稱註冊機構，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
3.  如果嚮導偵測到您的功能變數名稱不是由伺服器管理, 您必須手動設定必要的 DNS 記錄來完成設定。 如需指示，請參閱本主題稍後討論的 [如何以手動方式設定 Office 365 整合的 DNS 記錄](#BKMK_ManuallyConfigureDNS)。  
  
4.  確認在 Office 365 中使用的是網域。  
  
     在嚮導完成後, 會等待一些時間, 而網域註冊機構會驗證 DNS 記錄。 這會自動發生;您不需要執行任何動作。 但通常需要大約一小時的時間, 而且有時候會有更長的時間。 當網域驗證完成時, **Office 365**頁面會列出貴組織的網域。  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>如何手動設定 Office 365 整合的 DNS 記錄  
 如果̌ [將網域連結到 Office 365] 精靈偵測到您的網域名稱不是由伺服器管理，您必須手動設定必要的網域名稱伺服器 (DNS) 記錄，以完成設定。 在這種情況下, 您會在 **%username%\NewDNSRecords_ (n) .txt**中找到您必須設定的 DNS 記錄清單, 其中 *(n)* 是亂數字。  
  
 下表描述您必須加入的 DNS 記錄。 輸入法因不同的網域名稱註冊機構而有所不同。 如果您有任何問題，請向您的網域名稱註冊機構尋求協助。  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>將自訂的網際網路網域名稱連結到 Office 365 需要的 DNS 記錄  
  
|服務|需要的 DNS 記錄|用途|  
|-------------|--------------------------|-------------|  
|(多個服務)|MX| Office 365 會使用此記錄來確認您擁有特定的功能變數名稱。 這個 MX 記錄不會干擾電子郵件訊息路由。|  
|Exchange Online|MX|提供電子郵件訊息路由。 **重要事項：** 如果您要移轉電子郵件，請勿指派零 (**0**) 的喜好設定到新的 MX 記錄。 請確定記錄值大於指派給目前 MX 記錄的值。 當電子郵件遷移完成, 而且您已準備好將電子郵件伺服器變更為 Office 365 時, 請讓您的網域註冊機構重設新 MX 記錄的喜好設定值。|  
|Exchange Online|別名 (CNAME)|用來幫助使用者輕易設定 Exchange Online 與 Outlook 桌面用戶端或行動式電子郵件用戶端之間連線的自動探索記錄。 **注意：** 如果您想要使用組織自己的功能變數名稱 (例如, http://mail.contoso.com) 而不是標準 URL (https://outlook.com/owa/office365.com) ) 來存取 Outlook Web 存取, 您可以設定別名 (CName) 記錄, 如下所示:**Type = CNAME, TTL = 01:00:00, HostName = mail, Address = mail. office365 .com**|  
|Exchange Online|TXT|指定 outlook.com (由 Office 365 電子郵件伺服器使用的網域) 授權代表您的網域傳送電子郵件。 建立此記錄可協助防止您寄出的電子郵件被標示為垃圾郵件。|  
|Lync Online|SRV|協助啟用與其他 Windows Live 或 Yahoo! 等立即訊息服務聯盟。|  
|Lync Online|SRV|用來幫助使用者輕易設定 Lync 桌面用戶端與 Microsoft Lync Online 之間連線的自動探索記錄。|  
  
> [!IMPORTANT]
>  網域驗證完成後, 請勿嘗試從 Office 365 入口網站新增或對 DNS 記錄進行任何進一步的變更。  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>下一步  
  
-   為您的使用者建立 Microsoft Online Services 帳戶。  
  
     若要使用 Office 365 服務, 您的使用者必須具有與其網路使用者帳戶相關聯的 Microsoft Online Services 帳戶。 儀表板可讓此作業變得簡單。 如果您使用的是新的 Office 365 訂閱, 您可以為現有的使用者帳戶大量建立線上帳戶。 如果您要將新的伺服器與您已在使用的 Office 365 訂閱整合, 您可以從現有的線上帳戶匯入使用者帳戶。 如需相關程式, 請參閱[管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。  
  
> [!NOTE]
>  在 Windows Server Essentials 中的儀表板上, Microsoft Online Services 帳戶稱為 Office 365 帳戶。 帳戶是相同的；只有名稱做了變更。  
  
##  <a name="BKMK_ManageIntegration"></a>管理 Office 365 整合  
 將您的伺服器與 Office 365 整合之後, 儀表板上的 [ **Office 365** ] 頁面會顯示 office 365 訂閱的相關資訊, 並提供這些工作:  
  
-   [管理您的 Office 365 訂閱](#BKMK_ManageO365)？變更您用來管理訂閱的系統管理員帳戶。 開啟 Office 365 系統管理儀表板來管理您的訂用帳戶。  
  
-   將[您組織的網際網路網域連結至 Office 365](#BKMK_StepThree) ？如果您想要能夠傳送和接收已寄送到您自己網域的電子郵件, 您可以將網域連結至 Office 365。 (先前已討論過[, 步驟 3:將您組織的網域連結到 Office](#BKMK_StepThree)365。)  
  
-   要[停用 Office 365 整合](#BKMK_Disable)嗎？如果您不想從儀表板管理您的 Office 365 服務、訂閱及線上帳戶, 您可以停用 Office 365 整合。 這些服務仍可在 Office 365 入口網站上取得。  
  
###  <a name="BKMK_ManageO365"></a>管理您的 Office 365 訂閱  
 如果您在處理伺服器時需要變更 Office 365 訂閱, 您可以從儀表板的 [ **office 365** ] 頁面開啟 office 365 中的訂閱。 您也可以變更伺服器用來變更 Office 365 服務的系統管理員帳戶。  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>在 Office 365 管理儀表板上開啟訂閱  
  
1.  在 [Windows Server Essentials 儀表板] 上, 開啟 [ **Office 365** ] 頁面。  
  
2.  在 [設定工作]中，按一下 [管理 Office 365]。  
  
3.  使用您用來管理訂閱的 Microsoft 線上帳戶登入 Office 365。  
  
     Office 365 管理儀表板隨即開啟。  
  
##### <a name="to-change-the-office-365-administrator-account"></a>變更 Office 365 系統管理員帳戶  
  
1.  在儀表板上按一下 [Office 365]。  
  
2.  在 [設定工作]中，按一下 [變更 Office 365 系統管理員帳戶]。 [變更系統管理員帳戶精靈] 會隨即出現。 (在 Windows Server Essentials 中, 嚮導命名為 [設定 Office 365 系統管理員帳戶])。  
  
3.  輸入您要用來連接到 Office 365 訂閱的帳號憑證, 然後按 **[下一步]** 。  
  
4.  按一下 **關閉**。 儀表板會重新啟動。  
  
###  <a name="BKMK_Disable"></a>停用 Office 365 整合  
 如果您決定不要從儀表板管理您的 Office 365 服務和線上帳戶, 您可以停用 Office 365 整合。 您的 Office 365 訂閱仍然有效, 而您從儀表板所做的任何設定變更都會生效。 例如, 您會收到電子郵件, 寄送到您連結到 Office 365 訂閱的功能變數名稱。 您不會遺失任何電子郵件, 而且您為行動裝置設定的控制項仍然會使用 Exchange Online。  
  
 接下來, 您將在 Office 365 中管理 Office 365 訂閱、服務和資源, 而您的使用者將需要管理 Office 365 中的線上帳戶密碼。 不會再進行密碼同步化, 而停用或移除使用者帳戶將不會影響使用者的線上帳戶。  
  
 因為 Office 365 整合軟體安裝在本機伺服器上, 即使整合服務無法連線到 Office 365, 您仍可以停用此功能。  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>停用 Office 365 與伺服器的整合  
  
1.  在儀表板上按一下 [Office 365]。  
  
2.  按一下 [停用 Office 365 整合]。 [停用 Office 365 精靈] 會隨即出現。  
  
3.  遵循指示以完成精靈。  
  
> [!NOTE]
>  若要再次啟用 Office 365 整合, 請使用儀表板**首頁**的 [**服務**] 索引標籤上的 [**與 Office 365 整合**] 工作。 如需指示, [請參閱步驟 2:將您的 Windows Server Essentials 伺服器與本](#BKMK_StepTwo)主題稍早的 Microsoft Office 365 整合。  
  
##  <a name="BKMK_Troubleshoot"></a>針對 Office 365 整合進行疑難排解  
 本節提供的資訊可協助您針對在 Windows Server Essentials 中使用 Office 365 整合功能時可能會遇到的常見問題進行疑難排解。  
  
###  <a name="BKMK_AcctsNotCreated"></a>未建立某些 Microsoft Online Services 帳戶  
 **描述**  
  
 嘗試從儀表板建立一或多個 Microsoft Online Services 帳戶不成功。  
  
 **解決方法**  
  
1.  按一下精靈完成頁面上的連結開啟結果檔案，其中會包含未順利完成之每個帳戶建立要求的詳細資訊。 例如，結果可能會通知您所要求的帳戶名稱已經存在於 Microsoft Online Services 帳戶中。  
  
2.  請採取建議的動作，以解決每個錯誤。  
  
3.  如果這個問題持續發生，請重新啟動伺服器，然後重新嘗試建立線上帳戶。  
  
###  <a name="BKMK_ProblemUninstalling"></a>卸載 Office 365 整合時發生問題  
 **描述**  
  
 當您嘗試停用 Office 365 整合時, 發生未知的錯誤。  
  
 **解決方法**  
  
1.  確定您的電腦已連線到網際網路，然後再試一次。  
  
2.  如果錯誤再次發生，請重新啟動伺服器，然後再試一次。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Windows Server Essentials 的服務整合總覽-第1部分](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Windows Server Essentials 的服務整合總覽-第2部分](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [使用 Microsoft Office 365 的快速入門手冊](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
