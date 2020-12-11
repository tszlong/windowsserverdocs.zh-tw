---
description: '深入瞭解：跨樹系部署宣告 (示範步驟) '
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: 跨樹系部署宣告 (示範步驟)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 16f175baade8cffb16225ef195edf425762e839b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048516"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>跨樹系部署宣告 (示範步驟)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在本主題中，我們將討論說明如何設定信任和信任樹系之間宣告轉換的基本案例。 您將瞭解如何建立宣告轉換原則物件，並將其連結至信任樹系和受信任樹系上的信任。 然後，您將會驗證案例。

## <a name="scenario-overview"></a>案例概觀
Adatum Corporation 提供金融服務給 Contoso，公司。Adatum 的每一季都會將其帳戶試算表複製到位於 Contoso，公司的檔案伺服器上的資料夾。有從 Contoso 到 Adatum 的雙向信任設定。 Contoso，公司想要保護共用，因此只有 Adatum 員工可以存取遠端共用。

在此情節中：

1.  [設定必要條件和測試環境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)

2.  [設定信任樹系上的宣告轉換 (Adatum) ](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)

3.  [設定信任樹系中的宣告轉換 (Contoso) ](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)

4.  [驗證案例](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>設定必要條件和測試環境
測試設定牽涉到設定兩個樹系： Adatum Corporation 和 Contoso、公司，以及 Contoso 和 Adatum 之間的雙向信任。 "adatum.com" 是信任的樹系，而 "contoso.com" 是信任的樹系。

宣告轉換案例示範如何將信任樹系中的宣告轉換成信任樹系中的宣告。 若要這樣做，您必須設定名為 adatum.com 的新樹系，並將具有公司值 ' Adatum ' 的測試使用者填入樹系。 然後，您必須設定 contoso.com 與 adatum.com 之間的雙向信任。

> [!IMPORTANT]
> 設定 Contoso 和 Adatum 樹系時，您必須確定兩個根域都位於 Windows Server 2012 網域功能等級，宣告轉換才能運作。

您必須為實驗室設定下列各項。 [附錄 B：設定測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)中會詳細說明這些程式

您必須執行下列程式來設定此案例的實驗室：

1.  [將 Adatum 設為信任的樹系至 Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)

2.  [在 Contoso 上建立「公司」宣告類型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)

3.  [在 Contoso 上啟用「公司」資源屬性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)

4.  [建立集中存取規則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)

5.  [建立集中存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)

6.  [透過群組原則發佈新的原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)

7.  [在檔案伺服器上建立 Earnings 資料夾](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)

8.  [設定分類，並將集中存取原則套用到新資料夾](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)

您可以使用下列資訊來完成此案例：

|物件|詳細資料|
|-----------|-----------|
|使用者|Contoso 低，Contoso|
|Adatum 和 Contoso 上的使用者宣告|識別碼： ad://ext/Company:ContosoAdatum、<p>來源屬性：公司<p>建議值： Contoso，Adatum **重要：** 您必須將 Contoso 和 adatum 上「公司」宣告類型的識別碼設定為相同，宣告轉換才能運作。|
|Contoso 上的集中存取規則|AdatumEmployeeAccessRule|
|Contoso 上的集中存取原則|僅限 Adatum 存取原則|
|Adatum 和 Contoso 上的宣告轉換原則|DenyAllExcept 公司|
|Contoso 上的檔案資料夾|D:\EARNINGS|

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>設定信任樹系上的宣告轉換 (Adatum) 
在此步驟中，您會在 Adatum 中建立轉換原則，以拒絕將 ' Company ' 傳遞給 Contoso 以外的所有宣告。

適用于 Windows PowerShell 的 Active Directory 模組提供 **DenyAllExcept** 引數，此引數會卸載轉換原則中指定的宣告以外的所有專案。

若要設定宣告轉換，您需要建立宣告轉換原則，並將它連結至受信任和信任的樹系。

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>在 Adatum 中建立宣告轉換原則

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>若要建立轉換原則 Adatum 以拒絕「公司」以外的所有宣告

1. 以系統管理員身分登入網域控制站，並以密碼 adatum.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列命令：

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except Company"`
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"adatum.com" `

   ```

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>在 Adatum 的信任網域物件上設定宣告轉換連結
在此步驟中，您會在適用于 Contoso 的 Adatum 信任網域物件上套用新建立的宣告轉換原則。

##### <a name="to-apply-the-claims-transformation-policy"></a>套用宣告轉換原則

1. 以系統管理員身分登入網域控制站，並以密碼 adatum.com  <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列命令：

   ```

     Set-ADClaimTransformLink `
   -Identity:"contoso.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   '"TrustRole:Trusted `

   ```

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>設定信任樹系中的宣告轉換 (Contoso) 
在此步驟中，您會在 Contoso (信任樹系中建立宣告轉換原則，) 拒絕「公司」以外的所有宣告。 您必須建立宣告轉換原則，並將它連結至樹系信任。

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>在 Contoso 中建立宣告轉換原則

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>建立轉換原則 Adatum 以拒絕「公司」以外的所有

1. 以系統管理員身分登入網域控制站，並以密碼 contoso.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列命令：

   ```
   New-ADClaimTransformPolicy `
   -Description:"Claims transformation policy to deny all claims except company" `
   -Name:"DenyAllClaimsExceptCompanyPolicy" `
   -DenyAllExcept:company `
   -Server:"contoso.com" `

   ```

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>在 Contoso 的信任網域物件上設定宣告轉換連結
在此步驟中，您會將新建立的宣告轉換原則套用到 Adatum 的 contoso.com trust 網域物件上，以允許將「公司」傳遞到 contoso.com。 信任網域物件的名稱是 adatum.com。

##### <a name="to-set-the-claims-transformation-policy"></a>設定宣告轉換原則

1. 以系統管理員身分登入網域控制站，並以密碼 contoso.com <strong>pass@word1</strong> 。

2. 在 Windows PowerShell 中開啟提升許可權的命令提示字元，然後輸入下列命令：

   ```

     Set-ADClaimTransformLink
   -Identity:"adatum.com" `
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `
   -TrustRole:Trusting `

   ```

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>驗證案例
在此步驟中，您會嘗試存取在檔案伺服器 FILE1 上設定的 D:\EARNINGS 資料夾，以驗證使用者是否可存取共用資料夾。

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>確認 Adatum 使用者可以存取共用資料夾

1. 登入用戶端電腦，使用密碼 CLIENT1 為 Jeff 低 <strong>pass@word1</strong> 。

2. 流覽至資料夾 \\ \FILE1.contoso.com\Earnings。

3. Jeff 低應能存取資料夾。

## <a name="additional-scenarios-for-claims-transformation-policies"></a>宣告轉換原則的其他案例
以下是宣告轉換中的其他常見案例清單。


|                                                 案例                                                 |                                                                                                                                                                                                                                           原則                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  允許來自 Adatum 的所有宣告進入 Contoso Adatum                  |                                                          準則 <br />New-ADClaimTransformPolicy \`<br /> -Description：「宣告轉換原則以允許所有宣告」 \`<br />-Name： "AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso .com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum .com" \`<br />-Policy： "AllowAllClaimsPolicy" \`<br />-TrustRole：信任 \`<br />-Server:"contoso .com" \`                                                          |
|                  拒絕來自 Adatum 到 Contoso Adatum 的所有宣告                   |                                                            準則 <br />New-ADClaimTransformPolicy \`<br />-Description： "宣告轉換原則以拒絕所有宣告" \`<br />-Name： "DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso .com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum .com" \`<br />-Policy： "DenyAllClaimsPolicy" \`<br />-TrustRole：信任 \`<br />-Server:"contoso .com"\`                                                             |
| 允許來自 Adatum 的所有宣告（「公司」和「部門」除外）移至 Contoso Adatum | 程式碼 <br />-New-ADClaimTransformationPolicy \`<br />-Description：「宣告轉換原則以允許公司和部門以外的所有宣告」 \`<br /> -Name： "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept：公司、部門 \`<br />-Server:"contoso .com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum .com" \`<br />-Policy： "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole：信任 \`<br />-Server:"contoso .com" \` |

## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱

-   如需所有可用於宣告轉換的 Windows PowerShell Cmdlet 清單，請參閱 [Active Directory PowerShell Cmdlet 參考](https://go.microsoft.com/fwlink/?LinkId=243150)。

-   針對涉及在兩個樹系之間匯出和匯入 DAC 設定資訊的 advanced 工作，請使用 [動態存取控制 PowerShell 參考](https://go.microsoft.com/fwlink/?LinkId=243150)

-   [跨樹系部署宣告](Deploy-Claims-Across-Forests.md)

-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)

-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)


