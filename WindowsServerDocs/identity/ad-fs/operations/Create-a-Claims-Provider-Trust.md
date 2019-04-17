---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: "建立信任關係宣告提供者"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>建立信任關係宣告提供者

>適用於：Windows Server 2016、Windows Server 2012 R2

若要使用 AD FS 管理 snap\ 中新增新的宣告提供者信任以手動方式進行設定，執行下列程序資源合作夥伴組織中的資源合作夥伴聯盟伺服器上。  
  
資格在**系統管理員**，或相當於、在本機電腦上已完成此程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>若要手動建立信任關係宣告提供者  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在**動作**，按一下 [**新增宣告提供者信任**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在**歡迎**頁面上，按**[開始]**。 
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在**選取資料來源**頁面上，按一下 [**輸入宣告提供者信任資料手動**，然後按一下 [**下一步**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  在**指定顯示名稱**頁面上，輸入**顯示名稱**，在**筆記**，此宣告信任提供者，輸入描述，然後按一下**下一步**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  在**設定的 URL**頁面上指定**WS 同盟被動式 URL**如果有的話，按一下 [**下一步**。
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. 在**設定識別碼**頁面上，在**宣告提供者信任識別碼**，輸入的適當識別碼、，然後按**下一步**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. 在**設定憑證**頁面上，按一下 [**新增]**以找出憑證檔案並將它新增到清單的憑證，然後按一下 [**下一步**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. 在**準備好新增信任**頁面上，按一下 [**下一步**來儲存您宣告提供者信任的資訊。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. 在**完成**頁面上，按**關閉**。 這個動作會自動顯示**編輯理賠要求規則**對話方塊。 如需有關如何繼續與新增取得此宣告提供者信任規則，請查看下列的詳細參考。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>若要建立宣告提供者信任使用聯盟中繼資料
若要新增新的宣告提供者信任，會自動從區域網路或網際網路，發行合作夥伴之聯盟中繼資料匯入合作夥伴的相關設定資料使用 AD FS 管理嵌入式管理單元，執行下列程序聯盟資源合作夥伴組織的伺服器上。

>[!NOTE]
>雖然它已長時間使用憑證的主機不完整的名稱，例如 https://myserver 常見的方式，這些憑證不有任何安全性價值，並可以讓攻擊模擬同盟服務發行聯盟中繼資料。 因此，查詢時聯盟中繼資料，您應該只使用 https://myserver.contoso.com 例如的完整的網域名稱。

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在**動作**，按一下 [**新增可以方信任**。  
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在**歡迎**頁面上，按**[開始]**。 
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在**選取資料來源**頁面上，按**匯入宣告提供者的相關的資料發行 online 或本機網路**。 聯盟中繼資料地址（主機名稱或 URL），輸入**聯盟中繼資料 URL**或主機的合作夥伴，名稱，然後按一下 [**下**。
![宣告信任提供者](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  指定顯示名稱在頁面上輸入**顯示名稱**，資訊在輸入的這宣告信任提供者，描述，然後按**下**。

6.  在 [準備新增信任頁面上，按一下**下一步**以儲存您宣告提供者信任的資訊。

7.  在 [設定] 頁面中，按一下**關閉**。 這將會自動顯示編輯理賠要求規則] 對話方塊。 如需有關如何繼續與新增取得此宣告提供者信任規則，查看下列的詳細參考一節。



    
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定資源合作夥伴公司](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[檢查清單︰ 建立理賠要求規則宣告提供者信任](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>也了  
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md) 
  
