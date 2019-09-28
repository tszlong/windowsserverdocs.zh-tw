---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: 安裝同盟服務角色服務
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 73c564e1c1117b229f759ca114b18a2d4c8002fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408388"
---
# <a name="install-the-federation-service-role-service"></a>安裝同盟服務角色服務

既然您已正確設定具有必要條件應用程式和憑證的電腦，就可以開始安裝 Active Directory 同盟服務的同盟服務角色服務 \(AD FS @ no__t-1。 當您在電腦上安裝同盟服務時，該電腦會變成同盟伺服器。  
  
> [!NOTE]  
> 針對同盟 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 設計，您必須在帳戶夥伴組織中至少有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
您可以使用下列程式，在將成為第一部同盟伺服器的電腦上，或在將成為現有同盟伺服器陣列之同盟伺服器的電腦上，安裝 AD FS 的同盟服務角色服務。  
  
## <a name="prerequisites"></a>必要條件  
開始此程式之前，請先確認已安裝私密金鑰的 SSL 憑證，或已將其匯入本機憑證存放區 @no__t 0Personal 存放區 @ no__t-1。 如果您將使用由憑證授權單位單位 \(CA @ no__t-2 所發行的權杖 @ no__t-0signing 憑證，請確認已在本機憑證存放區中安裝或匯入具有私密金鑰的權杖 @ no__t-3signing 憑證在開始此程式之前，請先 @no__t 4Personal 儲存 @ no__t-5。 或者，您可以使用 [新增角色] [Wizard] 來建立自我 @ no__t-0signed、token @ no__t-1signing 憑證，如本程式中所述。 如需有關 token @ no__t-0signing 憑證的詳細資訊，請參閱[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(HTTP：\/ \/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
#### <a name="to-install-the-federation-service-role-service"></a>安裝 Federation Service 角色服務  
  
1.  在 [**開始**] 畫面上，輸入**伺服器管理員**，然後按 enter。  
  
2.  按一下 [**管理**]，然後按一下 [**新增角色及功能**] 來啟動 [新增角色及功能]。  
  
3.  在 [在您開始前] 頁面上，按一下 [下一步]。  
  
4.  在 [**選取安裝類型**] 頁面上，按一下 [ **Role @ no__t-2Based] 或 [功能 @ no__t-3based 安裝**]，然後按 **[下一步]** 。  
  
5.  在 [**選取目的地伺服器**] 頁面上，按一下 **[從伺服器集區選取伺服器**]，確認目的電腦已反白顯示，然後按 **[下一步]** 。  
  
6.  在 [**選取伺服器角色**] 頁面上，按一下 [ **Active Directory 同盟服務**]，然後按 [下一步]。  
  
    > [!NOTE]  
    > 如果系統提示您安裝其他 .NET Framework 或 Windows 進程啟用服務功能，請按一下 [**新增功能**] 來安裝它們。  
  
7.  在 [**選取功能**] 頁面上，確認已設定功能，然後按 **[下一步]** 。  
  
8.  在 [ **Active Directory 同盟服務 \(AD FS @ no__t-2** ] 頁面上，按 **[下一步]** 。  
  
9. 在 [**選取角色服務**] 頁面上，選取 [**同盟服務**] 核取方塊，然後按 **[下一步]** 。  
  
10. 在 [ **Web 服務器角色 \(IIS @ no__t-2** ] 頁面上，按 **[下一步]** 。  
  
11. 在 [選取角色服務] 頁面上，按 [下一步]。  
  
12. 驗證 [確認安裝選項] 頁面上的資訊之後，請選取 [需要時自動重新啟動目的伺服器] 核取方塊，然後按一下 [安裝]。  
  
13. 在 [安裝進度] 頁面上，確認每個項目都已正確安裝，然後按一下 [關閉]。  
  

