---
title: 步驟 2 - 設定 WSUS
description: Windows Server Update Service (WSUS) 主題 - 設定 WSUS 是部署 WSUS 四步驟程序當中的第二個步驟
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 9/18/2020
ms.openlocfilehash: d791151493e98c833a3981d305b81fe0b536b19c
ms.sourcegitcommit: b5c7eaa78e16f3b395611cdbdd4c88dfc653832b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2020
ms.locfileid: "91406281"
---
# <a name="step-2-configure-wsus"></a>步驟 2：設定 WSUS

>適用於：Windows Server 2019、Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在您的伺服器上安裝 WSUS 伺服器角色之後，需要正確地設定它。 下列檢查清單摘要為 WSUS 伺服器執行初始設定所需的步驟。

|工作|說明|
|----|--------|
|[2.1.設定網路連線](#21-configure-network-connections)|使用 [網路設定精靈] 設定叢集網路。|
|[2.2.使用 WSUS 設定精靈設定 WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|使用 WSUS 設定精靈執行基本 WSUS 設定。|
|[2.3.設定 WSUS 電腦群組](#23-configure-wsus-computer-groups)|在 WSUS 系統管理主控台中建立電腦群組，以管理組織中的更新。|
|[2.4.設定用戶端更新](#24-configure-client-updates)|指定將自動更新套用到用戶端電腦的方式和時機。|
|[2.5.使用安全通訊端層通訊協定來確保 WSUS 的安全](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|設定安全通訊端層 (SSL) 通訊協定，以協助保護 Windows Server Update Services (WSUS)。|

## <a name="21-configure-network-connections"></a>2.1. 設定網路連線
開始設定程序之前，務必確定您知道下列問題的答案：

1.  伺服器防火牆是否設定成允許用戶端存取伺服器？

2.  這部電腦可以連線到上游伺服器 (例如指定從 Microsoft Update 下載更新的伺服器) 嗎？

3.  如果需要的話，您有 Proxy 伺服器的名稱和使用者認證嗎？

根據預設，WSUS 會設定使用 Microsoft Update 做為取得更新的位置。 如果您的網路上有 Proxy 伺服器，可以設定 WSUS 來使用 Proxy 伺服器。 如果 WSUS 與網際網路之間設有公司防火牆，可能需要設定防火牆以確保 WSUS 可以取得更新。

> [!TIP]
> 雖然需要有網際網路連線才能從 Microsoft Update 下載更新，但是 WSUS 能夠讓您將更新匯入到沒有網際網路連線的網路。

如果您有上述問題的答案，就可以開始設定下列 WSUS 網路設定：

-   **更新** 指定這部伺服器取得更新的方式 (從 Microsoft Update 或其他 WSUS 伺服器)。

-   **Proxy**如果您發現 WSUS 需要使用 Proxy 伺服器才能存取網際網路，則必須在 WSUS 伺服器設定 Proxy 設定。

-   **防火牆** 如果您發現 WSUS 位於公司防火牆的後面，則必須在邊緣裝置執行額外的步驟，才能正確地允許 WSUS 流量。

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. WSUS 伺服器與網際網路之間的連線
如果 WSUS 與網際網路之間設有公司防火牆，可能需要設定防火牆以確保 WSUS 可以取得更新。 為了從 Microsoft Update 取得更新，WSUS 伺服器會為 HTTPS 通訊協定使用連接埠 443。 儘管大部分公司防火牆允許這類流量，但是有些公司因為安全性原則的緣故，限制伺服器的網際網路存取。 如果您的公司限制網際網路存取，則必須取得授權，才能允許 WSUS 到下列 URL 的網際網路存取：

- http\://windowsupdate.microsoft.com

- http\://\*.windowsupdate.microsoft.com

- https\://\*.windowsupdate.microsoft.com

- http\://\*.update.microsoft.com

- https\://\*.update.microsoft.com

- http\://\*.windowsupdate.com

- http\://download.windowsupdate.com

- https\://download.microsoft.com

- http\://\*.download.windowsupdate.com

- http\://wustat.windows.com

- http\://ntservicepack.microsoft.com

- http\://go.microsoft.com

- http\://dl.delivery.mp.microsoft.com

- https\://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> 如需 WSUS 因為防火牆設定而無法取得更新的案例，請參閱 Microsoft 知識庫[文章 885819](https://support.microsoft.com/kb/885819)。

下一節說明如何設定位於 WSUS 與網際網路之間的公司防火牆。 因為 WSUS 會初始化所有網路流量，所以您不需要在 WSUS 伺服器上設定 Windows 防火牆。 雖然 Microsoft Update 與 WSUS 之間的連線需要開啟連接埠 80 和 443，但是您仍然可以設定多個 WSUS 伺服器與自訂連接埠進行同步處理。

### <a name="212-connection-between-wsus-servers"></a>2.1.2. WSUS 伺服器之間的連線
WSUS 上游和下游伺服器會在由 WSUS 系統管理員設定的連接埠上進行同步處理。 根據預設，這些通訊埠的設定如下：

-   在 WSUS 3.2 和更早版本上，為 HTTP 設定使用連接埠 80，為 HTTPS 設定使用連接埠 443。

-   在 WSUS 6.2 和更新版本 (至少 Windows Server 2012) 上，為 HTTP 設定使用連接埠 8530，為 HTTPS 設定使用連接埠 8531。

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

3.  以系統管理員身分開啟命令提示字元 (Cmd.exe)。 若要以系統管理員身分開啟命令提示字元，請移至 [開始]  。 在 [開始搜尋]  中，輸入 **Command Prompt**。 在 [開始] 功能表的頂端，以滑鼠右鍵按一下 [命令提示字元]  ，然後按一下 [以系統管理員身分執行]  。 如果出現 [使用者帳戶控制]  對話方塊，請輸入適當的認證 (如有要求)，確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]  。

4.  在 [命令提示字元] 視窗中，移至 C:\Program Files\Update Services\Tools 資料夾。 輸入下列命令：

    **wsusutil ConfigureSSlproxy [< proxy_server proxy_port>] -enable**，其中：

    1.  proxy_server 是指支援 HTTPS 的 Proxy 伺服器名稱。

    2.  proxy_port 是指 Proxy 伺服器連接埠號碼。

5.  關閉命令提示字元視窗。

若要將使用 HTTP 通訊協定的 Proxy 伺服器新增至 WSUS 設定，請使用下列程序：

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>新增使用 HTTP 通訊協定的 Proxy 伺服器

1.  開啟 WSUS 系統管理主控台。

2.  在左窗格中，展開伺服器名稱，然後按一下 [選項]  。

3.  在 [選項]  窗格中，按一下 [更新來源和更新伺服器]  ，然後按一下 [Proxy 伺服器]  索引標籤。

4.  使用下列選項來修改現有的 Proxy 伺服器設定：

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>在 WSUS 設定中變更或新增 Proxy 伺服器

    1.  選取 [同步處理時使用 Proxy 伺服器]  核取方塊。

    2.  在 [Proxy 伺服器名稱]  文字方塊中，輸入 Proxy 伺服器的名稱。

    3.  在 [Proxy 連接埠號碼]  文字方塊中，輸入 Proxy 伺服器的連接埠號碼。 預設連接埠號碼為 80。

    4.  如果 Proxy 伺服器要求您使用特定的使用者帳戶，請選取 [使用使用者認證連線到 Proxy 伺服器]  核取方塊。 在對應的文字方塊中輸入必要的使用者名稱、網域及密碼。

    5.  如果 Proxy 伺服器支援基本驗證，請選取 [允許基本驗證 (密碼會以純文字傳送)]  核取方塊。

    6.  按一下 [確定]  。

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>將 Proxy 伺服器從 WSUS 設定中移除

    1.  若要將 Proxy 伺服器從 WSUS 設定中移除，請清除 [同步處理時使用 Proxy 伺服器]  核取方塊。

    2.  按一下 [確定]  。

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. 使用 WSUS 設定精靈設定 WSUS
這個程序假設您使用 WSUS 設定精靈，該設定精靈會在第一次啟動 WSUS 管理主控台時出現。 這個主題稍後將會說明如何使用 [選項]  頁面進行這些設定：

#### <a name="to-configure-wsus"></a>設定 WSUS

1.  在 [伺服器管理員] 瀏覽窗格中，依序按一下 [儀表板]  、[工具]  、[Windows Server Update Services]  。

    > [!NOTE]
    > 如果出現 [完整 WSUS 安裝]  對話方塊，按一下 [執行]  。 當安裝成功完成時，在 [完整 WSUS 安裝]  對話方塊中按一下 [關閉]  。

2.  [Windows Server Update Services 精靈] 隨即出現。 檢閱 [在您開始前]  頁面上的資訊，然後按 [下一步]  。

3.  閱讀 [參加 Microsoft Update 改進方案]  頁面中的內容，然後評估您是否願意參加。 如果願意參加這個方案。 保留預設的選項或清除核取方塊，然後按 [下一步]  。

4.  在 [選擇上游伺服器]  頁面上會有兩個選項：

    1.  使用 Microsoft Update 同步處理更新

    2.  從另一部 Windows Server Update Services 伺服器進行同步處理

        -   如果選擇從其他 WSUS 伺服器進行同步處理，指定伺服器名稱，以及這部伺服器與上游伺服器通訊所使用的連接埠。

        -   若要使用 SSL，請選取 [同步處理更新資訊時使用 SSL]  核取方塊。 伺服器將會使用連接埠 443 進行同步處理 (確定這部伺服器與上游伺服器支援 SSL)。

        -   如果這是複本伺服器，選取 [這是上游伺服器的複本]  核取方塊。

5.  針對您的部署選取適當的選項後，按 [下一步]  繼續。

6.  在 [指定 Proxy 伺服器]  頁面，選取 [同步處理時使用 Proxy 伺服器]  核取方塊，然後在對應的方塊中輸入 Proxy 伺服器名稱和連接埠號碼 (預設是連接埠 80)。

    > [!IMPORTANT]
    > 如果您發現 WSUS 需要有 Proxy 伺服器才能存取網際網路，則必須完成此步驟。

7.  如果您想要使用特定使用者認證連線到 Proxy 伺服器，選取 [使用使用者認證連線到 Proxy 伺服器]  核取方塊，然後在對應的方塊中輸入使用者名稱、網域和使用者密碼。 如果您要為連線到 Proxy 伺服器的使用者啟用基本驗證，選取 [允許基本驗證 (密碼會以純文字傳送)]  核取方塊。

8.  按 [下一步]  。 在 [連線到上游伺服器]  頁面，按一下 [開始連線]  。

9. 連線之後，按 [下一步]  繼續。

10. 在 [選擇語言]  頁面上，您可以選擇 WSUS 接收的更新語言，也就是所有語言或語言子集。 選取語言子集可以節省磁碟空間，但是針對這個 WSUS 伺服器的所有用戶端選擇需要的所有語言是非常重要的。 如果只選擇取得特定語言的更新，請選取 [只下載下列語言的更新]  ，然後選擇想要更新的語言；否則保留預設選項。

    > [!WARNING]
    > 如果選取 [只下載下列語言的更新]  選項，而且這部伺服器有連線的下游 WSUS 伺服器，這個選項將會強制下游伺服器只使用選取的語言。

11. 為您的部署選取適當的語言選項後，按 [下一步]  繼續。

12. [選擇產品]  頁面可以讓您指定想要更新的產品。 選取產品類別 (如 Windows) 或選取特定產品 (如 Windows Server 2008)。 選取產品類別將會選取該類別中的所有產品。

13. 為您的部署選取適當的產品選項，然後按 [下一步]  。

14. 在 [選擇分類]  頁面上選取想要取得的更新分類。 選擇所有的分類或子分類，然後按 [下一步]  。

15. [設定同步處理排程]  頁面可讓您選擇要手動或自動執行同步處理。

    -   如果選擇 [手動同步處理]  ，必須從 WSUS 管理主控台啟動同步處理程序。

    -   如果選擇 [自動同步處理]  ，WSUS 伺服器會依設定的間隔進行同步處理。

    設定 [第一次同步處理]  的時間，然後指定您要這部伺服器執行的 [每天同步處理]  次數。 例如，如果指定每天同步處理 4 次，從上午 3:00 開始，那麼同步處理的時間會在上午 3:00、上午 9:00、下午 3:00 以及下午 9:00。

16. 為您的部署選取適當的同步處理選項後，按 [下一步]  繼續。

17. 在 [完成]  頁面上，您可以選取 [開始初始同步處理]  核取方塊，立即開始同步處理。 如果沒有選取這個選項，則需要使用 WSUS 管理主控台執行初始同步處理。 如果想要讀取更多其他設定，按 [下一步]  ；或者，按一下 [完成]  結束這個精靈並完成初始 WSUS 設定。

18. 按一下 [完成]  之後，WSUS 管理主控台隨即出現。

現在您已經執行完基本的 WSUS 設定，請繼續閱讀下面幾個小節，了解使用 WSUS 管理主控台變更設定的詳細資料。

## <a name="23-configure-wsus-computer-groups"></a>2.3. 設定 WSUS 電腦群組
電腦群組是 Windows Server Update Services (WSUS) 部署很重要的一個部分。 電腦群組允許您測試並將更新對應到特定的電腦。 預設電腦群組有兩個：[所有電腦] 和 [尚未指派的電腦]。 根據預設，當每個用戶端電腦第一次連絡 WSUS 伺服器的時候，伺服器會將這個用戶端電腦新增到這兩個群組中。

您可以依需要的數量建立許多自訂電腦群組，以管理組織中的更新。 最佳做法是至少建立一個電腦群組來測試更新，然後再將這些更新部署到組織的其他電腦。

使用下列程序來建立新的群組，並將電腦指派到這個群組：

#### <a name="to-create-a-computer-group"></a>建立電腦群組

1.  在 WSUS 管理主控台中，展開 [更新服務]  下的 WSUS 伺服器，然後展開 [電腦]  ，以滑鼠右鍵按一下 [所有電腦]  ，然後按一下 [新增電腦群組]  。

2.  在 [新增電腦群組]  對話方塊的 [名稱]  中指定新群組的名稱，然後按一下 [新增]  。

3.  按一下 [電腦]  ，然後選取想要指派到這個新群組的電腦。

4.  在上一個步驟選取的電腦名稱上按一下滑鼠右鍵，然後按一下 [變更成員資格]  。

5.  在 [設定電腦群組成員資格]  對話方塊中，選取您建立的測試群組，然後按一下 [確定]  。

## <a name="24-configure-client-updates"></a>2.4. 設定用戶端更新
WSUS 安裝程式會自動設定 IIS，將最新版本的自動更新散佈到與 WSUS 伺服器連絡的每個用戶端電腦上。 設定自動更新最佳的方式取決於網路環境。

-   在使用 Active Directory 目錄服務的環境中，您可以使用現有的網域群組原則物件 (GPO) 或建立新的 GPO。

-   在沒有 Active Directory 的環境中，可以使用本機群組原則編輯器來設定自動更新，然後將用戶端電腦指向 WSUS 伺服器。

> [!IMPORTANT]
> 下列程序假設您的網路執行 Active Directory。 這些程序也假設您熟悉群組原則並使用它來管理網路。

使用下列程序為用戶端電腦設定自動更新：

-   [步驟 4：設定自動更新的群組原則設定](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3.設定電腦群組](#23-configure-wsus-computer-groups)，在本主題中

### <a name="configure-automatic-updates-in-group-policy"></a>在群組原則中設定自動更新

如果您已經在網路中設定 Active Directory，可以在群組原則物件 (GPO) 中包含一或多部電腦，然後使用 WSUS 設定來設定這個 GPO，這樣就可以同時設定這些電腦。 建議您建立只包含 WSUS 設定的新 GPO。

將這個 WSUS GPO 連結到適合您環境的 Active Directory 容器。 在簡單的環境中，您可以將單一 WSUS GPO 連結到網域。 在較複雜的環境中，您可以將多個 WSUS GPO 連結到數個組織單元 (OU)，這樣可以將不同的 WSUS 原則設定套用到不同類型的電腦。

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>透過網域 GPO 啟用 WSUS

1.  在群組原則管理主控台 (GPMC) 中，瀏覽到想要設定 WSUS 的 GPO，然後按一下 [編輯]  。

2.  在 GPMC 中依序展開 [電腦設定]  、[原則]  、[系統管理範本]  及 [Windows 元件]  ，然後按一下 [Windows Update]  。

3.  在詳細資料窗格中，按兩下 [設定自動更新]  。 [設定自動更新]  原則隨即開啟。

4.  按一下 [啟用]  ，然後在 [設定自動更新]  設定下，選取下列其中一個選項：

    -   [通知我下載和通知我安裝]  。 這個選項會在您下載和安裝更新之前通知登入的系統管理使用者。

    -   [自動下載和通知我安裝]  。 這個選項會自動開始下載更新，然後在安裝更新之前先通知登入的系統管理使用者。 預設會選取這個選項。

    -   [自動下載和排程安裝]  。 這個選項會自動開始下載更新，然後在您指定的日期和時間安裝更新。

    -   [允許本機系統管理員選擇設定]  。 這個選項讓本機系統管理員使用 [控制台] 中的 [自動更新] 來選取設定選項。 例如，他們可以選擇排定的安裝時間。 本機系統管理員無法停用自動更新。

5.  依序選取 [啟用以用戶端為目標]  、[啟用]  ，然後在 [此電腦的目標群組名稱]  方塊中，輸入您要新增這部電腦的 WSUS 電腦群組名稱。

    > [!NOTE]
    > [啟用以用戶端為目標]  可讓用戶端電腦在將自動更新重新導向至 WSUS 伺服器時，可以將本身新增至 WSUS 伺服器的目標電腦群組。 如果狀態設定為 [啟用]，則這部電腦會在傳送資訊至 WSUS 伺服器時，將本身視為特定電腦群組的成員，WSUS 伺服器會用它來判斷這部電腦已部署哪些更新。 這個設定會向 WSUS 伺服器指示用戶端電腦即將使用的群組。 您必須在 WSUS 伺服器上建立該群組，並將網域成員的電腦新增至該群組。

6.  按一下 [確定]  以關閉 [啟用以用戶端為目標]  原則，並返回 Windows Update 的詳細資料窗格。

7.  按一下 [確定]  以關閉 [設定自動更新]  原則，並返回 Windows Update 的詳細資料窗格。

8.  在 [Windows Update]  詳細資料窗格中，按兩下 [指定內部網路 Microsoft 更新服務的位置]  。

9. 按一下 [啟用]  ，然後在 [設定偵測更新的近端內部網路更新服務]  和 [設定近端內部網路統計伺服器]  文字方塊中，輸入 WSUS 伺服器的相同 URL。 例如，在這兩個方塊中輸入 http://servername  (其中 servername  是指 WSUS 伺服器的名稱)。

    > [!WARNING]
    > 輸入您 WSUS 伺服器的內部網路位址時，請務必指定要使用的連接埠。 根據預設，WSUS 會為 HTTP 使用連接埠 8530，為 HTTPS 使用連接埠 8531。 例如，如果您使用 HTTP，應該輸入 **http://servername:8530** 。

10. 按一下 [確定]  。

設定好用戶端電腦之後，必須等待幾分鐘的時間，WSUS 管理主控台的 [電腦]  頁面才會出現該電腦。 對於使用網域群組原則物件設定的用戶端電腦，群組原則大約需要 20 分鐘的時間才能將新的原則設定套用到用戶端電腦。 根據預設，每 90 分鐘會在背景更新群組原則，隨機差距為 0 到 30 分鐘。 如果您希望縮短更新群組原則的時間，可以在用戶端電腦上開啟 [命令提示字元] 視窗並輸入 gpupdate /force。

對於使用本機群組原則編輯器設定的用戶端電腦，會立即套用 GPO，而更新則需要 20 分鐘。 如果您開始手動偵測，用戶端電腦不需要花 20 分鐘來連絡 WSUS。

因為等候偵測開始是相當費時的程序，您可以使用下列程序立即初始化偵測。

##### <a name="to-initiate-wsus-detection"></a>初始化 WSUS 偵測

1.  在用戶端電腦上，以提升的權限開啟 [命令提示字元] 視窗。

2.  輸入 wuauclt.exe /detectnow，然後按 ENTER 鍵。

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. 使用安全通訊端層通訊協定來確保 WSUS 的安全

您可以使用安全通訊端層 (SSL) 通訊協定來協助確保 WSUS 部署的安全。 WSUS 會使用 SSL 驗證對 WSUS 伺服器通訊的用戶端電腦與下游 WSUS 伺服器。 WSUS 還會使用 SSL 將更新中繼資料加密。

> [!IMPORTANT]
> 設定使用傳輸層安全性 (TLS) 或 HTTPS 的用戶端和下游伺服器也必須設定使用完整網域名稱 (FQDN)，以供其上游 WSUS 伺服器使用。

WSUS 只會針對中繼資料 (不會針對更新檔案) 使用 SSL。 這是 Microsoft Update 散佈更新的相同方式。 Microsoft 會透過簽署每個更新的方式，來降低透過未加密通道傳送更新檔案的風險。 此外，還會計算雜湊，並在每個更新中將它與中繼資料一起傳送。 下載更新之後，WSUS 會檢查數位簽章和雜湊。 如果更新已變更，便不會進行安裝。

### <a name="limitations-of-wsus-ssl-deployments"></a>WSUS SSL 部署的限制
使用 SSL 來確保 WSUS 部署的安全時，您必須考量下列限制：

1.  使用 SSL 會增加伺服器工作負載。 因為加密所有透過網路傳送的中繼資料的代價，您應該預期百分之 10 的損失效能。

2.  如果使用 WSUS 搭配遠端 SQL Server 資料庫，則 WSUS 伺服器與資料庫伺服器之間的連線不會受到 SSL 的保護。 如果資料庫連線必須要受到保護，請考慮下列建議：

-   將 WSUS 資料庫移到 WSUS 伺服器。

-   將遠端資料庫伺服器與 WSUS 伺服器移到私人網路。

-   部署網際網路通訊協定安全性 (IPsec) 以協助確保網路流量的安全。 如需有關 IPsec 的詳細資訊，請參閱 [建立和使用 IPsec 原則](https://go.microsoft.com/fwlink/?LinkID=203841)。

### <a name="configure-ssl-on-the-wsus-server"></a>在 WSUS 伺服器上設定 SSL
WSUS 需要兩個連接埠供 SSL 使用：一個連接埠會使用 HTTPS 來傳送加密的中繼資料，另一個連接埠會使用 HTTP 來傳送更新。 當您設定 WSUS 使用 SSL 時，請考量下列事項：

-   您無法設定整個 WSUS 網站要求使用 SSL，因為流向 WSUS 網站的所有流量都必須進行加密。 WSUS 僅會加密更新中繼資料。 如果電腦嘗試擷取 HTTPS 連接埠上的更新檔案，則傳輸將會失敗。

    您應該僅在下列虛擬根目錄中要求使用 SSL：

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    您不應該在下列虛擬根目錄中要求使用 SSL：

    -   **內容**

    -   **清查**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   憑證授權單位 (CA) 的憑證必須匯入本機電腦受信任的根 CA 存放區，或下游 WSUS 伺服器上的 Windows Server Update Service 受信任的根 CA 存放區。 如果憑證只匯入本機使用者受信任的根 CA 存放區，則無法在上游伺服器上驗證下游 WSUS 伺服器。

    如需有關如何在 IIS 中使用 SSL 憑證的詳細資訊，請參閱[需要安全通訊端層 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846)。

-   您必須將憑證匯入所有將與 WSUS 伺服器通訊的電腦。 這包括所有用戶端電腦、下游伺服器和執行 WSUS 系統管理主控台的電腦。 憑證應該匯入本機電腦受信任的根 CA 存放區，或 Windows Server Update Service 受信任的根 CA 存放區。

-   您可以針對 SSL 使用任何通訊埠。 不過，針對 SSL 設定的連接埠也可以決定 WSUS 用來傳送未加密 HTTP 流量的連接埠。 請考慮以下範例：

    -   如果您針對 HTTPS 流量使用業界標準通訊埠 443，則 WSUS 便會針對未加密 HTTP 流量使用業界標準通訊埠 80。

    -   如果您針對 HTTPS 流量使用 443 以外的任何通訊埠，則 WSUS 會透過供 HTTPS 使用之連接埠的前一個數值連接埠來傳送未加密 HTTP 流量。 例如，如果針對 HTTPS 使用連接埠 8531，則 WSUS 會針對 HTTP 使用連接埠 8530。

-   如果變更了伺服器名稱、SSL 設定或連接埠號碼，則您必須重新初始化 *ClientServicingProxy* 。

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>在 WSUS 根伺服器上設定 SSL

1.  使用 WSUS Administrators 群組或本機 Administrators 群組成員的帳戶登入 WSUS 伺服器。

2.  移至 [開始]  並輸入 **CMD**，於 [命令提示字元]  上按一下滑鼠右鍵，然後按一下 [以系統管理員身分執行]  。

3.  瀏覽至 _%ProgramFiles%_ **\\Update Services\\Tools\\** 資料夾。

4.  在 [命令提示字元] 視窗中輸入下列命令：

    **Wsusutil configuressl**_certificateName_

    其中：

    *certificateName* 是指 WSUS 伺服器的 DNS 名稱。

### <a name="configure-ssl-on-client-computers"></a>在用戶端電腦上設定 SSL
當您在用戶端電腦上設定 SSL 時，您應該考慮下列問題：

-   您必須包括 WSUS 伺服器上安全連接埠的 URL。 因為您無法在伺服器上要求使用 SSL，確定用戶端電腦可以使用安全性通道的唯一方式是使用指定 HTTPS 的 URL。 如果您針對 SSL 使用 443 以外的任何通訊埠，則您必須也在 URL 中包含該連接埠。

-   用戶端電腦上的憑證必須匯入本機電腦受信任的根 CA 存放區，或自動更新服務受信任的根 CA 存放區。 如果憑證僅匯入本機使用者的受信任的根 CA 存放區，則自動更新的伺服器驗證將會失敗。

-   用戶端電腦必須信任您繫結至 WSUS 伺服器的憑證。 根據使用的憑證類型，您可能必須設定可啟用用戶端電腦的服務，進而信任繫結至 WSUS 伺服器的憑證。

### <a name="configure-ssl-for-downstream-wsus-servers"></a>設定下游 WSUS 伺服器的 SSL
下列指示會設定下游伺服器以與使用 SSL 的上游伺服器同步處理。

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>同步處理下游伺服器與使用 SSL 的上游伺服器

1.  使用本機 Administrators 群組或 WSUS Administrators 群組成員的使用者帳戶登入電腦。

2.  依序按一下 [開始]  、[所有程式]  、[系統管理工具]  和 [Windows Server Update Services]  。

3.  在右窗格中展開伺服器名稱。

4.  按一下 [選項]  ，然後按一下 [更新來源和 Proxy 伺服器]  。

5.  在 [更新來源]  頁面上，選取 [從另一部 Windows Server Update Services 伺服器進行同步處理]  。

6.  在 [伺服器名稱]  文字方塊中，輸入上游伺服器的名稱。 在 [連接埠號碼]  文字方塊中輸入伺服器使用 SSL 連線的連接埠號碼。

7.  選取 [同步處理更新資訊時使用 SSL]  核取方塊，然後按一下 [確定]  。

### <a name="additional-ssl-resources"></a>其他 SSL 資源
設定憑證授權單位所需的步驟、將憑證繫結至 WSUS 網站，以及在用戶端電腦與憑證之間建立信任等已超出本指南的範圍。 如需詳細資訊和如何安裝憑證及設定此環境的相關指示，請參閱下列主題：

-   [Suite B PKI 的逐步指南](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [實作和管理憑證範本](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [Active Directory 憑證服務升級和移轉指南](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [設定憑證自動註冊](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. 完成 IIS 設定
依預設，已針對預設和所有新的 IIS 網站啟用匿名讀取權限。 有些應用程式 (值得注意的是 Windows SharePoint Services) 可能會移除匿名存取。 如果發生這種情況，您必須重新啟用匿名讀取權限才能順利地安裝及操作 WSUS。

若要啟用匿名讀取權限，請依照 IIS 適當版本中的步驟進行：

1.  [啟用匿名驗證 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316)，如 IIS 7 操作指南中所述。

2.  [啟用匿名驗證 (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391)，如 IIS 6.0 操作指南中所述。

### <a name="27-configure-a-signing-certificate"></a>2.7. 設定簽署憑證
WSUS 能夠發佈自訂更新套件，以更新 Microsoft 和非 Microsoft 產品。 WSUS 可以使用 Authenticode 憑證，為您自動簽署這些自訂更新套件。 若要啟用自訂更新簽署，您必須在您的 WSUS 伺服器上安裝套件簽署憑證。 有幾個與自訂更新簽署相關聯的考量。

1.  **憑證發佈**。 私密金鑰必須安裝在 WSUS 伺服器上，且公開金鑰必須明確地安裝在所有準備接收自訂簽署更新的用戶端電腦和伺服器上的受信任的憑證存放區。

2.  **到期**。 當自我簽署憑證到期或接近到期時，WSUS 會在事件記錄檔中記錄事件。

3.  **憑證更新/撤銷**。 如果您之前想要更新或撤銷憑證 (即發現它已過期之後)，WSUS 並不提供執行此動作的功能。 完成此動作會變成很難成功手動或自動進行的手動工作。


