---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: "安裝同盟服務 Proxy 角色服務"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>安裝同盟服務 Proxy 角色服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

憑證必要條件應用程式與設定電腦之後，您已經安裝的 Active Directory 同盟服務 \(AD FS\) 同盟服務 Proxy 角色服務。 您可以使用下列程序安裝同盟服務 Proxy 角色服務。 當您在電腦上安裝同盟服務 Proxy 角色服務時，該電腦就會聯盟 proxy 伺服器。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>若要安裝同盟服務 Proxy 角色服務  
  
1.  在**[開始]**畫面中，輸入**伺服器管理員]**，然後按 ENTER 鍵。  
  
2.  按一下**管理**，然後按一下 [**新增角色與功能**開始新增角色與精靈中的功能。  
  
3.  在**在您開始之前**頁面上，按一下 [**下**。  
  
4.  在**選擇安裝類型**頁面上，按一下 [ **Role\ 或 Feature\ 安裝**，並按一下 [**下一步**。  
  
5.  在**選取目的伺服器**頁面上，按一下 [**伺服器集區中選取 [伺服器**，確認的目標電腦反白，然後按**下一步**。  
  
6.  在**選擇伺服器角色**頁面上，按一下 [ **Active Directory 同盟服務**，然後按一下 [下一步。  
  
    > [!NOTE]  
    > 如果系統提示您安裝其他的.NET Framework 或 Windows 程序啟動服務的功能，請按一下**新增功能**進行安裝。  
  
7.  在**選擇功能**頁面上，確認功能的設定，然後按**下一步**。  
  
8.  在**Active Directory 同盟服務 \(AD FS\)**頁面上，按**下**。  
  
9. 在**選擇角色服務**頁面上，選取 [**同盟服務 Proxy**核取方塊，，然後按一下 [**下一步**。  
  
10. 在**網頁伺服器角色 \(IIS\)**頁面上，按一下 [**下**。  
  
11. 在**選擇角色服務**頁面上，按一下 [**下**。  
  
12. 在確認此資訊後**確認安裝選項**頁面上，選取**必要時自動重新開機目的伺服器**核取方塊，並再按**安裝**。  
  
13. 在**安裝進度**頁面，確認所有正確，安裝，然後按一下 [**關閉**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

