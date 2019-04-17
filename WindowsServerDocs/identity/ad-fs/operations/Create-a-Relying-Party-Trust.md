---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: "建立信任關係信賴派對"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>建立信任關係信賴派對

>適用於：Windows Server 2016、Windows Server 2012 R2

下列文件會提供資訊上手動建立信賴廠商信任使用聯盟中繼資料。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>若要建立宣告注意 Relying 廠商信任手動 

若要使用 AD FS 管理 snap\ 中新增新的依賴廠商信任以手動方式進行設定，執行下列程序聯盟的伺服器上。  

資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。
  
1. 在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在**動作**，按一下 [**新增可以方信任**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**歡迎**頁面上，選擇 [**感知宣告**，按一下 [ **[開始]**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在**選取資料來源**頁面上，按一下 [**輸入信賴的相關資料，以手動方式**，然後按一下 [**下一步**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  在**指定顯示名稱**頁面上，輸入名稱**顯示名稱**，在**筆記**輸入此信賴廠商信任、描述，然後按一下**下一步**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. 在**設定憑證**頁面上，如果您有選用權杖加密憑證，按一下 [**瀏覽]**以尋找憑證檔案時，然後按**下一步**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  在**設定的 URL**頁面上，執行下列一或兩個動作，按**下**，然後繼續執行「步驟 8:  
  
    -   選取 [**讓支援 WS\-聯盟被動式通訊協定的**核取方塊。 在**Relying 廠商 WS\-聯盟被動式通訊協定 URL**，輸入此信賴廠商信任的 URL，然後按**下**。  
  
    -   選取 [**讓支援 SAML 2.0 WebSSO 通訊協定的**核取方塊。 在**Relying 廠商 SAML 2.0 SSO 服務 URL**，輸入此信賴的派對信任，安全性判斷提示標記語言 \(SAML\) 服務端點 URL，然後按**下**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. 在**設定識別碼**頁面上，指定一個或多個識別碼這個信賴的按一下 [**新增**來將他們新增到清單，然後按一下 [**下一步**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  在**選擇存取控制原則**選取原則，然後按**下一步**。  如需存取控制原則的詳細資訊，請查看[存取控制原則 AD FS 在](Access-Control-Policies-in-AD-FS.md)。 
![信賴](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. 在**準備好新增信任**頁面上，檢視設定，然後按一下**下一步**以儲存您信賴信任的資訊。  
   ![信賴](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. 在**完成**頁面上，按**關閉**。 這個動作會自動顯示**編輯理賠要求規則**對話方塊。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>若要建立宣告注意 Relying 廠商信任使用聯盟中繼資料

若要新增新信賴廠商信任，會自動從聯盟中繼資料的合作夥伴發行的區域網路或網際網路，匯入之合作夥伴組態資料使用 AD FS 管理嵌入式管理單元，執行下列程序聯盟 account 合作夥伴組織的伺服器上。

>[!NOTE]
>雖然它已長時間使用憑證的主機不完整的名稱，例如 https://myserver 常見的方式，這些憑證不有任何安全性價值，並可以讓攻擊模擬同盟服務發行聯盟中繼資料。 因此，查詢時聯盟中繼資料，您應該只使用 https://myserver.contoso.com 例如的完整的網域名稱。

資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。


1. 在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在**動作**，按一下 [**新增可以方信任**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**歡迎**頁面上，選擇 [**感知宣告**，按一下 [ **[開始]**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在**選取資料來源**頁面上，按一下 [**匯入信賴有關的資料發行 online 或本機網路*。 在**聯盟中繼資料（主機名稱或位址 URL）**，輸入合作夥伴，聯盟中繼資料 URL 或主機名稱，然後按一下**下**。  
![信賴](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  指定顯示名稱頁面上輸入名稱**顯示名稱**，在筆記輸入此信賴廠商信任、描述，然後按一下**下**。

6.  在選擇發行授權規則頁面上，選取**允許所有使用者存取此信賴**或**拒絕所有使用者的存取此信賴**，然後按一下 [**下一步**。

7.  在 [準備新增信任頁面上，檢視設定，然後按一下**下一步**以儲存您信賴信任的資訊。

8.  在 [設定] 頁面中，按一下**關閉**。 這個動作會自動顯示編輯理賠要求規則] 對話方塊。 如需有關如何繼續與新增此信賴廠商信任理賠要求規則，額外的資訊尋找參考資料。




## <a name="see-also"></a>也了  
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md) 
