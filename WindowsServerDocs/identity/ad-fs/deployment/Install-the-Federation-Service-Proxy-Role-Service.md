---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: 安裝同盟服務角色服務
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed66800aa6bbfdf85816a992ee8eb39799efebb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865229"
---
# <a name="install-the-federation-service-proxy-role-service"></a>安裝同盟服務角色服務

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以設定電腦的必要條件應用程式和憑證之後，您就準備好要安裝 Federation Service Proxy 角色服務的 Active Directory Federation Services \(AD FS\)。 若要安裝 Federation Service Proxy 角色服務，您可以使用下列程序。 當您在電腦上安裝 Federation Service Proxy 角色服務時，該電腦會變成同盟伺服器 proxy。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>若要安裝 Federation Service Proxy 角色服務，使用 伺服器管理員
  
1.  在 **開始**畫面上，輸入**伺服器管理員**，然後按 ENTER 鍵。  
  
2.  按一下 **管理**，然後按一下**新增角色及功能**以啟動 新增角色及功能精靈。  
  
3.  在 [在您開始前]  頁面上，按一下 [下一步] 。  
  
4.  在上**選取安裝類型**頁面上，按一下**角色\-型或功能型\-基礎安裝**，然後按一下**下一步**。  
  
5.  在 **選取目的地伺服器**頁面上，按一下**從伺服器集區選取伺服器**，確認目標電腦會反白顯示，然後按一下 **下一步**。  
  
6.  在 **選取伺服器角色**頁面上，按一下**遠端存取**，然後按一下 下一步。  
  
    > [!NOTE]  
    > 如果提示您安裝其他的.NET Framework 或 Windows Process Activation Service 功能時，請按一下**將功能加入**安裝它們。  
  
7. 在 [**選取角色服務**頁面上，選取**Federation Service Proxy**核取方塊，然後按一下**下一步]**。  

8. 驗證 [確認安裝選項]  頁面上的資訊之後，請選取 [需要時自動重新啟動目的伺服器]  核取方塊，然後按一下 [安裝] 。  
  
13. 在 [安裝進度] 頁面上，確認每個項目都已正確安裝，然後按一下 [關閉]。  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>若要安裝 Federation Service Proxy 角色服務，使用 PowerShell

1. 開啟 Windows PowerShell （以系統管理員身分執行）

2. 輸入下列命令，然後按**Enter**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

