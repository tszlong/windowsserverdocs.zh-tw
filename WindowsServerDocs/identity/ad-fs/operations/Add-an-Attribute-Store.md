---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: 新增屬性存放區
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0f5c9d3b0f856ab72a16930ddb5c50686d747ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358393"
---
# <a name="add-an-attribute-store"></a>新增屬性存放區


需要存取受 Active Directory 同盟服務保護之資源的使用者帳戶和電腦帳戶 \(AD FS @ no__t-1 會儲存在屬性存放區中，例如 Active Directory Domain Services \(AD DS @ no__t-3。 宣告發行引擎會使用屬性存放區來收集發行宣告所需的資料。 然後會將屬性存放區中的資料投射為宣告。  
  
您可以使用下列程式，將屬性存放區新增至同盟服務。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-an-attribute-store"></a>新增屬性存放區  
  
1.  開啟**AD FS 管理**。  
  
2.  在 [**動作**] 下，按一下 [**新增屬性存放區**]。  

![新增屬性存放區](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. 在 [**新增屬性存放區**] 對話方塊中，為您想要新增的屬性存放區設定下列屬性：  
  
   -   在 [**顯示名稱**] 中，輸入您要用來識別屬性存放區的名稱。  
  
   -   在 [**屬性存放區類型**] 中，選取支援的屬性存放區類型， **Active Directory**、 **LDAP**或**SQL**。  
  
   -   在 [**連接字串**] 中，如果您選取了輕量目錄存取通訊協定 \(LDAP @ no__t-2 存放區或結構化查詢語言 (SQL) \(SQL @ no__t-4 存放區，請輸入您用來建立屬性連接的字串。存放區. 若為 Active Directory 屬性存放區，則不需要連接字串。因此，此欄位已停用。  
  
       > [!NOTE]  
       > AD FS 預設會自動建立 Active Directory 屬性存放區。  
 
![新增屬性存放區](media/Add-an-Attribute-Store/addstore2.PNG) 

4. 按一下 [確定]。  
  
## <a name="additional-references"></a>其他參考資料  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
