---
title: "管理適用於 Windows Server Essentials 使用者的 Online 帳號"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>管理適用於 Windows Server Essentials 使用者的 Online 帳號

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

當您將您的 Windows Server Essentials 伺服器整合使用 Microsoft Office 365 時，您可以管理加上帳號 online 帳號從儀表板。 本主題，就了解您可以藉由管理您的使用者 Microsoft Online Services 帳號從儀表板取得、如何建立及管理 online 帳號從儀表板，以及如何管理電子郵件地址和 distribution 群組的換貨 Online 從儀表板。  

  
> [!NOTE]
>  若要管理中 Windows Server Essentials 的 Microsoft Online Services 帳號，您必須使用 Office 365 整合您的伺服器。 適用於的指示，請查看[管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)。  
  
> [!IMPORTANT]
>  如果您管理在 Windows Server Essentials online 帳號，您慣用看到 Microsoft Online Services 帳號稱為*Office 365 帳號*。 在 Windows Server Essentials 儀表板上標籤已變更為*Microsoft Online Services 帳號*，或*Microsoft online 帳號*為 uif。 帳號和程序都相同。僅限變更標籤。 大部分的程序本主題中使用的字詞*online account*。  
  
## <a name="in-this-topic"></a>本主題  
  
-   [為何應該管理 online 帳號從儀表板？](#BKMK_WhyManageOnlineAccounts)  
  
-   [建立 online 帳號](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [管理 online 帳號](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [管理適用於 Online 換貨的電子郵件地址](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [適用於 Online 換貨管理通訊群組](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>為何應該管理 online 帳號從儀表板？  
 當您使用儀表板的使用者過去指派的 Microsoft Online Services 帳號時，自動同步處理 account 密碼，並且可以維護一起整個使用者 account s 週期兩個帳號。  
  
 它是便利的使用者，在伺服器上與 Office 365 中可以使用相同的密碼存取資源。 您可以將相同的密碼需求存取套用至中 Office 365，您需要為您的公司資源資源。  
  
### <a name="how-does-password-synchronization-work"></a>密碼同步處理的運作方式為何？  
 當您使用儀表板的使用者過去指派的 Microsoft Online Services 帳號時，使用者 account 密碼會自動同步處理的使用者 s online 帳號。 這表示使用者只需要存取這兩個的資源和 Office 365 伺服器上的單一密碼。 此外，您可以使用相同的名稱帳號及使用者 s online id。  
  
 密碼同步時，立即並自動時使用者變更其帳號的密碼，從加入網域的電腦或使用遠端網路存取。  
  
> [!IMPORTANT]
>  如果與 Windows Server Essentials 整合 Office 365，使用者應該從 Office 365 入口網站變更他們的 Microsoft online account 的密碼。 這樣將會使密碼同步處理。  
  
### <a name="simplified-account-creation"></a>簡化的 account 建立  
 當您從儀表板建立初始 online 帳號，還有其他優點。 您可以建立 online 帳號，適用於所有使用者的一項動作。 手動，如果您的員工，請使用 Office 365，您設定新的 Windows Server Essentials 伺服器，您可以從的一項動作 online 帳號所有使用者帳號建立。 如需詳細資訊，請查看[建立 online 帳號](#BKMK_SECTION_CreateOnlineAccounts)。  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>從儀表板管理電子郵件地址和 distribution 群組  
 您可以管理您的電子郵件地址和 distribution 群組的換貨 Online 從儀表板。 您可以使用您的組織的網際網路網域中的電子郵件地址。 您可以從儀表板，執行這一切都不登入 Office 365。 （您必須使用 Windows Server Essentials 的儀表板管理通訊群組。 這項功能會不支援 Windows Server Essentials。）如需詳細資訊，請查看[適用於 Online 換貨管理電子郵件地址](#BKMK_SECTION_ManageEmailAddresses)和[管理通訊群組 Online 換貨的](#BKMK_SECTION_ManageDistributionGroups)。  
  
### <a name="manage-the-user-account-and-online-account-together"></a>管理使用者 account online account 在一起  
 您可以管理加上整個 account s 週期帳號 online account。 如果您停用帳號，online 帳號也已停用 Microsoft Online Services 的。 如果您移除帳號，online account 也會移除。 如需詳細資訊，請查看[管理 online 帳號](#BKMK_SECTION_ManageOnlineAccounts)。  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>建立 online 帳號  
 您的伺服器整合 Office 365 之後，它是從儀表板中建立 Microsoft Online Services 帳號，您的使用者的優點。 就會有很多中建立 online 帳號彈性。 如果您有新的 Office 365 裝機費，您可以大量-建立 online 帳號適用於所有使用者。 如果您已經建立 online 帳號中 Office 365，請不要擔心。 如果您設定新的伺服器，您可以建立您的使用者帳號伺服器上 online 帳號匯入。 您可以指定新的或現有 online 帳號當您建立個人帳號或當您新增現有的使用者帳號 online account。  
  
 **授權需求**您需要為每個 online 帳號，您所建立的使用者的授權。 查看**Office 365**頁面上，以查看您的 Office 365 裝機費提供多少使用者授權儀表板。 如果您需要新增多個使用者的授權，就能開放您的 Office 365 裝機費中 Office 365，若要這樣做。  
  
 **使用者待處理**您新增使用者 account online 負責後，使用者所需變更的密碼，他們帳號下次登入。 與他們 online account 立即同步的新密碼。 在那之後，他們可以使用密碼登入 Office 365 他們 online id。  
  
> [!IMPORTANT]
>  提醒您的使用者應該不會變更其 online 密碼中 Office 365。 當他們變更其帳號的密碼，將會自動變更密碼。 如果他們變更密碼 online 中 Office 365，請密碼同步會中斷。  
  
 使用的程序本節中：  
  
-   [適用於您現有的使用者帳號大量建立 online 帳號](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [從您的 Microsoft Online Services 帳號匯入帳號](#BKMK_ToImportUserAccounts)  
  
-   [建立新的使用者 account 指派給其 online 帳號](#BKMK_ToCreateaNewUserAccount)  
  
-   [將 online account 指派給使用者 account](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  如果您使用 Windows Server Essentials，您將會看到*Office 365 account*而不是*Microsoft Online Services account*整個程序。 此程序相同，但在 Windows Server Essentials 的詞彙變更。  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>大量建立 online 帳號您現有的使用者帳號  
  
1.  系統管理員的身分的伺服器上登入並打開 Windows Server Essentials 儀表板。  
  
2.  儀表板，請打開**使用者**頁面。  
  
3.  在**使用者工作**，按一下 [**加入的 Microsoft online 帳號**。  
  
     **加入 Microsoft Online Services 帳號**頁面精靈的顯示所有使用者帳號，不需要的 Microsoft online account。 根據預設，選取所有帳號和的使用者名稱建議的 Microsoft online id 如果連結到您的 Office 365 裝機費自訂網際網路網域，將會預設使用的網域。  
  
4.  在**加入 Microsoft Online Services 帳號**頁面上，檢視帳號，才會顯示。 例如，使用者已經有不同的 online ID online 帳號，請確定已選取您想要使用的電子郵件地址網域檢查。 當您完成進行所需的變更，請按一下**下一步**。  
  
5.  在**指派 Microsoft Online Services 的授權**頁面上，選取 Office 365 服務會使用您的使用者。 當您按下**下一步**，將會開始 account 建立。  
  
    > [!NOTE]
    >  每個 online 帳號，您必須 Office 365 中有服務授權。 您可以查看可用授權，如果需要請打開您裝機費新增授權的**Office 365**頁面上儀表板。  
  
6.  通知的使用者，現在可以 Microsoft online account。 它們必須變更其網路使用者密碼，然後可以登入 Office 365。 適用於的指示，請查看[開始使用新的 Microsoft online account](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>若要開始使用新的 Microsoft online account  
  
1.  登入您的網路使用者的過去與您電腦上。  
  
2.  變更您的使用者 account 的密碼。 若要開始，按下 Ctrl + Alt + Delete，並按一下 [**變更密碼**。  
  
     當您變更您的密碼時，您的新 online 帳號同步處理的密碼。 您現在可以使用相同的密碼來登入 Office 365。  
  
3.  [登入 Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out)使用您新的 online ID 和您的使用者 account 密碼。  
  
    > [!IMPORTANT]
    >  請勿變更 Office 365 您 online 的密碼。 這將會使密碼同步。 您可以變更密碼網路使用者洽詢您的每次將更新 online 密碼。  
  
###  <a name="BKMK_ToImportUserAccounts"></a>從您現有的 online 帳號匯入帳號  
  
1.  儀表板，請打開**使用者**頁面。  
  
2.  在**使用者工作**，按一下 [**的 Office 365 匯入帳號**。  
  
     下一個頁面會顯示所有 online 帳號適用於您的 Office 365 裝機費不具有帳號，在伺服器上。 根據預設，選取所有帳號，並 online ID 建議的使用者名稱。  
  
3.  若要建立的使用者帳號：  
  
    1.  請建議的帳號所需的任何變更。  
  
    2.  或者按一下連結以檢視可指派給使用者帳號暫時密碼。 您將需要提供您的使用者 account 其名稱以及其臨時密碼。  
  
         (建立帳號之後，就會發現此檔案中列出這些密碼：*系統磁碟機*\Users\\*Office365admin*\\*NewServerUser*.txt 位置*Office365admin*網路帳號，可用來管理的伺服器上的 Office 365 和*NewServerUser*是新的使用者名稱 account。)  
  
    3.  按一下**下一步**若要建立的使用者帳號。  
  
4.  可讓您知道新使用者的帳號，其將用來登入，第一次 œ 的伺服器上，它們會需要變更密碼後他們登入臨時密碼的使用者。 適用於您的使用者的指示，請查看[開始使用新的 Microsoft online account](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
     請務必他們知道與他們帳號，將會同步他們 online account 的密碼，以及他們應該不會變更 online 中 Office 365 密碼。  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>若要建立新的使用者 account 指派給其 online 帳號  
  
1.  儀表板，請按一下**使用者**。  
  
2.  在**使用者工作**，按一下 [**新增使用者帳號**。 [新增使用者 Account 精靈會出現。  
  
3.  請依照下列指示來建立的使用者帳號。  
  
4.  在**指派的 Microsoft Online Services 帳號**頁面上，請建立新 online 的使用者或指派的現有 online account:  
  
    -   建立新的 online 帳號，請按**建立新的 Microsoft Online Services 帳號，並將它指派給此帳號**，然後輸入名稱的 Microsoft Online Services account（預設的使用者名稱用來 online ID）。 然後按一下**下一步**。  
  
    -   若要指定現有的 Microsoft online 帳號，請按一下**將現有的 Microsoft Online Services 帳號指派給此帳號**，從下拉式清單中選取現有的帳號，並。 然後按一下**下一步**。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 時，Microsoft Online Services 帳號稱為精靈標籤儀表板中的 Office 365 帳號。  
  
5.  請依照下列指示完成精靈。  
  
6.  通知的使用者，他們必須將其使用者密碼之前，就可以登入 Office 365 的新 online 帳號。 適用於的指示，請查看[開始使用新的 Microsoft online account](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>若要將 online account 指派給使用者 account  
  
1.  儀表板，請按一下**使用者**。  
  
2.  在清單中，帳號上按一下滑鼠右鍵，然後按一下**指派的 Microsoft online account**。 指派 Microsoft Online Services Account 精靈會出現。  
  
3.  指派現有 online 帳號，或建立新的使用者。 新的帳號預設 online ID 為使用者名稱。 然後按一下**下一步**將 online account 新增至帳號。  
  
4.  檢視精靈的最後一頁上的資訊，然後按一下**關閉**。  
  
5.  通知的使用者，他們必須將其使用者密碼之前，就可以登入 Office 365 的新 online 帳號。 適用於的指示，請查看[開始使用新的 Microsoft online account](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>管理 online 帳號  
 當您在 Windows Server Essentials 使用者 account 新增 online 帳號時，您可以在 account s 週期一起管理這兩個帳號。  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>了解 online account 狀態  
 當您使用者過去指派的 Microsoft Online Services 帳號時，帳號的電子郵件地址就會出現在**Microsoft online account**欄**使用者**頁面的儀表板。 (在 Windows Server Essentials，欄標籤是**Office 365 account**。)  
  
-   電子郵件地址旁邊的藍色圖示，表示 online account 為作用中。 是的 account 已目前的 Office 365 授權，並使用者可以使用 online ID 登入 Office 365。  
  
-   電子郵件地址旁邊的灰色的圖示表示 online account 非使用中 œ 可能是因為不再使用的授權或 online account 已 \ [尚未指派。 當您取消指派使用者 s online 帳號時，移除授權，並封鎖使用者從登入 Office 365 使用 account。 不過，伺服器會保留使用者名稱和 Office 365 電子郵件地址之間的對應。  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>只存取 online account  
 如果您要限制使用者的存取您的 Office 365 服務或使用者離開您的組織，您有哪些進行？ 如果您管理您的使用者 online 帳號，以及在 Windows Server Essentials 使用者的帳號，您有三個選項：  
  
-   **取消指派 online account**嗎？如果您想要讓使用者使用 Office 365，而不會防止存取伺服器上的資源，您必須先取消指派 online account。 將推出的 Office 365 授權，並封鎖使用者從登入 Office 365。 不過，伺服器會保留使用者名稱和 Office 365 電子郵件地址之間的對應。 指示，請查看[以取消 online account 指派使用者 account 的](#BKMK_ToUnassignAnOnlineAccount)。  
  
-   **停用帳號**嗎？如果因為員工離開永久，或是暫時停用帳號使用者 s online 帳號也已停用。 無法使用 online 帳號，但使用者資料，包括電子郵件會保留在 Microsoft Online Services。 適用於的指示，請查看[停用帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)在[管理使用者帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
-   **移除帳號**嗎？如果您移除帳號，online account 也會移除 Microsoft Online Services 的。 適用於的指示，請查看[移除帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)在[管理使用者帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
    > [!WARNING]
    >  請注意 online account 移除後，使用者資料的受到資料保留的 Microsoft Online Services 的原則。 如果您需要員工離開之後人員的使用者資料的保留，停用帳號，而不是將它移除。  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>若要從使用者帳號取消指派 online account  
  
1.  儀表板，請按一下**使用者**。  
  
2.  在清單中，帳號上按一下滑鼠右鍵，然後按一下**取消指派的 Microsoft online account** (在 Windows Server Essentials，按一下 [**取消指派 Office 365 account**)。  
  
3.  在 [確認命令提示字元中，按一下 [ **[是]**。  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>管理適用於 Online 換貨的電子郵件地址  
 將電子郵件地址新增到 Windows Server Essentials 中的使用者 s online 帳號，您可以讓使用者 Online Exchange 接收電子郵件在多個電子郵件地址。  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>將其他電子郵件地址新增到 Microsoft online account s 使用者  
  
1.  Windows Server Essentials 儀表板，請按一下**使用者**。  
  
2.  在清單中，帳號上按一下滑鼠右鍵，然後按一下**檢視 account 屬性**。  
  
3.  在**Microsoft online**屬性 account] 索引標籤 (或**Office 365**索引標籤，在 Windows Server Essentials)，按一下 [**新增**。  
  
4.  輸入新的電子郵件別名，，然後選取 [電子郵件網域。  
  
5.  按一下**[確定]**兩次。  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>管理通訊群組的換貨 Online (僅限 Windows Server Essentials)  
 您可以將您的 Windows Server Essentials 伺服器整合使用 Office 365 之後，您可以建立及管理通訊群組的換貨 Online 從 Windows Server Essentials 儀表板。 您市上執行此動作**群組 Distribution**索引標籤，新增至**使用者**頁面。 如果您的換貨 Online 裝機費，您只會看到這個] 索引標籤。 此功能不適用於 Windows Server Essentials。  
  
 使用下列程序:  
  
-   [新增 distribution 群組](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [變更 distribution 群組成員](#BKMK_ChangeGroupMembers)  
  
-   [變更使用者 s distribution 群組成員資格](#BKMK_EditUserMemberships)  
  
-   [移除 distribution 群組](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>若要新增的通訊群組  
  
1.  在 Windows Server Essentials 儀表板上，按一下 [**使用者**，然後按一下 [**通訊群組**索引標籤。  
  
2.  在**Distribution 群組工作**，按一下 [**新增 distribution 群組**。  
  
     新增新的 Distribution 群組精靈會出現。  
  
3.  在**新增 distribution 群組**頁面上，輸入下列資訊，然後按一下 [**下**:  
  
    -   輸入群組名稱、選擇性的描述和電子郵件別名新 distribution 群組。  
  
    -   根據預設，distribution 群組可以接收電子郵件從外您的組織連絡人。 如果您不想讓這，清除該選項。  
  
4.  在**新增群組成員**頁面上，使用**新增]**按鈕新增已指派給，online account 使用帳號，並新 distribution 群組其他散發群組。 然後按一下**下一步**。  
  
     新的通訊群組建立 Exchange Online。  
  
###  <a name="BKMK_ChangeGroupMembers"></a>若要變更 distribution 群組成員  
  
1.  儀表板中，按一下 [**使用者**，然後按一下 [**通訊群組**索引標籤。  
  
2.  在清單中，distribution 群組上按一下滑鼠右鍵，然後按一下**[變更成員資格**。  
  
3.  使用**新增**和**移除**按鈕來新增或移除 distribution 群組中使用 online 帳號。 然後按一下**下一步**來更新中換貨 Online 通訊群組成員資格。  
  
###  <a name="BKMK_EditUserMemberships"></a>若要變更使用者 s distribution 群組成員資格  
  
1.  儀表板，請按一下**使用者**。  
  
2.  在清單中，帳號上按一下滑鼠右鍵，然後按一下**檢視 account 屬性**。  
  
3.  在使用者 account 屬性中，按一下 [**群組 Distribution**索引標籤，然後按一下 [**編輯**。  
  
4.  在**編輯群組成員資格**方塊中，使用**新增]**和**移除**按鈕來新增或移除帳號，distribution 群組，然後按一下**關閉**。  
  
5.  按一下**[確定]**儲存更新的使用者 account 屬性。  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>若要移除的通訊群組  
  
1.  儀表板中，按一下 [**使用者**，然後按一下 [**通訊群組**索引標籤。  
  
2.  在清單中，distribution 群組上按一下滑鼠右鍵，然後按一下**群組中移除**。  
  
3.  在 [確認命令提示字元中，按一下 [**群組 Delete**。  
  
     從換貨 Online 移除 distribution 群組。  
  
## <a name="see-also"></a>也了  
  
-   [管理使用者帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
