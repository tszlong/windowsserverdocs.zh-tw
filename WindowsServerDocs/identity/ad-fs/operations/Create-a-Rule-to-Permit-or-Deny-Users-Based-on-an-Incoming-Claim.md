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
ms.openlocfilehash: d057c943b9c14b74b44472d446625b60f5ad9d22
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865957"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>建立規則依據傳入宣告允許或拒絕使用者 


在 Windows Server 2016 中，您可以使用**存取控制原則**來建立規則，以根據傳入宣告來允許或拒絕使用者。  在 Windows Server 2012 R2 中，使用 Active Directory 同盟服務\(AD FS\)中的 [**根據傳入宣告允許或拒絕使用者**] 規則範本，您可以建立將授與或拒絕使用者存取權的授權規則信賴憑證者是根據傳入宣告的類型和值。 

例如，您可以使用它來建立規則，只允許具有網域系統管理員值之群組宣告的使用者存取信賴憑證者。 如果您想要允許所有使用者存取信賴憑證者，請使用 [**允許每個人**存取控制原則] 或 [**允許所有使用者**] 規則範本（視您的 Windows Server 版本而定）。 從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。  
  
您可以使用下列程式，利用 AD FS 管理嵌入式管理單元\-來建立宣告規則。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>建立規則以允許以 Windows Server 2016 上的傳入宣告為基礎的使用者
 
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FS**] 底下，按一下 [**存取控制原則**]。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 按一下滑鼠右鍵，然後選取 [**新增存取控制原則**]。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入原則的名稱、描述，然後按一下 [**新增**]。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 在 [**規則編輯器**] 的 [使用者] 底下，以**要求中的特定宣告**進行簽入，然後按一下底部的底線**特定**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 在 [**選取宣告**] 畫面上，按一下 [宣告] 選項按鈕，然後依序選取 [**宣告** **類型**]、[**運算子**] 和 [**聲明] 值**，然後按一下 **[確定]** 。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  在 [**規則編輯器**] 上按一下 **[確定]** 。  在 [**新增存取控制原則**] 畫面上，按一下 **[確定]** 。

8. 在  **AD FS 管理** 主控台樹的  **AD FS**底下，按一下 信賴憑證者**信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下您想要允許存取的信賴憑證者**信任**，然後選取 [**編輯存取控制原則**]。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在 [存取控制原則] 上選取您的原則 **，然後按一下** **[套用] 和 [確定]** 。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>建立規則以根據 Windows Server 2016 上的連入宣告拒絕使用者
 
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FS**] 底下，按一下 [**存取控制原則**]。 
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 按一下滑鼠右鍵，然後選取 [**新增存取控制原則**]。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入原則的名稱、描述，然後按一下 [**新增**]。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 在 [**規則編輯器**] 上，確定已選取 [所有人]，並在 **[在要求中使用特定宣告**簽入] 底下，按一下底部的底線**特定**。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 在 [**選取宣告**] 畫面上，按一下 [宣告] 選項按鈕，然後依序選取 [**宣告** **類型**]、[**運算子**] 和 [**聲明] 值**，然後按一下 **[確定]** 。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  在 [**規則編輯器**] 上按一下 **[確定]** 。  在 [**新增存取控制原則**] 畫面上，按一下 **[確定]** 。

8. 在  **AD FS 管理** 主控台樹的  **AD FS**底下，按一下 信賴憑證者**信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下您想要允許存取的信賴憑證者**信任**，然後選取 [**編輯存取控制原則**]。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在 [存取控制原則] 上選取您的原則 **，然後按一下** **[套用] 和 [確定]** 。
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>建立規則以根據 Windows Server 2012 R2 上的連入宣告來允許或拒絕使用者
  
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。    
  
2.  在主控台樹的 [ **\\AD FS 信任關係\\** ] [信賴憑證者信任] 底下，按一下您要在其中建立此規則的清單中的特定信任。  
  
3.  以\-滑鼠右鍵按一下選取的信任，然後按一下 [**編輯宣告規則**]。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[發佈授權規則**] 索引標籤或 [ \(**委派授權規則**] 索引標籤（根據\)您所需的授權規則類型），然後按一下 [**新增規則]** 以啟動 [**新增授權宣告規則]** 。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**根據傳入宣告允許或拒絕使用者**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，在 [**傳入宣告類型**] 中選取清單中 [**傳入宣告值**] 底下的 [ \(宣告類型]，輸入一個值，或按一下 [流覽] （如果它是可用\)並選取一個值，然後根據您組織的需求，選取下列其中一個選項：  
  
    -   **允許具有這個傳入宣告的使用者存取權**  
  
    -   **拒絕具有此傳入宣告的使用者存取權**  
![建立規則](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  按一下 [ **完成**]。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
