---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: 建立信賴憑證者信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444367"
---
# <a name="create-a-relying-party-trust"></a>建立信賴憑證者信任


下列文件會提供有關手動建立信賴憑證者信任和使用同盟中繼資料資訊。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>若要建立宣告感知信賴憑證者信任以手動方式 

若要使用 AD FS 管理嵌入式管理單元加入新信賴憑證者信任\-中並手動進行設定，在同盟伺服器上執行下列程序。  

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。
  
1. 在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  底下**動作**，按一下**新增信賴憑證者信任**。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在 **歡迎**頁面上，選擇**宣告感知**，按一下 **啟動**。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在 [選取資料來源]  頁面上，按一下 [手動輸入信賴憑證者相關資料]  ，然後按一下 [下一步]  。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  在上**指定顯示名稱**頁面上，輸入中的名稱**顯示名稱**下方**備忘稿**輸入此信賴憑證者信任的描述，然後按一下**下一步**.  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. 在上**設定憑證**頁面上，如果有選擇性的權杖加密憑證，按一下**瀏覽**若要找出憑證檔案，然後按一下**下一步**。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  在 [**設定 URL**頁面上，執行下列一或兩個動作，按一下**下一步]** ，然後移至步驟 8:  
  
    -   選取 **啟用支援 WS\-同盟被動式通訊協定**核取方塊。 底下**信賴憑證者合作對象 WS\-同盟被動式通訊協定 URL**，輸入此信賴憑證者信任的 URL，然後按一下**下一步**。  
  
    -   選取 [啟用 SAML 2.0 WebSSO 通訊協定的支援]  核取方塊。 底下**信賴憑證者 SAML 2.0 SSO 服務 URL**，輸入安全性聲明標記語言\(SAML\)服務端點 URL 為此信賴憑證者信任，然後按一下**下一步**.  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. 在 [設定識別碼]  頁面上，指定此信賴憑證者的一或多個識別碼，按一下 [新增]  以將它們新增到清單中，然後按一下 [下一步]  。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  在 [**選擇存取控制原則**選取原則，然後按一下**下一步]** 。  如需有關存取控制原則的詳細資訊，請參閱 < [AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)。 
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. 在 [準備新增信任]  頁面上，檢閱設定，然後按一下 [下一步]  以儲存您的信賴憑證者信任資訊。  
   ![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. 在 [完成]  頁面上，按一下 [關閉]  。 此動作會自動顯示 [編輯宣告規則]  對話方塊。  
![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>若要建立宣告感知信賴憑證者信任使用同盟中繼資料

若要新增的新信賴憑證者信任，請使用 AD FS 管理嵌入式管理單元，會自動從夥伴發佈到區域網路或網際網路的同盟中繼資料匯入有關夥伴的設定資料上執行下列程序帳戶夥伴組織中的同盟伺服器。

>[!NOTE]
>雖然長久以來的常見的作法是使用憑證具有不完整的主機名稱，例如 https://myserver，這些憑證沒有安全性價值，並且可以讓攻擊者模擬 Federation Service 會發佈同盟中繼資料。 因此，當查詢同盟中繼資料，您應該只使用完整的網域名稱這類 https://myserver.contoso.com。

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。


1. 在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2. 底下**動作**，按一下**新增信賴憑證者信任**。  
   ![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. 在 **歡迎**頁面上，選擇**宣告感知**，按一下 **啟動**。  
   ![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. 在 **選取資料來源**頁面上，按一下<strong>相關的信賴憑證者的合作對象的匯入資料發佈到線上或本機網路 *。在 同盟中繼資料位址 （主機名稱或 URL）</strong>，輸入夥伴的同盟中繼資料 URL 或主機名稱，然後按一下**下一步**。  
   ![信賴憑證者的合作對象](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. 在 [指定顯示名稱] 頁面中輸入名稱**顯示名稱**下方資訊輸入此信賴憑證者信任的描述，然後按一下 [**下一步]** 。

6. 在選擇發佈授權規則] 頁面上，選取**允許所有使用者存取此信賴憑證者的合作對象**或**拒絕所有使用者存取此信賴憑證者的合作對象**，然後按一下 [**下一步**.

7. 在 準備新增信任 頁面上，檢閱設定，然後**下一步**以儲存您信賴憑證者信任資訊。

8. 在 完成 頁面上，按一下 **關閉**。 此動作會自動顯示 [編輯宣告規則] 對話方塊。 如需有關如何繼續為此信賴憑證者信任新增宣告規則的詳細資訊，請參閱＜其他參考資料＞。




## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
