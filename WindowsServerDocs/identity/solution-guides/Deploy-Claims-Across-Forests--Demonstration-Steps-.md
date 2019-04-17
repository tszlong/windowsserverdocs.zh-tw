---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: "部署宣告跨樹系（示範步驟）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>部署宣告跨樹系（示範步驟）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題中，我們將實體鍵盤保護蓋如何設定宣告轉換信任和受信任的樹系之間的基本案例。 您會了解可以如何建立並連結到信任上信任的樹系受信任的樹系宣告轉換原則物件。 然後，您將驗證案例。  
  
## <a name="scenario-overview"></a>案例概觀  
Adatum 公司提供金融服務，以 Contoso，ltd.每個季 Adatum 會計複製他們 account 試算表位於 Contoso，ltd.檔案伺服器上的資料夾還有雙向信任 Adatum Contoso 的設定。 Contoso，ltd.想要保護分享，以便僅 Adatum 員工可以存取遠端分享。  
  
在本案例：  
  
1.  [設定的必要元件以及測試環境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [設定宣告轉換上受信任的樹系 (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [在信任的樹系 (Contoso) 設定宣告轉換](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [驗證案例](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>設定的必要元件以及測試環境  
測試設定包括有兩個森林設定：Adatum Corporation Contoso，Ltd.，並有雙向 Contoso 之間 Adatum 信任。 「adatum.com「受信任的樹系，「contoso.com「且信任的樹系。  
  
宣告轉換案例示範轉換到宣告信任的樹系的受信任的樹系理賠要求。 若要這樣做，您需要設定新的樹系稱為 adatum.com 填入樹系公司值為 'Adatum' 測試使用者使用。 然後，您必須設定雙向 contoso.com 之間 adatum.com 信任。  
  
> [!IMPORTANT]  
> 當設定 Contoso 和 Adatum 森林，您必須確定兩根網域網域層級 Windows Server 2012 功能宣告轉換運作。  
  
您需要設定為實驗室下列。 下列程序的詳述在[附錄 b 設定好的測試環境](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
您必須實作下列程序，設定實驗室本案例：  
  
1.  [將 Adatum 設定為受信任的樹系，以 Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [建立 Contoso '公司' 宣告類型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [讓 Contoso '公司' 資源屬性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [建立的中央存取規則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [建立的中央存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [發行新原則透過群組原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [獲利伺服器上建立資料夾的檔案](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [設定分類與套用新資料夾的中央存取原則](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
若要完成此案例，使用下列資訊：  
  
|物件|詳細資料|  
|-----------|-----------|  
|使用者|Jeff 低，以 Contoso|  
|Adatum 和 Contoso 使用者宣告|ID: ad: / / ext 日公司：ContosoAdatum，<br /><br />來源屬性：公司<br /><br />建議的值：Contoso、Adatum**重要事項：**您必須設定 ID 對 '公司' 宣告類型 Contoso 和 Adatum 相同宣告轉換運作。|  
|在 Contoso 的中央存取規則|AdatumEmployeeAccessRule|  
|Contoso 中央存取原則|只存取原則 Adatum|  
|宣告 Adatum，以 Contoso 轉換原則|DenyAllExcept 公司|  
|檔案 Contoso 資料夾|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>設定宣告轉換上受信任的樹系 (Adatum)  
在此步驟建立 Adatum 拒絕 '公司' 以外的所有宣告傳遞至 Contoso 轉換原則。  
  
Windows PowerShell 模組 Active Directory 提供**DenyAllExcept**會指定宣告以外的所有項目中的轉換原則卸除引數。  
  
宣告轉換設定，您需要建立宣告轉換原則，並將它信任及信任的樹系之間的連結。  
  
### <a name="BKMK_2.2"></a>建立 Adatum 宣告轉換原則  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>若要建立轉換原則 Adatum 拒絕 '公司' 以外的所有宣告  
  
1.  網域控制站 adatum.com 以系統管理員身分使用密碼登入**pass@word1**。  
  
2.  打開提升權限的命令提示字元中，Windows PowerShell 中，輸入下列：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>設定 Adatum 的信任的網域物件宣告轉換連結  
在此步驟，您適用的新建立的宣告轉換原則 Adatum 的信任的網域物件 Contoso。  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>若要套用宣告轉換原則  
  
1.  網域控制站 adatum.com 以系統管理員身分使用密碼登入**pass@word1**。  
  
2.  打開提升權限的命令提示字元中，Windows PowerShell 中，輸入下列：  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>在信任的樹系 (Contoso) 設定宣告轉換  
您可以在此步驟建立拒絕 '公司。' 以外的所有宣告 Contoso（信任的樹系）宣告轉換原則 您需要建立宣告轉換原則連結到信任的樹系。  
  
### <a name="BKMK_4.1"></a>建立 Contoso 宣告轉換原則  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>若要建立轉換原則 Adatum 拒絕 '公司' 的全部項目  
  
1.  網域控制站 contoso.com 以系統管理員身分使用密碼登入**pass@word1**。  
  
2.  Windows PowerShell 中開放提升權限的命令提示字元中，輸入下列：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>設定 Contoso 信任的網域物件宣告轉換連結  
在此步驟，您將套用新建立宣告轉換原則 contoso.com 信任網域物件的 Adatum 允許」公司」會通過 contoso.com。信任的網域物件命名 adatum.com。  
  
##### <a name="to-set-the-claims-transformation-policy"></a>若要設定宣告轉換原則  
  
1.  網域控制站 contoso.com 以系統管理員身分使用密碼登入**pass@word1**。  
  
2.  Windows PowerShell 中開放提升權限的命令提示字元中，輸入下列：  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>驗證案例  
在此步驟您嘗試存取檔案伺服器 1 設定要驗證使用者的共用資料夾存取 D:\EARNINGS 資料夾。  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>若要確保 Adatum 使用者都可以存取的共用的資料夾  
  
1.  Client 電腦 CLIENT1 為 Jeff 低使用密碼登入**pass@word1**。  
  
2.  瀏覽至資料夾 \\\FILE1.contoso.com\Earnings。  
  
3.  Jeff 低才能存取該資料夾。  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>宣告轉換原則針對其他案例  
以下是清單額外的常見案例中宣告轉換。  
  
|案例|原則|  
|------------|----------|  
|允許所有來自前往 Contoso Adatum Adatum 宣告|程式碼- <br />New-ADClaimTransformPolicy \]<br /> 描述:「宣告轉換原則，以允許所有宣告」\]<br />-名稱:「AllowAllClaimsPolicy「\]<br />「全部允許-」\]<br />伺服器:「contoso.com「\]<br />Set-ADClaimTransformLink \]<br />-身分:「adatum.com「\]<br />原則:「AllowAllClaimsPolicy「\]<br />-TrustRole：信任 \]<br />伺服器:「contoso.com「'|  
|拒絕來自前往 Contoso Adatum Adatum 所有宣告|程式碼- <br />New-ADClaimTransformPolicy \]<br />描述:「宣告拒絕所有宣告轉換原則」\]<br />-名稱:「DenyAllClaimsPolicy「\]<br /> -DenyAll \]<br />伺服器:「contoso.com「\]<br />Set-ADClaimTransformLink \]<br />-身分:「adatum.com「\]<br />原則:「DenyAllClaimsPolicy「\]<br />-TrustRole：信任 \]<br />伺服器:「contoso.com「'|  
|允許來自 Adatum」公司」和「部門「前往 Contoso Adatum 以外的所有宣告|程式碼 <br />-New-ADClaimTransformationPolicy \]<br />描述:「宣告轉換原則，以允許所有宣告以外的公司和部門「\]<br /> -名稱:「AllowAllClaimsExceptCompanyAndDepartmentPolicy「\]<br />-AllowAllExcept：公司、部門 \]<br />伺服器:「contoso.com「\]<br />Set-ADClaimTransformLink \]<br /> -身分:「adatum.com「\]<br />原則:「AllowAllClaimsExceptCompanyAndDepartmentPolicy「\]<br /> -TrustRole：信任 \]<br />伺服器:「contoso.com「'|  
  
## <a name="BKMK_Links"></a>也了  
  
-   適用於所有 Windows PowerShell cmdlet 可供宣告轉換的清單，請查看[Active Directory PowerShell Cmdlet 參考](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
-   進階任務涉及匯出與匯入的兩個樹系之間 DAC 設定資訊，請使用[動態存取控制 PowerShell 參考資料](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [跨樹系部署宣告](Deploy-Claims-Across-Forests.md)  
  
-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  

