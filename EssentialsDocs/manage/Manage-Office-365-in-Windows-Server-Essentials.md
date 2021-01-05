---
title: 管理 Windows Server Essentials 中的 Microsoft 365
description: 瞭解如何從 Windows Server Essentials 儀表板管理您的 Microsoft 365 服務和線上帳戶，以及您的內部部署資源。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 1df50d94d8ec118742d089c11afbc5fff17f9800
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811355"
---
# <a name="manage-microsoft-365-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Microsoft 365

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

當您將 Windows Server Essentials 伺服器與 Microsoft 365 整合時，您可以從 Windows Server Essentials 儀表板管理您的 Microsoft 365 服務和線上帳戶，以及您的內部部署資源。 在本主題中，您將瞭解如何藉由整合您的伺服器與 Microsoft 365、如何進行，以及如何管理和疑難排解您的 Microsoft 365 整合，來取得所能獲得的好處。



> [!IMPORTANT]
>   僅在單一網域控制站環境中支援 Microsoft 365 整合。 此外，Microsoft 365 integration wizard 必須在網域控制站上執行。

## <a name="in-this-topic"></a>本主題內容

-   [為什麼我應該將 Microsoft 365 與我的伺服器整合？](#BKMK_IntegrationOverview)

-   [設定 Microsoft 365 整合](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)

-   [管理 Microsoft 365 整合](#BKMK_ManageIntegration)

-   [針對 Microsoft 365 整合進行疑難排解](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)

##  <a name="why-should-i-integrate-microsoft-365-with-my-server"></a><a name="BKMK_IntegrationOverview"></a> 為什麼我應該將 Microsoft 365 與我的伺服器整合？
 將 Microsoft 365 與 Windows Server Essentials 伺服器整合有很大的理由。 如果您在內部管理某些資源，但使用其他服務的 Microsoft 365，您將能夠從儀表板管理您的 Microsoft 365 服務和資源，以及內部部署資源，而不是在兩個地方工作。

- 管理可讓使用者存取 Microsoft 365 的線上帳戶以及使用者帳戶：

  -   在單一步驟中為您的使用者建立 Microsoft Online Services 帳戶，或是在伺服器上為您現有的線上帳戶建立使用者帳戶。 您也可以將線上帳戶新增到新的或現有的使用者帳戶。

       當您從儀表板建立線上帳戶時，使用者會以他們在伺服器上使用的相同密碼登入 Microsoft 365。 如果他們變更了使用者帳戶的密碼，線上密碼也會變更。 這樣您就知道他們的線上帳戶密碼，永遠符合您為使用者帳戶設定的安全性需求。

  -   在整個使用者帳戶生命週期裡管理線上帳戶以及使用者帳戶。 如果您停用使用者帳戶，線上帳戶也會停用。 如果您移除使用者帳戶，線上帳戶也會移除。

  -   在 Windows Server Essentials 伺服器上，也可以管理電子郵件的 Exchange Online 通訊群組。

- 從您組織的網際網路網域傳送和接收電子郵件 (例如，將自訂網際網路網域連結至 Microsoft 365 訂用帳戶，以 contoso.com) 。

- 從儀表板管理您的訂用帳戶和 Microsoft 365 整合。

- 如果您的訂閱包含 SharePoint Online 文件庫，Microsoft 365 與 Windows Server Essentials 伺服器的整合可讓您：

  - 從儀表板建立及管理您的 SharePoint Online 文件庫。

    > [!NOTE]
    >  您也可以使用 My Server 2012 R2 應用程式從您的膝上型電腦、行動裝置或 Windows phone 處理 SharePoint Online 文件庫中的檔。 這項功能僅適用于 Windows Server Essentials。 如需詳細資訊，請參閱 [使用 My Server 應用程式](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。

  - 從儀表板變更 SharePoint Online 小組網站的許可權，或從儀表板開啟小組網站以進行其他變更。

    如需詳細資訊，請參閱 [管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。

- 如果您訂閱了 Exchange Online，Microsoft 365 與 Windows Server Essentials 伺服器的整合，可讓您管理使用者用來連線到公司電子郵件伺服器的行動裝置：

  -   當行動裝置連線到公司的電子郵件伺服器時，需要密碼保護。 設定最小密碼長度、可允許登入嘗試失敗次數，以及登入嘗試之間所需的最小時間。

  -   如果您知道有安全性問題的裝置型號，可封鎖該行動裝置與 Exchange Online 的連線 。

  -   如果行動裝置遺失或遭竊，在下一次開啟時，抹除此裝置來刪除任何公司機密資料。

- Microsoft 365 整合可讓您連線至 Microsoft 365 服務和資源的新方法：

  -   從 Windows Server Essentials 啟動控制板開啟 Microsoft 365 服務。 如需詳細資訊，請參閱 [快速入門手冊使用 Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。

  -   使用 My Server 2012 R2 應用程式從您的膝上型電腦、行動裝置或 Windows phone 處理 SharePoint Online 文件庫中的檔。 如需詳細資訊，請參閱 [使用 My Server 應用程式](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 這項功能僅適用于 Windows Server Essentials。

##  <a name="set-up-microsoft-365-integration"></a><a name="BKMK_Configure"></a> 設定 Microsoft 365 整合
 完成伺服器安裝之後，您可以隨時將伺服器與 Microsoft 365 整合。 如果您還沒有 Microsoft 365 訂用帳戶，您可以購買或註冊免費的試用訂用帳戶。

 您將執行下列工作：

-   [步驟1：確認 Microsoft 365 整合需求](#BKMK_StepOne_VERIFY)

-   [步驟2：整合伺服器與 Microsoft 365](#BKMK_StepTwo)

-   [步驟3：將您組織的網際網路功能變數名稱連結至 Microsoft 365 (選擇性) ](#BKMK_StepThree)

###  <a name="step-1-verify-microsoft-365-integration-requirements"></a><a name="BKMK_StepOne_VERIFY"></a> 步驟1：確認 Microsoft 365 整合需求
 在開始之前，請確定伺服器符合這些需求：

-   伺服器可以有下列任何一種作業系統： Windows Server Essentials、Windows Server Essentials 或已安裝 Windows Server Essentials 體驗角色的 windows server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 作業系統。

-   環境只能有一個網域控制站，您必須在網域控制站上執行 Microsoft 365 整合。

-   您必須能從伺服器連線到網際網路。

-   您應該先在伺服器上安裝所有重大和重要的更新，然後再開始 Microsoft 365 整合。

-   如果您想要在電子郵件地址和 SharePoint Online 資源中使用您組織的網際網路網域，您必須註冊功能變數名稱，如此您就可以在整合期間連結網域以 Microsoft 365。 如需詳細資訊，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。

> [!NOTE]
>  您不需要事先訂閱 Microsoft 365。 您將能夠在 Microsoft 365 整合期間購買訂用帳戶或註冊免費試用版。 如果您想要查看 Microsoft 365 的計畫和定價，請 [比較企業 Microsoft 365 方案](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。

###  <a name="step-2-integrate-the-server-with-microsoft-365"></a><a name="BKMK_StepTwo"></a> 步驟2：整合伺服器與 Microsoft 365
 請在網域控制站上執行下列程式，將您的 Windows Server Essentials 伺服器與 Microsoft 365 整合。

> [!NOTE]
>  無論您是否有 Windows Server Essentials 或 Windows Server Essentials，程式都相同，但您會從首頁上的不同位置開始。 在 Windows Server Essentials 中，您會將伺服器與 [ **服務** ] 索引標籤上的 Microsoft 365 及其他 Microsoft Online Services 整合。在 Windows Server Essentials 中，會在 [ **電子郵件** ] 索引標籤上執行 Microsoft 365 整合。

##### <a name="to-integrate-the-server-with-microsoft-365"></a>若要整合伺服器與 Microsoft 365

1. 以系統管理員身分登入伺服器，然後開啟 [Windows Server Essentials 儀表板]。

2. 在 **首頁** 上，按一下 [Windows Server Essentials 中的 **服務** (]，按一下 [ **電子郵件** ]) ，按一下 [ **與 Microsoft 365 整合**]，然後按一下 [ **設定 Microsoft 365 整合**]。

    [與 Microsoft 365 Wizard 整合] 隨即出現。

3. 在 [開始使用] 頁面上，執行下列動作之一：

   -   如果您沒有 Microsoft 365 的訂用帳戶，請按 [ **下一步]**，然後依照指示訂閱 Microsoft 365 或註冊試用版訂用帳戶。

        您將需要先登入 Microsoft 365，然後再返回嚮導。 但是您不需要在 Microsoft 365 入口網站的 [ **從此處開始** ] 區段中執行任何工作。

   -   如果您已經有要與伺服器整合的 Microsoft 365 訂用帳戶，請選取 [ **我已經有 Microsoft 365 的訂用** 帳戶]，然後按 **[下一步]**。

4. 遵循指示以完成精靈。

   當 wizard 順利完成之後，您會注意到儀表板的下列變更：

-   有新的 **Microsoft 365** 頁面可用來管理整合和您的 Microsoft 365 訂用帳戶。

-   在 [ **使用者** ] 頁面上，您會看到 [ **線上帳戶** ] 索引標籤，您可以在其中建立及管理可讓使用者存取 Microsoft 365 的 Microsoft Online Services 帳戶。 如果您是使用 Exchange Online 並具有 Windows Server Essentials 伺服器，您也會看到 [ **通訊群組** ] 索引標籤。

-   Windows Server Essentials 伺服器上的 [ **儲存體** ] 頁面具有 [ **sharepoint** 程式庫] 索引標籤，可用於管理 sharepoint Online 文件庫及變更小組網站的許可權。 Microsoft 365 的每個業務計畫都包含這些基本的 SharePoint Online 功能。

###  <a name="step-3-link-your-organizations-internet-domain-name-to-microsoft-365-optional"></a><a name="BKMK_StepThree"></a> 步驟3：將您組織的網際網路功能變數名稱連結至 Microsoft 365 (選擇性) 
 如果您想要在寄給組織的電子郵件中或 SharePoint Online 資源的 Url 中使用您自己的網際網路網域，您可以將自訂網域連結至您的 Microsoft 365 訂用帳戶。 如果您要將 Windows Server Essentials 伺服器與 Microsoft 365 整合，您可以從儀表板進行。

 在您為使用者建立線上帳戶之前，最簡單的作法是建立線上帳戶，讓您可以在大量建立線上帳戶時使用網域。

 當您註冊 Microsoft 365 *時，會取得功能變數名稱，例如* onmicrosoft.com。 如果您想要使用不同的功能變數名稱，也就是 contoso.com？您可以。 如果您還沒有功能變數名稱，您必須購買功能變數名稱，並變更部分 DNS 記錄。

 設定自訂網域以搭配 Microsoft 365 使用包含四項工作：

1. **購買網域名稱。** 也就是向網域註冊機構或 DNS 主機服務提供者註冊網域。

   -   挑選可搭配 Microsoft 365 使用的功能變數名稱。 您可以使用第2層的功能變數名稱（例如，buycontoso.com？但不是第3層的功能變數名稱），例如 marketing.contoso.com。 如需有關選擇要在 Microsoft 365 中使用之網域的詳細資訊，請參閱 [網域](/office365/servicedescriptions/office-365-platform-service-description/domains)。

   -   從允許功能變數名稱伺服器 (DNS) Microsoft 365 所需記錄的網域註冊機構購買。 若要找出哪些網域註冊機構允許所需的 DNS 記錄，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果您已經向不同的註冊機構註冊網域，別擔心;當您將網域連結至 Microsoft 365 時，您可以將網域轉移到不同的註冊機構。

2. **設定允許 Microsoft 365 服務使用功能變數名稱的 DNS 記錄。** 最簡單的方式是當您在步驟3中將網域連結至您的 Microsoft 365 訂用帳戶時，讓嚮導為您設定 DNS 記錄。 如果您想自行操作，請參閱 [如何手動設定 DNS 記錄以進行 Microsoft 365 整合](#BKMK_ManuallyConfigureDNS)。

3. **將您的自訂網際網路網域連結至您的 Microsoft 365 訂用帳戶。** 您將使用 **連結網域來 Microsoft 365** 動作。

4. **確認您的 Microsoft 365 服務使用新的功能變數名稱。**

   如果您在使用 [ **連結網域] Microsoft 365** 工作之前完成步驟1和2，則嚮導可以將功能變數名稱連結至 Microsoft 365。 或者，您可以讓精靈協助您進行部分或全部的步驟 1 和 2。

##### <a name="to-link-your-organizations-internet-domain-to-microsoft-365"></a>將您組織的網際網路網域連結至 Microsoft 365

1.  在 [儀表板] 上，開啟 [ **Microsoft 365** ] 頁面，然後按一下 [將 **網域連結到 Microsoft 365**]。

2.  遵循指示以完成精靈。

     此嚮導可協助您進行註冊、設定和連結新的或現有的網際網路功能變數名稱以在 Microsoft 365 中使用的部分或全部步驟。

     按一下精靈頁面上的說明連結，以取得完成工作所需的資訊。 或者，如需流程總覽和需求，請參閱在 [Windows Server Essentials 中管理遠端 Web 存取](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) 的 [設定功能變數名稱] 區段。

    > [!NOTE]
    >  若要使用精靈註冊新的網域名稱，您必須使用與 Microsoft 有合作關係的網域名稱服務提供者，才能與精靈密切整合。 若要尋找網域名稱註冊機構，請參閱 [如何購買網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。

3.  如果 wizard 偵測到您的功能變數名稱未受伺服器管理，您必須手動設定必要的 DNS 記錄，才能完成設定。 如需相關指示，請參閱本主題稍後的 [如何手動設定 Microsoft 365 整合的 DNS 記錄](#BKMK_ManuallyConfigureDNS)。

4.  確認 Microsoft 365 中使用網域。

     在 wizard 完成之後，網域註冊機構會驗證 DNS 記錄。 這會自動發生;您不必採取任何動作。 但它通常需要一小時的時間，有時會花上一點時間。 當網域驗證完成時， **Microsoft 365** 頁面會列出您組織的網域。

####  <a name="how-to-manually-configure-dns-records-for-microsoft-365-integration"></a><a name="BKMK_ManuallyConfigureDNS"></a> 如何手動設定 Microsoft 365 整合的 DNS 記錄
 如果您的網域要 Microsoft 365 Wizard 的連結偵測到您的功能變數名稱不是由伺服器管理，若要完成設定，您必須以手動方式將所需的功能變數名稱伺服器設定 (DNS) 記錄。 在這種情況下，您會在 **% username% \ NewDNSRecords_ (n) .txt** 找到必須設定的 DNS 記錄清單，其中 *(n)* 是亂數字。

 下表描述您必須加入的 DNS 記錄。 輸入法因不同的網域名稱註冊機構而有所不同。 如果您有任何問題，請向您的網域名稱註冊機構尋求協助。

### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-microsoft-365"></a>將自訂網際網路功能變數名稱連結至 Microsoft 365 所需的 DNS 記錄

|服務|需要的 DNS 記錄|目的|
|-------------|--------------------------|-------------|
|(多個服務)|MX| Microsoft 365 使用此記錄來確認您擁有特定的功能變數名稱。 這個 MX 記錄不會干擾電子郵件訊息路由。|
|Exchange Online|MX|提供電子郵件訊息路由。 **重要事項：**  如果您要遷移電子郵件，請勿將零 (**0**) 的喜好設定指派給新的 MX 記錄。 請確定記錄值大於指派給目前 MX 記錄的值。 當電子郵件遷移完成，而且您已準備好將電子郵件伺服器變更為 Microsoft 365 時，請讓您的網域註冊機構重設新 MX 記錄的喜好設定值。|
|Exchange Online|別名 (CNAME)|用來幫助使用者輕易設定 Exchange Online 與 Outlook 桌面用戶端或行動式電子郵件用戶端之間連線的自動探索記錄。 **注意：**  如果您想要使用組織本身的功能變數名稱來存取 Outlook Web 存取 (例如 http://mail.contoso.com) (， https://outlook.com/owa/office365.com) 您可以將別名設定 (cname) 記錄，如下所示： **輸入 = CNAME、TTL = 01：00：00、HostName = mail、Address = office365 .com**|
|Exchange Online|TXT|指定 outlook.com （Microsoft 365 電子郵件伺服器所使用的網域）有權代表您的網域傳送電子郵件。 建立此記錄可協助防止您寄出的電子郵件被標示為垃圾郵件。|
|Lync Online|SRV|協助啟用與其他 Windows Live 或 Yahoo! 等立即訊息服務聯盟。|
|Lync Online|SRV|用來幫助使用者輕易設定 Lync 桌面用戶端與 Microsoft Lync Online 之間連線的自動探索記錄。|

> [!IMPORTANT]
>  網域驗證完成後，請勿嘗試從 Microsoft 365 入口網站新增或對 DNS 記錄進行任何進一步的變更。

###  <a name="next-step"></a><a name="BKMK_StepFour_ACCOUNTS"></a> 下一步

-   為您的使用者建立 Microsoft Online Services 帳戶。

     若要使用 Microsoft 365 服務，您的使用者必須具有與其網路使用者帳戶相關聯的 Microsoft Online Services 帳戶。 儀表板可讓此作業變得簡單。 如果您要使用新的 Microsoft 365 訂用帳戶，您可以為現有的使用者帳戶大量建立線上帳戶。 如果您要將新的伺服器與您已在使用的 Microsoft 365 訂用帳戶整合，您可以從現有的線上帳戶匯入使用者帳戶。 如需相關程式，請參閱 [管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。

> [!NOTE]
>  在 Windows Server Essentials 的儀表板上，Microsoft Online Services 帳戶稱為 Microsoft 365 帳戶。 帳戶是相同的；只有名稱做了變更。

##  <a name="manage-microsoft-365-integration"></a><a name="BKMK_ManageIntegration"></a> 管理 Microsoft 365 整合
 將您的伺服器與 Microsoft 365 整合之後，儀表板上的 [ **Microsoft 365** ] 頁面會顯示您 Microsoft 365 訂用帳戶的相關資訊，並讓這些工作可供使用：

-   [管理您的 Microsoft 365 訂](#BKMK_ManageO365) 用帳戶？變更您用來管理訂用帳戶的系統管理員帳戶。 開啟 Microsoft 365 管理儀表板來管理您的訂用帳戶。

-   [要將您組織的網際網路網域連結到 Microsoft 365](#BKMK_StepThree) 嗎？如果您想要能夠傳送和接收發給您自己網域的電子郵件，您可以將網域連結至 Microsoft 365。  (稍早所討論的 [步驟3：將您組織的網域連結到 Microsoft 365](#BKMK_StepThree)。 ) 

-   要[停用 Microsoft 365 整合](#BKMK_Disable)嗎？如果您不想從儀表板管理您的 Microsoft 365 服務、訂用帳戶和線上帳戶，您可以停用 Microsoft 365 整合。 這些服務仍可在 Microsoft 365 入口網站上取得。

###  <a name="manage-your-microsoft-365-subscription"></a><a name="BKMK_ManageO365"></a> 管理您的 Microsoft 365 訂用帳戶
 如果您在使用伺服器時需要變更 Microsoft 365 訂用帳戶，您可以在 [儀表板] 的 [ **Microsoft 365** ] 頁面中，從 Microsoft 365 開啟訂用帳戶。 您也可以變更伺服器用來對 Microsoft 365 服務進行變更的系統管理員帳戶。

##### <a name="to-open-your-subscription-on-the-microsoft-365-admin-dashboard"></a>在 Microsoft 365 系統管理員儀表板上開啟您的訂用帳戶

1.  在 [Windows Server Essentials 儀表板] 上，開啟 [ **Microsoft 365** ] 頁面。

2.  在 [設定工作 **] 中，** 按一下 [ **管理 Microsoft 365**]。

3.  使用您用來管理訂閱的 Microsoft 線上帳戶登入 Microsoft 365。

     Microsoft 365 的系統管理員儀表板隨即開啟。

##### <a name="to-change-the-microsoft-365-administrator-account"></a>若要變更 Microsoft 365 系統管理員帳戶

1.  在儀表板上，按一下 [ **Microsoft 365**]。

2.  在 [設定工作 **] 中，** 按一下 **[變更 Microsoft 365 系統管理員帳戶**]。 [變更系統管理員帳戶精靈] 會隨即出現。  (在 Windows Server Essentials 中，嚮導會命名為「設定 Microsoft 365 系統管理員帳戶。 ) 

3.  輸入您要用來連線到 Microsoft 365 訂用帳戶之帳戶的認證，然後按 **[下一步]**。

4.  按一下 [關閉]  。 儀表板會重新啟動。

###  <a name="disable-microsoft-365-integration"></a><a name="BKMK_Disable"></a> 停用 Microsoft 365 整合
 如果您決定不想從儀表板管理您的 Microsoft 365 服務和線上帳戶，您可以停用 Microsoft 365 整合。 您的 Microsoft 365 訂用帳戶會保持作用中狀態，而您從儀表板所做的任何設定變更都會保持有效。 例如，您會收到一封電子郵件，寄給您連結至 Microsoft 365 訂用帳戶的功能變數名稱。 您不會遺失任何電子郵件，而且您為行動裝置設定的控制項仍會使用 Exchange Online。

 接下來，您將會在 Microsoft 365 中管理 Microsoft 365 的訂用帳戶、服務和資源，而您的使用者將需要在 Microsoft 365 中管理其線上帳戶的密碼。 密碼同步處理不會再發生，而且停用或移除使用者帳戶不會影響使用者的線上帳戶。

 因為 Microsoft 365 整合軟體安裝在本機伺服器上，即使整合服務無法連線到 Microsoft 365，您也可以停用此功能。

##### <a name="to-disable-microsoft-365-integration-with-the-server"></a>停用 Microsoft 365 與伺服器整合

1.  在儀表板上，按一下 [ **Microsoft 365**]。

2.  按一下 [ **停用 Microsoft 365 整合**]。 [停用] Microsoft 365 Wizard 隨即出現。

3.  遵循指示以完成精靈。

> [!NOTE]
>  若要再次啟用 Microsoft 365 整合，請使用儀表板 **首頁** 的 [**服務**] 索引標籤上的 [**與 Microsoft 365 整合**] 工作。 如需相關指示，請參閱本主題稍早的 [步驟2：將您的 Windows Server Essentials 伺服器與 Microsoft 365 整合](#BKMK_StepTwo)。

##  <a name="troubleshoot-microsoft-365-integration"></a><a name="BKMK_Troubleshoot"></a> 針對 Microsoft 365 整合進行疑難排解
 本節提供的資訊可協助您針對使用 Windows Server Essentials 中的 Microsoft 365 整合功能時可能遇到的常見問題進行疑難排解。

###  <a name="some-microsoft-online-services-accounts-were-not-created"></a><a name="BKMK_AcctsNotCreated"></a> 未建立某些 Microsoft Online Services 帳戶
 **描述**

 嘗試從儀表板建立一或多個 Microsoft Online Services 帳戶失敗。

 **解決方案**

1.  按一下精靈完成頁面上的連結開啟結果檔案，其中會包含未順利完成之每個帳戶建立要求的詳細資訊。 例如，結果可能會通知您所要求的帳戶名稱已經存在於 Microsoft Online Services 帳戶中。

2.  請採取建議的動作，以解決每個錯誤。

3.  如果這個問題持續發生，請重新啟動伺服器，然後重新嘗試建立線上帳戶。

###  <a name="there-was-a-problem-uninstalling-microsoft-365-integration"></a><a name="BKMK_ProblemUninstalling"></a> 卸載 Microsoft 365 整合時發生問題
 **描述**

 當您嘗試停用 Microsoft 365 整合時發生未知的錯誤。

 **解決方案**

1.  確定您的電腦已連線到網際網路，然後再試一次。

2.  如果錯誤再次發生，請重新啟動伺服器，然後再試一次。

## <a name="additional-references"></a>其他參考資料

-   [適用于 Windows Server Essentials 的服務整合總覽-第1部分](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-1)

-   [適用于 Windows Server Essentials 的服務整合總覽-第2部分](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-2)

-   [快速入門手冊使用 Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)

-   [管理 Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)