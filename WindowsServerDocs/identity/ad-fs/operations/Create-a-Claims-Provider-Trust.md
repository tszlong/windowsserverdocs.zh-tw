---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: 建立宣告提供者信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fbc8bb63435211a92cb7fc6aa05b1413aef939c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854229"
---
# <a name="create-a-claims-provider-trust"></a>建立宣告提供者信任

>適用於：Windows Server 2016, Windows Server 2012 R2

若要新增宣告提供者信任使用 AD FS 管理嵌入式管理單元\-中並手動進行設定、 資源夥伴組織中的資源夥伴同盟伺服器上執行下列程序。  
  
中的成員資格**系統管理員**，或同等權限，在本機電腦上完成此程序的最低需求。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>手動建立宣告提供者信任  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  底下**動作**，按一下**新增宣告提供者信任**。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在 [歡迎使用] 頁面上按一下 [開始]。 
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在 [選取資料來源] 頁面上按一下 [手動輸入宣告提供者信任資料]，然後按 [下一步]。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  在 [指定顯示名稱] 頁面上，輸入 [顯示名稱]，在 [記事] 底下，輸入此宣告提供者信任的描述，然後按 [下一步]。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  在 **設定 URL**頁面上，指定**WS-同盟被動式 URL**的話，按一下 **下一步**。
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. 在 [設定識別碼] 頁面的 [宣告提供者信任識別碼] 底下，輸入適當的識別碼，然後按 [下一步]。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. 在 [設定憑證] 頁面上，按一下 [新增] 找出憑證檔案，將它新增到憑證清單中，然後按 [下一步]。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. 在 [準備新增信任] 頁面上，按 [下一步] 儲存您的宣告提供者信任資訊。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. 在 [完成] 頁面上，按一下 [關閉]。 此動作會自動顯示 [編輯宣告規則] 對話方塊。 如需有關如何繼續新增宣告規則，此宣告提供者信任，請參閱下列的其他參考。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>若要建立宣告提供者的信任關係，使用同盟中繼資料
若要新增宣告提供者信任，使用 AD FS 管理嵌入式管理單元，自動從夥伴已經發佈到區域網路或網際網路的同盟中繼資料匯入有關夥伴的設定資料來執行下列程序資源夥伴組織中的同盟伺服器。

>[!NOTE]
>雖然長久以來的常見的作法是使用憑證具有不完整的主機名稱，例如 https://myserver，這些憑證沒有安全性價值，並且可以讓攻擊者模擬 Federation Service 會發佈同盟中繼資料。 因此，當查詢同盟中繼資料，您應該只使用完整的網域名稱這類 https://myserver.contoso.com。

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  底下**動作**，按一下**新增宣告提供者信任**。  
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在 [歡迎使用] 頁面上按一下 [開始]。 
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在 [選取資料來源] 頁面，按一下 [匯入於線上或區域網路上發行的宣告提供者相關資料]。 在 同盟中繼資料位址 （主機名稱或 URL），鍵入**同盟中繼資料 URL**或 主機合作夥伴，命名，然後按一下**下一步**。
![宣告提供者信任](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  在 [指定顯示名稱] 頁面中輸入**顯示名稱**下方資訊輸入此宣告提供者信任的描述，然後按一下 [**下一步]**。

6.  在 [準備新增信任] 頁面上，按一下 [**下一步]** 來儲存您的宣告提供者信任資訊。

7.  在 完成 頁面上，按一下 **關閉**。 這會自動顯示 [編輯宣告規則] 對話方塊。 如需有關如何繼續新增宣告規則，此宣告提供者信任，請參閱其他參考一節。



    
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定資源夥伴組織](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[檢查清單：建立宣告規則的宣告提供者信任](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
  
