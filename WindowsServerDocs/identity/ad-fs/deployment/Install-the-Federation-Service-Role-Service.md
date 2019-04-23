---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: 安裝同盟服務角色服務
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883709"
---
# <a name="install-the-federation-service-role-service"></a>安裝同盟服務角色服務

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

既然您已正確設定的必要條件應用程式和憑證的電腦，即準備好安裝 Federation Service 角色服務的 Active Directory Federation Services \(AD FS\)。 當您在電腦上安裝 Federation Service 時，該電腦會變成同盟伺服器。  
  
> [!NOTE]  
> 同盟網頁單一\-號\-上\(SSO\)設計中，您必須在帳戶夥伴組織中的至少一部同盟伺服器和資源夥伴組織中的至少一部同盟伺服器. 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
您可以使用下列程序將成為第一部同盟伺服器的電腦上，或將成為現有的同盟伺服器陣列的同盟伺服器的電腦上安裝 AD FS 的 Federation Service 角色服務。  
  
## <a name="prerequisites"></a>先決條件  
確認 SSL 憑證的私密金鑰，已安裝或匯入本機憑證存放區\(個人存放區\)開始此程序之前。 如果您要使用的語彙基元\-簽署的憑證授權單位所發出的憑證\(CA\)，確認語彙基元\-使用私密金鑰簽署憑證已經安裝或匯入本機憑證存放區\(個人存放區\)開始此程序之前。 或者，您可以建立自我\-簽署，k\-簽署憑證，使用 [新增角色精靈] 中，此程序中所述。 如需語彙基元\-簽署憑證，請參閱[Certificate Requirements for Federation Servers](https://technet.microsoft.com/library/dd807040.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
#### <a name="to-install-the-federation-service-role-service"></a>安裝 Federation Service 角色服務  
  
1.  在 **開始**畫面上，輸入**伺服器管理員**，然後按 ENTER 鍵。  
  
2.  按一下 **管理**，然後按一下**新增角色及功能**以啟動 新增角色及功能精靈。  
  
3.  在 [在您開始前]  頁面上，按一下 [下一步] 。  
  
4.  在上**選取安裝類型**頁面上，按一下**角色\-型或功能型\-基礎安裝**，然後按一下**下一步**。  
  
5.  在 **選取目的地伺服器**頁面上，按一下**從伺服器集區選取伺服器**，確認目標電腦會反白顯示，然後按一下 **下一步**。  
  
6.  在 **選取伺服器角色**頁面上，按一下**Active Directory Federation Services**，然後按一下 下一步。  
  
    > [!NOTE]  
    > 如果提示您安裝其他的.NET Framework 或 Windows Process Activation Service 功能時，請按一下**將功能加入**安裝它們。  
  
7.  在 [**選取的功能**頁面上，確認功能設定，然後按一下**下一步]**。  
  
8.  在 [ **Active Directory Federation Service \(AD FS\)** 頁面上，按一下**下一步]**。  
  
9. 在 [**選取角色服務**頁面上，選取**Federation Service**核取方塊，然後按一下**下一步]**。  
  
10. 在 [**網頁伺服器角色\(IIS\)** 頁面上，按一下**下一步]**。  
  
11. 在 [選取角色服務] 頁面上，按 [下一步]。  
  
12. 驗證 [確認安裝選項]  頁面上的資訊之後，請選取 [需要時自動重新啟動目的伺服器]  核取方塊，然後按一下 [安裝] 。  
  
13. 在 [安裝進度] 頁面上，確認每個項目都已正確安裝，然後按一下 [關閉]。  
  

