---
title: 步驟2設定多網站基礎結構
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6f020dc2bf5c0dc11d18e886346a98a4a40f3855
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314049"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>步驟2設定多網站基礎結構

>適用目標︰Windows Server 2012 R2、Windows Server 2012

若要設定多網站部署，必須執行幾個步驟來修改網路基礎結構設定，包括：設定額外的 Active Directory 網站和網域控制站、設定其他安全性群組，以及設定如果您未使用自動設定的 Gpo，群組原則物件（Gpo）。  
  
|工作|描述|  
|----|--------|  
|2.1. 設定其他 Active Directory 網站|設定部署的其他 Active Directory 網站。|  
|2.2. 設定其他網域控制站|視需要設定其他 Active Directory 網域控制站。|  
|2.3. 設定安全性群組|設定任何 Windows 7 用戶端電腦的安全性群組。|  
|2.4. 設定 GPO|視需要設定其他群組原則物件。|  
  
> [!NOTE]  
> 本主題包含可讓您用以自動化文中所述部分程序的範例 Windows PowerShell 指令程式。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-configure-additional-active-directory-sites"></a><a name="BKMK_ConfigAD"></a>2.1。 設定其他 Active Directory 網站  
所有進入點都可以位於單一 Active Directory 網站。 因此，在多網站設定中執行遠端存取服務器時，至少需要一個 Active Directory 網站。 如果您需要建立第一個 Active Directory 網站，或如果您想要針對多網站部署使用其他 Active Directory 網站，請使用此程式。 使用 [Active Directory 網站和服務] 嵌入式管理單元，在組織的網路中建立新的網站。  

若要完成此程式，至少需要樹系中的**Enterprise Admins**群組或樹系根域中的**Domain admins**群組的成員資格或同等許可權。 請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

如需詳細資訊，請參閱[將網站新增至樹](https://technet.microsoft.com/library/cc732761.aspx)系。  

### <a name="to-configure-additional-active-directory-sites"></a>設定其他 Active Directory 網站  
  
1.  在網域主控站上，按一下 [**開始**]，然後按一下 [ **Active Directory 網站和服務**]。  
  
2.  在 [Active Directory 網站和服務] 主控台的主控台樹中，以滑鼠右鍵按一下 [**網站**]，然後按一下 [**新增網站**]。  
  
3.  在 [**新增物件-網站**] 對話方塊的 [**名稱**] 方塊中，輸入新網站的名稱。  
  
4.  在 [**連結名稱**] 中，按一下站台連結物件，然後按兩次 **[確定]** 。  
  
5.  在主控台樹中，展開 [**網站**]，以滑鼠右鍵按一下 [**子網**]，然後按一下 [**新增子網**]。  
  
6.  在 [**新物件-子網**] 對話方塊的 [**前置**詞] 底下，輸入 IPv4 或 IPv6 子網首碼，在 [**選取此首碼的網站物件**] 清單中，按一下要與此子網產生關聯的網站，然後按一下 **[確定]** 。  
  
7.  重複步驟5和6，直到您已建立部署中所需的所有子網為止。  
  
8.  關閉 Active Directory 的網站和服務。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
若要安裝 Windows 功能「適用于 Windows PowerShell 的 Active Directory 模組」：  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
或透過 OptionalFeatures 新增「Active Directory PowerShell 嵌入式管理單元」。  
  
如果在 Windows 7 或 Windows Server 2008 R2 上執行下列 Cmdlet，則必須匯入 Active Directory PowerShell 模組：  
  
```  
Import-Module ActiveDirectory  
```  
  
若要使用內建 DEFAULTIPSITELINK 來設定名為「次要網站」的 Active Directory 網站：  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
若要設定第二個網站的 IPv4 和 IPv6 子網：  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="22-configure-additional-domain-controllers"></a><a name="BKMK_AddDC"></a>2.2。 設定其他網域控制站  
若要在單一網域中設定多網站部署，建議您在部署中的每個網站至少有一個可寫入的網域控制站。  
  
若要執行此程式，您至少必須是安裝網域控制站之網域中的 Domain Admins 群組成員。  
  
如需詳細資訊，請參閱[安裝額外的網域控制站](https://technet.microsoft.com/library/cc733027.aspx)。
  
### <a name="to-configure-additional-domain-controllers"></a>設定其他網域控制站  
  
1.  在要做為網域控制站的伺服器上，在**伺服器管理員**的 [**儀表板**] 上，按一下 [**新增角色及功能**]。  
  
2.  按三次 **[下一步]** 以移至伺服器角色選取畫面  
  
3.  在 [**選取伺服器角色**] 頁面上，選取 [ **Active Directory Domain Services**]。 出現提示時，按一下 [**新增功能**]，然後按 **[下一步]** 三次。  
  
4.  在 [確認] 頁面上，按一下 [安裝]。  
  
5.  當安裝成功完成時，請按一下 [將**此伺服器升級為網域控制站**]。  
  
6.  在 [Active Directory Domain Services Configuration Wizard] 的 [**部署**設定] 頁面上，按一下 [**將網域控制站新增至現有網域**]。  
  
7.  在 [**網域**] 中，輸入功能變數名稱;例如，corp.contoso.com。  
  
8.  在 **[提供認證以執行此**作業] 底下，按一下 [**變更**]。 在 [ **Windows 安全性**] 對話方塊中，提供可安裝其他網域控制站之帳戶的使用者名稱和密碼。 若要安裝其他網域控制站，您必須是 Enterprise Admins 群組或 Domain Admins 群組的成員。 完成提供認證之後，按 [下一步]。  
  
9. 在 [**網域控制站選項**] 頁面上，執行下列動作：  
  
    1.  進行下列選擇：  
  
        -   **網域名稱系統（DNS）伺服器**：預設會選取此選項，讓您的網域控制站可以做為網域名稱系統（DNS）伺服器。 若您不要讓網域控制站當做 DNS 伺服器，請取消選取此選項。  
  
            如果 DNS 伺服器角色未安裝在樹系根域的主域控制站（PDC）模擬器上，則無法使用在其他網域控制站上安裝 DNS 伺服器的選項。 因應措施是在此情況下，您可以在 AD DS 安裝之前或之後安裝 DNS 伺服器角色。  
  
            > [!NOTE]  
            > 如果您選取安裝 DNS 伺服器的選項，可能會收到一則訊息，指出無法建立 DNS 伺服器的 DNS 委派，而且您應該手動建立 dns 伺服器的 DNS 委派，以確保可靠的名稱解析。 如果您要在樹系根域或樹狀根域中安裝額外的網域控制站，則不需要建立 DNS 委派。 在此情況下，請按一下 **[是]** 並忽略訊息。  
  
        -   **通用類別目錄（GC）** ：預設會選取此選項。 它會新增通用類別目錄唯讀目錄分割至網域控制站，並啟用通用類別目錄搜尋功能。  
  
        -   **唯讀網域控制站（RODC）** ：預設不會選取此選項。 它會使額外的網域控制站變成隻讀;也就是說，它會使網域控制站成為 RODC。  
  
    2.  在 [**網站名稱**] 中，從清單中選取網站。  
  
    3.  在 **[輸入目錄服務還原模式（DSRM）密碼]** 下的 [**密碼**] 和 [**確認密碼**] 中，輸入強式密碼兩次，然後按 **[下一步]** 。 此密碼必須用來針對必須離線執行的工作，以 DSRM 啟動 AD DS。  
  
10. 如果您想要在角色安裝期間更新 DNS 委派，請在 [ **DNS 選項**] 頁面上選取 [**更新 dns 委派**] 核取方塊，然後按 **[下一步]** 。  
  
11. 在 [**其他選項**] 頁面上，輸入或流覽資料庫檔案、目錄服務記錄檔和系統磁碟區（SYSVOL）檔案的磁片區和資料夾位置。 視需要指定複寫選項，然後按 **[下一步]** 。  
  
12. 在 [**審查選項**] 頁面上，檢查安裝選項，然後按 **[下一步]** 。  
  
13. 在 [**必要條件檢查**] 頁面上，驗證必要條件之後，按一下 [**安裝**]。  
  
14. 等到 wizard 完成設定，然後按一下 [**關閉**]。  
  
15. 如果電腦未自動重新開機，請將它重新開機。  
  
## <a name="23-configure-security-groups"></a><a name="BKMK_ConfigSG"></a>2.3。 設定安全性群組  
針對部署中的每個進入點允許存取 Windows 7 用戶端電腦的 Windows 7 用戶端電腦，多網站部署需要一個額外的安全性群組。 如果有多個網域包含 Windows 7 用戶端電腦，建議您在每個網域中為相同的進入點建立一個安全性群組。 或者，也可以使用一個包含這兩個網域之用戶端電腦的通用安全性群組。 例如，在有兩個網域的環境中，如果您想要允許存取進入點1和3中的 Windows 7 用戶端電腦，而不是進入點2，則請建立兩個新的安全性群組，以包含每個中每個進入點的 Windows 7 用戶端電腦。林中.  
  
### <a name="to-configure-additional-security-groups"></a>設定其他安全性群組  
  
1.  在網域主控站上，按一下 [**開始**]，然後按一下 [ **Active Directory 使用者和電腦**]。  
  
2.  在主控台樹中，以滑鼠右鍵按一下您要新增群組的資料夾，例如 corp.contoso.com/Users。 指向 [新增]，然後按一下 [群組]。  
  
3.  在 [**新增物件-群組**] 對話方塊的 [**組名**] 底下，輸入新群組的名稱，例如 Win7_Clients_Entrypoint1。  
  
4.  在 [**群組領域**] 底下，按一下 [**通用**]，在 [**群組類型**] 底下按一下 [**安全性**]，然後按一下 **[確定]**  
  
5.  若要將電腦新增至新的安全性群組，請按兩下安全性群組，然後在 [ **< Group_Name >** 內容] 對話方塊中，按一下 [**成員**] 索引標籤。  
  
6.  在 [成員] 索引標籤上，按一下 [新增]。  
  
7.  選取要新增到此安全性群組的 Windows 7 電腦，然後按一下 **[確定]** 。  
  
8.  重複此程式，視需要為每個進入點建立一個安全性群組。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
若要安裝 Windows 功能「適用于 Windows PowerShell 的 Active Directory 模組」：  
  
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
  
## <a name="24-configure-gpos"></a><a name="ConfigGPOs"></a>2.4。 設定 GPO  
多網站遠端存取部署需要下列群組原則物件：  
  
-   適用于遠端存取服務器之每個進入點的 GPO。  
  
-   適用于每個網域的任何 Windows 8 用戶端電腦的 GPO。  
  
-   每個網域中包含 Windows 7 用戶端電腦的 GPO，分別適用于設定為支援 Windows 7 用戶端的每個進入點。  
  
    > [!NOTE]  
    > 如果您沒有任何 Windows 7 用戶端電腦，則不需要為 Windows 7 電腦建立 Gpo。  
  
當您設定「遠端存取」時，嚮導會自動建立必要的群組原則物件（如果它們不存在）。 如果您沒有建立群組原則物件的必要許可權，就必須在設定遠端存取之前先建立它們。 DirectAccess 系統管理員必須擁有 Gpo 的完整許可權（[編輯 + 修改安全性 + 刪除]）。  
  
> [!IMPORTANT]  
> 手動建立遠端存取的 Gpo 之後，您必須在與遠端存取服務器相關聯的 Active Directory 網站中，允許 Active Directory 和 DFS 複寫有足夠的時間來進行網域控制站。 如果遠端存取會自動建立群組原則物件，則不需要等待時間。  
  
若要建立群組原則物件，請參閱[建立和編輯群組原則物件](https://technet.microsoft.com/library/cc754740.aspx)。  
  
### <a name="domain-controller-maintenance-and-downtime"></a><a name="DCMaintandDowntime"></a>網域控制站維護與停機時間  
當以 PDC 模擬器執行的網域控制站或管理伺服器 Gpo 的網域控制站發生停機時，無法載入或修改遠端存取設定。 如果有其他網域控制站可供使用，這不會影響用戶端連線能力。  
  
若要載入或修改遠端存取設定，您可以將 PDC 模擬器角色轉移至用戶端或應用程式伺服器 Gpo 的不同網域控制站;針對 [伺服器 Gpo]，變更管理伺服器 Gpo 的網域控制站。  
  
> [!IMPORTANT]  
> 這項作業只能由網域系統管理員執行。 變更主域控制站的影響並不限於遠端存取;因此，在傳送 PDC 模擬器角色時請務必小心。  
  
> [!NOTE]  
> 修改網域控制站關聯之前，請確定遠端存取部署中的所有 Gpo 都已複寫到網域中的所有網域控制站。 如果 GPO 未同步處理，可能會在修改網域控制站關聯之後遺失最近的設定變更，這可能會導致設定損毀。 若要確認 GPO 同步處理，請參閱[檢查群組原則的基礎結構狀態](https://technet.microsoft.com/library/jj134176.aspx)。  
  
#### <a name="to-transfer-the-pdc-emulator-role"></a><a name="TransferPDC"></a>若要轉移 PDC 模擬器角色  
  
1.  在 [**開始**] 畫面上，輸入**dsa.msc**，然後按 enter。  
  
2.  在 [Active Directory 使用者和電腦] 主控台的左窗格中，以滑鼠右鍵按一下 [ **Active Directory 使用者和電腦**]，然後按一下 [**變更網域控制站**]。 在 [變更目錄伺服器] 對話方塊上，按一下 [**此網域控制站] 或 [AD LDS 實例**]，在清單中按一下將成為新角色持有者的網域控制站，然後按一下 **[確定]** 。  
  
    > [!NOTE]  
    > 如果您不在要轉移角色的網域控制站上，就必須執行此步驟。 如果您已經連線到要轉移角色的網域控制站，請勿執行此步驟。  
  
3.  在主控台樹中，以滑鼠右鍵按一下 [ **Active Directory 使用者和電腦**]，指向 [**所有**工作]，然後按一下 [**操作主機**]。  
  
4.  在 [操作主機] 對話方塊中，按一下 [ **PDC** ] 索引標籤，然後按一下 [**變更**]。  
  
5.  按一下 **[是]** 確認您要轉移角色，然後按一下 [**關閉**]。  
  
#### <a name="to-change-the-domain-controller-that-manages-server-gpos"></a><a name="ChangeDC"></a>變更管理伺服器 Gpo 的網域控制站  
  
-   在遠端存取服務器上執行 Windows PowerShell Cmdlet `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC`，並為*ExistingDC*參數指定無法連線的網域控制站名稱。 此命令會針對目前由該網域控制站管理的進入點，修改其伺服器 Gpo 的網域控制站關聯。  
  
    -   若要以網域控制站 "dc2.corp.contoso.com" 取代無法連線的網域控制站 "dc1.corp.contoso.com"，請執行下列動作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   若要以最接近的 Active Directory 網站中的網域控制站，將無法連線的網域控制站 "dc1.corp.contoso.com" 取代為遠端存取服務器 "DA1.corp.contoso.com"，請執行下列動作：  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="change-two-or-more-domain-controllers-that-manage-server-gpos"></a><a name="ChangeTwoDCs"></a>變更兩個或多個管理伺服器 Gpo 的網域控制站  
在最少的情況下，管理伺服器 Gpo 的兩個或多個網域控制站無法使用。 如果發生這種情況，則需要更多步驟來變更伺服器 Gpo 的網域控制站關聯。  
  
網域控制站關聯資訊會同時儲存在遠端存取服務器的登錄和所有伺服器 Gpo 中。 在下列範例中，有兩個具有兩個遠端存取服務器的進入點： "DA1" （在 "entry point 1" 中）和 "DA2" （在 "Entry point 2" 中）。 「進入點1」的伺服器 GPO 是在網域控制站 "DC1" 中進行管理，而「進入點2」的伺服器 GPO 則是在網域控制站 "DC2" 中管理。 「DC1」和「DC2」都無法使用。 第三個網域控制站在網域中仍然可用，"DC3"，而來自 "DC1" 和 "DC2" 的資料已複寫至「DC3」。  
  
![設定多網站基礎結構](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>若要變更兩個或更多個管理伺服器 Gpo 的網域控制站  
  
1.  若要以網域控制站 "DC3" 取代無法使用的網域控制站 "DC2"，請執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令會更新 DA2 登錄中和「進入點2」伺服器 GPO 本身的「進入點2」伺服器 GPO 的網域控制站關聯;不過，它不會更新「進入點1」伺服器 GPO，因為管理它的網域控制站無法使用。  
  
    > [!TIP]  
    > 此命令會使用*ErrorAction*參數的 Continue 值，這會更新「進入點2」伺服器 gpo，儘管無法更新「進入點1」伺服器 gpo。  
  
    產生的設定如下圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  若要以網域控制站 "DC3" 取代無法使用的網域控制站 "DC1"，請執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    此命令會更新 DA1 登錄中的「進入點1」伺服器 GPO 的網域控制站關聯，以及「進入點1」和「進入點2」伺服器 Gpo。 產生的設定如下圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  若要同步「進入點1」伺服器 GPO 中「進入點2」伺服器 GPO 的網域控制站關聯，請執行命令以「DC3」取代 "DC2"，並指定其伺服器 GPO 未同步處理的遠端存取服務器（在此案例中為 "DA1"），代表*ComputerName*參數。  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    最後的設定如下圖所示。  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="optimization-of-configuration-distribution"></a><a name="ConfigDistOptimization"></a>設定散發的優化  
進行設定變更時，只有在伺服器 Gpo 傳播至遠端存取服務器之後，才會套用變更。 若要減少設定散發時間，遠端存取會自動選取可寫入的網域控制站，這是在建立其伺服器 GPO 時，與遠端存取服務器最接近的超連結「<https://technet.microsoft.com/library/cc978016.aspx>」。  
  
在某些情況下，可能需要手動修改管理伺服器 GPO 的網域控制站，以便優化設定散發時間：  
  
-   在遠端存取服務器的 Active Directory 網站中，沒有可寫入的網域控制站做為進入點。 可寫入的網域控制站現在會新增至遠端存取服務器的 Active Directory 網站。  
  
-   IP 位址變更，或 Active Directory 網站和子網變更可能已將遠端存取服務器移到不同的 Active Directory 網站。  
  
-   由於網域控制站上的維護工作，已手動修改進入點的網域控制站關聯，現在網域控制站已重新上線。  
  
在這些案例中，請在遠端存取服務器上執行 PowerShell Cmdlet `Set-DAEntryPointDC`，並使用參數*EntryPointName*來指定您想要優化的進入點名稱。 只有在目前儲存伺服器 GPO 的網域控制站的 GPO 資料已完全複寫到所需的新網域控制站之後，您才應該執行此動作。  
  
> [!NOTE]  
> 修改網域控制站關聯之前，請確定遠端存取部署中的所有 Gpo 都已複寫到網域中的所有網域控制站。 如果 GPO 未同步處理，可能會在修改網域控制站關聯之後遺失最近的設定變更，這可能會導致設定損毀。 若要確認 GPO 同步處理，請參閱[檢查群組原則的基礎結構狀態](https://technet.microsoft.com/library/jj134176.aspx)。  
  
若要優化設定散發時間，請執行下列其中一項動作：  
  
-   若要在最近 Active Directory 網站的網域控制站上管理進入點「進入點1」的伺服器 GPO 至遠端存取服務器 "DA1.corp.contoso.com"，請執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   若要在網域控制站 "dc2.corp.contoso.com" 上管理進入點「進入點1」的伺服器 GPO，請執行下列命令：  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > 修改與特定進入點相關聯的網域控制站時，您必須指定屬於*ComputerName*參數之該進入點成員的遠端存取服務器。  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱  
  
-   [步驟3：設定多網站部署](Step-3-Configure-the-Multisite-Deployment.md)  
-   [步驟1：執行單一伺服器遠端存取部署](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

