---
title: "管理 Windows Server Essentials 中的 Office 365"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Office 365

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

當您使用 Microsoft Office 365 整合您 Windows Server Essentials 的伺服器時，您可以管理您的 Office 365 服務及 online 帳號，以及從 Windows Server Essentials 儀表板上場所資源。 本主題中，就了解您可以取得您的伺服器整合使用 Office 365、如何，以及如何管理和您的 Office 365 整合的疑難排解。  
  
  
  
> [!IMPORTANT]
>   Office 365 整合僅支援單一網域控制站環境中。 此外，Office 365 整合精靈必須網域控制站執行。  
  
## <a name="in-this-topic"></a>本主題  
  
-   [為何應該整合 Office 365 使用我的伺服器？](#BKMK_IntegrationOverview)  
  
-   [Office 365 整合設定](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [管理 Office 365 整合](#BKMK_ManageIntegration)  
  
-   [Office 365 整合的疑難排解](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>為何應該整合 Office 365 使用我的伺服器？  
 有許多與 Windows Server Essentials 伺服器整合 Office 365 的原因。 如果您管理的一些您公司的資源，但其他服務使用 Office 365，就能從儀表板，以及您先資源，而不是在兩個位置管理您的 Office 365 服務和資源。  
  
-   管理 online 帳號，可供您的使用者存取 Office 365 加上您使用者帳號：  
  
    -   建立您的使用者的 Microsoft Online Services 帳號，只要單一步驟，或是建立帳號您現有的 online 帳號的伺服器上。 您也可以新增 online account 到新的或現有帳號。  
  
         當您從儀表板中建立 online 帳號時，使用者登入 Office 365 使用相同的密碼，他們使用的伺服器。 如果他們變更其帳號的密碼，也變更 online 密碼。 而且您知道該 online account 密碼一律符合安全性需求使用者帳號，您設定。  
  
    -   管理加上生命帳號週期帳號 online account。 如果您停用帳號，online 帳號也已停用。 如果您移除帳號，online account 也會移除。  
  
    -   Windows Server Essentials 的伺服器上也管理 Online 換貨的電子郵件的通訊群組。  
  
-   傳送和接收的電子郵件從您的組織的網際網路網域 (例如，contoso.com) 來將自訂網際網路網域連結到您的 Office 365 裝機費。  
  
-   從儀表板中管理裝機費與 Office 365 整合。  
  
-   如果您裝機費包含 SharePoint Online 媒體櫃、Office 365 整合與 Windows Server Essentials 伺服器可讓您：  
  
    -   建立及管理您 SharePoint Online 媒體櫃的儀表板。  
  
        > [!NOTE]
        >  市您也可以使用與從您的膝上型電腦、行動裝置版的裝置或 Windows phone 您 SharePoint Online 媒體櫃中的文件我 Server 2012 R2 應用程式。 此功能僅適用於 Windows Server Essentials。 如需詳細資訊，請查看[使用我的伺服器 App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。  
  
    -   變更小組 SharePoint Online 網站的權限的儀表板，或從儀表板進行其他變更開放團隊網站。  
  
     如需詳細資訊，請查看[管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。  
  
-   如果您希望 Online 換貨，Office 365 整合與 Windows Server Essentials 伺服器可讓您管理您的使用者使用連接到您的公司的電子郵件伺服器的行動裝置版裝置：  
  
    -   行動裝置版裝置連接到公司電子郵件伺服器時，需要密碼保護。 設定密碼最小長度、數目失敗的登入嘗試允許，以及登入嘗試之間所需的最低時間。  
  
    -   封鎖的行動裝置版裝置連接到 Online 換貨，如果您知道的裝置型號的安全性問題。  
  
    -   如果您的行動裝置版裝置遺失或遭竊，擦拭 delete 任何重要的公司資料的裝置已在下一次裝置。  
  
-    Office 365 整合為您提供新的方式可以連接 Office 365 服務和資源：  
  
    -   從 Windows Server Essentials Launchpad 開放 Office 365 服務。 資訊，請查看[到使用 Microsoft Office 365 的快速入門指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。  
  
    -   使用我的 Server 2012 R2 應用程式可以從您的膝上型電腦、行動裝置版的裝置或 Windows phone 您 SharePoint Online 媒體櫃中的文件。 資訊，請查看[使用我的伺服器 App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 Windows Server Essentials 只有這項功能。  
  
##  <a name="BKMK_Configure"></a>Office 365 整合設定  
 在任何時候伺服器安裝完成後，您可以使用 Office 365 整合您的伺服器。 如果您不 t 已經有 Office 365 裝機費，您可以登入取得免費試用版裝機費或購買。  
  
 您將會執行下列工作：  
  
-   [步驟 1：檢查 Office 365 整合需求](#BKMK_StepOne_VERIFY)  
  
-   [步驟 2：使用 Microsoft Office 365 整合伺服器](#BKMK_StepTwo)  
  
-   [步驟 3：連結組織的網際網路的網域名稱（選擇性）的 Office 365](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>步驟 1：檢查 Office 365 整合需求  
 在您開始之前，請確定伺服器符合下列需求：  
  
-   伺服器可以有這些作業系統：Windows Server Essentials、Windows Server Essentials 或 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter 作業系統已安裝 Windows Server Essentials 體驗角色。  
  
-   環境只能有一個的網域控制站，您必須執行網域控制站 Office 365 整合。  
  
-   您必須是連接到網際網路，從伺服器。  
  
-   Office 365 整合開始之前，您應該在伺服器上安裝所有重要和重要更新。  
  
-   如果您想要使用的組織的網際網路網域中的電子郵件地址與 SharePoint Online 資源，就必須為隊伍登記的網域名稱，以便您可以與 Office 365 連結網域期間整合。 如需詳細資訊，請查看[如何購買的網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
> [!NOTE]
>  您不 t 需求事先希望 Office 365。 就可以購買裝機費或登入取得免費試用期間 Office 365 整合。 如果 d 您想要看看方案和價格的 Office 365、[適用於企業的 Office 365 方案的比較](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。  
  
###  <a name="BKMK_StepTwo"></a>步驟 2：使用 Microsoft Office 365 整合伺服器  
 將您的 Windows Server Essentials 伺服器整合使用 Office 365 的網域控制站執行下列程序。  
  
> [!NOTE]
>  此程序 s 相同您擁有的是 Windows Server Essentials 或 Windows Server Essentials，但就開始，從 [首頁] 頁面上的不同位置。 在 Windows Server Essentials，您將整合使用 Office 365 和其他 Microsoft Online Services 的伺服器上**服務**索引標籤。在 Windows Server Essentials，Office 365 整合在執行**的電子郵件**索引標籤。  
  
##### <a name="to-integrate-the-server-with-office-365"></a>若要使用 Office 365 整合伺服器  
  
1.  系統管理員的身分的伺服器上登入並打開 Windows Server Essentials 儀表板。  
  
2.  在**Home**頁面上，按一下 [**服務**(在 Windows Server Essentials，按一下 [**的電子郵件**)，按一下**整合使用 Microsoft Office 365**，，然後按一下 [ **Microsoft Office 365 整合設定**。  
  
     使用 Microsoft Office 365 精靈整合就會出現。  
  
3.  在**開始**頁面上，進行下列動作：  
  
    -   如果您不 t 裝機費與 Office 365，請按一下**下一步**，然後依照指示希望或 Office 365 上的試用版裝機費。  
  
         您必須登入 Office 365 之前您回到精靈。 您不 t 需要執行任何中的工作，但**開始以下**區段的 Office 365 入口網站。  
  
    -   如果您已經有 Office 365 整合的伺服器上，選取您想要裝機費**裝機費已經有 Office 365**，然後按一下 [**下**。  
  
4.  請依照下列指示完成精靈。  
  
 成功完成精靈之後，就會注意到儀表板，下列變更：  
  
-   那里 s 一個新的**Office 365**頁面上，可用來管理整合與您的 Office 365 裝機費。  
  
-   在**使用者**頁面上，您市查看**Online 帳號**索引標籤上，您可以在此建立及管理 Microsoft Online Services 帳號，可供您的使用者存取 Office 365。 如果您重新使用 Online 交換與 Windows Server Essentials 伺服器，也就會看到**通訊群組**索引標籤。  
  
-   **存放裝置**頁面上的 Windows Server Essentials 伺服器已**SharePoint 媒體櫃**管理 SharePoint Online 媒體櫃和變更您的小組網站的權限] 索引標籤。 每個商務用 Office 365 計劃包含這些基本 SharePoint Online 功能。  
  
###  <a name="BKMK_StepThree"></a>步驟 3：連結組織的網際網路的網域名稱（選擇性）的 Office 365  
 如果您想要使用網際網路網域中的電子郵件傳送給您的組織和 SharePoint Online 資源的 Url，您可以到您的 Office 365 裝機費連結自訂的網域。 如果您的 Windows Server Essentials 伺服器整合使用 Office 365，您可以從儀表板。  
  
 它，因此當您大量-建立 online 帳號，您可以使用網域 s 最簡單的方式執行此動作，才能建立 online 帳號為您的使用者。  
  
 當您登入 Office 365，您會收到的網域名稱？，例如*以 contoso*。onmicrosoft.com。如果您 d 而不會使用不同的網域名稱？顯示，只要 contoso.com？，您可以。 您必須購買的網域名稱如果您不 t 已經自己，並 DNS 記錄的一些變更。  
  
 若要使用 Office 365 設定自訂網域包含四個工作：  
  
1.  **購買的網域名稱。** 它也就是登記域或 DNS 裝載提供者。  
  
    -   請選擇使用 Office 365 的網域名稱。 您可以使用的第 2 層級的網域名稱？，例如 buycontoso.com？，但不是第 3 層級的網域名稱？，例如 marketing.contoso.com。如需關於選擇要使用 Office 365 中的網域資訊，請查看[網域](https://technet.microsoft.com/library/office-365-domains.aspx)。  
  
    -   從可所需的 Office 365 的網域名稱 (DNS) 伺服器記錄域購買。 若要了解哪些網域登錄程式允許所需的 DNS 記錄，請查看[如何購買的網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果您已經登記您的網域，以不同的登錄器，請不要擔心;當您的網域連結 Office 365，您可以到不同的登錄器傳輸網域。  
  
2.  **設定 DNS 記錄允許 Office 365 服務使用的網域名稱。** 簡單的方法就是讓您設定的 DNS 記錄，當您連結到您的 Office 365 裝機費中執行「步驟 3 的網域精靈。 如果您 d 而它自己，請查看[如何手動設定 DNS 記錄 Office 365 整合的](#BKMK_ManuallyConfigureDNS)。  
  
3.  **您的 Office 365 裝機費連結您自訂網際網路網域。** 市使用**與 Office 365 連結網域**的動作。  
  
4.  **確認您的 Office 365 服務會使用新的網域名稱。**  
  
 如果您使用之前，先完成步驟 1 到 2**與 Office 365 連結網域**工作，精靈可以連結 Office 365 的網域名稱。 或者，您可以有幫助您的部分或所有步驟 1 到 2 精靈。  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>若要連結 [您的組織的網際網路網域與 Office 365  
  
1.  儀表板，請打開**Office 365**頁面中，按一下 [**連結 Office 365 網域**。  
  
2.  請依照下列指示完成精靈。  
  
     精靈可協助您的部分或所有登記、設定及連結新的或現有網際網路網域名稱使用 Office 365 中的步驟。  
  
     按一下 [協助頁面上的連結精靈將以完成任務所需的資訊。 查看設定的網域名稱區段或者[在 Windows Server Essentials 管理遠端 Web 存取](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx)的處理程序概觀和需求。  
  
    > [!NOTE]
    >  若要使用登記新的網域名稱精靈中，您必須使用與 Microsoft 提供的精靈順暢整合的網域名稱服務提供者的其中一個。 若要尋找域名稱，請查看[如何購買的網域名稱](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
3.  如果精靈偵測到您的網域名稱到 t 由伺服器，您必須手動設定所需的 DNS 記錄以完成設定。 指示，請查看[如何手動設定 DNS 記錄 Office 365 整合的](#BKMK_ManuallyConfigureDNS)、本主題中的更新版本。  
  
4.  請確認的網域中 Office 365 使用。  
  
     那里精靈完成之後、時名稱域驗證的 DNS 記錄小等待 s。 發生這種情形自動;您不 t 需要做任何事情。 但通常花大約一小時？和有時候較長的時間。 網域驗證完成時，**Office 365**頁面會列出您的組織的網域。  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>如何手動設定 DNS 記錄 Office 365 整合  
 Office 365 精靈連結您網域偵測到您的網域名稱不受伺服器，以完成設定，您必須手動設定所需的網域名稱 (DNS) 伺服器記錄。 若是如此，就尋找 DNS 記錄，您必須在設定清單**(n) %username%\NewDNSRecords_.txt**，其中*(n)*就是。  
  
 下表描述您必須將新增的 DNS 記錄。 項目方法可以使用不同的網域名稱登錄程式而有所不同。 如果您有任何問題，您的網域名稱登錄器尋求協助。  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>所需的 DNS 記錄連結自訂網際網路網域名稱與 Office 365  
  
|服務|需要的 DNS 記錄|用途|  
|-------------|--------------------------|-------------|  
|（多服務）|MX| Office 365 使用這個記錄以確認您擁有的特定的網域名稱。 這個 MX 記錄不會干擾路由傳送電子郵件。|  
|換貨 Online|MX|提供的電子郵件訊息路由。 **重要事項：**如果您是移轉的電子郵件，請勿將 0 的喜好設定 (**0**) 的新 MX 記錄。 請確定您的值為記錄大於已指派給目前 MX 記錄值。 當您的電子郵件移轉已完成，您就可以變更 Office 365 郵件伺服器有您重設值新 MX 記錄喜好設定的網域名稱登錄器。|  
|換貨 Online|別名 (CNAME)|用來協助使用者輕鬆自動探索記錄設定換貨 Online 與他們 Outlook 傳統型 client 或行動裝置版的電子郵件 client 之間的連結。 **注意：**如果您偏好使用 Outlook Web Access 存取，而不標準 URL (https://outlook.com/owa/office365.com) 中使用您的組織 s 自己的網域名稱 (例如，http://mail.contoso.com)，您可以以下列方式設定別名 (CName) 記錄：**輸入 = CNAME，TTL = 01: 00:00，主機名稱 = 郵件地址 = mail.office365.com**|  
|換貨 Online|TXT|指定的授權 outlook.com，網域使用 Office 365 郵件伺服器，代表您的網域傳送電子郵件。 建立這記錄有助於避免您輸出的電子郵件會標示為垃圾郵件。|  
|Lync Online|SRV|協助讓其他即時訊息中心服務，例如 Windows Live 或 yahoo！奇摩聯盟。|  
|Lync Online|SRV|Lync 桌面 client 和 Microsoft Lync Online 之間的連接自動探索記錄使用輕鬆協助使用者的設定。|  
  
> [!IMPORTANT]
>  網域後確認已完成，請勿嘗試新增或變更進一步的 DNS 記錄從 Office 365 入口網站。  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>下一步  
  
-   建立 Microsoft Online Services 帳號，您的使用者。  
  
     若要使用 Office 365 服務，您的使用者必須他們網路使用者 account 相關聯的 Microsoft Online Services account。 儀表板輕鬆這。 如果您使用新的 Office 365 裝機費 re，您可以大量-建立 online 帳號的現有的使用者帳號。 如果您使用您已經使用 re，您可以匯入帳號 online 現有的 Office 365 裝機費整合新的伺服器會帳號。 程序，請查看[適用於使用者管理 Online 帳號](Manage-Online-Accounts-for-Users.md)。  
  
> [!NOTE]
>  在 Windows Server Essentials 儀表板上 Microsoft Online Services 帳號稱為帳號 Office 365。 帳號是一樣;僅限變更詞彙。  
  
##  <a name="BKMK_ManageIntegration"></a>管理 Office 365 整合  
 之後您的伺服器整合 Office 365、**Office 365**頁面儀表板上的顯示您的 Office 365 裝機費的相關資訊，讓這些工作：  
  
-   [管理您的 Office 365 裝機費](#BKMK_ManageO365)嗎？變更您用來管理裝機費管理員。 打開 Office 365 系統管理員儀表板以管理您裝機費。  
  
-   [Office 365 連結您的組織的網際網路網域](#BKMK_StepThree)嗎？如果您想要無法傳送和接收的電子郵件傳送給自己網域，則您可以與 Office 365 連結網域。 (討論之前，於[執行「步驟 3：您的組織的網域連結 Office 365](#BKMK_StepThree)。)  
  
-   [停用 Office 365 整合](#BKMK_Disable)嗎？如果您不 t 想要從儀表板管理您的 Office 365 服務、裝機費及 online 帳號，您可以停用 Office 365 整合。 服務會仍可使用 Office 365 入口網站上。  
  
###  <a name="BKMK_ManageO365"></a>管理您的 Office 365 裝機費  
 如果您需要變更您同時重新伺服器處理您 Office 365 裝機費，您可以開放裝機費中的 Office 365 從**Office 365**頁面上的儀表板。 您也可以變更管理員伺服器用 Office 365 服務進行變更。  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>打開您裝機費 Office 365 系統管理員儀表板  
  
1.  Windows Server Essentials 儀表板，請打開**Office 365**頁面。  
  
2.  在**設定工作**，按一下 [**管理 Office 365**。  
  
3.  與 Microsoft online 帳號，您用來管理您裝機費登入 Office 365。  
  
     Office 365 系統管理員儀表板開啟。  
  
##### <a name="to-change-the-office-365-administrator-account"></a>若要變更的 Office 365 系統管理員帳號  
  
1.  儀表板，請按一下**Office 365**。  
  
2.  在**設定工作**，按一下 [**變更 Office 365 系統管理員 account**。 變更系統管理員 Account 精靈會出現。 （在 Windows Server Essentials，精靈命名設定好 Office 365 系統管理員 Account。）  
  
3.  輸入認證帳號，您要用來連接您的 Office 365 裝機費、，然後按一下**下一步**。  
  
4.  按一下**關閉**。 儀表板重新開機。  
  
###  <a name="BKMK_Disable"></a>停用 Office 365 整合  
 如果您認為您不 t 想要從儀表板中管理您的 Office 365 服務及 online 帳號，您可以停用 Office 365 整合。 您的 Office 365 裝機費維持使用中狀態，並從儀表板做的任何設定變更維持作用中。 例如，就會收到電子郵件傳送給您連結到您的 Office 365 裝機費網域名稱。 您贏得 t 會遺失任何電子郵件，並控制您的行動裝置版裝置設定仍會使用 Exchange Online。  
  
 接下來，您管理您的 Office 365 裝機費、服務及中 Office 365、資源和您的使用者必須管理適用於 online 帳號 Office 365 中的密碼。 密碼同步不再發生，且停用或移除帳號使用者 s online 帳號不會影響。  
  
 本機伺服器上安裝的 Office 365 整合軟體，由於即使的整合服務無法連接到 Office 365 您可以停用此功能。  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>若要停用 Office 365 整合與伺服器  
  
1.  儀表板，請按一下**Office 365**。  
  
2.  按一下**停用 Office 365 整合**。 停用 Office 365 精靈會出現。  
  
3.  請依照下列指示完成精靈。  
  
> [!NOTE]
>  要使用 Office 365 整合再試一次，請使用**使用 Office 365 整合**工作上**服務**索引標籤的儀表板 s **Home**頁面。 指示，請查看[步驟 2：將您的 Windows Server Essentials 伺服器整合 Microsoft Office 365 的](#BKMK_StepTwo)，舊版本主題中。  
  
##  <a name="BKMK_Troubleshoot"></a>Office 365 整合的疑難排解  
 本節中的資訊可協助您疑難排解常見使用 Office 365 整合功能在 Windows Server Essentials 時，可能會遇到的問題。  
  
###  <a name="BKMK_AcctsNotCreated"></a>部分 Microsoft Online Services 帳號尚未建立  
 **描述**  
  
 嘗試從儀表板並 t 一或多個 Microsoft Online Services 帳號建立成功。  
  
 **方案**  
  
1.  按一下精靈完成打開結果檔案，包含每個 account 建立要求未成功的詳細的資訊頁面上的連結。 例如，結果可能會通知您 Online Services 的 Microsoft account 已經有要求 account 的名稱。  
  
2.  需要建議的動作解析每個錯誤。  
  
3.  如果問題持續發生，請重新開機伺服器，並再試一次建立 online 帳號。  
  
###  <a name="BKMK_ProblemUninstalling"></a>發生問題解除安裝的 Office 365 整合  
 **描述**  
  
 您嘗試要停用 Office 365 整合時發生錯誤。  
  
 **方案**  
  
1.  請確定電腦已連接到網際網路，並再試一次。  
  
2.  如果再次發生錯誤，重新開機伺服器，並再試一次。  
  
## <a name="see-also"></a>也了  
  
-   [針對 Windows Server Essentials 的一部分 1 服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [針對 Windows Server Essentials 的一部分 2 服務整合概觀](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [使用 Microsoft Office 365 的快速入門指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
