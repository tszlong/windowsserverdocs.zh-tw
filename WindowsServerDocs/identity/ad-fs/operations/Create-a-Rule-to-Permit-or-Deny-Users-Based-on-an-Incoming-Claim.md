---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: 建立規則依據傳入宣告允許或拒絕使用者
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 72fe425b040f83a217a144976265c7754830c91b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189499"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>建立規則依據傳入宣告允許或拒絕使用者 


在 Windows Server 2016 中，您可以使用**存取控制原則**建立規則，以允許或拒絕使用者根據傳入宣告。  Windows Server 2012 r2 中使用**允許或拒絕使用者根據傳入宣告**規則範本，在 Active Directory Federation Services \(AD FS\)，您可以建立會授與的授權規則或拒絕使用者存取信賴憑證者的合作對象為基礎的類型和傳入宣告的值。 

比方說，您可以使用此建立規則，允許群組宣告值是網域系統管理員存取信賴憑證者的合作對象的使用者。 如果您想要允許所有使用者存取信賴憑證者的合作對象，請使用**都允許 Everyone**存取控制原則或有**都允許所有使用者**規則範本的 Windows Server 版本而定。 從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。  
  
您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要建立規則以允許使用者根據 Windows Server 2016 上的連入宣告
 
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**存取控制原則**。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下並選取**新增存取控制原則**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入您的原則，描述，然後按一下名稱**新增**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 在上**規則編輯器**，使用者底下，勾選**要求中的特定宣告**按一下加底線**特定**底部。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 上**選取宣告**畫面上，按一下**宣告**選項按鈕，選取**宣告類型**，則**運算子**，而**宣告值**然後按一下**Ok**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  在 **規則編輯器**按一下 **確定**。  在 **新增存取控制原則**畫面上，按一下**確定**。

8. 在  **AD FS 管理**下方主控台樹狀目錄中， **AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下**信賴憑證者信任**您想要允許存取，然後選取**編輯存取控制原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在上的存取控制原則選取原則，然後按一下**套用**並**確定**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要建立規則以拒絕使用者根據 Windows Server 2016 上的連入宣告
 
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**存取控制原則**。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下並選取**新增存取控制原則**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入您的原則，描述，然後按一下名稱**新增**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 上**規則編輯器**，請確定每個人都已選取，並在**除了**核取**包含在要求中的特定宣告**按一下加底線**特定**底部。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 上**選取宣告**畫面上，按一下**宣告**選項按鈕，選取**宣告類型**，則**運算子**，而**宣告值**然後按一下**Ok**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  在 **規則編輯器**按一下 **確定**。  在 **新增存取控制原則**畫面上，按一下**確定**。

8. 在  **AD FS 管理**下方主控台樹狀目錄中， **AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下**信賴憑證者信任**您想要允許存取，然後選取**編輯存取控制原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在上的存取控制原則選取原則，然後按一下**套用**並**確定**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>若要建立規則以允許或拒絕使用者根據 Windows Server 2012 R2 上的連入宣告
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。    
  
2.  在主控台樹狀目錄之下**AD FS\\信任關係\\信賴憑證者信任**，按一下您要建立此規則的清單中的特定信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  中**編輯宣告規則** 對話方塊中，按一下**發佈授權規則** 索引標籤或**委派授權規則** 索引標籤\(依據的型別您需要的授權規則\)，然後按一下**加入規則**以啟動**新增的授權宣告規則精靈** 。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**允許或拒絕使用者根據傳入宣告**從清單中，然後按一下  **下一步** 。  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**傳入宣告類型**下在清單中，選取的宣告類型**內送宣告值**輸入值，或按一下 瀏覽\(是否可用\)並選取一個值，然後選取下列選項之一，視您組織的需求而定：  
  
    -   **允許具有這個傳入宣告的使用者存取**  
  
    -   **拒絕具有這個傳入宣告的使用者存取**  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  按一下 **[完成]** 。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
