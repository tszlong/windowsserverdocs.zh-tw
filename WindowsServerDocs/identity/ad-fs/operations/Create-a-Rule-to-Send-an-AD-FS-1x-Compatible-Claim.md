---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: "建立轉換傳入理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>建立傳送給 AD FS 1.x 相容理賠要求規則

>適用於：Windows Server 2016、Windows Server 2012 R2


在您使用 Active Directory 同盟服務情形 \(AD FS\) 發行宣告，將會收到同盟伺服器執行 AD FS 1.0 \ (Windows Server 2003 R2\) 或 AD FS 1.1 \（Windows Server 2008 或 Windows Server 2008 R2\），您必須執行下列動作：  
  
-   建立規則，將會傳送名稱 ID 宣告類型格式 UPN、電子郵件或一般的名稱。  
  
-   所有其他宣告傳送必須理賠要求下列類型：  
  
    -   AD FS 1。*x*電子郵件地址  
  
    -   AD FS 1。*x* UPN  
  
    -   一般的名稱  
  
    -   群組  
  
    -   任何其他開頭 https://schemas.xmlsoap.org/claims/，例如 https://schemas.xmlsoap.org/claims/EmployeeID 宣告類型  
  
根據您的組織的需求，使用下列程序來建立 AD FS 1。*x*相容 NameID 宣告：  
  
-   建立問題 AD FS 1.x 名稱 ID 宣告使用此規則**傳遞透過或篩選傳入取得規則範本**  
  
-   建立問題 AD FS 1.x 名稱 ID 宣告使用此規則**轉換輸入宣告規則範本**。 您可以使用此規則範本中您想来變更將會使用 AD FS 1 新宣告類型現有宣告類型。 *x*主張。  
  
> [!NOTE]  
> 此規則如預期般運作，請確定該信賴廠商信任或建立此規則宣告提供者信任已設定為使用**AD FS 1.0 和 1.1 設定檔**。 




## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立發出 AD FS 1 規則。*x*名稱 ID 取得使用傳遞透過或篩選傳入取得規則範本，可以方信任 Windows Server 2016 上 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**傳遞透過或篩選連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**名稱 ID**清單中。  
  
8.  在**連入名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
9. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立發出 AD FS 1 規則。*x*名稱 ID 取得使用傳遞透過或篩選傳入取得規則範本，Windows Server 2016 宣告提供者信任上 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**傳遞透過或篩選連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**名稱 ID**清單中。  
  
8.  在**連入名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
9. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立轉換可以方信任 Windows Server 2016 上的連入理賠要求規則 

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
  
7.  在**傳入宣告類型**，選取您想要轉換的清單中，連入宣告的類型。  
  
8.  在**傳出宣告類型**、**名稱 ID**清單中。  
  
9. 在**撥出名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
10. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立轉換在 Windows Server 2016 宣告提供者信任傳入理賠要求規則 
  
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
  
7.  在**傳入宣告類型**，選取您想要轉換的清單中，連入宣告的類型。  
  
8.  在**傳出宣告類型**、**名稱 ID**清單中。  
  
9. 在**撥出名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
10. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  













  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>若要建立發出 AD FS 1 規則。*x*名稱 ID 取得使用傳遞透過或篩選傳入取得規則範本，Windows Server 2012 R2 上
  
1.  在伺服器管理員中，按一下**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\\Trust 關係**，按一下**宣告提供者信任**或**可以廠商信任**，，然後按一下 [特定信任在清單中您想要用來建立本規則。  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在**編輯理賠要求規則**對話方塊中，選取其中一個下列索引標籤，根據您正在編輯，設定您的規則信任想要建立單元，此規則，然後按一下**新增規則**以開始規則該組相關聯的規則精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發行授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**傳遞透過或篩選連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)  
  
6.  在**設定規則**頁面上，輸入宣告規則的名稱。  
  
7.  在**傳入宣告類型**、**名稱 ID**清單中。  
  
8.  在**連入名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
9. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要建立發出 AD FS 1 規則。*x* Windows Server 2012 R2 使用轉換輸入宣告規則範本名稱 ID 宣告  
  
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
  
7.  在**傳入宣告類型**，選取您想要轉換的清單中，連入宣告的類型。  
  
8.  在**傳出宣告類型**、**名稱 ID**清單中。  
  
9. 在**撥出名稱 ID 格式**，選取其中一個 AD FS 1 下列。*x*\-compatible 取得格式清單：  
  
    -   **UPN**  
  
    -   **E\ 郵件**  
  
    -   **一般的名稱**  
  
10. 選取下列其中一個選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. 按一下**完成**，然後按一下 [ **[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
