---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: "建立通過或篩選傳入理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>建立通過或篩選傳入理賠要求規則

>適用於：Windows Server 2016、Windows Server 2012 R2

在 Active Directory 同盟服務 \(AD FS\) 使用傳遞透過或篩選傳入取得規則範本，您可以通過所有連入宣告選取的宣告類型。 您也可以篩選宣告傳入的值與選取的宣告類型。 例如，您可以使用此規則範本建立將會傳送所有連入的群組宣告規則。 您也可以使用此規則傳送只使用者主體名稱 \(UPN\) 宣告結尾的@fabrikam。  
  
您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立通過或篩選可以方信任 Windows Server 2016 上的連入理賠要求規則 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**傳遞透過或篩選連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**設定規則**頁面上，在**理賠要求規則名稱**輸入顯示名稱，則本規則**傳入宣告類型**選取宣告類型清單中，並選取其中一項下列選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立通過或篩選在 Windows Server 2016 宣告提供者信任傳入理賠要求規則 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，選取**傳遞透過或篩選連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**設定規則**頁面上，在**理賠要求規則名稱**輸入顯示名稱，則本規則**傳入宣告類型**選取宣告類型清單中，並選取其中一項下列選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>若要建立通過或篩選在 Windows Server 2012 R2 傳入理賠要求規則

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FSAD FS\\Trust 關係**，按一下**宣告提供者信任**或**可以廠商信任**，，然後按一下 [特定信任在清單中您想要用來建立本規則。  
  
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

6.  在**設定規則**頁面上，在**理賠要求規則名稱**輸入顯示名稱，則本規則**傳入宣告類型**選取宣告類型清單中，並選取其中一項下列選項，根據您的組織的需求：  
  
    -   **通過所有宣告值**  
  
    -   **只有在特定取得值通過**  
  
    -   **通過符合尾碼值特定的電子郵件宣告並值**  
  
    -   **通過的 [開始] 的特定值宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  



  
## <a name="additional-references"></a>其他參考資料  
[設定理賠要求規則](Configure-Claim-Rules.md)  
  
[使用通過或篩選理賠要求規則](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
