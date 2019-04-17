---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: "部署中央存取原則（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>部署中央存取原則（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在本案例中，指定的中央存取原則的需求，讓他們可以保護保存的財經資訊儲存檔案的伺服器上的中央的資訊安全使用財經部門安全性作業。 從每個國家/地區的保存的財經資訊可以存取為唯讀財經員工相同的國家/地區。 群組中央財經系統管理員可以存取的財經資訊的所有國家/地區。  
  
部署的中央存取原則，包括下列階段：  
  
|階段|描述  
|---------|---------------  
|[規劃：找出需原則和部署所需的設定](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|找出需原則和部署所需的設定。 
|[實作：設定原則和元件](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|設定原則和元件。  
|[部署的中央存取原則](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|部署的原則。  
|[維護：變更與階段原則](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|變更原則和執行。 
  
## <a name="BKMK_1.1"></a>設定測試環境  
在您開始之前，您需要設定實驗室測試本案例。 設定實驗室的步驟會詳述在[附錄 b 設定好的測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)。  
  
## <a name="BKMK_1.2"></a>規劃：找出需原則和部署所需的設定  
本節高階一系列，以協助您的部署的規劃階段中的步驟執行。  
  
||步驟|範例|  
|-|--------|-----------|  
|1.1|商務用判斷，所需的中央存取原則|為保護儲存檔案的伺服器上的財經資訊，財經部門安全性作業正在使用指定的中央存取原則需要中央的資訊安全。|  
|1.2|快速存取原則|財經部門的成員，應該只讀取財經文件。 財經部門的成員應該只存取自己的國家/地區中的文件。 只有財經系統管理員必須寫入存取。 將會例外允許 FinanceException 群組成員。 此群組的可以朗讀存取。|  
|1.3|快速存取 Windows Server 2012 建構原則|目標：<br /><br />-Resource.Department 包含財經<br /><br />存取的規則：<br /><br />-允許讀取 User.Country=Resource.Country 和 User.department = Resource.Department<br />-允許完全控制 User.MemberOf(FinanceAdmin)<br /><br />例外：<br /><br />讓朗讀的 memberOf(FinanceException)|  
|1.4|判斷檔案屬性所需的原則|使用標記檔案：<br /><br />-部門<br />-國家/地區|  
|1.5|判斷群組所需的原則與宣告類型|宣告類型：<br /><br />-國家/地區<br />-部門<br /><br />使用者群組：<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|確定要將這項原則套用伺服器|所有財經檔案伺服器上套用原則。|  
  
## <a name="BKMK_1.3"></a>實作：設定原則和元件  
本節提供的範例部署財經文件的中央存取原則。  
  
|否]|步驟|範例|  
|------|--------|-----------|  
|2.1|建立宣告類型|建立理賠要求下列類型：<br /><br />-部門<br />-國家/地區|  
|2.2|建立資源屬性|建立以及下列資源屬性：<br /><br />-部門<br />-國家/地區|  
|2.3|設定中央存取規則|建立財經文件規則包含判斷一節中的原則。|  
|2.4|設定的中央存取原則（端點）|建立稱為財經原則筆蓋，並新增財經文件規則該筆蓋。|  
|2.5|若要將檔案伺服器目標中央存取原則|發行的檔案伺服器的財經原則筆蓋。|  
|2.6|讓 \ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \。|讓 \ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \ 的 contoso.com 的。|  
  
下列程序，您可以建立兩個宣告類型：國家/地區和部門。  
  
#### <a name="to-create-claim-types"></a>若要建立宣告類型  
  
1.  為 contoso\administrator，密碼，在開放伺服器 DC1 HYPER-V 管理員和登入**pass@word1**。  
  
2.  打開 Active Directory 系統管理員中心。  
  
3.  按一下**樹檢視] 圖示**，展開**動態存取控制**，然後選取**宣告類型**。  
  
    以滑鼠右鍵按一下**宣告類型**，按一下 [**新**，然後按一下 [**宣告類型**。  
  
    > [!TIP]  
    > 您也可以開放**建立宣告類型：**視窗中的**工作**窗格。 在**工作**窗格中，按**新**，，然後按一下 [**理賠要求輸入**。  
  
4.  在**來源屬性**清單中，向下捲動清單的屬性，按**部門**。 這應會填入**顯示名稱**欄位的**部門**。 按一下**[確定]**。  
  
5.  在**工作**窗格中，按**新**，，然後按一下 [**理賠要求輸入**。  
  
6.  在**來源屬性**清單中，向下捲動清單的屬性，然後按**c**（國家的名稱）屬性。 在**顯示名稱**欄位中，輸入**國家/地區**。  
  
7.  在**建議值**區段中，選取**下列值的建議：**，然後按一下 [**新增**。  
  
8.  在**值**和**顯示名稱**欄位，輸入**美國**，然後按一下 [ **[確定]**。  
  
9. 重複執行上述步驟。 在**新增建議值**對話方塊中，輸入**JP**中**值**和**顯示名稱**的欄位，然後再按一下**[確定]**。  
  
![方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> 您可以使用 Windows PowerShell 歷史檢視器中 Active Directory 管理中心查詢 Active Directory 管理中心您執行每個程序的 Windows PowerShell cmdlet。 如需詳細資訊，請查看[Windows PowerShell 歷史檢視器](https://technet.microsoft.com/library/hh831702)  
  
下一個步驟是建立資源屬性。 下列程序建立會自動新增至通用的資源屬性清單上網域控制站的資源屬性，讓該檔案伺服器即可。  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>若要建立以及預先建立的資源屬性  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [**樹檢視**。 展開**動態存取控制**，然後選取 [**資源屬性**。  
  
2.  以滑鼠右鍵按一下**資源屬性**，按一下 [**新**，然後按一下 [**參考資源屬性**。  
  
    > [!TIP]  
    > 您也可以選擇的資源屬性**工作**窗格。 按一下**新增]**，然後按一下 [**參考資源屬性**。  
  
3.  在**選擇来分享的理賠要求類型建議值清單**，按一下 [**國家/地區**。  
  
4.  在**顯示名稱**欄位中，輸入**國家/地區**，然後按一下 [ **[確定]**。  
  
5.  按兩下**資源屬性**清單中，向下捲動到**部門**資源屬性。 按一下滑鼠右鍵，然後按一下**讓**。 這會讓建**部門**資源屬性。  
  
6.  在**資源屬性**清單上的 Active Directory 管理中心瀏覽窗格中，您現在將會有兩個讓的資源屬性：  
  
    -   國家/地區  
  
    -   部門  
  
![方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
下一個步驟是建立中央存取規則定義人可以存取的資源。 本案例中的企業規則︰  
  
-   財經文件只可以讀取財經部門的成員。  
  
-   財經部門的成員可以存取自己的國家/地區中的文件。  
  
-   只有財經系統管理員可以存取寫入。  
  
-   我們將允許例外 FinanceException 群組成員。 此群組的可以朗讀存取。  
  
-   系統管理員和文件擁有者仍然可以存取的完整。  
  
或快速與 Windows Server 2012 建構的規則：  
  
目標：Resource.Department 包含財經  
  
存取的規則：  
  
-   讓朗讀 User.Country=Resource.Country 和 User.department = Resource.Department  
  
-   允許完全控制 User.MemberOf(FinanceAdmin)  
  
-   讓朗讀 User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>若要建立的中央存取規則  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [**樹檢視**，請選取**動態存取控制**，然後按一下 [**中央存取規則**。  
  
2.  以滑鼠右鍵按一下**中央存取規則**，按一下 [**新**，然後按一下 [**中央存取規則**。  
  
3.  在**名稱**欄位中，輸入**財經文件規則**。  
  
4.  在**目標資源**區段中，按一下 [**編輯**，在**中央存取規則**] 對話方塊中，按一下**[新增條件**。 新增下列條件：   
    [**資源**][**部門**][**等於**][**Value**][**財經**]，然後按**[確定]**。  
  
5.  在**權限]**區段中，選取**為目前的權限的權限之後使用**，按一下 [**編輯**，在**權限] 的進階安全性設定**索引標籤中按一下**新增**。  
  
    > [!NOTE]  
    > **使用下列的使用權限建議權限]**選項可讓您建立臨時原則。 如需有關如何維持參考：變更與階段本主題中的原則一節。  
  
6.  在**權限的項目權限**對話方塊中，按一下 [**選取主體**，輸入**Authenticated Users**，，然後按一下**[確定]**。  
  
7.  在**權限的項目權限**對話方塊中，按**新增條件**，然後新增下列條件：   
    [**User**][**國家/地區**][**Any of**][**資源**][**國家/地區**]   
     按一下**[新增條件**。   
     [**And**]   
    按一下 [**使用者**] [**部門**] [**的任何**] [**資源**] [**部門**]。 設定**權限]**以**朗讀**。  
  
8.  按一下**[確定]**，然後按一下 [**新增]**。 按一下**選取主體**，輸入**FinanceAdmin**，然後按一下 [ **[確定]**。  
  
9. 選取 [**修改、讀取並執行、朗讀、寫入**的權限，然後再按一下**[確定]**。  
  
10. 按一下**新增**，按一下**選取主體**，輸入**FinanceException**，然後按一下**[確定]**。 選取要權限]**朗讀**和**讀取和執行**。  
  
11. 按一下**[確定]**以完成，並回到 Active Directory 管理中心三次。  
  
    ![方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
    下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
 
    $countryClaimType = Get-ADClaimType 國家/地區  
    $departmentClaimType = Get-ADClaimType 部門  
    $countryResourceProperty = Get-ADResourceProperty 國家/地區  
    $departmentResourceProperty = Get-ADResourceProperty 部門  
    $currentAcl = O:SYG:SYD:AR(A;;」FA;;W) (A;FA;;BA)（A; 0x1200a9;;S-1-5-21-1787166779-1215870801-2157059049-1113)（A; 0x1301bf;;S-1-5-21-1787166779-1215870801-2157059049-1112) (A;FA;;SY) (XA; 0x1200a9;;AU;((@USER." + $countryClaimType.Name +」Any_of @RESOURCE。」 + $countryResourceProperty.Name +」) 與與 (@USER。」 + $departmentClaimType.Name +」Any_of @RESOURCE。」 + $departmentResourceProperty.Name +」)))」  
    $resourceCondition =」(@RESOURCE。」 + $departmentResourceProperty.Name +」包含 {`"Finance`「})」  
    New-ADCentralAccessRule「財務文件規則」CurrentAcl $currentAcl-ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> 在上面 cmdlet 範例中，安全性識別碼群組 FinanceAdmin (Sid) 使用者在建立時判斷並將會在您的範例不同。 例如，提供的 SID 的值 (S-1-5-21-1787166779-1215870801-2157059049-1113) 針對您想要建立您的部署 FinanceAdmin 群組的實際 sid 更換 FinanceAdmins 需求。 您可以使用 Windows PowerShell 來尋找此群組的 SID 值、指派給變數，該值，然後使用變數以下。 如需詳細資訊，請查看[Windows PowerShell 秘訣：處理 Sid](https://go.microsoft.com/fwlink/?LinkId=253545)。  
  
您現在應該會有的中央存取規則，可讓使用者從相同的國家/地區和相同部門存取文件。 規則允許 FinanceAdmin 群組編輯文件，並可讓朗讀文件 FinanceException 群組。 此規則目標只有歸類為財經文件。  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>加入的中央存取原則的中央存取規則  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [**動態存取控制**，然後按**的中央存取原則**。  
  
2.  在**工作**窗格中，按一下**新**，然後按一下 [**的中央存取原則**。  
  
3.  在**建立的中央存取原則：**，輸入**原則財經**中**名稱**方塊。  
  
4.  在**成員中央存取規則**，按一下 [**新增]**。  
  
5.  按兩下**財經文件規則**來新增它**新增下列中央存取規則**清單，然後再按**[確定]**。  
  
6.  按一下**[確定]**來完成。 您現在應該會有名財經原則的中央存取原則。  
  
    ![方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
    下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>將檔案伺服器上套用的中央存取原則，使用群組原則  
  
1.  在**[開始]**畫面上，在**搜尋**方塊中，輸入**群組原則管理**。 按兩下**群組原則管理**。  
  
    > [!TIP]  
    > 如果**顯示系統管理工具]**已停用設定，**系統管理工具]**資料夾和內容將不會出現在 [**設定**結果。  
  
    > [!TIP]  
    > 在您的實際執行環境，您應該建立檔案伺服器組織單位（組織單位）及您要將這項原則套用到此組織單位，加入您的所有檔案伺服器。 然後，您可以建立群組原則，並新增這組織單位該原則。  
  
2.  在此步驟，您可以編輯群組原則物件您建立[組建網域控制站](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build)一節包含您建立的中央存取原則測試環境中。 群組原則管理編輯器] 中，瀏覽與 (在此範例中 contoso.com) 網域中，選取組織單位：**群組原則管理**，**的樹系：contoso.com**、**網域**、**contoso.com**、**Contoso**，**FileServerOU**。  
  
3.  以滑鼠右鍵按一下**FlexibleAccessGPO**，然後按**編輯**。  
  
4.  在群組原則編輯器] 管理視窗中，瀏覽至**電腦設定**，展開**原則**，展開**Windows 設定**，然後按一下**的安全性設定**。  
  
5.  展開**檔案系統**，以滑鼠右鍵按一下**的中央存取原則**，然後按一下 [**管理中央存取原則**。  
  
6.  在**中央存取原則設定**對話方塊方塊中，將新增**財經原則**，然後按一下 [ **[確定]**。  
  
7.  向下捲動**進階稽核原則設定**，展開它。  
  
8.  展開**稽核原則**，然後選取 [**物件存取]**。  
  
9. 按兩下**稽核中央存取原則階段**。 選取所有三個核取方塊，然後按一下**[確定]**。 這個步驟可讓您收到稽核中央存取臨時原則相關的事件系統。  
  
10. 按兩下**稽核檔案系統摘要**。 選取 [所有三個核取方塊，然後按一下**[確定]**。  
  
11. 關閉 「 群組原則管理編輯器。 您現在包含在群組原則的中央存取原則。  
  
網域的網域控制站提供宣告或裝置授權資料，為網域控制站需要以支援動態存取控制設定。  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>若要讓支援宣告和 contoso.com 複合驗證  
  
1.  打開群組原則管理，請按一下**contoso.com**，然後按一下 [**網域控制站**。  
  
2.  以滑鼠右鍵按一下**預設網域控制站原則**，然後按**編輯**。  
  
3.  在群組原則編輯器] 管理視窗中，按兩下 [**電腦設定**，按兩下 [**原則**，按兩下 [**系統管理範本]**，按兩下**系統**，，然後按兩下 [ **KDC**。  
  
4.  按兩下**\ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \**。 在**\ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \**對話方塊中，按**啟用**，然後選取**支援**的**選項**下拉式清單。 （您需要，可讓使用者宣告用於中央存取原則這項設定）。  
  
5.  關閉**群組原則管理**。  
  
6.  打開命令提示字元中，輸入`gpupdate /force`。  
  
## <a name="BKMK_1.4"></a>部署的中央存取原則  
  
||步驟|範例|  
|-|--------|-----------|  
|3.1|將檔案伺服器上的適當的共用資料夾筆蓋。|將檔案伺服器的適當共用資料夾的中央存取原則。|  
|3.2|確認已正確設定好存取。|檢查使用者從不同的國家與部門的存取權。|  
  
在此步驟您將指派給檔案伺服器的中央存取原則。 您將會登入，以接收您所建立的上一個步驟的中央存取原則檔案伺服器，指定原則的共用資料夾。  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>若要將檔案伺服器的中央存取原則  
  
1.  在 [HYPER-V 管理員連接伺服器 1。 登入密碼使用 contoso\administrator 伺服器：**pass@word1**。  
  
2.  打開提升權限的命令提示字元中，輸入： **gpupdate /force**。 這樣可確保您的群組原則變更，才會生效，在您的伺服器上。  
  
3.  您也需要重新整理全球 Active directory 資源屬性。 打開提升權限的 Windows PowerShell 視窗，並輸入`Update-FSRMClassificationpropertyDefinition`。 按一下 [輸入]，然後關閉 [Windows PowerShell。  
  
    > [!TIP]  
    > 您也可以登入該檔案伺服器更新全球的資源屬性。 重新整理全球檔案伺服器的資源屬性，請執行下列動作  
    >   
    > 1.  若要檔案伺服器 1 contoso\administrator，以使用密碼登入**pass@word1**。  
    > 2.  打開檔案伺服器資源管理員。 若要打開檔案伺服器資源管理員中，按一下 [ **[開始]**，輸入**檔案伺服器資源管理員**，然後按一下 [**檔案伺服器資源管理員**。  
    > 3.  檔案伺服器資源管理員] 中，按一下**檔案分類管理]**，以滑鼠右鍵按一下**分類屬性**，然後按一下 [**重新整理**。  
  
4.  打開 Windows 檔案總管]，並在左窗格中，按一下 [磁碟機上按一下滑鼠右鍵 D.**財經文件**資料夾，然後按**屬性**。  
  
5.  按一下**分類**索引標籤上，按一下 [**國家/地區**，]，然後選取**美國**在**值**欄位。  
  
6.  按一下**部門**，然後選取**財經**中**值**欄位，然後按一下**套用]**。  
  
    > [!NOTE]  
    > 請記住的中央存取原則設定為部門的財經的目標檔案。 上述步驟標記所有國家/地區和部門屬性的資料夾中的文件。  
  
7.  按一下**安全性**索引標籤，然後按一下 [**進階]**。 按一下**的中央原則**索引標籤。  
  
8.  按一下**變更**，請選取**原則財經**從下拉式功能表，然後按一下 [**套用]**。 您可以查看**財經文件規則**列中的原則。 展開以檢視所有的設定建立規則 Active Directory 中的權限的項目。  
  
9. 按一下**[確定]**以返回 [Windows 檔案總管]。  
  
在下一個步驟中，確定已正確設定好存取。  需要有適當部門屬性設定帳號（設定使用 Active Directory 管理中心）。 檢視有效的新原則結果的最簡單方式是使用**有效的存取**索引標籤中 [Windows 檔案總管]。 **有效的存取**索引標籤顯示特定的帳號的存取權限。  
  
#### <a name="to-examine-the-access-for-various-users"></a>若要檢查各種不同的使用者存取  
  
1.  在 [HYPER-V 管理員連接伺服器 1。 使用 contoso\administrator 登入伺服器。 瀏覽至 D:\ Windows 檔案總管] 中。 以滑鼠右鍵按一下**財經文件**資料夾，然後再按**屬性**。  
  
2.  按一下**安全性**索引標籤上，按一下 [**進階**，然後按一下 [**有效的存取**索引標籤。  
  
3.  若要檢查使用者的權限，請按一下**選取一位使用者**，並輸入使用者名稱，然後按一下 [**檢視有效的存取**來查看有效的存取權限。 例如：  
  
    -   Myriam Delesalle (MDelesalle) 財務部門且應該會有讀取的資料夾。  
  
    -   英哩 Reid (MReid) FinanceAdmin 群組的成員，並可以修改存取資料夾。  
  
    -   不財務部門; Esther 耶 (EValle)不過，她 FinanceException 群組的成員，應該朗讀存取。  
  
    -   Maira Wenzel (MWenzel) 財經部門並不是不的其中一個成員 FinanceAdmin 或 FinanceException 群組。 她不應該會有任何存取的資料夾。  
  
    請注意，最後一欄名為**存取受限於**在視窗有效的存取。 此欄位會告訴您的 gates 的影響的權限的人員。 若是如此，共用與 NTFS 權限允許所有使用者完整控制權。 不過的中央存取原則限制存取根據您之前所設定的規則。  
  
## <a name="BKMK_1.5"></a>維護：變更與階段原則  
  
||||  
|-|-|-|  
|數字|步驟|範例|  
|4.1|設定為戶端裝置宣告|若要讓裝置宣告的群組原則設定的設定|  
|4.2|讓裝置理賠要求。|讓裝置的國家/地區宣告類型。|  
|4.3|加入現有的中央存取規則修改您想要執行的原則。|修改財經文件以新增原則臨時規則。|  
|4.4|檢視臨時原則的結果。|檢查 Ester Velle 權限。|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>若要設定群組原則設定可讓宣告的裝置  
  
1.  登入 DC1，群組原則管理開放，按一下**contoso.com**，按一下 [**預設網域原則**，以滑鼠右鍵按一下，然後選取**編輯**。  
  
2.  在群組原則編輯器] 管理視窗中，瀏覽至**電腦設定**，**原則**、**系統管理範本]**、**系統**，**Kerberos**。  
  
3.  選取 [ **Kerberos client 支援宣告、複合驗證以及 Kerberos 保護 \**，按一下 [**支援**。  
  
#### <a name="to-enable-a-claim-for-devices"></a>若要讓裝置宣告  
  
1.  為 contoso\Administrator，密碼，在開放伺服器 DC1 HYPER-V 管理員和登入**pass@word1**。  
  
2.  從**工具**功能表上，開放 Active Directory 管理中心。  
  
3.  按一下**樹檢視**，展開 [**動態存取控制**，按兩下 [**取得類型**，按兩下**國家/地區**取得。  
  
4.  在**適用於下列類別發出宣告這種類型的**，請選取**電腦**核取方塊。 按一下**[確定]**。   
    同時**使用者**和**電腦**現在選取核取方塊。 國家/地區宣告現在可以搭配使用者除了裝置。  
  
下一個步驟是建立臨時的原則。 臨時原則用於監視它讓您的新的原則項目效果。 在下列步驟中，您將會建立臨時的原則項目及監視會影響您的共用資料夾。  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>若要建立臨時原則規則，並將它新增到的中央存取原則  
  
1.  為 contoso\Administrator，密碼，在開放伺服器 DC1 HYPER-V 管理員和登入**pass@word1**。  
  
2.  打開 Active Directory 系統管理員中心。  
  
3.  按一下**樹檢視**，展開 [**動態存取控制**，然後選取**中央存取規則**。  
  
4.  以滑鼠右鍵按一下**財經文件規則**，然後按一下 [**屬性**。  
  
5.  在**提議的權限**區段中，選取**讓執行設定的權限**核取方塊、按一下 [**編輯**，，然後按一下**新增**。 在**權限提議的權限的項目**視窗中，按一下 [**選取主體**連結，輸入**Authenticated Users**，，然後按一下**[確定]**。  
  
6.  按一下**[新增條件**連結，並新增下列條件：   
     [**User**][**國家/地區**][**Any of**][**資源**][**國家/地區**]。  
  
7.  按一下**[新增條件**，然後新增下列條件：  
    [**And**]   
     [**裝置**][**國家/地區**][**Any of**][**資源**][**國家/地區**]  
  
8.  按一下**[新增條件**，然後新增下列條件。  
    [並]   
     [**User**][**Group**][**的任何成員**][**值**] \ (**FinanceException**)  
  
9. 若要設定 FinanceException，群組中，按一下 [**新增項目**在**選取 [使用者、電腦、或群組**] 視窗中，輸入**FinanceException**。  
  
10. 按一下**權限]**，請選取**完全控制**，並按一下 [ **[確定]**。  
  
11. 在 [建議的權限] 視窗進階安全性設定，請選取**FinanceException**，按一下 [**移除**。  
  
12. 按一下**[確定]**以完成兩次。  
  
![方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> 上述 cmdlet 範例中，在伺服器值反映伺服器實驗室測試環境中。 您可以使用 Windows PowerShell 歷史檢視器查詢 Active Directory 管理中心您執行每個程序的 Windows PowerShell cmdlet。 如需詳細資訊，請查看[Windows PowerShell 歷史檢視器](https://technet.microsoft.com/library/hh831702)  
  
在這個提議的權限設定時，FinanceException 群組成員必須完整存取權的檔案從他們自己的國家/地區時他們透過相同的國家/地區的文件從裝置存取。 稽核項目提供檔案伺服器安全性登入時財經人員嘗試存取檔案。 不過的安全性設定未執行直到從臨時升級原則。  
  
在下一個程序，檢查臨時原則的結果。 您存取共用的資料夾與權限根據目前規則的使用者名稱。 Esther 耶 (EValle) FinanceException 的成員，以及她目前已讀取權限。 根據我們臨時的原則，EValle 不應該會有任何權限。  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>若要確認臨時原則的結果  
  
1.  連接到檔案伺服器 1 HYPER-V 管理員和登入密碼 contoso\administrator，以**pass@word1**。  
  
2.  打開命令提示字元視窗中，輸入**gpupdate /force**。 這樣可確保群組原則變更會影響您的伺服器。  
  
3.  在 [HYPER-V 管理員連接到伺服器 CLIENT1。 目前登入的使用者關閉登入。 重新開機一樣，CLIENT1。 再登入電腦使用 contoso\EValle pass@word1。  
  
4.  按兩下 \\\FILE1\Finance 文件] 桌面捷徑。 EValle 應該仍然可以存取檔案。 切換回 1。  
  
5.  開放**事件檢視器]**從桌面的快速鍵。 展開**Windows 登**，然後選取 [**的安全性**。 打開的項目使用**事件 ID 4818**在**中央存取原則階段**分類工作。 您將會看到 EValle，已允許存取。不過，根據臨時的原則，使用者會遭拒存取。  
  
## <a name="next-steps"></a>後續步驟  
如果您有例如 System Center Operations Manager 中央伺服器管理系統，您可以也設定監視事件。 這可讓系統管理員的中央存取原則效果監視之前，請先執行它們。  
  

