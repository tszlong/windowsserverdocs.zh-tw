---
title: 步驟 2： 設定 WSUS
description: Windows Server Update Service (WSUS) 主題-將 WSUS 設定是在部署 WSUS 的程序有四個步驟中的步驟 2
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e51d5f69ff62c120cabfbe549a84fc9fd26f822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820479"
---
# <a name="step-2-configure-wsus"></a>步驟 2：將 WSUS 設定

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在您的伺服器上安裝 WSUS 伺服器角色之後，需要正確地設定它。 下列檢查清單彙總所需執行的 WSUS 伺服器的初始設定的步驟。

|工作|描述|
|----|--------|
|[2.1.設定網路連線](2-configure-wsus.md#BKM_ConfigurenetworkConnections)|使用 [網路設定精靈] 設定叢集網路。|
|[2.2.使用 WSUS 設定精靈設定 WSUS](2-configure-wsus.md#BKM_ConfigureWSUSusingConfigurationWizard)|使用 WSUS 設定精靈執行基本 WSUS 設定。|
|[2.3.設定 WSUS 電腦群組](2-configure-wsus.md#BKMK_ConfigcomputerGroups)|在 WSUS 系統管理主控台中建立電腦群組，以管理組織中的更新。|
|[2.4.設定用戶端更新](2-configure-wsus.md#BKM_ConfigureClientUpdates)|指定將自動更新套用到用戶端電腦的方式和時機。|
|[2.5.確保 WSUS 的安全通訊端層通訊協定的安全](2-configure-wsus.md#bkmk_2.5.ConfigSSL)|設定安全通訊端層 (SSL) 通訊協定，以協助保護 Windows Server Update Services (WSUS)。|

## <a name="BKM_ConfigurenetworkConnections"></a>2.1. 設定網路連線
開始設定程序之前，務必確定您知道下列問題的答案：

1.  伺服器防火牆是否設定成允許用戶端存取伺服器？

2.  這部電腦可以連線到上游伺服器 (例如指定從 Microsoft Update 下載更新的伺服器) 嗎？

3.  如果需要的話，您有 Proxy 伺服器的名稱和使用者認證嗎？

根據預設，WSUS 會設定使用 Microsoft Update 做為取得更新的位置。 如果您在網路上有 proxy 伺服器，您可以設定 WSUS 設定為使用 proxy 伺服器。 如果 WSUS 與網際網路之間的公司防火牆，您可能必須設定防火牆，以確保 WSUS 可以取得更新。

> [!TIP]
> 雖然需要有網際網路連線才能從 Microsoft Update 下載更新，但是 WSUS 能夠讓您將更新匯入到沒有網際網路連線的網路。

如果您有上述問題的答案，就可以開始設定下列 WSUS 網路設定：

-   **更新** 指定這部伺服器取得更新的方式 (從 Microsoft Update 或其他 WSUS 伺服器)。

-   **Proxy**如果您發現 WSUS 需要使用 proxy 伺服器才能存取網際網路，您必須在 WSUS 伺服器設定 proxy 設定。

-   **防火牆**如果您發現 WSUS 位於公司防火牆後方，有一些額外的步驟，必須在邊緣裝置，才能正確地允許 WSUS 流量。

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. WSUS 伺服器與網際網路之間的連線
如果 WSUS 與網際網路之間的公司防火牆，您可能必須設定防火牆以確保 WSUS 可以取得更新。 為了從 Microsoft Update 取得更新，WSUS 伺服器會為 HTTPS 通訊協定使用連接埠 443。 雖然大部分公司防火牆允許這種類型的流量，但有從公司的安全性原則因為伺服器限制網際網路存取某些公司。 如果您的公司限制存取，您需要取得授權，才能允許來自 WSUS 的網際網路存取下列 Url 的清單：

-   http://windowsupdate.microsoft.com

-   http://*.windowsupdate.microsoft.com

-   https://*.windowsupdate.microsoft.com

-   http://*.update.microsoft.com

-   https://*.update.microsoft.com

-   http://*.windowsupdate.com

-   http://download.windowsupdate.com

-   https://download.microsoft.com

-   http://*.download.windowsupdate.com

-   http://wustat.windows.com

-   http://ntservicepack.microsoft.com

-   http://go.microsoft.com

-   http://dl.delivery.mp.microsoft.com

-   https://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> WSUS 無法取得更新，因為防火牆設定的案例，請參閱[文章 885819](https://support.microsoft.com/kb/885819) Microsoft 知識庫中。

下一節說明如何設定位於 WSUS 與網際網路之間的公司防火牆。 WSUS 會初始化所有網路流量，因為它不需要將 WSUS 伺服器上設定 Windows 防火牆。 雖然 Microsoft Update 與 WSUS 之間的連線需要開啟連接埠 80 和 443，但是您仍然可以設定多個 WSUS 伺服器與自訂連接埠進行同步處理。

### <a name="212-connection-between-wsus-servers"></a>2.1.2. WSUS 伺服器之間的連線
WSUS 上游和下游伺服器會在由 WSUS 系統管理員設定的連接埠上進行同步處理。 根據預設，這些通訊埠的設定如下：

-   在 WSUS 3.2 和更早版本上，為 HTTP 設定使用連接埠 80，為 HTTPS 設定使用連接埠 443。

-   WSUS 6.2 和更新版本 (至少 Windows Server 2012)，HTTP 和 8531 連接埠 8530，為使用 HTTPS

WSUS 伺服器上的防火牆必須設定為允許這些連接埠上的輸入流量。

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. 用戶端 (Windows Update 代理程式) 與 WSUS 伺服器之間的連線
在 WSUS 的 IIS 站台和在用來設定用戶端電腦的任何群組原則設定中設定接聽介面和連接埠。 預設連接埠會與上一節 **WSUS 伺服器之間的連線**中所指定的連接埠相同，而且 WSUS 伺服器上的防火牆也必須設定為允許這些連接埠上的輸入流量。

## <a name="configure-the-proxy-server"></a>設定 Proxy 伺服器
如果公司網路使用 Proxy 伺服器，則 Proxy 伺服器必須支援 HTTP 和 SSL 通訊協定，並使用基本驗證或 Windows 驗證。 使用下列其中一項設定來滿足這些需求：

1.  支援兩種通訊協定通道的單一 Proxy 伺服器。 在這個情況下，請設定一個通道使用 HTTP，並設定另一個通道使用 HTTPS。

    > [!NOTE]
    > 您可以設定一個 Proxy 伺服器，在 WSUS 伺服器軟體安裝期間處理 WSUS 的兩個通訊協定。

2.  兩個 Proxy 伺服器，每個伺服器支援單一通訊協定。 在這種情況下，一個 Proxy 伺服器會設定為使用 HTTP，另一個 Proxy 伺服器會設定為使用 HTTPS。

若要設定兩個 Proxy 伺服器，每個伺服器處理 WSUS 的一種通訊協定，請使用下列程序：

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>設定 WSUS 使用兩個 Proxy 伺服器

1.  使用本機 Administrators 群組成員的帳戶登入即將成為 WSUS 伺服器的電腦。

2.  安裝 WSUS 伺服器角色。 在 WSUS 設定精靈 (會在下一節中進行討論) 期間，請勿指定 Proxy 伺服器。

3.  以系統管理員身分開啟命令提示字元 (Cmd.exe)。 若要以系統管理員身分開啟命令提示字元，請移至 [開始] 。 在 **開始搜尋**，型別**命令提示字元**。 在 [開始] 功能表的頂端，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。 如果**使用者帳戶控制**出現對話方塊，請輸入適當的認證 （如有要求），請確認，它會顯示的動作是您想，然後按一下**繼續**。

4.  在 [命令提示字元] 視窗中，移至 C:\Program Files\Update Services\Tools 資料夾。 輸入下列命令：

    **wsusutil ConfigureSSlproxy [< proxy_server proxy_port >]-啟用**，其中：

    1.  proxy_server 是指支援 HTTPS 的 Proxy 伺服器名稱。

    2.  proxy_port 是指 Proxy 伺服器連接埠號碼。

5.  關閉 [命令提示字元] 視窗。

若要將使用 HTTP 通訊協定的 Proxy 伺服器新增至 WSUS 設定，請使用下列程序：

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>新增使用 HTTP 通訊協定的 Proxy 伺服器

1.  開啟 WSUS 系統管理主控台。

2.  在左窗格中，展開伺服器名稱，然後按一下 [選項] 。

3.  在 [選項] 窗格中，按一下 [更新來源和更新伺服器]，然後按一下 [Proxy 伺服器] 索引標籤。

4.  使用下列選項來修改現有的 Proxy 伺服器設定：

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>在 WSUS 設定中變更或新增 Proxy 伺服器

    1.  選取 [同步處理時使用 Proxy 伺服器] 核取方塊。

    2.  在 [Proxy 伺服器名稱] 文字方塊中，輸入 Proxy 伺服器的名稱。

    3.  在 [Proxy 連接埠號碼]  文字方塊中，輸入 Proxy 伺服器的連接埠號碼。 預設連接埠號碼為 80。

    4.  如果 proxy 伺服器需要您使用特定的使用者帳戶，請選取**使用使用者認證來連線到 proxy 伺服器**核取方塊。 對應的文字方塊中輸入必要的使用者名稱、 網域及密碼。

    5.  如果 Proxy 伺服器支援基本驗證，請選取 [允許基本驗證 (密碼會以純文字傳送)] 核取方塊。

    6.  按一下 [確定] 。

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>將 Proxy 伺服器從 WSUS 設定中移除

    1.  若要將 Proxy 伺服器從 WSUS 設定中移除，請清除 [同步處理時使用 Proxy 伺服器] 核取方塊。

    2.  按一下 [確定] 。

## <a name="BKM_ConfigureWSUSusingConfigurationWizard"></a>2.2. 使用 WSUS 設定精靈設定 WSUS
這個程序假設您使用 WSUS 設定精靈，該設定精靈會在第一次啟動 WSUS 管理主控台時出現。 這個主題稍後將會說明如何使用 [選項]  頁面進行這些設定：

#### <a name="to-configure-wsus"></a>設定 WSUS

1.  在 [伺服器管理員] 瀏覽窗格中，依序按一下 [儀表板]、[工具]、[Windows Server Update Services]。

    > [!NOTE]
    > 如果**完成安裝 WSUS**  對話方塊出現時，按一下**執行**。 在**完成安裝 WSUS**  對話方塊中，按一下**關閉**當安裝順利完成。

2.  [Windows Server Update Services 精靈] 隨即出現。 檢閱 [在您開始前]  頁面上的資訊，然後按 [下一步] 。

3.  閱讀 [參加 Microsoft Update 改進方案]  頁面中的內容，然後評估您是否願意參加。 如果願意參加這個方案。 保留預設的選項，或是清除核取方塊，然後再按**下一步**。

4.  在 [選擇上游伺服器]  頁面上會有兩個選項：

    1.  使用 Microsoft Update 同步處理更新

    2.  從另一部 Windows Server Update Services 伺服器進行同步處理

        -   如果您選擇從另一部 WSUS 伺服器同步處理，請與上游伺服器指定伺服器名稱，此伺服器會進行通訊的連接埠。

        -   若要使用 SSL，請選取 [同步處理更新資訊時使用 SSL] 核取方塊。 伺服器將會使用連接埠 443 進行同步處理 (確定這部伺服器與上游伺服器支援 SSL)。

        -   如果這是複本伺服器，請選取**這是上游伺服器的複本**核取方塊。

5.  針對您的部署選取適當的選項後，按 [下一步] 繼續。

6.  在 [指定 Proxy 伺服器] 頁面，選取 [同步處理時使用 Proxy 伺服器] 核取方塊，然後在對應的方塊中輸入 Proxy 伺服器名稱和連接埠號碼 (預設是連接埠 80)。

    > [!IMPORTANT]
    > 如果您發現 WSUS 需要有 Proxy 伺服器才能存取網際網路，則必須完成此步驟。

7.  如果您想要使用特定的使用者認證，選取連線到 proxy 伺服器**使用使用者認證來連線到 proxy 伺服器**核取方塊，，然後輸入在對應的 使用者名稱、 網域和使用者的密碼方塊。 如果您想要為連線到 proxy 伺服器，選取的使用者啟用基本驗證**允許基本驗證 （密碼會以純文字傳送）** 核取方塊。

8.  按一下 [下一步] 。 在 **連線到上游伺服器**頁面上，按一下**開始連接**。

9. 連線之後，按 [下一步]  繼續。

10. 在 **選擇語言**頁面上，您可以選擇 WSUS 接收的更新： 所有語言或語言子集的語言。 選取語言子集可以節省磁碟空間，但選擇的所有語言的此 WSUS 伺服器的所有用戶端所需的重要事項。 如果您選擇只取得特定語言的更新，請選取**僅以這些語言下載更新**，然後選取 語言中，您想要更新; 否則保留預設選項。

    > [!WARNING]
    > 如果您選取的選項**僅以這些語言下載更新**，和此伺服器有下游 WSUS 伺服器連線到它，此選項會強制下游伺服器只使用選取的語言。

11. 為您的部署選取適當的語言選項後，按 [下一步]  繼續。

12. [選擇產品]  頁面可以讓您指定想要更新的產品。 選取產品類別目錄，例如 Windows 或特定的產品，例如 Windows Server 2008。 選取產品類別目錄選取該類別中的所有產品。

13. 選取適當的產品選項，為您的部署，然後按一下**下一步**。

14. 在 [選擇分類]  頁面上選取想要取得的更新分類。 選擇所有的分類或子分類，然後按 [下一步]。

15. [設定同步處理排程] 頁面可讓您選擇要手動或自動執行同步處理。

    -   如果您選擇**手動同步處理**，您必須從 WSUS 管理主控台啟動同步處理程序。

    -   如果您選擇**自動同步處理**，WSUS 伺服器會同步處理設定的間隔。

    設定 [第一次同步處理] 的時間，然後指定您要這部伺服器執行的 [每天同步處理]  次數。 例如，如果指定每天同步處理 4 次，從上午 3:00 開始，那麼同步處理的時間會在上午 3:00、上午 9:00、下午 3:00 以及下午 9:00。

16. 為您的部署選取適當的同步處理選項後，按 [下一步] 繼續。

17. 在 [完成]  頁面上，您可以選取 [開始初始同步處理]  核取方塊，立即開始同步處理。 如果您未選取此選項，您需要使用 WSUS 管理主控台來執行初始同步處理。 如果想要讀取更多其他設定，按 [下一步]；或者，按一下 [完成] 結束這個精靈並完成初始 WSUS 設定。

18. 按一下 [完成] 之後，WSUS 管理主控台隨即出現。

現在您已經執行完基本的 WSUS 設定，請繼續閱讀下面幾個小節，了解使用 WSUS 管理主控台變更設定的詳細資料。

## <a name="BKMK_ConfigcomputerGroups"></a>2.3. 設定 WSUS 電腦群組
電腦群組是 Windows Server Update Services (WSUS) 部署中很重要的一部分。 電腦群組允許您測試並將更新對應到特定的電腦。 預設電腦群組有兩個：所有的電腦與未指派的電腦。 根據預設，當每個用戶端電腦第一次連絡 WSUS 伺服器的時候，伺服器會將這個用戶端電腦新增到這兩個群組中。

您可以依需要的數量建立許多自訂電腦群組，以管理組織中的更新。 最佳做法是至少建立一個電腦群組來測試更新，然後再將這些更新部署到組織的其他電腦。

使用下列程序來建立新的群組，並將電腦指派到這個群組：

#### <a name="to-create-a-computer-group"></a>建立電腦群組

1.  在 WSUS 管理主控台中，在**Update Services**、 展開 WSUS 伺服器，再展開**電腦**，以滑鼠右鍵按一下**所有電腦**，然後按一下  **新增電腦群組**。

2.  在 **新增電腦群組**對話方塊的 **名稱**中指定新群組的名稱，然後按一下**新增**。

3.  按一下 **電腦**，然後選取您想要指派給這個新群組的電腦。

4.  以滑鼠右鍵按一下您在上一個步驟中，選取的電腦名稱，然後按一下**變更成員資格**。

5.  在 **設定電腦群組成員資格**對話方塊中，選取的測試群組，您所建立，然後按一下**確定**。

## <a name="BKM_ConfigureClientUpdates"></a>2.4. 設定用戶端更新
WSUS 安裝程式會自動設定 IIS，將最新版本的自動更新散佈到與 WSUS 伺服器連絡的每個用戶端電腦上。 設定自動更新最佳的方式取決於網路環境。

-   在環境中使用 active directory 目錄服務，您可以使用現有網域型群組原則物件 (GPO)，或建立新的 GPO。

-   在沒有 active directory 的環境，使用本機群組原則編輯器來設定自動更新，然後將用戶端電腦指向 WSUS 伺服器。

> [!IMPORTANT]
> 下列程序假設您的網路執行 active directory。 這些程序也假設您熟悉群組原則並使用它來管理網路。

使用下列程序為用戶端電腦設定自動更新：

-   [步驟 4：設定自動更新的群組原則設定](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3.設定電腦群組](2-configure-wsus.md#BKMK_ConfigcomputerGroups)本主題中

### <a name="BKMK_ConfigureAU"></a>在 群組原則中設定自動更新
如果您已設定 active directory 網路中，您可以在同時包含在群組原則物件 (GPO)，並接著使用 WSUS 設定來設定這個 GPO 中設定一個或多部電腦。 建議您建立只包含 WSUS 設定的新 GPO。

這個 WSUS GPO 連結到適用於您環境的 active directory 容器。 在簡單的環境中，您可以將單一 WSUS GPO 連結到網域。 在較複雜的環境中，您可以將多個 WSUS GPO 連結到數個組織單元 (OU)，這樣可以將不同的 WSUS 原則設定套用到不同類型的電腦。

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>透過網域 GPO 啟用 WSUS

1.  在群組原則管理主控台 (GPMC)，瀏覽至您要設定 WSUS 的 GPO，然後按一下**編輯**。

2.  在 GPMC 中，依序展開**電腦組態**，展開**原則**，展開**系統管理範本**，展開  **Windows 元件**，然後按一下**Windows Update**。

3.  在詳細資料窗格中，按兩下 [設定自動更新] 。 [設定自動更新]  原則隨即開啟。

4.  按一下 [啟用]，然後在 [設定自動更新] 設定下，選取下列其中一個選項：

    -   [通知我下載和通知我安裝]。 這個選項會在您下載和安裝更新之前通知登入的系統管理使用者。

    -   [自動下載和通知我安裝]。 這個選項會自動開始下載更新，然後在安裝更新之前先通知登入的系統管理使用者。 預設會選取這個選項。

    -   [自動下載和排程安裝]。 這個選項會自動開始下載更新，然後在您指定的日期和時間安裝更新。

    -   [允許本機系統管理員選擇設定]。 這個選項讓本機系統管理員使用 [控制台] 中的 [自動更新] 來選取設定選項。 例如，他們可以選擇排定的安裝時間。 本機系統管理員無法停用自動更新。

5.  選取 [**啟用用戶端為目標**，選取**已啟用**，然後輸入您要新增這部電腦中的 WSUS 電腦群組名稱**這台電腦的目標群組名稱** ] 方塊中。

    > [!NOTE]
    > [啟用以用戶端為目標] 可讓用戶端電腦在將自動更新重新導向至 WSUS 伺服器時，可以將本身新增至 WSUS 伺服器的目標電腦群組。 如果狀態設定為 已啟用，此電腦將本身視為特定電腦群組的成員傳送資訊到 WSUS 伺服器，會用它來判斷哪些更新已部署到這台電腦時。 這個設定會向 WSUS 伺服器指示用戶端電腦即將使用的群組。 您必須在 WSUS 伺服器上建立該群組，並將網域成員的電腦新增至該群組。

6.  按一下 [確定] 以關閉 [啟用以用戶端為目標] 原則，並返回 Windows Update 的詳細資料窗格。

7.  按一下 [確定]  以關閉 [設定自動更新]  原則，並返回 Windows Update 的詳細資料窗格。

8.  在 [Windows Update]  詳細資料窗格中，按兩下 [指定內部網路 Microsoft 更新服務的位置] 。

9. 按一下 [啟用]，然後在 [設定偵測更新的近端內部網路更新服務] 和 [設定近端內部網路統計伺服器] 文字方塊中，輸入 WSUS 伺服器的相同 URL。 例如，輸入*http://servername*這兩個方塊中 (其中*servername*是 WSUS 伺服器的名稱)。

    > [!WARNING]
    > 輸入您 WSUS 伺服器的內部網路位址時，請務必指定要使用的連接埠。 根據預設，WSUS 會為 HTTP 使用連接埠 8530，為 HTTPS 使用連接埠 8531。 例如，如果您使用 HTTP，您應該輸入**http://servername:8530**。

10. 按一下 [確定] 。

您設定用戶端電腦之後，將會需要幾分鐘的時間才會出現在電腦**電腦**在 WSUS 管理主控台中的頁面。 對於使用網域群組原則物件設定的用戶端電腦，群組原則大約需要 20 分鐘的時間才能將新的原則設定套用到用戶端電腦。 根據預設，隨機差距為 0 到 30 分鐘每 90 分鐘在背景中的群組原則更新。 如果您希望縮短更新群組原則，您可以開啟命令提示字元 視窗，在用戶端電腦，然後輸入 gpupdate /force。

使用本機群組原則編輯器設定的用戶端電腦，會立即套用 GPO，並更新所需的大約 20 分鐘的時間。 如果您開始手動偵測，您不必靜待 20 分鐘來連絡 WSUS 用戶端電腦。

因為等候偵測開始是相當費時的程序，您可以使用下列程序立即初始化偵測。

##### <a name="to-initiate-wsus-detection"></a>初始化 WSUS 偵測

1.  用戶端電腦上，請使用提高的權限開啟命令提示字元視窗。

2.  輸入 wuauclt.exe /detectnow，然後按 ENTER 鍵。

## <a name="bkmk_2.5.ConfigSSL"></a>2.5. 使用安全通訊端層通訊協定來確保 WSUS 的安全
您可以使用安全通訊端層 (SSL) 通訊協定來協助確保 WSUS 部署的安全。 WSUS 會使用 SSL 驗證對 WSUS 伺服器通訊的用戶端電腦與下游 WSUS 伺服器。 WSUS 還會使用 SSL 將更新中繼資料加密。

> [!IMPORTANT]
> 設定使用傳輸層安全性 (TLS) 或 HTTPS 的用戶端和下游伺服器也必須設定使用完整網域名稱 (FQDN)，以供其上游 WSUS 伺服器使用。

WSUS 只會針對中繼資料 (不會針對更新檔案) 使用 SSL。 這是 Microsoft Update 散佈更新的相同方式。 Microsoft 會透過簽署每個更新的方式，來降低透過未加密通道傳送更新檔案的風險。 此外，還會計算雜湊，並在每個更新中將它與中繼資料一起傳送。 下載更新之後，WSUS 會檢查數位簽章和雜湊。 如果更新已變更，則不會安裝。

### <a name="ssllmits"></a>WSUS SSL 部署的限制
使用 SSL 來確保 WSUS 部署的安全時，您必須考量下列限制：

1.  使用 SSL 會增加伺服器工作負載。 因為加密所有透過網路傳送的中繼資料的代價，您應該預期百分之 10 的損失效能。

2.  如果您使用 WSUS 搭配遠端 SQL Server 資料庫時，WSUS 伺服器與資料庫伺服器之間的連線未受到 SSL。 如果資料庫連線必須受到保護，請考慮下列建議：

-   將 WSUS 資料庫移至 WSUS 伺服器。

-   將遠端資料庫伺服器與 WSUS 伺服器移到私人網路。

-   部署網際網路通訊協定安全性 (IPsec) 以協助確保網路流量的安全。 如需有關 IPsec 的詳細資訊，請參閱 [建立和使用 IPsec 原則](https://go.microsoft.com/fwlink/?LinkID=203841)。

### <a name="consswsus"></a>WSUS 伺服器上設定 SSL
WSUS 需要兩個連接埠供 SSL 使用：一個連接埠會使用 HTTPS 來傳送加密的中繼資料，另一個連接埠會使用 HTTP 來傳送更新。 當您設定 WSUS 使用 SSL 時，請考量下列事項：

-   您無法設定整個 WSUS 網站要求使用 SSL，因為流向 WSUS 網站的所有流量都必須進行加密。 WSUS 僅會加密更新中繼資料。 如果電腦嘗試擷取 HTTPS 連接埠上的更新檔案，則傳輸將會失敗。

    您應該僅在下列虛擬根目錄中要求使用 SSL：

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    您不應該在下列虛擬根目錄中要求使用 SSL：

    -   **Content**

    -   **清查**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   憑證授權單位 (CA) 的憑證必須匯入本機電腦受信任的根 CA 存放區，或下游 WSUS 伺服器上的 Windows Server Update Service 受信任的根 CA 存放區。 如果憑證只匯入到本機使用者信任的根 CA 存放區，就無法在上游伺服器進行驗證下游 WSUS 伺服器。

    如需如何在 IIS 中使用 SSL 憑證的詳細資訊，請參閱[需要安全通訊端層 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846)。

-   您必須將憑證匯入所有將與 WSUS 伺服器通訊的電腦。 這包括所有用戶端電腦、下游伺服器和執行 WSUS 系統管理主控台的電腦。 憑證應該匯入本機電腦受信任的根 CA 存放區，或 Windows Server Update Service 受信任的根 CA 存放區。

-   您可以針對 SSL 使用任何通訊埠。 不過，針對 SSL 設定的連接埠也可以決定 WSUS 用來傳送未加密 HTTP 流量的連接埠。 請考量下列範例：

    -   如果您使用業界標準通訊埠 443 的 HTTPS 流量時，WSUS 會針對未加密 HTTP 流量使用業界標準通訊埠 80。

    -   如果您的 HTTPS 流量使用 443 以外的任何通訊埠，WSUS 會透過加上數字前面的連接埠 https 連接埠來傳送未加密 HTTP 流量。 例如，如果針對 HTTPS 使用連接埠 8531，則 WSUS 會針對 HTTP 使用連接埠 8530。

-   如果變更了伺服器名稱、SSL 設定或連接埠號碼，則您必須重新初始化 *ClientServicingProxy* 。

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>在 WSUS 根伺服器上設定 SSL

1.  使用 WSUS Administrators 群組或本機 Administrators 群組成員的帳戶登入 WSUS 伺服器。

2.  移至**開始**，型別**CMD**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。

3.  瀏覽至 *%ProgramFiles%***\Update Services\Tools\\** 資料夾。

4.  在 [命令提示字元] 視窗中，輸入下列命令：

    **Wsusutil configuressl***certificateName*

    其中：

    *certificateName* 是指 WSUS 伺服器的 DNS 名稱。

### <a name="consslclient"></a>用戶端電腦上設定 SSL
當您在用戶端電腦上設定 SSL 時，您應該考慮下列問題：

-   您必須包括 WSUS 伺服器上安全連接埠的 URL。 因為您無法在伺服器上要求使用 SSL，確定用戶端電腦可以使用安全性通道的唯一方式是使用指定 HTTPS 的 URL。 如果您針對 SSL 使用 443 以外的任何通訊埠，您必須在 URL 中也包含該連接埠。

-   用戶端電腦上的憑證必須匯入到本機電腦受信任的根 CA 存放區或自動更新服務的 受信任的根 CA 存放區。 如果憑證已匯入至本機使用者的受信任的根 CA 存放區時，自動更新將會失敗伺服器驗證。

-   用戶端電腦必須信任您繫結至 WSUS 伺服器的憑證。 根據使用的憑證類型，您可能必須設定可啟用用戶端電腦的服務，進而信任繫結至 WSUS 伺服器的憑證。

### <a name="consslds"></a>設定下游 WSUS 伺服器的 SSL
下列指示會設定下游伺服器以與使用 SSL 的上游伺服器同步處理。

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>同步處理下游伺服器與使用 SSL 的上游伺服器

1.  使用本機 Administrators 群組或 WSUS Administrators 群組成員的使用者帳戶登入電腦。

2.  按一下 **開始**，按一下**所有程式**，按一下 **系統管理工具**，然後按一下**Windows Server Update Service**。

3.  在右窗格中展開伺服器名稱。

4.  按一下 [選項] ，然後按一下 [更新來源和 Proxy 伺服器] 。

5.  在 [更新來源]  頁面上，選取 [從另一部 Windows Server Update Services 伺服器進行同步處理] 。

6.  輸入上游伺服器的名稱**伺服器名稱**文字方塊。 輸入伺服器使用 SSL 連線的連接埠號碼**連接埠號碼**文字方塊。

7.  選取 **同步處理更新資訊時使用 SSL**核取方塊，然後按一下**確定**。

### <a name="addlsl"></a>其他 SSL 資源
設定憑證授權單位所需的步驟、將憑證繫結至 WSUS 網站，以及在用戶端電腦與憑證之間建立信任等已超出本指南的範圍。 如需詳細資訊和如何安裝憑證及設定此環境的相關指示，請參閱下列主題：

-   [Suite B PKI 的逐步指南](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [實作和管理憑證範本](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [active directory 憑證服務升級和移轉指南](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [設定憑證自動註冊](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. 完成 IIS 設定
依預設，已針對預設和所有新的 IIS 網站啟用匿名讀取權限。 有些應用程式 (值得注意的是 Windows SharePoint Services) 可能會移除匿名存取。 如果發生這種情況，您必須重新啟用匿名讀取權限，才能成功安裝及操作 WSUS。

若要啟用匿名讀取權限，請依照 IIS 適當版本中的步驟進行：

1.  [啟用匿名驗證 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316)，如 IIS 7 操作指南中所述。

2.  [啟用匿名驗證 (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391)，如 IIS 6.0 操作指南中所述。

### <a name="27-configure-a-signing-certificate"></a>2.7. 設定簽署憑證
WSUS 能夠發佈自訂更新套件，以更新 Microsoft 和非 Microsoft 產品。 WSUS 可以使用 Authenticode 憑證，為您自動簽署這些自訂更新套件。 若要啟用自訂更新簽署，您必須在您的 WSUS 伺服器上安裝套件簽署憑證。 有幾個與自訂更新簽署相關聯的考量。

1.  **憑證發佈**。 私密金鑰必須安裝在 WSUS 伺服器上，且公開金鑰必須明確地安裝在所有準備接收自訂簽署更新的用戶端電腦和伺服器上的受信任的憑證存放區。

2.  **到期**。 當自我簽署憑證到期或接近到期時，WSUS 會在事件記錄檔中記錄事件。

3.  **憑證更新/撤銷**。 如果您想要更新或撤銷的憑證 （也就是在探索之後它已過期） 時，WSUS 會提供任何功能，若要這麼做。 完成此動作會變成很難成功手動或自動進行的手動工作。


