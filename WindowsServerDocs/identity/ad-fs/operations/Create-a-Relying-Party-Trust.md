---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: 建立信賴憑證者信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0d32edd7ebc23fa724439710c6511642d9c49a3
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323050"
---
# <a name="create-a-relying-party-trust"></a>建立信賴憑證者信任


下列檔提供手動建立信賴憑證者信任和使用同盟中繼資料的相關資訊。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>手動建立宣告感知信賴憑證者信任 

若要使用中的 [AD FS 管理] 嵌入式管理單元\-新增新的信賴憑證者信任，並手動設定這些設定，請在同盟伺服器上執行下列程式。  

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。
  
1. 在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在 [**動作**] 底下，按一下 [**新增信賴**憑證者信任]。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在 [**歡迎使用**] 頁面上，選擇 [**宣告感知**]，然後按一下 [**啟動**]。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在 [選取資料來源] 頁面上，按一下 [手動輸入信賴憑證者相關資料]，然後按一下 [下一步]。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  在 [**指定顯示名稱**] 頁面的 [**顯示名稱**] 中輸入名稱，在 [**附注**] 下輸入此信賴憑證者信任的描述，然後按 **[下一步]** 。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. 在 [**設定憑證**] 頁面上，如果您有選擇性的權杖加密憑證，請按一下 **[流覽]** 找出憑證檔案，然後按 **[下一步]** 。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  在 [**設定 URL** ] 頁面上，執行下列其中一項或兩項動作，按 **[下一步]** ，然後移至步驟8：  
  
    -   選取 [**啟用 WS\-同盟被動通訊協定的支援**] 核取方塊。 在 [信賴憑證者**WS\-同盟被動式通訊協定 URL**] 底下，輸入此信賴憑證者信任的 url，然後按 **[下一步]** 。  
  
    -   選取 [啟用 SAML 2.0 WebSSO 通訊協定的支援] 核取方塊。 在 [信賴憑證者**SAML 2.0 SSO 服務 URL**] 下，輸入此信賴憑證者信任的安全性聲明標記語言 \(SAML\) 服務端點 URL，然後按 **[下一步]** 。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. 在 [設定識別碼] 頁面上，指定此信賴憑證者的一或多個識別碼，按一下 [新增] 以將它們新增到清單中，然後按一下 [下一步]。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  在 [**選擇存取控制原則**] 上選取原則，然後按 **[下一步]** 。  如需存取控制原則的詳細資訊，請參閱[AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)。 
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. 在 [準備新增信任] 頁面上，檢閱設定，然後按一下 [下一步] 以儲存您的信賴憑證者信任資訊。  
   ![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. 在 [完成] 頁面上，按一下 [關閉]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>使用同盟中繼資料建立宣告感知信賴憑證者信任

若要使用 [AD FS 管理] 嵌入式管理單元來新增新的信賴憑證者信任，請從合作夥伴發佈到區域網路或網際網路的同盟中繼資料自動匯入有關合作夥伴的設定資料，然後在上執行下列程式：帳戶夥伴組織中的同盟伺服器。

>[!NOTE]
>雖然使用具有不合格主機名稱的憑證（例如 https://myserver）很長，但這些憑證沒有安全性價值，而且可以讓攻擊者模擬發行同盟中繼資料的同盟服務。 因此，在查詢同盟中繼資料時，您應該只使用完整功能變數名稱，例如 https://myserver.contoso.com。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。


1. 在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2. 在 [**動作**] 底下，按一下 [**新增信賴**憑證者信任]。  
   ![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. 在 [**歡迎使用**] 頁面上，選擇 [**宣告感知**]，然後按一下 [**啟動**]。  
   ![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. 在 [**選取資料來源**] 頁面上，按一下 [匯<strong>入發佈于線上或區域網路的信賴憑證者相關資料] *。在 [同盟中繼資料位址（主機名稱或 URL）] 中</strong>，輸入夥伴的同盟中繼資料 URL 或主機名稱，然後按 **[下一步]** 。  
   ![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. 在 [指定顯示名稱] 頁面上的 [**顯示名稱**] 中輸入名稱，在 [附注] 下輸入此信賴憑證者信任的描述，然後按 **[下一步]** 。

6. 在 [選擇發佈授權規則] 頁面上，選取 [**允許所有使用者存取此信賴**憑證者] 或 [**拒絕所有使用者存取此信賴**憑證者]，然後按 **[下一步]** 。

7. 在 [準備新增信任] 頁面上，檢查設定，然後按 **[下一步]** 以儲存您的信賴憑證者信任資訊。

8. 在 [完成] 頁面上，按一下 [**關閉**]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。 如需有關如何繼續為此信賴憑證者信任新增宣告規則的詳細資訊，請參閱＜其他參考資料＞。




## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
