---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: "建立允許所有使用者規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>建立允許所有使用者規則

>適用於：Windows Server 2016、Windows Server 2012 R2

在 Windows Server 2016，您可以使用**存取控制原則**來建立可讓所有的使用者存取信賴規則。  在 Windows Server 2012 R2，使用**允許所有使用者**在 Active Directory 同盟服務 \(AD FS\) 規則範本，您可以建立的可讓所有的使用者存取信賴授權規則。 

您可以使用其他授權規則進一步的限制存取。 使用者可以存取信賴從同盟服務可能仍然無法服務信賴。  
  
您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>建立允許所有使用者在 Windows Server 2016 規則

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  以滑鼠右鍵按一下**可以方信任**您想要允許存取，然後選取 [**編輯存取控制項原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. 存取控制原則選取**讓每個人都**，然後按一下 [**套用]**和**[確定]**。
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>建立允許所有使用者在 Windows Server 2012 R2 規則 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS\\Trust Relationships\\Relying 廠商信任**，按一下您想要用來建立此規則清單中的特定信任。  

3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  在**編輯理賠要求規則**對話方塊中，按一下 [**發行授權規則**索引標籤或**委派授權規則**] 索引標籤 \（根據授權規則類型您 require\），，然後按一下**新增規則**到 [開始]**新增授權理賠要求規則精靈**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**允許所有使用者**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  在**設定規則**頁面上，按**完成]**。  
  
7.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
