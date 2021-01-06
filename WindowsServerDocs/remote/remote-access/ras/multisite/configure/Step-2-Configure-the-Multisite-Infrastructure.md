---
title: 步驟2設定多網站基礎結構
description: 若要瞭解如何設定多網站部署，必須執行幾個步驟來修改網路基礎結構設定。
manager: brianlic
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 04181093bac2bb70d9832e88dd90df79155ad5d9
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941734"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>步驟2設定多網站基礎結構

>適用目標︰Windows Server 2012 R2、Windows Server 2012

若要設定多網站部署，需要進行一些步驟來修改網路基礎結構設定，包括：設定額外的 Active Directory 網站和網域控制站、設定額外的安全性群組，以及設定群組原則物件 (Gpo) 如果您未使用自動設定的 Gpo。

|Task|描述|
|----|--------|
|2.1. 設定其他 Active Directory 網站|設定部署的額外 Active Directory 網站。|
|2.2. 設定其他網域控制站|視需要設定其他 Active Directory 網域控制站。|
|2.3. 設定安全性群組|設定任何 Windows 7 用戶端電腦的安全性群組。|
|2.4. 設定 GPO|視需要設定其他群組原則物件。|

> [!NOTE]
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="21-configure-additional-active-directory-sites"></a><a name="BKMK_ConfigAD"></a>2.1. 設定其他 Active Directory 網站
所有進入點都可以位於單一 Active Directory 網站中。 因此，在多網站設定中執行遠端存取服務器時，至少需要一個 Active Directory 網站。 如果您需要建立第一個 Active Directory 網站，或如果您想要針對多網站部署使用其他 Active Directory 網站，請使用此程式。 使用 Active Directory 網站及服務] 嵌入式管理單元，在您組織的網路中建立新的網站。

若要完成此程式，至少需要樹系中的 **Enterprise Admins** 群組成員資格或樹系根域中的 **Domain admins** 群組的成員資格或同等許可權。 請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

如需詳細資訊，請參閱 [將網站新增至樹](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732761(v=ws.11))系。

### <a name="to-configure-additional-active-directory-sites"></a>設定其他 Active Directory 網站

1.  在網域主控站上，按一下 [ **開始**]，然後按一下 [ **Active Directory 網站和服務**]。

2.  在 [Active Directory 網站和服務] 主控台的主控台樹中，以滑鼠右鍵按一下 [ **網站**]，然後按一下 [ **新增網站**]。

3.  在 [ **新增物件-網站** ] 對話方塊的 [ **名稱** ] 方塊中，輸入新網站的名稱。

4.  在 [ **連結名稱**] 中，按一下站台連結物件，然後按兩次 **[確定]** 。

5.  在主控台樹中，展開 [ **網站**]，以滑鼠右鍵按一下 [ **子網**]，然後按一下 [ **新增子網**]。

6.  在 [ **新物件-子網** ] 對話方塊的 [ **前置** 詞] 底下，輸入 IPv4 或 IPv6 子網首碼，在 [ **選取此首碼的網站物件** ] 清單中，按一下要與此子網建立關聯的網站，然後按一下 **[確定]**。

7.  重複步驟5和6，直到您已建立部署所需的所有子網為止。

8.  關閉 [Active Directory 站台及服務]。

:::image type="icon" source="../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif":::**_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

若要安裝 Windows 功能 "Active Directory module for Windows PowerShell"：

```
Install-WindowsFeature "Name RSAT-AD-PowerShell
```

或透過 OptionalFeatures 新增「Active Directory PowerShell 嵌入式管理單元」。

如果在 Windows 7 或 Windows Server 2008 R2 上執行下列 Cmdlet，則必須匯入 Active Directory PowerShell 模組：

```
Import-Module ActiveDirectory
```

若要使用內建 DEFAULTIPSITELINK 來設定名為「第二個網站」的 Active Directory 網站：

```
New-ADReplicationSite -Name "Second-Site"
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}
```

若要為第二個網站設定 IPv4 和 IPv6 子網：

```
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"
```

## <a name="22-configure-additional-domain-controllers"></a><a name="BKMK_AddDC"></a>2.2. 設定其他網域控制站
若要在單一網域中設定多網站部署，建議您在部署中的每個網站都至少有一個可寫入的網域控制站。

若要執行此程式，您至少必須是要安裝網域控制站之網域中的 Domain Admins 群組成員。

如需詳細資訊，請參閱 [安裝額外的網域控制站](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc733027(v=ws.10))。

### <a name="to-configure-additional-domain-controllers"></a>設定其他網域控制站

1.  在做為網域控制站的伺服器上，于 _ * 伺服器管理員 * * 的 **儀表板** 上，按一下 [ **新增角色及功能**]。

2.  按三次 [ **下一步]** 以進入伺服器角色選取畫面

3.  在 [ **選取伺服器角色** ] 頁面上，選取 [ **Active Directory Domain Services**]。 當出現提示時，按一下 [ **新增功能** ]，然後按三次 **[下一步]** 。

4.  在 [確認] 頁面上，按一下 [安裝]。

5.  當安裝成功完成時，請按一下 [ **將此伺服器升級為網域控制站**]。

6.  在 [Active Directory Domain Services 設定] 頁面的 [ **部署** 設定] 頁面上，按一下 [ **新增現有網域的網域控制站**]。

7.  在 [ **網域**] 中，輸入功能變數名稱;例如，corp.contoso.com。

8.  在 **[提供用來執行此作業的認證**] 下，按一下 [ **變更**]。 在 [ **Windows 安全性** ] 對話方塊中，提供可安裝其他網域控制站之帳戶的使用者名稱和密碼。 若要安裝其他網域控制站，您必須是 Enterprise Admins 群組或 Domain Admins 群組的成員。 完成提供認證之後，按 [下一步]。

9. 在 [ **網域控制站選項** ] 頁面上，執行下列動作：

    1.  進行下列選取：

        -   **網域名稱系統 (dns) server**」預設會選取此選項，讓您的網域控制站可以作為網域名稱系統 (DNS) 伺服器。 如果您不要讓網域控制站當做 DNS 伺服器，請取消選取此選項。

            如果 DNS 伺服器角色未安裝在樹系根域的網域主控站 (PDC) 模擬器中，則無法使用在其他網域控制站上安裝 DNS 伺服器的選項。 因應措施是在這種情況下，您可以在 AD DS 安裝之前或之後安裝 DNS 伺服器角色。

            > [!NOTE]
            > 如果您選取安裝 DNS 伺服器的選項，您可能會收到一則訊息，指出無法建立 DNS 伺服器的 DNS 委派，而且您應該手動建立 dns 伺服器的 DNS 委派，以確保可靠的名稱解析。 如果您要在樹系根域或樹系根域中安裝額外的網域控制站，則不需要建立 DNS 委派。 在此情況下，請按一下 **[是]** ，並忽略該訊息。

        -   **通用類別目錄 (GC)**」預設會選取此選項。 它會新增通用類別目錄，唯讀目錄磁碟分割至網域控制站，並啟用通用類別目錄搜尋功能。

        -   **唯讀網域控制站 (RODC)**」預設不會選取此選項。 它會使額外的網域控制站成為唯讀;也就是說，它會讓網域控制站成為 RODC。

    2.  在 [ **網站名稱**] 中，從清單中選取網站。

    3.  在 **[輸入目錄服務還原模式 (DSRM) 密碼]** 下的 [ **密碼** ] 和 [ **確認密碼**] 中，輸入強式密碼兩次，然後按 **[下一步]**。 您必須使用此密碼來開始 AD DS，以 DSRM 執行必須離線執行的工作。

10. 如果您想要在角色安裝期間更新 DNS 委派，請在 [ **Dns 選項** ] 頁面上，選取 [ **更新 dns 委派** ] 核取方塊，然後按 **[下一步]**。

11. 在 [ **其他選項** ] 頁面上，輸入或流覽資料庫檔案、目錄服務記錄檔和系統磁碟區 (SYSVOL) 檔的磁片區和資料夾位置。 視需要指定複寫選項，然後按 **[下一步]**。

12. 在 [ **審核選項** ] 頁面上，檢查安裝選項，然後按一下 **[下一步]**。

13. 在 [ **必要條件檢查** ] 頁面上，于驗證必要條件後，按一下 [ **安裝**]。

14. 等候嚮導完成設定，然後按一下 [ **關閉**]。

15. 如果電腦未自動重新開機，請重新開機電腦。

## <a name="23-configure-security-groups"></a><a name="BKMK_ConfigSG"></a>2.3. 設定安全性群組
針對部署中允許存取 Windows 7 用戶端電腦的每個進入點，多網站部署需要 Windows 7 用戶端電腦的額外安全性群組。 如果有多個網域包含 Windows 7 用戶端電腦，建議您在每個網域中建立相同進入點的安全性群組。 或者，您可以使用一個包含來自兩個網域之用戶端電腦的通用安全性群組。 例如，在有兩個網域的環境中，如果您想要允許存取進入點1和3中的 Windows 7 用戶端電腦，而不是進入點2，請建立兩個新的安全性群組，以針對每個網域中的每個進入點包含 Windows 7 用戶端電腦。

### <a name="to-configure-additional-security-groups"></a>設定其他安全性群組

1.  在網域主控站上，按一下 [ **開始**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2.  在主控台樹中，以滑鼠右鍵按一下您要新增群組的資料夾，例如 corp.contoso.com/Users。 指向 [新增]，然後按一下 [群組]。

3.  在 [ **新增物件-群組** ] 對話方塊的 [ **組名**] 底下，輸入新群組的名稱，例如 Win7_Clients_Entrypoint1。

4.  在 [**群組領域**] 底下，按一下 [**通用**]，在 [**群組類型**] 底下按一下 [**安全性**]，然後按一下 **[確定]**

5.  若要將電腦新增至新的安全性群組，請按兩下安全性群組，然後在 **<Group_Name>** 內容] 對話方塊中，按一下 [ **成員** ] 索引標籤。

6.  在 [成員] 索引標籤上，按一下 [新增]。

7.  選取要新增至此安全性群組的 Windows 7 電腦，然後按一下 **[確定]**。

8.  重複此程式，視需要為每個進入點建立安全性群組。

:::image type="icon" source="../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif":::**_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

若要安裝 Windows 功能 "Active Directory module for Windows PowerShell"：

```
Install-WindowsFeature "Name RSAT-AD-PowerShell
```

或透過 OptionalFeatures 新增「Active Directory PowerShell 嵌入式管理單元」。

如果在 Windows 7 或 Windows Server 2008 R2 上執行下列 Cmdlet，則必須匯入 Active Directory PowerShell 模組：

```
Import-Module ActiveDirectory
```

若要設定名為 Win7_Clients_Entrypoint1 的安全性群組，並新增名為 CLIENT2 的用戶端電腦：

```
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$
```

## <a name="24-configure-gpos"></a><a name="ConfigGPOs"></a>2.4. 設定 GPO
多站遠端存取部署需要下列群組原則物件：

-   遠端存取服務器的每個進入點的 GPO。

-   適用于每個網域之任何 Windows 8 用戶端電腦的 GPO。

-   每個網域中包含 Windows 7 用戶端電腦的 GPO，適用于設定為支援 Windows 7 用戶端的每個進入點。

    > [!NOTE]
    > 如果您沒有任何 Windows 7 用戶端電腦，則不需要為 Windows 7 電腦建立 Gpo。

當您設定「遠端存取」時，如果不存在所需的群組原則物件，則嚮導會自動建立這些物件。 如果您沒有建立群組原則物件的必要許可權，則必須在設定遠端存取之前先建立它們。 DirectAccess 系統管理員必須有 Gpo 的完整許可權 ([編輯 + 修改安全性 + 刪除]) 。

> [!IMPORTANT]
> 在手動建立 Gpo 以進行遠端存取之後，您必須在與遠端存取服務器相關聯的 Active Directory 網站中，將 Active Directory 和 DFS 複寫的足夠時間提供給網域控制站。 如果遠端存取自動建立群組原則的物件，則不需要等候時間。

若要建立群組原則物件，請參閱 [建立和編輯群組原則物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11))。

### <a name="domain-controller-maintenance-and-downtime"></a><a name="DCMaintandDowntime"></a>網域控制站維護和停機時間
當網域控制站以 PDC 模擬器的形式執行，或管理伺服器 Gpo 的網域控制站發生停機時，就無法載入或修改遠端存取設定。 如果有其他網域控制站可用，這不會影響用戶端連線能力。

若要載入或修改遠端存取設定，您可以將 PDC 模擬器角色轉移至用戶端或應用程式伺服器 Gpo 的不同網域控制站;針對伺服器 Gpo，變更管理伺服器 Gpo 的網域控制站。

> [!IMPORTANT]
> 這項作業只能由網域系統管理員執行。 變更主域控制站的影響不限於遠端存取;因此，傳送 PDC 模擬器角色時請務必小心。

> [!NOTE]
> 在修改網域控制站關聯之前，請確定遠端存取部署中的所有 Gpo 都已複寫到網域中的所有網域控制站。 如果 GPO 未同步處理，則修改網域控制站關聯之後可能會遺失最近的設定變更，這可能會導致設定損毀。 若要確認 GPO 同步，請參閱 [檢查群組原則基礎結構狀態](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134176(v=ws.11))。

#### <a name="to-transfer-the-pdc-emulator-role"></a><a name="TransferPDC"></a>傳送 PDC 模擬器角色

1.  在 _ [*開始*] 畫面上輸入 **dsa.msc**，然後按 enter。

2.  在 Active Directory 消費者和電腦主控台的左窗格中，以滑鼠右鍵按一下 [ **Active Directory 消費者和電腦**]，然後按一下 [ **變更網域控制站**]。 在 [變更目錄伺服器] 對話方塊中，按一下 [ **此網域控制站或 AD LDS 實例**]，在清單中按一下將成為新角色持有者的網域控制站，然後按一下 **[確定]**。

    > [!NOTE]
    > 如果您不是在要傳送角色的網域控制站上，您必須執行此步驟。 如果您已經連接到您要傳送角色的網域控制站，請勿執行此步驟。

3.  在主控台樹中，以滑鼠右鍵按一下 [ **Active Directory 消費者和電腦**]，指向 [ **所有** 工作]，然後按一下 [ **操作主機**]。

4.  在 [操作主機] 對話方塊中，按一下 [ **PDC** ] 索引標籤，然後按一下 [ **變更**]。

5.  按一下 **[是]** 以確認您要轉移角色，然後按一下 [ **關閉**]。

#### <a name="to-change-the-domain-controller-that-manages-server-gpos"></a><a name="ChangeDC"></a>變更管理伺服器 Gpo 的網域控制站

-   在遠端存取服務器上執行 Windows PowerShell Cmdlet  [set-daentrypointdc](/powershell/module/remoteaccess/set-daentrypointdc) ，並為 *ExistingDC* 參數指定無法連線的網域控制站名稱。 此命令會針對目前由網域控制站管理的進入點，修改其伺服器 Gpo 的網域控制站關聯。

    -   若要以網域控制站 "dc2.corp.contoso.com" 取代無法連線的網域控制站 "dc1.corp.contoso.com"，請執行下列動作：

        ```
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire
        ```

    -   若要以最接近的 Active Directory 網站中的網域控制站取代無法連線的網域控制站 "dc1.corp.contoso.com" 到遠端存取服務器 "DA1.corp.contoso.com"，請執行下列動作：

        ```
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
        ```

### <a name="change-two-or-more-domain-controllers-that-manage-server-gpos"></a><a name="ChangeTwoDCs"></a>變更兩個或多個管理伺服器 Gpo 的網域控制站
在極少數的情況下，管理伺服器 Gpo 的兩個或多個網域控制站無法使用。 如果發生這種情況，變更伺服器 Gpo 的網域控制站關聯需要更多步驟。

網域控制站關聯資訊會儲存在遠端存取服務器的登錄和所有伺服器 Gpo 中。 在下列範例中，有兩個具有兩個遠端存取服務器的進入點： "Entry point 1" 中的 "DA1" 和 "Entry point 2" 中的 "DA2"。 "Entry point 1" 的伺服器 GPO 是在網域控制站 "DC1" 中進行管理，而 "Entry point 2" 的伺服器 GPO 則是在網域控制站 "DC2" 中管理。 "DC1" 和 "DC2" 都無法使用。 第三個網域控制站仍可在網域 "DC3" 中使用，而來自 "DC1" 和 "DC2" 的資料已複寫至 "DC3"。

![設定多網站基礎結構](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)

##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>變更兩個或多個管理伺服器 Gpo 的網域控制站

1.  若要以網域控制站 "DC3" 取代無法使用的網域控制站 "DC2"，請執行下列命令：

    ```
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue
    ```

    此命令會在 DA2 的登錄和「進入點2」伺服器 GPO 本身中，更新「進入點2」伺服器 GPO 的網域控制站關聯;不過，它不會更新 "Entry point 1" 伺服器 GPO，因為管理它的網域控制站無法使用。

    > [!TIP]
    > 此命令會使用 *ErrorAction* 參數的 Continue 值，此參數會更新 "entry point 2" 伺服器 gpo，儘管無法更新 "entry point 1" 伺服器 gpo。

    產生的設定如下圖所示。

    ![顯示所產生設定的圖表。](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)

2.  若要以網域控制站 "DC3" 取代無法使用的網域控制站 "DC1"，請執行下列命令：

    ```
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue
    ```

    此命令會更新 DA1 登錄和「進入點1」和「進入點2」伺服器 Gpo 中的「進入點1」伺服器 GPO 的網域控制站關聯。 產生的設定如下圖所示。

    ![顯示第一個網域控制站關聯更新的圖表。](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)

3.  若要在「進入點1」伺服器 GPO 中同步處理「進入點2」伺服器 GPO 的網域控制站關聯，請執行命令以 "DC3" 取代 "DC2"，並指定未同步處理其伺服器 GPO 的遠端存取服務器（在此案例中為 "DA1"），代表 *ComputerName* 參數。

    ```
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue
    ```

    最後的設定如下圖所示。

    ![顯示最終設定的圖表。](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)

### <a name="optimization-of-configuration-distribution"></a><a name="ConfigDistOptimization"></a>設定散發的優化
進行設定變更時，只會在伺服器 Gpo 傳播至遠端存取服務器之後，才會套用變更。 為了縮短設定分配時間，遠端存取會自動選取 [最接近遠端存取服務器](/previous-versions/windows/it-pro/windows-2000-server/cc978016(v=technet.10)) 的可寫入網域控制站（建立其伺服器 GPO 時）。

在某些情況下，可能需要手動修改管理伺服器 GPO 的網域控制站，以優化設定分配時間：

-   在遠端存取服務器的 Active Directory 網站中，在將其新增為進入點時，沒有可寫入的網域控制站。 現在會將可寫入的網域控制站新增至遠端存取服務器的 Active Directory 網站。

-   IP 位址變更或 Active Directory 網站和子網變更可能已將遠端存取服務器移到不同的 Active Directory 網站。

-   因為網域控制站上的維護工作已手動修改進入點的網域控制站關聯，現在網域控制站已重新上線。

在這些情況下，請在 `Set-DAEntryPointDC` 遠端存取服務器上執行 PowerShell Cmdlet，並使用參數 *EntryPointName* 指定要優化的進入點名稱。 只有在目前儲存伺服器 GPO 的網域控制站上的 GPO 資料已完全複寫至所需的新網域控制站之後，才應該這樣做。

> [!NOTE]
> 在修改網域控制站關聯之前，請確定遠端存取部署中的所有 Gpo 都已複寫到網域中的所有網域控制站。 如果 GPO 未同步處理，則修改網域控制站關聯之後可能會遺失最近的設定變更，這可能會導致設定損毀。 若要確認 GPO 同步，請參閱 [檢查群組原則基礎結構狀態](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134176(v=ws.11))。

若要將設定分配時間優化，請執行下列其中一項：

-   若要在最近 Active Directory 網站中的網域控制站上，管理進入點「進入點1」的伺服器 GPO，請執行下列命令：

    ```
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
    ```

-   若要在網域控制站 "dc2.corp.contoso.com" 上管理進入點 "Entry point 1" 的伺服器 GPO，請執行下列命令：

    ```
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire
    ```

    > [!NOTE]
    > 修改與特定進入點相關聯的網域控制站時，您必須指定遠端存取服務器，該伺服器是 *ComputerName* 參數的該進入點成員。

## <a name="see-also"></a><a name="BKMK_Links"></a>請參閱

-   [步驟3：設定多網站部署](Step-3-Configure-the-Multisite-Deployment.md)
-   [步驟1：執行單一伺服器遠端存取部署](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)
