---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: 建立規則允許所有使用者
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822589"
---
# <a name="create-a-rule-to-permit-all-users"></a>建立規則允許所有使用者

>適用於：Windows Server 2016, Windows Server 2012 R2

在 Windows Server 2016 中，您可以使用**存取控制原則**建立規則，可讓所有使用者存取信賴憑證者的合作對象。  Windows Server 2012 r2 中使用**允許所有使用者**規則範本，在 Active Directory Federation Services \(AD FS\)，您可以建立會讓所有使用者存取信賴憑證者的授權規則合作對象。 

您可以使用額外的授權規則進一步限制存取。 從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。  
  
您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>若要建立規則以允許 Windows Server 2016 中的所有使用者

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  以滑鼠右鍵按一下**信賴憑證者信任**您想要允許存取，然後選取**編輯存取控制原則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. 在 存取控制原則選取**允許所有人**，然後按一下**套用**並**Ok**。
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>若要建立規則以允許 Windows Server 2012 R2 中的所有使用者 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS\\信任關係\\信賴憑證者信任**，按一下您要建立此規則的清單中的特定信任。  

3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  中**編輯宣告規則** 對話方塊中，按一下**發佈授權規則** 索引標籤或**委派授權規則** 索引標籤\(依據的型別您需要的授權規則\)，然後按一下**加入規則**以啟動**新增的授權宣告規則精靈**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**允許所有使用者**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  在 **設定規則**頁面上，按一下**完成**。  
  
7.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：建立信賴憑證者信任宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
