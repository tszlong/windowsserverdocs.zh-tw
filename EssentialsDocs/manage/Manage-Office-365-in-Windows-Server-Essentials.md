---
title: 在 Windows Server Essentials 中管理 Office 365
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d897c9186ff74a80d531cf84961709de54459f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433285"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理 Office 365

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

當您與 Microsoft Office 365 整合您的 Windows Server Essentials 伺服器時，則您可以管理您的 Office 365 服務和線上帳戶，以及 Windows Server Essentials 儀表板從內部部署資源。 本主題中，您將了解您可以取得藉由整合您的伺服器與 Office 365、 如何這麼做，以及如何管理和疑難排解您的 Office 365 整合。  
  
  
  
> [!IMPORTANT]
>   Office 365 整合只支援單一網域控制站的環境中。 此外，Office 365 整合精靈必須執行網域控制站上。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [為什麼應該整合 Office 365 與我的伺服器？](#BKMK_IntegrationOverview)  
  
-   [設定 Office 365 整合](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [管理 Office 365 整合](#BKMK_ManageIntegration)  
  
-   [Office 365 整合疑難排解](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> 為什麼應該整合 Office 365 與我的伺服器？  
 有許多的好理由整合 Office 365 與 Windows Server Essentials 伺服器。 如果您管理某些內部資源，但用於其他服務中的 Office 365，您可以從儀表板，以及在內部部署資源，而不是在兩個地方管理您的 Office 365 服務和資源。  
  
- 管理線上帳戶，為 Office 365 中的使用者存取，以及您的使用者帳戶：  
  
  -   在單一步驟中為您的使用者建立 Microsoft Online Services 帳戶，或是在伺服器上為您現有的線上帳戶建立使用者帳戶。 您也可以將線上帳戶新增到新的或現有的使用者帳戶。  
  
       當您從儀表板建立線上帳戶時，使用者登入 Office 365 與伺服器使用相同的密碼。 如果他們變更了使用者帳戶的密碼，線上密碼也會變更。 這樣您就知道他們的線上帳戶密碼，永遠符合您為使用者帳戶設定的安全性需求。  
  
  -   在整個使用者帳戶生命週期裡管理線上帳戶以及使用者帳戶。 如果您停用使用者帳戶，線上帳戶也會停用。 如果您移除使用者帳戶，線上帳戶也會移除。  
  
  -   在 Windows Server Essentials 伺服器上，也可以管理電子郵件的 Exchange Online 通訊的群組。  
  
- 傳送和接收來自您組織的網際網路網域 (例如，contoso.com) 的電子郵件將自訂網際網路網域連結至 Office 365 訂用帳戶。  
  
- 從儀表板管理您的訂用帳戶和 Office 365 整合。  
  
- 如果您的訂閱包含 SharePoint Online 文件庫，與 Windows Server Essentials 伺服器的 Office 365 整合可讓您：  
  
  - 建立和管理您的 SharePoint Online 文件庫，從儀表板。  
  
    > [!NOTE]
    >  您也可以使用 My Server 2012 R2 應用程式，才能使用您從膝上型電腦、 行動裝置或 Windows phone 的 SharePoint Online 程式庫中的文件。 這項功能只適用於 Windows Server Essentials。 如需詳細資訊，請參閱 < [Use the My Server App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。  
  
  - 變更 SharePoint Online 小組網站的權限的儀表板，或進行其他變更儀表板開啟小組網站。  
  
    如需詳細資訊，請參閱 <<c0> [ 管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。  
  
- 如果您有訂閱 Exchange online，與 Windows Server Essentials 伺服器的 Office 365 整合可讓您管理之行動裝置的使用者用來連接到您的公司電子郵件伺服器：  
  
  -   當行動裝置連線到公司的電子郵件伺服器時，需要密碼保護。 設定最小密碼長度、可允許登入嘗試失敗次數，以及登入嘗試之間所需的最小時間。  
  
  -   如果您知道有安全性問題的裝置型號，可封鎖該行動裝置與 Exchange Online 的連線 。  
  
  -   如果行動裝置遺失或遭竊，在下一次開啟時，抹除此裝置來刪除任何公司機密資料。  
  
- Office 365 整合可讓您連線到 Office 365 服務和資源的新方法：  
  
  -   從 Windows Server Essentials 啟動列開啟 Office 365 服務。 如需資訊，請參閱[使用 Microsoft Office 365 的快速入門指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。  
  
  -   使用 My Server 2012 R2 應用程式來處理您的 SharePoint Online 程式庫，從您的膝上型電腦、 行動裝置或 Windows phone 中的文件。 如需資訊，請參閱[Use the My Server App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 這項功能僅供以 Windows Server Essentials。  
  
##  <a name="BKMK_Configure"></a> 設定 Office 365 整合  
 在任何時間，完成伺服器安裝之後，您可以使用 Office 365 整合您的伺服器。 如果您還沒有 Office 365 訂用帳戶，您可以購買或註冊免費的試用版訂用帳戶。  
  
 您將執行下列工作：  
  
-   [步驟 1：確認 Office 365 整合需求](#BKMK_StepOne_VERIFY)  
  
-   [步驟 2：將伺服器與 Microsoft Office 365 整合](#BKMK_StepTwo)  
  
-   [步驟 3：您的組織網際網路網域名稱連結到 Office 365 （選用）](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> 步驟 1:確認 Office 365 整合需求  
 在開始之前，請確定伺服器符合這些需求：  
  
-   伺服器具備下列作業系統之一：Windows Server Essentials、 Windows Server Essentials 或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 作業系統已安裝的 Windows Server Essentials 體驗角色。  
  
-   環境中只能有一個網域控制站，以及您必須在網域控制站上執行 Office 365 整合。  
  
-   您必須能從伺服器連線到網際網路。  
  
-   開始 Office 365 整合之前，您應該在伺服器上安裝所有重大和重要更新。  
  
-   如果您想要使用電子郵件地址，並使用 SharePoint Online 資源，您的組織網際網路網域，您必須註冊網域名稱，因此您可以將網域連結到 Office 365 整合期間。 如需詳細資訊，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
> [!NOTE]
>  您不需要事先訂閱 Office 365。 您可以購買訂用帳戶或註冊免費的試用版，Office 365 整合期間。 如果您想要查看方案與定價適用於 Office 365[比較適用於商務的 Office 365 方案](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。  
  
###  <a name="BKMK_StepTwo"></a> 步驟 2:將伺服器與 Microsoft Office 365 整合  
 與 Office 365 整合您的 Windows Server Essentials 伺服器的網域控制站上執行下列程序。  
  
> [!NOTE]
>  是否有 Windows Server Essentials 或 Windows Server Essentials 中，但您會開始在首頁上不同的地方，此程序的相同。 在 Windows Server Essentials 中，您必須整合與 Office 365 和其他 Microsoft Online Services 的伺服器上**Services**  索引標籤。Windows Server Essentials 中，在 Office 365 整合在執行**電子郵件** 索引標籤。  
  
##### <a name="to-integrate-the-server-with-office-365"></a>將伺服器與 Office 365 整合  
  
1. 身為管理員，在伺服器上登入，然後開啟 Windows Server Essentials 儀表板。  
  
2. 在上**首頁**頁面上，按一下**服務**(在 Windows Server Essentials 中，按一下 **電子郵件**)，按一下 **與 Microsoft Office 365 整合**，然後按一下**設定 Microsoft Office 365 整合**。  
  
    [與 Microsoft Office 365 整合精靈] 會隨即出現。  
  
3. 在 [開始使用]  頁面上，執行下列動作之一：  
  
   -   如果您沒有 Office 365 訂用帳戶，請按一下**下一步**，並遵循指示訂閱 Office 365 或登入註冊試用版訂用帳戶。  
  
        您必須登入 Office 365 然後返回精靈。 您不需要執行任何一項工作中，但**從這裡開始**Office 365 入口網站的區段。  
  
   -   如果您已經有您想要與伺服器上，選取整合的 Office 365 訂用帳戶**我已經訂閱 Office 365**，然後按一下**下一步**。  
  
4. 遵循指示以完成精靈。  
  
   精靈成功完成之後，您會注意到儀表板的下列變更：  
  
-   新**Office 365**頁面上，用來管理整合與 Office 365 訂用帳戶。  
  
-   在 [**使用者**頁面上，您會看到**線上帳戶**您可以在其中建立並管理 Microsoft Online Services 帳戶，讓您的使用者存取 Office 365] 索引標籤。 如果您正在使用 Exchange Online 並具有 Windows Server Essentials 伺服器時，您也會看到**發佈群組** 索引標籤。  
  
-   **儲存體**Windows Server Essentials 伺服器上的頁面已**SharePoint 文件庫** 索引標籤來管理 SharePoint Online 文件庫和變更您的小組網站權限。 適用於 Office 365 的每個商務計劃包含下列基本的 SharePoint Online 功能。  
  
###  <a name="BKMK_StepThree"></a> 步驟 3:您的組織網際網路網域名稱連結到 Office 365 （選用）  
 如果您想要使用您自己的網際網路網域，您的組織和 SharePoint Online 資源的 Url 定址的電子郵件中，您可以在 Office 365 訂用帳戶連結的自訂網域。 如果您使用 Office 365 整合您的 Windows Server Essentials 伺服器，您可以從儀表板。  
  
 它是最簡單的方式執行這項操作，因此當您大量建立線上帳戶時，您可以使用的網域，為您的使用者建立線上帳戶之前。  
  
 當您註冊 Office 365 時，取得網域名稱嗎？ 例如*contoso*。 onmicrosoft.com。 如果您想要使用不同的網域名稱？ 說，只要 contoso.com？ 即可。 您必須購買網域名稱，如果您已經不一個，並變更部分 DNS 記錄。  
  
 若要使用與 Office 365 設定自訂網域包含四項工作：  
  
1. **購買網域名稱。** 也就是向網域註冊機構或 DNS 主機服務提供者註冊網域。  
  
   -   選擇與 Office 365 搭配運作的網域名稱。 您可以使用第 2 個層級網域名稱嗎？ 例如，buycontoso.com？ 但不是第 3 層級網域名稱嗎？ 例如，marketing.contoso.com。 如需有關如何選擇要使用 Office 365 中的網域的詳細資訊，請參閱[網域](https://technet.microsoft.com/library/office-365-domains.aspx)。  
  
   -   可讓 Office 365 所需的網域名稱伺服器 (DNS) 記錄的網域註冊機構購買。 若要找出哪些網域註冊機構允許所需的 DNS 記錄，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果您已經向不同的註冊機構註冊網域，別擔心;您將網域連結到 Office 365 之後，您就可以將網域轉移到不同的註冊機構。  
  
2. **設定 DNS 紀錄以允許 Office 365 服務使用網域名稱。** 最簡單的方式是讓精靈為您設定的 DNS 記錄，當您將網域連結至在步驟 3 中您 Office 365 訂用帳戶。 如果您想自行操作，請參閱[如何以手動設定 DNS 記錄的 Office 365 整合](#BKMK_ManuallyConfigureDNS)。  
  
3. **將自訂的網際網路網域連結到您 Office 365 訂用帳戶。** 您將使用**將網域連結到 Office 365**動作。  
  
4. **確認您的 Office 365 服務使用新的網域名稱。**  
  
   如果您使用之前完成步驟 1 和 2**將網域連結到 Office 365**工作中，精靈可以連結到 Office 365 的網域名稱。 或者，您可以讓精靈協助您進行部分或全部的步驟 1 和 2。  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>若要將公司網際網路網域連結到 Office 365  
  
1.  在儀表板上，開啟 [Office 365]  頁面，然後按一下 [將網域連結到 Office 365]  。  
  
2.  遵循指示以完成精靈。  
  
     此精靈可協助您進行部分或全部的註冊、 設定及連結新的或現有網際網路網域名稱來使用 Office 365 中的步驟。  
  
     按一下精靈頁面上的說明連結，以取得完成工作所需的資訊。 或參閱 設定網域名稱一段[Windows Server Essentials 中管理遠端 Web 存取](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx)程序概觀和需求。  
  
    > [!NOTE]
    >  若要使用精靈註冊新的網域名稱，您必須使用與 Microsoft 有合作關係的網域名稱服務提供者，才能與精靈密切整合。 若要尋找網域名稱註冊機構，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
3.  如果精靈偵測到您的網域名稱不管理伺服器，您必須手動設定必要的 DNS 記錄，以完成設定。 如需指示，請參閱本主題稍後討論的 [如何以手動方式設定 Office 365 整合的 DNS 記錄](#BKMK_ManuallyConfigureDNS)。  
  
4.  確認 Office 365 中使用該網域。  
  
     在精靈完成後，網域名稱註冊機構確認 DNS 記錄時，沒有一小段的等候。 發生這種情況而您不必採取任何動作。 但它通常會花大約一小時？ 和有時候會更久。 當網域驗證完畢**Office 365**頁面會列出貴組織的網域。  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> 如何手動設定 Office 365 整合的 DNS 記錄  
 如果̌ [將網域連結到 Office 365] 精靈偵測到您的網域名稱不是由伺服器管理，您必須手動設定必要的網域名稱伺服器 (DNS) 記錄，以完成設定。 在此情況下，您會發現一份您必須在設定的 DNS 記錄 **%username%\NewDNSRecords_ (n).txt**，其中 *(n)* 是隨機的數字。  
  
 下表描述您必須加入的 DNS 記錄。 輸入法因不同的網域名稱註冊機構而有所不同。 如果您有任何問題，請向您的網域名稱註冊機構尋求協助。  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>將自訂的網際網路網域名稱連結到 Office 365 需要的 DNS 記錄  
  
|服務|需要的 DNS 記錄|用途|  
|-------------|--------------------------|-------------|  
|(多個服務)|MX| Office 365 使用此記錄確認您擁有特定的網域名稱。 這個 MX 記錄不會干擾電子郵件訊息路由。|  
|Exchange Online|MX|提供電子郵件訊息路由。 **重要事項：** 如果您要移轉電子郵件，請勿指派零 (**0**) 的喜好設定到新的 MX 記錄。 請確定記錄值大於指派給目前 MX 記錄的值。 當電子郵件移轉完成，且您已準備好將電子郵件伺服器變更為 Office 365 時，有網域名稱註冊機構重設新 MX 記錄的喜好設定值。|  
|Exchange Online|別名 (CNAME)|用來幫助使用者輕易設定 Exchange Online 與 Outlook 桌面用戶端或行動式電子郵件用戶端之間連線的自動探索記錄。 **注意：** 如果您想要使用您的組織自己的網域名稱來存取 Outlook Web Access (例如 http://mail.contoso.com)而非標準的 URL (https://outlook.com/owa/office365.com)，您可以設定的別名 (CName) 記錄，如下所示：**Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|指定 Office 365 電子郵件伺服器所使用的網域 outlook.com 已獲授權代表您的網域傳送電子郵件。 建立此記錄可協助防止您寄出的電子郵件被標示為垃圾郵件。|  
|Lync Online|SRV|協助啟用與其他 Windows Live 或 Yahoo! 等立即訊息服務聯盟。|  
|Lync Online|SRV|用來幫助使用者輕易設定 Lync 桌面用戶端與 Microsoft Lync Online 之間連線的自動探索記錄。|  
  
> [!IMPORTANT]
>  網域之後，確認已完成，請勿嘗試新增或變更任何進一步的 DNS 記錄從 Office 365 入口網站。  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> 下一個步驟  
  
-   為您的使用者建立 Microsoft Online Services 帳戶。  
  
     若要使用 Office 365 服務，您的使用者必須具有與其網路使用者帳戶相關聯的 Microsoft Online Services 帳戶。 儀表板可讓此作業變得簡單。 如果您使用新的 Office 365 訂用帳戶，您可以大量建立線上帳戶為您現有的使用者帳戶。 如果您要將新的伺服器整合與您已經使用 Office 365 訂用帳戶，您可以從現有的線上帳戶匯入的使用者帳戶。 如需程序，請參閱[管理之使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。  
  
> [!NOTE]
>  在 Windows Server Essentials 儀表板，Microsoft Online Services 帳戶稱為 Office 365 帳戶。 帳戶是相同的；只有名稱做了變更。  
  
##  <a name="BKMK_ManageIntegration"></a> 管理 Office 365 整合  
 與 Office 365 整合您的伺服器之後**Office 365**儀表板 頁面會顯示您 Office 365 訂用帳戶的相關資訊，並進行這些工作：  
  
-   [管理 Office 365 訂用帳戶](#BKMK_ManageO365)嗎？變更您用來管理訂用帳戶的系統管理員帳戶。 開啟 Office 365 管理儀表板來管理您的訂用帳戶。  
  
-   [您的組織網際網路網域連結到 Office 365](#BKMK_StepThree)嗎？如果您想要能夠傳送和接收電子郵件寄給您自己的網域，您可以將網域連結到 Office 365。 (在稍早所述[步驟 3:您的組織網域連結到 Office 365](#BKMK_StepThree)。)  
  
-   [停用 Office 365 整合](#BKMK_Disable)嗎？如果您不想從儀表板管理您的 Office 365 服務、 訂閱及線上帳戶，您可以停用 Office 365 整合。 服務會在 Office 365 入口網站上仍然可用。  
  
###  <a name="BKMK_ManageO365"></a> 管理 Office 365 訂用帳戶  
 如果您需要 Office 365 訂用帳戶進行變更，當您在伺服器上處理時，您可以從 Office 365 中開啟訂用帳戶**Office 365**儀表板。 您也可以變更伺服器用來變更 Office 365 服務的系統管理員帳戶。  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>在 Office 365 管理儀表板上開啟訂閱  
  
1.  在 Windows Server Essentials 儀表板上開啟**Office 365**頁面。  
  
2.  在 [設定工作]  中，按一下 [管理 Office 365]  。  
  
3.  您用來管理您的訂用帳戶的 Microsoft 線上帳戶登入 Office 365。  
  
     Office 365 管理儀表板隨即開啟。  
  
##### <a name="to-change-the-office-365-administrator-account"></a>變更 Office 365 系統管理員帳戶  
  
1.  在儀表板上按一下 [Office 365]  。  
  
2.  在 [設定工作]  中，按一下 [變更 Office 365 系統管理員帳戶]  。 [變更系統管理員帳戶精靈] 會隨即出現。 （在 Windows Server Essentials 中，精靈會稱為設定 Office 365 系統管理員帳戶。）  
  
3.  輸入您想要使用以連接到您 Office 365 訂用帳戶，然後按一下帳戶的認證**下一步**。  
  
4.  按一下 **關閉**。 儀表板會重新啟動。  
  
###  <a name="BKMK_Disable"></a> 停用 Office 365 整合  
 如果您決定不要從儀表板管理您的 Office 365 服務和線上帳戶，您可以停用 Office 365 整合。 Office 365 訂用帳戶會保持作用中，與您從儀表板所做的任何組態變更持續有效。 例如，您會收到電子郵件寄給您連結到 Office 365 訂用帳戶的網域名稱。 您不必擔心會遺失任何電子郵件，並仍在您設定行動裝置的控制項使用 Exchange Online。  
  
 接下來，您將管理您的 Office 365 訂用帳戶、 服務和 Office 365 中的資源，和您的使用者需要管理他們在 Office 365 中的線上帳戶的密碼。 不再進行密碼同步處理，並停用或移除使用者帳戶不會影響使用者的線上帳戶。  
  
 因為 Office 365 整合軟體安裝在本機伺服器上，您可以在即使整合服務無法連線到 Office 365 停用此功能。  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>停用 Office 365 與伺服器的整合  
  
1.  在儀表板上按一下 [Office 365]  。  
  
2.  按一下 [停用 Office 365 整合]  。 [停用 Office 365 精靈] 會隨即出現。  
  
3.  遵循指示以完成精靈。  
  
> [!NOTE]
>  若要啟用 Office 365 整合一次，請使用**與 Office 365 整合**上的工作**服務** 索引標籤的儀表板**首頁**頁面。 如需指示，請參閱[步驟 2:整合您的 Windows Server Essentials 伺服器與 Microsoft Office 365](#BKMK_StepTwo)稍早在本主題中。  
  
##  <a name="BKMK_Troubleshoot"></a> Office 365 整合疑難排解  
 本節中的資訊可協助您疑難排解使用 Windows Server Essentials 中的 Office 365 整合功能時，可能會遇到的常見問題。  
  
###  <a name="BKMK_AcctsNotCreated"></a> 某些 Microsoft Online Services 帳戶未建立  
 **描述**  
  
 若要從儀表板建立一或多個 Microsoft Online Services 帳戶嘗試未成功。  
  
 **解決方法**  
  
1.  按一下精靈完成頁面上的連結開啟結果檔案，其中會包含未順利完成之每個帳戶建立要求的詳細資訊。 例如，結果可能會通知您所要求的帳戶名稱已經存在於 Microsoft Online Services 帳戶中。  
  
2.  請採取建議的動作，以解決每個錯誤。  
  
3.  如果這個問題持續發生，請重新啟動伺服器，然後重新嘗試建立線上帳戶。  
  
###  <a name="BKMK_ProblemUninstalling"></a> 發生問題，解除安裝 Office 365 整合  
 **描述**  
  
 當您嘗試停用 Office 365 整合時，就會發生未知的錯誤。  
  
 **解決方法**  
  
1.  確定您的電腦已連線到網際網路，然後再試一次。  
  
2.  如果錯誤再次發生，請重新啟動伺服器，然後再試一次。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Windows Server Essentials-第 1 部分的服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Windows Server Essentials-第 2 部分的服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [使用 Microsoft Office 365 快速入門指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
