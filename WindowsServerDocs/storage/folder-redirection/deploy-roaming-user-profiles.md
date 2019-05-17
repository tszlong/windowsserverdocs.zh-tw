---
Title: 部署漫遊使用者設定檔
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 07/09/2018
ms.author: jgerend
ms.openlocfilehash: b977af31663b675a56c65e06a2a0d60b1d2ad811
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857139"
---
# <a name="deploying-roaming-user-profiles"></a>部署漫遊使用者設定檔

>適用於：Windows 10，Windows 8.1，Windows 8、 Windows 7、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

本主題描述如何使用 Windows Server 部署[漫遊使用者設定檔](folder-redirection-rup-overview.md)Windows 用戶端電腦。 漫遊使用者設定檔會重新導向使用者設定檔的檔案共用，讓使用者接收相同的作業系統和應用程式設定多部電腦上。

如需本主題的最新變更的清單，請參閱 <<c0> [ 修訂歷程記錄](#change-history)本主題一節。

>[!IMPORTANT]
>因為所做的安全性變更[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)，我們已更新[步驟 4:選擇性建立漫遊使用者設定檔的 GPO](#step-4:-optionally-create-a-gpo-for-roaming-user-profiles)本主題，讓該 Windows 可以正確地套用漫遊使用者設定檔原則 （並還原為受影響的電腦上的本機原則）。

> [!IMPORTANT]
>  在下列組態中的作業系統就地升級之後，會遺失使用者開始自訂：
> - 使用者設定漫遊設定檔
> - 允許使用者變更開始
>
> 如此一來，[開始] 功能表會重設為新的作業系統版本的預設值之後的作業系統就地升級。 因應措施，請參閱[附錄 c:工作大約之後，重設 [開始] 功能表配置升級](#appendix-c-workaround)。

## <a name="prerequisites"></a>先決條件

### <a name="hardware-requirements"></a>硬體需求

漫遊使用者設定檔需要 x64 或 x86 型電腦;它不支援 Windows rt

### <a name="software-requirements"></a>軟體需求

漫遊使用者設定檔具有下列軟體需求：

- 如果您要在已經有本機使用者設定檔的環境中部署漫遊使用者設定檔與資料夾重新導向，請先部署資料夾重新導向再部署漫遊設定檔，以減少漫遊設定檔的大小。 現有的使用者資料夾成功重新導向之後，您就可以部署漫遊使用者設定檔。
- 若要管理漫遊使用者設定檔，您必須以 Domain Administrators 安全性群組、Enterprise Administrators 安全性群組或 Group Policy Creator Owners 安全性群組的成員身分登入。
- 用戶端電腦必須執行 Windows 10，Windows 8.1，Windows 8、 Windows 7、 Windows Vista、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008。
- 用戶端電腦必須加入您所管理的 Active Directory 網域服務 (AD DS)。
- 必須有一部電腦安裝群組原則管理與 Active Directory 系統管理中心。
- 必須有一部檔案伺服器裝載漫遊使用者設定檔。

    - 如果檔案共用使用 DFS 命名空間，DFS 資料夾 (連結) 必須有單一的目標，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用使用 DFS 複寫與與另一部伺服器複寫內容，使用者必須只能存取來源伺服器，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用已叢集，停用檔案共用的持續可用性以避免效能問題。
- 若要使用漫遊使用者設定檔中的主要電腦支援，還有其他用戶端電腦和 Active Directory 結構描述的需求。 如需詳細資訊，請參閱 <<c0> [ 部署資料夾重新導向及漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)。
- 如果它們使用一個以上的電腦、 遠端桌面工作階段主機或虛擬桌面基礎結構 (VDI) 伺服器，將不會漫遊 Windows 10 或 Windows Server 2016 的功能表的使用者入門的版面配置。 因應措施，您可以指定開始配置，如本主題中所述。 或者您可以使用的使用者設定檔磁碟，正確漫遊的遠端桌面工作階段主機伺服器或 VDI 伺服器搭配使用時，開始功能表設定。 如需詳細資訊，請參閱 <<c0> [ 方便使用者資料管理與 Windows Server 2012 中的使用者設定檔磁碟](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/)。

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>在多個版本的 Windows 上使用漫遊使用者設定檔時的考量

如果您決定在多個版本的 Windows 上使用漫遊使用者設定檔，我們建議您採取下列動作：

- 設定 Windows 為每個作業系統版本維護不同的設定檔版本。 這有助於防止不必要和無法預期的問題，例如設定檔損毀。
- 使用資料夾重新導向來儲存使用者檔案，例如使用者設定檔以外的文件和圖片。 這可讓使用者跨多個作業系統版本使用相同的檔案。 也可以維持小型的設定檔，並且快速登入。
- 為漫遊使用者設定檔配置足夠的儲存空間。 如果您支援兩種作業系統版本，設定檔的數目 (以及總空間) 也會加倍，因為要為每個作業系統版本維護不同的設定檔。
- 不在執行 Windows Vista/Windows Server 2008 和 Windows 7/Windows Server 2008 R2 的電腦之間使用漫遊使用者設定檔。 因不相容性設定檔版本中而不支援這些作業系統版本之間漫遊。
- 請通知您的使用者，在某個作業系統版本進行的變更，將不會漫遊到另一個作業系統版本。
- 當您將環境移至使用不同的設定檔版本的 Windows 版本 (例如從 Windows 10 版本 1607年的 Windows 10，請參閱[附錄 b:設定檔版本參考資訊](#appendix-b:-profile-version-reference-information)清單)，使用者會收到新的空白漫遊使用者設定檔。 您可以透過將常見的資料夾重新導向的資料夾重新導向取得新的設定檔的影響降到最低。 沒有受支援的移轉漫遊使用者設定檔從一個設定檔版本到另一個方法。

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>步驟 1：使用不同的設定檔版本

如果您要在執行 Windows 8.1，Windows 8、 Windows Server 2012 R2 或 Windows Server 2012 的電腦上部署漫遊使用者設定檔，我們建議您在部署之前的 Windows 環境進行一些變更。 這些變更可協助確保未來的作業系統升級順暢進行，並提昇同時執行多個版本的 Windows 與漫遊使用者設定檔的能力。

若要進行這些變更，請使用下列程序。

1. 在所有您要使用漫遊、強制、超級強制或網域預設設定檔的電腦下載並安裝適當的軟體更新：

    - Windows 8.1 或 Windows Server 2012 R2： 安裝文章所述的軟體更新[2887595](http://support.microsoft.com/kb/2887595) Microsoft 知識庫中 （發行時）。
    - Windows 8 或 Windows Server 2012：安裝 Microsoft 知識庫中 [2887239](http://support.microsoft.com/kb/2887239) 文章所述的軟體更新。
2. 所有在電腦上執行 Windows 8.1，Windows 8、 Windows Server 2012 R2 或 Windows Server 2012 且您要使用漫遊使用者設定檔，使用 登錄編輯程式或群組原則來建立下列登錄機碼 DWORD 值，並將它設定為`1`。 如需使用群組原則建立登錄機碼的相關資訊，請參閱 [設定登錄項目](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

    ```PowerShell
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    >[!WARNING]
    >不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。
3. 重新啟動電腦。

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>步驟 2：建立漫遊使用者設定檔安全性群組

如果您的環境尚未設定漫遊使用者設定檔，第一個步驟是建立安全性群組，其中包含您要套用漫遊使用者設定檔原則設定的所有使用者和/或電腦。

- 一般用途漫遊使用者設定檔部署的系統管理員通常會建立使用者的安全性群組。
- 遠端桌面服務或虛擬桌面部署的系統管理員通常會為使用者與共用的電腦使用安全性群組。

以下是如何漫遊使用者設定檔建立的安全性群組：

1. 已安裝的電腦上開啟的伺服器管理員，Active Directory 系統管理中心。
2. 在 **工具**功能表上，選取**Active Directory 系統管理中心**。 [Active Directory 系統管理中心] 隨即顯示。
3. 以滑鼠右鍵按一下適當的網域或 OU 中，選取**的新**，然後選取**群組**。
4. 在 [建立群組] 視窗內的 [群組] 區段中，指定下列設定：

    - 在 [群組名稱] 中，輸入安全性群組的名稱，例如：**漫遊使用者設定檔使用者和電腦**。
    - 在 **群組領域**，選取**安全性**，然後選取**Global**。
5. 在 **成員**區段中，選取**新增**。 [選取使用者、連絡人、電腦、服務帳戶或群組] 對話方塊隨即顯示。
6. 如果您想要在安全性群組包含電腦帳戶，請選取**物件類型**，選取**電腦**核取方塊，然後選取**確定**。
7. 輸入的使用者、 群組和/或您想要部署漫遊使用者設定檔，請選取的電腦名稱 **[確定]**，然後選取**確定**一次。

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>步驟 3：建立漫遊使用者設定檔的檔案共用

如果您還沒有個別的檔案共用漫遊使用者設定檔 （獨立於任何重新導向的資料夾，以避免不小心快取漫遊設定檔資料夾的共用），會使用下列程序，在執行 Windows 的伺服器上建立檔案共用伺服器。

>[!NOTE]
>某些功能可能會不同，或無法根據您使用的 Windows Server 的版本。

以下是如何在 Windows Server 上建立檔案共用：

1. 在 [伺服器管理員瀏覽] 窗格中，選取**檔案和存放服務**，然後選取**共用**以顯示 [共用] 頁面。
2. 在 [共用] 磚中，選取**任務**，然後選取**新的共用**。 「新增共用精靈」隨即顯示。
3. 在 **選取設定檔**頁面上，選取**SMB 共用-快速**。 如果您已安裝的檔案伺服器資源管理員，並使用資料夾管理屬性，改為選取**SMB 共用-進階**。
4. 在 [共用位置] 頁面上，選取您要在上面建立共用的伺服器和磁碟區。
5. 在 [共用名稱]  頁面的 [共用名稱] 方塊中輸入共用的名稱 (例如， **使用者設定檔$** )。

    >[!TIP]
    >建立共用時，在共用名稱後面放一個 ```$``` 可隱藏共用。 這會隱藏共用，不會隨便被瀏覽。
6. 在 [其他設定] 頁面上，清除 [啟用持續可用性] 核取方塊 (如果存在)，選擇性地選取 [啟用存取型列舉] 和 [加密資料存取] 核取方塊。
7. 在 **權限**頁面上，選取**自訂權限...**. [進階安全性設定] 對話方塊隨即出現。
8. 選取 **停用繼承**，然後選取**繼承的轉換成此物件的明確權限的權限**。
9. 中所述設定權限[必要的權限的檔案共用裝載漫遊使用者設定檔](#required-permissions-for-the-file-share-hosting-roaming-user-profiles)且顯示下列螢幕擷取畫面中，移除未列出的群組和帳戶權限，並加入特殊您在步驟 1 中建立的漫遊使用者設定檔使用者和電腦群組的權限。
    
    ![進階安全性設定 視窗顯示在 表 1 中所述的權限](media\advanced-security-user-profiles.jpg)
    
    **圖 1** 設定漫遊使用者設定檔共用的權限
10. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [管理屬性]  頁面上，選取 [使用者檔案]  資料夾使用方式值。
11. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [配額]  頁面上，選擇性地選取套用至共用之使用者的配額。
12. 在 **確認**頁面上，選取**建立。**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>檔案共用裝載漫遊使用者設定檔的必要權限

<table>
<tbody>
<tr class="odd">
<td>使用者帳戶</td>
<td>存取權</td>
<td>適用於</td>
</tr>
<tr class="even">
<td>系統</td>
<td>完全控制</td>
<td>這個資料夾、子資料夾及檔案</td>
</tr>
<tr class="odd">
<td>Administrators</td>
<td>完全控制</td>
<td>只有這個資料夾</td>
</tr>
<tr class="even">
<td>建立者/擁有者</td>
<td>完全控制</td>
<td>子資料夾及檔案</td>
</tr>
<tr class="odd">
<td>需要將資料放在共用上的使用者安全性群組 (漫遊使用者設定檔使用者和電腦)</td>
<td>列出資料夾 / 讀取資料<sup>1</sup><br />
<br />
建立資料夾 / 附加資料<sup>1</sup></td>
<td>只有這個資料夾</td>
</tr>
<tr class="even">
<td>其他群組與帳戶</td>
<td>無 (移除)</td>
<td></td>
</tr>
</tbody>
</table>

1 進階權限

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>步驟 4：選擇性建立漫遊使用者設定檔的 GPO

如果您還沒有針對漫遊使用者設定檔設定建立 GPO，請使用下列程序建立空的 GPO 以用於漫遊使用者設定檔。 這個 GPO 可以讓您設定漫遊使用者設定檔設定 (例如主要電腦支援，將分別討論)，也可用來啟用電腦上的漫遊使用者設定檔，這通常會在部署虛擬桌面環境時或使用遠端桌面服務進行。

以下是如何漫遊使用者設定檔建立的 GPO:

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從**工具**功能表中，選取**群組原則管理**。 [群組原則管理] 隨即會出現。
3. 以滑鼠右鍵按一下網域或 OU，您要設定漫遊使用者設定檔，然後選取**在這個網域中建立 GPO 並連結到**。
4. 在**新的 GPO**  對話方塊中，輸入 gpo 的名稱 (例如**漫遊使用者設定檔設定**)，然後選取**確定**。
5. 以滑鼠右鍵按一下新建立的 GPO，然後清除 [啟用連結]  核取方塊。 這可以防止在您完成設定之前就套用 GPO。
6. 選取 GPO。 在**安全性篩選**一節**範圍**索引標籤上，選取**Authenticated Users**，然後選取**移除**以防止 GPO套用到每個人。
7. 在 **安全性篩選**區段中，選取**新增**。
8. 在**選取使用者、 電腦或群組**] 對話方塊中，輸入您在步驟 1 中建立的安全性群組名稱群組 (例如**漫遊使用者設定檔使用者和電腦**)，然後選取 [ **[確定]**.
9. 選取**委派**索引標籤上，選取**新增**，型別**Authenticated Users**，選取**確定**，然後選取  **確定**一次接受預設值的讀取權限。
    
    這是步驟中所做的安全性變更，因此需要[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)。

>[!IMPORTANT]
>因為所做的安全性變更[MS16 072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)，現在您必須提供已驗證的使用者委派的群組的讀取權限給 GPO-否則 GPO 將不會套用到使用者，或如果已套用，則會移除 GPO，將使用者設定檔重新導向回到本機電腦。 如需詳細資訊，請參閱 <<c0> [ 部署群組原則安全性更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>步驟 5：選擇性設定使用者帳戶的漫遊使用者設定檔

如果您要對使用者帳戶部署漫遊使用者設定檔，請使用下列程序在 Active Directory 網域服務中指定使用者帳戶的漫遊使用者設定檔。 如果您要對電腦，部署漫遊使用者設定檔，通常遠端桌面服務或虛擬桌面部署，請改為使用中記載的程序[步驟 6:（選擇性） 在電腦上設定漫遊使用者設定檔](#step-6:-optionally-set-up-roaming-user-profiles-on-computers)。

>[!NOTE]
>如果您分別使用 Active Directory 對使用者帳戶，以及使用群組原則對電腦設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

以下是如何設定 使用者帳戶中的 漫遊使用者設定檔：

1. 在 Active Directory 系統管理中心，瀏覽到適當網域中的 [使用者] 容器 (或 OU)。
2. 選取您想要指派漫遊使用者設定檔，以滑鼠右鍵按一下使用者，然後選取所有使用者**屬性**。
3. 在 **設定檔**區段中，選取**設定檔路徑：** 核取方塊，然後輸入路徑的檔案共用您想要用來儲存使用者的漫遊使用者設定檔，後面接著`%username%`（即自動取代使用者第一次使用者登入名稱）。 例如: 
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    若要指定強制漫遊使用者設定檔，指定您先前建立，例如的 NTuser.man 檔案的路徑`fs1.corp.contoso.comUser Profiles$default`。 如需詳細資訊，請參閱 <<c0> [ 建立強制使用者設定檔](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
4. 選取 [確定]。

>[!NOTE]
>根據預設，使用漫遊使用者設定檔時，允許部署所有 Windows® 執行階段型 (Windows 市集) 應用程式。 不過，當使用特殊設定檔時，預設不會部署應用程式。 特殊設定檔是使用者登出後捨棄變更的使用者設定檔：
><br><br>若要移除特殊設定檔的應用程式部署限制，請啟用 [允許特殊設定檔的部署作業] (位於 [電腦設定\原則\系統管理範本\Windows 元件\應用程式套件部署])。 不過，在此案例中部署的應用程式會保留部分的資料儲存在電腦上，假如一部電腦有數百位使用者時，資料可能會不斷累積。 若要清除應用程式，找出或開發使用的工具[CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx)清除不再需要在電腦的 設定檔的使用者的應用程式套件的 API。
><br><br>如需 Windows 市集應用程式的額外背景資訊，請參閱[管理 Windows 市集的用戶端存取](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>)。

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>步驟 6：選擇性設定電腦的漫遊使用者設定檔

如果您要對電腦部署漫遊使用者設定檔 (通常是為遠端桌面服務或虛擬桌面部署時進行)，請使用下列程序。 如果您要部署漫遊使用者設定檔給使用者帳戶，改為使用中所述的程序[步驟 5:選擇性地設定使用者帳戶上的 漫遊使用者設定檔](#step-5:-optionally-set-up-roaming-user-profiles-on-user-accounts)。

您可以使用群組原則將漫遊使用者設定檔套用至執行 Windows 8.1，Windows 8、 Windows 7、 Windows Vista、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的電腦。

>[!NOTE]
>如果您分別使用群組原則對電腦，以及使用 Active Directory 對使用者帳戶設定漫遊使用者設定檔，以電腦為基礎的原則設定的優先順序較高。

以下是如何在電腦上設定漫遊使用者設定檔：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從**工具**功能表上，選取**群組原則管理**。 會出現 群組原則管理。
3. 在 群組原則管理，以滑鼠右鍵按一下您在步驟 3 中建立的 GPO (例如**漫遊使用者設定檔設定**)，然後選取**編輯**。
4. 在 [群組原則管理編輯器] 視窗中，依序瀏覽至 [電腦設定]、[原則]、[系統管理範本]、[系統]，然後是 [使用者設定檔]。
5. 以滑鼠右鍵按一下**設定漫遊設定檔路徑，登入此電腦的所有使用者**，然後選取**編輯**。
    > [!TIP]
    > 使用者的主目錄資料夾 (如果有設定) 是由某些程式 (例如 Windows PowerShell) 使用的預設資料夾。 您可以使用 AD DS 中使用者帳戶內容的 [主目錄資料夾]  區段，為每一位使用者設定替代或網路位置。 若要設定所有使用者的主資料夾位置的虛擬桌面環境中執行 Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的電腦，啟用**設定使用者主目錄資料夾**原則設定，然後指定要對應 （或指定本機資料夾） 的檔案共用及磁碟機代號。 不要使用環境變數或省略符號。 在使用者登入期間，使用者別名會附加到指定的路徑結尾。
6. 在 **屬性**對話方塊中，選取**已啟用**
7. 在 **登入此電腦的使用者應該使用此漫遊設定檔路徑**方塊中，輸入您想要用來儲存使用者的漫遊使用者設定檔，後面加上檔案共用路徑`%username%`（這會自動取代使用使用者名稱的第一次使用者登入）。 例如: 

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    若要指定強制漫遊使用者設定檔，也就是預先設定的設定檔的使用者無法進行永久變更 （當使用者登出時，變更會重設），指定您先前建立的 NTuser.man 檔案的路徑， `\\fs1.corp.contoso.com\User Profiles$\default`。 如需詳細資訊，請參閱 [建立強制使用者設定檔](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
8. 選取 [確定]。

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>步驟 7：選擇性地指定為 Windows 10 電腦的 [開始] 配置

您可以使用群組原則來套用特定的 [開始] 功能表配置，讓使用者看到相同的 [開始] 配置，在所有電腦上。 如果使用者的登入到一個以上的電腦和您想要不同的電腦有一致的 [開始] 配置，請確定 GPO 套用到所有他們的電腦。

若要指定 [開始] 配置，執行下列作業：

1. 更新您的 Windows 10 電腦為 Windows 10 版本 1607 （也稱為年度更新版） 或更新版本，並安裝 2017 年 3 月 14 日累積更新 ([KB4013429](https://support.microsoft.com/kb/4013429)) 或更新版本。
2. 建立完整或部分開始功能表配置的 XML 檔案。 若要這樣做，請參閱[自訂與匯出 [開始] 配置](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)。
    * 如果您指定*完整*開始 配置中，使用者無法自訂 開始 功能表的任何部分。 如果您指定*部分*開始 配置中，使用者可以自訂的圖格，您指定的鎖定群組以外的所有內容。 不過，部分的 [開始] 配置中，使用 [開始] 功能表的使用者自訂設定不會漫遊至其他電腦。
3. 您可以使用群組原則，將自訂的 [開始] 配置套用至您建立漫遊使用者設定檔的 GPO。 若要這樣做，請參閱[使用群組原則來套用自訂的 [開始] 配置，在網域中](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment)。
4. 您可以使用 群組原則來設定您的 Windows 10 電腦上的下列登錄值。 若要這樣做，請參閱[設定登錄項目](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

    <table>
    <thead>
    <tr class="header">
    <th>動作</th>
    <th>Update</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Hive</td>
    <td><strong>HKEY_LOCAL_MACHINE</strong></td>
    </tr>
    <tr class="even">
    <td>機碼路徑</td>
    <td><strong>Software\Microsoft\Windows\CurrentVersion\Explorer</strong></td>
    </tr>
    <tr class="odd">
    <td>值名稱</td>
    <td><strong>SpecialRoamingOverrideAllowed</strong></td>
    </tr>
    <tr class="even">
    <td>值類型</td>
    <td><strong>REG_DWORD</strong></td>
    </tr>
    <tr class="odd">
    <td>[數值資料]</td>
    <td><strong>1</strong> (或<strong>0</strong>停用)</td>
    </tr>
    <tr class="even">
    <td>Base</td>
    <td><strong>十進位</strong></td>
    </tr>
    </tbody>
    </table>
5. （選擇性）啟用第一次登入最佳化，讓使用者更快速登入。 若要這樣做，請參閱[套用原則以改善登入時間](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time)。
6. （選擇性）從您用來部署用戶端電腦的 Windows 10 基底映像移除不需要應用程式，進一步減少登入時間。 沒有任何預先佈建的應用程式，Windows Server 2016，因此您可以在伺服器映像上略過此步驟。
-若要移除應用程式，請使用[移除 AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)中解除安裝下列應用程式的 Windows PowerShell cmdlet。 如果已部署您的電腦可以編寫指令碼使用這些應用程式移除[移除 Add-appxpackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps)。
    
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
>解除安裝這些應用程式會降低登入時間，但您可以將它們安裝，如果您的部署需要其中任何保留。

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>步驟 8：啟用漫遊使用者設定檔 GPO

如果您使用群組原則對電腦設定漫遊使用者設定檔，或如果您使用群組原則自訂其他漫遊使用者設定檔設定，下一步是要啟用 GPO，允許它套用至受影響的使用者。

>[!TIP]
>如果您計劃實作主要電腦支援，這樣做，才能啟用 GPO。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 如需特定原則設定中，請參閱[部署資料夾重新導向及漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)。

以下是如何啟用漫遊使用者設定檔 GPO:

1. 開啟 [群組原則管理]。
2. 以滑鼠右鍵按一下您所建立的 GPO，然後選取**啟用連結**。 功能表項目旁會顯示核取方塊。

## <a name="step-9-test-roaming-user-profiles"></a>步驟 9：測試漫遊使用者設定檔

若要測試漫遊使用者設定檔，請使用設定漫遊使用者設定檔的使用者帳戶登入電腦，或登入設定漫遊使用者設定檔的電腦。 然後確認重新導向設定檔。

以下是如何測試漫遊使用者設定檔：

1. 使用您已經啟用漫遊使用者設定檔的使用者帳戶，登入主要電腦 (如果您已啟用主要電腦支援)。 如果您在特定電腦上啟用漫遊使用者設定檔，請登入其中一部電腦。
2. 如果使用者先前已登入電腦，請開啟提升權限的命令提示字元，然後輸入下列命令，以確保最新的群組原則設定套用到用戶端電腦：
    
    ```PowerShell
    GpUpdate /Force
    ```
3. 若要確認，漫遊使用者設定檔，請開啟**控制台中**，選取**系統及安全性**，選取**System**，選取**進階系統設定**，選取**設定**使用者設定檔 區段，然後尋找**漫遊**中**型別**資料行。

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>附錄 A：部署漫遊使用者設定檔的檢查清單

|狀態|動作|
|:---:|---|
|☐<br>☐<br>☐<br>☐<br>☐|1.準備網域<br>-將電腦加入網域<br>-啟用使用不同的設定檔版本<br>建立使用者帳戶<br>-（選擇性） 部署資料夾重新導向|
|☐<br><br><br>|2.建立漫遊使用者設定檔的安全性群組<br>群組名稱：<br>成員：|
|☐<br><br>|3.建立漫遊使用者設定檔的檔案共用<br>檔案共用名稱：|
|☐<br><br>|4.建立漫遊使用者設定檔的 GPO<br>-GPO 名稱：|
|☐|5.設定漫遊使用者設定檔原則設定|
|☐<br>☐<br>☐|6.啟用漫遊使用者設定檔<br>的啟用使用者帳戶上的 AD DS 中？<br>-已啟用群組原則中的電腦帳戶？<br>|
|☐|7.（選擇性）指定必要的 [開始] 配置為 Windows 10 電腦|
|☐<br>☐<br><br>☐<br><br>☐|8.（選擇性）啟用主要電腦支援<br>-指定使用者的主要電腦<br>使用者和主要電腦對應的位置：<br>-（選擇性） 啟用資料夾重新導向主要電腦支援<br>-電腦為基礎或以使用者為基礎？<br>-（選擇性） 啟用漫遊使用者設定檔的主要電腦支援|
|☐|9.啟用漫遊使用者設定檔 GPO|
|☐|10.測試漫遊使用者設定檔|

## <a name="appendix-b-profile-version-reference-information"></a>附錄 B：設定檔版本參考資訊

每個設定檔有大約對應版本的 Windows 使用的設定檔的設定檔版本。 比方說，Windows 10 1703年版，兩者都使用的 1607年版。V6 設定檔版本。 Microsoft 會建立新的設定檔版本只在需要維持相容性，也就是每個 Windows 版本為何不包含新的設定檔版本時。

下表列出各種版本的 Windows 上漫遊使用者設定檔的位置。

|作業系統版本|漫遊使用者設定檔位置|
|---|---|
|Windows XP 與 Windows Server 2003|```\\<servername>\<fileshare>\<username>```|
|Windows Vista 和 Windows Server 2008|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 7 和 Windows Server 2008 R2|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 8 與 Windows Server 2012|```\\<servername>\<fileshare>\<username>.V3``` （軟體更新和登錄機碼套用之後）<br>```\\<servername>\<fileshare>\<username>.V2``` （之前的軟體更新和登錄機碼會套用）|
|Windows 8.1 和 Windows Server 2012 R2|```\\<servername>\<fileshare>\<username>.V4``` （軟體更新和登錄機碼套用之後）<br>```\\<servername>\<fileshare>\<username>.V2``` （之前的軟體更新和登錄機碼會套用）|
|Windows 10|```\\<servername>\<fileshare>\<username>.V5```|
|Windows 10 1703年版，版本 1607|```\\<servername>\<fileshare>\<username>.V6```|

## <a id="appendix-c-workaround"></a>附錄 c:工作大約之後，重設 [開始] 功能表配置升級

以下是一些方法可以解決取得就地升級之後，重設的開始功能表配置：

 - 如果只有一位使用者曾使用的裝置，且 IT 系統管理員會使用受管理的 OS 部署安裝策略，例如 SCCM 他們可以執行下列作業：
    
    1. 匯出與匯出 Startlayout 開始 功能表配置，在升級之前 
    2. OOBE 之後但在使用者登入之前，請匯入與匯入 StartLayout 的 [開始] 功能表配置  
 
    > [!NOTE] 
    > 匯入 StartLayout 修改預設的使用者設定檔。 發生匯入之後所建立的所有使用者設定檔會都收到匯入的開始配置。
 
 - IT 系統管理員可以選擇管理群組原則的開始的配置。 使用群組原則提供集中式的管理解決方案，以標準化的啟動配置套用至使用者。 有 2 種模式可以使用群組原則，開始管理模式。 完整的鎖定，而且部分的鎖定。 完整的鎖定案例可防止使用者變更開始的配置。 部分鎖定案例可讓使用者啟動的特定區域中進行變更。 如需詳細資訊，請參閱 <<c0> [ 自訂與匯出 [開始] 配置](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)。
        
    > [!NOTE]
    > 在部分鎖定的案例中的使用者所做變更仍會遺失在升級期間。

 -  可讓的開始版面配置重設發生，並允許使用者重新啟動設定。 使用者可以傳送 「 通知電子郵件或其他通知 」，預期要在作業系統升級為最小化的影響之後重設其 [開始] 配置。 

# <a name="change-history"></a>變更歷程記錄

下表摘要說明本主題中一些最重要的變更。

|Date|描述 |原因|
|--- |---         |---   |
|2018 年 4 月 10 日，|已新增的使用者開始自訂時遺失的作業系統就地升級之後的討論|已知問題的註標。|
|2018 年 3 月 13日日 |適用於 Windows Server 2016 更新 | 移出舊版程式庫，針對目前版本的 Windows Server 更新。|
|2017 年 4 月 13日日|加入 Windows 10 版本 1703年的設定檔資訊，並釐清如何漫遊設定檔版本工作，升級作業系統時，請參閱[多個版本的 Windows 上使用漫遊使用者設定檔時的考量](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)。|客戶的意見反應。|
|2017 年 3 月 14日日|已新增選擇性的步驟，來指定必要的 [開始] 配置中的 Windows 10 電腦的[附錄 a:部署漫遊使用者設定檔的檢查清單](#appendix-a:-checklist-for-displaying-roaming-user-profiles)。|最新的 Windows 更新的功能變更。|
|2017 年 1 月 23日日|新增步驟[步驟 4:選擇性建立漫遊使用者設定檔的 GPO](#step-4:-optionally-create-a-gpo-for-roaming-user-profiles)委派已驗證的使用者，現在的 「 讀取 」 權限需要因為群組原則安全性更新。|安全性變更群組原則處理。|
|2016 年 12 月 29日日|新增連結，以在[步驟 7:啟用漫遊使用者設定檔 GPO](#step-7:-enable-the-roaming-user-profiles-gpo)方便您取得有關如何將群組原則設定的主要電腦的資訊。 同時已修正幾個步驟 5 和 6，有數字錯誤參考。|客戶的意見反應。|
|2016 年 12 月 5 日，|已新增說明漫遊問題開始功能表設定的資訊。|客戶的意見反應。|
|2016 年 7 月 6日日|加入 Windows 10 設定檔版本尾碼中的[附錄 b:設定檔版本參考資訊](#appendix-b:-profile-version-reference-information)。 也從支援的作業系統清單中移除 Windows XP 和 Windows Server 2003。|新版本的 Windows，並移除的資訊的不受支援的 Windows 版本的更新。|
|2015 年 7 月 7日|使用叢集的檔案伺服器時新增需求和步驟以停用持續可用性。|當停用持續可用性時，叢集的檔案共用對於小型寫入 (一般用於漫遊使用者設定檔) 擁有較佳的效能。|
|2014 年 3 月 19 日|大寫的設定檔版本尾碼 (。V2。V3。V4) 在[附錄 b:設定檔版本參考資訊](#appendix-b:-profile-version-reference-information)。|雖然 Windows 不區分大小寫，如果您使用 NFS 檔案共用，就一定要有設定檔後置詞正確 （大寫） 的大小寫。|
|2013 年 10 月 9 日|修訂的 Windows Server 2012 R2 和 Windows 8.1，釐清一些事情，並新增[多個版本的 Windows 上使用漫遊使用者設定檔時的考量](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)和[附錄 b:設定檔版本參考資訊](#appendix-b:-profile-version-reference-information)區段。|更新為新的版本; 的客戶的意見反應。|

## <a name="more-information"></a>詳細資訊

- [部署資料夾重新導向、 離線檔案和漫遊使用者設定檔](deploy-folder-redirection.md)
- [部署資料夾重新導向及漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)
- [實作使用者狀態管理](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Microsoft 的支援聲明複寫的使用者設定檔資料](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [使用 DISM 側載應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [疑難排解封裝、 部署及查詢 Windows 執行階段為基礎的應用程式](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)