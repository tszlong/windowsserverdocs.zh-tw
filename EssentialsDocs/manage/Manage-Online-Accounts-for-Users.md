---
title: 管理 Windows Server Essentials 使用者的線上帳戶
description: 瞭解如何從 Windows Server Essentials 儀表板管理您的線上帳戶以及使用者帳戶。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 684afa6d17e4be5282e1596244689a501ffaf5be
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811345"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>管理 Windows Server Essentials 使用者的線上帳戶

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

當您將 Windows Server Essentials 伺服器與 Microsoft 365 整合時，您可以從儀表板管理您的線上帳戶以及使用者帳戶。 在本主題中，您將瞭解如何從儀表板管理使用者的 Microsoft Online Services 帳戶、如何從儀表板建立及管理線上帳戶，以及如何從儀表板管理 Exchange Online 的電子郵件地址和通訊群組。


> [!NOTE]
>  若要在 Windows Server Essentials 中管理您的 Microsoft Online Services 帳戶，您必須將伺服器與 Microsoft 365 整合。 如需相關指示，請參閱 [管理 Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md)。

> [!IMPORTANT]
>  如果您在 Windows Server Essentials 中管理線上帳戶，您習慣看到稱為 *Microsoft 365 帳戶* 的 Microsoft online Services 帳戶。 在 Windows Server Essentials 的儀表板上，標籤已變更為 *Microsoft Online Services 帳戶*，或簡短的 *microsoft 線上帳戶* 。 帳戶與程序皆相同；只有標籤做了變更。 本主題中的大部分程序使用「線上帳戶」這個詞彙。

## <a name="in-this-topic"></a>本主題內容

-   [為什麼應該從儀表板管理我的線上帳戶？](#BKMK_WhyManageOnlineAccounts)

-   [建立線上帳戶](#BKMK_SECTION_CreateOnlineAccounts)

-   [管理線上帳戶](#BKMK_SECTION_ManageOnlineAccounts)

-   [管理 Exchange Online 電子郵件地址](#BKMK_SECTION_ManageEmailAddresses)

-   [管理 Exchange Online 通訊群組](#BKMK_SECTION_ManageDistributionGroups)

##  <a name="why-should-i-manage-my-online-accounts-from-the-dashboard"></a><a name="BKMK_WhyManageOnlineAccounts"></a> 為什麼我應該從儀表板管理我的線上帳戶？
 當您使用儀表板將 Microsoft Online Services 帳戶指派給使用者帳戶時，系統會自動同步處理帳戶密碼，而且您可以在整個使用者帳戶的生命週期中同時維護兩個帳戶。

 這對使用者來說很方便，而使用者可以使用相同的密碼來存取伺服器和 Microsoft 365 中的資源。 您可以套用相同的密碼需求，以存取內部資源所需 Microsoft 365 中的資源。

### <a name="how-does-password-synchronization-work"></a>密碼同步化的運作方式？
 當您使用儀表板將 Microsoft Online Services 帳戶指派給使用者帳戶時，使用者帳戶的密碼會自動與使用者的線上帳戶同步處理。 這表示使用者只需要單一密碼，即可存取伺服器和 Microsoft 365 中的資源。 此外，您可以針對使用者帳戶和使用者的線上識別碼使用相同的名稱。

 當使用者從已加入網域的電腦或使用遠端 Web 存取變更其使用者帳戶的密碼時，密碼同步處理便會立即自動進行。

> [!IMPORTANT]
>  如果 Microsoft 365 與 Windows Server Essentials 整合，則使用者不應該從 Microsoft 365 入口網站變更其 Microsoft online 帳戶的密碼。 這樣會中斷密碼同步化。

### <a name="simplified-account-creation"></a>簡化的帳戶建立
 當您從儀表板建立初始線上帳戶時，還有另一個優點。 您可以藉由單一動作，為您所有的使用者建立線上帳戶。 另一方面，如果您的員工正在使用 Microsoft 365，且您設定了新的 Windows Server Essentials 伺服器，則可以使用單一動作從線上帳戶建立您所有的使用者帳戶。 如需詳細資訊，請參閱[建立線上帳戶](#BKMK_SECTION_CreateOnlineAccounts)。

### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>從儀表板管理電子郵件地址與通訊群組
 您能夠從儀表板管理 Exchange Online 電子郵件地址與通訊群組。 而且，您可以在電子郵件地址中使用您組織的網際網路網域。 您可以從儀表板進行所有這項作業，而不需登入 Microsoft 365。  (您必須使用 Windows Server Essentials 從儀表板管理通訊群組。 這項功能在 Windows Server Essentials 中不受支援 ) 。如需詳細資訊，請參閱 [管理 Exchange online 的電子郵件地址](#BKMK_SECTION_ManageEmailAddresses) ，以及 [管理 exchange online 的通訊群組](#BKMK_SECTION_ManageDistributionGroups)。

### <a name="manage-the-user-account-and-online-account-together"></a>同時管理使用者帳戶與線上帳戶
 您也可以在整個帳戶的生命週期中，管理線上帳戶以及使用者帳戶。 如果您停用使用者帳戶，Microsoft Online Services 中的線上帳戶也會停用。 如果您移除使用者帳戶，線上帳戶也會移除。 如需詳細資訊，請參閱[管理線上帳戶](#BKMK_SECTION_ManageOnlineAccounts)。

##  <a name="create-online-accounts"></a><a name="BKMK_SECTION_CreateOnlineAccounts"></a> 建立線上帳戶
 將您的伺服器與 Microsoft 365 整合之後，您可以從 [儀表板] 為您的使用者建立 Microsoft Online Services 帳戶。 您在建立線上帳戶時有很大的彈性。 如果您有新的 Microsoft 365 訂用帳戶，您可以為所有使用者大量建立線上帳戶。 如果您已在 Microsoft 365 中建立線上帳戶，請不要擔心。 如果您設定新的伺服器，您可以匯入線上帳戶，在伺服器上建立使用者帳戶。 您可以在建立個別使用者帳戶時，或是在將線上帳戶新增至現有的使用者帳戶時，指派新的或現有的線上帳戶。

 **授權需求** 您所建立的每個線上帳戶都需要使用者授權。 查看儀表板上的 [ **Microsoft 365** ] 頁面，以查看有多少使用者授權可透過您的 Microsoft 365 訂用帳戶取得。 如果您需要新增更多的使用者授權，您可以在 Microsoft 365 中開啟您的 Microsoft 365 訂用帳戶來進行這項作業。

 **使用者後續操作** 當您新增使用者帳戶的線上帳戶之後，使用者下次登入時必須變更其使用者帳戶的密碼。 新密碼會立即與其線上帳戶同步處理。 之後，他們就可以使用密碼，以線上識別碼登入 Microsoft 365。

> [!IMPORTANT]
>  請強調您的使用者，他們絕對不應該在 Microsoft 365 中變更他們的線上帳戶密碼。 每當他們變更使用者帳戶的密碼，線上帳戶密碼會自動變更。 如果他們變更 Microsoft 365 中的線上密碼，密碼同步化將會中斷。

 請使用本節中的程序來：

-   [為現有使用者帳戶大量建立線上帳戶](#BKMK_ToBulkCreateOnlineAccounts)

-   [從 Microsoft Online Services 帳戶匯入使用者帳戶](#BKMK_ToImportUserAccounts)

-   [建立已指派線上帳戶的新使用者帳戶](#BKMK_ToCreateaNewUserAccount)

-   [將線上帳戶指派給使用者帳戶](#BKMK_ToAssignAnOnlineAccount)

> [!NOTE]
>  如果您使用 Windows Server Essentials，您將會在這些程式中看到 *Microsoft 365 的帳戶* ，而不是 *Microsoft Online Services 帳戶* 。 程式相同，但 Windows Server Essentials 中的術語已變更。

###  <a name="to-bulk-create-online-accounts-for-your-existing-user-accounts"></a><a name="BKMK_ToBulkCreateOnlineAccounts"></a> 若要為現有的使用者帳戶大量建立線上帳戶

1.  以系統管理員身分登入伺服器，然後開啟 [Windows Server Essentials 儀表板]。

2.  在儀表板上，開啟 [使用者] 頁面。

3.  在 [使用者作業] 中，按一下 [新增 Microsoft 線上帳戶]。

     [新增 Microsoft Online Services 帳戶] 精靈頁面會顯示沒有 Microsoft 線上帳戶的所有使用者帳戶。 依預設，會選取所有的帳戶，並為 Microsoft 線上識別碼建議使用者名稱。 如果您已將自訂網際網路網域連結至 Microsoft 365 訂用帳戶，則預設會使用該網域。

4.  在 [新增 Microsoft Online Services 帳戶] 頁面上，檢視將建立的帳戶。 例如，檢查已經有不同線上識別碼之線上帳戶的使用者，並確定已選取您想在電子郵件地址中使用的網域。 當您完成任何所需的變更時，按一下 [下一步]。

5.  在 [ **指派 Microsoft Online Services 授權** ] 頁面上，選取使用者將使用 Microsoft 365 服務。 當您按一下 [下一步]，就會開始建立帳戶。

    > [!NOTE]
    >  您必須擁有每個線上帳戶 Microsoft 365 的服務授權。 您可以檢查您的可用授權，並在需要時開啟您的訂用帳戶，以從儀表板上的 **Microsoft 365** 頁面新增授權。

6.  請通知使用者他們已擁有 Microsoft 線上帳戶。 他們必須變更網路使用者帳戶密碼，才能登入 Microsoft 365。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。

###  <a name="to-begin-using-a-new-microsoft-online-account"></a><a name="BKMK_ToBeginUsingAnOnlineAccount"></a> 開始使用新的 Microsoft 線上帳戶

1.  以網路使用者帳戶登入您的電腦。

2.  變更您的使用者帳戶密碼。 若要開始，請按 Ctrl + Alt + Delete，並按一下 [變更密碼]。

     當您變更密碼時，密碼會與新的線上帳戶同步處理。 您現在可以使用相同的密碼登入 Microsoft 365。

3.  使用新的線上識別碼和使用者帳戶密碼登[入 Microsoft 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) 。

    > [!IMPORTANT]
    >  請勿在 Microsoft 365 中變更您的線上帳戶密碼。 這樣會中斷密碼同步化。 您的線上密碼會在每次您更新網路使用者帳戶密碼時進行更新。

###  <a name="to-import-user-accounts-from-your-existing-online-accounts"></a><a name="BKMK_ToImportUserAccounts"></a> 從現有的線上帳戶匯入使用者帳戶

1.  在儀表板上，開啟 [使用者] 頁面。

2.  在 [ **使用者** 工作] 中，按一下 [ **從 Microsoft 365 匯入帳戶**]。

     下一個頁面會顯示您的 Microsoft 365 訂用帳戶在伺服器上沒有使用者帳戶的所有線上帳戶。 依預設，會選取所有帳戶，並為使用者名稱建議線上識別碼。

3.  若要建立使用者帳戶：

    1.  對任何建議的使用者帳戶進行所需的變更。

    2.  (選擇性) 按一下連結以檢視指派給使用者帳戶的暫時密碼。 您需要提供使用者暫時密碼以及他們的新帳戶名稱。

          (建立帳戶之後，您會在此檔案中找到這些密碼： *SystemDrive*\Users \\ *Office365admin* \\ *NewServerUser*.txt，其中 *Office365admin* 是用來管理伺服器上 Microsoft 365 的網路帳戶，而 *NewServerUser* 則是新的使用者帳戶名稱。 ) 

    3.  按一下 [下一步] 建立使用者帳戶。

4.  讓您的使用者知道其新的使用者帳戶，以及他們將在第一次–時用來登入伺服器的暫時密碼，而且他們必須在登入後變更密碼。 如需使用者的相關指示，請參閱 [開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。

     請確定他們知道線上帳戶的密碼會與其使用者帳戶進行同步處理，而且不應該在 Microsoft 365 中變更其線上密碼。

###  <a name="to-create-a-new-user-account-with-an-online-account-assigned-to-it"></a><a name="BKMK_ToCreateaNewUserAccount"></a> 若要建立新的使用者帳戶，並將其指派給他們的線上帳戶

1.  在儀表板上，按一下 [使用者]。

2.  在 [使用者工作] 中，按一下 [新增使用者帳戶]。 [新增使用者帳戶精靈] 會隨即出現。

3.  請依照下列指示來建立使用者帳戶。

4.  在 [指派 Microsoft Online Services 帳戶] 頁面上，請為使用者建立新的線上帳戶，或是指派現有的線上帳戶：

    -   若要建立新的線上帳戶，請按一下 [建立新的 Microsoft Online Services 帳戶並指派給此使用者帳戶]，然後輸入 Microsoft Online Services 帳戶名稱 (依預設，使用者名稱會作為線上識別碼)。 然後按一下 [下一步]。

    -   若要指派現有的 Microsoft 線上帳戶，請按一下 [將現有的 Microsoft Online Services 帳戶指派給此使用者帳戶]，並從下拉式清單中選取現有的帳戶。 然後按一下 [下一步]。

    > [!NOTE]
    >  在 Windows Server Essentials 中，Microsoft Online Services 帳戶在 [嚮導] 和 [儀表板] 標籤中稱為 Microsoft 365 帳戶。

5.  遵循指示以完成精靈。

6.  通知使用者他們將需要變更其使用者帳戶密碼，才能使用新的線上帳戶登入 Microsoft 365。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。

#### <a name="to-assign-an-online-account-to-a-user-account"></a><a name="BKMK_ToAssignAnOnlineAccount"></a>將線上帳戶指派給使用者帳戶

1.  在儀表板上，按一下 [使用者]。

2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [指派 Microsoft 線上帳戶]。 [指派 Microsoft Online Services 帳戶精靈] 隨即出現。

3.  請指派現有的線上帳戶或為使用者建立新的帳戶。 新帳戶的預設線上識別碼是使用者名稱。 然後按一下 [下一步] 將線上帳戶新增到使用者帳戶。

4.  檢視精靈最後一頁上的資訊，然後按一下 [關閉]。

5.  通知使用者他們將需要變更其使用者帳戶密碼，才能使用新的線上帳戶登入 Microsoft 365。 如需指示，請參閱[開始使用新的 Microsoft 線上帳戶](#BKMK_ToBeginUsingAnOnlineAccount)。

##  <a name="manage-online-accounts"></a><a name="BKMK_SECTION_ManageOnlineAccounts"></a> 管理線上帳戶
 當您將線上帳戶新增到 Windows Server Essentials 中的使用者帳戶時，您可以在整個帳戶的生命週期中同時管理這兩個帳戶。

###  <a name="understanding-the-online-account-status"></a><a name="BKMK_UnderstandingAccountStatus"></a> 瞭解線上帳戶狀態
 當您將 Microsoft Online Services 帳戶指派給使用者帳戶，帳戶的電子郵件地址會出現在儀表板 [使用者] 頁面上的 [Microsoft 線上帳戶] 欄中。  (在 Windows Server Essentials 中，資料行標籤是 **Microsoft 365 帳戶**。 ) 

-   電子郵件地址旁的藍色圖示表示線上帳戶作用中。 也就是說，帳戶具有目前的 Microsoft 365 授權，而且使用者可以使用線上識別碼登入 Microsoft 365。

-   電子郵件地址旁的陰影圖示表示線上帳戶為非使用中的–，可能是因為授權已不再使用中或已取消指派線上帳戶。 當您取消指派使用者的線上帳戶時，系統會移除授權，並封鎖使用者使用該帳戶登入 Microsoft 365。 但是，伺服器會維護使用者帳戶名稱與 Microsoft 365 電子郵件地址之間的對應。

###  <a name="restrict-access-to-an-online-account"></a><a name="BKMK_UnassignOnlineAccount"></a> 限制存取線上帳戶
 如果使用者離開您的組織，或想要限制使用者對 Microsoft 365 服務的存取權，該怎麼辦？ 如果您在 Windows Server Essentials 中管理使用者的線上帳戶以及他們的使用者帳戶，您有三個選項：

-   要 **取消指派線上帳戶** 嗎？如果您想要讓使用者無法使用 Microsoft 365 而不想存取伺服器上的資源，您應該取消指派線上帳戶。 將會釋出 Microsoft 365 的授權，並封鎖使用者登入 Microsoft 365。 但是，伺服器會維護使用者帳戶名稱與 Microsoft 365 電子郵件地址之間的對應。 如需相關指示，請參閱 [從使用者帳戶取消指派線上帳戶](#BKMK_ToUnassignAnOnlineAccount)。

-   要 **停用使用者帳戶** 嗎？如果您因為員工暫時或永久離開而停用使用者帳戶，則也會停用使用者的線上帳戶。 線上帳戶將無法繼續使用，但是使用者資料 (包括電子郵件) 會保留在 Microsoft Online Services 中。 如需相關指示，請參閱在[管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)中[停用使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)。

-   要 **移除使用者帳戶** 嗎？如果您移除使用者帳戶，線上帳戶也會從 Microsoft Online Services 中移除。 如需相關指示，請參閱在[管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)中[移除使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)。

    > [!WARNING]
    >  請注意當移除線上帳戶時，使用者資料會遵循 Microsoft Online Services 的資料保留原則。 如果您需要在員工離職後保留使用者資料，請停用使用者帳戶，而不是移除該使用者帳戶。

####  <a name="to-unassign-an-online-account-from-a-user-account"></a><a name="BKMK_ToUnassignAnOnlineAccount"></a> 從使用者帳戶取消指派線上帳戶

1.  在儀表板上，按一下 [使用者]。

2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [ **取消指派 Microsoft 線上帳戶** (在 Windows Server Essentials 中，按一下 [ **取消指派 Microsoft 365 帳戶** ]) 。

3.  在確認提示按一下 [是]。

##  <a name="manage-email-addresses-for-exchange-online"></a><a name="BKMK_SECTION_ManageEmailAddresses"></a> 管理 Exchange Online 的電子郵件地址
 藉由在 Windows Server Essentials 中將電子郵件地址新增到使用者的線上帳戶，您可以允許使用者在 Exchange Online 中的多個電子郵件地址接收電子郵件。

###  <a name="to-add-additional-email-addresses-to-a-user-s-microsoft-online-account"></a><a name="BKMK_PROC_AddEmailAliases"></a> 將其他電子郵件地址新增至使用者的 Microsoft 線上帳戶

1.  在 [Windows Server Essentials 儀表板] 上，按一下 [ **使用者**]。

2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [檢視帳戶內容]。

3.  在 [帳戶內容] (的 [ **Microsoft 線上** ] 索引標籤或 Windows Server Essentials) 的 [ **Microsoft 365** ] 索引標籤中，按一下 [ **新增**]。

4.  輸入新的電子郵件別名，然後選取電子郵件網域。

5.  按兩次 **[確定]**。

##  <a name="manage-distribution-groups-for-exchange-online-windows-server-essentials-only"></a><a name="BKMK_SECTION_ManageDistributionGroups"></a> 管理 Exchange Online 的通訊群組 (僅限 Windows Server Essentials) 
 將您的 Windows Server Essentials 伺服器與 Microsoft 365 整合之後，您可以從 Windows Server Essentials 儀表板建立與管理 Exchange Online 的通訊群組。 您將在新增至 [**使用者**] 頁面的 [**通訊群組**] 索引標籤上進行這項操作。 只有在訂閱 Exchange Online 的情況下，您才會看到這個索引標籤。 這項功能在 Windows Server Essentials 中無法使用。

 請使用下列程序來：

-   [新增通訊群組](#BKMK_PROCEDURE_AddDistGroup)

-   [變更通訊群組成員](#BKMK_ChangeGroupMembers)

-   [變更使用者的通訊群組成員資格](#BKMK_EditUserMemberships)

-   [移除通訊群組](#BKMK_RemoveDistributionGroup)

###  <a name="to-add-a-distribution-group"></a><a name="BKMK_PROCEDURE_AddDistGroup"></a> 新增通訊群組

1.  在 Windows Server Essentials 的儀表板上，按一下 [ **使用者**]，然後按一下 [ **通訊群組** ] 索引標籤。

2.  在 [通訊群組工作] 中，按一下 [新增通訊群組]。

     [新增新的通訊群組精靈] 會隨即出現。

3.  在 [新增新的通訊群組] 頁面上，輸入下列資訊，然後按一下 [下一步]：

    -   輸入新通訊群組的群組名稱、選擇性描述及電子郵件別名。

    -   依預設，通訊群組可以接收來自公司外部之人員的電子郵件。 如果您不允許這麼做，請清除該選項。

4.  在 [新增群組成員] 頁面上，使用 [新增] 按鈕，將已指派線上帳戶的作用中使用者帳戶以及其他通訊群組，新增到新的通訊群組。 然後按一下 [下一步]。

     新的通訊群組會建立在 Exchange Online 中。

###  <a name="to-change-the-members-of-a-distribution-group"></a><a name="BKMK_ChangeGroupMembers"></a> 變更通訊群組的成員

1.  在儀表板上，按一下 [使用者]，然後按一下 [通訊群組] 索引標籤。

2.  在清單中的通訊群組上按一下滑鼠右鍵，然後按一下 [變更群組成員資格]。

3.  使用 [新增] 與 [移除] 按鈕，將作用中線上帳戶新增到通訊群組中，或從通訊群組中移除。 然後按一下 [下一步] 更新 Exchange Online 中的通訊群組成員資格。

###  <a name="to-change-a-user-s-distribution-group-memberships"></a><a name="BKMK_EditUserMemberships"></a> 變更使用者的通訊群組成員資格

1.  在儀表板上，按一下 [使用者]。

2.  在清單中的使用者帳戶上按一下滑鼠右鍵，然後按一下 [檢視帳戶內容]。

3.  在使用者帳戶內容中，按一下 [通訊群組] 索引標籤，然後按一下 [編輯]。

4.  在 [編輯群組成員資格] 方塊中，使用 [新增] 與 [移除] 按鈕，將通訊群組新增到使用者帳戶中，或從使用者帳戶中移除，然後按一下 [關閉]。

5.  按一下 [確定] 以儲存更新的使用者帳戶內容。

###  <a name="to-remove-a-distribution-group"></a><a name="BKMK_RemoveDistributionGroup"></a> 移除通訊群組

1.  在儀表板上，按一下 [使用者]，然後按一下 [通訊群組] 索引標籤。

2.  在清單中的通訊群組上按一下滑鼠右鍵，然後按一下 [移除群組]。

3.  在確認提示按一下 [刪除群組]。

     通訊群組會從 Exchange Online 移除。

## <a name="additional-references"></a>其他參考資料

-   [管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)

-   [管理 Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md)

-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
