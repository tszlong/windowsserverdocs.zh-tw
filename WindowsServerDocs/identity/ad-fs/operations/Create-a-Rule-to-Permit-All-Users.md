---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: 建立規則允許所有使用者
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1356218c5f9f47073f007286e8acfdf4c3608b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407628"
---
# <a name="create-a-rule-to-permit-all-users"></a>建立規則允許所有使用者

在 Windows Server 2016 中，您可以使用**存取控制原則**來建立規則，讓所有使用者都能存取信賴憑證者。  在 Windows Server 2012 R2 中，使用 Active Directory 同盟服務 \(AD FS @ no__t-2 中的 [**允許所有使用者**] 規則範本，您可以建立授權規則，讓所有使用者都能存取信賴憑證者。 

您可以使用其他授權規則進一步限制存取權。 從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。  
  
您可以使用下列程式，利用 AD FS Management snap @ no__t-0in 來建立宣告規則。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>建立規則以允許 Windows Server 2016 中的所有使用者

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。 
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  以滑鼠右鍵按一下您想要允許存取的信賴憑證者**信任**，然後選取 [**編輯存取控制原則**]。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. 在存取控制原則上，選取 [**允許每個人**] **，然後按一下** **[套用] 和 [確定]** 。
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>建立規則以允許 Windows Server 2012 R2 中的所有使用者 
  
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **\\AD FS 信任關係\\** ] [信賴憑證者信任] 底下，按一下您要在其中建立此規則的清單中的特定信任。  

3.  以\-滑鼠右鍵按一下選取的信任，然後按一下 [**編輯宣告規則**]。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[發佈授權規則**] 索引標籤或 [ \(**委派授權規則**] 索引標籤（根據\)您所需的授權規則類型），然後按一下 [**新增規則]** 以啟動 [**新增授權宣告規則]** 。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**允許所有使用者**從清單中]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  在 [**設定規則**] 頁面上，按一下 **[完成]** 。  
  
7.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
