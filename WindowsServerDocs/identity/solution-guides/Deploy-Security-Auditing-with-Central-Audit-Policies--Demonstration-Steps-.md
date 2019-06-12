---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: 使用集中稽核原則部署安全性稽核 (示範步驟)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b14ded98c4f1a340349119bd9f5f42e3a1bf9434
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445741"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>使用集中稽核原則部署安全性稽核 (示範步驟)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在此案例中，您會使用您在建立 「 財務原則稽核財務文件 資料夾中檔案的存取權[部署集中存取原則&#40;示範步驟&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)。 如果未獲得授權存取資料夾的使用者嘗試存取該資料夾，就會在事件檢視器中擷取該活動。   
 以下為測試這個案例的必要步驟。  
  
|工作|描述|  
|--------|---------------|  
|[設定全域物件存取](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|在這個步驟中，您在網域控制站上設定全域物件存取原則。|  
|[更新群組原則設定](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|登入檔案伺服器並套用群組原則更新。|  
|[確認已套用全域物件存取原則](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|在事件檢視器中檢視相關的事件。 事件應該包含國家或地區及文件類型的中繼資料。|  
  
## <a name="BKMK_1"></a>設定全域物件存取原則  
在這個步驟中，您在網域控制站中設定全域物件存取原則。  
  
#### <a name="to-configure-a-global-object-access-policy"></a>設定全域物件存取原則  
  
1. 以 contoso\administrator 的身分搭配密碼登入網域控制站 DC1 <strong>pass@word1</strong>。  
  
2. 在 [伺服器管理員] 中，指向 [工具]  ，然後按一下 [群組原則管理]  。  
  
3. 在主控台樹狀目錄中，依序按兩下 [網域]  和 [contoso.com]  ，按一下 [Contoso]  ，然後按兩下 [檔案伺服器]  。  
  
4. 在 [FlexibleAccessGPO]  上按一下滑鼠右鍵，然後按一下 [編輯]  。  
  
5. 依序按兩下 [電腦設定]  、[原則]  及 [Windows 設定]  。  
  
6. 依序按兩下 [安全性設定]  、[進階稽核原則設定]  及 [稽核原則]  。  
  
7. 按兩下 [物件存取]  ，然後按兩下 [稽核檔案系統]  。  
  
8. 選取 [設定下列事件]  核取方塊，選取 [成功]  和 [失敗]  核取方塊，然後按一下 [確定]  。  
  
9. 在瀏覽窗格中，依序按兩下 [全域物件存取稽核]  和 [檔案系統]  。  
  
10. 選取 [定義這個原則設定]  核取方塊，然後按一下 [設定]  。  
  
11. 在 [全域檔案 SACL 的進階安全性設定]  方塊中，依序按一下 [新增]  和 [選取主體]  ，輸入 **Everyone**，然後按一下 [確定]  。  
  
12. 在 [全域檔案 SACL 的稽核項目]  方塊中，選取 [權限]  方塊中的 [完全控制]  。  
  
13. 在 **新增條件：** 區段中，按一下**新增條件**並在下拉式清單中選取   
    [**Resource**] [**部門**] [**任一**] [**值**] [**財務**]。  
  
14. 按三次 [確定]  ，以完成設定全域物件存取稽核原則設定。  
  
15. 在瀏覽窗格中，按一下 [物件存取]  ，然後在結果窗格中，按兩下 [稽核控制代碼操作]  。 按一下 [設定下列稽核事件]  、[成功]  及 [失敗]  ，按一下 [確定]  ，然後關閉彈性存取 GPO。  
  
## <a name="BKMK_2"></a>更新群組原則設定  
在這個步驟中，您會在建立稽核原則之後更新群組原則設定。  
  
#### <a name="to-update-group-policy-settings"></a>更新群組原則設定  
  
1. 登入的檔案伺服器，以 contoso\administrator 的身分，使用密碼的 FILE1 <strong>pass@word1</strong>。  
  
2. 按 Windows 鍵 + R，然後輸入 **cmd**，以開啟命令提示字元視窗。  
  
   > [!NOTE]  
   > 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
3. 輸入 **gpupdate /force**，然後按 ENTER。  
  
## <a name="BKMK_3"></a>確認已套用全域物件存取原則  
套用群組原則設定之後，您可以確認稽核原則設定已正確套用。  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>確認已套用全域物件存取原則  
  
1.  以 Contoso\MReid 身分登入用戶端電腦 CLIENT1。 瀏覽至資料夾 HYPERLINK"file:///\\\\\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance Documents，並修改 Word Document 2。  
  
2.  以 contoso\administrator 身分登入檔案伺服器 FILE1。 開啟 [事件檢視器]，瀏覽至 [Windows 記錄]  ，選取 [安全性]  ，並確認您的活動產生稽核事件 **4656** 和 **4663** (即使您並未在所建立、修改和刪除的檔案或資料夾上明確設定稽核 SACL 也一樣)。  
  
> [!IMPORTANT]  
> 系統會代表已檢查其有效存取權的使用者，於資源所在的電腦上產生新的登入事件。 分析使用者登入活動的安全性稽核記錄時，為了區分因為有效存取權而產生的登入事件，以及因為互動式網路使用者登入而產生的登入事件，會包含模擬等級資訊。 因為有效存取權而產生登入事件時，模擬等級將會是「識別碼」。 網路互動式使用者登入通常會產生模擬等級為「模擬」或「委派」的登入事件。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [案例：檔案存取稽核](Scenario--File-Access-Auditing.md)  
  
-   [規劃檔案存取稽核](Plan-for-File-Access-Auditing.md)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

