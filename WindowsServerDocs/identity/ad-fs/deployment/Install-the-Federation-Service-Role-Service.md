---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: 安裝同盟服務角色服務
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6c214d18c7075187ead3122ce5b6bfc3c5c8e4d5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963770"
---
# <a name="install-the-federation-service-role-service"></a>安裝同盟服務角色服務

既然您已使用必要的應用程式和憑證正確設定電腦，您就可以安裝 Active Directory 同盟服務 AD FS 的同盟服務角色服務 \( \) 。 當您在電腦上安裝同盟服務時，該電腦會變成同盟伺服器。  
  
> [!NOTE]  
> 針對同盟 Web 單一 \- 登錄 \- \( SSO \) 設計，您必須在帳戶夥伴組織中至少有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11))。  
  
您可以使用下列程式，在將成為第一部同盟伺服器的電腦上，或在將成為現有同盟伺服器陣列之同盟伺服器的電腦上，安裝 AD FS 的同盟服務角色服務。  
  
## <a name="prerequisites"></a>必要條件  
開始此程式之前，請先確認已將具有私密金鑰的 SSL 憑證安裝或匯入到本機憑證存放區 \( 個人存放區 \) 。 如果您將使用 \- 由憑證授權單位單位 CA 所發行的權杖簽署憑證 \( ，請在 \) 開始此程式之前，確認已將 \- 含有私密金鑰的權杖簽署憑證安裝或匯入到本機憑證存放區 \( 個人存放區 \) 。 或者，您可以 \- \- 使用 [新增角色] [Wizard] 來建立自我簽署的權杖簽署憑證，如本程式中所述。 如需權杖簽署憑證的詳細資訊 \- ，請參閱[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkId \= 83477 \) 。   
  
#### <a name="to-install-the-federation-service-role-service"></a>安裝 Federation Service 角色服務  
  
1.  在 [**開始**] 畫面上，輸入**伺服器管理員**，然後按 enter。  
  
2.  按一下 [管理]****，然後按 [新增角色及功能]**** 啟動 [新增角色及功能精靈]。  
  
3.  在 [在您開始前]  頁面上，按一下 [下一步]  。  
  
4.  在 [**選取安裝類型**] 頁面上，按一下 [**角色型 \- 或功能型 \- 安裝**]，然後按 **[下一步]**。  
  
5.  在 [選取目的地伺服器]**** 頁面中，按一下 [從伺服器集區選取伺服器]****，確認目標電腦已醒目提示，然後再按 [下一步]****。  
  
6.  在 [選取伺服器角色]**** 頁面上，按一下 [Active Directory Federation Services]****，然後按 [下一步]。  
  
    > [!NOTE]  
    > 如果系統提示您安裝其他 .NET Framework 或 Windows 處理序啟用服務功能，請按一下 [新增功能]**** 進行安裝。  
  
7.  在 [選取功能]**** 頁面上，確認已設定功能，然後按 [下一步]****。  
  
8.  在 [ **Active Directory 同盟服務 \( AD FS \) ** ] 頁面上，按 **[下一步]**。  
  
9. 在 [選取角色服務]**** 頁面上，選取 [Federation Service]**** 核取方塊，然後按 [下一步]****。  
  
10. 在 [**網頁伺服器角色 \( IIS \) ** ] 頁面上，按 **[下一步]**。  
  
11. 在 [選取角色服務]**** 頁面上，按 [下一步]****。  
  
12. 在您驗證 [確認安裝選項]**** 頁面上的資訊之後，請選取 [必要時自動重新啟動目的地伺服器]**** 核取方塊，然後按一下 [安裝]****。  
  
13. 在 [安裝進度]**** 頁面中，確認所有項目均已順利安裝，然後按一下 [關閉]****。  
  
