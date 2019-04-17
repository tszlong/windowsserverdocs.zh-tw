---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: "部署安全性稽核中央稽核原則（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>部署安全性稽核中央稽核原則（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在本案例中，您將會使用您在 [財經原則稽核存取財經文件] 資料夾中的檔案[部署的中央存取原則與 #40; 示範步驟和 #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). 如果存取嘗試存取資料夾未授權的使用者，事件檢視器擷取的活動。   
 測試案例這需要下列步驟。  
  
|工作|描述|  
|--------|---------------|  
|[設定全球物件存取](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|在此步驟，您可以設定全球物件存取原則網域控制站。|  
|[更新的群組原則設定](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|登入該檔案伺服器，並套用群組原則的更新。|  
|[確認已套用全球物件存取原則](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|檢視相關事件的事件檢視器中。 事件應包含的國家/地區和文件類型中繼資料。|  
  
## <a name="BKMK_1"></a>設定全球物件存取原則  
在此步驟，您可以設定的網域控制站全球物件存取原則。  
  
#### <a name="to-configure-a-global-object-access-policy"></a>若要設定的全域物件存取原則  
  
1.  contoso\administrator 使用密碼登入的網域控制站 DC1 **pass@word1**。  
  
2.  在伺服器管理員中，移至**工具**，然後按**群組原則管理**。  
  
3.  在主控台按兩下 [**網域**，按兩下 [ **contoso.com**，按一下 [**以 Contoso**，，然後按兩下 [**檔案伺服器**。  
  
4.  以滑鼠右鍵按一下**FlexibleAccessGPO**，按一下 [**編輯**。  
  
5.  按兩下**電腦設定**，按兩下 [**原則**，然後按兩下 [**的 Windows 設定**。  
  
6.  按兩下**安全性設定**，按兩下 [**進階稽核原則設定**，然後按兩下 [**稽核原則**。  
  
7.  按兩下**物件存取**，然後按兩下 [**稽核檔案系統**。  
  
8.  選取 [**設定下列事件**核取方塊中，選取**成功**並**失敗**核取方塊，然後按一下**[確定]**。  
  
9. 在瀏覽窗格中，按兩下 [**全球物件存取稽核**，然後按兩下 [**檔案系統**。  
  
10. 選取 [**定義這項原則設定**核取方塊，然後按**設定**。  
  
11. 在**的全球檔案 SACL 進階安全性設定**方塊中，按一下 [**新增**，然後按一下 [**選取主體**，輸入**每個人都**，，然後按一下 [ **[確定]**。  
  
12. 在**的全球檔案 SACL 稽核項目**方塊中，選取**完全控制**中**權限]**方塊。  
  
13. 在**[新增條件：**區段中，按一下 [ **[新增條件**及中下拉式清單選取   
    [**資源**][**部門**][**Any of**][**Value**][**財經**]。  
  
14. 按一下**[確定]**三次，以完成設定存取全球物件的稽核原則設定。  
  
15. 在瀏覽窗格中，按一下 [**存取物件**，並在 [結果] 窗格中，按兩下 [**稽核處理操作**。 按一下**設定下列稽核事件**，**成功**，並**失敗**，按一下**[確定]**，，然後關閉 [GPO 彈性存取。  
  
## <a name="BKMK_2"></a>更新群組原則」設定  
在此步驟，您更新的群組原則設定之後您所建立的稽核原則。  
  
#### <a name="to-update-group-policy-settings"></a>若要更新的群組原則設定  
  
1.  該檔案伺服器，1 為 contoso\Administrator，使用密碼登入**pass@word1**。  
  
2.  長按 Windows 鍵 + R，然後輸入**cmd**打開在命令提示字元視窗。  
  
    > [!NOTE]  
    > 如果**使用者 Account 控制項**對話方塊，請確認您的動作，它會顯示是您想要然後按一下 [**是**。  
  
3.  輸入**gpupdate /force**，然後按 ENTER 鍵。  
  
## <a name="BKMK_3"></a>確認已套用全球物件存取原則  
已套用群組原則設定之後，您就可以驗證已正確套用的稽核原則設定。  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>若要確認已套用全球物件存取原則  
  
1.  Client 電腦上，為 Contoso\MReid CLIENT1 登入。 瀏覽到超連結「file:///\\\ID_AD_FILE1\\\Finance「\\\ FILE1\Finance 文件，以及修改 Word 文件 2。  
  
2.  登入該檔案伺服器，為 contoso\administrator 1。 打開事件檢視器、瀏覽] **Windows 登**，請選取**安全性**，並確認您的活動，會導致稽核事件**4656**和**4663**（即使您並未設定明確稽核 Sacl 上的檔案或資料夾，您所建立，修改，以及刪除）。  
  
> [!IMPORTANT]  
> 新的登入事件也資源所在的位置、正在對象檢查有效的存取權的使用者代表電腦上。 當分析安全性稽核登的使用者登入活動，若要登入事件專因為有效的存取，因為生成來區分公司互動式網路使用者登入，包含模擬層級資訊。 登入事件也因為有效的存取，模擬層級會的身分。 網路互動式使用者登入通常產生模擬層級的登入事件 = 模擬或委派。  
  
## <a name="BKMK_Links"></a>也了  
  
-   [案例：檔案存取稽核](Scenario--File-Access-Auditing.md)  
  
-   [檔案計劃存取稽核](Plan-for-File-Access-Auditing.md)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

