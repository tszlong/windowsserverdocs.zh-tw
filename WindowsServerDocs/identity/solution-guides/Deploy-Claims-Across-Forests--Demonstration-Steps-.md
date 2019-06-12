---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: 跨樹系部署宣告 (示範步驟)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a6f2e5d3a227384b20735ab99ee1ab5ea77bd913
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445839"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>跨樹系部署宣告 (示範步驟)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題中，我們將涵蓋基本的案例，說明如何設定信任和受信任的樹系之間的宣告轉換。 您將了解可以如何建立並連結至信任的樹系和受信任的樹系信任的宣告轉換原則物件。 然後，您將驗證案例。  

## <a name="scenario-overview"></a>案例概觀  
Adatum Corporation 提供給 Contoso，Ltd.的金融服務每一季 Adatum 會計師就會將其帳戶試算表複製到位於 Contoso，Ltd.的檔案伺服器上的資料夾沒有設定從 Contoso adatum 的雙向信任關係。 Contoso，Ltd.想要保護的共用，這樣一來，只有 Adatum 員工可以存取遠端共用。  

在這個案例中：  

1.  [設定必要條件和測試環境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [設定 受信任的樹系 (Adatum) 中的 宣告轉型](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [在 信任的樹系 (Contoso) 中設定宣告轉換](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [驗證案例](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="BKMK_1.1"></a>設定必要條件和測試環境  
測試組態牽涉到兩個樹系的設定：Adatum Corporation 和 Contoso，Ltd，而 Contoso 及 Adatum 間建立雙向信任。 "adatum.com"是受信任的樹系，"contoso.com"信任的樹系。  

宣告轉換案例中示範受信任的樹系中的宣告，以信任的樹系中的宣告轉換。 若要這樣做，您需要設定稱為 adatum.com 的新樹系並填入具有公司值是 'Adatum' 的測試使用者的樹系。 然後，您必須設定 contoso.com 和 adatum.com 之間的雙向信任。  

> [!IMPORTANT]  
> 在 Contoso 及 Adatum 樹系時，您必須確定這兩個根網域是在 Windows Server 2012 網域功能等級的宣告轉型來運作。  

您需要下列實驗室的設定。 中的詳細說明這些程序[附錄 b:設定測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)  

您需要實作下列程序，設定此案例中實驗室：  

1.  [將 Adatum 設為受信任的樹系至 Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [建立 Contoso 上的 'Company' 宣告類型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [啟用在 Contoso 上的 'Company' 資源內容](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [建立集中存取規則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [建立集中存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [發佈新的原則，透過群組原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [檔案伺服器上建立 Earnings 資料夾](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [設定分類，並將集中存取原則套用新的資料夾](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

若要完成此案例中使用下列資訊：  

|Objects|詳細資料|  
|-----------|-----------|  
|使用者|Jeff Low Contoso|  
|Adatum 和 Contoso 上的使用者宣告|ID: ad://ext/Company:ContosoAdatum,<br /><br />來源屬性： 公司<br /><br />建議的值：Contoso，Adatum**重要：** 您必須在 Contoso 和 Adatum 宣告轉型為相同工作上設定上的 'Company' 的識別碼宣告類型。|  
|在 Contoso 上的集中存取規則|AdatumEmployeeAccessRule|  
|在 Contoso 上的集中存取原則|Adatum 唯一的存取原則|  
|Adatum 和 Contoso 的宣告轉換原則|DenyAllExcept Company|  
|在 Contoso 上的檔案資料夾|D:\EARNINGS|  

## <a name="BKMK_3"></a>設定 受信任的樹系 (Adatum) 中的 宣告轉型  
在此步驟中，您會建立在 Adatum 拒絕要傳遞至 Contoso 的 'Company' 以外的所有宣告轉換原則。  

提供適用於 Windows PowerShell 的 Active Directory 模組**DenyAllExcept**引數，卸除指定的宣告以外的所有內容中的轉換原則。  

若要設定宣告轉型，您需要建立宣告轉換原則並連結到受信任且信任的樹系之間。  

### <a name="BKMK_2.2"></a>建立 Adatum 中的宣告轉換原則  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>建立 Adatum 拒絕 'Company' 以外的所有宣告轉換原則  

1. 登入網域控制站，以系統管理員身分使用密碼的 adatum.com <strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中，開啟提升權限的命令提示字元並輸入下列命令：  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="BKMK_2.3"></a>Adatum 的信任網域物件上設定的宣告轉換連結  
在此步驟中，您套用的新建立的宣告轉換原則 Adatum 的信任的網域物件上的 Contoso。  

##### <a name="to-apply-the-claims-transformation-policy"></a>若要套用的宣告轉換原則  

1. 登入網域控制站，以系統管理員身分使用密碼的 adatum.com <strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中，開啟提升權限的命令提示字元並輸入下列命令：  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="BKMK_4"></a>在 信任的樹系 (Contoso) 中設定宣告轉換  
在此步驟中，您建立來拒絕 'Company。' 以外的所有宣告的 Contoso （信任的樹系） 中的宣告轉換原則 您要建立宣告轉換原則，並將它連結到樹系信任。  

### <a name="BKMK_4.1"></a>建立 Contoso 中的宣告轉換原則  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>建立 Adatum 拒絕 'Company' 以外的所有轉換原則  

1. 登入網域控制站，以系統管理員身分使用密碼的 contoso.com <strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中開啟提升權限的命令提示字元並輸入下列命令：  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="BKMK_4.2"></a>Contoso 信任網域物件上設定的宣告轉換連結  
在此步驟中，您要套用新建立宣告轉換原則以允許 「 公司 」 的 Adatum contoso.com 信任網域物件上傳遞到 contoso.com。 信任的網域物件稱為 adatum.com。  

##### <a name="to-set-the-claims-transformation-policy"></a>若要設定的宣告轉換原則  

1. 登入網域控制站，以系統管理員身分使用密碼的 contoso.com <strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中開啟提升權限的命令提示字元並輸入下列命令：  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="BKMK_5"></a>驗證案例  
在此步驟中，您嘗試存取已設定檔案伺服器 FILE1 驗證使用者擁有的共用資料夾的權限 D:\EARNINGS 資料夾。  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>若要確認 Adatum 使用者可以存取共用的資料夾  

1. 登入用戶端電腦，Jeff Low，密碼為 CLIENT1 <strong>pass@word1</strong>。  

2. 瀏覽至資料夾\\\FILE1.contoso.com\Earnings。  

3. Jeff Low，應該能夠存取的資料夾。  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>宣告轉換原則的其他案例  
以下是宣告轉型中的其他常見案例的清單。  


|                                                 狀況                                                 |                                                                                                                                                                                                                                           原則                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  允許所有來自 傳播至 Contoso Adatum Adatum 的宣告                  |                                                          Code - <br />New-ADClaimTransformPolicy \`<br /> 描述: 「 宣告轉換原則以允許所有的宣告 」 \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-「 全部允許 」 \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole： 信任 \`<br />-Server:"contoso.com" \`                                                          |
|                  拒絕來自 傳播至 Contoso Adatum Adatum 的所有宣告                   |                                                            Code - <br />New-ADClaimTransformPolicy \`<br />描述: 「 宣告轉換原則以拒絕所有的宣告 」 \`<br />-Name:"DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"DenyAllClaimsPolicy" \`<br />-TrustRole： 信任 \`<br />-Server:"contoso.com"\`                                                             |
| 允許來自 Adatum 「 公司 」 和 「 部門 」 傳播至 Contoso Adatum 以外的所有宣告 | 程式碼 <br />- New-ADClaimTransformationPolicy \`<br />描述: 「 宣告轉換原則以允許所有公司和部門以外的宣告 」 \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept:company,department \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole： 信任 \`<br />-Server:"contoso.com" \` |

## <a name="BKMK_Links"></a>另請參閱  

-   如需適用於宣告轉換的所有 Windows PowerShell cmdlet 的清單，請參閱 < [Active Directory PowerShell Cmdlet 參考](https://go.microsoft.com/fwlink/?LinkId=243150)。  

-   對於涉及匯出和匯入的 DAC 之間兩個樹系的組態資訊的進階工作，使用[動態存取控制 PowerShell 參考](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [跨樹系部署宣告](Deploy-Claims-Across-Forests.md)  

-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)  

-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  


