---
title: 管理 Windows Server Essentials 使用者的線上帳戶
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824089"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>管理 Windows Server Essentials 使用者的線上帳戶

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

當您整合您的 Windows Server Essentials 伺服器與 Microsoft Office 365 時，您可以從儀表板來管理您的線上帳戶以及使用者帳戶。 本主題中，您將了解您可以取得儀表板管理您的使用者 Microsoft Online Services 帳戶、 如何建立及管理線上帳戶，從儀表板，以及如何管理 Exchange online 的電子郵件地址與通訊群組從儀表板。  

  
> [!NOTE]
>  若要管理您的 Microsoft Online Services 帳戶，在 Windows Server Essentials 中，您必須與 Office 365 整合您的伺服器。 如需相關指示，請參閱 <<c0> [ 管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)。  
  
> [!IMPORTANT]
>  如果您管理 Windows Server Essentials 中的線上帳戶，您習慣看到 Microsoft Online Services 帳戶被稱為*Office 365 帳戶*。 在 Windows Server Essentials 儀表板，標籤已變更為*Microsoft Online Services 帳戶*，或*Microsoft 線上帳戶*簡稱。 帳戶與程序皆相同；只有標籤做了變更。 本主題中的大部分程序使用「線上帳戶」 這個詞彙。  
  
## <a name="in-this-topic"></a>本主題內容  
  
-   [為什麼應該管理我的線上帳戶的儀表板？](#BKMK_WhyManageOnlineAccounts)  
  
-   [建立線上帳戶](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [管理線上帳戶](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [管理 Exchange Online 電子郵件地址](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [管理 Exchange Online 通訊群組](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a> 為什麼應該管理我的線上帳戶的儀表板？  
 當您使用儀表板將 Microsoft Online Services 帳戶指派給使用者帳戶時，會自動同步處理的帳戶密碼，並可以維護兩個帳戶一起在使用者帳戶 s 生命週期。  
  
 它方便的使用者，可以使用相同的密碼，才能存取資源，在伺服器上，在 Office 365 中的 s。 而且您可以套用相同的密碼需求，以存取內部資源所需的 Office 365 中的資源。  
  
### <a name="how-does-password-synchronization-work"></a>密碼同步化的運作方式？  
 當您使用儀表板將 Microsoft Online Services 帳戶指派給使用者帳戶時，使用者帳戶密碼會自動同步處理的使用者 s 線上帳戶。 這表示使用者只需要單一密碼，才能存取這兩個資源的伺服器上，並在 Office 365 中。 此外，您可以使用相同名稱的使用者帳戶和使用者 s 線上識別碼。  
  
 當使用者從已加入網域的電腦或使用遠端 Web 存取變更其使用者帳戶的密碼時，密碼同步處理便會立即自動進行。  
  
> [!IMPORTANT]
>  如果 Office 365 與 Windows Server Essentials 整合，使用者應該不會從 Office 365 入口網站變更 Microsoft 線上帳戶的密碼。 這樣會中斷密碼同步化。  
  
### <a name="simplified-account-creation"></a>簡化的帳戶建立  
 該處 s 另一個優點，當您從儀表板建立初始線上帳戶。 您可以藉由單一動作，為您所有的使用者建立線上帳戶。 相反地，如果您的員工已經使用 Office 365 和您設定新的 Windows Server Essentials 伺服器，您可以建立所有的使用者帳戶從線上帳戶，以單一動作。 如需詳細資訊，請參閱 [建立線上帳戶](#BKMK_SECTION_CreateOnlineAccounts)。  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>從儀表板管理電子郵件地址與通訊群組  
 您能夠從儀表板管理 Exchange Online 電子郵件地址與通訊群組。 而且，您可以使用您的組織的網際網路網域，在 電子郵件地址。 您可以從儀表板中，執行所有這些登入 Office 365。 （您必須使用 Windows Server Essentials 儀表板管理通訊群組。 這項功能不支援 Windows Server Essentials 中。）如需詳細資訊，請參閱[管理 Exchange Online 電子郵件地址](#BKMK_SECTION_ManageEmailAddresses)與[管理 Exchange Online 通訊群組](#BKMK_SECTION_ManageDistributionGroups)。  
  
### <a name="manage-the-user-account-and-online-account-together"></a>同時管理使用者帳戶與線上帳戶  
 而且您可以管理線上帳戶以及整個帳戶 s 生命週期中的使用者帳戶。 如果您停用使用者帳戶，Microsoft Online Services 中的線上帳戶也會停用。 如果您移除使用者帳戶，線上帳戶也會移除。 如需詳細資訊，請參閱[管理線上帳戶](#BKMK_SECTION_ManageOnlineAccounts)。  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a> 建立線上帳戶  
 與 Office 365 整合您的伺服器之後它 s 優點來建立 Microsoft Online Services 帳戶為您的儀表板的使用者。 您會有很大的彈性建立線上帳戶。 如果您有新的 Office 365 訂用帳戶，您可以大量建立線上帳戶的所有使用者。 如果您已經在 Office 365 中建立您的線上帳戶，不擔心。 如果您設定新的伺服器，您可以建立您的使用者帳戶在伺服器上匯入線上帳戶。 您可以指派新的或現有的線上帳戶建立個別的使用者帳戶時或當您將線上帳戶新增至現有的使用者帳戶。  
  
 **授權需求**您必須針對每個您所建立的線上帳戶的使用者授權。 請檢查**Office 365**若要查看有多少使用者授權可透過 Office 365 訂用帳戶儀表板上的頁面。 如果您需要新增更多使用者授權，您將能夠在 Office 365，若要這樣做，開啟您的 Office 365 訂用帳戶。  
  
 **使用者後續動作**加入線上帳戶的使用者帳戶之後，使用者需要的下次登入時變更其使用者帳戶的密碼。 新密碼會立即與其線上帳戶同步處理。 在那之後，使用者可以使用密碼來登入 Office 365，並提供其線上識別碼。  
  
> [!IMPORTANT]
>  提醒您的使用者應該永遠不會變更線上帳戶密碼在 Office 365 中。 每當他們變更使用者帳戶的密碼，線上帳戶密碼會自動變更。 如果他們變更 Office 365 中的線上密碼，密碼同步處理將會中斷。  
  
 請使用本節中的程序來：  
  
-   [您現有的使用者帳戶的大量建立線上帳戶的](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [從您的 Microsoft Online Services 帳戶匯入的使用者帳戶](#BKMK_ToImportUserAccounts)  
  
-   [建立新的使用者帳戶，線上帳戶指派給它](#BKMK_ToCreateaNewUserAccount)  
  
-   [將線上帳戶指派給使用者帳戶](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  如果您使用 Windows Server Essentials，您會看到*Office 365 帳戶*而不是*Microsoft Online Services 帳戶*在整個這些程序。 此程序相同，但是在 Windows Server Essentials 中變更的術語。  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a> 若要大量建立線上帳戶，為您現有的使用者帳戶  
  
1.  身為管理員，在伺服器上登入，然後開啟 Windows Server Essentials 儀表板。  
  
2.  在儀表板上，開啟 [使用者] 頁面。  
  
3.  在 [使用者作業] 中，按一下 [新增 Microsoft 線上帳戶]。  
  
     [新增 Microsoft Online Services 帳戶]  精靈頁面會顯示沒有 Microsoft 線上帳戶的所有使用者帳戶。 依預設，會選取所有的帳戶，並為 Microsoft 線上識別碼建議使用者名稱。 如果您已在 Office 365 訂用帳戶連結了自訂的網際網路網域，依預設會使用該網域。  
  
4.  在 [新增 Microsoft Online Services 帳戶]  頁面上，檢視將建立的帳戶。 例如，檢查已經有不同線上識別碼之線上帳戶的使用者，並確定已選取您想在電子郵件地址中使用的網域。 當您完成任何所需的變更時，按一下 [下一步] 。  
  
5.  在 **指派 Microsoft Online Services 授權**頁面上，選取您的使用者將使用的 Office 365 服務。 當您按一下 [下一步]，就會開始建立帳戶。  
  
    > [!NOTE]
    >  您必須有 Office 365 中的服務授權，每個線上帳戶。 您可以從儀表板上的 [Office 365]  頁面檢查可用的授權，並在必要時開啟您的訂閱以新增授權。  
  
6.  請通知使用者他們已擁有 Microsoft 線上帳戶。 他們必須變更網路使用者帳戶密碼，才可以登入 Office 365。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a> 若要開始使用新的 Microsoft 線上帳戶  
  
1.  以網路使用者帳戶登入您的電腦。  
  
2.  變更您的使用者帳戶密碼。 若要開始，請按 Ctrl + Alt + Delete，並按一下 [變更密碼] 。  
  
     當您變更密碼時，密碼會與新的線上帳戶同步處理。 您現在可以使用相同的密碼來登入 Office 365。  
  
3.  請使用新的線上識別碼和使用者帳戶密碼[登入 Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) 。  
  
    > [!IMPORTANT]
    >  不變更您在 Office 365 中的線上帳戶密碼。 這樣會中斷密碼同步化。 您的線上密碼會在每次您更新網路使用者帳戶密碼時進行更新。  
  
###  <a name="BKMK_ToImportUserAccounts"></a> 若要從現有的線上帳戶匯入的使用者帳戶  
  
1.  在儀表板上，開啟 [使用者] 頁面。  
  
2.  在 [使用者作業] 中，按一下 [從 Office 365 匯入帳戶] 。  
  
     下一個頁面會顯示所有線上帳戶的 Office 365 訂用帳戶，不需要在伺服器上的使用者帳戶。 依預設，會選取所有帳戶，並為使用者名稱建議線上識別碼。  
  
3.  若要建立使用者帳戶：  
  
    1.  對任何建議的使用者帳戶進行所需的變更。  
  
    2.  (選擇性) 按一下連結以檢視指派給使用者帳戶的暫時密碼。 您需要提供使用者暫時密碼以及他們的新帳戶名稱。  
  
         (您建立帳戶之後，您就可以找到此檔案中列出這些密碼：*SystemDrive*\Users\\*Office365admin*\\*NewServerUser*.txt 其中*Office365admin*是網路帳戶用來管理伺服器上的 Office 365 及*NewServerUser*是新的使用者帳戶名稱。)  
  
    3.  按一下 [下一步] 建立使用者帳戶。  
  
4.  讓使用者知道他們新的使用者帳戶，以及它們將用以登入第一次 œ 的伺服器上，而且他們將不必之後在登入時變更密碼的暫時密碼。 如需使用者的相關指示，請參閱 [開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
     請確定他們知道將同步處理的密碼與其線上帳戶，接下來，其使用者帳戶，他們不應該變更線上密碼在 Office 365 中。  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a> 若要建立新的使用者帳戶，線上帳戶指派給它  
  
1.  在儀表板上，按一下 [使用者]。  
  
2.  在 [使用者工作] 中，按一下 [新增使用者帳戶]。 [新增使用者帳戶精靈] 會隨即出現。  
  
3.  請依照下列指示來建立使用者帳戶。  
  
4.  在 [指派 Microsoft Online Services 帳戶] 頁面上，請為使用者建立新的線上帳戶，或是指派現有的線上帳戶：  
  
    -   若要建立新的線上帳戶，請按一下 [建立新的 Microsoft Online Services 帳戶並指派給此使用者帳戶] ，然後輸入 Microsoft Online Services 帳戶名稱 (依預設，使用者名稱會作為線上識別碼)。 然後按一下 [下一步] 。  
  
    -   若要指派現有的 Microsoft 線上帳戶，請按一下 [將現有的 Microsoft Online Services 帳戶指派給此使用者帳戶] ，並從下拉式清單中選取現有的帳戶。 然後按一下 [下一步] 。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，Microsoft Online Services 帳戶稱為精靈和儀表板的標籤中的 Office 365 帳戶。  
  
5.  遵循指示以完成精靈。  
  
6.  請通知使用者在使用新的線上帳戶登入 Office 365 之前，需要變更其使用者帳戶密碼。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>若要將線上帳戶指派給使用者帳戶  
  
1.  在儀表板上，按一下 [使用者]。  
  
2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [指派 Microsoft 線上帳戶] 。 [指派 Microsoft Online Services 帳戶精靈] 隨即出現。  
  
3.  請指派現有的線上帳戶或為使用者建立新的帳戶。 新帳戶的預設線上識別碼是使用者名稱。 然後按一下 [下一步]  將線上帳戶新增到使用者帳戶。  
  
4.  檢視精靈最後一頁上的資訊，然後按一下 [關閉] 。  
  
5.  請通知使用者在使用新的線上帳戶登入 Office 365 之前，需要變更其使用者帳戶密碼。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a> 管理線上帳戶  
 當您將線上帳戶加入 Windows Server Essentials 中的使用者帳戶時，您可以在整個帳戶 s 生命週期一起管理這兩個帳戶。  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a> 了解線上帳戶狀態  
 當您將 Microsoft Online Services 帳戶指派給使用者帳戶，帳戶的電子郵件地址會出現在儀表板 [使用者]  頁面上的 [Microsoft 線上帳戶]  欄中。 (在 Windows Server Essentials 中，是在資料行標籤**Office 365 帳戶**。)  
  
-   電子郵件地址旁的藍色圖示表示線上帳戶作用中。 也就是帳戶目前的 Office 365 授權，而且使用者可以使用該線上識別碼登入 Office 365。  
  
-   電子郵件地址旁邊的灰色的圖示表示線上帳戶非使用中的 œ 可能是因為的授權不再作用中或線上帳戶已被取消指派。 當您取消指派使用者 s 線上帳戶時，授權會被移除，並會封鎖使用者進行登入 Office 365 使用的帳戶。 不過，伺服器會維護使用者帳戶名稱和 Office 365 電子郵件地址之間的對應。  
  
###  <a name="BKMK_UnassignOnlineAccount"></a> 限制存取線上帳戶  
 如果使用者離開組織，或您想要限制使用者的存取 Office 365 服務您該怎麼辦？ 如果您管理使用者線上帳戶以及他們在 Windows Server Essentials 中的使用者帳戶，您有三個選項：  
  
-   **取消指派線上帳戶**嗎？如果您想要讓使用者使用 Office 365 而又可以存取伺服器上的資源，您應該取消指派線上帳戶。 Office 365 授權就會釋出，並會封鎖使用者進行登入 Office 365。 不過，伺服器會維護使用者帳戶名稱和 Office 365 電子郵件地址之間的對應。 如需相關指示，請參閱 <<c0> [ 若要取消指派線上帳戶的使用者帳戶](#BKMK_ToUnassignAnOnlineAccount)。  
  
-   **停用使用者帳戶**嗎？如果您停用使用者帳戶，因為員工離開，暫時或永久，使用者 s 線上帳戶也會停用。 線上帳戶將無法繼續使用，但是使用者資料 (包括電子郵件) 會保留在 Microsoft Online Services 中。 如需相關指示，請參閱 <<c0> [ 停用使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)中[Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
-   **移除使用者帳戶**嗎？如果您移除使用者帳戶，線上帳戶也會移除從 Microsoft Online Services。 如需相關指示，請參閱 <<c0> [ 移除使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)中[Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md)。  
  
    > [!WARNING]
    >  請注意當移除線上帳戶時，使用者資料會遵循 Microsoft Online Services 的資料保留原則。 如果您需要保留人員的使用者資料，在員工離開之後，停用使用者帳戶，而不是移除它。  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a> 若要取消指派線上帳戶的使用者帳戶  
  
1.  在儀表板上，按一下 [使用者]。  
  
2.  以滑鼠右鍵按一下 在清單中，使用者帳戶，然後按一下**取消指派 Microsoft 線上帳戶**(在 Windows Server Essentials 中，按一下**取消指派 Office 365 帳戶**)。  
  
3.  在確認提示按一下 [是] 。  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a> 管理 Exchange Online 電子郵件地址  
 將電子郵件地址新增到 Windows Server Essentials 中的使用者 s 線上帳戶，您可以允許使用者接收來自多個電子郵件地址的電子郵件在 Exchange Online。  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a> 若要將其他電子郵件地址新增至使用者的 Microsoft 線上帳戶  
  
1.  在 Windows Server Essentials 儀表板，按一下 **使用者**。  
  
2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [檢視帳戶內容] 。  
  
3.  在上**Microsoft online**帳戶內容] 索引標籤 (或**Office 365** Windows Server Essentials 中的索引標籤)，按一下 [**新增**。  
  
4.  輸入新的電子郵件別名，然後選取電子郵件網域。  
  
5.  按兩次 **[確定]** 。  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a> 管理 Exchange Online (僅 Windows Server Essentials) 通訊群組  
 與 Office 365 整合您的 Windows Server Essentials 伺服器之後，您可以建立並管理 Exchange Online 的 Windows Server Essentials 儀表板發佈群組。 您將執行這項操作上**發佈群組**索引標籤加入至**使用者**頁面。 只有在訂閱 Exchange Online 的情況下，您才會看到這個索引標籤。 無法在 Windows Server Essentials 中使用這項功能。  
  
 請使用下列程序來：  
  
-   [新增通訊群組](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [變更通訊群組的成員](#BKMK_ChangeGroupMembers)  
  
-   [變更使用者的通訊群組成員資格](#BKMK_EditUserMemberships)  
  
-   [移除通訊群組](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a> 若要新增通訊群組  
  
1.  在 Windows Server Essentials 儀表板，按一下**使用者**，然後按一下**發佈群組** 索引標籤。  
  
2.  在 [通訊群組工作] 中，按一下 [新增通訊群組]。  
  
     [新增新的通訊群組精靈] 會隨即出現。  
  
3.  在 [新增新的通訊群組] 頁面上，輸入下列資訊，然後按一下 [下一步]：  
  
    -   輸入新通訊群組的群組名稱、選擇性描述及電子郵件別名。  
  
    -   依預設，通訊群組可以接收來自公司外部之人員的電子郵件。 如果您不允許這麼做，請清除該選項。  
  
4.  在 [新增群組成員] 頁面上，使用 [新增] 按鈕，將已指派線上帳戶的作用中使用者帳戶以及其他通訊群組，新增到新的通訊群組。 然後按一下 [下一步] 。  
  
     新的通訊群組會建立在 Exchange Online 中。  
  
###  <a name="BKMK_ChangeGroupMembers"></a> 若要變更通訊群組的成員  
  
1.  在儀表板上，按一下 [使用者]，然後按一下 [通訊群組] 索引標籤。  
  
2.  在清單中的通訊群組上按一下滑鼠右鍵，然後按一下 [變更群組成員資格] 。  
  
3.  使用 [新增]  與 [移除]  按鈕，將作用中線上帳戶新增到通訊群組中，或從通訊群組中移除。 然後按一下 [下一步] 更新 Exchange Online 中的通訊群組成員資格。  
  
###  <a name="BKMK_EditUserMemberships"></a> 若要變更使用者的通訊群組成員資格  
  
1.  在儀表板上，按一下 [使用者]。  
  
2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [檢視帳戶內容] 。  
  
3.  在使用者帳戶內容中，按一下 [通訊群組] 索引標籤，然後按一下 [編輯]。  
  
4.  在 [編輯群組成員資格] 方塊中，使用 [新增] 與 [移除] 按鈕，將通訊群組新增到使用者帳戶中，或從使用者帳戶中移除，然後按一下 [關閉]。  
  
5.  按一下 [確定] 以儲存更新的使用者帳戶內容。  
  
###  <a name="BKMK_RemoveDistributionGroup"></a> 若要移除通訊群組  
  
1.  在儀表板上，按一下 [使用者]，然後按一下 [通訊群組] 索引標籤。  
  
2.  在清單中的通訊群組上按一下滑鼠右鍵，然後按一下 [移除群組]。  
  
3.  在確認提示按一下 [刪除群組] 。  
  
     通訊群組會從 Exchange Online 移除。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [管理 Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
