---
title: 部署漫遊使用者設定檔
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: b7a89ce8d72cf4f060e83b3653b3b2d93eed5cfd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402033"
---
# <a name="deploying-roaming-user-profiles"></a>部署漫遊使用者設定檔

>適用於：Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

本主題說明如何使用 Windows Server 將[漫遊使用者設定檔](folder-redirection-rup-overview.md)部署至 windows 用戶端電腦。 漫遊使用者設定檔會將使用者設定檔重新導向到檔案共用，讓使用者可在多部電腦上接收相同的作業系統和應用程式設定。

如需本主題的最近變更清單，請參閱本主題的[變更歷程記錄](#change-history)一節。

> [!IMPORTANT]
> 由於[MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)中所做的安全性變更，我們更新[了步驟4：（選擇性）在本主題中建立](#step-4-optionally-create-a-gpo-for-roaming-user-profiles)漫遊使用者設定檔的 GPO，讓 Windows 可以適當地套用漫遊使用者設定檔原則（而不是還原為受影響電腦上的本機原則）。

> [!IMPORTANT]
>  在下列設定中進行 OS 就地升級之後，將會遺失使用者自訂啟動：
> - 已設定漫遊設定檔的使用者
> - 允許使用者進行變更以開始
>
> 因此，在 OS 就地升級之後，[開始] 功能表會重設為預設的新 OS 版本。 如需因應[措施，請參閱附錄 C：在升級](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)之後，請解決重設開始功能表的版面配置。

## <a name="prerequisites"></a>必要條件

### <a name="hardware-requirements"></a>硬體需求

漫遊使用者設定檔需要 x64 型或 x86 型電腦;Windows RT 並不支援。

### <a name="software-requirements"></a>軟體需求

漫遊使用者設定檔具有下列軟體需求：

- 如果您要在已經有本機使用者設定檔的環境中部署漫遊使用者設定檔與資料夾重新導向，請先部署資料夾重新導向再部署漫遊設定檔，以減少漫遊設定檔的大小。 現有的使用者資料夾成功重新導向之後，您就可以部署漫遊使用者設定檔。
- 若要管理漫遊使用者設定檔，您必須以 Domain Administrators 安全性群組、Enterprise Administrators 安全性群組或 Group Policy Creator Owners 安全性群組的成員身分登入。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。
- 用戶端電腦必須加入您所管理的 Active Directory 網域服務 (AD DS)。
- 必須有一部電腦安裝群組原則管理與 Active Directory 系統管理中心。
- 必須有一部檔案伺服器裝載漫遊使用者設定檔。

    - 如果檔案共用使用 DFS 命名空間，DFS 資料夾 (連結) 必須有單一的目標，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用使用 DFS 複寫與與另一部伺服器複寫內容，使用者必須只能存取來源伺服器，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用已叢集，停用檔案共用的持續可用性以避免效能問題。
- 若要使用漫遊使用者設定檔中的主要電腦支援，有其他用戶端電腦和 Active Directory 架構需求。 如需詳細資訊，請參閱[部署資料夾重新導向和漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)。
- 使用者的 [開始] 功能表配置在使用多部電腦、遠端桌面工作階段主機或虛擬桌面基礎結構（VDI）伺服器時，不會在 Windows 10、Windows Server 2019 或 Windows Server 2016 上漫遊。 因應措施是，您可以指定開始配置，如本主題中所述。 或者，您可以利用使用者設定檔磁片，在與遠端桌面工作階段主機伺服器或 VDI 伺服器搭配使用時，適當地漫遊 [開始] 功能表設定。 如需詳細資訊，請參閱[Windows Server 2012 中的使用者設定檔磁片，讓使用者更容易資料管理](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/)。

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>在多個版本的 Windows 上使用漫遊使用者設定檔時的考量

如果您決定在多個版本的 Windows 上使用漫遊使用者設定檔，我們建議您採取下列動作：

- 設定 Windows 為每個作業系統版本維護不同的設定檔版本。 這有助於防止不必要和無法預期的問題，例如設定檔損毀。
- 使用資料夾重新導向來儲存使用者檔案，例如使用者設定檔以外的文件和圖片。 這可讓使用者跨多個作業系統版本使用相同的檔案。 也可以維持小型的設定檔，並且快速登入。
- 為漫遊使用者設定檔配置足夠的儲存空間。 如果您支援兩種作業系統版本，設定檔的數目 (以及總空間) 也會加倍，因為要為每個作業系統版本維護不同的設定檔。
- 不要在執行 Windows Vista/Windows Server 2008 和 Windows 7/Windows Server 2008 R2 的電腦之間使用漫遊使用者設定檔。 因為設定檔版本不相容，所以不支援在這些作業系統版本之間漫遊。
- 通知使用者在某個作業系統版本上所做的變更不會漫遊到另一個作業系統版本。
- 將您的環境移至使用不同設定檔版本的 windows 版本時（例如從 Windows 10 到 windows 10 版本1607），請參閱[附錄 B：分析清單的版本](#appendix-b-profile-version-reference-information)參考資訊），使用者會收到新的空白漫遊使用者設定檔。 您可以使用資料夾重新導向來重新導向一般資料夾，以將取得新設定檔的影響降至最低。 不支援將漫遊使用者設定檔從一個設定檔版本遷移到另一個的方法。

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>步驟 1:使用不同的設定檔版本

如果您要在執行 Windows 8.1、Windows 8、Windows Server 2012 R2 或 Windows Server 2012 的電腦上部署漫遊使用者設定檔，建議您在部署之前，先對 Windows 環境進行幾項變更。 這些變更可協助確保未來的作業系統升級順暢進行，並提昇同時執行多個版本的 Windows 與漫遊使用者設定檔的能力。

若要進行這些變更，請使用下列程序。

1. 在您要使用漫遊、強制、超級強制或網域預設設定檔的所有電腦上，下載並安裝適當的軟體更新：

    - Windows 8.1 或 Windows Server 2012 R2：安裝 Microsoft 知識庫文章[2887595](http://support.microsoft.com/kb/2887595) （發行時）中所述的軟體更新。
    - Windows 8 或 Windows Server 2012：安裝 Microsoft 知識庫中 [2887239](http://support.microsoft.com/kb/2887239) 文章所述的軟體更新。

2. 在執行 Windows 8.1、Windows 8、Windows Server 2012 R2 或 Windows Server 2012 （您將在其上使用漫遊使用者設定檔）的所有電腦上，使用登錄編輯程式或群組原則建立下列登錄機碼 DWORD 值`1`，並將它設定為。 如需使用群組原則建立登錄機碼的相關資訊，請參閱 [設定登錄項目](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。
3. 重新啟動電腦。

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>步驟 2:建立漫遊使用者設定檔安全性群組

如果您的環境尚未設定漫遊使用者設定檔，第一個步驟是建立安全性群組，其中包含您要套用漫遊使用者設定檔原則設定的所有使用者和/或電腦。

- 一般用途漫遊使用者設定檔部署的系統管理員通常會建立使用者的安全性群組。
- 遠端桌面服務或虛擬桌面部署的系統管理員通常會為使用者與共用的電腦使用安全性群組。

以下說明如何建立漫遊使用者設定檔的安全性群組：

1. 在已安裝 Active Directory 系統管理中心的電腦上開啟伺服器管理員。
2. 在 [**工具**] 功能表上，選取 [ **Active Directory 系統管理中心**]。 [Active Directory 系統管理中心] 隨即顯示。
3. 在適當的網域或 OU 上按一下滑鼠右鍵，選取 [**新增**]，然後選取 [**群組**]。
4. 在 [建立群組] 視窗內的 [群組] 區段中，指定下列設定：

    - 在 [群組名稱] 中，輸入安全性群組的名稱，例如：**漫遊使用者設定檔使用者和電腦**。
    - 在 [**群組領域**] 中，選取 [**安全性**]，然後選取 [**全域**]。

5. 在 [**成員**] 區段中，選取 [**新增**]。 [選取使用者、連絡人、電腦、服務帳戶或群組] 對話方塊隨即顯示。
6. 如果您想要在安全性群組中包含電腦帳戶，請選取 [**物件類型**]，選取 [**電腦**] 核取方塊，然後選取 **[確定]** 。
7. 輸入您要部署漫遊使用者設定檔的使用者、群組和/或電腦的名稱，選取 **[確定]** ，然後再次選取 **[確定]** 。

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>步驟 3：建立漫遊使用者設定檔的檔案共用

如果漫遊使用者設定檔還沒有個別的檔案共用（獨立于任何重新導向資料夾的共用，以防止不小心快取漫遊設定檔資料夾），請使用下列程式在執行 Windows 的伺服器上建立檔案共用。伺服器.

> [!NOTE]
> 根據您所使用的 Windows Server 版本而定，某些功能可能有所不同或無法使用。

以下說明如何在 Windows Server 上建立檔案共用：

1. 在伺服器管理員流覽窗格中，選取 [檔案**和存放服務**]，然後選取 [**共用**] 以顯示 [共用] 頁面。
2. 在 [共用] 磚中 **，選取 [** 工作]，然後選取 [**新增共用**]。 「新增共用精靈」隨即顯示。
3. 在 [**選取設定檔**] 頁面上，選取 [ **SMB 共用-快速**]。 如果您已安裝檔案伺服器 Resource Manager 並使用資料夾管理屬性，請改為選取 [ **SMB 共用-Advanced**]。
4. 在 [共用位置] 頁面上，選取您要在上面建立共用的伺服器和磁碟區。
5. 在 [共用名稱] 頁面的 [共用名稱]方塊中輸入共用的名稱 (例如， **使用者設定檔$** )。

    > [!TIP]
    > 建立共用時，在共用名稱後面放一個 ```$``` 可隱藏共用。 這會隱藏共用，不會隨便被瀏覽。

6. 在 [其他設定] 頁面上，清除 [啟用持續可用性] 核取方塊 (如果存在)，選擇性地選取 [啟用存取型列舉] 和 [加密資料存取] 核取方塊。
7. 在 [**許可權**] 頁面上，選取 [**自訂許可權**]。 [進階安全性設定] 對話方塊隨即出現。
8. 選取 [**停用繼承**]，然後選取 [**將繼承的許可權轉換成此物件的明確許可權**]。
9. 依照[主控漫遊使用者設定檔之檔案共用的必要許可權](#required-permissions-for-the-file-share-hosting-roaming-user-profiles)中所述，設定許可權，並在下列螢幕擷取畫面中顯示、移除未列出群組和帳戶的許可權，以及將特殊許可權新增到漫遊使用者分析您在步驟1中建立的 [使用者和電腦] 群組。
    
    ![進階安全性設定 視窗顯示在 表 1 中所述的權限](media/advanced-security-user-profiles.jpg)
    
    **圖 1** 設定漫遊使用者設定檔共用的權限
10. 如果您選擇 [SMB 共用 - 進階] 設定檔，在 [管理屬性] 頁面上，選取 [使用者檔案] 資料夾使用方式值。
11. 如果您選擇 [SMB 共用 - 進階] 設定檔，在 [配額] 頁面上，選擇性地選取套用至共用之使用者的配額。
12. 在 [**確認**] 頁面上，選取 [**建立]。**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>主控漫遊使用者設定檔的檔案共用所需的許可權

| 使用者帳戶 | 存取權 | 適用於 |
|   -   |   -   |   -   |
|   系統    |  完全控制     |  這個資料夾、子資料夾及檔案     |
|  Administrators     |  完全控制     |  只有這個資料夾     |
|  建立者/擁有者     |  完全控制     |  子資料夾及檔案     |
| 需要將資料放在共用上的使用者安全性群組 (漫遊使用者設定檔使用者和電腦)      |  列出資料夾/讀取資料 *（Advanced 許可權）* <br />建立資料夾/附加資料 *（Advanced 許可權）* |  只有這個資料夾     |
| 其他群組與帳戶   |  無 (移除)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>步驟 4：選擇性建立漫遊使用者設定檔的 GPO

如果您還沒有針對漫遊使用者設定檔設定建立 GPO，請使用下列程序建立空的 GPO 以用於漫遊使用者設定檔。 這個 GPO 可以讓您設定漫遊使用者設定檔設定 (例如主要電腦支援，將分別討論)，也可用來啟用電腦上的漫遊使用者設定檔，這通常會在部署虛擬桌面環境時或使用遠端桌面服務進行。

以下說明如何建立漫遊使用者設定檔的 GPO：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從 [**工具**] 功能表中，選取 [**群組原則管理**]。 [群組原則管理] 隨即會出現。
3. 在您要設定漫遊使用者設定檔的網域或 OU 上按一下滑鼠右鍵，然後選取 [**在這個網域中建立 GPO 並連結到**]。
4. 在 [**新增 gpo** ] 對話方塊中，輸入 GPO 的名稱（例如，**漫遊使用者設定檔設定**），然後選取 **[確定]** 。
5. 以滑鼠右鍵按一下新建立的 GPO，然後清除 [啟用連結] 核取方塊。 這可以防止在您完成設定之前就套用 GPO。
6. 選取 GPO。 在 [**領域**] 索引標籤的 [**安全性篩選**] 區段中，選取 [**已驗證的使用者**]，然後選取 [**移除**] 以防止將 GPO 套用至所有人。
7. 在 [**安全性篩選**] 區段中，選取 [**新增**]。
8. 在 [**選取使用者、電腦或群組**] 對話方塊中，輸入您在步驟1中建立的安全性群組名稱（例如，[**漫遊使用者設定檔] [使用者和電腦**]），然後選取 **[確定]** 。
9. 選取 [**委派**] 索引標籤，選取 [**新增**]，輸入**已驗證的使用者**，選取 **[確定]** ，然後再次選取 **[確定]** 以接受預設的讀取權限。
    
    這是必要步驟，因為[MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)中所做的安全性變更。

>[!IMPORTANT]
>由於[MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)中所做的安全性變更，您現在必須授與已驗證的使用者群組對 GPO 的讀取權限，否則 gpo 將不會套用至使用者，或者如果已套用，則會移除 gpo，重新導向使用者設定檔本機電腦。 如需詳細資訊，請參閱[部署群組原則安全性更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>步驟 5：選擇性設定使用者帳戶的漫遊使用者設定檔

如果您要對使用者帳戶部署漫遊使用者設定檔，請使用下列程序在 Active Directory 網域服務中指定使用者帳戶的漫遊使用者設定檔。 如果您要將漫遊使用者設定檔部署到電腦，通常是針對遠端桌面服務或虛擬桌面部署進行，請改用[步驟6：選擇性設定電腦](#step-6-optionally-set-up-roaming-user-profiles-on-computers)上的漫遊使用者設定檔。

> [!NOTE]
> 如果您分別使用 Active Directory 對使用者帳戶，以及使用群組原則對電腦設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

以下說明如何設定使用者帳戶的漫遊使用者設定檔：

1. 在 Active Directory 系統管理中心，瀏覽到適當網域中的 [使用者] 容器 (或 OU)。
2. 選取您要指派漫遊使用者設定檔的所有使用者，以滑鼠右鍵按一下使用者，然後選取 [**屬性**]。
3. 在 [**設定檔**] 區段中，選取 [**設定檔路徑：** ] 核取方塊，然後輸入您要儲存使用者的漫遊使用者設定檔的檔案共用路徑`%username%` ，後面加上（它會自動取代為第一個的使用者名稱使用者登入的時間）。 例如:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    若要指定強制的漫遊使用者設定檔，請指定您先前建立之 Ntuser.dat 的路徑， `fs1.corp.contoso.comUser Profiles$default`例如。 如需詳細資訊，請參閱[建立強制使用者設定檔](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
4. 選取 [確定]。

> [!NOTE]
> 根據預設，使用漫遊使用者設定檔時，允許部署所有 Windows® 執行階段型 (Windows 市集) 應用程式。 不過，當使用特殊設定檔時，預設不會部署應用程式。 特殊設定檔是使用者登出後捨棄變更的使用者設定檔：
> <br><br>若要移除特殊設定檔的應用程式部署限制，請啟用 [允許特殊設定檔的部署作業] (位於 [電腦設定\原則\系統管理範本\Windows 元件\應用程式套件部署])。 不過，在此案例中部署的應用程式會保留部分的資料儲存在電腦上，假如一部電腦有數百位使用者時，資料可能會不斷累積。 若要清除應用程式，請找出或開發使用[CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API 的工具，針對在電腦上不再有設定檔的使用者清除應用程式套件。
> <br><br>如需 Windows 市集應用程式的額外背景資訊，請參閱[管理 Windows 市集的用戶端存取](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>)。

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>步驟 6：選擇性設定電腦的漫遊使用者設定檔

如果您要對電腦部署漫遊使用者設定檔 (通常是為遠端桌面服務或虛擬桌面部署時進行)，請使用下列程序。 如果您要將漫遊使用者設定檔部署到使用者帳戶，請改為使用[步驟5：選擇性地設定使用者帳戶](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)的漫遊使用者設定檔。

您可以使用群組原則將漫遊使用者設定檔套用至執行 Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的電腦。

> [!NOTE]
> 如果您分別使用群組原則對電腦，以及使用 Active Directory 對使用者帳戶設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

以下是在電腦上設定漫遊使用者設定檔的方法：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從 [**工具**] 功能表中，選取 [**群組原則管理**]。 將會顯示群組原則管理。
3. 在群組原則管理 中，以滑鼠右鍵按一下您在步驟3中建立的 GPO （例如，**漫遊使用者設定檔** 設定），然後選取 **編輯**。
4. 在 [群組原則管理編輯器] 視窗中，依序瀏覽至 [電腦設定]、[原則]、[系統管理範本]、[系統]，然後是 [使用者設定檔]。
5. 在 [**設定所有登入這部電腦的使用者漫遊設定檔路徑] 上**按一下滑鼠右鍵，然後選取 [**編輯**]。
    > [!TIP]
    > 使用者的主目錄資料夾 (如果有設定) 是由某些程式 (例如 Windows PowerShell) 使用的預設資料夾。 您可以使用 AD DS 中使用者帳戶內容的 [主目錄資料夾] 區段，為每一位使用者設定替代或網路位置。 若要為在虛擬桌面環境中執行 Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 之電腦的所有使用者設定主資料夾位置，請啟用 [**設定使用者主資料夾**]。原則設定，然後指定要對應的檔案共用和磁碟機號（或指定本機資料夾）。 不要使用環境變數或省略符號。 使用者的別名會附加到使用者登入期間所指定的路徑結尾。
6. 在 [**屬性**] 對話方塊中，選取 [**已啟用**]
7. 在 [**登入這部電腦的使用者應該使用此漫遊設定檔路徑**] 方塊中，輸入您要儲存使用者的漫遊使用者設定檔的檔案共用路徑，後面`%username%`加上（它會自動取代為使用者名稱使用者第一次登入）。 例如:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    若要指定強制漫遊使用者設定檔，這是使用者無法進行永久變更的預先設定設定檔（當使用者登出時，會重設變更），請指定您先前建立之 Ntuser.dat 檔的路徑， `\\fs1.corp.contoso.com\User Profiles$\default`例如。 如需詳細資訊，請參閱 [建立強制使用者設定檔](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
8. 選取 [確定]。

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>步驟 7：選擇性地指定 Windows 10 電腦的開始版面配置

您可以使用群組原則來套用特定的 [開始] 功能表配置，讓使用者在所有電腦上看到相同的開始配置。 如果使用者登入一部以上的電腦，而您想要讓他們在電腦之間擁有一致的開始配置，請確定 GPO 適用于所有的電腦。

若要指定開始版面配置，請執行下列動作：

1. 將您的 Windows 10 電腦更新為 Windows 10 1607 版（也稱為年度更新版）或更新版本，並安裝3月14日、2017累積更新（[KB4013429](https://support.microsoft.com/kb/4013429)）或更新版本。
2. 建立完整或部分的 [開始] 功能表版面配置 XML 檔案。 若要這麼做，請參閱[自訂和匯出開始](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)配置。
    * 如果您指定*完整*的開始版面配置，使用者就無法自訂 [開始] 功能表的任何部分。 如果您指定*部分*起始版面配置，使用者可以自訂所有專案，但您指定的磚群組已鎖定。 不過，使用部分起始版面配置時，[開始] 功能表的使用者自訂不會漫遊到其他電腦。
3. 使用 [群組原則] 將自訂的開始配置套用至您為漫遊使用者設定檔所建立的 GPO。 若要這麼做，請參閱[使用群組原則將自訂的開始配置套用到網域中](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment)。
4. 使用群組原則在 Windows 10 電腦上設定下列登錄值。 若要這麼做，請參閱[設定登錄專案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

| **動作**   | **更新**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| 金鑰路徑     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| 值名稱   | **SpecialRoamingOverrideAllowed** |
| 值類型   | **REG_DWORD**               |
| [數值資料]   | **1** （或**0**表示停用） |
| Base         | **十**                 |

5. 選擇性啟用第一次登入優化，以更快速地為使用者進行註冊。 若要這麼做，請參閱套用[原則以改善登入時間](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time)。
6. 選擇性從用來部署用戶端電腦的 Windows 10 基礎映射移除不必要的應用程式，以進一步減少登入次數。 Windows Server 2019 和 Windows Server 2016 沒有任何預先布建的應用程式，因此您可以在伺服器映射上略過此步驟。
    - 若要移除應用程式，請使用 Windows PowerShell 中的[AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) Cmdlet 來卸載下列應用程式。 如果您的電腦已經部署，您可以使用[add-appxpackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps)來編寫這些應用程式的移除腳本。
    
      - Windowscommunicationsapps\_8wekyb3d8bbwe
      - BingWeather\_8wekyb3d8bbwe
      - DesktopAppInstaller\_8wekyb3d8bbwe
      - Getstarted\_8wekyb3d8bbwe
      - Microsoft. Windows 相片\_8wekyb3d8bbwe
      - WindowsCamera\_8wekyb3d8bbwe
      - WindowsFeedbackHub\_8wekyb3d8bbwe
      - WindowsStore\_8wekyb3d8bbwe
      - XboxApp\_8wekyb3d8bbwe
      - XboxIdentityProvider\_8wekyb3d8bbwe
      - ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>卸載這些應用程式會減少登入次數，但如果您的部署需要其中任何一項，您可以將它們保留為已安裝。

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>步驟 8：啟用漫遊使用者設定檔 GPO

如果您使用群組原則對電腦設定漫遊使用者設定檔，或如果您使用群組原則自訂其他漫遊使用者設定檔設定，下一步是要啟用 GPO，允許它套用至受影響的使用者。

>[!TIP]
>如果您打算執行主要電腦支援，請在啟用 GPO 之前先執行此動作。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 如需特定的原則設定，請參閱[部署資料夾重新導向和漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)。

以下是啟用漫遊使用者設定檔 GPO 的方式：

1. 開啟 [群組原則管理]。
2. 以滑鼠右鍵按一下您所建立的 GPO，然後選取 [**啟用連結**]。 功能表項目旁會顯示核取方塊。

## <a name="step-9-test-roaming-user-profiles"></a>步驟 9：測試漫遊使用者設定檔

若要測試漫遊使用者設定檔，請使用設定漫遊使用者設定檔的使用者帳戶登入電腦，或登入設定漫遊使用者設定檔的電腦。 然後確認重新導向設定檔。

以下是測試漫遊使用者設定檔的方法：

1. 使用您已經啟用漫遊使用者設定檔的使用者帳戶，登入主要電腦 (如果您已啟用主要電腦支援)。 如果您在特定電腦上啟用漫遊使用者設定檔，請登入其中一部電腦。
2. 如果使用者先前已登入電腦，請開啟提升權限的命令提示字元，然後輸入下列命令，以確保最新的群組原則設定套用到用戶端電腦：
    
    ```PowerShell
    GpUpdate /Force
    ```
3. 若要確認使用者設定檔是否為漫遊，請開啟 [**控制台**]，依序選取 [**系統及安全性**]、[**系統**]、[**高級系統設定**]、[使用者設定檔] 區段中的 [**設定**]，然後尋找[**類型**] 資料行中的漫遊。

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>附錄 A：部署漫遊使用者設定檔的檢查清單

| 狀態                     | Action                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1.準備網域<br>-將電腦加入網域<br>-啟用個別設定檔版本的使用<br>-建立使用者帳戶<br>-（選用）部署資料夾重新導向 |
| ☐<br><br><br>             | 2.建立漫遊使用者設定檔的安全性群組<br>-組名：<br>屬於 |
| ☐<br><br>                 | 3.建立漫遊使用者設定檔的檔案共用<br>-檔案共用名稱： |
| ☐<br><br>                 | 4.建立漫遊使用者設定檔的 GPO<br>-GPO 名稱：|
| ☐                         | 5.設定漫遊使用者設定檔原則設定    |
| ☐<br>☐<br>☐              | 6.啟用漫遊使用者設定檔<br>-在使用者帳戶的 AD DS 中啟用嗎？<br>-在電腦帳戶的群組原則中啟用嗎？<br> |
| ☐                         | 7.選擇性指定 Windows 10 電腦的強制啟動版面配置 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8.選擇性啟用主要電腦支援<br>-指定使用者的主要電腦<br>-使用者和主要電腦對應的位置：<br>-（選擇性）啟用資料夾重新導向的主要電腦支援<br>-以電腦為基礎或以使用者為基礎的？<br>-（選擇性）啟用漫遊使用者設定檔的主要電腦支援 |
| ☐                        | 9.啟用漫遊使用者設定檔 GPO                |
| ☐                        | 10.測試漫遊使用者設定檔                         |

## <a name="appendix-b-profile-version-reference-information"></a>附錄 B：設定檔版本參考資訊

每個設定檔都有一個設定檔版本，大致上會對應到使用設定檔的 Windows 版本。 例如，Windows 10 版本1703和1607版都使用。V6 設定檔版本。 只有在需要維護相容性時，Microsoft 才會建立新的設定檔版本，這也是為什麼並非每個 Windows 版本都包含新的設定檔版本。

下表列出各種版本的 Windows 上漫遊使用者設定檔的位置。

| 作業系統版本 | 漫遊使用者設定檔位置 |
| --- | --- |
| Windows XP 與 Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista 和 Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 和 Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 與 Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3```（套用軟體更新和登錄機碼之後）<br>```\\<servername>\<fileshare>\<username>.V2```（在套用軟體更新和登錄機碼之前） |
| Windows 8.1 和 Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4```（套用軟體更新和登錄機碼之後）<br>```\\<servername>\<fileshare>\<username>.V2```（在套用軟體更新和登錄機碼之前） |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10 版本1703和版本1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>附錄 C：在升級後解決重設開始功能表的版面配置

以下是在就地升級之後，重設的 [開始] 功能表配置的一些解決方法：

- 如果只有一位使用者使用裝置，而 IT 系統管理員使用受控 OS 部署策略（例如 SCCM），則可以執行下列動作：
    
  1. 在升級之前，使用 Startlayout 匯出 [開始] 功能表配置 
  2. 在 OOBE 之後但在使用者登入之前，使用 StartLayout 匯入 [開始] 功能表配置  
 
     > [!NOTE] 
     > 匯入 StartLayout 會修改預設的使用者設定檔。 在匯入之後建立的所有使用者設定檔都會取得匯入的開始配置。
 
- IT 系統管理員可以選擇使用群組原則來管理開始的版面配置。 使用群組原則可提供集中式管理解決方案，將標準化的開始配置套用至使用者。 使用 [開始管理] 群組原則的模式有2種模式。 完整鎖定和部分鎖定。 完整的鎖定案例可防止使用者對開始的版面配置進行任何變更。 部分鎖定案例可讓使用者對啟動的特定區域進行變更。 如需詳細資訊，請參閱[自訂和匯出開始](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)配置。
        
   > [!NOTE]
   > 在部分鎖定案例中，使用者進行的變更仍會在升級期間遺失。

- 讓開始版面配置重設發生，並允許使用者重新設定 Start。 您可以將通知電子郵件或其他通知傳送給終端使用者，以預期其開始版面配置會在 OS 升級後重設，以將影響降至最低。 

# <a name="change-history"></a>變更歷程記錄

下表摘要說明本主題中一些最重要的變更。

| Date | 描述 |`Reason`|
| --- | ---         | ---   |
| 5月1日，2019 | 已新增 Windows Server 2019 的更新 |
| 2018年4月10日 | 新增在 OS 就地升級之後，何時會失去開始進行使用者自訂的討論|標注的已知問題。 |
| 2018年3月13日 | 已針對 Windows Server 2016 更新 | 已移出舊版的程式庫，並已更新最新版本的 Windows Server。 |
| 2017年4月13日 | 新增 Windows 10 1703 版的設定檔資訊，並闡明漫遊設定檔版本在升級作業系統時的運作方式—請參閱在[多個版本的 Windows 上使用漫遊使用者設定檔時的考慮](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)。 | 客戶意見反應。 |
| 2017年3月14日 | 已新增在附錄 a 中[為 Windows 10 電腦指定強制啟動版面配置的選擇性步驟：部署漫遊使用者設定檔](#appendix-a-checklist-for-deploying-roaming-user-profiles)的檢查清單。 |最新 Windows update 中的功能變更。 |
| 2017年1月23日 | 已將步驟新增[至步驟4：選擇性地建立漫遊使用者設定檔](#step-4-optionally-create-a-gpo-for-roaming-user-profiles)的 GPO，將讀取權限委派給已驗證的使用者，因為這是群組原則的安全性更新，所以現在是必要的。|群組原則處理的安全性變更。 |
| 2016年12月29日 | 已在步驟 8 [中新增連結：啟用漫遊使用者設定檔 GPO](#step-8-enable-the-roaming-user-profiles-gpo) ，讓您更輕鬆地取得如何設定主要電腦群組原則的相關資訊。 也修正了步驟5和6的幾個參考，其中包含錯誤的數位。|客戶意見反應。 |
| 2016年12月5日 | 已新增說明 [開始] 功能表設定漫遊問題的資訊。 | 客戶意見反應。 |
| 2016年7月6日 | 已在附錄 B 中[新增 Windows 10 設定檔版本尾碼：設定檔版本參考](#appendix-b-profile-version-reference-information)資訊。 同時從支援的作業系統清單中移除 Windows XP 和 Windows Server 2003。 | 新版本 Windows 的更新，並移除不再支援之 Windows 版本的相關資訊。 |
| 2015 年 7 月 7日 | 使用叢集的檔案伺服器時新增需求和步驟以停用持續可用性。 | 當停用持續可用性時，叢集的檔案共用對於小型寫入 (一般用於漫遊使用者設定檔) 擁有較佳的效能。 |
| 2014 年 3 月 19 日 | 大寫的設定檔版本尾碼（。V2、。V3、。V4）， [位於附錄 B：設定檔版本參考](#appendix-b-profile-version-reference-information)資訊。 | 雖然 Windows 不區分大小寫，但如果您將 NFS 與檔案共用搭配使用，則設定檔尾碼一定要有正確的大小寫（大寫）。 |
| 2013 年 10 月 9 日 | 已針對 windows Server 2012 R2 和 Windows 8.1 修改過一些事項，並已新增[在[多個 Windows 版本和附錄 B 上使用漫遊使用者設定檔時的考慮事項](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)：設定檔版本參考](#appendix-b-profile-version-reference-information)資訊區段。 | 新版本的更新;客戶意見反應。 |

## <a name="more-information"></a>詳細資訊

- [部署資料夾重新導向、離線檔案和漫遊使用者設定檔](deploy-folder-redirection.md)
- [部署資料夾重新導向和漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)
- [執行使用者狀態管理](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Microsoft 針對複寫的使用者設定檔資料所提供的支援聲明](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [使用 DISM 側載應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [針對以 Windows 執行階段為基礎的應用程式進行封裝、部署和查詢的疑難排解](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)