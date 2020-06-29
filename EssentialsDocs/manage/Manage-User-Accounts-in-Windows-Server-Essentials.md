---
title: 在 Windows Server Essentials 中管理使用者帳戶
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 350c9aa4cf4a2e71f3a3b7600ec2acd9016fe50e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470503"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理使用者帳戶

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

[Windows Server Essentials 儀表板] 的 [使用者] 頁面集中了可幫助您管理您小型企業網路上使用者帳戶的資訊和工作。 如需使用者儀表板的總覽，請參閱[儀表板總覽](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)。


##  <a name="managing-user-accounts"></a><a name="BKMK_ManageAccounts"></a>管理使用者帳戶
 下列主題提供有關如何使用 [Windows Server Essentials 儀表板] 來管理伺服器上的使用者帳戶資訊：

-   [新增使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)

-   [移除使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)

-   [檢視使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)

-   [變更使用者帳戶的顯示名稱](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)

-   [啟用使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)

-   [停用使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)

-   [了解使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)

-   [使用儀表板來管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)

###  <a name="add-a-user-account"></a><a name="BKMK_Manage1"></a>新增使用者帳戶
 在您新增使用者帳戶之後，所指派的使用者便可以登入網路，而您可以授與該使用者存取網路資源 (例如共用資料夾和「遠端 Web 存取」網站) 的權限。 Windows Server Essentials 包含 [新增使用者帳戶精靈]，可協助您：

-   提供使用者帳戶的名稱和密碼。

-   將帳戶定義為系統管理員或標準使用者。

-   選取使用者帳戶可以存取的共用資料夾。

-   指定使用者帳戶是否能夠遠端存取網路。

-   選取電子郵件選項 (如果適用)。

-   指派 Microsoft Online Services 帳戶（在 Windows Server Essentials 中稱為「Office 365 帳戶」）（如果適用的話）。

-   指派使用者群組（僅限 Windows Server Essentials）。

> [!NOTE]
> - Microsoft Azure Active Directory （Azure AD）中不支援非 ASCII 字元。 如果您的伺服器與 Azure AD 整合，請勿在您的密碼中使用任何非 ASCII 字元。
>   -   只有在您所安裝的增益集能夠提供電子郵件服務時，才可使用電子郵件選項。

##### <a name="to-add-a-user-account"></a>新增使用者帳戶

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在 [使用者工作]  **** 窗格中，按一下 [新增使用者帳戶] ****。 [新增使用者帳戶精靈] 會隨即出現。

4.  遵循指示以完成精靈。

###  <a name="remove-a-user-account"></a><a name="BKMK_Remove"></a>移除使用者帳戶
 當您選擇從伺服器中移除使用者帳戶時，精靈會刪除選取的帳戶。 因此，您無法再使用該帳戶來登入網路，或存取任何網路資源。 您也可以選擇在移除使用者帳戶的同時刪除該帳戶的檔案。 如果您不想要永久移除使用者帳戶，可以改為停用使用者帳戶以暫停其網路資源存取權。

> [!IMPORTANT]
>  如果使用者帳戶已指派 Microsoft online 帳戶，則當您移除使用者帳戶時，線上帳戶也會從 Microsoft Online Services 中移除，而使用者的資料（包括電子郵件）則受 Microsoft 線上服務中的資料保留原則所制約。 如果您想要保留線上帳戶的使用者資料，請停用該使用者帳戶，而不要將它移除。 如需詳細資訊，請參閱[管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。

##### <a name="to-remove-a-user-account"></a>移除使用者帳戶

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要移除的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[移除使用者帳戶**]。 [刪除使用者帳戶精靈] 隨即出現。

5.  在 [**您要保留檔案嗎？** ] 頁面上，您可以選擇刪除使用者的檔案，包括檔案歷程記錄備份，以及使用者帳戶的重新導向資料夾。 若要保留使用者的檔案，請將核取方塊保留空白。 選取完成後，請按 [下一步]****。

6.  按一下 [刪除帳戶]****。

> [!NOTE]
>  移除使用者帳戶之後，該帳戶就不會再出現在使用者帳戶清單中。 如果您選擇刪除檔案，伺服器會從 [**使用者**] 伺服器資料夾和 [檔案歷程**記錄備份**] 伺服器資料夾中永久刪除使用者的資料夾。
>
>  如果您使用的是整合式電子郵件服務，則指派給該使用者帳戶的電子郵件帳戶也會一併被移除。

###  <a name="view-user-accounts"></a><a name="BKMK_Manage3"></a>查看使用者帳戶
 [Windows Server Essentials 儀表板] 的 [使用者]**** 區段會顯示網路使用者帳戶的清單。 這份清單也會提供各帳戶的其他相關資訊。

##### <a name="to-view-a-list-of-user-accounts"></a>檢視使用者帳戶清單

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在主瀏覽列上，按一下 [使用者]****。

3.  [儀表板] 會顯示目前的使用者帳戶清單。

##### <a name="to-view-or-change-properties-for-a-user-account"></a>檢視或變更使用者帳戶的內容

1.  在使用者帳戶清單中，選取您想要檢視或變更內容的帳戶。

2.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[查看帳戶屬性**]。 使用者帳戶的 [內容]**** 頁面隨即出現。

3.  按一下索引標籤以顯示該帳戶功能的內容。

4.  若要儲存您對使用者帳戶內容所做的任何變更，請按一下 [套用]****。

###  <a name="change-the-display-name-for-the-user-account"></a><a name="BKMK_Manage4"></a>變更使用者帳戶的顯示名稱
 顯示名稱是在 [儀表板] 的 [使用者]**** 頁面中，出現於 [名稱]**** 欄中的名稱。 變更顯示名稱並不會變更使用者帳戶的登入名稱。

##### <a name="to-change-the-display-name-for-a-user-account"></a>變更使用者帳戶的顯示名稱

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要變更的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[查看帳戶屬性**]。 使用者帳戶的 [內容]**** 頁面隨即出現。

5.  在 [一般]**** 索引標籤上，為使用者帳戶輸入新的 [名字]**** 和 [姓氏]****，然後按一下 [確定]****。

     新顯示名稱會出現在使用者帳戶清單中。

###  <a name="activate-a-user-account"></a><a name="BKMK_Manage5"></a>啟用使用者帳戶
 在您啟用使用者帳戶之後，所指派的使用者便可以登入帳戶並存取帳戶擁有權限的網路資源 (例如共用資料夾和「遠端 Web 存取」網站)。

> [!NOTE]
>  您只能啟用已停用的使用者帳戶。 將使用者帳戶從伺服器中移除之後，您就無法再啟用該帳戶。

##### <a name="to-activate-a-user-account"></a>啟用使用者帳戶

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在清單檢視中，選取您想要啟用的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 [**啟用使用者帳戶**]。

5.  在確認視窗中，按一下 [是]**** 來確認您的動作。

> [!NOTE]
>  啟用使用者帳戶之後，該帳戶的狀態就會顯示為 [使用中]****。 如某權限曾於停用前指派至此使用者帳戶，此時便能重新予以取得。
>
>  如果您有整合式電子郵件提供者，則指派給該使用者帳戶的電子郵件帳戶也會一併被啟用。

###  <a name="deactivate-a-user-account"></a><a name="BKMK_Manage6"></a>停用使用者帳戶
 當您停用使用者帳戶時，會暫停帳戶對伺服器的存取權。 因此，所指派的使用者會無法使用該帳戶來存取網路資源 (例如共用資料夾或「遠端 Web 存取」網站)，直到您啟用帳戶為止。

 如果使用者帳戶已被指派 Microsoft 線上帳戶，該線上帳戶也會一併被停用。 使用者無法使用 Office 365 中的資源，以及您訂閱的其他線上服務，但是使用者的資料（包括電子郵件）會保留在 Microsoft Online Services 中。

> [!NOTE]
>  您只能停用目前使用中的使用者帳戶。

##### <a name="to-deactivate-a-user-account"></a>停用使用者帳戶

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在清單檢視中，選取您想要停用的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[停用使用者帳戶**]。

5.  在確認視窗中，按一下 [是]**** 來確認您的動作。

> [!NOTE]
>  停用使用者帳戶之後，該帳戶的狀態就會顯示為 [非使用中]****。
>
>  如果您有整合式電子郵件提供者，則指派給該使用者帳戶的電子郵件帳戶也會一併被停用。

###  <a name="understand-user-accounts"></a><a name="BKMK_Manage7"></a>瞭解使用者帳戶
 使用者帳戶可提供重要的資訊給 Windows Server Essentials，讓個人能夠存取儲存在伺服器上的資訊，並讓個別的使用者能夠建立及管理他們的檔案和設定。 使用者如果擁有 Windows Server Essentials 使用者帳戶及電腦存取權限，便可以登入網路上的任何電腦。 使用者是以其使用者名稱和密碼來存取其使用者帳戶。

 使用者帳戶的類型主要有兩種。 每一種類型各提供使用者不同層級的電腦控制權：

-   **標準**帳戶是用於日常運算。 標準帳戶可防止使用者進行影響其他使用者的變更 (例如刪除檔案或變更網路設定)，從而協助您保護網路。

-   **系統管理員**帳戶提供最充分的電腦網路控制權。 您應該只有在必要時才指派系統管理員帳戶類型。

###  <a name="manage-user-accounts-using-the-dashboard"></a><a name="BKMK_Manage8"></a>使用儀表板管理使用者帳戶
 Windows Server Essentials 可讓您使用 [Windows Server Essentials 儀表板] 來執行一般系統管理工作。 [儀表板] 的 [**使用者**] 頁面預設包含兩個索引標籤： [**使用者**] 和 [**使用者] 群組**。

> [!NOTE]
> - 如果您將執行 Windows Server Essentials 的伺服器與 Office 365 整合，則 [儀表板] 的 [**使用者**] 頁面中也會新增名為 [**通訊群組**] 的新索引標籤。
>   -   在 Windows Server Essentials 中，儀表板的 [**使用者**] 頁面只包含一個索引標籤-[**使用者**]。

 [使用者]**** 索引標籤包含下列各項：

- 一份使用者帳戶清單，當中顯示：

  -   使用者的名稱。

  -   使用者帳戶的登入名稱。

  -   使用者帳戶是否具有「隨處存取」權限。 使用者帳戶的「隨處存取」權限是 [允許]**** 或 [不允許]****。

  -   這個使用者帳戶的「檔案歷程記錄」是否受執行 Windows Server Essentials 的伺服器管理。 使用者帳戶的「檔案歷程記錄」狀態是 [受管理]**** 或 [不受管理]****。

  -   指派給使用者帳戶的存取權層級。 您可以為使用者帳戶指派 [標準使用者]**** 存取權或 [系統管理員]**** 存取權。

  -   使用者帳戶狀態。 使用者帳戶可以是 [使用中]****、[非使用中]**** 或 [不完整]****。

  -   在 Windows Server Essentials 中，如果伺服器已與 Office 365 或 Windows Intune 整合，就會顯示 Microsoft 線上帳戶。

  -   在 Windows Server Essentials 中，如果伺服器與 Microsoft Office 365 整合，則會顯示使用者帳戶的 Office 365 帳戶狀態（在 Windows Server Essentials 中稱為 Microsoft online 帳戶）。

- 一個詳細資料窗格，含有所選使用者帳戶的其他相關資訊。

- 一個包含下列資訊的工作窗格：

  -   一組使用者帳戶系統管理工作，例如檢視和移除使用者帳戶，以及變更密碼。

  -   可讓您全域設定或變更網路中所有使用者帳戶之設定的工作。

  下表說明可從 [**使用者**] 索引標籤使用的各種使用者帳戶工作。某些工作是使用者帳戶特有的，只有當您在清單中選取使用者帳戶時才會顯示。

> [!NOTE]
>  如果您將 Office 365 與 Windows Server Essentials 整合，則會有其他工作可供使用。 如需詳細資訊，請參閱[管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。

### <a name="user-account-tasks-in-the-dashboard"></a>儀表板中的使用者帳戶工作

|工作名稱|描述|
|---------------|-----------------|
|檢視帳戶內容|可讓您檢視和變更所選使用者帳戶的內容，以及指定該帳戶的資料夾存取權限。|
|停用使用者帳戶|已停用的使用者帳戶無法登入網路或存取網路資源 (例如共用資料夾或印表機)。|
|啟用使用者帳戶|已啟用的使用者帳戶可以登入網路，並可依據帳戶權限的定義存取網路資源。|
|移除使用者帳戶|可讓您移除選取的使用者帳戶。|
|變更使用者帳戶密碼|可讓您重設所選使用者帳戶的網路密碼。|
|新增使用者帳戶|啟動 [新增使用者帳戶精靈]，可讓您建立具有標準使用者存取權或系統管理員存取權的單一新使用者帳戶。|
|指派 Microsoft 線上帳戶|將 Microsoft 線上帳戶新增到選取的區域網路使用者帳戶中 。<br /><br /> 當您的伺服器已與 Microsoft 線上服務 (例如 Office 365) 整合時，就會顯示這項工作。|
|新增 Microsoft 線上帳戶|新增 Microsoft 線上帳戶並將它們與區域網路的使用者帳戶建立關聯。<br /><br /> 當您的伺服器已與 Microsoft 線上服務 (例如 Office 365) 整合時，就會顯示這項工作。|
|設定密碼原則|可讓您變更您網路的密碼原則值。|
|匯入 Microsoft 線上帳戶|執行將帳戶從 Microsoft 線上服務大量匯入到區域網路的工作。<br /><br /> 當您的伺服器已與 Microsoft 線上服務 (例如 Office 365) 整合時，就會顯示這項工作。|
|重新整理|重新整理 [使用者] 索引標籤。<br /><br /> 這項工作適用于 Windows Server Essentials。|
|變更檔案歷程記錄設定|可讓您變更「檔案歷程記錄」設定，例如備份頻率或備份期間。<br /><br /> 這項工作適用于 Windows Server Essentials。|
|匯出所有遠端連線|將過去 30 天所有連到伺服器的遠端連線建立為一個 .CSV 格式檔案。|

##  <a name="managing-passwords-and-access"></a><a name="BKMK_ManageAccess"></a>管理密碼和存取
 下列主題提供有關如何使用 [Windows Server Essentials 儀表板] 來管理使用者帳戶密碼和使用者對伺服器上共用資料夾之存取權的資訊：

-   [變更或重設使用者帳戶的密碼](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)

-   [密碼原則須知](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)

-   [變更密碼原則](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)

-   [共用資料夾的存取層級](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)

-   [保留和管理已移除之使用者帳戶的檔案存取權](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)

-   [將 DSRM 密碼與網路系統管理員密碼同步](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)

-   [授與使用者帳戶遠端桌面權限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)

-   [讓使用者能夠存取伺服器上的資源](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)

-   [變更使用者帳戶的遠端存取權限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)

-   [變更使用者帳戶的虛擬私人網路權限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)

-   [變更使用者帳戶的內部共用資料夾存取權](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)

-   [允許使用者帳戶對其電腦建立遠端桌面工作階段](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)

###  <a name="change-or-reset-the-password-for-a-user-account"></a><a name="BKMK_Access1"></a>變更或重設使用者帳戶的密碼
 若要變更或重設使用者帳戶密碼，請依照下列步驟執行。

##### <a name="to-reset-the-password-for-a-user-account"></a>重設使用者帳戶的密碼

1. 開啟 [Windows Server Essentials 儀表板]。

2. 在瀏覽列上，按一下 [使用者]****。

3. 在使用者帳戶清單中，選取您想要重設的使用者帳戶。

4. 在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 [**變更使用者帳戶密碼**]。 [變更使用者帳戶密碼精靈] 隨即出現。

5. 輸入使用者帳戶的新密碼，然後再次輸入密碼來進行確認。

6. 按一下 [變更密碼]****。

7. 將新密碼提供給使用者。

   > [!IMPORTANT]
   > - 如果您帳戶的密碼原則已設定為 [密碼永久有效]****，您就無法變更密碼。
   >   -   Azure AD 中不支援非 ASCII 字元。 因此，如果您的伺服器與 Azure AD 整合，請勿在您的密碼中使用任何非 ASCII 字元。
   >   -   如果將 Microsoft 線上帳戶（在 Windows Server Essentials 中稱為 Office 365 帳戶）指派給使用者，密碼就會與線上帳戶密碼同步處理。 使用者將使用新密碼來登入伺服器或登入 Office 365。 如需詳細資訊，請參閱[管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。

###  <a name="what-you-should-know-about-password-policies"></a><a name="BKMK_Access3"></a>密碼原則的相關須知
 密碼原則是一組定義使用者如何建立和使用密碼的規則。 這個原則可協助防止對使用者資料和儲存在伺服器上的其他資訊進行未經授權的存取。 密碼原則會套用到所有存取網路的使用者帳戶。

 Windows Server Essentials 密碼原則是由三個主要元素組成，如下所示：

- **密碼長度**。  密碼越長越安全。 空白密碼不安全。

- **密碼複雜性**。  複雜密碼包含大寫與小寫字母 (a 到 z、A 到 Z)、基本數字 (0-9) 及非字母符號 (例如 !、@、#、_、-) 的混合組合。 複雜密碼是較不容易受到未經授權的存取利用。 包含使用者名稱、生日日期或其他個人資訊的密碼無法提供適當的安全性。

- **密碼使用期限**。  Windows Server Essentials 要求使用者至少每 180 天變更一次他們的密碼。 您也可以選擇讓密碼永久有效。

  為了讓您更容易在您的電腦網路上實作密碼原則，Windows Server Essentials 提供一個簡單工具，可讓您將密碼原則設定或變更成下列四個預先定義之原則設定檔中的任何一個：

- **弱式**。  使用者可以指定任何不是空白的密碼。

- **中等**。  這些密碼必須至少包含 5 個字元。 不需要使用複雜密碼。

- **中等強度**。  這些密碼必須至少包含 5 個字元，而且必須包含字母、數字及符號。

- **強式**。  這些密碼必須至少包含 7 個字元，而且必須包含字母、數字及符號。 這些密碼更安全，但可能會讓使用者更難記住。

  > [!NOTE]
  >  密碼不能包含使用者名稱或電子郵件地址。
  >
  >  如果您已與 Office 365 整合，此整合會強制使用 [強式] **** 密碼原則，並且會更新該原則以包含下列要求條件：
  >
  > - 密碼必須包含 8 16 個字元。
  >   -   密碼不能包含空格或 Office 365 電子郵件名稱。

  根據預設，伺服器安裝會將預設密碼原則設定為**強式**選項。

###  <a name="change-the-password-policy"></a><a name="BKMK_Access4"></a>變更密碼原則
 您可以使用下列程序將密碼原則設定或變更成四個預先定義之原則設定檔中的任何一個。

##### <a name="to-change-the-password-policy"></a>變更密碼原則

1.  開啟 [Windows Server Essentials 儀表板]，然後按一下 [使用者]****。

2.  在 [使用者工作]**** 窗格中，按一下 [設定密碼原則]****。

3.  在 [變更密碼原則]**** 畫面中，移動滑桿來設定密碼強度的層級。

     Microsoft 建議您將密碼強度設定為 [強式]****。

    > [!NOTE]
    >  您也可以選擇選取 [密碼永久有效]****。 這個設定較不安全，所以並不建議選取。

4.  按一下 [變更原則]****。

###  <a name="level-of-access-to-shared-folders"></a><a name="BKMK_Access5"></a>共用資料夾的存取層級
 最佳做法是指派最低可用又仍可讓使用者執行必要工作的權限。

 針對伺服器上的共用資料夾，您有三個可用的存取設定：

-   **讀取/寫入**。  如果您想要讓使用者帳戶有建立、變更及刪除共用資料夾中任何檔案的權限，請選擇這個設定。

-   **唯讀**。  如果您想要讓使用者帳戶只有讀取共用資料夾中檔案的權限，請選擇這個設定。 具有唯讀存取權的使用者帳戶無法建立、變更或刪除共用資料夾中的任何檔案。

-   **不允許存取**。  如果您不想要讓使用者帳戶存取共用資料夾中的任何檔案，請選擇這個設定。

###  <a name="retain-and-manage-access-to-files-for-removed-user-accounts"></a><a name="BKMK_Access6"></a>保留和管理已移除使用者帳戶之檔案的存取權
 網路系統管理員可以移除使用者帳戶，並選擇保留使用者的檔案供日後使用。 在這個案例中，已移除的使用者帳戶無法再被用來登入網路；不過，這位使用者的檔案將會儲存在共用資料夾中，可以與其他使用者共用。

> [!IMPORTANT]
>  請注意，如果您移除已被指派 Microsoft 線上帳戶的使用者帳戶，該線上帳戶也會一併被移除，而使用者資料 (包括電子郵件) 的去留則取決於 Microsoft Online Services 中的資料保留原則。 若要保留線上帳戶的使用者資料，請停用該使用者帳戶，而不要將它移除。 如需詳細資訊，請參閱[管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)。

##### <a name="to-remove-a-user-account-but-retain-access-to-the-users-files"></a>移除使用者帳戶，但保留使用者檔案的存取權

1. 開啟 [Windows Server Essentials 儀表板]。

2. 在瀏覽列上，按一下 [使用者]****。

3. 在使用者帳戶清單中，選取您想要移除的使用者帳戶。

4. 在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[移除使用者帳戶**]。 [刪除使用者帳戶精靈] 隨即出現。

5. 在 [您要保留檔案嗎?]**** 頁面上，確定 [刪除此使用者帳戶的檔案 (包括檔案歷程記錄備份與重新導向的資料夾)]**** 核取方塊已取消選取，然後按 [下一步]****。

    確認頁面隨即出現，警告您正在刪除帳戶但會保留檔案。

6. 按一下 [刪除帳戶]**** 來移除使用者帳戶。

   移除使用者帳戶之後，系統管理員可以將共用資料夾的存取權提供給另一個使用者帳戶。

##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>授與使用者帳戶存取共用資料夾的權限

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [存放]****，然後按一下 [伺服器資料夾]**** 索引標籤。

3.  在資料夾清單中，選取 [使用者]**** 資料夾。

4.  在 [使用者工作]**** 窗格中，按一下 [開啟資料夾]****。 [Windows 檔案總管] 隨即開啟並顯示 [使用者]**** 資料夾的內容。

5.  在您想要共用之使用者帳戶的資料夾上按一下滑鼠右鍵，然後按一下 [內容]****。

6.  在 **<使用者帳戶 \> **內容] 中，按一下 [**共用**] 索引標籤，然後按一下 [**共用**]。

7.  在 [檔案共用]**** 視窗中，輸入或選取您想要與其共用資料夾的使用者帳戶名稱，然後按一下 [新增]****。

8.  選擇要讓使用者帳戶擁有的 [權限層級]****，然後按一下 [共用]****。

###  <a name="synchronize-the-dsrm-password-with-the-network-administrator-password"></a><a name="BKMK_Access7"></a>同步處理 DSRM 密碼與網路系統管理員密碼
 「目錄服務還原模式」(DSRM) 是一種可修復或復原 Active Directory 的特殊開機模式。 當 Active Directory 失敗或需要還原時，作業系統會使用 DSRM 來登入電腦。 如果您的網路系統管理員密碼和 DSRM 密碼不同，DSRM 將不會載入。

 在乾淨、第一次的 Windows Server Essentials 安裝中，程式會將 DSRM 密碼設定為您在安裝期間或在移轉回應檔案中指定的網路系統管理員帳戶密碼。 當您變更您的網路系統管理員密碼 (一般建議是每 60 天變更一次以增加伺服器安全性) 時，密碼變更並不會轉送給 DSRM。 這會導致密碼不相符。 如果發生這種情況，您可以使用下列解決方案，手動或自動將您的網路系統管理員密碼與 DSRM 密碼同步。

##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>手動將 DSRM 密碼同步至網路系統管理員帳戶

1. 在命令提示字元中，執行 `ntdsutil.exe` 以開啟 ntdsutil 工具。

2. 若要重設 DSRM 密碼，請輸入 **set dsrm password**。

3. 若要在網域控制站上使用目前網路系統管理員的帳戶同步處理 DSRM 密碼，請輸入：

    **從網域帳戶同步**處理 *<current_network_administrator_account>*，然後按 enter。

   由於您將會定期變更網路系統管理員帳戶的密碼，因此為了確保 DSRM 密碼永遠與目前的網路系統管理員密碼相同，建議您建立一個排程工作來每日自動將 DSRM 密碼同步至網路系統管理員密碼。

##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>自動將 DSRM 密碼同步至網路系統管理員帳戶

1.  從伺服器開啟 [系統管理工具]****，然後按兩下 [工作排程器]****。

2.  在 [工作排程器] 的 [動作]**** 窗格中，按一下 [建立工作]****。

3.  在 [名稱]**** 文字方塊中，輸入工作的名稱 (例如**自動同步 DSRM 密碼**)，然後選取 [以最高權限執行]**** 選項。

4.  定義工作的執行時機：

    1.  在 [建立工作]**** 對話方塊中，按一下 [觸發程序]**** 索引標籤，然後按一下 [新增]****。

    2.  在 [新增觸發程序]**** 對話方塊中，選取您的週期選項、指定週期間隔中，然後選擇開始時間。

        > [!NOTE]
        >  最佳做法是設定讓工作每日在非上班時間執行。

    3.  按一下 [確定]**** 以儲存變更並返回 [建立工作]**** 對話方塊。

5.  定義工作動作：

    1.  按一下 [動作]**** 索引標籤，然後按一下 [新增]****。 [新增動作]**** 對話方塊隨即出現。

    2.  在 [動作]**** 清單中，按一下 [啟動程式]****，然後瀏覽至 **C:\WINDOWS\SYSTEM32\ntdsutil.exe**。

    3.  在 [**新增引數**（選擇性）] 文字方塊中，輸入下列內容（您必須包含引號）：**從網域帳戶設定 dsrm 密碼同步 SBS_network_administrator_account q** q，其中*SBS_network_administrator_account*是目前網路系統管理員的帳戶名稱。

6.  按一下 [確定]**** 兩次來儲存工作並關閉 [建立工作]**** 對話方塊。 新工作會出現在 [工作排程]**** 的 [進行中工作]**** 區段中。

###  <a name="give-user-accounts-remote-desktop-permission"></a><a name="BKMK_Access8"></a>授與使用者帳戶遠端桌面許可權
 在 Windows Server Essentials 的預設安裝中，網路使用者沒有可對電腦或網路上其他資源建立遠端連線的權限。

 您必須先設定「隨處存取」，網路使用者才能對網路資源建立遠端連線。 在您設定「隨處存取」之後，使用者便可以從任何位置的裝置，透過網際網路連線來存取您辦公室網路中的檔案、應用程式及電腦。

 [設定隨處存取精靈] 可讓您啟用兩種遠端存取方法：

- 虛擬私人網路 (VPN)

- 遠端 Web 存取

  執行精靈時，您也可以選擇允許所有目前和新加入的使用者帳戶使用「隨處存取」。

  若要設定「隨處存取」，請開啟 [儀表板] 的 [首頁]****，按一下 [設定]****，然後按一下 [設定隨處存取]****。

  如需隨處存取的詳細資訊，請參閱[管理隨處存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。

###  <a name="enable-users-to-access-resources-on-the-server"></a><a name="BKMK_Access9"></a>讓使用者能夠存取伺服器上的資源
  本節適用于執行 Windows Server Essentials 或 Windows Server Essentials 的伺服器，或是執行 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 且已安裝 Windows Server Essentials 體驗角色的伺服器。

 如果您希望使用者使用遠端存取和 (或) 擁有個別的使用者帳戶，在您完成將電腦連線到伺服器的工作之後，可以使用 [儀表板] 為伺服器上連線到網路之電腦的使用者建立新的網路使用者帳戶。 如需有關建立使用者帳戶的詳細資訊，請參閱[新增使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。 建立使用者帳戶之後, 您必須將網路使用者名稱和密碼資訊提供給用戶端電腦的使用者，這樣他們才可以使用 [啟動列] 來存取伺服器上的資源。

 針對您建立的每個使用者帳戶，您都可以透過使用者帳戶內容設定下列項目的存取權：

-   **共用資料夾**。  根據預設，網路系統管理員會有所有共用資料夾的 [讀取/寫入]**** 權限，而標準使用者帳戶會有 [公司] 資料夾的 [唯讀]**** 權限。 如果已啟用媒體串流處理，您便可以為個別的標準使用者帳戶指派下列共用資料夾的資料夾存取權限：[音樂] ****、[圖片] ****、[錄製的節目] **** 以及 [視訊] ****。 您可以在使用者帳戶內容的 [共用資料夾]**** 索引標籤上，設定使用者帳戶對共用資料夾的存取權限。

-   **隨處存取**。  根據預設，網路系統管理員可以使用 VPN 或「遠端 Web 存取」來存取伺服器資源。 針對標準使用者帳戶，您必須在 [隨處存取]**** 索引標籤上設定使用者帳戶權限。

-   **電腦存取**。  根據預設，網路系統管理員可以存取網路中的所有電腦。 不過，針對標準使用者帳戶，您可以在使用者帳戶內容的 [電腦存取]**** 索引標籤上，設定個別的使用者帳戶權限來存取網路上的電腦。

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>在 Windows Server Essentials 2012 R2 中編輯使用者帳戶內容

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要編輯的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[查看帳戶屬性**]。

5.  在 [ **<使用者帳戶 \> 屬性**] 中，執行下列動作：

    1.  在 [共用資料夾]**** 索引標籤上，視需要為每個共用資料夾設定適當的資料夾權限。

    2.  在 [隨處存取]**** 索引標籤上：

        1.  若要允許使用者使用 VPN 來連線到伺服器，請選取 [允許虛擬私人網路 (VPN)]**** 核取方塊。

        2.  若要允許使用者使用「遠端 Web 存取」來連線到伺服器，請選取 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取]**** 核取方塊。

    3.  在 [電腦存取]**** 索引標籤上，選取您想要讓使用者存取的網路電腦。

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>在 Windows Server Essentials 2012 中編輯使用者帳戶內容

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要編輯的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 [**屬性**]。

5.  在 [ **<使用者帳戶 \> 屬性**] 中，執行下列動作：

    1.  如果使用者帳戶需要存取網路健康情況報告，請在 [一般]**** 索引標籤上，選取 [使用者可以檢視網路健康狀態警示]****。

    2.  在 [共用資料夾]**** 索引標籤上，視需要為每個共用資料夾設定適當的資料夾權限。

    3.  在 [隨處存取]**** 索引標籤上：

        1.  若要允許使用者使用 VPN 來連線到伺服器，請選取 [允許虛擬私人網路 (VPN)]**** 核取方塊。

        2.  若要允許使用者使用「遠端 Web 存取」來連線到伺服器，請選取 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取]**** 核取方塊。

    4.  在 [電腦存取]**** 索引標籤上，選取您想要讓使用者存取的網路電腦。

###  <a name="change-remote-access-permissions-for-a-user-account"></a><a name="BKMK_Access10"></a>變更使用者帳戶的遠端存取許可權
 使用者可以藉由使用虛擬私人網路 (VPN)、「遠端 Web 存取」或其他 Web 服務應用程式，從遠端位置存取位於伺服器上的資源。 當您使用 [儀表板] 來設定 Windows Server Essentials 中的「隨處存取」時，預設會為網路使用者開啟遠端存取權限。

##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>變更使用者帳戶的遠端存取權限

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要變更的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 **[查看帳戶屬性**]。 使用者帳戶的 [內容]**** 頁面隨即出現。

5.  在 [隨處存取]**** 索引標籤上，執行下列動作：

    -   選取 [允許虛擬私人網路 (VPN)]**** 核取方塊，以允許使用者使用 VPN 來連線到伺服器。

    -   選取 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取]**** 核取方塊，以允許使用者使用「遠端 Web 存取」來連線到伺服器。

6.  按一下 [套用]****，然後按一下 [確定]****。

###  <a name="change-virtual-private-network-permissions-for-a-user-account"></a><a name="BKMK_Access11"></a>變更使用者帳戶的虛擬私人網路許可權
 您可以使用虛擬私人網路 (VPN) 來連線到 Windows Server Essentials，並存取您所有儲存在伺服器上的資源。 當您有已設定網路帳戶的用戶端電腦且那些帳戶可透過 VPN 連線來連線到託管的 Windows Server Essentials 伺服器時，這會特別有用。 所有在託管的 Windows Server Essentials 伺服器上新建立的使用者帳戶，在第一次登入用戶端電腦時都必須使用 VPN。

##### <a name="to-change-vpn-permissions-for-network-users"></a>變更網路使用者的 VPN 權限

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要授與桌面遠端存取權限的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 [**屬性**]。

5.  在 **<的使用者帳戶 \> 屬性**中，按一下 [**隨處存取**] 索引標籤。

6.  若要允許使用者使用 VPN 來連線到伺服器，請在 [隨處存取]**** 索引標籤上，選取 [允許虛擬私人網路 (VPN)]**** 核取方塊。

7.  按一下 [套用]****，然後按一下 [確定]****。

###  <a name="change-access-to-internal-shared-folders-for-a-user-account"></a><a name="BKMK_Access12"></a>變更使用者帳戶內部共用資料夾的存取權
 您可以使用 [儀表板] 之 [伺服器資料夾]**** 索引標籤上的工作，來管理伺服器上任何共用資料夾的存取權。 安裝 Windows Server Essentials 時，預設會建立下列伺服器資料夾：

-   **用戶端電腦備份**。  用來儲存 Windows Server Backup 所建立的用戶端電腦備份。 這個伺服器資料夾不是共用資料夾。

-   **公司**。  用來供網路使用者儲存及存取與您組織相關的文件。

-   **檔案歷程記錄備份**。  Windows Server Essentials 預設會儲存使用「檔案歷程記錄」建立的檔案備份。 這個伺服器資料夾不是共用資料夾。

-   **資料夾重新導向**。  用來供網路使用者儲存及存取為資料夾重新導向設定的資料夾。 這個伺服器資料夾不是共用資料夾。

-   **音樂**。  用來供網路使用者儲存及存取音樂檔案。 當您開啟媒體共用時，就會建立這個資料夾。

-   **圖片**。  用來供網路使用者儲存及存取圖片。 當您開啟媒體共用時，就會建立這個資料夾。

-   **錄製的節目**。  用來供網路使用者儲存及存取錄製的電視節目。 當您開啟媒體共用時，就會建立這個資料夾。

-   **視訊**。  用來供網路使用者儲存及存取視訊。 當您開啟媒體共用時，就會建立這個資料夾。

-   **使用者**。  用來供網路使用者儲存及存取檔案。 在 [使用者]**** 伺服器資料夾中，會自動為您建立的每個網路使用者帳戶產生一個使用者特定資料夾。

##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>變更使用者帳戶的共用資料夾存取權

1.  開啟 [Windows Server Essentials 儀表板]。

2.  按一下 [存放]****，然後按一下 [伺服器資料夾]****。

3.  瀏覽並選取您想要修改權限的伺服器資料夾。

4.  在工作窗格中，按一下 [檢視資料夾內容]****。

5.  在 [ **<資料夾] \> 屬性**中，按一下 [**共用**]，然後為列出的使用者帳戶選取適當的使用者存取層級，**然後按一下 [** 套用]。

    > [!NOTE]
    >  您無法修改 [檔案歷程記錄備份]****、[資料夾重新導向]**** 及 [使用者]**** 伺服器資料夾的共用權限。 因此，這些伺服器資料夾的資料夾內容也就沒有包含 [共用]**** 索引標籤。

###  <a name="allow-user-accounts-to-establish-a-remote-desktop-session-to-their-computer"></a><a name="BKMK_Access13"></a>允許使用者帳戶建立其電腦的遠端桌面會話
  本節適用于執行 Windows Server Essentials 或 Windows Server Essentials 的伺服器，或是執行 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 且已安裝 Windows Server Essentials 體驗角色的伺服器。

 網路系統管理員可以將權限授與網路使用者，以允許他們從遠端位置存取他們的網路電腦。

##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>讓使用者從遠端位置存取他們的網路電腦

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列上，按一下 [使用者]****。

3.  在使用者帳戶清單中，選取您想要授與桌面遠端存取權限的使用者帳戶。

4.  在 [ **<使用者帳戶 \> **工作] 窗格中，按一下 [**屬性**]。

5.  在 [ **<使用者帳戶 \> **內容] 中，按一下 [**電腦存取**] 索引標籤。

6.  選取您要讓這個使用者帳戶能夠遠端存取的電腦，然後按一下 [確定]****。

## <a name="additional-references"></a>其他參考

-   [管理使用者的線上帳戶](Manage-Online-Accounts-for-Users.md)

-   [取得連線](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
