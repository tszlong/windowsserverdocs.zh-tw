---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: 建立規則傳送驗證方法相容宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be3a16bac9c146637117aa7b9720cb4aa76177e2
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189394"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>建立規則傳送驗證方法相容宣告


您可以使用**傳送做為宣告的群組成員資格**規則範本或有**轉換傳入宣告**傳送的驗證方法宣告規則範本。 信賴憑證者的合作對象可以使用的驗證方法宣告，來判斷使用者用來驗證，並取得登入機制宣告從 Active Directory Federation Services \(AD FS\)。 您也可以使用 Active Directory Federation Services 的驗證機制保證功能\(AD FS\)在 Windows Server 2012 R2 做為輸入，在其中產生的情況下的驗證方法宣告中的信賴憑證者的合作對象想要判斷智慧卡登入為基礎的存取層級。 比方說，開發人員可以將不同存取層級指派給信賴憑證者的合作對象應用程式的同盟使用者。 存取的層級取決於是否在使用者登入時使用其使用者名稱和密碼認證，而不是其智慧卡。  
  
根據您組織的需求，請使用下列程序的其中一個：  
  
-   建立此規則使用 **傳送做為宣告的群組成員資格**規則範本\-當您想要的群組，您最後指定此範本，以判斷何種驗證方法時，您可以使用此規則範本要發出宣告。  
  
-   建立此規則使用 **傳輸傳入宣告**規則範本\-想要將現有的驗證方法變更為新的驗證方法，可以使用產品時，您可以使用此規則範本無法辨識標準的 AD FS 驗證方法宣告。  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要利用傳送群組成員資格為信賴憑證者信任 Windows Server 2016 中的宣告規則範本建立 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下 **瀏覽**，選取的群組的成員應該接收此驗證方法宣告，然後按一下**確定**。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 在**傳出宣告值**，輸入預設的統一資源識別項的其中一個\(URI\)值在下列資料表中，根據您慣用的驗證方法中，按一下 **完成**，然後按一下 **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸層安全性\(TLS\)會使用 X.509 憑證的相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要利用傳送群組成員資格為宣告提供者信任 Windows Server 2016 中的宣告規則範本建立 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下 **瀏覽**，選取的群組的成員應該接收此驗證方法宣告，然後按一下**確定**。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 在**傳出宣告值**，輸入預設的統一資源識別項的其中一個\(URI\)值在下列資料表中，根據您慣用的驗證方法中，按一下 **完成**，然後按一下 **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸層安全性\(TLS\)會使用 X.509 憑證的相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立此規則使用 轉換傳入宣告規則範本，在信賴憑證者信任 Windows Server 2016 中 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**驗證方法**清單中。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 選取 **傳入宣告值取代為不同的傳出宣告值**，然後執行下列操作：  
  
    1.  在 **連入宣告值**，輸入下列 URI 值之一，根據實際的驗證方法原本使用的 URI，按一下**完成**，然後按一下  **確定** 以儲存規則。  
  
    2.  在 **傳出宣告值**，輸入其中一個預設 URI 值取決於您新的慣用的驗證方法選擇下表中，按一下**完成**，然後按一下  **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|會使用 X.509 憑證的 TLS 相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> 除了資料表中的值可使用其他 URI 值。 會顯示 ion URI 值上表反映接受預設的信賴憑證者的合作對象的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立此規則使用 轉換傳入宣告在宣告提供者信任 Windows Server 2016 中的規則範本 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**驗證方法**清單中。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 選取 **傳入宣告值取代為不同的傳出宣告值**，然後執行下列操作：  
  
    1.  在 **連入宣告值**，輸入下列 URI 值之一，根據實際的驗證方法原本使用的 URI，按一下**完成**，然後按一下  **確定** 以儲存規則。  
  
    2.  在 **傳出宣告值**，輸入其中一個預設 URI 值取決於您新的慣用的驗證方法選擇下表中，按一下**完成**，然後按一下  **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|會使用 X.509 憑證的 TLS 相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>若要建立此規則使用 傳送群組成員資格為 Windows Server 2012 R2 中的宣告規則範本  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄中下, **AD FS\\信任關係**，按一下**宣告提供者信任**或是**信賴憑證者信任**，然後按一下 特定您要建立此規則的清單中的信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取其中一個下列索引標籤，根據信任您要編輯，以及哪一個規則可設定您要建立，此規則，然後按一下**加入規則**啟動規則該規則集相關聯的精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**以宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  按一下 **瀏覽**，選取的群組的成員應該接收此驗證方法宣告，然後按一下**確定**。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 在**傳出宣告值**，輸入預設的統一資源識別項的其中一個\(URI\)值在下列資料表中，根據您慣用的驗證方法中，按一下 **完成**，然後按一下 **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|傳輸層安全性\(TLS\)會使用 X.509 憑證的相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> 除了資料表中的值可使用其他 URI 值。 上表所示的 URI 值會反映接受預設的信賴憑證者的合作對象的 Uri。  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要建立此規則使用轉換傳入宣告規則範本在 Windows Server 2012 R2  
   
  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中下, **AD FS\\信任關係**，按一下**宣告提供者信任**或是**信賴憑證者信任**，然後按一下 特定您要建立此規則的清單中的信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  在 **編輯宣告規則**對話方塊中，選取其中一個下列索引標籤中，這取決於您要編輯，並在哪一個規則中設定您的信任要建立這項規則，然後按一下**新增規則**啟動規則該規則集相關聯的精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**驗證方法**清單中。  
  
8.  在 **連出的宣告型別**，選取**驗證方法**清單中。  
  
9. 選取 **傳入宣告值取代為不同的傳出宣告值**，然後執行下列操作：  
  
    1.  在 **連入宣告值**，輸入下列 URI 值之一，根據實際的驗證方法原本使用的 URI，按一下**完成**，然後按一下  **確定** 以儲存規則。  
  
    2.  在 **傳出宣告值**，輸入其中一個預設 URI 值取決於您新的慣用的驗證方法選擇下表中，按一下**完成**，然後按一下  **確定** 儲存規則。  
  
|實際的驗證方法|對應的 URI|  
|--------------------------------|---------------------|  
|使用者名稱和密碼驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|會使用 X.509 憑證的 TLS 相互驗證|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基礎的驗證，不會使用 TLS|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> 除了資料表中的值可使用其他 URI 值。 會顯示 ion URI 值上表反映接受預設的信賴憑證者的合作對象的 Uri。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：為宣告提供者信任建立宣告規則](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
