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
ms.openlocfilehash: b197268de3c5335e30c2a24a74c5a7b01224b500
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859691"
---
# <a name="add-an-attribute-store"></a>新增屬性存放區


需要存取 Active Directory 同盟服務 \(AD FS\) 所保護之資源的使用者帳戶和電腦帳戶，會儲存在屬性存放區中，例如 Active Directory Domain Services \(AD DS\)。 宣告發行引擎會使用屬性存放區來收集發行宣告所需的資料。 然後會將屬性存放區中的資料投射為宣告。  
  
您可以使用下列程式，將屬性存放區新增至同盟服務。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-an-attribute-store"></a>新增屬性存放區  
  
1.  開啟**AD FS 管理**。  
  
2.  在 [**動作**] 下，按一下 [**新增屬性存放區**]。  

![新增屬性存放區](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. 在 [**新增屬性存放區**] 對話方塊中，為您想要新增的屬性存放區設定下列屬性：  
  
   -   在 [**顯示名稱**] 中，輸入您要用來識別屬性存放區的名稱。  
  
   -   在 [**屬性存放區類型**] 中，選取支援的屬性存放區類型， **Active Directory**、 **LDAP**或**SQL**。  
  
   -   在 [**連接字串**] 中，如果您已選取 \(LDAP\) 存放區或結構化查詢語言 (SQL) \(SQL\) 存放區的輕量型目錄存取協定，請輸入您用來建立與屬性存放區之連接的字串。 若為 Active Directory 屬性存放區，則不需要連接字串。因此，此欄位已停用。  
  
       > [!NOTE]  
       > AD FS 預設會自動建立 Active Directory 屬性存放區。  
 
![新增屬性存放區](media/Add-an-Attribute-Store/addstore2.PNG) 

4. 按一下 [確定]。  
  
## <a name="additional-references"></a>其他參考資料  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
