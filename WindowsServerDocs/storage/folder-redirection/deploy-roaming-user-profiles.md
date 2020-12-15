---
title: 部署漫遊使用者設定檔
description: 深入了解：部署漫遊使用者設定檔
TOCTitle: Deploying Roaming User Profiles
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 1ed7e52b5408e654d007e1f5c02cb9e61ad5c8bb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049186"
---
# <a name="deploying-roaming-user-profiles"></a>部署漫遊使用者設定檔

>適用於：Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server (半年通道)、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2

本主題說明如何使用 Windows Server 將[漫遊使用者設定檔](folder-redirection-rup-overview.md)部署到 Windows 用戶端電腦。 漫遊使用者設定檔會將使用者設定檔重新導向到檔案共用，讓使用者可在多部電腦上接收相同的作業系統及應用程式設定。

如需本主題的近期變更清單，請參閱本主題的[變更歷程記錄](#change-history)一節。

> [!IMPORTANT]
> 由於 [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)中的安全性變更，我們更新了 [步驟4：可選擇性在本主題中建立適用於漫遊使用者設定檔](#step-4-optionally-create-a-gpo-for-roaming-user-profiles)的 GPO，讓 Windows 可以適當地套用漫遊使用者設定檔原則 (而不是還原為受影響電腦上的本機原則)。

> [!IMPORTANT]
>  在下列組態中進行作業系統就地升級後，將會遺失使用者自訂的開始功能表：
> - 已設定漫遊設定檔的使用者
> - 允許使用者變更開始功能表
>
> 因此，在作業系統就地升級後，系統會將開始功能表會重設為預設的新作業系統版本。 如需因應措施，請參閱[附錄 C：升級後重設開始功能表配置的因應措施](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)。

## <a name="prerequisites"></a>必要條件

### <a name="hardware-requirements"></a>硬體需求

漫遊使用者設定檔需要 x64 或 x86 型電腦；但 Windows RT. 並不支援。

### <a name="software-requirements"></a>軟體需求

漫遊使用者設定檔具有下列軟體需求：

- 如果您要在已經有本機使用者設定檔的環境中部署漫遊使用者設定檔與資料夾重新導向，請先部署資料夾重新導向再部署漫遊設定檔，以減少漫遊設定檔的大小。 現有的使用者資料夾成功重新導向之後，您就可以部署漫遊使用者設定檔。
- 若要管理漫遊使用者設定檔，您必須以 Domain Administrators 安全性群組、Enterprise Administrators 安全性群組或 Group Policy Creator Owners 安全性群組的成員身分登入。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。
- 用戶端電腦必須加入您所管理的 Active Directory 網域服務 (AD DS)。
- 必須有一部電腦安裝群組原則管理與 Active Directory 系統管理中心。
- 必須有一部檔案伺服器裝載漫遊使用者設定檔。

    - 如果檔案共用使用 DFS 命名空間，DFS 資料夾 (連結) 必須有單一的目標，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用使用 DFS 複寫與與另一部伺服器複寫內容，使用者必須只能存取來源伺服器，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用已叢集，停用檔案共用的持續可用性以避免效能問題。
- 若要使用漫遊使用者設定檔中的主要電腦支援，還有其他用戶端電腦和 Active Directory 架構需求。 如需詳細資訊，請參閱[針對資料夾重新導向和漫遊使用者設定檔部署主要電腦](deploy-primary-computers.md)。
- 如果使用者使用不只一台電腦、遠端桌面工作階段主機或虛擬桌面基礎結構 (VDI) 伺服器，則開始功能表的配置不會在 Windows 10、Windows Server 2019 或 Windows Server 2016 上漫遊。 因應措施是，您可以如本主題中所述，指定開始功能表配置。 或者，您可以利用使用者設定檔磁碟，在與遠端桌面工作階段主機伺服器或 VDI 伺服器搭配使用時，適當地漫遊開始功能表設定。 如需詳細資訊，請參閱[在 Windows Server 2012 中透過使用者設定檔磁碟輕鬆管理使用者資料](https://techcommunity.microsoft.com/t5/microsoft-security-and/easier-user-data-management-with-user-profile-disks-in-windows/ba-p/247555)。

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>在多個版本的 Windows 上使用漫遊使用者設定檔時的考量

如果您決定在多個版本的 Windows 上使用漫遊使用者設定檔，我們建議您採取下列動作：

- 設定 Windows 為每個作業系統版本維護不同的設定檔版本。 這有助於防止不必要和無法預期的問題，例如設定檔損毀。
- 使用資料夾重新導向來儲存使用者檔案，例如使用者設定檔以外的文件和圖片。 這可讓使用者跨多個作業系統版本使用相同的檔案。 也可以維持小型的設定檔，並且快速登入。
- 為漫遊使用者設定檔配置足夠的儲存空間。 如果您支援兩種作業系統版本，設定檔的數目 (以及總空間) 也會加倍，因為要為每個作業系統版本維護不同的設定檔。
- 不要在執行 Windows Vista/Windows Server 2008 和 Windows 7/Windows Server 2008 R2 的電腦之間使用漫遊使用者設定檔。 因為這些設定檔版本不相容，所以不支援在這些作業系統版本之間漫遊。
- 請通知您的使用者，在某個作業系統版本進行的變更，將不會漫遊到另一個作業系統版本。
- 將環境移至使用不同設定檔版本 (例如從 Windows 10 移到 Windows 10 版本 1607 - 請參閱[附錄 B：設定檔版本參考資訊](#appendix-b-profile-version-reference-information)中所列清單) 時，使用者會收到新的空白漫遊使用者設定檔。 您可以使用資料夾重新導向來重新導向至一般資料夾，將取得新設定檔的影響降至最低。 沒有任何一種支援方法，可將漫遊使用者設定檔從一個設定檔版本遷移到另一個版本。

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>步驟 1：使用不同的設定檔版本

如果您在執行 Windows 8.1、Windows 8、Windows Server 2012 R2 或 Windows Server 2012 的電腦上部署漫遊使用者設定檔，建議您在部署之前，先對 Windows 環境進行一些變更。 這些變更可協助確保未來的作業系統升級順暢進行，並提昇同時執行多個版本的 Windows 與漫遊使用者設定檔的能力。

若要進行這些變更，請使用下列程序。

1. 在所有您要使用漫遊、強制、超級強制或網域預設設定檔的電腦下載並安裝適當的軟體更新：

    - Windows 8.1 或 Windows Server 2012 R2：安裝 Microsoft 知識庫中 [2887595](https://support.microsoft.com/kb/2887595) 文章所述的軟體更新 (發行後)。
    - Windows 8 或 Windows Server 2012：安裝 Microsoft 知識庫中 [2887239](https://support.microsoft.com/kb/2887239) 文章所述的軟體更新。

2. 在執行 Windows 8.1、Windows 8、Windows Server 2012 R2 或 Windows Server 2012 且您要使用漫遊使用者設定檔的所有電腦上，使用登錄編輯程式或群組原則來建立下列登錄機碼 DWORD 值，並將它設定為 `1`。 如需使用群組原則建立登錄機碼的相關資訊，請參閱 [設定登錄項目](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。
3. 重新啟動電腦。

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>步驟 2：建立漫遊使用者設定檔安全性群組

如果您的環境尚未設定漫遊使用者設定檔，第一個步驟是建立安全性群組，其中包含您要套用漫遊使用者設定檔原則設定的所有使用者和/或電腦。

- 一般用途漫遊使用者設定檔部署的系統管理員通常會建立使用者的安全性群組。
- 遠端桌面服務或虛擬桌面部署的系統管理員通常會為使用者與共用的電腦使用安全性群組。

如何建立漫遊使用者設定檔的安全性群組：

1. 在已安裝 Active Directory 系統管理中心的電腦上開啟伺服器管理員。
2. 在 [工具] 功能表上，選取 [Active Directory 系統管理中心]。 [Active Directory 系統管理中心] 隨即顯示。
3. 以滑鼠右鍵按一下適當的網域或 OU，選取 [新增]，然後選取 [群組] 。
4. 在 [建立群組]  視窗內的 [群組]  區段中，指定下列設定：

    - 在 [群組名稱]  中，輸入安全性群組的名稱，例如：**漫遊使用者設定檔使用者和電腦**。
    - 在 [群組領域]  中，選取 [安全性]  ，然後選取 [全域]  。

5. 在 [成員] 區段中，選取 [新增]。 [選取使用者、連絡人、電腦、服務帳戶或群組] 對話方塊隨即顯示。
6. 如果想要在安全性群組納入電腦帳戶，請選取 [物件類型]  、[電腦]  核取方塊，然後選取 [確定]  。
7. 輸入想要部署漫遊使用者設定檔的使用者、群組和/或電腦的名稱，選取 [確定]  ，然後再選取 [確定]  。

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>步驟 3：建立漫遊使用者設定檔的檔案共用

如果漫遊使用者設定檔還沒有個別的檔案共用 (獨立於任何重新導向資料夾的共用，以防止不小心快取漫遊設定檔資料夾)，請使用下列程序在執行 Windows Server 的伺服器上建立檔案共用。

> [!NOTE]
> 根據您所使用的 Windows Server 版本而定，部分功能可能有所不同或無法使用。

以下說明如何在 Windows Server 上建立檔案共用：

1. 在 [伺服器管理員] 瀏覽窗格中，選取 [檔案和存放服務]  ，然後選取 [共用]  以顯示 [共用] 頁面。
2. 在 [共用] 磚中，選取 [工作]  ，然後選取 [新增共用]  。 「新增共用精靈」隨即顯示。
3. 在 [選取設定檔] 頁面上，選取 [SMB 共用 - 快速]。 如果您已安裝檔案伺服器資源管理員，並使用資料夾管理屬性，則改為選取 [SMB 共用 - 進階]。
4. 在 [共用位置]  頁面上，選取您要在上面建立共用的伺服器和磁碟區。
5. 在 [共用名稱]  頁面的 [共用名稱]  方塊中輸入共用的名稱 (例如， **使用者設定檔$** )。

    > [!TIP]
    > 建立共用時，在共用名稱後面放一個 ```$``` 可隱藏共用。 這會隱藏共用，不會隨便被瀏覽。

6. 在 [其他設定]  頁面上，清除 [啟用持續可用性]  核取方塊 (如果存在)，選擇性地選取 [啟用存取型列舉]  和 [加密資料存取]  核取方塊。
7. 在 [權限]  頁面上，選取 [自訂權限...]  。 [進階安全性設定] 對話方塊隨即出現。
8. 選取 [停用繼承]  ，然後選取 [將繼承的權限轉換成此物件中的明確權限]  。
9. 如[檔案共用裝載漫遊使用者設定檔的必要權限](#required-permissions-for-the-file-share-hosting-roaming-user-profiles)和以下螢幕截取畫面中所述內容設定權限，移除未列出群組和帳戶的權限，並將特殊權限新增至您在步驟 1 中建立的漫遊使用者設定檔使用者和電腦群組。

    ![表 1 中說明的顯示安全性設定視窗權限](media/advanced-security-user-profiles.jpg)

    **圖 1** 設定漫遊使用者設定檔共用的權限
10. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [管理屬性]  頁面上，選取 [使用者檔案]  資料夾使用方式值。
11. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [配額]  頁面上，選擇性地選取套用至共用之使用者的配額。
12. 在 [確認]  頁面中，選取 [建立]  。

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>檔案共用裝載漫遊使用者設定檔所需的權限

| 使用者帳戶 | 存取權 | 適用對象 |
|   -   |   -   |   -   |
|   System    |  完全控制     |  這個資料夾、子資料夾及檔案     |
|  Administrators     |  完全控制     |  只有這個資料夾     |
|  建立者/擁有者     |  完全控制     |  子資料夾及檔案     |
| 需要將資料放在共用上的使用者安全性群組 (漫遊使用者設定檔使用者和電腦)      |  列出資料夾/讀取資料 (進階權限)  <br />建立資料夾/附加資料 (進階權限)  |  只有這個資料夾     |
| 其他群組與帳戶   |  無 (移除)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>步驟 4：選擇性建立漫遊使用者設定檔的 GPO

如果您還沒有針對漫遊使用者設定檔設定建立 GPO，請使用下列程序建立空的 GPO 以用於漫遊使用者設定檔。 這個 GPO 可以讓您設定漫遊使用者設定檔設定 (例如主要電腦支援，將分別討論)，也可用來啟用電腦上的漫遊使用者設定檔，這通常會在部署虛擬桌面環境時或使用遠端桌面服務進行。

如何建立漫遊使用者設定檔的 GPO：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 在 [工具]  功能表上，選取 [群組原則管理]  。 [群組原則管理] 隨即會出現。
3. 以滑鼠右鍵按一下要設定漫遊使用者設定檔的網域或 OU，然後選取 [在這個網域中建立 GPO 並連結]  。
4. 在 [新增 GPO]  對話方塊中，輸入 GPO 的名稱 (例如 **漫遊使用者設定檔設定**)，然後選取 [確定]  。
5. 以滑鼠右鍵按一下新建立的 GPO，然後清除 [啟用連結]  核取方塊。 這可以防止在您完成設定之前就套用 GPO。
6. 選取 GPO。 在 [範圍] 索引標籤的 [安全性篩選] 區段中，選取 [已驗證的使用者]，然後選取 [移除]，避免將 GPO 套用至所有人。
7. 在 [安全性篩選]  區段中，選取 [新增]  。
8. 在 [選取使用者、電腦或群組]  對話方塊中，輸入您在步驟 1 建立的安全性群組名稱 (例如 **漫遊使用者設定檔使用者和電腦**)，然後選取 [確定]  。
9. 選取 [委派]  索引標籤並選取 [新增]  ，輸入 **已驗證的使用者**，再選取 [確定]  ，然後再次選取 [確定]  以接受預設的 [讀取] 權限。

    由於 [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016) 中所做的安全性變更，這是必要步驟。

>[!IMPORTANT]
>由於 [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016) 中所做的安全性變更，現在您必須授與已驗證的使用者群組對 GPO 的「讀取」權限，否則 GPO 將不會套用至使用者；或就算已套用，系統也會移除 GPO，並將使用者設定檔重新導向至本機電腦。 如需詳細資訊，請參閱[部署群組原則安全性更新 MS16-072](/archive/blogs/askds/deploying-group-policy-security-update-ms16-072-kb3163622)。

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>步驟 5：選擇性設定使用者帳戶的漫遊使用者設定檔

如果您要對使用者帳戶部署漫遊使用者設定檔，請使用下列程序在 Active Directory 網域服務中指定使用者帳戶的漫遊使用者設定檔。 如果您要對電腦部署漫遊使用者設定檔 (通常是為遠端桌面服務或虛擬桌面部署時進行)，請改為使用[步驟 6：選擇性設定電腦的漫遊使用者設定檔](#step-6-optionally-set-up-roaming-user-profiles-on-computers)中記載的程序。

> [!NOTE]
> 如果您分別使用 Active Directory 對使用者帳戶，以及使用群組原則對電腦設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

如何設定使用者帳戶的漫遊使用者設定檔：

1. 在 Active Directory 系統管理中心，瀏覽到適當網域中的 [使用者]  容器 (或 OU)。
2. 選取想要指派漫遊使用者設定檔的所有使用者，以滑鼠右鍵按一下使用者，然後選取 [內容]  。
3. 在 [設定檔]  區段中，選取 [設定檔路徑：]  核取方塊，然後輸入想要儲存使用者的漫遊使用者設定檔的檔案共用路徑，後面加上 `%username%` (它會自動取代為使用者第一次登入的使用者名稱)。 例如：

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    若要指定強制的漫遊使用者設定檔，請指定先前建立的 NTuser.man 檔案的路徑，例如 `fs1.corp.contoso.comUser Profiles$default`。 如需詳細資訊，請參閱[建立強制使用者設定檔](/windows/client-management/mandatory-user-profile)。
4. 選取 [確定]  。

> [!NOTE]
> 根據預設，使用漫遊使用者設定檔時，允許部署所有 Windows® 執行階段型 (Windows 市集) 應用程式。 不過，當使用特殊設定檔時，預設不會部署應用程式。 特殊設定檔是使用者登出後捨棄變更的使用者設定檔：
> <br><br>若要移除特殊設定檔的應用程式部署限制，請啟用 **Allow deployment operations in special profiles** 原則設定 (位於 [電腦設定\原則\系統管理範本\Windows 元件\應用程式套件部署])。 不過，在此案例中部署的應用程式會保留部分的資料儲存在電腦上，假如一部電腦有數百位使用者時，資料可能會不斷累積。 若要清除應用程式，請找出或開發使用 [CleanupPackageForUserAsync](/uwp/api/Windows.Management.Deployment.PackageManager?view=winrt-19041#windows_management_deployment_packagemanager_cleanuppackageforuserasync_system_string_system_string_) API 的工具，對於在電腦上不再有設定檔的使用者清除應用程式套件。
> <br><br>如需 Windows 市集應用程式的額外背景資訊，請參閱 [管理 Windows 市集的用戶端存取](</previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>)。

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>步驟 6：選擇性設定電腦的漫遊使用者設定檔

如果您要對電腦部署漫遊使用者設定檔 (通常是為遠端桌面服務或虛擬桌面部署時進行)，請使用下列程序。 如果要對使用者帳戶部署漫遊使用者設定檔，請使用[步驟 5 中所描述的程序：選擇性設定使用者帳戶的漫遊使用者設定檔](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)。

您可以使用群組原則將漫遊使用者設定檔套用至執行 Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的電腦。

> [!NOTE]
> 如果您分別使用群組原則對電腦，以及使用 Active Directory 對使用者帳戶設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

如何在電腦上設定漫遊使用者設定檔：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 在 [工具]  功能表上，選取 [群組原則管理]  。 [群組原則管理] 隨即出現。
3. 在 [群組原則管理] 中，以滑鼠右鍵按一下在步驟 3 中建立的 GPO (例如，[漫遊使用者設定檔設定]  )，然後選取 [編輯]  。
4. 在 [群組原則管理編輯器] 視窗中，依序瀏覽至 [電腦設定]  、[原則]  、[系統管理範本]  、[系統]  ，然後是 [使用者設定檔]  。
5. 以滑鼠右鍵按一下 [為登入此電腦的所有使用者設定漫遊設定檔路徑]  ，然後選取 [編輯]  。
    > [!TIP]
    > 使用者的主目錄資料夾 (如果有設定) 是由某些程式 (例如 Windows PowerShell) 使用的預設資料夾。 您可以使用 AD DS 中使用者帳戶內容的 [主目錄資料夾]  區段，為每一位使用者設定替代或網路位置。 若要為在虛擬桌面環境中執行 Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 之電腦的所有使用者設定主目錄資料夾位置，請啟用 [設定使用者主目錄資料夾]  原則設定，然後指定要對應的檔案共用及磁碟機代號 (或指定本機資料夾)。 不要使用環境變數或省略符號。 在使用者登入期間，使用者別名會附加到指定的路徑結尾。
6. 在 [系統內容]  對話方塊中選取 [啟用] 
7. 在 [登入此電腦的使用者必須使用此漫遊設定檔路徑]  方塊中，輸入想要儲存使用者的漫遊使用者設定檔的檔案共用路徑，後面加上 `%username%` (它會自動取代為使用者第一次登入的使用者名稱)。 例如：

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    若要指定強制漫遊使用者設定檔，這是一個使用者無法進行永久性變更、預先設定的設定檔 (當使用者登出時，變更會重設)，請指定您先前建立的 NTuser.man 檔案的路徑，例如 `\\fs1.corp.contoso.com\User Profiles$\default`。 如需詳細資訊，請參閱 [建立強制使用者設定檔](/windows/client-management/mandatory-user-profile)。
8. 選取 [確定]  。

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>步驟 7：選擇性指定 Windows 10 電腦的開始功能表配置

您可以使用群組原則來套用特定的開始功能表配置，讓使用者在所有電腦上看到相同的開始功能表配置。 如果使用者登入一部以上的電腦，而您想要讓他們在電腦之間擁有一致的開始功能表配置，請務必將 GPO 套用到所有電腦。

若要指定開始功能表配置，請執行下列動作：

1. 將 Windows 10 電腦更新為 Windows 10 1607 版本 (也稱為年度更新版) 或更高版本，並安裝 3 月 14 日的 2017 累積更新 ([KB4013429](https://support.microsoft.com/kb/4013429)) 或較新版本。
2. 建立完整或部分的開始功能表配置 XML 檔案。 若要這樣做，請參閱[自訂和匯出開始功能表配置](/windows/configuration/customize-and-export-start-layout)。
    * 如果您指定 *完整* 開始功能表配置，使用者就無法自訂開始功能表的任何部分。 如果您指定 *部分* 開始功能表配置，使用者可以自訂任何項目，但系統會鎖定您指定的磚群組。 不過，使用部分開始功能表配置時，其使用者自訂項目不會漫遊到其他電腦。
3. 使用群組原則將自訂的開始功能表配置套用至您為漫遊使用者設定檔所建立的 GPO。 若要這樣做，[使用群組原則在網域中套用自訂的開始功能表配置](/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment)。
4. 使用群組原則在 Windows 10 電腦上設定下列登錄值。 若要這樣做，請參閱[設定登錄項目](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

| **動作**   | **更新**                  |
| ------------ | ------------                |
| 登錄區         | **HKEY_LOCAL_MACHINE**      |
| 機碼路徑     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| 值名稱   | **SpecialRoamingOverrideAllowed** |
| 值類型   | **REG_DWORD**               |
| [數值資料]   | **1** (或 **0** 表示停用) |
| 基本         | **十進位**                 |

5. (選用) 啟用第一次登入最佳化，以更快速地登入使用者。 若要這麼做，請參閱[套用原則以改善登入時間](/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time)。
6. (選用) 從用來部署用戶端電腦的 Windows 10 基礎映像中移除不必要的應用程式，以進一步減少登入次數。 Windows Server 2019 和 Windows Server 2016 沒有任何預先佈建的應用程式，因此您可以在伺服器映像上略過此步驟。
    - 若要移除應用程式，請使用 Windows PowerShell 中的 [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage) Cmdlet 將下列應用程式解除安裝。 如果您的電腦已部署完成，可以使用 [Remove-AppxPackage](/powershell/module/appx/remove-appxpackage) 來編寫移除這些應用程式的程式碼。

      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>將這些應用程式解除安裝會減少登入次數，但如果您的部署需要其中任何一項應用程式，可以將它們保留為已安裝。

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>步驟 8：啟用漫遊使用者設定檔 GPO

如果您使用群組原則對電腦設定漫遊使用者設定檔，或如果您使用群組原則自訂其他漫遊使用者設定檔設定，下一步是要啟用 GPO，允許它套用至受影響的使用者。

>[!TIP]
>如果您計劃實作主要電腦支援，請立即進行此動作，才能啟用 GPO。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 如需詳細資訊，請參閱[針對資料夾資料夾和漫遊使用者設定檔部署主要電腦](deploy-primary-computers.md)。

如何啟用漫遊使用者設定檔 GPO：

1. 開啟 [群組原則管理]。
2. 以滑鼠右鍵按一下您所建立的 GPO，然後選取 [啟用連結]  。 功能表項目旁會顯示核取方塊。

## <a name="step-9-test-roaming-user-profiles"></a>步驟 9：測試漫遊使用者設定檔

若要測試漫遊使用者設定檔，請使用設定漫遊使用者設定檔的使用者帳戶登入電腦，或登入設定漫遊使用者設定檔的電腦。 然後確認重新導向設定檔。

如何測試漫遊使用者設定檔：

1. 使用您已經啟用漫遊使用者設定檔的使用者帳戶，登入主要電腦 (如果您已啟用主要電腦支援)。 如果您在特定電腦上啟用漫遊使用者設定檔，請登入其中一部電腦。
2. 如果使用者先前已登入電腦，請開啟提升權限的命令提示字元，然後輸入下列命令，以確保最新的群組原則設定套用到用戶端電腦：

    ```PowerShell
    GpUpdate /Force
    ```
3. 若要確認使用者設定檔正在漫遊，請開啟 [控制台]  ，依序選取 [系統及安全性]  、[系統]  、[進階系統設定]  ，選取 [使用者設定檔] 區段中的 [設定]  ，然後在 [類型]  欄位中尋找 [漫遊]  。

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>附錄 A：部署漫遊使用者設定檔的檢查清單

| 狀態                     | 動作                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1.準備網域<br>- 將電腦加入網域<br>- 使用不同的設定檔版本<br>- 建立使用者帳戶<br>- (選用) 部署資料夾重新導向 |
| ☐<br><br><br>             | 2.建立漫遊使用者設定檔的安全性群組<br>- 群組名稱：<br>- 成員： |
| ☐<br><br>                 | 3.建立漫遊使用者設定檔的檔案共用<br>- 檔案共用名稱： |
| ☐<br><br>                 | 4.建立漫遊使用者設定檔的 GPO<br>- GPO 名稱：|
| ☐                         | 5.設定漫遊使用者設定檔原則設定    |
| ☐<br>☐<br>☐              | 6.啟用漫遊使用者設定檔<br>- 是否已在 AD DS 中的使用者帳戶啟用？<br>- 是否已在群組原則中的電腦帳戶啟用？<br> |
| ☐                         | 7.(選用) 指定 Windows 10 電腦的強制開始功能表配置 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8.(選用) 啟用主要電腦支援<br>- 指定使用者的主要電腦<br>- 使用者與主要電腦的位置對應：<br>- (選用) 啟用資料夾重新導向的主要電腦支援<br>- 以電腦為目標或使用者為目標？<br>- (選用) 啟用漫遊使用者設定檔的主要電腦支援 |
| ☐                        | 9.啟用漫遊使用者設定檔 GPO                |
| ☐                        | 10.測試漫遊使用者設定檔                         |

## <a name="appendix-b-profile-version-reference-information"></a>附錄 B：設定檔版本參考資訊

每個設定檔都有一個設定檔版本，大致上會對應到使用此設定檔的 Windows 版本。 例如，Windows 10 版本 1703 和版本 1607 都使用 .V6 設定檔版本。 只有在需要維護相容性時，Microsoft 才會建立新的設定檔版本，這也是為什麼並非每個 Windows 版本都包含新的設定檔版本。

下表列出各種版本的 Windows 上漫遊使用者設定檔的位置。

| 作業系統版本 | 漫遊使用者設定檔位置 |
| --- | --- |
| Windows XP 與 Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista 與 Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 與 Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 與 Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (套用軟體更新和登錄機碼後)<br>```\\<servername>\<fileshare>\<username>.V2``` (套用軟體更新和登錄機碼前) |
| Windows 8.1 與 Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (套用軟體更新和登錄機碼後)<br>```\\<servername>\<fileshare>\<username>.V2``` (套用軟體更新和登錄機碼前) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10 版本 1703 和版本 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>附錄 C：升級後重設開始功能表配置的因應措施

以下是在就地升級之後，開始功能表配置重設問題的一些解決方法：

- 如果只有一位使用者使用裝置，而 IT 系統管理員使用受控作業系統部署策略 (例如 Configuration Manager)，則系統管理員可以執行下列動作：

  1. 在升級之前，使用 Export-Startlayout 匯出開始功能表配置
  2. 在 OOBE 之後但在使用者登入之前，使用 Import-StartLayout 匯入開始功能表配置

     > [!NOTE]
     > 匯入 StartLayout 會修改預設的使用者設定檔。 在匯入之後建立的所有使用者設定檔都會取得匯入的 Start-Layout。

- IT 系統管理員可以選擇使用群組原則來管理開始功能表的配置。 使用群組原則可提供集中式管理解決方案，將標準化的開始功能表配置套用至使用者。 使用開始管理的群組原則的模式有 2 種，分別是： 完整鎖定和部分鎖定。 完整鎖定的情況可防止使用者對開始功能表配置進行任何變更。 部分鎖定的情況可讓使用者對開始功能表的特定區域進行變更。 如需詳細資訊，請參閱[自訂和匯出開始功能表配置](/windows/configuration/customize-and-export-start-layout)。

   > [!NOTE]
   > 若使用者在部分鎖定的情況下進行變更，升級時仍然無法保有這些變更。

- 如此系統會重設開始功能表配置，並允許終端使用者重新設定開始功能表。 您可以將通知電子郵件或其他通知傳送給終端使用者，說明開始功能表配置會在作業系統升級後重設，以將影響降至最低。

## <a name="change-history"></a>變更歷程記錄

下表摘要說明本主題中一些最重要的變更。

| 日期 | 說明 |原因|
| --- | ---         | ---   |
| 2019 年 5 月 1 日 | 新增了 Windows Server 2019 的更新 |
| 2018 年 4 月 10 日 | 新增了在作業系統就地升級後，何時會失去開始功能表中使用者自訂的討論|圖說文字已知問題。 |
| 2018 年 3 月 13 日 | Windows Server 2016 更新 | 已移出舊版的程式庫，並已更新為最新版本的 Windows Server。 |
| 2017 年 4 月 13 日 | 新增了 Windows 10 版本 1703 的設定檔資訊，並闡明漫遊設定檔版本在升級作業系統時的運作方式 - 請參閱[在多個版本的 Windows 上使用漫遊使用者設定檔的考量](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)。 | 客戶意見反應。 |
| 2017 年 3 月 14 日 | 在[附錄 A：部署漫遊使用者設定檔的檢查清單](#appendix-a-checklist-for-deploying-roaming-user-profiles)中新增了選用步驟，為 Windows 10 電腦指定強制開始功能表配置。 |最新 Windows Update 中的功能變更。 |
| 2017 年 1 月 23 日 | 將步驟新增至[步驟 4：選擇性建立漫遊使用者設定檔的 GPO](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) 中，以將讀取權限委派給已驗證的使用者；由於群組原則的安全性更新，因此這是必須進行的步驟。|群組原則處理的安全性變更。 |
| 2016 年 12 月 29 日 | 在[步驟 8：啟用漫遊使用者設定檔 GPO](#step-8-enable-the-roaming-user-profiles-gpo) 中新增了連結，讓您更輕鬆地取得如何為主要電腦設定群組原則的資訊； 也修正了步驟 5 和 6 的幾個參考資料，其中包含錯誤的數字。|客戶意見反應。 |
| 2016 年 12 月 5 日 | 新增了說明開始功能表設定漫遊問題的資訊。 | 客戶意見反應。 |
| 2016 年 7 月 6 日 | 已在[附錄 B：設定檔版本參考資訊](#appendix-b-profile-version-reference-information)中新增了 Windows 10 設定檔版本尾碼， 同時從支援的作業系統清單中移除了 Windows XP 和 Windows Server 2003。 | 新 Windows 版本的更新，並移除了不再支援的 Windows 版本相關資訊。 |
| 2015 年 7 月 7日 | 使用叢集的檔案伺服器時新增需求和步驟以停用持續可用性。 | 當停用持續可用性時，叢集的檔案共用對於小型寫入 (一般用於漫遊使用者設定檔) 擁有較佳的效能。 |
| 2014 年 3 月 19 日 | [附錄 B：設定檔版本參考資訊](#appendix-b-profile-version-reference-information)中大寫的設定檔版本尾碼 (.V2、.V3、.V4)。 | 雖然 Windows 不區分大小寫，但如果您使用 NFS 做為檔案共用，則設定檔尾碼一定要有正確的大小寫 (大寫)。 |
| 2013 年 10 月 9 日 | 修訂並釐清了 Windows Server 2012 R2 和 Windows 8.1 的幾項內容，並新增了[在多個版本的 Windows 上使用漫遊使用者設定檔時的考量](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)和[附錄 B：設定檔版本參考資訊](#appendix-b-profile-version-reference-information)各節。 | 更新為新版本；客戶意見反應。 |

## <a name="more-information"></a>其他資訊

- [部署資料夾重新導向、離線檔案及漫遊使用者設定檔](deploy-folder-redirection.md)
- [部署資料夾重新導向及漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)
- [實作使用者狀態管理](</previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Microsoft 對於複寫使用者設定檔資料的支援聲明](/archive/blogs/askds/microsofts-support-statement-around-replicated-user-profile-data)
- [使用 DISM 側載應用程式](</previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [封裝、部署及查詢 Windows 執行階段型應用程式疑難排解](/windows/win32/appxpkg/troubleshooting)
