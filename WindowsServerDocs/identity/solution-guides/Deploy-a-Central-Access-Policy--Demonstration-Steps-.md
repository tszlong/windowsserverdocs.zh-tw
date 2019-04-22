---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: 部署集中存取原則 (示範步驟)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817139"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>部署集中存取原則 (示範步驟)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在此案例中，財務部門安全性作業使用中央資訊安全性，以指定集中存取原則的需求，讓它們可以保護儲存在檔案伺服器上的封存財務資訊。 每個國家/地區的已封存財經資訊可以由來自相同的國家/地區的財務員工以唯讀方式存取。 中央財務管理群組可以存取所有國家/地區的財務資訊。  
  
部署集中存取原則包含下列階段：  
  
|階段|描述  
|---------|---------------  
|[計劃：識別原則以及部署所需的設定需求](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|識別原則的需求，以及部署所需的設定。 
|[實作：設定元件與原則](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|設定元件與原則。  
|[部署集中存取原則](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|部署原則。  
|[維護：變更和暫存原則](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|原則變更和預備環境。 
  
## <a name="BKMK_1.1"></a>設定測試環境  
在開始之前，您需要設定測試此案例的實驗室。 設定實驗室的步驟會說明詳細[附錄 b:設定測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)。  
  
## <a name="BKMK_1.2"></a>計劃：識別原則的需求，以及部署所需的設定  
本節提供一系列高階步驟，協助部署的規劃階段。  
  
||步驟|範例|  
|-|--------|-----------|  
|1.1|公司決定需要集中存取原則。|為了保護儲存在檔案伺服器的財務資訊，財務部門安全性作業使用中央資訊安全性，指定需要集中存取原則。|  
|1.2|表達存取原則|財務文件只應該由財務部門的成員讀取。 財務部門的成員應該只能存取他們自己國家/地區的文件。 只有財務系統管理員具有寫入存取權。 對於 FinanceException 群組的成員，可允許例外狀況。 此群組將具有讀取存取權。|  
|1.3|表達存取原則在 Windows Server 2012 建構中|目標：<br /><br />-Resource.Department 包含財務<br /><br />存取規則：<br /><br />-允許讀取 User.Country=Resource.Country 和 User.department = Resource.Department<br />-允許完整控制 User.MemberOf(FinanceAdmin)<br /><br />例外狀況：<br /><br />Allow read memberOf(FinanceException)|  
|1.4|決定原則所需的檔案屬性|標記檔案：<br /><br />-部門<br />-國家/地區|  
|1.5|判斷原則所需的宣告類型和群組|宣告類型：<br /><br />-國家/地區<br />-部門<br /><br />使用者群組：<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|決定要套用此原則的伺服器|將原則套用到所有的財務檔案伺服器。|  
  
## <a name="BKMK_1.3"></a>實作：設定元件與原則  
本節說明部署財務文件集中存取原則的範例。  
  
|否|步驟|範例|  
|------|--------|-----------|  
|2.1|建立宣告類型|建立下列宣告類型：<br /><br />-部門<br />-國家/地區|  
|2.2|建立資源內容|建立並啟用下列資源內容：<br /><br />-部門<br />-國家/地區|  
|2.3|設定集中存取規則|建立包含上節決定之原則的財務文件規則。|  
|2.4|設定集中存取原則 (CAP)|建立稱為「財務原則」的 CAP，並將「財務文件」規則新增到該 CAP。|  
|2.5|檔案伺服器的目標集中存取原則|將財務原則 CAP 發佈至檔案伺服器。|  
|2.6|啟用宣告、複合驗證和 Kerberos 防護的 KDC 支援。|為 contoso.com 啟用宣告、複合驗證和 Kerberos 防護的 KDC 支援。|  
  
在下列程序中，您會建立兩個宣告類型：國家/地區和部門。  
  
#### <a name="to-create-claim-types"></a>建立宣告類型  
  
1.  以 contoso\administrator 的身分，使用密碼上開啟 HYPER-V 管理員和記錄檔中的伺服器 DC1 **pass@word1**。  
  
2.  開啟「Active Directory 管理中心」。  
  
3.  按一下 [樹狀檢視圖示]，展開 [動態存取控制]，然後選取 [宣告類型]。  
  
    以滑鼠右鍵按一下 [宣告類型]，按一下 [新增]，然後按一下 [宣告類型]。  
  
    > [!TIP]  
    > 您也可以從 [工作] 窗格開啟 [建立宣告類型:] 視窗。 在 [工作] 窗格中，依序按一下 [新增] 和 [宣告類型]。  
  
4.  在 [來源屬性] 清單中，向下捲動屬性清單，按一下 [部門]。 如此會在 [顯示名稱] 欄位填入 [部門]。 按一下 [確定] 。  
  
5.  在 [工作] 窗格中，依序按一下 [新增] 和 [宣告類型]。  
  
6.  在 [來源屬性] 清單中，向下捲動屬性清單，然後按一下 [c] 屬性 (國家/地區名稱)。 在 [顯示名稱] 欄位，輸入**國家/地區**。  
  
7.  在 [建議值] 區段中，選取 [下列是建議值:]，然後按一下 [新增]。  
  
8.  在 [值] 和 [顯示名稱] 欄位中，輸入**美國**，然後按一下 [確定]。  
  
9. 重複上述步驟。 在 [新增建議值] 對話方塊中的 [值] 和 [顯示名稱] 欄位中輸入**日本**，然後按一下 [確定]。  
  
![解決方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> 您可以使用 Active Directory 管理中心內的 Windows PowerShell 歷程記錄檢視器，查詢在 Active Directory 管理中心中執行的每個程序的 Windows PowerShell Cmdlet。 如需詳細資訊，請參閱 [Windows PowerShell 歷程記錄檢視器](https://technet.microsoft.com/library/hh831702)  
  
下一個步驟是建立資源內容。 您會在下列程序中建立資源內容，它會被自動新增到網域控制站上的全域資源內容清單，以供檔案伺服器使用。  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>建立並啟用預先建立的資源內容  
  
1.  在 [Active Directory 管理中心] 的左窗格中，按一下 [樹狀檢視]。 展開 [動態存取控制]，然後選取 [資源內容]。  
  
2.  以滑鼠右鍵按一下 [資源內容]，按一下 [新增]，然後按一下 [參考資源內容]。  
  
    > [!TIP]  
    > 您也可以從 [工作] 窗格選擇資源內容。 按一下 [新增]，然後按一下 [參考資源屬性]。  
  
3.  在 [選取要共用其建議值清單的宣告類型] 中，按一下 [國家/地區]。  
  
4.  在 [顯示名稱] 欄位中，輸入**國家/地區**，然後按一下 [確定]。  
  
5.  按兩下 [資源內容]清單，向下捲動到 [部門] 資源內容。 按一下滑鼠右鍵，然後按一下 [啟用]。 這樣會啟用內建 [部門] 資源內容。  
  
6.  在 Active Directory 管理中心內瀏覽窗格上的 [資源內容] 清單中，現在會有兩個已啟用的資源內容：  
  
    -   國家/地區  
  
    -   部門  
  
![解決方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
下一步是建立集中存取規則，定義可以存取資源的人員。 在此案例中的商務規則如下：  
  
-   只有財務部門的成員可以讀取財務文件。  
  
-   財務部門的成員只能存取他們自己國家/地區的文件。  
  
-   只有財務系統管理員具有寫入存取權。  
  
-   我們將會允許 FinanceException 群組成員的例外狀況。 此群組將具有讀取存取權。  
  
-   系統管理員和文件擁有者仍然擁有完整存取權。  
  
或 express 與 Windows Server 2012 建構規則：  
  
目標：Resource.Department Contains Finance  
  
存取規則：  
  
-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department  
  
-   Allow Full control User.MemberOf(FinanceAdmin)  
  
-   Allow Read User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>建立集中存取規則  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [樹狀檢視]，選取 [動態存取控制]，然後按一下 [集中存取規則]。  
  
2.  以滑鼠右鍵按一下 [集中存取規則]，按一下 [新增]，再按 [集中存取規則]。  
  
3.  在 [名稱] 欄位中，輸入**財務文件規則**。  
  
4.  在 [目標資源] 區段中，按一下 [編輯]，然後在 [集中存取規則] 對話方塊中，按一下 [新增條件]。 新增下列條件：   
    [資源] [部門] [等於] [值] [財務]，然後按一下 [確定]。  
  
5.  在 [權限] 區段中，選取 [使用下列權限做為目前權限]，按一下 [編輯]，然後在 [權限的進階安全性設定] 對話方塊中，按一下 [新增]。  
  
    > [!NOTE]  
    > [使用下列權限做為建議權限] 選項可以讓您建立暫存原則。 如需如何執行此動作的詳細資訊，請參閱本主題中「維護：變更和暫存原則」一節。  
  
6.  在 [權限的權限項目] 對話方塊中，按一下 [選取主體]，輸入**已驗證的使用者**，然後按一下 [確定]。  
  
7.  在 [權限的權限項目] 對話方塊中，按一下 [新增條件]，並新增下列條件：   
    [**使用者**] [**國家/地區**] [**任一**] [**資源**] [**國家/地區**]   
     按一下 **\[新增條件\]**。   
     [**And**]   
    按一下 [**使用者**] [**部門**] [**任一**] [**資源**] [**部門**]。 將 [權限] 設定為 [讀取]。  
  
8.  按一下 [確定]，然後按一下 [新增]。 按一下 [選取主體]，輸入 **FinanceAdmin**，然後按一下 [確定]。  
  
9. 選取 [修改]、[讀取和執行]、[讀取]、[寫入] 權限，然後按一下 [確定]。  
  
10. 按一下 [新增]，按一下 [選取主體]，輸入 **FinanceException**，然後按一下 [確定]。 選取 [讀取] 和 [讀取和執行] 權限。  
  
11. 按一下 [確定]  三次，以完成並返回 Active Directory 管理中心。  
  
    ![解決方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
    下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
 
    $countryClaimType = Get-ADClaimType country  
    $departmentClaimType = Get-ADClaimType department  
    $countryResourceProperty = Get-ADResourceProperty Country  
    $departmentResourceProperty = Get-ADResourceProperty Department  
    $currentAcl ="O:SYG:SYD:AR(A;;FA;; OW) (A;FA;;BA) （A; 0x1200a9;;S-1-5-21-1787166779-1215870801-2157059049-1113) （A; 0x1301bf;;S-1-5-21-1787166779-1215870801-2157059049-1112) (A;FA;;SY) (XA; 0x1200a9;;AU;((@USER。 」 + $countryClaimType.Name +"Any_of @RESOURCE。 」 + $countryResourceProperty.Name +") & & (@USER。 」 + $departmentClaimType.Name +"Any_of @RESOURCE。 」 + $departmentResourceProperty.Name + ")))"  
    $resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + 」 包含 {`"Finance`"})"  
    New-ADCentralAccessRule "Finance Documents Rule" -CurrentAcl $currentAcl -ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> 在上列 Cmdlet 範例中，FinanceAdmin 群組和使用者的安全性識別碼 (SID) 是在建立時決定，因此會與範例不同。 例如，為 FinanceAdmin 群組提供的 SID 值 (S-1-5-21-1787166779-1215870801-2157059049-1113)，必須以您在部署時建立的 FinanceAdmin 群組的實際 SID 取代。 您可以使用 Windows PowerShell 查詢此群組的 SID 值，將該值指派給變數，然後在這裡使用的變數。 如需詳細資訊，請參閱[Windows PowerShell 秘訣：使用 Sid](https://go.microsoft.com/fwlink/?LinkId=253545)。  
  
您現在應該會有集中存取規則，允許人員存取相同國家和相同部門的文件。 這個規則允許 FinanceAdmin 群組編輯文件，也允許 FinanceException 群組讀取文件。 這個規則只針對分類為「財務」的文件。  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>將集中存取規則新增到集中存取原則  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [動態存取控制]，然後按一下 [集中存取原則]。  
  
2.  在 [工作] 窗格中，依序按一下 [新增] 和 [集中存取原則]。  
  
3.  在 [建立集中存取原則:] 中，在 [名稱] 方塊中輸入**財務原則**。  
  
4.  在 [成員集中存取規則] 中，按一下 [新增]。  
  
5.  按兩下 [財務文件規則]，將它新增至 [新增下列集中存取規則] 清單，然後按一下 [確定]。  
  
6.  按一下 [確定] 以完成。 您現在應該有一個稱為「財務原則」的集中存取原則。  
  
    ![解決方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
    下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>使用群組原則在檔案伺服器之間套用集中存取原則  
  
1.  在 [開始] 畫面上，於 [搜尋] 方塊中輸入**群組原則管理**。 按兩下 [群組原則管理]。  
  
    > [!TIP]  
    > 如果停用 [顯示系統管理工具] 設定，[系統管理工具] 資料夾及其內容就不會出現在 [設定] 結果中。  
  
    > [!TIP]  
    > 在生產環境中，您應該建立檔案伺服器組織單位 (OU)，並將所有的檔案伺服器新增至要套用此原則的這個 OU 中。 接著您可以建立群組原則，然後將這個 OU 新增到該原則。  
  
2.  在這個步驟中，您會編輯在＜測試環境＞中[建立網域控制站](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build)所建立的群組原則物件，以包含剛才建立的集中存取原則。 在 [群組原則管理編輯器] 中，瀏覽至網域中的組織單位並加以選取 (在這個範例中為 contoso.com)：[群組原則管理]、[樹系: contoso.com]、[網域]、[contoso.com]、[Contoso]、[FileServerOU]。  
  
3.  以滑鼠右鍵按一下 [FlexibleAccessGPO]，然後按一下 [編輯]。  
  
4.  在 [群組原則管理編輯器] 中，瀏覽至 [電腦設定]，依序展開 [原則] 和 [Windows 設定]，然後按一下 [安全性設定]。  
  
5.  展開 [檔案系統]，以滑鼠右鍵按一下 [集中存取原則]，然後按一下 [管理集中存取原則]。  
  
6.  在 [集中存取原則設定] 對話方塊方塊中，新增 [財務原則]，然後按一下 [確定]。  
  
7.  向下捲動到 [進階稽核原則設定]，並展開它。  
  
8.  展開 [稽核原則]，然後選取 [物件存取]。  
  
9. 按兩下 [稽核集中存取原則暫存]。 選取全部三個核取方塊，然後按一下 [確定]。 這個步驟可讓系統接收與「集中存取暫存原則」相關的稽核事件。  
  
10. 按兩下 [稽核檔案系統內容]。 選取全部三個核取方塊，然後按一下 [確定]。  
  
11. 關閉 \[群組原則管理編輯器\]。 您現在已將集中存取原則包含在群組原則中。  
  
提供宣告或裝置授權資料網域的網域控制站，網域控制站必須設定為支援動態存取控制。  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>為 contoso.com 啟用宣告和複合驗證的支援  
  
1.  開啟 [群組原則管理]，按一下 [contoso.com]，再按一下 [網域控制站]。  
  
2.  以滑鼠右鍵按一下 [預設網域控制站原則] ，然後按一下 [編輯] 。  
  
3.  在 [群組原則管理編輯器] 視窗中，依序按兩下 [電腦設定]、[原則]、[系統管理範本]、[系統]、[KDC]。  
  
4.  按兩下 [宣告、複合驗證和 Kerberos 防護的 KDC 支援]。 在 [宣告、複合驗證和 Kerberos 防護的 KDC 支援] 對話方塊中，按一下 [啟用]，從 [選項] 下拉式清單選取 [支援]。 (您需要啟用此設定，才能在集中存取原則中使用使用者宣告)。  
  
5.  關閉 [群組原則管理]。  
  
6.  開啟命令提示字元，然後輸入 `gpupdate /force`：  
  
## <a name="BKMK_1.4"></a>部署集中存取原則  
  
||步驟|範例|  
|-|--------|-----------|  
|3.1|將 CAP 指派給檔案伺服器上適當的共用資料夾。|將集中存取原則指派給檔案伺服器上適當的共用資料夾。|  
|3.2|請確認已適當地設定存取權。|請查看來自不同國家/地區與部門的使用者存取權。|  
  
在此步驟中，您會將集中存取原則指派給檔案伺服器。 您將登入到接收前面步驟中建立的集中存取原則的檔案伺服器，然後將原則指派給共用資料夾。  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>將集中存取原則指派給檔案伺服器  
  
1.  在 [HYPER-V 管理員] 中，連線至伺服器 FILE1。 登入伺服器使用 contoso\administrator 和密碼： **pass@word1**。  
  
2.  開啟提升權限的命令提示字元，輸入： **gpupdate /force**。 這可確保您的群組原則變更能在伺服器上生效。  
  
3.  您也需要重新整理 Active Directory 的全域資源內容。 開啟提升權限的 Windows PowerShell 視窗並輸入 `Update-FSRMClassificationpropertyDefinition`。 按一下 Enter 鍵，然後關閉 Windows PowerShell。  
  
    > [!TIP]  
    > 您也可以藉由登入檔案伺服器，以重新整理全域資源內容。 若要從檔案伺服器重新整理全域資源內容，請執行下列動作  
    >   
    > 1.  檔案伺服器 file1，以 contoso\administrator 的身分使用密碼的登入**pass@word1**。  
    > 2.  開啟檔案伺服器資源管理員。 若要開啟檔案伺服器資源管理員，按一下 [開始] ，輸入 **檔案伺服器資源管理員**，然後按一下 [檔案伺服器資源管理員] 。  
    > 3.  在 [檔案伺服器資源管理員] 中按一下 [檔案分類管理]，以滑鼠右鍵按一下 [分類內容]，然後按一下 [重新整理]。  
  
4.  開啟 [Windows 檔案總管]，在左窗格中，按一下磁碟機 D。以滑鼠右鍵按一下 [財務文件] 資料夾，然後按一下 [內容]。  
  
5.  按一下 [分類] 索引標籤，按一下 [國家/地區]，然後在 [值] 欄位中選取 [美國]。  
  
6.  按一下 [部門]，在 [值] 欄位選取 [財務]，然後按一下 [套用]。  
  
    > [!NOTE]  
    > 請記住，集中存取原則已設定為針對財務部門的檔案。 上述步驟會以 [國家/地區] 和 [部門] 屬性標記資料夾中所有文件。  
  
7.  按一下 [安全性] 索引標籤，然後按一下 [進階]。 按一下 [集中原則] 索引標籤。  
  
8.  按一下 [變更]，從下拉式功能表選取 [財務原則]，然後按一下 [套用]。 您可以看到原則中列出了 [財務文件規則]。 您可以展開項目以檢視在 Active Directory 中建立規則時設定的所有權限。  
  
9. 按一下 [確定] 返回 [Windows 檔案總管]。  
  
在下一個步驟中，您將確認已適當地設定存取權。  使用者帳戶必須設定擁有適當的 [部門] 屬性 (使用 Active Directory 管理中心來設定)。 檢視新原則的有效結果最簡單的方式是使用 [Windows 檔案總管] 中的 [有效存取權] 索引標籤。 [有效存取權] 索引標籤會顯示指定的使用者帳戶的存取權限。  
  
#### <a name="to-examine-the-access-for-various-users"></a>檢查各種使用者的存取權  
  
1.  在 [HYPER-V 管理員] 中，連線至伺服器 FILE1。 使用 contoso\administrator 的身分登入伺服器。 在 [Windows 檔案總管] 中瀏覽至 D:\。 以滑鼠右鍵按一下 [財務文件] 資料夾，然後按一下 [內容]。  
  
2.  按一下 [安全性] 索引標籤，按一下 [進階]，然後按一下 [有效存取權] 索引標籤。  
  
3.  若要檢查使用者的權限，請按一下**選取使用者**，輸入使用者的名稱，然後按一下**檢視有效存取權**來查看有效存取權限。 例如:   
  
    -   Myriam Delesalle (MDelesalle) 在財務部門中工作，應該具有資料夾的讀取存取權。  
  
    -   Miles Reid (MReid) 是 FinanceAdmin 群組的成員，應該具有資料夾的修改存取權。  
  
    -   Esther Valle (EValle) 不在財務部門工作，不過，她是 FinanceException 群組的成員，所以應該具有讀取存取權。  
  
    -   Maira Wenzel (MWenzel) 不在財務部門工作，而且也不是 FinanceAdmin 或 FinanceException 群組的成員。 她不應該具有資料夾的任何存取權。  
  
    請注意有效存取權視窗中名為 [存取限制依據] 的最後一欄。 本專欄會告訴您的閘道會影響人員權限。 在此例中，共用和 NTFS 權限允許所有使用者具有完全控制的權限。 不過，集中存取原則會根據您先前設定的規則限制存取權。  
  
## <a name="BKMK_1.5"></a>維護：變更和暫存原則  
  
||||  
|-|-|-|  
|Number|步驟|範例|  
|4.1|設定用戶端裝置宣告|設定群組原則設定，以啟用裝置宣告|  
|4.2|啟用裝置宣告。|啟用裝置的國家/地區宣告類型。|  
|4.3|將暫存原則新增至您想要修改的現有集中存取規則。|修改財務文件規則以新增暫存原則。|  
|4.4|檢視暫存原則的結果。|檢查 Velle 的權限。|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>設定群組原則設定以啟用裝置宣告  
  
1.  登入 DC1，開啟 [群組原則管理]，按一下 [contoso.com]，按一下 [預設網域原則]，按一下滑鼠右鍵，然後選取 [編輯]。  
  
2.  在 [群組原則管理編輯器] 視窗中，依序瀏覽至 [電腦設定]、[原則]、[系統管理範本]、[系統]、[Kerberos]。  
  
3.  選取 [宣告、複合驗證和 Kerberos 防護的 Kerberos 用戶端支援]，按一下 [啟用]。  
  
#### <a name="to-enable-a-claim-for-devices"></a>啟用裝置宣告  
  
1.  以 contoso\administrator 的身分，使用密碼上開啟 HYPER-V 管理員和記錄檔中的伺服器 DC1 **pass@word1**。  
  
2.  從 [工具] 功能表上開啟 [Active Directory 管理中心]。  
  
3.  按一下 [樹狀檢視]，展開 [動態存取控制]，按兩下 [宣告類型]，按兩下 [國家/地區] 宣告。  
  
4.  在 [可以針對下列類別發行此類型的宣告] 中，選取 [電腦] 核取方塊。 按一下 [確定] 。   
    [使用者] 和 [電腦] 兩個核取方塊現在應該都已選取。 國家/地區宣告現在除了可以搭配使用者以外，還可以搭配裝置。  
  
下一個步驟是建立暫存原則規則。 暫存原則可以用來在啟用新的原則項目之前先監視它的效果。 在下列步驟中，您將建立暫存原則項目，並監視它在共用資料夾上的效果。  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>建立暫存原則規則並將它新增到集中存取原則  
  
1.  以 contoso\administrator 的身分，使用密碼上開啟 HYPER-V 管理員和記錄檔中的伺服器 DC1 **pass@word1**。  
  
2.  開啟「Active Directory 管理中心」。  
  
3.  按一下 [樹狀檢視]，展開 [動態存取控制]，然後選取 [集中存取規則]。  
  
4.  用滑鼠右鍵按一下 [財務文件規則]，然後按一下 [內容]。  
  
5.  在 [建議的權限] 區段中，選取 [啟用權限暫存組態] 核取方塊，按一下 [編輯]，然後按一下 [新增]。 在 [提議權限的權限項目] 視窗中，按一下 [選取主體] 連結，輸入**已驗證的使用者**，然後按一下 [確定]。  
  
6.  按一下 [新增條件] 連結並新增下列條件：   
     [使用者] [國家/地區] [任何] [資源] [國家/地區]。  
  
7.  再次按一下 [新增條件] 並新增下列條件：  
    [**And**]   
     [裝置] [國家/地區] [任何] [資源] [國家/地區]  
  
8.  再次按一下 [新增條件] 並新增下列條件。  
    [And]   
     [**使用者**] [**群組**] [**成員隸屬任何**] [**值**]\(**FinanceException**)  
  
9. 若要設定 FinanceException 群組，按一下 [新增項目]，然後在 [選取使用者、電腦、服務帳戶或群組] 視窗中，輸入 **FinanceException**。  
  
10. 按一下 [權限]，選取 [完全控制]，然後按一下 [確定]。  
  
11. 在 [建議權限的進階安全性設定] 視窗中，選取 [FinanceException]，按一下 [移除]。  
  
12. 按一下 [確定] 兩次以完成。  
  
![解決方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> 在上述的 Cmdlet 範例中，[伺服器] 值反映測試實驗室環境中的伺服器。 您可以使用 Windows PowerShell 歷程記錄檢視器，查詢在 Active Directory 管理中心中執行的每個程序的 Windows PowerShell Cmdlet。 如需詳細資訊，請參閱 [Windows PowerShell 歷程記錄檢視器](https://technet.microsoft.com/library/hh831702)  
  
在這個建議的權限集中，當 FinanceException 群組的成員存取自己國家/地區的檔案時，如果使用的裝置與文件屬於相同國家/地區，他們對這些檔案具有完整存取權。 當財務部門中某個人嘗試存取檔案時，檔案伺服器安全性記錄檔中便會有稽核項目。 不過，直到原則從暫存狀態升級之後，才會強制執行安全性設定。  
  
在下一個程序中，您要檢查暫存原則的結果。 您將會使用在目前規則中具有權限的使用者名稱來存取共用資料夾。 Esther Valle (EValle) 是 FinanceException 的成員，她目前具有讀取權限。 根據我們的暫存原則，EValle 不應有任何權限。  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>確認暫存原則的結果  
  
1.  以 contoso\administrator 的身分，使用的密碼連線到 HYPER-V 管理員和記錄檔中的檔案伺服器 FILE1 上**pass@word1**。  
  
2.  開啟 [命令提示字元] 視窗，然後輸入 **gpupdate /force**。 這可確保您的群組原則變更能在伺服器上生效。  
  
3.  在 [HYPER-V 管理員] 中，連線至 CLIENT1。 登出目前已登入的使用者。 重新啟動虛擬機器 CLIENT1。 然後登入的電腦使用 contoso\EValle pass@word1。  
  
4.  按兩下桌面捷徑\\\FILE1\Finance 文件。 EValle 應該還是可以存取檔案。 切換回 FILE1。  
  
5.  從桌面上的捷徑開啟 [事件檢視器]。 展開 [Windows 記錄檔]，然後選取 [安全性]。 開啟項目**Event ID 4818**下方**集中存取原則暫存**工作分類。 您會看到 EValle 被允許存取。不過，根據暫存原則，使用者會被拒絕存取。  
  
## <a name="next-steps"></a>後續步驟  
如果您有中央伺服器管理系統 (如 System Center Operations Manager)，則也可以設定監視事件。 這可讓系統管理員強制執行集中存取原則之前先監視它們的效果。  
  

