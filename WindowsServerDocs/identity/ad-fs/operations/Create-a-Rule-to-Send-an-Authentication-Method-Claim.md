---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: 建立規則傳送驗證方法相容宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4efcae02b96904c9f869a5ed9e14eba161892b74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358165"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>建立規則傳送驗證方法相容宣告


您可以使用 [**以宣告方式傳送群組成員資格**] 規則範本或 [**轉換傳入**宣告] 規則範本來傳送驗證方法宣告。 信賴憑證者可以使用驗證方法宣告來判斷使用者用來驗證和取得來自 Active Directory 同盟服務 \(AD FS\)之宣告的登入機制。 您也可以使用 Windows Server 2012 R2 中的 Active Directory 同盟服務 \(AD FS\) 的驗證機制保證功能作為輸入，以產生驗證方法宣告，以便信賴憑證者想要判斷以智慧卡登入為基礎的存取層級。 例如，開發人員可以為信賴憑證者應用程式的同盟使用者指派不同層級的存取權。 存取層級取決於使用者是否以其使用者名稱和密碼認證登入，而不是使用其智慧卡。  

視組織的需求而定，請使用下列其中一個程式：  

-   使用 [**以宣告方式傳送群組成員資格**] 規則範本建立此規則 \- 當您想要在此範本中指定的群組最後判斷要發出的驗證方法時，可以使用此規則範本。  

-   使用**轉換傳入**宣告規則範本來建立此規則 \- 當您想要將現有的驗證方法變更為與無法辨識標準 AD FS 驗證方法宣告的產品搭配使用的新驗證方法時，可以使用此規則範本。  



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用 Windows Server 2016 中的信賴憑證者信任上的 [以宣告方式傳送群組成員資格] 規則範本來建立 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**以宣告方式傳送群組成員資格**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  按一下 **[流覽]** ，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]** 。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 在 [**傳出宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的統一資源識別項 \(URI\) 值，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 傳輸層安全性 \(TLS\) 使用 x.509 憑證的相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  以 x.509\-為基礎的驗證（不使用 TLS）                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用 Windows Server 2016 中的宣告提供者信任上的 [以宣告方式傳送群組成員資格] 規則範本來建立 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在主控台樹的  **AD FS**底下，按一下 **宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**以宣告方式傳送群組成員資格**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  按一下 **[流覽]** ，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]** 。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 在 [**傳出宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的統一資源識別項 \(URI\) 值，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 傳輸層安全性 \(TLS\) 使用 x.509 憑證的相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  以 x.509\-為基礎的驗證（不使用 TLS）                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用 Windows Server 2016 中的信賴憑證者信任上的 [轉換傳入宣告規則] 範本來建立此規則 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**驗證方法**]。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 選取 [**以不同的傳出宣告值取代傳入宣告值**]，然後執行下列動作：  

    1.  在 [**傳入宣告值**] 中，輸入下列其中一個 URI 值（以原先使用的實際驗證方法 uri 為基礎），按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

    2.  在 [**傳出宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   以 x.509\-為基礎的驗證（不使用 TLS）    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]  
> 除了資料表中的值之外，還可以使用其他的 URI 值。 上表中顯示的 URI 值會反映信賴憑證者預設接受的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用 Windows Server 2016 中的宣告提供者信任上的「轉換傳入宣告」規則範本來建立此規則 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在主控台樹的  **AD FS**底下，按一下 **宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**驗證方法**]。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 選取 [**以不同的傳出宣告值取代傳入宣告值**]，然後執行下列動作：  

    1.  在 [**傳入宣告值**] 中，輸入下列其中一個 URI 值（以原先使用的實際驗證方法 uri 為基礎），按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

    2.  在 [**傳出宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   以 x.509\-為基礎的驗證（不使用 TLS）    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>若要使用 Windows Server 2012 R2 中的「以宣告方式傳送群組成員資格」規則範本來建立此規則  

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD FS\\信任關係**] 底下，按一下 [**宣告提供者信任**] 或 [信賴憑證者**信任**]，然後在清單中按一下您想要建立此規則的特定信任。  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  

4.  在 [**編輯宣告規則**] 對話方塊中，根據您要編輯的信任以及要在其中建立此規則的規則集，選取下列其中一個索引標籤，然後按一下 [**新增規則**]，啟動與該規則集相關聯的規則嚮導：  

    -   **接受轉換規則**  

    -   **發行轉換規則**  

    -   **發佈授權規則**  

    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**以宣告方式傳送群組成員資格**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  按一下 **[流覽]** ，選取成員應接收此驗證方法宣告的群組，然後按一下 **[確定]** 。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 在 [**傳出宣告值**] 中，根據您慣用的驗證方法，在下表中輸入其中一個預設的統一資源識別項 \(URI\) 值，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|                            實際的驗證方法                             |                                對應的 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        使用者名稱和密碼驗證                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 驗證                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 傳輸層安全性 \(TLS\) 使用 x.509 憑證的相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  以 x.509\-為基礎的驗證（不使用 TLS）                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]  
> 除了資料表中的值之外，還可以使用其他的 URI 值。 上表所示的 URI 值會反映信賴憑證者預設接受的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要使用 Windows Server 2012 R2 中的「轉換傳入宣告」規則範本來建立此規則  



1.  在伺服器管理員中，按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD FS\\信任關係**] 底下，按一下 [**宣告提供者信任**] 或 [信賴憑證者**信任**]，然後在清單中按一下您想要建立此規則的特定信任。  

3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 

4.  在 [**編輯宣告規則**] 對話方塊中，選取下列其中一個索引標籤，這取決於您正在編輯的信任，以及您要建立此規則的規則集，然後按一下 [**新增規則**] 來啟動與該規則集相關聯的規則嚮導：  

    -   **接受轉換規則**  

    -   **發行轉換規則**  

    -   **發佈授權規則**  

    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。  

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**驗證方法**]。  

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**驗證方法**]。  

9. 選取 [**以不同的傳出宣告值取代傳入宣告值**]，然後執行下列動作：  

    1.  在 [**傳入宣告值**] 中，輸入下列其中一個 URI 值（以原先使用的實際驗證方法 uri 為基礎），按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

    2.  在 [**傳出宣告值**] 中，輸入下表中的其中一個預設 URI 值，這取決於您的新慣用驗證方法選擇，按一下 **[完成]** ，然後按一下 **[確定]** 以儲存規則。  

|              實際的驗證方法              |                                對應的 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         使用者名稱和密碼驗證          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 驗證                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 憑證的 TLS 相互驗證 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   以 x.509\-為基礎的驗證（不使用 TLS）    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![建立規則](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]  
> 除了資料表中的值之外，還可以使用其他的 URI 值。 上表中顯示的 URI 值會反映信賴憑證者預設接受的 Uri。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  

[檢查清單：建立信賴憑證者信任的宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：建立宣告提供者信任的宣告規則](https://technet.microsoft.com/library/ee913564.aspx)  

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
