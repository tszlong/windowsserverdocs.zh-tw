---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: "建立傳送主張使用自訂規則規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f0eef89e651585d48ba87d14bc782efa49087669
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>建立傳送主張使用自訂規則規則

>適用於：Windows Server 2016、Windows Server 2012 R2

使用**傳送主張使用自訂規則**範本 Active Directory 同盟 Services (AD FS) 中的，您可以建立自訂宣告規則情形一般規則範本不符合您的組織的需求。 自訂宣告規則撰寫理賠要求規則語言和必須複製到**自訂規則**之前規則集合中所使用的文字方塊。 針對建構語法進階規則的詳細資訊，請查看[的角色理賠要求規則語言的](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。  
  
您可以使用下列程序，請使用 AD FS 管理 snap\ 中建立理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上已完成此程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立通過或篩選可以方信任 Windows Server 2016 上的連入理賠要求規則 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**自訂規則**中，輸入或貼上您想要這個規則理賠要求規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立通過或篩選在 Windows Server 2016 宣告提供者信任傳入理賠要求規則 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**自訂規則**中，輸入或貼上您想要這個規則理賠要求規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要建立傳送主張使用在 Windows Server 2012 R2 的自訂理賠要求規則 
  
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
  
5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱本規則。 在**自訂規則**中，輸入或貼上您想要這個規則理賠要求規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
