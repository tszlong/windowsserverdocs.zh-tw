---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: 新增屬性存放區
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 352c66bbbd2aade0f212605ca1416fb6581dab0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962660"
---
# <a name="add-an-attribute-store"></a>新增屬性存放區


需要存取 Active Directory 同盟服務 AD FS 所保護之資源的使用者帳戶和電腦帳戶， \( \) 會儲存在屬性存放區中，例如 Active Directory Domain Services \( AD DS \) 。 宣告發行引擎會使用屬性存放區來收集發行宣告所需的資料。 然後會將屬性存放區中的資料投射為宣告。  
  
您可以使用下列程式，將屬性存放區新增至同盟服務。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-an-attribute-store"></a>新增屬性存放區  
  
1.  開啟**AD FS 管理**]。  
  
2.  在 [**動作**] 下，按一下 [**新增屬性存放區**]。  

![新增屬性存放區](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. 在 [**新增屬性存放區**] 對話方塊中，為您想要新增的屬性存放區設定下列屬性：  
  
   -   在 [**顯示名稱**] 中，輸入您要用來識別屬性存放區的名稱。  
  
   -   在 [**屬性存放區類型**] 中，選取支援的屬性存放區類型， **Active Directory**、 **LDAP**或**SQL**。  
  
   -   在 [**連接字串**] 中，如果您已選取 [輕量目錄存取通訊協定] \( LDAP \) 存放區或結構化查詢語言 (SQL) 的 \( SQL \) 存放區，請輸入您用來建立與屬性存放區之連接的字串。 若為 Active Directory 屬性存放區，則不需要連接字串。因此，此欄位已停用。  
  
       > [!NOTE]  
       > 根據預設，AD FS 會自動建立 Active Directory 屬性存放區。  
 
![新增屬性存放區](media/Add-an-Attribute-Store/addstore2.PNG) 

4. 按一下 [確定]。  
  
## <a name="additional-references"></a>其他參考  

[AD FS 操作](../ad-fs-operations.md)
  
[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
