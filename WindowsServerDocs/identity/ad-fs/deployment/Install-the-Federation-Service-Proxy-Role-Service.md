---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: 安裝同盟服務角色服務
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b9db53267afed5ffc68b117676d38293e5121ec2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972165"
---
# <a name="install-the-federation-service-proxy-role-service"></a>安裝同盟服務角色服務

使用必要的應用程式和憑證設定電腦之後，您就可以安裝 Active Directory 同盟服務 AD FS 的同盟服務 Proxy 角色服務 \( \) 。 您可以使用下列程式來安裝同盟服務 Proxy 角色服務。 當您在電腦上安裝同盟服務 Proxy 角色服務時，該電腦會成為同盟伺服器 Proxy。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>使用伺服器管理員安裝同盟服務 Proxy 角色服務

1.  在 [**開始**] 畫面上，輸入**伺服器管理員**，然後按 enter。

2.  按一下 [管理]****，然後按 [新增角色及功能]**** 啟動 [新增角色及功能精靈]。

3.  在 [在您開始前]  頁面上，按一下 [下一步]  。

4.  在 [**選取安裝類型**] 頁面上，按一下 [**角色型 \- 或功能型 \- 安裝**]，然後按 **[下一步]**。

5.  在 [選取目的地伺服器]**** 頁面中，按一下 [從伺服器集區選取伺服器]****，確認目標電腦已醒目提示，然後再按 [下一步]****。

6.  在 [**選取伺服器角色**] 頁面上，按一下 [**遠端存取**]，然後按 [下一步]。

    > [!NOTE]
    > 如果系統提示您安裝其他 .NET Framework 或 Windows 處理序啟用服務功能，請按一下 [新增功能]**** 進行安裝。

7. 在 [選取角色服務]**** 頁面上，選取 [Federation Service Proxy]**** 核取方塊，然後按 [下一步]****。

8. 在您驗證 [確認安裝選項]**** 頁面上的資訊之後，請選取 [必要時自動重新啟動目的地伺服器]**** 核取方塊，然後按一下 [安裝]****。

13. 在 [安裝進度]**** 頁面中，確認所有項目均已順利安裝，然後按一下 [關閉]****。

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>使用 PowerShell 安裝同盟服務 Proxy 角色服務

1. 開啟 Windows PowerShell ([以系統管理員身分執行]) 

2. 輸入下列命令，然後按**enter**鍵：

    ```
    Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools
    ```

## <a name="additional-references"></a>其他參考資料

- [檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

- [檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)
