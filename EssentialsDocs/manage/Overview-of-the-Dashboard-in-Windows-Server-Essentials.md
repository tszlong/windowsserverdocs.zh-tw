---
title: Windows Server Essentials 中的儀表板概觀
description: 瞭解系統管理儀表板，此儀表板可簡化您管理 Windows Server Essentials 網路和伺服器所執行的工作。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f70a79de-9c56-4496-89b5-20a1bff2293e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4c9a5464d2a33d0116b38befd5a572407cac5b3a
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811155"
---
# <a name="overview-of-the-dashboard-in-windows-server-essentials"></a>Windows Server Essentials 中的儀表板概觀

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 Windows Server Essentials 和已啟用「Windows Server Essentials 體驗」角色的 Windows Server 2012 R2 Standard 都包含一個系統管理儀表板，可簡化您為管理 Windows Server Essentials 網路和伺服器而執行的工作。 藉由使用 [Windows Server Essentials 儀表板]，您可以：

- 完成您的伺服器設定

- 存取和執行一般系統管理工作

- 檢視伺服器警示並對其採取動作

- 設定和變更伺服器設定

- 存取或搜尋網路上的說明主題

- 存取網路上的社群資源

- 管理使用者帳戶

- 管理裝置和備份

- 管理伺服器資料夾和硬碟的存取權與設定

- 檢視和管理增益集應用程式

- 與 Microsoft 線上服務整合

  本主題包含：

- [儀表板的基本功能](#BKMK_Design)

- [儀表板首頁的功能](#BKMK_Home)

- [儀表板的系統管理區段](#BKMK_Features)

- [存取 Windows Server Essentials 儀表板](#BKMK_AccessDb)

- [使用安全模式](#BKMK_UseSafeMode)

##  <a name="basic-features-of-the-dashboard"></a><a name="BKMK_Design"></a> 儀表板的基本功能
 [Windows Server Essentials 儀表板] 可協助您快速存取您伺服器的重要資訊和管理功能。 儀表板包含數個區段。 接下來的表格描述這些區段。



|Item|儀表板功能|描述|
|----------|-----------------------|-----------------|
|1|導覽列|按一下瀏覽列上的區段以存取與該區段關聯的資訊和工作。 每次您開啟 [儀表板] 時，預設會顯示 [首頁]。|
|2|子區段索引標籤|子區段索引標籤可讓您存取第二層的 Windows Server Essentials 系統管理工作。|
|3|清單窗格|清單檢視會顯示您可以管理的物件，並包含每個物件的基本資訊。|
|4|[詳細資料] 窗格|詳細資料窗格會顯示您在清單檢視中所選物件的其他相關資訊。|
|5|工作窗格|[工作] 窗格包含工具和資訊的連結，可協助您管理特定物件 (例如使用者帳戶或電腦) 的內容，或物件類別之全域設定的內容。 [工作] 窗格分為下列兩個區段：<br /><br /> [**物件** 工作] –包含工具和資訊的連結，可協助您管理在清單視圖中選取之物件的屬性， (例如使用者帳戶或電腦) 。<br /><br /> **全域**  工作–包含工具和資訊的連結，可協助您管理功能區域的全域工作。 全域工作包括新增物件、設定原則等工作。|
|6|資訊和設定|這個區段可讓您直接存取伺服器設定，並提供您正在檢視之儀表板頁面的相關資訊說明連結。|
|7|警示狀態|警示狀態提供有關伺服器健康情況的快速視覺指示。 按一下警示影像以檢視重大和重要的警示。<br /><br /> **注意：** 在 Windows Server Essentials 和已安裝「Windows Server Essentials 體驗」角色的 Windows Server 2012 R2 Standard 中，[ **資訊和設定** ] 索引標籤上會提供警示狀態。|
|8|狀態列|狀態列會顯示在清單檢視中出現的物件數目。 增益集應用程式可能也會顯示其他狀態。|

##  <a name="features-of-the-dashboard-home-page"></a><a name="BKMK_Home"></a> [儀表板] 首頁的功能
 當您開啟 [儀表板] 時，預設為顯示 [首頁] 並顯示 [設定] 類別。 [Windows Server Essentials 儀表板] 的 [首頁] 可讓您快速存取可協助您自訂伺服器及設定重要功能的工作和資訊。 [首頁] 是由四個功能區域所組成，會針對您所選取的選項顯示資訊和設定工作。 接下來的表格描述這些功能。

|Item|功能|描述|
|----------|-------------|-----------------|
|1|巡覽列|按一下瀏覽列上的區段以存取與該區段關聯的資訊和工作。 每次您開啟儀表板時，預設會顯示 [首頁]。|
|2|類別窗格|這個窗格顯示功能區域，可讓您快速存取可協助您設定及自訂伺服器的資訊和設定工具。 按一下類別以顯示與該類別關聯的工作和資源。|
|3|工作窗格|這個窗格顯示適用於所選類別之工作和資訊的連結。 按一下任務或資源以顯示其他資訊。|
|4|動作窗格|這個窗格提供功能或工作的簡短描述，並提供可開啟設定精靈和資訊頁面的連結。 按一下連結以採取進一步的動作。|

##  <a name="administrative-sections-of-the-dashboard"></a><a name="BKMK_Features"></a> 儀表板的系統管理區段
 [Windows Server Essentials 儀表板] 會將網路資訊和系統管理工作組織成功能區域。 每個功能區域的管理網頁皆提供與該區域關聯之物件 (例如 [使用者] 或 [裝置]) 的相關資訊。 管理頁面也包含您可用來檢視或變更設定的工作，或可用來執行程式 (可將需要多個步驟的工作自動化) 的工作。

 下表說明安裝後預設可用的 [儀表板] 系統管理區段。 這個表格也列出每個區段內可用的工作。

|區段|描述|
|-------------|-----------------|
|首頁|[首頁] 預設會在您每次開啟 [儀表板] 時顯示。 它包含下列類別的工作和資訊：<br /><br /> **安裝**  –完成此類別中的工作，以在第一次設定您的伺服器。 如需這些工作的相關資訊，請參閱 [安裝和設定 Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md)。<br /><br /> **電子郵件**  –選擇這個類別中的選項，將電子郵件服務與伺服器整合。<br /><br /> **注意：** 此類別僅適用于 Windows Server Essentials。<br /><br /> **服務**  –選擇這個類別中的工作，將 Microsoft 線上服務與伺服器整合。<br /><br /> **注意：** 此類別僅適用于已啟用「Windows Server Essentials 體驗」角色的 Windows Server Essentials 和 Windows Server 2012 R2 Standard。<br /><br /> **增益集**  –按一下這個類別，即可為您的公司安裝有用的增益集。<br /><br /> [**快速狀態**] –會顯示高層級的伺服器狀態。 按一下狀態以檢視該功能的資訊和設定選項。 如果您完成 [設定] 類別中的所有工作，這個類別就會出現在 [類別] 窗格的頂端。<br /><br /> 說明 **–使用**[搜尋] 方塊來搜尋網路上的說明。 按一下連結以瀏覽所選支援選項的網站。|
|使用者|若要讓使用者存取 Windows Server Essentials 所提供的資源，您需要使用 [Windows Server Essentials 儀表板] 來建立使用者帳戶。 建立使用者帳戶之後，您可以使用 [儀表板] 之 [使用者] 頁面上可用的工作來管理帳戶。 您可以在這個頁面上執行的工作包括：<br /><br /> -查看使用者帳戶清單。<br /><br /> -查看和管理使用者帳戶屬性。<br /><br /> -啟用或停用使用者帳戶。<br /><br /> -新增或移除使用者帳戶。<br /><br /> -如果您的伺服器已與 Microsoft 365 整合，請將區域網路帳戶指派給 Microsoft 線上服務帳戶。<br /><br /> -變更使用者帳戶密碼和管理密碼原則。<br /><br /> 如需管理使用者帳戶的相關資訊，請參閱 [管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md)。|
|使用者群組|**注意：** 這項功能僅適用于已啟用「Windows Server Essentials 體驗」角色的 Windows Server Essentials 和 Windows Server 2012 R2 Standard。<br /><br /> 您可以在這個頁面上執行的工作包括：<br /><br /> -查看使用者群組清單。<br /><br /> -查看和管理使用者群組。<br /><br /> -新增或移除使用者群組。|
|通訊群組|**注意：** 這項功能僅適用于已啟用「Windows Server Essentials 體驗」角色的 Windows Server Essentials 和 Windows Server 2012 R2 Standard。 只有當 Windows Server Essentials 已與 Microsoft 365 整合時，才會顯示此索引標籤。<br /><br /> 您可以在這個頁面上執行的工作包括：<br /><br /> -查看通訊群組的清單。<br /><br /> -新增或移除通訊群組。|
|裝置|在您將電腦連線到 Windows Server Essentials 網路之後，您可以從 [儀表板] 的 [裝置] 頁面管理電腦。 您可以在這個頁面上執行的工作包括：<br /><br /> -查看已加入網路的電腦清單。<br /><br /> -利用 Microsoft 365 行動裝置管理功能來管理行動裝置。<br /><br /> **注意：** 這項功能僅適用于已啟用「Windows Server Essentials 體驗」角色的 Windows Server Essentials 和 Windows Server 2012 R2 Standard。<br /><br /> -查看每部電腦的電腦內容和健全狀況警示。<br /><br /> -設定和管理電腦備份。<br /><br /> -將檔案和資料夾還原至電腦。<br /><br /> -建立與電腦的遠端桌面連線<br /><br /> -自訂電腦備份和檔案歷程記錄設定<br /><br /> 如需管理電腦和備份的相關資訊，請參閱 [管理裝置](Manage-Devices-in-Windows-Server-Essentials.md)。|
|儲存體|視您目前執行的 Windows Server Essentials 版本而定，[儀表板] 的 [存放] 區段預設會包含下列各區段：<br /><br /> -[ **伺服器資料夾** ] 子區段包含可協助您查看和管理伺服器資料夾屬性的工作。 這個頁面也包含可開啟和新增伺服器資料夾的工作。<br /><br /> -[ **硬碟** ] 頁面包含可協助您查看和檢查連接到伺服器之磁片磁碟機健康情況的工作。<br /><br /> -在 Windows Server Essentials 和已啟用「Windows Server Essentials 體驗」角色的 Windows server 2012 R2 Standard 中，[ **SharePoint 程式庫** ] 頁面包含的工作可協助您管理 Microsoft 365 service 中的 SharePoint 文件庫。<br /><br /> 如需管理伺服器資料夾的相關資訊，請參閱 [管理伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md)。<br /><br /> 如需管理硬碟的相關資訊，請參閱 [管理伺服器儲存體](Manage-Server-Storage-in-Windows-Server-Essentials.md)。|
|應用程式|-Windows Server Essentials 儀表板的 [ **應用程式** ] 區段預設包含兩個子區段。<br /><br /> 如需管理增益集應用程式的相關資訊，請參閱 [管理應用程式](Manage-Applications-in-Windows-Server-Essentials.md)。<br /><br /> -[ **增益集** ] 子區段會顯示已安裝的增益集清單，並提供可讓您移除增益集及存取所選增益集之其他相關資訊的工作。<br /><br /> -Microsoft 的 [ **找** 出] 子區段會顯示可從 Microsoft 定點取得的應用程式清單。|
|Microsoft 365|只有當 Windows Server Essentials 已與 Microsoft 365 整合時，才會顯示 [ **Microsoft 365** ] 索引標籤。 本節包含 Microsoft 365 的訂用帳戶和系統管理員帳戶資訊。|

> [!NOTE]
>  如果您安裝適用於 [Windows Server Essentials 儀表板] 的增益集，該增益集可能會建立額外的系統管理區段。 這些區段可能會出現在主導覽列或子區段索引標籤上。

##  <a name="access-the-windows-server-essentials-dashboard"></a><a name="BKMK_AccessDb"></a> 存取 Windows Server Essentials 儀表板
 您可以使用下列其中一種方法來存取 [儀表板]。 您所選擇的方法取決於您是從伺服器、從連線到 Windows Server Essentials 網路的電腦、還是從遠端電腦存取儀表板。

 為了維護網路安全，只有具備系統管理權限的使用者可以存取 [Windows Server Essentials 儀表板]。 此外，您也無法使用內建的系統管理員帳戶從 [啟動列] 登入 [Windows Server Essentials 儀表板]。

###  <a name="access-the-dashboard-from-the-server"></a><a name="BKMK_Server"></a> 從伺服器存取儀表板
 當您安裝 Windows Server Essentials 時，安裝程序會在 [開始] 畫面和桌面上都建立一個 [儀表板] 捷徑。 如果這些位置中沒有捷徑，您可以使用 [搜尋] 窗格來尋找並執行 [儀表板] 程式。

##### <a name="to-access-the-dashboard-from-the-server"></a>從伺服器存取儀表板

-   以系統管理員身分登入伺服器，然後執行下列其中一個動作：

    -   在 [開始] 畫面上，按一下 [儀表板]。

    -   在桌面上，按兩下 [儀表板]。

    -   在 [搜尋] 窗格中，輸入 **dashboard**，然後按一下搜尋結果中的 [儀表板]。

###  <a name="access-the-dashboard-from-a-computer-that-is-connected-to-the-network"></a><a name="BKMK_Network"></a> 從連線到網路的電腦存取儀表板
 在 Windows Server Essentials 中，在您將電腦加入網路之後，系統管理員便可使用 [啟動列] 從電腦存取伺服器 [儀表板]。

> [!WARNING]
>  若要從用戶端電腦連線至儀表板，請在 Windows Server Essentials 中，以滑鼠右鍵按一下系統匣圖示，然後從操作功能表開啟 [儀表板]。

##### <a name="to-access-the-dashboard-by-using-the-launchpad"></a>使用啟動列來存取儀表板

1.  從連線到網路的電腦，開啟 [啟動列]。

2.  在 [啟動列] 功能表上，按一下 [儀表板]。

3.  在 [儀表板] 的 [登入] 頁面上，輸入您的網路系統管理員認證，然後按 ENTER 鍵。

     連到 [Windows Server Essentials 儀表板] 的遠端連線隨即開啟。

###  <a name="access-the-dashboard-from-a-remote-computer"></a><a name="BKMK_Remote"></a> 從遠端電腦存取儀表板
 當您從遠端電腦工作時，您可以使用遠端 Web 存取網站來存取 Windows Server Essentials 儀表板。

##### <a name="to-access-the-dashboard-by-using-remote-web-access"></a>使用遠端 Web 存取來存取儀表板

1.  從遠端電腦，開啟網際網路瀏覽器 (例如 Internet Explorer)。

2.  在網際網路瀏覽器網址列中，輸入下列內容：**HTTPs://<domainname \> /remote**，其中 *domainname* 是您組織的外部功能變數名稱 (例如 contoso.com) 。

3.  輸入您的使用者名稱和密碼以登入遠端 Web 存取網站。

4.  在 [遠端 Web 存取 **首頁**] 的 [**電腦**] 區段中;找出您的伺服器，然後按一下 **[連線]**。

     連到 [儀表板] 的遠端連線隨即開啟。

    > [!NOTE]
    >  如果您的伺服器沒有出現在 [首頁] 的 [電腦] 區段中，請按一下 [連線至更多電腦]，在清單中尋找您的伺服器，然後按一下該伺服器來進行連線。
    >
    >  若要授與使用者從遠端 Web 存取存取儀表板的許可權，請開啟該使用者帳戶的 [內容] 頁面，然後選取 [**隨處存取**] 索引標籤上的 [**伺服器儀表板**] 選項。

##  <a name="use-the-safe-mode"></a><a name="BKMK_UseSafeMode"></a> 使用安全模式
 Windows Server Essentials 的「安全模式」功能會監視伺服器上安裝的增益集。 如果 [儀表板] 變得不穩定或沒有回應，「安全模式」會偵測並隔離可能會造成這個問題的增益集，並在下次您開啟 [儀表板] 時顯示在 [安全模式設定] 頁面上。 從 [安全模式設定] 頁面上，您可以將增益集一一停用後再啟用來判斷造成問題的增益集。 然後您可以選擇將增益集保持停用以供日後啟動，或是將增益集解除安裝。

 您可以隨時檢視安裝在伺服器上的所有增益集清單。 您也可以在安全模式中開啟儀表板，它會自動停用所有非 Microsoft 增益集。Windows Server Essentials 也可讓您從網路上的另一部電腦，以安全模式存取儀表板。

#### <a name="to-view-a-list-of-installed-add-ins"></a>檢視已安裝的增益集清單

-   從 [儀表板] 按一下 [說明]，然後按一下 [安全模式設定]。

#### <a name="to-open-the-dashboard-in-safe-mode"></a>以安全模式開啟儀表板

-   在 [搜尋] 窗格中，輸入 **safe**，然後按一下搜尋結果中的 [儀表板 (安全模式)]。

#### <a name="to-open-the-dashboard-in-safe-mode-from-another-computer-on-the-network"></a>從網路上的另一部電腦以安全模式開啟儀表板

1.  從連線到網路的電腦，開啟 [Windows Server Essentials 啟動列]，然後按一下 [儀表板]。

2.  在 [儀表板] 登入頁面上，輸入具有伺服器登入權限之帳戶的使用者名稱和密碼，選取 [允許我選取要載入的增益集] 核取方塊，然後按一下箭號來進行登入。

## <a name="additional-references"></a>其他參考資料

-   [管理應用程式](Manage-Applications-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
