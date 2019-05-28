---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: 新增屬性存放區
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 103ee707c88f4e88b231a833f739cf75b6503e18
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190105"
---
# <a name="add-an-attribute-store"></a>新增屬性存放區


使用者帳戶和電腦帳戶需要受保護的 Active Directory Federation Services 資源的存取權\(AD FS\)會儲存在屬性存放區，例如 Active Directory 網域服務\(AD DS\). 宣告發行引擎會使用屬性存放區，來收集資料所需發出宣告。 從屬性存放區的資料然後投射為宣告。  
  
若要新增至 Federation Service 屬性存放區，您可以使用下列程序。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-an-attribute-store"></a>若要新增的屬性存放區  
  
1.  開啟**AD FS 管理**。  
  
2.  底下**動作**按一下 **新增屬性存放區**。  

![新增屬性存放區](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  在 **新增屬性存放區**對話方塊方塊中，設定您想要新增的屬性存放區的下列屬性：  
  
    -   在 **顯示名稱**，輸入您想要用來識別屬性存放區的名稱。  
  
    -   在 **的屬性存放區型別**，選取支援的屬性存放區類型是**Active Directory**， **LDAP**，或**SQL**。  
  
    -   在 **連接字串**，如果您已選取任一個輕量型目錄存取通訊協定\(LDAP\)存放區或結構化的查詢語言\(SQL\)存放區中，輸入字串您用來連接到 屬性存放區。 Active Directory 屬性存放區中，任何連接字串不是必要的;因此，會停用此欄位。  
  
        > [!NOTE]  
        > 根據預設，AD FS 會自動建立 Active Directory 屬性存放區。  
 
![新增屬性存放區](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  按一下 [確定]  。  
  
## <a name="additional-references"></a>其他參考資料  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[角色的屬性存放區](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
