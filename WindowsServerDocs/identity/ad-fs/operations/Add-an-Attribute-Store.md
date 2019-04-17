---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: "新增屬性網上商店"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>新增屬性網上商店

>適用於：Windows Server 2016、Windows Server 2012 R2

儲存在屬性市集中，例如 Active Directory Domain Services \(AD DS\) 帳號及電腦帳號需要資源 Active Directory 同盟服務 \(AD FS\) 受保護的存取權。 宣告發行引擎會收集資料的必要發行宣告使用屬性存放區。 做為宣告是然後投影屬性存放區中的資料。  
  
您可以使用下列程序同盟服務新增屬性存放區。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-an-attribute-store"></a>若要新增的屬性網上商店  
  
1.  開放**AD FS 管理**。  
  
2.  在**動作**按**新增屬性市集**。  

![新增屬性網上商店](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  在**新增屬性市集**對話方塊方塊中，設定下列屬性針對您想要新增的屬性市集：  
  
    -   在**顯示名稱**，輸入您想要用來找出屬性網上商店的名稱。  
  
    -   在**屬性存放區類型**，可選取支援的屬性存放區類型，**Active Directory**、**LDAP**，或**SQL**。  
  
    -   在**連接字串**，如果您已選取的輕量型 Directory 存取通訊協定 \(LDAP\) 存放區或結構化查詢語言 \(SQL\) 存放區中，輸入您用來建立連接至市集] 屬性字串。 Active Directory 屬性商店未連接字串是必要的;因此，此欄位會停用。  
  
        > [!NOTE]  
        > AD FS 預設會自動建立 Active Directory 屬性市集。  
 
![新增屬性網上商店](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  按一下**[確定]**。  
  
## <a name="additional-references"></a>其他參考資料  

[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)
  
[此屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
