---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: "建立非宣告注意信賴派對信任"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>建立宣告感知可以派對信任

>適用於：Windows Server 2016、Windows Server 2012 R2

AD FS 管理 snap\ 中、non\ claims\ 感知可以廠商信任是建立來表示同盟服務之間單一 web\ 型應用程式，並不知道 claims\ 和 Web 應用程式 Proxy 透過存取信任的物件。  
  
Non\ claims\ 感知信賴廠商信任是信賴的派對信任其透過 Web 應用程式 Proxy 存取信賴的派對信任時所組成識別碼、名稱及驗證和授權規則。 這些 web\ 型應用程式不會依賴宣告，亦即，這些整合式 Windows Authentication\ 為基礎的應用程式，可以有執行時存取公司網路透過 Web 應用程式 Proxy 外部根據宣告的存取權的授權規則。  
  
若要新增新 non\ claims\ 感知信賴廠商信任，請使用 AD FS 管理 snap\ 中，執行下列程序。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>若要手動建立非宣告注意可以方信任 
1. 在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在**動作**，按一下 [**新增可以方信任**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**歡迎**頁面上，選擇 [**感知非宣告**，按一下 [ **[開始]**。  
![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  在**指定顯示名稱**頁面上，輸入名稱**顯示名稱**，在**筆記**輸入此信賴廠商信任、描述，然後按一下**下一步**。  
![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. 在**設定識別碼**頁面上，指定一個或多個識別碼這個信賴的按一下 [**新增**來將他們新增到清單，然後按一下 [**下一步**。  
![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  在**選擇存取控制原則**選取原則，然後按**下一步**。  如需存取控制原則的詳細資訊，請查看[存取控制原則 AD FS 在](Access-Control-Policies-in-AD-FS.md)。 
![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. 在**準備好新增信任**頁面上，檢視設定，然後按一下**下一步**以儲存您信賴信任的資訊。  
   ![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. 在**完成**頁面上，按**關閉**。 這個動作會自動顯示**編輯理賠要求規則**對話方塊。  
![信賴](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>也了  
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md) 
