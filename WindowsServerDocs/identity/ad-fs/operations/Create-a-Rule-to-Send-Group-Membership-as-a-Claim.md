---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: "建立傳送群組成員資格為理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d4fb03de9de2dcca36b18ce089db11ed599de820
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>建立傳送群組成員資格為理賠要求規則

>適用於：Windows Server 2016、Windows Server 2012 R2

在 Active Directory 同盟服務 \(AD FS\) 理賠要求規則範本為使用傳送群組成員資格，您可以建立可讓您選擇做為理賠要求傳送給 Active Directory 安全性群組規則。 從依據您所選取的群組，此規則會發出單一理賠要求。 例如，您可以使用此規則範本建立網域管理安全性群組成員使用者是否會傳送值為系統管理員使用群組理賠要求規則。 此規則適用於只本機 Active Directory 網域中的使用者。  
  
您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>建立群組成員資格傳送為可以方信任 Windows Server 2016 上理賠要求規則 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**為理賠要求傳送給群組成員資格**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱，則本規則**使用者群組**按一下**瀏覽**底下選取群組，**傳出宣告類型**選取您想要理賠要求類型，，接著在 [**傳出宣告輸入**輸入值。
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。
  
## <a name="to-create-a-rule-to-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立到能在 Windows Server 2016 宣告提供者信任理賠要求將群組成員資格規則 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**為理賠要求傳送給群組成員資格**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱，則本規則**使用者群組**按一下**瀏覽**底下選取群組，**傳出宣告類型**選取您想要理賠要求類型，，接著在 [**傳出宣告輸入**輸入值。 
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>建立能在 Windows Server 2012 R2 理賠要求將群組成員資格規則 
  
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

6.  在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱，則本規則**使用者群組**按一下**瀏覽**底下選取群組，**傳出宣告類型**選取您想要理賠要求類型，，接著在 [**傳出宣告輸入**輸入值。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  



## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
