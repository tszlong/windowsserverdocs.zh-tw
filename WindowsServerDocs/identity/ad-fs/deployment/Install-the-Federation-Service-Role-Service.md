---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: "安裝同盟服務的角色"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-role-service"></a>安裝同盟服務的角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

既然您正確憑證必要條件應用程式與設定電腦，您就準備好要安裝的 Active Directory 同盟服務 \(AD FS\) 同盟服務角色服務。 當您在電腦上安裝同盟服務時，該電腦就會聯盟伺服器。  
  
> [!NOTE]  
> 聯盟網路 Single\-Sign\-On \(SSO\) 設計，您必須至少一個聯盟伺服器 account 合作夥伴組織和資源合作夥伴組織中的至少一個聯盟伺服器。 如需詳細資訊，請查看[放置聯盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
您可以使用下列程序會成為第一個聯盟伺服器的電腦或會成為聯盟伺服器現有聯盟伺服器發電廠的電腦上安裝同盟服務角色服務 AD fs。  
  
## <a name="prerequisites"></a>必要條件  
請確認使用私密金鑰 SSL 憑證的已安裝或此程序您在開始之前，匯入至本機憑證存放區 \(Personal store\)。 如果您要使用的 token\ 簽署憑證授權單位發行憑證 \(CA\)，驗證，以私密金鑰 token\ 簽署憑證已經安裝或此程序您在開始之前，匯入至本機憑證存放區 \(Personal store\)。 或者，您可以建立 self\ 簽署、 token\ 簽署憑證使用加入角色精靈中，此程序中所述。 如需 token\ 簽署的憑證的詳細資訊，請查看[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
#### <a name="to-install-the-federation-service-role-service"></a>若要安裝同盟服務的角色  
  
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
  
9. 在**選擇角色服務**頁面上，選取 [**同盟服務**核取方塊，，然後按一下 [**下一步**。  
  
10. 在**網頁伺服器角色 \(IIS\)**頁面上，按一下 [**下**。  
  
11. 在**選擇角色服務**頁面上，按一下 [**下**。  
  
12. 在確認此資訊後**確認安裝選項**頁面上，選取**必要時自動重新開機目的伺服器**核取方塊，並再按**安裝**。  
  
13. 在**安裝進度**頁面，確認所有正確，安裝，然後按一下 [**關閉**。  
  

