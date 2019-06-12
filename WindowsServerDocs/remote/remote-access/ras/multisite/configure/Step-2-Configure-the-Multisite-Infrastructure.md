---
title: 步驟 2 設定多站台的基礎結構
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f3b2eb55c11348c3abcb1ef9e234cd19ba727758
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446597"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>步驟 2 設定多站台的基礎結構

>適用於：Windows Server 2012 R2, Windows Server 2012

若要設定站台部署，有幾個步驟，才能修改網路基礎結構設定，包括： 設定額外的 Active Directory 站台和網域控制站、 設定額外的安全性群組，以及設定群組原則物件 (Gpo) (如果您不使用自動設定的 Gpo。  
  
|工作|描述|  
|----|--------|  
|2.1. 設定其他 Active Directory 站台|設定部署的其他 Active Directory 站台。|  
|2.2. 設定其他網域控制站|視需要設定其他 Active Directory 網域控制站。|  
|2.3. 設定安全性群組|設定任何 Windows 7 用戶端電腦的安全性群組。|  
|2.4. 設定 GPO|視需要設定其他群組原則物件。|  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigAD"></a>2.1. 設定其他 Active Directory 站台  
所有進入點可以都位於單一的 Active Directory 站台。 因此，至少一個 Active Directory 站台是在多站台設定中的遠端存取伺服器的實作所需的。 如果您要建立第一個的 Active Directory 站台，或如果您想要用於多站台部署中的其他 Active Directory 站台，請使用此程序。 若要建立新站台"s 網路您組織中使用 Active Directory 站台及服務嵌入式管理單元。  

中的成員資格**Enterprise Admins**樹系中群組或有**Domain Admins**樹系根網域或對等項目，最小值中的群組，才能完成此程序。 請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

如需詳細資訊，請參閱 <<c0> [ 新增至樹系的站台](https://technet.microsoft.com/library/cc732761.aspx)。  

### <a name="to-configure-additional-active-directory-sites"></a>若要設定其他 Active Directory 站台  
  
1.  在主要網域控制站上按一下**開始**，然後按一下**Active Directory 站台及服務**。  
  
2.  在 Active Directory 站台和服務主控台中，在主控台樹狀目錄中，以滑鼠右鍵按一下**站台**，然後按一下**新的站台**。  
  
3.  在 **新物件-站台**對話方塊中，於**名稱**方塊中，輸入新的站台的名稱。  
  
4.  在 **連結名稱**，按一下 站台連結物件，然後按一下 **確定**兩次。  
  
5.  在主控台樹狀目錄中，依序展開**站台**，以滑鼠右鍵按一下**子網路**，然後按一下**新的子網路**。  
  
6.  在上**新增物件-子網路**對話方塊的 **前置詞**，輸入 IPv4 或 IPv6 子網路前置詞**選取此首碼的站台物件**清單中，按一下要建立關聯的站台使用此子網路，然後再按一下**確定**。  
  
7.  重複步驟 5 和 6，直到您已建立您的部署中所需的所有子網路。  
  
8.  關閉 Active Directory 站台及服務。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
若要安裝 Windows 功能 「 適用於 Windows PowerShell 的 Active Directory 模組 」:  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或 「 Active Directory PowerShell 嵌入式管理單元 」 透過 OptionalFeatures 新增。  
  
如果在 Windows 7 」 或 Windows Server 2008 R2 上執行下列 cmdlet，必須要匯入 Active Directory PowerShell 模組：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要設定 Active Directory 站台命名為 「 第二個網站 」 使用內建 DEFAULTIPSITELINK:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
若要設定 IPv4 和 IPv6 子網路的第二個站台：  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2. 設定其他網域控制站  
若要在單一網域設定多站台部署，建議您部署中有至少一個可寫入的網域控制站，每個站台。  
  
若要執行此程序，至少必須在安裝網域控制站之網域中 Domain Admins 群組的成員。  
  
如需詳細資訊，請參閱 <<c0> [ 安裝其他網域控制站](https://technet.microsoft.com/library/cc733027.aspx)。
  
### <a name="to-configure-additional-domain-controllers"></a>若要設定其他網域控制站  
  
1.  在做為網域控制站的伺服器上**伺服器管理員**上**儀表板**，按一下 **新增角色及功能**。  
  
2.  按一下 [**下一步]** 三次，進入伺服器角色選擇畫面  
  
3.  在 **選取伺服器角色**頁面上，選取**Active Directory 網域服務**。 按一下 [**將功能加入**當出現提示，然後按一下**下一步]** 三次。  
  
4.  在 [確認]  頁面上，按一下 [安裝]  。  
  
5.  安裝成功完成時，按一下**此伺服器升級為網域控制站**。  
  
6.  在 Active Directory 網域服務設定精靈，在**部署組態**頁面上，按一下**加入現有網域的網域控制站**。  
  
7.  在 **網域**，輸入網域名稱; 例如，corp.contoso.com。  
  
8.  底下**提供的認證來執行這項作業**，按一下**變更**。 在  **Windows 安全性**對話方塊方塊中，提供可以安裝其他網域控制站帳戶的使用者名稱和密碼。 若要安裝其他網域控制站，您必須是 Enterprise Admins 群組或 Domain Admins 群組的成員。 完成提供認證之後，按 [下一步]  。  
  
9. 在 **網域控制站選項**頁面上，執行下列動作：  
  
    1.  進行下列選擇：  
  
        -   **網域名稱系統 (DNS) 伺服器**"選取此選項會根據預設，讓您的網域控制站可以當做網域名稱系統 (DNS) 伺服器。 如果您不要讓網域控制站當做 DNS 伺服器，請取消選取此選項。  
  
            如果未在樹系根網域中的主要網域控制站 (PDC) 模擬器上安裝 DNS 伺服器角色，然後在其他網域控制站上安裝 DNS 伺服器的選項無法使用。 解決方法是在此情況下，您可以安裝 DNS 伺服器角色之前或之後的 AD DS 安裝。  
  
            > [!NOTE]  
            > 如果您選取要安裝 DNS 伺服器選項，您可能會收到訊息，指出無法建立 DNS 委派 DNS 伺服器，而且您應該以手動方式建立的 DNS 伺服器，以確保可靠的名稱解析的 DNS 委派。 如果您要安裝其他網域控制站的樹系根網域或樹狀目錄根網域中，您不必建立 DNS 委派。 在此情況下，按一下**是**和可忽略此訊息。  
  
        -   **通用類別目錄 (GC)** 」 預設會選取此選項。 它會新增通用類別目錄，唯讀目錄磁碟分割至網域控制站，並啟用通用類別目錄搜尋功能。  
  
        -   **唯讀網域控制站 (RODC)** 」 預設不會選取此選項。 它可讓其他網域控制站唯讀的;也就是讓網域控制站的 RODC。  
  
    2.  在 **站台名稱**，從清單中選取站台。  
  
    3.  底下**輸入目錄服務還原模式 (DSRM) 密碼**，請在**密碼**並**確認密碼**兩次，輸入強式密碼，然後按一下 [ **下一步]** 。 針對必須離線執行的工作，以 DSRM 啟動 AD DS，就必須使用此密碼。  
  
10. 在 [ **DNS 選項**頁面上，選取**更新 DNS 委派**核取方塊，如果您想要更新 DNS 委派，在角色安裝期間，然後按一下**下一步]** 。  
  
11. 在 **其他選項**頁面上，輸入或瀏覽資料庫檔案、 目錄服務記錄檔和系統磁碟區 (SYSVOL) 檔案的磁碟區及資料夾的位置。 視需要指定複寫選項，然後按一下**下一步**。  
  
12. 在 **檢閱選項**頁面上，檢閱安裝選項，然後按一下 **下一步**。  
  
13. 在 **先決條件檢查**頁面上之後會驗證必要元件，, 按一下**安裝**。  
  
14. 等候直到精靈完成組態，然後按**關閉**。  
  
15. 如果它未自動重新啟動，請重新啟動電腦。  
  
## <a name="BKMK_ConfigSG"></a>2.3. 設定安全性群組  
站台部署需要額外的安全性群組，可讓您存取 Windows 7 用戶端電腦的部署中每個進入點的 Windows 7 用戶端電腦。 如果有多個包含 Windows 7 用戶端電腦的網域，它會建議在相同的進入點的每個網域中建立安全性群組。 或者，您可以使用一個通用安全性群組，其中包含來自兩個網域的用戶端電腦。 例如，在環境中有兩個網域，如果您想要允許存取進入點 1 和 3，在 Windows 7 用戶端電腦，但不是在項目中第 2 點，然後建立包含 Windows 7 用戶端電腦，每個進入點，每個中的兩個新的安全性群組 網域。  
  
### <a name="to-configure-additional-security-groups"></a>若要設定其他的安全性群組  
  
1.  在主要網域控制站中，按一下**開始**，然後按一下**Active Directory 使用者和電腦**。  
  
2.  在主控台樹狀目錄中，以滑鼠右鍵按一下您要加入新的群組，例如 corp.contoso.com/Users 的資料夾。 指向 [新增]  ，然後按一下 [群組]  。  
  
3.  在 **新增物件-群組**對話方塊的 **群組名稱**，輸入新的群組，例如 Win7_Clients_Entrypoint1 的名稱。  
  
4.  底下**群組領域**，按一下**通用**底下**群組類型**，按一下 **安全性**，然後按一下 **確定**.  
  
5.  若要將電腦新增至新的安全性群組中，按兩下 安全性群組，然後在 **< 群組名稱 > 屬性** 對話方塊中，按一下**成員** 索引標籤。  
  
6.  在 [成員]  索引標籤上，按一下 [新增]  。  
  
7.  選取 Windows 7 電腦新增到這個安全性群組，然後再按**確定**。  
  
8.  重複此程序來建立每個進入點，所需的安全性群組。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
若要安裝 Windows 功能 「 適用於 Windows PowerShell 的 Active Directory 模組 」:  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或 「 Active Directory PowerShell 嵌入式管理單元 」 透過 OptionalFeatures 新增。  
  
如果在 Windows 7 」 或 Windows Server 2008 R2 上執行下列 cmdlet，必須要匯入 Active Directory PowerShell 模組：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要設定一個名為 Win7_Clients_Entrypoint1 安全性群組，並新增名為 CLIENT2 的用戶端電腦：  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4. 設定 GPO  
多站台的遠端存取部署需要下列群組原則物件：  
  
-   遠端存取伺服器的每個進入點所需的 GPO。  
  
-   針對每個網域的任何 Windows 8 用戶端電腦所需的 GPO。  
  
-   包含每個進入點的 Windows 7 用戶端電腦的每個網域中的 GPO 會設定為支援 Windows 7 用戶端。  
  
    > [!NOTE]  
    > 如果您沒有任何 Windows 7 用戶端電腦，您不需要建立 Gpo 適用於 Windows 7 的電腦。  
  
如果它們不 「 t 已經存在當您設定遠端存取時，精靈會自動建立必要群組原則物件。 如果您沒有建立群組原則物件的必要權限，他們必須設定遠端存取之前先建立它。 DirectAccess 系統管理員必須對 gpo 具有完整權限 （編輯 + 修改安全性 + delete）。  
  
> [!IMPORTANT]  
> 在之後以手動方式建立遠端存取 Gpo 中，您必須允許足夠的時間 Active Directory 和 Active Directory 站台與遠端存取伺服器相關聯的網域控制站的 DFS 複寫。 如果遠端存取會自動建立群組原則物件，無須等待便是必要的。  
  
若要建立群組原則物件，請參閱[建立和編輯群組原則物件](https://technet.microsoft.com/library/cc754740.aspx)。  
  
### <a name="DCMaintandDowntime"></a>網域控制站維護和停機時間  
當 PDC 模擬器，以執行網域控制站或管理伺服器 Gpo 的網域控制站遇到停機時，它不可能載入或修改 「 遠端存取 」 設定。 如果可用的其他網域控制站，這不影響用戶端連接性。  
  
載入或修改 「 遠端存取 」 設定，可以將 PDC 模擬器角色傳輸不同的網域控制站用戶端或應用程式伺服器 Gpo; 的伺服器 Gpo，變更管理伺服器 Gpo 的網域控制站。  
  
> [!IMPORTANT]  
> 只能由網域系統管理員，就可以執行這項作業。 變更主要網域控制站的影響並不侷限於遠端存取;因此，小心時傳輸 PDC 模擬器角色。  
  
> [!NOTE]  
> 之前修改網域控制站關聯，請確定，所有 「 遠端存取 」 部署中的 Gpo 沒有被複寫到所有網域控制站，網域中。 如果 GPO 未同步處理，新的組態變更可能會遺失之後修改網域控制站關聯，可能會導致設定損毀。 若要確認 GPO 同步處理，請參閱[檢查 Group Policy Infrastructure Status](https://technet.microsoft.com/library/jj134176.aspx)。  
  
#### <a name="TransferPDC"></a>若要將 PDC 模擬器角色傳輸  
  
1.  在 **開始**畫面上，輸入**dsa.msc**，然後按 ENTER 鍵。  
  
2.  在 [Active Directory 使用者和電腦] 主控台的左窗格中，以滑鼠右鍵按一下**Active Directory 使用者和電腦**，然後按一下**變更網域控制站**。 在 變更目錄伺服器 對話方塊中，按一下 **此網域控制站或 AD LDS 執行個體**，請在清單中按一下 網域控制站，將會成為新的角色持有者，然後按一下**確定**。  
  
    > [!NOTE]  
    > 如果您不想要將角色轉移到網域控制站，您必須執行此步驟。 如果您已經連接到您要將角色轉移的網域控制站，請不要執行此步驟。  
  
3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**Active Directory 使用者和電腦**，指向**所有工作**，然後按一下**操作主機**。  
  
4.  在 操作主機 對話方塊中，按一下  **PDC**索引標籤，然後再按一下**變更**。  
  
5.  按一下  **是**確認您想要轉移角色，然後按一下**關閉**。  
  
#### <a name="ChangeDC"></a>若要變更網域控制站的管理伺服器 Gpo  
  
-   執行 Windows PowerShell cmdlet`HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC`遠端存取伺服器上，指定無法連線到網域控制站名稱*ExistingDC*參數。 此命令會修改目前由該網域控制站管理的進入點的伺服器 Gpo 的網域控制站關聯。  
  
    -   若要取代的網域控制站 」 dc2.corp.contoso.com 」 無法連線到網域控制站 」 dc1.corp.contoso.com 」，執行下列作業：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   無法連線到網域控制站 」 dc1.corp.contoso.com"取代成遠端存取伺服器 」 DA1.corp.contoso.com 「 最接近的 Active Directory 站台中的網域控制站，執行下列作業：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>變更管理伺服器 Gpo 的兩個或多個網域控制站  
在最少的情況下，管理伺服器 Gpo 的兩個或多個網域控制站都無法使用。 如果發生這種情況，較多的步驟，才能變更伺服器 Gpo 的網域控制站關聯。  
  
網域控制站關聯的資訊會儲存在遠端存取伺服器的登錄和所有伺服器 Gpo 中。 在下列範例中，有兩個遠端存取伺服器，「 DA1"的兩個進入點中有 「 進入點 1 」 和 「 da2 做 」 在 「 進入點 2 」。 「 進入點 1 」 的伺服器 GPO 管理時的伺服器 GPO 的網域控制站 「 DC1"，在 「 進入點 2 」 是在中管理網域控制站 「 DC2"。 「 DC1"及"DC2"都無法使用。 第三個網域控制站仍然可以使用在網域中，「 dc3 進行複寫，「 且 」 DC1"和"DC2"的資料已複寫到 「 DC3"。  
  
![設定多站台的基礎結構](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>若要變更管理伺服器 Gpo 的兩個或多個網域控制站  
  
1.  若要取代的網域控制站 」 DC3"無法使用的網域控制站 「 DC2 」，執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令會更新網域控制站關聯性的 「 進入點 2 」 伺服器的登錄中的 GPO 可以在 da2 和 「 項目點 2 」 的伺服器 GPO 本身;不過，它不會更新 「 項目點 1 」 的伺服器 GPO 因為管理它的網域控制站無法使用。  
  
    > [!TIP]  
    > 此命令會使用繼續 algoritmus *ErrorAction*參數，以更新 「 進入點 2 」 伺服器 gpo 來說，儘管更新項目點 1 」 的伺服器 GPO 失敗。  
  
    所產生的組態是由下列圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  若要取代的網域控制站 」 DC3"無法使用的網域控制站 「 DC1"，執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令會更新 「 進入點 1 」 的伺服器 GPO 的 DA1 登錄中並 「 進入點 1 」 和 「 項目點 2 」 中伺服器 Gpo 的網域控制站關聯。 所產生的組態是由下列圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  若要同步處理 「 進入點 2 」 伺服器中項目點 1 」 伺服器 gpo 來說，執行命令，以取代 「 DC2"與"dc3 進行複寫 」，並指定其伺服器 GPO 未同步處理，在此情況下的遠端存取伺服器 GPO 的網域控制站關聯 」 DA1"，如*ComputerName*參數。  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    最後的設定是由下列圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>設定散發的最佳化  
進行組態變更時，伺服器 Gpo 傳播到遠端存取伺服器時，才套用變更。 若要減少設定散發時，遠端存取會自動選取的可寫入的網域控制站也就是超連結"<https://technet.microsoft.com/library/cc978016.aspx>「 最接近的遠端存取伺服器時建立其伺服器 GPO。  
  
在某些情況下，它可能需要以手動方式修改以獲得最佳的設定散發時管理以伺服器 GPO 的網域控制站：  
  
-   沒有任何可寫入網域控制站中的遠端存取伺服器的 Active Directory 站台時將它加入做為進入點。 可寫入網域控制站現在已加入至遠端存取伺服器的 Active Directory 站台。  
  
-   變更 IP 位址或 Active Directory 站台和子網路的變更則可能已移至不同的 Active Directory 站台的遠端存取伺服器。  
  
-   因為網域控制站上的維護工作以手動方式修改做為進入點網域控制站關聯和網域控制站現已上線。  
  
在這些情況下，執行 PowerShell cmdlet`Set-DAEntryPointDC`遠端存取伺服器上，並指定您想要最佳化使用參數的進入點名稱*EntryPointName*。 您應該執行這項操作的 GPO 資料後，才從目前用來儲存的伺服器 GPO 已經完整地複寫到所需的新網域控制站的網域控制站。  
  
> [!NOTE]  
> 之前修改網域控制站關聯，請確定，所有 「 遠端存取 」 部署中的 Gpo 沒有被複寫到所有網域控制站，網域中。 如果 GPO 未同步處理，新的組態變更可能會遺失之後修改網域控制站關聯，可能會導致設定損毀。 若要確認 GPO 同步處理，請參閱[檢查 Group Policy Infrastructure Status](https://technet.microsoft.com/library/jj134176.aspx)。  
  
若要最佳化設定散發時，執行下列其中一項動作：  
  
-   若要管理伺服器 GPO 的項目指向項目點 1 」 最接近 Active Directory 站台中的網域控制站上遠端存取伺服器 」 DA1.corp.contoso.com 」，執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   若要管理伺服器 GPO 的項目指向項目點 1 」 在網域控制站 」 dc2.corp.contoso.com 」，執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > 當修改與特定進入點相關聯的網域控制站，您必須指定屬於該進入點的遠端存取伺服器*ComputerName*參數。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [步驟 3：設定多站台部署](Step-3-Configure-the-Multisite-Deployment.md)  
-   [步驟 1：實作單一伺服器部署 「 遠端存取](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

