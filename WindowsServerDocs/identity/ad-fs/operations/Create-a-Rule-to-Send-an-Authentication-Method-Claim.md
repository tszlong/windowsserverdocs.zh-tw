---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: "建立驗證方法理賠要求傳送給規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>建立驗證方法理賠要求傳送給規則

>適用於：Windows Server 2016、Windows Server 2012 R2

您可以使用**傳送群組成員資格宣告以**規則範本或**轉換輸入宣告**傳送驗證方法理賠要求規則範本。 信賴可用來判斷使用者使用驗證，並取得宣告 Active Directory 同盟服務 \(AD FS\) 從登入機制驗證方法理賠要求。 您也可以使用 Active Directory 同盟服務 \(AD FS\) 驗證機制保證」的功能在 Windows Server 2012 R2 做為輸入產生驗證方法宣告供信賴要判斷智慧卡登入為基礎的存取層級。 例如開發人員可以指定聯盟使用者信賴方應用程式的不同層級的存取。 層級的存取權限依據是否使用者使用認證登入他們使用者名稱和密碼，而不是智慧卡。  
  
根據您的組織的需求，使用下列程序其中一項：  
  
-   此規則建立使用**為宣告傳送群組成員資格**規則範本 \-想指定最終判斷驗證方法此範本群組取得發行時，您可以使用此規則範本。  
  
-   此規則建立使用**轉換連入宣告**規則範本 \-當您想要變更新的驗證方法可以搭配無法辨識標準 AD FS 驗證方法宣告 product 現有的驗證方法，您可以使用此規則範本。  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用傳送群組成員資格為宣告規則範本可以方信任 Windows Server 2016 上建立 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**為理賠要求傳送給群組成員資格**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下**瀏覽]**，選取的群組成員應該收到此驗證方法宣告、，然後按一下 [ **[確定]**。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 在**傳出宣告值**、輸入其中一個預設統一資源識別碼 \(URI\) 值表，根據您慣用的驗證方法、按一下 [**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸使用 x.509 層安全性 \(TLS\) 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立藉由傳送群組成員資格使用在 Windows Server 2016 宣告提供者信任宣告規則範本 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**為理賠要求傳送給群組成員資格**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下**瀏覽]**，選取的群組成員應該收到此驗證方法宣告、，然後按一下 [ **[確定]**。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 在**傳出宣告值**、輸入其中一個預設統一資源識別碼 \(URI\) 值表，根據您慣用的驗證方法、按一下 [**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸使用 x.509 層安全性 \(TLS\) 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用轉換建立此規則傳入取得可以方信任 Windows Server 2016 上的 [規則範本 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**轉換連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**的驗證方法**清單中。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 選取 [**以不同的傳出宣告值取代傳入宣告值**，然後執行下列：  
  
    1.  在**傳入取得值**，輸入下列其中 URI 值為基礎的實際的驗證方法原先 URI 後，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
    2.  在**傳出宣告值**，其中一個預設 URI 值在下表，您全新的慣用的驗證方法選擇而定，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 x.509 TLS 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> 除了表中的值可以使用其他 URI 值。 顯示中心 URI 值一個表格反映信賴接受預設 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用轉換建立此規則傳入取得在 Windows Server 2016 宣告提供者信任規則範本 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**轉換連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**的驗證方法**清單中。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 選取 [**以不同的傳出宣告值取代傳入宣告值**，然後執行下列：  
  
    1.  在**傳入取得值**，輸入下列其中 URI 值為基礎的實際的驗證方法原先 URI 後，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
    2.  在**傳出宣告值**，其中一個預設 URI 值在下表，您全新的慣用的驗證方法選擇而定，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 x.509 TLS 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>此規則建立藉由傳送群組成員資格使用在 Windows Server 2012 R2 宣告規則範本  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS\\Trust 關係**，按一下**宣告提供者信任**或**可以廠商信任**，，然後按一下 [特定信任在清單中您想要用來建立本規則。  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在**編輯理賠要求規則**對話方塊中，選取其中一個下列索引標籤，根據您正在編輯，並設定您的規則信任想要建立單元，此規則，然後按一下**新增規則**以開始規則該組相關聯的規則精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發行授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**為理賠要求傳送給群組成員資格**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下**瀏覽]**，選取的群組成員應該收到此驗證方法宣告、，然後按一下 [ **[確定]**。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 在**傳出宣告值**、輸入其中一個預設統一資源識別碼 \(URI\) 值表，根據您慣用的驗證方法、按一下 [**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸使用 x.509 層安全性 \(TLS\) 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> 除了表中的值可以使用其他 URI 值。 先前表所示 URI 值反映信賴接受預設 Uri。  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>此規則使用建立轉換連入宣告規則範本在 Windows Server 2012 R2  
   
  
  
1.  在伺服器管理員中，按一下**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\\Trust 關係**，按一下**宣告提供者信任**或**可以廠商信任**，，然後按一下 [特定信任在清單中您想要用來建立本規則。  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  在**編輯理賠要求規則**對話方塊中，選取其中一種下列索引標籤，而定信任您正在編輯，並在哪一個規則設定您想要建立本規則，然後按一下 [ **[新增規則**以開始規則該組相關聯的規則精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發行授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**轉換連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**的驗證方法**清單中。  
  
8.  在**傳出宣告類型**、**的驗證方法**清單中。  
  
9. 選取 [**以不同的傳出宣告值取代傳入宣告值**，然後執行下列：  
  
    1.  在**傳入取得值**，輸入下列其中 URI 值為基礎的實際的驗證方法原先 URI 後，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
    2.  在**傳出宣告值**，其中一個預設 URI 值在下表，您全新的慣用的驗證方法選擇而定，按**完成**，，然後按一下 [ **[確定]**儲存規則。  
  
|實際的驗證方法|對應 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 x.509 TLS 互加好友驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 式驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> 除了表中的值可以使用其他 URI 值。 顯示中心 URI 值一個表格反映信賴接受預設 Uri。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
