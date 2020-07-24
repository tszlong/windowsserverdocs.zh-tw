---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: 建立一個非宣告感知信賴憑證者信任
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2bc08d2797812d6001f2a792037bcfb8bab4fc0a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965640"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>建立非宣告感知信賴憑證者信任


在 [AD FS 管理] 嵌入式管理單元中 \- ，非 \- 宣告 \- 感知信賴憑證者信任是建立來代表 federation service 與單一 web 應用 \- 程式（不是宣告 \- 感知，而且可透過 web 應用程式 Proxy 存取）之間信任關係的物件。  
  
非 \- 宣告 \- 感知信賴憑證者信任是信賴憑證者信任，其中包含在透過 Web 應用程式 Proxy 存取信賴憑證者信任時，用於驗證和授權的識別碼、名稱和規則。 這些以宣告為基礎的 web 應用 \- 程式（亦即，這些整合式 Windows 驗證型 \- 應用程式）可能會擁有授權規則，以在透過 Web 應用程式 Proxy 存取公司網路外部時，強制以宣告為基礎的存取權。  
  
若要加入新的非 \- 宣告 \- 感知信賴憑證者信任，請使用中的 [AD FS 管理] 嵌入式管理單元 \- ，執行下列程式。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>手動建立非宣告感知信賴憑證者信任 
1. 在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。  
  
2.  在 [**動作**] 底下，按一下 [**新增信賴**憑證者信任]。  
![信賴憑證者](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在 [**歡迎使用**] 頁面上，選擇 [**非宣告感知**]，然後按一下 [**啟動**]。  
![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  在 [指定顯示名稱]**** 頁面的 [顯示名稱]**** 中輸入名稱、在 [備忘稿]**** 下輸入此信賴憑證者信任的描述，然後按 [下一步]****。  
![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. 在 [設定識別碼]**** 頁面上，指定此信賴憑證者的一或多個識別碼，按一下 [新增]**** 以將它們新增到清單中，然後按一下 [下一步]****。  
![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  在 [選擇存取控制原則]**** 上選擇原則，然後按 [下一步]****。  如需存取控制原則的詳細資訊，請參閱[AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)。 
![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. 在 [準備新增信任]**** 頁面上，檢閱設定，然後按一下 [下一步]**** 以儲存您的信賴憑證者信任資訊。  
   ![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. 在 [完成]**** 頁面上，按一下 [關閉]****。 此動作會自動顯示 [編輯宣告規則]**** 對話方塊。  
![信賴憑證者](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../ad-fs-operations.md) 
