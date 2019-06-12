---
title: 部署資料夾重新導向與離線檔案
description: 如何使用 Windows Server，將離線檔案的資料夾重新導向至 Windows 用戶端電腦。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4b6c58f0f33b45052e038a9af941297a294d17b2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812503"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>部署資料夾重新導向與離線檔案

>適用於：Windows 10，Windows 7、 Windows 8、 Windows 8.1，Windows Vista、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2008 R2 及 Windows Server （半年通道）

本主題描述如何使用 Windows Server 部署至 Windows 用戶端電腦的離線檔案的資料夾重新導向。

如需本主題的最新變更的清單，請參閱 <<c0> [ 修訂歷程記錄](#change-history)。

> [!IMPORTANT]
> 因為所做的安全性變更[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)，我們已更新[步驟 3:建立 GPO 的資料夾重新導向](#step-3-create-a-gpo-for-folder-redirection)本主題，讓該 Windows 可以正確地套用資料夾重新導向原則 （並還原受影響的電腦上的重新導向的資料夾）。

## <a name="prerequisites"></a>先決條件

### <a name="hardware-requirements"></a>硬體需求

資料夾重新導向需要 x64 或 x86 型電腦;Windows® RT 不支援

### <a name="software-requirements"></a>軟體需求

資料夾重新導向的軟體需求如下：

- 若要管理資料夾重新導向，您必須以 Domain Administrators 安全性群組、 Enterprise Administrators 安全性群組或 Group Policy Creator Owners 安全性群組的成員登入。
- 用戶端電腦必須執行 Windows 10、 Windows 8.1，Windows 8、 Windows 7、 Windows Server 2019、 Windows Server 2016、 Windows Server （半年通道）、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008。
- 用戶端電腦必須加入您所管理的 Active Directory 網域服務 (AD DS)。
- 必須有一部電腦安裝群組原則管理與 Active Directory 系統管理中心。
- 檔案伺服器必須是可用來裝載重新導向的資料夾。
    - 如果檔案共用使用 DFS 命名空間，DFS 資料夾 (連結) 必須有單一的目標，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用使用 DFS 複寫與與另一部伺服器複寫內容，使用者必須只能存取來源伺服器，以防止使用者在不同伺服器上進行衝突的編輯。
    - 當使用叢集的檔案共用，請停用持續可用性以避免效能問題，資料夾重新導向和離線檔案與檔案共用上。 此外，離線檔案可能不轉換成離線模式 3-6 分鐘之後使用者遺失存取持續可用的檔案共用，可能感到挫折尚未使用離線檔案的 「 永遠離線模式的使用者。

> [!NOTE]
> 資料夾重新導向的一些較新的功能有其他的用戶端電腦和 Active Directory 結構描述的需求。 如需詳細資訊，請參閱 <<c0> [ 部署的主要電腦](deploy-primary-computers.md)，[停用離線檔案的資料夾](disable-offline-files-on-folders.md)，[啟用永遠離線模式](enable-always-offline.md)，和[啟用最佳化資料夾移動](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>步驟 1：建立資料夾重新導向的安全性群組

如果您的環境尚未設定資料夾重新導向，第一個步驟是建立包含您要套用資料夾重新導向原則設定的所有使用者的安全性群組。

以下是如何建立安全性群組的資料夾重新導向：

1. 已安裝的電腦上開啟的伺服器管理員，Active Directory 系統管理中心。
2. 在 **工具**功能表上，選取**Active Directory 系統管理中心**。 [Active Directory 系統管理中心] 隨即顯示。
3. 以滑鼠右鍵按一下適當的網域或 OU 中，選取**的新**，然後選取**群組**。
4. 在 [建立群組]  視窗內的 [群組]  區段中，指定下列設定：
    - 在 [群組名稱]  中，輸入安全性群組的名稱，例如：**資料夾重新導向使用者**。
    - 在 **群組領域**，選取**安全性**，然後選取**Global**。
5. 在 **成員**區段中，選取**新增**。 [選取使用者、連絡人、電腦、服務帳戶或群組] 對話方塊隨即顯示。
6. 輸入名稱的使用者或群組，您想要部署資料夾重新導向，請選取 **[確定]** ，然後選取**確定**一次。

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>步驟 2：建立重新導向資料夾的檔案共用

如果您還沒有重新導向資料夾的檔案共用，請使用下列程序來執行 Windows Server 2012 的伺服器上建立檔案共用。

> [!NOTE]
> 如果您在執行另一個版本的 Windows Server 的伺服器上建立檔案共用，某些功能可能不同或無法使用。

以下是如何在 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 上建立檔案共用：

1. 在 [伺服器管理員瀏覽] 窗格中，選取**檔案和存放服務**，然後選取**共用**以顯示 [共用] 頁面。
2. 在 **共用**磚中，選取**工作**，然後選取**新共用**。 「新增共用精靈」隨即顯示。
3. 在 **選取設定檔**頁面上，選取**SMB 共用-快速**。 如果您已安裝的檔案伺服器資源管理員，並使用資料夾管理屬性，改為選取**SMB 共用-進階**。
4. 在 [共用位置]  頁面上，選取您要在上面建立共用的伺服器和磁碟區。
5. 在上**共用名稱**頁面上，輸入共用的名稱 (例如**使用者 $** ) 中**共用名稱** 方塊中。
    >[!TIP]
    >建立共用時，在共用名稱後面放一個 ```$``` 可隱藏共用。 這樣會隱藏業餘的瀏覽器中的共用。
6. 在 **其他設定**頁面上，若有的話，清除 啟用持續可用性 核取方塊，然後選擇**啟用存取型列舉**和**加密資料存取**核取方塊。
7. 在 **權限**頁面上，選取**自訂權限...** . [進階安全性設定] 對話方塊隨即出現。
8. 選取 **停用繼承**，然後選取**繼承的轉換成此物件的明確權限的權限**。
9. 設定權限，以及說明 [表 1 所示的圖 1] 移除未列出的群組和帳戶權限，並將特殊權限新增至您在步驟 1 中建立的資料夾重新導向使用者群組。
    
    ![設定重新導向的資料夾共用的權限](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **圖 1**重新導向的資料夾共用設定的權限
10. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [管理屬性]  頁面上，選取 [使用者檔案]  資料夾使用方式值。
11. 如果您選擇 [SMB 共用 - 進階]  設定檔，在 [配額]  頁面上，選擇性地選取套用至共用之使用者的配額。
12. 在 **確認**頁面上，選取**建立。**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>必要的權限的檔案共用裝載重新導向的資料夾

| 使用者帳戶  | 存取權  | 適用於  |
| --------- | --------- | --------- |
| 使用者帳戶 | 存取權 | 適用於 |
| 系統     | 完全控制        |    這個資料夾、子資料夾及檔案     |
| Administrators     | 完全控制       | 只有這個資料夾        |
| 建立者/擁有者     |   完全控制      |   子資料夾及檔案      |
| 使用者需要將資料放在共用 （資料夾重新導向使用者） 的安全性群組     |   列出資料夾 / 讀取資料 *（進階權限）* <br /><br />建立資料夾 / 附加資料 *（進階權限）* <br /><br />讀取屬性 *（進階權限）* <br /><br />讀取擴充屬性 *（進階權限）* <br /><br />讀取權限 *（進階權限）*      |  只有這個資料夾       |
| 其他群組與帳戶     |  無 (移除)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>步驟 3：建立 GPO 的資料夾重新導向

如果您還沒有建立資料夾重新導向設定的 GPO，使用下列程序來建立一個。

以下是如何建立 GPO 的資料夾重新導向：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從**工具**功能表上，選取**群組原則管理**。
3. 以滑鼠右鍵按一下網域或 OU，您要設定資料夾重新導向，然後選取**在這個網域中建立 GPO 並連結到**。
4. 在**新的 GPO**  對話方塊中，輸入 gpo 的名稱 (例如**資料夾重新導向設定**)，然後選取**確定**。
5. 以滑鼠右鍵按一下新建立的 GPO，然後清除 [啟用連結]  核取方塊。 這可以防止在您完成設定之前就套用 GPO。
6. 選取 GPO。 在**安全性篩選**一節**範圍**索引標籤上，選取**Authenticated Users**，然後選取**移除**以防止 GPO套用到每個人。
7. 在 **安全性篩選**區段中，選取**新增**。
8. 在**選取使用者、 電腦或群組**] 對話方塊中，輸入您在步驟 1 中建立的安全性群組名稱群組 (例如**資料夾重新導向使用者**)，然後選取 [ **[確定]** .
9. 選取**委派**索引標籤上，選取**新增**，型別**Authenticated Users**，選取**確定**，然後選取  **確定**一次接受預設值的讀取權限。
    
    這是步驟中所做的安全性變更，因此需要[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)。

> [!IMPORTANT]
> 因為所做的安全性變更[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)，現在您必須提供已驗證的使用者委派的群組的讀取權限給資料夾重新導向 GPO-給使用者，否則不會套用 GPO 或 GPO 已套用，如果是移除，將資料夾重新導向回到本機電腦。 如需詳細資訊，請參閱 <<c0> [ 部署群組原則安全性更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>步驟 4：設定資料夾重新導向與離線檔案

之後建立的資料夾重新導向設定的 GPO，請編輯群組原則設定來啟用及設定資料夾重新導向，如下列程序所述。

> [!NOTE]
> 離線檔案是在 Windows 用戶端電腦上的重新導向資料夾的預設會啟用和停用執行 Windows Server 的電腦上，除非經使用者變更。 若要使用群組原則控制是否啟用離線檔案，使用**允許或禁止使用離線檔案功能**原則設定。
> 如需其他離線檔案群組原則設定的一些資訊，請參閱[啟用進階離線檔案功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)，和[離線檔案的設定群組原則](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>)。

以下是如何在群組原則中設定資料夾重新導向：

1. 在 群組原則管理，以滑鼠右鍵按一下您建立的 GPO (例如**資料夾重新導向設定**)，然後選取**編輯**。
2. 在 [群組原則管理編輯器] 視窗中，瀏覽至**使用者設定**，然後**原則**，然後**Windows 設定**，然後**資料夾重新導向**。
3. 以滑鼠右鍵按一下您想要重新導向的資料夾 (例如**文件**)，然後選取**屬性**。
4. 在 [**屬性**] 對話方塊中，從**設定**方塊中，選取**基本-將每個人的資料夾重新導向至相同的位置**。

    > [!NOTE]
    > 若要套用資料夾重新導向至用戶端電腦執行 Windows XP 或 Windows Server 2003 中，選取**設定**索引標籤，然後選取 **也將重新導向原則套用至 Windows 2000、 Windows 2000 Server、 Windows XP 和Windows Server 2003 作業系統**核取方塊。

5. 在 **目標資料夾位置**區段中，選取**為每個使用者在根路徑建立一個資料夾**，然後在**根路徑**方塊中，輸入檔案共用儲存的路徑重新導向的資料夾，例如：  **\\ \\fs1.corp.contoso.com\\使用者 $**
6. 選取 [**設定**] 索引標籤，然後在**移除原則時**區段中，選擇性地選取**移除原則時，將資料夾重新導向回本機使用者設定檔的位置**（這項設定有助於讓 adminisitrators 和使用者更可預測的方式運作的資料夾重新導向）。
7. 選取  **確定**，然後選取**是**在警告對話方塊中。

## <a name="step-5-enable-the-folder-redirection-gpo"></a>步驟 5：啟用資料夾重新導向的 GPO

當您完成設定資料夾重新導向群組原則設定時下, 一個步驟是啟用 GPO，允許將它套用至受影響的使用者。

> [!TIP]
> 如果您計劃實作主要電腦支援或其他原則設定，請立即進行此動作，才能啟用 GPO。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。

以下是如何啟用資料夾重新導向 GPO:

1. 開啟 [群組原則管理]。
2. 以滑鼠右鍵按一下您所建立，然後選取 GPO**啟用連結**。 旁邊的功能表項目會出現一個核取方塊。

## <a name="step-6-test-folder-redirection"></a>步驟 6：測試資料夾重新導向

若要測試資料夾重新導向，使用登入的電腦設定資料夾重新導向的使用者帳戶。 然後確認資料夾和設定檔會重新導向。

以下是如何測試資料夾重新導向：

1. 登入主要電腦 （如果您啟用主要電腦支援），您已啟用資料夾重新導向的使用者帳戶。
2. 如果使用者先前已登入電腦，請開啟提升權限的命令提示字元，然後輸入下列命令，以確保最新的群組原則設定套用到用戶端電腦：
    
    ```PowerShell
    gpupdate /force
    ```
3. 開啟 [檔案總管]。
4. 以滑鼠右鍵按一下 [重新導向的資料夾 （比方說，是在文件庫中的我的文件] 資料夾），然後按**屬性**。
5. 選取 **位置**索引標籤，並確認路徑中顯示您指定而不是本機路徑的檔案共用。

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>附錄 A：部署資料夾重新導向的檢查清單

| 狀態           | 動作 |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1.準備網域<br>-將電腦加入網域<br>建立使用者帳戶 |
| ☐<br><br><br>   | 2.資料夾重新導向的中建立安全性群組<br>群組名稱：<br>成員： |
| ☐<br><br>       | 3.建立重新導向資料夾的檔案共用<br>檔案共用名稱： |
| ☐<br><br>       | 4.建立 GPO 的資料夾重新導向<br>-GPO 名稱： |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5.設定資料夾重新導向和離線檔案原則設定<br>-重新導向的資料夾：<br>-Windows 2000、 Windows XP 和 Windows Server 2003 支援啟用嗎？<br>-已啟用離線的檔案嗎？ （Windows 用戶端電腦上的預設為啟用）<br>-已啟用永遠離線模式嗎？<br>已啟用背景檔案同步處理？<br>最佳化移動已啟用重新導向資料夾的安全？ |
| ☐<br><br>☐<br><br>☐<br>☐ | 6.（選擇性）啟用主要電腦支援<br>-電腦為基礎或以使用者為基礎？<br>-指定使用者的主要電腦<br>使用者和主要電腦對應的位置：<br>-（選擇性） 啟用資料夾重新導向主要電腦支援<br>-（選擇性） 啟用漫遊使用者設定檔的主要電腦支援 |
| ☐         | 7.啟用資料夾重新導向的 GPO |
| ☐         | 8.測試資料夾重新導向 |

## <a name="change-history"></a>變更歷程記錄

下表摘要說明本主題中一些最重要的變更。

| Date | 描述 | `Reason`|
| --- | --- | --- |
| 2017 年 1 月 18日日 | 新增步驟[步驟 3:建立 GPO 的資料夾重新導向](#step-3-create-a-gpo-for-folder-redirection)委派已驗證的使用者，現在的 「 讀取 」 權限需要因為群組原則安全性更新。 | 客戶回函 |

## <a name="more-information"></a>詳細資訊

* [資料夾重新導向、 離線檔案和漫遊使用者設定檔](folder-redirection-rup-overview.md)
* [部署資料夾重新導向及漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)
* [啟用進階的離線檔案功能](enable-always-offline.md)
* [Microsoft 的支援聲明複寫的使用者設定檔資料](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [使用 DISM 側載應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [疑難排解封裝、 部署及查詢 Windows 執行階段為基礎的應用程式](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)