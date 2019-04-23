---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: 建立規則轉換傳入宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881039"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>建立規則，以傳送 AD FS 1.x 相容宣告

>適用於：Windows Server 2016, Windows Server 2012 R2


在您使用 Active Directory Federation Services 的情況下\(AD FS\)將會執行 AD FS 1.0 同盟伺服器收到的問題宣告\(Windows Server 2003 R2\)或 AD FS 1.1 \(Windows Server 2008 或 Windows Server 2008 R2\)，您必須執行下列動作：  
  
-   建立規則，它會傳送名稱識別碼宣告類型與格式的 UPN、 電子郵件或一般名稱。  
  
-   傳送的所有其他宣告必須具有下列宣告類型的其中一個：  
  
    -   AD FS 1。*x*電子郵件地址  
  
    -   AD FS 1。*x* UPN  
  
    -   一般名稱  
  
    -   群組  
  
    -   開頭的任何其他宣告類型 https://schemas.xmlsoap.org/claims/，例如 https://schemas.xmlsoap.org/claims/EmployeeID  
  
根據您組織的需求，使用下列程序的其中一個來建立 AD FS 1。*x*相容的 NameID 宣告：  
  
-   建立此規則問題的 AD FS 1.x 名稱識別碼宣告使用**傳遞或篩選傳入宣告 」 規則範本**  
  
-   建立此問題，AD FS 1.x 名稱識別碼宣告使用的規則**轉換傳入宣告 」 規則範本**。 在您要將現有的宣告類型變更為新的宣告型別適用於 AD FS 1 的情況下，您可以使用此規則範本。 *x*宣告。  
  
> [!NOTE]  
> 此規則中如預期般運作，請確定您要在其中建立此規則的宣告提供者信任的信賴憑證者信任，已設定為使用**AD FS 1.0 和 1.1 設定檔**。 




## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則以發出的 AD FS 1。*x*名稱識別碼宣告使用 「 傳遞或篩選傳入宣告 」 規則範本在信賴憑證者信任中 Windows Server 2016 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**名稱識別碼**清單中。  
  
8.  在 **連入的名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
9. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 按一下 **完成**，然後按一下**確定**儲存規則。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則以發出的 AD FS 1。*x*名稱識別碼宣告使用 「 傳遞或篩選傳入宣告 」 規則範本上宣告在 Windows Server 2016 中的提供者信任 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**名稱識別碼**清單中。  
  
8.  在 **連入的名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
9. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 按一下 **完成**，然後按一下**確定**儲存規則。  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則來轉換傳入宣告在信賴憑證者信任中 Windows Server 2016 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取您想要轉換的清單中的連入宣告的型別。  
  
8.  在 **連出的宣告型別**，選取**名稱識別碼**清單中。  
  
9. 在 **傳出名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
10. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 按一下 **完成**，然後按一下**確定**儲存規則。  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立來轉換傳入宣告在 Windows Server 2016 中的提供者信任宣告規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取您想要轉換的清單中的連入宣告的型別。  
  
8.  在 **連出的宣告型別**，選取**名稱識別碼**清單中。  
  
9. 在 **傳出名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
10. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 按一下 **完成**，然後按一下**確定**儲存規則。  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>若要建立規則以發出的 AD FS 1。*x*名稱識別碼宣告使用 「 傳遞或篩選傳入宣告 」 規則範本在 Windows Server 2012 R2
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中下, **AD FS\\信任關係**，按一下**宣告提供者信任**或是**信賴憑證者信任**，然後按一下 特定您要建立此規則的清單中的信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取其中一個下列索引標籤，根據信任您要編輯，以及哪一個規則可設定您要建立，此規則，然後按一下**加入規則**啟動規則精靈這就是該規則集相關聯：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取**名稱識別碼**清單中。  
  
8.  在 **連入的名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
9. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. 按一下 **完成**，然後按一下**確定**儲存規則。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要建立規則以發出的 AD FS 1。*x* Windows Server 2012 R2 中使用轉換傳入宣告規則範本的名稱識別碼宣告  
  
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
  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  在 **設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在 **傳入宣告類型**，選取您想要轉換的清單中的連入宣告的型別。  
  
8.  在 **連出的宣告型別**，選取**名稱識別碼**清單中。  
  
9. 在 **傳出名稱識別碼格式**，選取其中一個下列的 AD FS 1。*x*\-相容宣告從清單的格式：  
  
    -   **UPN**  
  
    -   **E\-郵件**  
  
    -   **一般名稱**  
  
10. 選取下列選項之一，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. 按一下 **完成**，然後按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：建立信賴憑證者信任宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：建立宣告規則的宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
