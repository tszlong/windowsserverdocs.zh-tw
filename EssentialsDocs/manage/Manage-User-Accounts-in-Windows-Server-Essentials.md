---
title: "管理使用者帳號，在 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>管理使用者帳號，在 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

Windows Server Essentials 儀表板的使用者頁面集中資訊及協助您的工作管理小型企業網路上的使用者帳號。 概觀使用者儀表板，請查看[儀表板概觀](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)。  
  
  
##  <a name="BKMK_ManageAccounts"></a>管理使用者帳號  
 下列主題會提供有關如何使用 Windows Server Essentials 儀表板的使用者帳號伺服器上的資訊：  
  
-   [[新增使用者 account](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [移除帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [檢視帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [變更帳號的顯示名稱](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [啟動帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [停用帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [了解帳號](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [管理使用者帳號使用儀表板](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>[新增使用者 account  
 當您新增帳號時，已指派的使用者可以登入網路，您可以讓使用者存取網路資源，例如共用的資料夾和遠端 Web 存取網站的權限。 Windows Server Essentials 包含 [新增使用者 Account 精靈可協助您：  
  
-   提供使用者 account 名稱和密碼。  
  
-   定義帳號，以系統管理員或使用者標準。  
  
-   選取的共用的資料夾帳號可以存取。  
  
-   指定帳號是否遠端存取該網路。  
  
-   如果有的話，請選取 [電子郵件選項。  
  
-   如果有的話，請指派的 Microsoft Online Services account （稱為在 Windows Server Essentials 的 Office 365 account）。  
  
-   指派給使用者群組 (僅限 Windows Server Essentials)。  
  
> [!NOTE]
>  -   Microsoft Azure Active Directory (Azure AD) 不支援 ASCII 非字元。 如果您的伺服器整合使用 Azure AD，請使用您的密碼，任何非 ASCII 字元。  
> -   只有如果您安裝的增益集，可提供的電子郵件服務提供的電子郵件的選項。  
  
##### <a name="to-add-a-user-account"></a>若要新增的使用者 account  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在**使用者工作**窗格中，按**新增使用者帳號**。 [新增使用者 Account 精靈會出現。  
  
4.  請依照下列指示完成精靈。  
  
###  <a name="BKMK_Remove"></a>移除帳號  
 當您選擇移除伺服器帳號時，精靈刪除選取的 account。 因此，您不再可以使用 account 網路登入或存取的任何網路資源。 選項，您也可以 delete 使用者 account 在此同時，您移除 account 的檔案。 如果您不希望永久移除帳號，您可以停用帳號改為暫停資源網路的存取權。  
  
> [!IMPORTANT]
>  如果使用者 account Microsoft online account 指派，當您移除帳號，online 也會移除帳號的 Microsoft Online Services，使用者的資料，包括電子郵件已受到資料保留在 Microsoft Online Services 的原則。 如果您想要保留使用者資料的 online 帳號，停用帳號，而不是將它移除。 如需詳細資訊，請查看[適用於使用者管理 Online 帳號](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account"></a>若要移除的使用者 account  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要移除的使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**移除帳號**。 Delete 使用者 Account 精靈會出現。  
  
5.  在**您想要保留檔案嗎？**頁面的精靈中，您可以選擇 delete 使用者的檔案，包含歷史檔案的備份，重新導向的資料夾的使用者 account。 若要讓使用者的檔案，空白核取方塊。 選擇之後，按一下 [**下一步**。  
  
6.  按一下**Delete account**。  
  
> [!NOTE]
>  之後您移除帳號，account 不會再出現在清單中帳號。 如果您選擇 delete 檔案，伺服器永久刪除使用者的資料夾的**使用者**資料夾伺服器從**檔案歷史備份**伺服器資料夾。  
>   
>  如果您擁有的整合式電子郵件提供者，也會移除的電子郵件帳號，指派給使用者 account。  
  
###  <a name="BKMK_Manage3"></a>檢視帳號  
 **使用者**Windows Server Essentials 儀表板的區段會顯示網路帳號的清單。 清單也提供每個 account 的其他資訊。  
  
##### <a name="to-view-a-list-of-user-accounts"></a>若要檢視帳號清單  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在主要瀏覽列中，按一下 [**使用者**。  
  
3.  儀表板顯示目前的帳號清單。  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>若要檢視，或變更帳號屬性  
  
1.  在清單中的使用者帳號，選取您要檢視，或變更屬性的 account。  
  
2.  在**< 使用者 Account\ > 工作**窗格中，按**檢視 account 屬性**。 **屬性**頁面上的使用者 account 顯示。  
  
3.  按一下 [索引標籤，以顯示該 account 功能的屬性。  
  
4.  若要儲存您對使用者 account 屬性的任何變更，請按一下**套用]**。  
  
###  <a name="BKMK_Manage4"></a>變更帳號的顯示名稱  
 顯示名稱為的名稱會出現在**名稱**欄在**使用者**頁面的儀表板。 變更顯示名稱不會變更登入或登入的使用者 account 名稱。  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>若要變更使用者 account 的顯示名稱  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要變更使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**檢視 account 屬性**。 **屬性**頁面上的使用者 account 顯示。  
  
5.  在**一般**索引標籤上，輸入新的**名字**並**姓氏**做為帳號，然後按一下**[確定]**。  
  
     新的顯示名稱會出現在清單中帳號。  
  
###  <a name="BKMK_Manage5"></a>啟動帳號  
 當您帳號，已指派的使用者可以登入的 account 有權限，例如共用的資料夾和遠端 Web 存取網站的網路及存取網路資源。  
  
> [!NOTE]
>  您可以僅限啟動帳號，停用。 移除伺服器之後，您無法啟動帳號。  
  
##### <a name="to-activate-a-user-account"></a>若要啟動帳號  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單檢視中，選取您想要啟動使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**啟動帳號**。  
  
5.  在 [確認] 視窗中，按一下**[是]**以確認您的動作。  
  
> [!NOTE]
>  Account 狀態啟動帳號後，會顯示**使用**。 使用者 account 重新取得的相同的存取權限已指派之前 account 停用。  
>   
>  如果您擁有的整合式電子郵件提供者，將也會啟動指派給使用者 account 的電子郵件帳號。  
  
###  <a name="BKMK_Manage6"></a>停用帳號  
 當您停用帳號時，請暫時暫停 account 伺服器的存取權。 此功能，因為已指派的使用者無法使用 account 存取網路資源，例如共用資料夾或存取的網頁網站，直到您啟動 account。  
  
 如果使用者 account 受指派的 Microsoft online 帳號，online 帳號也已停用。 使用者無法使用 Office 365 和其他 online services 您希望，但保留使用者的資料，包括電子郵件中的 Microsoft Online Services 的資源。  
  
> [!NOTE]
>  您只可以帳號，目前已停用。  
  
##### <a name="to-deactivate-a-user-account"></a>若要停用帳號  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單檢視中，選取您想要停止帳號。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**停用帳號**。  
  
5.  在 [確認] 視窗中，按一下**[是]**以確認您的動作。  
  
> [!NOTE]
>  停用帳號之後，account 狀態會顯示**非使用中**。  
>   
>  如果您擁有的整合式電子郵件提供者，電子郵件帳號指派給使用者 account 將也會停用。  
  
###  <a name="BKMK_Manage7"></a>了解帳號  
 Windows Server Essentials，這可讓存取資訊，會儲存在伺服器上，可讓使用者建立和管理他們的設定和檔案的人員帳號提供的重要資訊。 使用者可以登入在網路上的任何電腦如果他們使用的是 Windows Server Essentials 帳號，他們使用的是電腦的存取權限。 使用者存取它們帳號，使用者名稱與密碼。  
  
 有兩個主要帳號類型。 每個輸入可讓使用者控制電腦的不同層級：  
  
-   **標準**帳號是日常的願景。 標準 account 可協助保護您的網路，以防止使用者變更會影響其他使用者，例如刪除檔案或變更網路設定。  
  
-   **系統管理員**帳號提供最多的控制權電腦網路。 您應該指派管理員考量需要時才類型。  
  
###  <a name="BKMK_Manage8"></a>管理使用者帳號使用儀表板  
 Windows Server Essentials 可讓您可以使用 Windows Server Essentials 儀表板執行一般管理工作。 根據預設，**使用者**頁面上的儀表板包含兩個索引標籤**使用者**和**使用者群組]**。  
  
> [!NOTE]
>  -   如果您執行 Windows Server Essentials 的 Office 365 的伺服器整合，新的索引標籤稱為**通訊群組**也新增了在**使用者**頁面的儀表板。  
> -   在 Windows Server Essentials，**使用者**頁面上的儀表板包含只有一個索引標籤-**使用者**。  
  
 **使用者**索引標籤包含動作：  
  
-   一份帳號，它會顯示：  
  
    -   使用者名稱。  
  
    -   登入的使用者 account 名稱。  
  
    -   是否帳號有任何地方存取權限。 任何地方存取權限的使用者 account 是**允許**或**不允許**。  
  
    -   此使用者帳號歷史檔案是否受 「 執行 Windows Server Essentials 的伺服器。 使用者 account 檔案歷史狀態是**管理**或**不受管理的**。  
  
    -   已指派給使用者 account 的存取層級。 您可以指定任一個**標準使用者**存取或**系統管理員**帳號存取。  
  
    -   使用者 account 狀態。 使用者 account 可以**使用**，**非使用中**，或**完整**。  
  
    -   在 Windows Server Essentials，如果伺服器整合 Office 365 或 Windows Intune，會顯示 Microsoft online account。  
  
    -   在 Windows Server Essentials，如果伺服器整合使用 Microsoft Office 365，會顯示使用者 account 的 Office 365 帳號 （在 Windows Server Essentials 的 Microsoft online 帳號為已知） 的狀態。  
  
-   選取的帳號的其他資訊的詳細資料窗格。  
  
-   工作] 窗格中，其中包含：  
  
    -   檢視及移除帳號，並變更密碼，例如使用者 account 管理工作一組。  
  
    -   可讓您全球設定或變更所有使用者帳號，在網路設定的工作。  
  
 下表描述各種不同的使用者 account 工作，可從**使用者**索引標籤。 一些工作 account 特定的使用者且都是只顯示當您在清單中選取 [使用者 account。  
  
> [!NOTE]
>  如果您是與 Windows Server Essentials 整合 Office 365，才可使用其他工作。 如需詳細資訊，請查看[適用於使用者管理 Online 帳號](Manage-Online-Accounts-for-Users.md)。  
  
### <a name="user-account-tasks-in-the-dashboard"></a>儀表板中的使用者 account 工作  
  
|工作的名稱|描述|  
|---------------|-----------------|  
|檢視 account 屬性|可讓您以檢視及變更選取的帳號的屬性，指定資料夾的存取權限 account。|  
|停用帳號|停用帳號網路或存取網路資源，例如共用的資料夾或的印表機無法登入。|  
|啟動帳號|使用者 account 會變成網路可以登入，可以存取網路資源，所定義 account 權限。|  
|移除帳號|可讓您移除選取的使用者 account。|  
|變更使用者密碼|可讓您重設選取的帳號網路密碼。|  
|[新增使用者 account|會開始新增使用者 Account 精靈，如此可讓您建立單一的新使用者 account 具有標準使用者存取的系統管理員。|  
|指派的 Microsoft online account|將選取的區域網路使用者帳號 Microsoft online account。<br /><br /> 當您的伺服器整合 Microsoft online 服務，例如 Office 365，會顯示這項工作。|  
|加入 Microsoft online 帳號|加入 Microsoft online 帳號，並將它們相關聯的區域網路使用者帳號。<br /><br /> 當您的伺服器整合 Microsoft online 服務，例如 Office 365，會顯示這項工作。|  
|設定密碼原則|可讓您可以變更密碼原則的網路。|  
|匯入 Microsoft online 帳號|執行 Microsoft online services 的帳號大量匯入到本機網路。<br /><br /> 當您的伺服器整合 Microsoft online 服務，例如 Office 365，會顯示這項工作。|  
|重新整理|重新整理使用者] 索引標籤。<br /><br /> 這個任務適用於 Windows Server Essentials。|  
|變更檔案歷史設定|可讓您變更檔案歷史設定，例如備份頻率或備份的持續時間。<br /><br /> 這個任務適用於 Windows Server Essentials。|  
|匯出所有遠端連接|建立。所有遠端伺服器連接過去 30 天之後發生的 CSV 格式的檔案。|  
  
##  <a name="BKMK_ManageAccess"></a>存取和密碼管理  
 下列主題會提供有關如何使用 Windows Server Essentials 儀表板管理使用者 account 密碼和使用者存取伺服器上的共用資料夾：  
  
-   [變更或重設使用者 account 的密碼](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [您應該會了解如何密碼原則](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [變更密碼原則](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [層級的共用資料夾的存取權](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [保留存取及管理帳號已移除的使用者的檔案](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [同步 DSRM 密碼的網路系統管理員密碼](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [允許帳號遠端桌面](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [讓使用者存取伺服器上的資源](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [變更帳號遠端存取權限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [變更帳號 virtual 私人網路權限](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [變更存取帳號內部共用資料夾](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [讓使用者帳號，建立到其電腦的遠端桌面工作階段](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>變更或重設使用者 account 的密碼  
 若要變更或重設使用者密碼，請依照下列步驟執行。  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>若要重設使用者 account 的密碼  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要重設使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**變更使用者 account 密碼**。 [變更使用者 Account 密碼精靈會出現。  
  
5.  輸入使用者帳號，新的密碼，然後輸入密碼來確認它。  
  
6.  按一下**變更密碼**。  
  
7.  向使用者提供新的密碼。  
  
    > [!IMPORTANT]
    >  -   您可能無法變更您的密碼，如果您的密碼原則為**永久有效的密碼**。  
    > -   不支援 Azure AD 非 ASCII 字元。 因此，如果您的伺服器整合使用 Azure AD 時，請勿使用任何非 ASCII 字元您的密碼。  
    > -   Microsoft online account （中 Office 365 account 與 Windows Server Essentials 已知） 已指派給使用者，如果 online 密碼同步處理的密碼。 使用者將使用新密碼來登入在伺服器上或是登入 Office 365。 如需詳細資訊，請查看[適用於使用者管理 Online 帳號](Manage-Online-Accounts-for-Users.md)。  
  
###  <a name="BKMK_Access3"></a>您應該會了解如何密碼原則  
 密碼原則是一組規則的定義使用者建立和使用密碼的方式。 協助防止未經授權的存取權的使用者資料和其他資訊會儲存在伺服器上的原則。 密碼原則會套用到所有的使用者帳號存取該網路。  
  
 Windows Server Essentials 密碼原則有 3 個主要元素，如下所示：  
  
-   **密碼長度**。  較長的時間密碼是，更安全。 空白的密碼並不安全。  
  
-   **複雜密碼**。  複雜密碼包含混合和小寫 (為-z，A Z)，(0-9)，以及非字母符號的基底 (例如; ！，@，#、 _、-)。 複雜密碼的較容易未經授權的存取。 包含的使用者名稱、 生日或其他個人資訊的密碼不提供安全性。  
  
-   **密碼年齡**。  Windows Server Essentials 需要使用者變更密碼至少一次每個 180 天。 選項，您可以選擇將永久有效的密碼。  
  
 若要讓您更輕鬆地在您的電腦網路實作密碼原則、 Windows Server Essentials 提供簡單的工具，可讓您設定或變更密碼原則任一下列四個預先定義的原則設定檔：  
  
-   **低**。  使用者可以指定任何不是空白的密碼。  
  
-   **媒體**。  這些密碼必須包含至少 5 個字元。 不需要複雜密碼。  
  
-   **媒體強**。  必須包含至少 5 字元，這些密碼，並必須包含字母、 數字和符號。  
  
-   **強**。  必須包含至少 7 字元，這些密碼，並必須包含字母、 數字和符號。 這些密碼更安全，但可能會更難以記住使用者。  
  
    > [!NOTE]
    >  密碼不能的使用者名稱或電子郵件地址。  
    >   
    >  如果您使用 Office 365 整合，整合執行**強**密碼原則，和更新的原則，包含下列需求：  
    >   
    >  -   密碼必須包含 8 16 字元。  
    > -   密碼不能空格或 Office 365 郵件名稱。  
  
 根據預設，安裝伺服器將預設密碼的原則設定為**強**選項。  
  
###  <a name="BKMK_Access4"></a>變更密碼原則  
 使用下列程序，設定或變更密碼原則任何四個預先定義的原則設定檔。  
  
##### <a name="to-change-the-password-policy"></a>若要變更密碼原則  
  
1.  打開 Windows Server Essentials 儀表板，，然後按一下**使用者**。  
  
2.  在**使用者工作**窗格中，按**設定密碼原則**。  
  
3.  在**變更密碼原則，**畫面上，移動滑設定的層級的力量的密碼。  
  
     Microsoft 建議您將密碼越設定為**強**。  
  
    > [!NOTE]
    >  選項，您也可以選取**永久有效的密碼**。 此設定為較不安全，並讓我們不建議。  
  
4.  按一下**變更原則**。  
  
###  <a name="BKMK_Access5"></a>層級的共用資料夾的存取權  
 做為最佳做法，您應該指派最低使用權限提供仍可讓使用者執行所需的工作。  
  
 您有伺服器上的共用資料夾三個存取設定：  
  
-   **讀取/寫入**。  如果您想要讓使用者 account 的權限來建立、 變更或 delete 共用資料夾中的任何檔案，請選擇這個設定。  
  
-   **唯讀**。  如果您想要讓使用者 account 的權限只讀取的共用資料夾中的檔案，請選擇這個設定。 唯讀存取帳號無法建立、 變更或 delete 共用資料夾中的任何檔案。  
  
-   **不能存取**。  如果您不想要存取的共用資料夾中的任何檔案帳號，請選擇這個設定。  
  
###  <a name="BKMK_Access6"></a>保留存取及管理帳號已移除的使用者的檔案  
 網路系統管理員可以移除帳號，並選擇要保留使用者 s 未來使用的檔案。 在本案例中，移除的帳號不再可用來登入網路。不過，這位使用者的檔案將會儲存在共用資料夾，這可使用另一位使用者。  
  
> [!IMPORTANT]
>  請注意，如果您移除已受指派的 Microsoft online account 帳號，也會移除 online 帳號，及使用者資料，包括電子郵件，皆受 Microsoft Online Services 的資料保留原則。 適用於 online account 使用者資料的保留，停用帳號，而不是將它移除。 如需詳細資訊，請查看[適用於使用者管理 Online 帳號](Manage-Online-Accounts-for-Users.md)。  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>若要移除帳號，但保留使用者的檔案的存取權  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要移除的使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**移除帳號**。 Delete 使用者 Account 精靈會出現。  
  
5.  在**您想要保留檔案嗎？**頁面上，請確定**Delete 包括備份檔案歷史及此帳號重新導向的資料夾的檔案**核取方塊已清除、，然後按一下**下一步**。  
  
     確認頁面會顯示警告您正在刪除帳號，但保留檔案。  
  
6.  按一下**Delete account**若要移除的使用者帳號。  
  
 已移除使用者 account 之後，系統管理員可以讓共用資料夾其他使用者 account 存取。  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>若要讓使用者 account 共用的資料夾的存取權限  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**儲存**，然後按一下 [**伺服器資料夾**索引標籤。  
  
3.  在清單的資料夾中，選取 [**使用者**資料夾。  
  
4.  在**使用者工作**窗格中，按**打開資料夾**。 Windows 檔案總管開啟並顯示到**使用者**資料夾。  
  
5.  以滑鼠右鍵按一下您想要分享，然後按一下 [使用者 account 資料夾**屬性**。  
  
6.  在**< 使用者 Account\ > 屬性**，按一下 [**共用**索引標籤，然後按一下 [**共用**。  
  
7.  在**檔案共用**視窗中，輸入，或選取您想要共用資料夾，然後按一下 [使用者 account 名稱**新增]**。  
  
8.  選擇 [**的權限等級**您想要帳號，並再按**共用**。  
  
###  <a name="BKMK_Access7"></a>同步 DSRM 密碼的網路系統管理員密碼  
 Directory 服務還原模式 (DSRM) 為修復或復原 Active Directory 特殊開機模式。 作業系統使用 DSRM Active Directory 失敗是否需要還原電腦登入。 如果您的網路系統管理員密碼及密碼 DSRM 不同，將不會載入 DSRM。  
  
 Windows Server Essentials 的全新、 第一次安裝期間計畫將設定您在設定期間，或移轉回應檔案中指定的網路系統管理員密碼 DSRM 密碼。 當您變更您的網路系統管理員密碼 （如同建議通常會增加的伺服器安全性每個 60 天） 時，以 DSRM 不轉送變更密碼。 這會導致密碼不相符。 發生這種情形，如果您可以使用下列方案以手動或自動 DSRM 密碼同步您的網路系統管理員的密碼。  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>若要手動同步的網路系統管理員帳號 DSRM 密碼  
  
1.  在命令提示字元中執行`ntdsutil.exe`打開 ntdsutil 工具。  
  
2.  若要重設密碼 DSRM，輸入**設定 dsrm 密碼**。  
  
3.  若要同步的網域控制站的 DSRM 密碼與目前的網路系統管理員的帳號，請輸入：  
  
     **同步核對從** *< current_network_administrator_account >*，然後按 Enter 鍵。  
  
 您將會定期變更的密碼的網路管理員帳號，以確保 DSRM 密碼都相同的網路系統管理員目前的密碼，我們建議您自動建立排程工作，因為每天同步 DSRM 密碼的網路系統管理員密碼。  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>若要自動同步處理的網路系統管理員帳號 DSRM 密碼  
  
1.  從伺服器，請打開**系統管理工具]**，然後按兩下 [**工作排程器**。  
  
2.  在 [工作排程器**動作**窗格中，按**建立工作**。  
  
3.  在**名稱**文字] 方塊中，輸入名稱，例如工作**AutoSync DSRM 密碼**，]，然後選取**以最高的權限來執行**選項。  
  
4.  定義何時執行工作：  
  
    1.  在**建立工作**對話方塊中，按一下 [**觸發程序**索引標籤，然後按一下 [**新增]**。  
  
    2.  在**新觸發程序**對話方塊中，選擇循環，指定循環長的時間間隔，然後選擇開始的時間。  
  
        > [!NOTE]
        >  做為最佳做法，您應該設定執行每天在非商業時間工作。  
  
    3.  按一下**[確定]**以儲存您的變更，並回到**建立工作**對話方塊。  
  
5.  定義工作動作：  
  
    1.  按一下**動作**索引標籤，然後按一下 [**新增]**。 **動作]**對話方塊中出現。  
  
    2.  在**動作**清單中，按一下 [**程式 [開始]**，然後瀏覽至**C:\WINDOWS\SYSTEM32\ntdsutil.exe**。  
  
    3.  在**新增引數**（選擇性） 的文字方塊中，輸入的下列命令 （您必須包含引號）：**設定核對 SBS_network_administrator_account q q dsrm 密碼同步**其中*SBS_network_administrator_account*是目前的網路系統管理員 s account 名稱。  
  
6.  按一下**[確定]**兩次，若要儲存工作，並關閉**建立工作**對話方塊。 新的任務會出現在**作用中的工作**區段**工作排程**。  
  
###  <a name="BKMK_Access8"></a>允許帳號遠端桌面  
 在 Windows Server Essentials 的預設安裝網路使用者無權來連接遠端電腦或其他網路上的資源。  
  
 網路使用者可以連接遠端到網路資源之前，您必須第一次設定隨處存取。 隨處存取設定之後，使用者可以存取檔案、 應用程式，以及 office 網路中的電腦的任何位置與網際網路連接裝置。  
  
 隨處存取精靈設定可讓您兩種遠端存取的方法：  
  
-   Virtual 私人網路 (VPN)  
  
-   遠端存取  
  
 當您執行精靈中時，您也可以選擇允許所有使用者的目前與新增帳號隨處存取。  
  
 隨處存取設定，請打開儀表板**Home**頁面上，按一下 [**設定**，，然後按一下 [**設定隨處存取**。  
  
 如需任何地方存取的相關資訊，請查看[管理隨處存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_Access9"></a>讓使用者存取伺服器上的資源  
  本節適用於已安裝 Windows Server Essentials 體驗角色執行 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter server 或執行的是 Windows Server Essentials 或 Windows Server Essentials，伺服器。  
  
 如果您希望使用者使用遠端存取，並/或已登入電腦，當您完成伺服器連接電腦時，您可以建立新的網路帳號網路的電腦上的使用者在伺服器上使用儀表板。 如需有關建立帳號，請查看[[新增使用者 account](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。 建立的使用者帳號之後, 您必須 client 電腦的使用者提供的網路使用者名稱和密碼資訊，讓他們可以利用 Launchpad 存取伺服器上的資源。  
  
 為您建立的每個使用者帳號您可以設定 account 屬性使用者透過下列存取：  
  
-   **共用資料夾**。  預設的網路系統管理員可以**讀取/寫入**有權限所有的共用的資料夾和標準使用者帳號**唯讀**資料夾公司的權限。 如果支援媒體串流處理時，您可以指定資料夾的存取權限標準登入電腦下列共用資料夾：**音樂**，**圖片**、**錄製電視**，和**影片**。 您可以設定的權限存取上的共用的資料夾帳號**的共用資料夾**索引標籤上的使用者 account 屬性。  
  
-   **隨處存取**。  預設的網路系統管理員可以使用 VPN 或遠端 Web 存取存取伺服器資源。 對於標準使用者帳號，您必須設定使用者 account 權限在**隨處存取**索引標籤。  
  
-   **存取電腦**。  預設的網路系統管理員可以存取網路中的所有的電腦。 不過的標準帳號您可以設定個人使用者 account 權限存取網路上的電腦上的**電腦存取**索引標籤上的使用者 account 屬性。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>若要編輯的使用者 account 屬性，在 Windows Server Essentials 2012 R2  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您要編輯的使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**檢視 account 屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，執行下列動作：  
  
    1.  在**的共用資料夾**索引標籤上，視需要設定分享的每個資料夾的資料夾適當權限。  
  
    2.  在**隨處存取**索引標籤：  
  
        1.  若要讓使用者使用 VPN 伺服器連接，請選取 [**允許 Virtual 私人網路 (VPN)**核取方塊。  
  
        2.  若要讓使用者透過遠端 Web 存取伺服器連接，請選取 [**允許遠端 Web Access 與網路服務] 應用程式存取**核取方塊。  
  
    3.  在**電腦存取**索引標籤上，選取您想要存取的使用者的網路電腦。  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>若要編輯的使用者 account 屬性 Windows Server 程式集 2012 」 中  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您要編輯的使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，執行下列動作：  
  
    1.  在**一般**索引標籤，選取**使用者都可以檢視網路健康警示**如果帳號需要存取網路健康報告。  
  
    2.  在**的共用資料夾**索引標籤上，視需要設定分享的每個資料夾的資料夾適當權限。  
  
    3.  在**隨處存取**索引標籤：  
  
        1.  若要讓使用者使用 VPN 伺服器連接，請選取 [**允許 Virtual 私人網路 (VPN)**核取方塊。  
  
        2.  若要讓使用者透過遠端 Web 存取伺服器連接，請選取 [**允許遠端 Web Access 與網路服務] 應用程式存取**核取方塊。  
  
    4.  在**電腦存取**索引標籤上，選取您想要存取的使用者的網路電腦。  
  
###  <a name="BKMK_Access10"></a>變更帳號遠端存取權限  
 使用者可以存取資源位於從遠端伺服器上使用 virtual 私人網路 (VPN)、 遠端網路存取或其他網路服務] 應用程式。 根據預設，遠端存取權限的亮起來網路使用者當您設定 Windows Server Essentials 隨處存取使用儀表板。  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>若要變更使用者 account 遠端存取權限  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要變更使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**檢視 account 屬性**。 **屬性**頁面上的使用者 account 顯示。  
  
5.  在**隨處存取**索引標籤上，執行下列動作：  
  
    -   選取 [**允許 Virtual 私人網路 (VPN)**核取方塊，允許使用者連伺服器上使用 VPN。  
  
    -   選取 [**允許遠端 Web Access 與網路服務] 應用程式存取**核取方塊，允許使用者透過遠端 Web 存取伺服器連接。  
  
6.  按一下**套用**，然後按**[確定]**。  
  
###  <a name="BKMK_Access11"></a>變更帳號 virtual 私人網路權限  
 您可以使用 (VPN) virtual 私人網路連接到 Windows Server Essentials 並存取所有您儲存在伺服器的資源。 這是您的電腦與網路帳號，可以用來連接的 VPN 連接到 Windows Server Essentials 伺服器裝載設定使用 client 的尤其是實用。 Windows Server Essentials 裝載伺服器上的所有新建立的使用者帳號必須使用 VPN 第一次登入 client 的電腦。  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>若要變更 VPN 網路使用者的權限  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要從遠端存取桌面的權限授與的使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，按一下 [**隨處存取**索引標籤。  
  
6.  在**隨處存取**索引標籤，讓使用者使用 VPN、 連接到伺服器選取**允許 Virtual 私人網路 (VPN)**核取方塊。  
  
7.  按一下**套用**，然後按**[確定]**。  
  
###  <a name="BKMK_Access12"></a>變更存取帳號內部共用資料夾  
 您可以管理任何伺服器上的共用資料夾的存取權的工作，利用**資料夾伺服器**索引標籤的儀表板。 根據預設，下列伺服器資料夾會建立當您安裝 Windows Server Essentials:  
  
-   **Client 電腦備份**。  用來儲存 client 電腦建立的備份，Windows Server 備份。 不會共用此伺服器資料夾。  
  
-   **公司**。  用來儲存和存取您的組織網路使用者的相關文件。  
  
-   **檔案歷史備份**。  根據預設，Windows Server Essentials 儲存檔案使用檔案歷史所建立的備份。 不會共用此伺服器資料夾。  
  
-   **資料夾重新導向**。  用來儲存和存取資料夾的資料夾重新導向網路使用者的設定。 不會共用此伺服器資料夾。  
  
-   **音樂**。  用來儲存和存取網路使用者的音樂檔案。 當您要共用的媒體建立資料夾。  
  
-   **圖片**。  用來儲存和存取網路使用者的圖片。 當您要共用的媒體建立資料夾。  
  
-   **錄製電視**。  用來儲存和存取錄製的網路使用者的電視節目。 當您要共用的媒體建立資料夾。  
  
-   **影片**。  用來儲存和存取網路使用者的影片。 當您要共用的媒體建立資料夾。  
  
-   **使用者**。  用來儲存和存取網路使用者的檔案。 使用者特定資料夾自動也在**使用者**伺服器的每個網路帳號，您所建立的資料夾。  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>若要變更帳號使用者的共用資料夾的存取  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  瀏覽並選取 [伺服器] 資料夾，您想要修改權限。  
  
4.  在 [工作] 窗格中，按一下**檢視資料夾屬性**。  
  
5.  在**< FolderName\ > 屬性**，按一下 [**共用**，並選取適當的使用者存取層級列出的帳號，然後按一下 [**套用]**。  
  
    > [!NOTE]
    >  您無法修改共用的權限的**檔案歷史備份**，**資料夾重新導向**，並**使用者**伺服器資料夾。 因此，不包含下列伺服器資料夾的資料夾屬性**共用**索引標籤。  
  
###  <a name="BKMK_Access13"></a>讓使用者帳號，建立到其電腦的遠端桌面工作階段  
  本節適用於已安裝 Windows Server Essentials 體驗角色執行 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter server 或執行的是 Windows Server Essentials 或 Windows Server Essentials，伺服器。  
  
 網路系統管理員可以存取他們網路的電腦從遠端位置允許的網路使用者給予權限。  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>若要讓使用者可以存取他們網路的電腦從遠端位置  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要從遠端存取桌面權限授與帳號。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，按一下 [**電腦存取**索引標籤。  
  
6.  選取您想要這個帳號，將無法存取遠端電腦上，，然後按一下 [電腦**[確定]**。  
  
## <a name="see-also"></a>也了  
  
-   [Online 帳號管理使用者](Manage-Online-Accounts-for-Users.md)  
  
-   [連接](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
