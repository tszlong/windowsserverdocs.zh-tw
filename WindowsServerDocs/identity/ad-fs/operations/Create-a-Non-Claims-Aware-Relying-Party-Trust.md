---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: 建立一個非宣告感知信賴憑證者信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0ea877170a07db6abe9ac82e72d1722600ec933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358106"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>建立非宣告感知信賴憑證者信任


在 AD FS Management snap @ no__t-0in 中，非 @ no__t-1claims @ no__t-2aware 信賴憑證者信任是建立來代表 federation service 與單一 web @ no__t-3based 應用程式（非宣告 @ no__t-4aware）之間信任的物件。會透過 Web 應用程式 Proxy 來存取。  
  
非 @ no__t-0claims @ no__t-1aware 信賴憑證者信任是信賴憑證者信任，其中包含在透過 Web 應用程式 Proxy 存取信賴憑證者信任時，用於驗證和授權的識別碼、名稱和規則。 這些 web @ no__t-0based 不依賴宣告的應用程式（換句話說，這些整合式 Windows 驗證 @ no__t 1based 應用程式）可能會有授權規則，而這些許可權會強制執行以宣告為基礎的存取。透過 Web 應用程式 Proxy 的公司網路。  
  
若要新增非 @ no__t-0claims @ no__t-1aware 信賴憑證者信任，請使用 AD FS Management snap @ no__t-2in，執行下列程式。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>手動建立非宣告感知信賴憑證者信任 
1. 在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在 [**動作**] 底下，按一下 [**新增信賴**憑證者信任]。  
@no__t 0relying 合作物件 @ no__t-1   

3.  在 [**歡迎使用**] 頁面上，選擇 [**非宣告感知**]，然後按一下 [**啟動**]。  
@no__t 0relying 合作物件 @ no__t-1 
  
4.  在 [**指定顯示名稱**] 頁面的 [**顯示名稱**] 中輸入名稱，在 [**附注**] 下輸入此信賴憑證者信任的描述，然後按 **[下一步]** 。  
@no__t 0relying 合作物件 @ no__t-1

5. 在 [設定識別碼] 頁面上，指定此信賴憑證者的一或多個識別碼，按一下 [新增] 以將它們新增到清單中，然後按一下 [下一步]。  
@no__t 0relying 合作物件 @ no__t-1

6.  在 [**選擇存取控制原則**] 上選取原則，然後按 **[下一步]** 。  如需存取控制原則的詳細資訊，請參閱[AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)。 
@no__t 0relying 合作物件 @ no__t-1

7. 在 [準備新增信任] 頁面上，檢閱設定，然後按一下 [下一步] 以儲存您的信賴憑證者信任資訊。  
   @no__t 0relying 合作物件 @ no__t-1 

8. 在 [完成] 頁面上，按一下 [關閉]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。  
@no__t 0relying 合作物件 @ no__t-1  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
