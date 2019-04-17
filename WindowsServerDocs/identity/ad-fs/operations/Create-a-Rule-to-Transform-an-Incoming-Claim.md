---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: "建立轉換傳入理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>建立轉換傳入理賠要求規則

>適用於：Windows Server 2016、Windows Server 2012 R2

使用**轉換取得連入**規則範本 Active Directory 同盟服務 \(AD FS\)，在選取傳入理賠要求、變更其宣告類型，並變更其理賠要求值。 例如，您可以使用此規則範本建立會傳送具有相同的理賠要求值，連入的群組宣告的角色理賠要求規則。 您也可以使用此規則傳送群組宣告項採購宣告值的值為系統管理員，連入的群組宣告或您可以傳送只使用者主體名稱 \(UPN\) 宣告使用該結束時@fabrikam。  
  
您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上已完成此程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。 

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

6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**傳入宣告類型**，請選取清單鍵入理賠要求。 在**傳出宣告類型**，選取 [宣告類型清單中，選取其中一項下列選項，您的組織需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。
  
> [!NOTE]  
> 如果您的設定，請使用 AD FS\ 發行宣告動態存取控制案例，第一次建立轉換規則信任宣告提供者，以及**傳入宣告類型**，輸入名稱，連入宣告，或如果您之前已建立宣告描述，從清單中選取它。 第二個，在**傳出宣告類型**、選取您想要宣告 URL，然後建立轉換規則信賴廠商信任發行裝置理賠要求。  
>   
> 如需動態存取控制案例中，請查看[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或[使用 AD DS 宣告 AD FS 使用](https://technet.microsoft.com/library/hh831504.aspx)。 

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

6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**傳入宣告類型**，請選取清單鍵入理賠要求。 在**傳出宣告類型**，選取 [宣告類型清單中，選取其中一項下列選項，您的組織需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

> [!NOTE]  
> 如果您的設定，請使用 AD FS\ 發行宣告動態存取控制案例，第一次建立轉換規則信任宣告提供者，以及**傳入宣告類型**，輸入名稱，連入宣告，或如果您之前已建立宣告描述，從清單中選取它。 第二個，在**傳出宣告類型**、選取您想要宣告 URL，然後建立轉換規則信賴廠商信任發行裝置理賠要求。  
>   
> 如需動態存取控制案例中，請查看[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或[使用 AD DS 宣告 AD FS 使用](https://technet.microsoft.com/library/hh831504.aspx)。   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>若要建立轉換在 Windows Server 2012 R2 傳入理賠要求規則 
  
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

6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**傳入宣告類型**，請選取清單鍵入理賠要求。 在**傳出宣告類型**，選取 [宣告類型清單中，選取其中一項下列選項，您的組織需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **使用新的電子郵件 e\ 尾碼取代連入 e\ 郵件尾碼宣告**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> 如果您的設定，請使用 AD FS\ 發行宣告動態存取控制案例，第一次建立轉換規則信任宣告提供者，以及**傳入宣告類型**，輸入名稱，連入宣告，或如果您之前已建立宣告描述，從清單中選取它。 第二個，在**傳出宣告類型**、選取您想要宣告 URL，然後建立轉換規則信賴廠商信任發行裝置理賠要求。  
>   
> 如需動態存取控制案例中，請查看[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或[使用 AD DS 宣告 AD FS 使用](https://technet.microsoft.com/library/hh831504.aspx)。  
  
7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
