---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: 建立一個非宣告感知信賴憑證者信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839339"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>建立非宣告感知信賴憑證者信任

>適用於：Windows Server 2016, Windows Server 2012 R2

在 AD FS 管理嵌入式管理單元\-中，非\-宣告\-感知信賴憑證者信任是建立來代表 federation service 與單一 web 之間的信任物件\-不是應用程式宣告\-注意並透過 Web Application Proxy 的存取。  
  
非\-宣告\-感知信賴憑證者信任是信賴憑證者信任的信賴憑證者信任透過 Web Application Proxy 存取時，組成識別項、 名稱和用於驗證和授權規則。 這些 web\-不依賴宣告，亦即，這些整合式 Windows 驗證的應用程式\-架構的應用程式，可以強制執行存取權時，以宣告為基礎的存取權的授權規則透過 Web Application Proxy 的公司網路外部。  
  
若要新增非\-宣告\-感知信賴憑證者信任，使用 AD FS 管理嵌入式管理單元\-中，執行下列程序。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>若要手動建立非宣告感知信賴憑證者信任 
1. 在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  底下**動作**，按一下**新增信賴憑證者信任**。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在 **歡迎**頁面上，選擇**非宣告感知**，按一下 **啟動**。  
![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  在上**指定顯示名稱**頁面上，輸入中的名稱**顯示名稱**下方**備忘稿**輸入此信賴憑證者信任的描述，然後按一下**下一步**.  
![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. 在 [設定識別碼] 頁面上，指定此信賴憑證者的一或多個識別碼，按一下 [新增] 以將它們新增到清單中，然後按一下 [下一步]。  
![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  在 [**選擇存取控制原則**選取原則，然後按一下**下一步]**。  如需有關存取控制原則的詳細資訊，請參閱 < [AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)。 
![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. 在 [準備新增信任] 頁面上，檢閱設定，然後按一下 [下一步] 以儲存您的信賴憑證者信任資訊。  
   ![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. 在 [完成] 頁面上，按一下 [關閉]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。  
![信賴憑證者的合作對象](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
