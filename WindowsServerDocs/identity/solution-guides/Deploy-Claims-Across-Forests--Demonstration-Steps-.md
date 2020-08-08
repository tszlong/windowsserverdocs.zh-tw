---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: 跨樹系部署宣告 (示範步驟)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 828b7256854f9d2fd6d58567c773d4abc288cd0a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952870"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>跨樹系部署宣告 (示範步驟)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在本主題中，我們將討論一個基本案例，說明如何在信任與受信任的樹系之間設定宣告轉換。 您將瞭解如何建立宣告轉換原則物件，並將其連結至信任樹系和受信任樹系上的信任。 接著，您將會驗證案例。

## <a name="scenario-overview"></a>案例概觀
Adatum Corporation 為 Contoso，公司提供財務服務。在每一季，Adatum 會計將其帳戶試算表複製到位於 Contoso，公司之檔案伺服器上的資料夾。從 Contoso 到 Adatum 的雙向信任設定。 Contoso，公司想要保護共用，讓只有 Adatum 員工可以存取遠端共用。

在此情節中：

1.  [設定必要條件和測試環境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)

2.  [在受信任的樹系上設定宣告轉換 (Adatum) ](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)

3.  [在信任樹系中設定宣告轉換 (Contoso) ](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)

4.  [驗證案例](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>設定必要條件和測試環境
測試設定牽涉到設定兩個樹系： Adatum Corporation 和 Contoso，公司，並具有 Contoso 和 Adatum 之間的雙向信任關係。 「adatum.com」是受信任的樹系，而「contoso.com」是信任的樹系。

宣告轉換案例示範如何將受信任樹系中的宣告轉換為信任樹系中的宣告。 若要這樣做，您需要設定一個名為 adatum.com 的新樹系，並在樹系中填入公司值為 ' Adatum ' 的測試使用者。 接著，您必須設定 contoso.com 與 adatum.com 之間的雙向信任。

> [!IMPORTANT]
> 設定 Contoso 和 Adatum 樹系時，您必須確定兩個根域都位於 Windows Server 2012 網域功能等級，宣告轉換才能夠運作。

您需要為實驗室設定下列各項。 這些程式會在[附錄 B：設定測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)中詳細說明

您必須執行下列程式來設定此案例的實驗室：

1.  [將 Adatum 設定為 Contoso 的信任樹系](Appendix-B--Setting-Up-the-Test-Environment.md)

2.  [在 Contoso 上建立「公司」宣告類型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)

3.  [在 Contoso 上啟用 [公司] 資源屬性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)

4.  [建立集中存取規則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)

5.  [建立集中存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)

6.  [透過群組原則發佈新的原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)

7.  [在檔案伺服器上建立 Earnings 資料夾](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)

8.  [設定分類並在新資料夾上套用集中存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)

使用下列資訊來完成此案例：

|物件|詳細資料|
|-----------|-----------|
|使用者|Jeff Low，Contoso|
|Adatum 和 Contoso 上的使用者宣告|識別碼： ad://ext/Company:ContosoAdatum、<p>來源屬性：公司<p>建議值： Contoso，Adatum**重要事項：** 您必須將 Contoso 和 adatum 上「公司」宣告類型的識別碼設定為相同，宣告轉換才能正常執行。|
|Contoso 上的集中存取規則|AdatumEmployeeAccessRule|
|Contoso 上的集中存取原則|僅限 Adatum 存取原則|
|Adatum 和 Contoso 上的宣告轉換原則|DenyAllExcept 公司|
|Contoso 上的檔案資料夾|D:\EARNINGS|

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>在受信任的樹系上設定宣告轉換 (Adatum) 
在此步驟中，您會在 Adatum 中建立轉換原則，以拒絕「公司」以外的所有宣告傳送至 Contoso。

適用于 Windows PowerShell 的 Active Directory 模組提供**DenyAllExcept**引數，它會捨棄轉換原則中指定宣告以外的所有專案。

若要設定宣告轉換，您需要建立宣告轉換原則，並將它連結到受信任和信任的樹系。

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>在 Adatum 中建立宣告轉換原則

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>建立轉換原則 Adatum 以拒絕所有宣告，但「公司」除外

1. 以系統管理員身分使用密碼登入網域控制站 adatum.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列內容：

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except Company"`
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"adatum.com" `

   ```

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>在 Adatum 的信任網域物件上設定宣告轉換連結
在此步驟中，您會將新建立的宣告轉換原則套用至 Contoso 的 Adatum 信任網域物件。

##### <a name="to-apply-the-claims-transformation-policy"></a>套用宣告轉換原則

1. 以系統管理員身分使用密碼登入網域控制站 adatum.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列內容：

   ```

     Set-ADClaimTransformLink `
   -Identity:"contoso.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   '"TrustRole:Trusted `

   ```

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>在信任樹系中設定宣告轉換 (Contoso) 
在此步驟中，您會在 Contoso (信任樹系中建立宣告轉換原則，) 拒絕「公司」以外的所有宣告。 您必須建立宣告轉換原則，並將它連結至樹系信任。

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>在 Contoso 中建立宣告轉換原則

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>若要建立轉換原則 Adatum 以拒絕所有「公司」以外的所有

1. 以系統管理員身分使用密碼登入網域控制站 contoso.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列內容：

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except company" `
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"contoso.com" `

   ```

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>在 Contoso 的信任網域物件上設定宣告轉換連結
在此步驟中，您會將新建立的宣告轉換原則套用到 Adatum 的 contoso.com 信任網域物件，以允許將 "Company" 傳遞至 contoso.com。 信任網域物件的名稱是 adatum.com。

##### <a name="to-set-the-claims-transformation-policy"></a>若要設定宣告轉換原則

1. 以系統管理員身分使用密碼登入網域控制站 contoso.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列內容：

   ```

     Set-ADClaimTransformLink
   -Identity:"adatum.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   -TrustRole:Trusting `

   ```

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>驗證案例
在此步驟中，您會嘗試存取在檔案伺服器 FILE1 上設定的 D:\EARNINGS 資料夾，以驗證使用者是否具有共用資料夾的存取權。

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>若要確保 Adatum 使用者可以存取共用資料夾

1. 登入用戶端電腦，使用密碼 CLIENT1 為 Jeff Low <strong>pass@word1</strong> 。

2. 流覽至資料夾 \\ \FILE1.contoso.com\Earnings。

3. Jeff Low 應該能夠存取資料夾。

## <a name="additional-scenarios-for-claims-transformation-policies"></a>宣告轉換原則的其他案例
以下是宣告轉換中的其他常見案例清單。


|                                                 狀況                                                 |                                                                                                                                                                                                                                           原則                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  允許來自 Adatum 的所有宣告進入 Contoso Adatum                  |                                                          錯誤碼 <br />新增-ADClaimTransformPolicy\`<br /> -描述：「允許所有宣告的宣告轉換原則」\`<br />-Name： "AllowAllClaimsPolicy"\`<br />-AllowAll\`<br />-Server:"contoso .com"\`<br />設定-ADClaimTransformLink\`<br />-Identity:"adatum .com"\`<br />-Policy： "AllowAllClaimsPolicy"\`<br />-TrustRole：信任\`<br />-Server:"contoso .com"\`                                                          |
|                  拒絕來自 Adatum 的所有宣告，進入 Contoso Adatum                   |                                                            錯誤碼 <br />新增-ADClaimTransformPolicy\`<br />-描述：「拒絕所有宣告的宣告轉換原則」\`<br />-Name： "DenyAllClaimsPolicy"\`<br /> -DenyAll\`<br />-Server:"contoso .com"\`<br />設定-ADClaimTransformLink\`<br />-Identity:"adatum .com"\`<br />-Policy： "DenyAllClaimsPolicy"\`<br />-TrustRole：信任\`<br />-Server:"contoso .com"\`                                                             |
| 允許來自 Adatum 的所有宣告（「公司」和「部門」除外）進入 Contoso Adatum | 程式碼 <br />-新增-ADClaimTransformationPolicy\`<br />-描述：「宣告轉換原則，允許公司和部門以外的所有宣告」\`<br /> -Name： "AllowAllClaimsExceptCompanyAndDepartmentPolicy"\`<br />-AllowAllExcept：公司、部門\`<br />-Server:"contoso .com"\`<br />設定-ADClaimTransformLink\`<br /> -Identity:"adatum .com"\`<br />-Policy： "AllowAllClaimsExceptCompanyAndDepartmentPolicy"\`<br /> -TrustRole：信任\`<br />-Server:"contoso .com"\` |

## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱

-   如需所有可用於宣告轉換的 Windows PowerShell Cmdlet 清單，請參閱[Active Directory PowerShell Cmdlet 參考](https://go.microsoft.com/fwlink/?LinkId=243150)。

-   如需在兩個樹系之間匯出和匯入 DAC 設定資訊的 advanced 工作，請使用[Dynamic 存取控制 PowerShell 參考](https://go.microsoft.com/fwlink/?LinkId=243150)

-   [跨樹系部署宣告](Deploy-Claims-Across-Forests.md)

-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)

-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)


