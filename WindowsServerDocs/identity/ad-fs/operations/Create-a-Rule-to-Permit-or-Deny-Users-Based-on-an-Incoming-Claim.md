---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: "建立允許或拒絕使用者根據傳入理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>建立允許或拒絕使用者根據傳入理賠要求規則 

>適用於：Windows Server 2016、Windows Server 2012 R2

在 Windows Server 2016，您可以使用**存取控制原則**來建立，允許的規則拒絕根據傳入理賠要求的使用者。  在 Windows Server 2012 R2，使用**允許] 或 [拒絕使用者根據取得連入**在 Active Directory 同盟服務 \(AD FS\) 規則範本，您可以建立會授與或拒絕信賴根據類型及值，連入理賠要求的使用者的存取權的授權規則。 

例如，您可以使用此建立，允許的值為網域存取信賴的系統管理員取得群組使用者規則。 如果您想要允許所有使用者存取信賴，請使用**都允許所有人**存取控制原則或**都允許所有使用者**規則範本根據您的 Windows Server 版本。 使用者可以存取信賴從同盟服務可能仍然無法服務信賴。  
  
您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>建立允許使用者在 Windows Server 2016 上連入理賠要求規則
 
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**存取控制原則**。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下，然後選取 [**存取控制原則**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中輸入名稱的原則、描述，然後按一下**新增**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 在**規則編輯器**的使用者，在勾選 [在**特定宣告在要求中的**，按一下 [底線**特定**底部。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 在**選取 [宣告**畫面中，按一下 [**宣告**選項按鈕，選取**取得輸入**、**電信業者**，和**宣告值**然後按一下 [ **[確定]**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  在**規則編輯器**按**[確定]**。  在**存取控制原則**畫面中，按**[確定]**。

8. 在**AD FS 管理**底下主機樹，**AD FS**，按一下 [**可以廠商信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下**可以方信任**您想要允許存取，然後選取 [**編輯存取控制項原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在存取控制原則選取您的原則，然後按一下**套用]**和**[確定]**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要建立拒絕使用者基礎 Windows Server 2016 上連入理賠要求規則
 
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**存取控制原則**。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下，然後選取 [**存取控制原則**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中輸入名稱的原則、描述，然後按一下**新增**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 在**規則編輯器**，請確定已選取 [所有人都並在**以外**勾選 [中**的特定宣告在要求中**底線按一下**特定**底部。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 在**選取 [宣告**畫面中，按一下 [**宣告**選項按鈕，選取**取得輸入**、**電信業者**，和**宣告值**然後按一下 [ **[確定]**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  在**規則編輯器**按**[確定]**。  在**存取控制原則**畫面中，按**[確定]**。

8. 在**AD FS 管理**底下主機樹，**AD FS**，按一下 [**可以廠商信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下**可以方信任**您想要允許存取，然後選取 [**編輯存取控制項原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在存取控制原則選取您的原則，然後按一下**套用]**和**[確定]**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>建立允許或拒絕使用者基礎 Windows Server 2012 R2 上連入理賠要求規則
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。    
  
2.  在主控台在**AD FS\\Trust Relationships\\Relying 廠商信任**，按一下您想要用來建立此規則清單中的特定信任。  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  在**編輯理賠要求規則**對話方塊中，按一下 [**發行授權規則**索引標籤或**委派授權規則**] 索引標籤 \（根據授權規則類型您 require\），，然後按一下**新增規則**到 [開始]**新增授權理賠要求規則精靈**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**允許] 或 [拒絕使用者根據連入宣告**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱，則本規則**傳入宣告類型**底下選取 [宣告類型清單中，**傳入取得值**輸入或按一下 [瀏覽 \（如果有 available\）並選取一個值，然後選取其中一項下列選項，根據您的組織的需求：  
  
    -   **允許此傳入理賠要求的使用者存取**  
  
    -   **拒絕這個傳入理賠要求的使用者存取**  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  按一下**完成**。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
