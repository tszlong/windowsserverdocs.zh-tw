---
title: 使用離線檔案部署資料夾重新導向
description: 如何使用 Windows Server 將資料夾重新導向離線檔案部署至 Windows 用戶端電腦。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21172d9d3e6d91af691986bfd84b0e32049f3b88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401969"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>使用離線檔案部署資料夾重新導向

>適用於：Windows 10、Windows 7、Windows 8、Windows 8.1、Windows Vista、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2、Windows Server 2008 R2、Windows Server （半年通道）

本主題說明如何使用 Windows Server，將資料夾重新導向離線檔案部署至 Windows 用戶端電腦。

如需本主題的最近變更清單，請參閱[變更歷程記錄](#change-history)。

> [!IMPORTANT]
> 由於[MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)中所做的安全性變更，我們更新[了步驟3：建立本主題](#step-3-create-a-gpo-for-folder-redirection)之資料夾重新導向的 GPO，讓 Windows 可以適當地套用資料夾重新導向原則（而不是在受影響的電腦上還原重新導向的資料夾）。

## <a name="prerequisites"></a>必要條件

### <a name="hardware-requirements"></a>硬體需求

資料夾重新導向需要 x64 型或 x86 型電腦;Windows® RT 不支援。

### <a name="software-requirements"></a>軟體需求

資料夾重新導向具有下列軟體需求：

- 若要管理資料夾重新導向，您必須以 Domain Administrators 安全性群組、Enterprise Administrators 安全性群組或群組原則 Creator Owners 安全性群組的成員身分登入。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。
- 用戶端電腦必須加入您所管理的 Active Directory 網域服務 (AD DS)。
- 必須有一部電腦安裝群組原則管理與 Active Directory 系統管理中心。
- 必須要有檔案伺服器，才能裝載重新導向的資料夾。
    - 如果檔案共用使用 DFS 命名空間，DFS 資料夾 (連結) 必須有單一的目標，以防止使用者在不同伺服器上進行衝突的編輯。
    - 如果檔案共用使用 DFS 複寫與與另一部伺服器複寫內容，使用者必須只能存取來源伺服器，以防止使用者在不同伺服器上進行衝突的編輯。
    - 使用叢集檔案共用時，請停用檔案共用上的持續可用性，以避免資料夾重新導向和離線檔案的效能問題。 此外，在使用者失去持續可用的檔案共用的存取權之後，離線檔案可能不會轉換為離線模式3-6 分鐘，這可能會讓尚未使用離線檔案永遠離線模式的使用者感到不快。

> [!NOTE]
> 資料夾重新導向中的某些較新功能會有額外的用戶端電腦和 Active Directory 架構需求。 如需詳細資訊，請參閱[部署主要電腦](deploy-primary-computers.md)、[停用資料夾上的離線檔案](disable-offline-files-on-folders.md)、[啟用永遠離線模式](enable-always-offline.md)，以及[啟用優化資料夾移動](enable-optimized-moving.md)。

## <a name="step-1-create-a-folder-redirection-security-group"></a>步驟 1:建立資料夾重新導向安全性群組

如果您的環境尚未設定資料夾重新導向，第一個步驟是建立一個安全性群組，其中包含您要套用資料夾重新導向原則設定的所有使用者。

以下說明如何建立資料夾重新導向的安全性群組：

1. 在已安裝 Active Directory 系統管理中心的電腦上開啟伺服器管理員。
2. 在 [**工具**] 功能表上，選取 [ **Active Directory 系統管理中心**]。 [Active Directory 系統管理中心] 隨即顯示。
3. 在適當的網域或 OU 上按一下滑鼠右鍵，選取 [**新增**]，然後選取 [**群組**]。
4. 在 [建立群組] 視窗內的 [群組] 區段中，指定下列設定：
    - 在 [群組名稱] 中，輸入安全性群組的名稱，例如：**資料夾**重新導向使用者。
    - 在 [**群組領域**] 中，選取 [**安全性**]，然後選取 [**全域**]。
5. 在 [**成員**] 區段中，選取 [**新增**]。 [選取使用者、連絡人、電腦、服務帳戶或群組] 對話方塊隨即顯示。
6. 輸入您要部署資料夾重新導向的使用者或群組的名稱，選取 **[確定]** ，然後再次選取 **[確定]** 。

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>步驟 2:為重新導向的資料夾建立檔案共用

如果您還沒有已重新導向資料夾的檔案共用，請使用下列程式在執行 Windows Server 2012 的伺服器上建立檔案共用。

> [!NOTE]
> 如果您在執行另一個版本的 Windows Server 的伺服器上建立檔案共用，某些功能可能不同或無法使用。

以下說明如何在 Windows Server 2019、Windows Server 2016 和 Windows Server 2012 上建立檔案共用：

1. 在伺服器管理員流覽窗格中，選取 [檔案**和存放服務**]，然後選取 [**共用**] 以顯示 [共用] 頁面。
2. 在 [**共用**] 磚中 **，選取 [** 工作]，然後選取 [**新增共用**]。 「新增共用精靈」隨即顯示。
3. 在 [**選取設定檔**] 頁面上，選取 [ **SMB 共用-快速**]。 如果您已安裝檔案伺服器 Resource Manager 並使用資料夾管理屬性，請改為選取 [ **SMB 共用-Advanced**]。
4. 在 [共用位置] 頁面上，選取您要在上面建立共用的伺服器和磁碟區。
5. 在 [**共用名稱**] 頁面的 [**共用名稱**] 方塊中，輸入共用的名稱（例如，**使用者 $** ）。
    >[!TIP]
    >建立共用時，在共用名稱後面放一個 ```$``` 可隱藏共用。 這將會隱藏來自偶爾瀏覽器的共用。
6. 在 [**其他設定**] 頁面上，清除 [啟用持續可用性] 核取方塊（如果有的話），並選擇性地選取 [**啟用存取型列舉**] 和 [**加密資料存取**] 核取方塊。
7. 在 [**許可權**] 頁面上，選取 [**自訂許可權**]。 [進階安全性設定] 對話方塊隨即出現。
8. 選取 [**停用繼承**]，然後選取 [**將繼承的許可權轉換成此物件的明確許可權**]。
9. 依照 [圖 1] 所述設定許可權，並移除未列出群組和帳戶的許可權，並將特殊許可權新增至您在步驟1中建立的 [資料夾重新導向使用者] 群組。
    
    ![設定重新導向資料夾共用的許可權](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **圖 1**設定重新導向資料夾共用的許可權
10. 如果您選擇 [SMB 共用 - 進階] 設定檔，在 [管理屬性] 頁面上，選取 [使用者檔案] 資料夾使用方式值。
11. 如果您選擇 [SMB 共用 - 進階] 設定檔，在 [配額] 頁面上，選擇性地選取套用至共用之使用者的配額。
12. 在 [**確認**] 頁面上，選取 [**建立]。**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>主控重新導向資料夾之檔案共用的必要許可權

| 使用者帳戶  | 存取權  | 適用於  |
| --------- | --------- | --------- |
| 使用者帳戶 | 存取權 | 適用於 |
| 系統     | 完全控制        |    這個資料夾、子資料夾及檔案     |
| Administrators     | 完全控制       | 只有這個資料夾        |
| 建立者/擁有者     |   完全控制      |   子資料夾及檔案      |
| 需要將資料放在共用上的使用者安全性群組（資料夾重新導向使用者）     |   列出資料夾/讀取資料 *（Advanced 許可權）* <br /><br />建立資料夾/附加資料 *（Advanced 許可權）* <br /><br />讀取屬性 *（Advanced 許可權）* <br /><br />讀取擴充屬性 *（Advanced 許可權）* <br /><br />讀取權限 *（Advanced 許可權）*      |  只有這個資料夾       |
| 其他群組與帳戶     |  無 (移除)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>步驟 3：建立資料夾重新導向的 GPO

如果您還沒有為資料夾重新導向設定建立的 GPO，請使用下列程式建立一個。

以下是建立資料夾重新導向的 GPO 的方法：

1. 在已安裝群組原則管理的電腦上開啟伺服器管理員。
2. 從 [**工具**] 功能表中，選取 [**群組原則管理**]。
3. 在您要設定資料夾重新導向的網域或 OU 上按一下滑鼠右鍵，然後選取 [**在這個網域中建立 GPO 並連結到**]。
4. 在 [**新增 gpo** ] 對話方塊中，輸入 GPO 的名稱（例如，[資料夾重新導向**設定**]），然後選取 **[確定]** 。
5. 以滑鼠右鍵按一下新建立的 GPO，然後清除 [啟用連結] 核取方塊。 這可以防止在您完成設定之前就套用 GPO。
6. 選取 GPO。 在 [**領域**] 索引標籤的 [**安全性篩選**] 區段中，選取 [**已驗證的使用者**]，然後選取 [**移除**] 以防止將 GPO 套用至所有人。
7. 在 [**安全性篩選**] 區段中，選取 [**新增**]。
8. 在 [**選取使用者、電腦或群組**] 對話方塊中，輸入您在步驟1中建立的安全性群組名稱（例如，[資料夾重新導向**使用者**]），然後選取 **[確定]** 。
9. 選取 [**委派**] 索引標籤，選取 [**新增**]，輸入**已驗證的使用者**，選取 **[確定]** ，然後再次選取 **[確定]** 以接受預設的讀取權限。
    
    這是必要步驟，因為[MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)中所做的安全性變更。

> [!IMPORTANT]
> 由於[MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)中所做的安全性變更，您現在必須授與已驗證的使用者群組對資料夾重新導向 GPO 的委派讀取權限; 否則 gpo 將不會套用至使用者，或如果已套用，則會移除 gpo，重新導向資料夾回到本機電腦。 如需詳細資訊，請參閱[部署群組原則安全性更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>步驟 4：使用離線檔案設定資料夾重新導向

建立資料夾重新導向設定的 GPO 之後，請編輯群組原則設定以啟用和設定資料夾重新導向，如下列程式所述。

> [!NOTE]
> Windows 用戶端電腦上的重新導向資料夾預設會啟用離線檔案，除非使用者變更，否則會在執行 Windows Server 的電腦上停用。 若要使用群組原則來控制是否啟用離線檔案，請使用 [**允許] 或 [不允許使用離線檔案功能**原則] 設定。
> 如需其他離線檔案群組原則設定的詳細資訊，請參閱[啟用 Advanced 離線檔案功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)和設定[群組原則以進行離線檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>)。

以下說明如何在群組原則中設定資料夾重新導向：

1. 在群組原則管理 中，以滑鼠右鍵按一下您建立的 GPO （例如 資料夾重新導向**設定**），然後選取 **編輯**。
2. 在 [群組原則管理編輯器] 視窗中，依序流覽至 [**使用者**設定]、[**原則**]、[ **Windows 設定**] 和 [**資料夾**重新導向]。
3. 以滑鼠右鍵按一下您想要重新導向的資料夾（例如 [**檔**]），然後選取 [**屬性**]。
4. 在 [**屬性**] 對話方塊的 [**設定**] 方塊中，選取 [**基本]-將每個人的資料夾重新導向到相同的位置**。

    > [!NOTE]
    > 若要將資料夾重新導向套用至執行 Windows XP 或 Windows Server 2003 的用戶端電腦，請選取 [**設定**] 索引標籤，並選取 [**同時將重新導向原則套用至 Windows 2000、windows 2000 SERVER、Windows XP 及 windows Server 2003 操作][系統**] 核取方塊。

5. 在 [**目的檔案夾位置**] 區段中，**針對 [根路徑] 下的每個使用者選取 [建立資料夾**]，然後在 [**根路徑**] 方塊中，輸入儲存重新導向資料夾之檔案共用的路徑，例如：  **\\ \\fs1.corp.contoso.com\\使用者 $**
6. 選取 [**設定**] 索引標籤，然後在 [**原則移除**] 區段中，選擇性地選取 **[在移除原則時將資料夾重新導向回本機 userprofile] 位置**（這項設定有助於讓資料夾重新導向的行為更多可預測的 adminisitrators 和使用者）。
7. 選取 **[確定]** ，然後在警告對話方塊中選取 [**是**]。

## <a name="step-5-enable-the-folder-redirection-gpo"></a>步驟 5：啟用資料夾重新導向 GPO

完成資料夾重新導向群組原則設定後，下一個步驟是啟用 GPO，允許將它套用至受影響的使用者。

> [!TIP]
> 如果您計劃實作主要電腦支援或其他原則設定，請立即進行此動作，才能啟用 GPO。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。

以下是啟用資料夾重新導向 GPO 的方式：

1. 開啟 [群組原則管理]。
2. 以滑鼠右鍵按一下您建立的 GPO，然後選取 [**啟用連結**]。 功能表項目旁邊會出現一個核取方塊。

## <a name="step-6-test-folder-redirection"></a>步驟 6：測試檔案夾重新導向

若要測試檔案夾重新導向，請使用為資料夾重新導向設定的使用者帳戶登入電腦。 然後，確認資料夾和設定檔已重新導向。

以下是測試檔案夾重新導向的方法：

1. 使用已啟用資料夾重新導向的使用者帳戶，登入主要電腦（如果您已啟用主要電腦支援）。
2. 如果使用者先前已登入電腦，請開啟提升權限的命令提示字元，然後輸入下列命令，以確保最新的群組原則設定套用到用戶端電腦：
    
    ```PowerShell
    gpupdate /force
    ```
3. 開啟 [檔案總管]。
4. 以滑鼠右鍵按一下重新導向的資料夾（例如，文件庫中的 [我的文件] 資料夾），然後選取 [**屬性**]。
5. 選取 [**位置**] 索引標籤，並確認路徑顯示您指定的檔案共用，而不是本機路徑。

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>附錄 A：部署資料夾重新導向的檢查清單

| 狀態           | Action |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1.準備網域<br>-將電腦加入網域<br>-建立使用者帳戶 |
| ☐<br><br><br>   | 2.建立資料夾重新導向的安全性群組<br>-組名：<br>屬於 |
| ☐<br><br>       | 3.為重新導向的資料夾建立檔案共用<br>-檔案共用名稱： |
| ☐<br><br>       | 4.建立資料夾重新導向的 GPO<br>-GPO 名稱： |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5.設定資料夾重新導向和離線檔案原則設定<br>-重新導向的資料夾：<br>-Windows 2000、Windows XP 及 Windows Server 2003 支援已啟用嗎？<br>-離線檔案已啟用？ （在 Windows 用戶端電腦上預設為啟用）<br>-一律啟用離線模式？<br>-已啟用背景檔案同步處理？<br>-已啟用優化的重新導向資料夾移動功能？ |
| ☐<br><br>☐<br><br>☐<br>☐ | 6.選擇性啟用主要電腦支援<br>-以電腦為基礎或以使用者為基礎的？<br>-指定使用者的主要電腦<br>-使用者和主要電腦對應的位置：<br>-（選擇性）啟用資料夾重新導向的主要電腦支援<br>-（選擇性）啟用漫遊使用者設定檔的主要電腦支援 |
| ☐         | 7.啟用資料夾重新導向 GPO |
| ☐         | 8.測試檔案夾重新導向 |

## <a name="change-history"></a>變更歷程記錄

下表摘要說明本主題中一些最重要的變更。

| Date | 描述 | `Reason`|
| --- | --- | --- |
| 2017年1月18日 | 已將步驟新增[至步驟3：建立資料夾](#step-3-create-a-gpo-for-folder-redirection)重新導向的 GPO，將讀取權限委派給已驗證的使用者，因為這是群組原則的安全性更新，所以現在是必要的。 | 客戶回函 |

## <a name="more-information"></a>詳細資訊

* [資料夾重新導向、離線檔案和漫遊使用者設定檔](folder-redirection-rup-overview.md)
* [部署資料夾重新導向和漫遊使用者設定檔的主要電腦](deploy-primary-computers.md)
* [啟用 Advanced 離線檔案功能](enable-always-offline.md)
* [Microsoft 針對複寫的使用者設定檔資料所提供的支援聲明](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [使用 DISM 側載應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [針對以 Windows 執行階段為基礎的應用程式進行封裝、部署和查詢的疑難排解](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)