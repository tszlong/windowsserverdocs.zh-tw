---
description: 深入瞭解：設定同盟伺服器
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: 為 Windows Server 2012 R2 AD FS 設定同盟伺服器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3ba586f7512f38a10cd516ace9545da4bbb6d7ed
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697006"
---
# <a name="configure-a-federation-server"></a>設定同盟伺服器


在您的 \( 電腦上安裝 Active Directory 同盟服務 AD FS \) 角色服務之後，您就可以設定這部電腦成為同盟伺服器。 您可以執行下列其中一項：

-   [在新的同盟伺服器陣列中設定第一部同盟伺服器](Configure-a-Federation-Server.md#configure-the-first-federation-server-in-a-new-federation-server-farm)

-   [新增同盟伺服器至現有的同盟伺服器陣列](Configure-a-Federation-Server.md#add-a-federation-server-to-an-existing-federation-server-farm)

## <a name="configure-the-first-federation-server-in-a-new-federation-server-farm"></a>在新的同盟伺服器陣列中設定第一部同盟伺服器

### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>使用 Active Directory 同盟服務設定向導，在新的同盟伺服器陣列中設定第一部同盟伺服器

> [!NOTE]
> 執行此程式之前，請確定您具有網域系統管理員許可權，或具有網域系統管理員認證可供使用。

1.  在伺服器管理員的 [儀表板] 頁面上，按一下 [通知] 旗標，然後按一下 [在伺服器上設定同盟服務]。

    [Active Directory Federation Service 設定精靈] 隨即開啟。

2.  在 [歡迎] 頁面上，選取 [在同盟伺服器陣列中建立第一部同盟伺服器]，然後按一下 [下一步]。

3.  在 [連線 **到 AD DS]** 頁面上，使用此電腦加入之 Active Directory AD 網域的網域系統管理員許可權來指定帳戶 \( \) ，然後按 **[下一步]**。

4.  在 [指定服務內容] 頁面上執行下列動作，然後按一下 [下一步]：

    -   匯入 .pfx 檔案，其中包含您稍早取得的安全通訊端層 \( SSL \) 憑證和金鑰。 在 [步驟2：註冊 AD FS 的 SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)時，您已取得此憑證，並將它複製到您要設定為同盟伺服器的電腦上。 若要透過 wizard 匯入 .pfx 檔案，請按一下 [匯 **入**]，然後流覽至檔案的位置。 當系統提示您時，請輸入 .pfx 檔案的密碼。

    -   提供同盟服務的名稱。 例如，**fs.contoso.com**。 這個名稱必須符合憑證中的其中一個主旨或主旨替代名稱。

    -   提供同盟服務的顯示名稱。 例如，**Contoso Corporation**。 使用者會在 Active Directory 同盟服務 \( AD FS 登 \) \- 入頁面上看到此名稱。

5.  在 [指定服務帳戶] 頁面上，指定服務帳戶。 您可以建立或使用現有的群組受管理的服務帳戶 \( gMSA \) 或使用現有的網域使用者帳戶。 如果您選取建立新 gMSA 帳戶的選項，請指定新帳戶的名稱。 如果您選取使用現有 gMSA 或網域帳戶的選項，請按一下 [ **選取** ] 來選取帳戶。

    > [!NOTE]
    > 使用 gMSA 帳戶的優點是它的自動 \- 協商密碼更新功能。

    > [!WARNING]
    > 如果您想要使用 gMSA 帳戶，您的環境中至少必須有一個執行 Windows Server 2012 作業系統的網域控制站。
    >
    > 如果 [gMSA] 選項已停用，而且您看到錯誤訊息，例如 **[群組受管理的服務帳戶] 無法使用，因為尚未設定 Kds 根金鑰根金鑰**，您可以在 Active Directory 網域中執行 Windows Server 2012 或更新版本的網域控制站上執行下列 Windows PowerShell 命令，以啟用網域中的 gMSA： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)` 。 然後返回嚮導，按一下 [ **上** 一步]，然後按一下 [ **下一步]** ，再 \- 輸入 [ **指定服務帳戶** ] 頁面。 現在應該會啟用 gMSA 選項。 您可以選取它，然後輸入您想要使用的 gMSA 帳戶名稱。

6.  在 [ **指定設定資料庫** ] 頁面上，指定 AD FS 設定資料庫，然後按 **[下一步]**。 您可以使用 Windows 內部資料庫 WID 在這部電腦上建立資料庫，也可以 \( \) 指定 Microsoft SQL Server 的位置和實例名稱。

    如需詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。

    > [!IMPORTANT]
    > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。

7.  在 [檢閱選項] 頁面上，檢查您的設定選項，然後按一下 [下一步]。

8.  在 [ **先決條件 \- 檢查** ] 頁面上，確認所有先決條件檢查都已順利完成，然後按一下 [ **設定**]。

9. 在 [ **結果** ] 頁面上，檢查結果並檢查設定是否已順利完成，然後按一下 **[完成 federation service 部署所需的後續步驟]**。 如需詳細資訊，請參閱 [完成 AD FS 安裝的後續步驟](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下 [關閉] 結束精靈。

### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-via-windows-powershell"></a>透過 Windows PowerShell 以在新的同盟伺服器陣列中設定第一部同盟伺服器
您可以使用新的或現有的 gMSA 帳戶或現有的網域使用者帳戶，來建立新的同盟伺服器陣列。

-   **如果您想要使用新的 gMSA 帳戶來建立新的同盟伺服器，請執行下列動作：**

    > [!IMPORTANT]
    > 您必須具有網域系統管理員權限，才能在新的同盟伺服器陣列中建立第一部同盟伺服器。

    1.  在您要設定為同盟伺服器的電腦上，確定所需的 SSL 憑證已匯入至本機 **電腦 \\ My Store** 目錄。 您可以在 Windows PowerShell 命令視窗中執行下列命令，以確認 SSL 憑證是否已匯入： `dir Cert:\LocalMachine\My` 。 憑證會依憑證指紋在本機 **電腦 \\ My Store** 目錄中列出。

    2.  在您的網域控制站上，開啟 [Windows PowerShell 命令] 視窗並執行下列命令，以確認是否已在您的網域中建立 KDS 根金鑰的根金鑰： `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)` 。 如果尚未建立，因此輸出不會顯示任何資訊，請執行下列命令來建立金鑰： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)` 。

    3.  在您要設定為同盟伺服器的電腦上，開啟 Windows PowerShell 命令視窗，然後執行下列命令：

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$
        ```

        > [!WARNING]
        > `$`需要上述命令結尾的正負號。

        若要取得的值 `<certificate_thumbprint>` ，請執行 `dir Cert:\LocalMachine\My` ，然後選取 SSL 憑證的指紋。 `<federation_service_name>` 的值是同盟服務的名稱，例如，**fs.contoso.com**。

        > [!NOTE]
        > 如果這不是您第一次執行此命令，請新增 `OverwriteConfiguration` 參數。

        > [!NOTE]
        > 上述命令會建立 WID 伺服器陣列。 如果您想要建立 SQL Server 服務器陣列，您必須已安裝且可運作的 SQL Server 實例。
        >
        > 您可以使用下列命令，在新的伺服器陣列中建立第一部同盟伺服器，該伺服器使用 SQL Server 的實例： `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` 其中 **<sql \_ 主機 \_ 名>** 是 SQL Server 執行所在的伺服器名稱，而 **<SQL \_ 實例 \_ 名稱>** 是 SQL Server 實例的名稱。 如果您使用 SQL Server 的預設實例，請使用「**資料來源 \=<SQL \_ 主機 \_ 名>; 整合式安全性 \= True**」的 **SQLConnectionString** 值。

        > [!IMPORTANT]
        > 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。

-   **如果您想要使用現有的網域使用者帳戶來建立新的同盟伺服器，請執行下列動作：**

    1.  在您要設定為同盟伺服器的電腦上，確定所需的 SSL 憑證已匯入至本機 **電腦 \\ My Store** 目錄。 您可以在 Windows PowerShell 命令視窗中執行下列命令，以確認 SSL 憑證是否已匯入： `dir Cert:\LocalMachine\My` 。 憑證會依憑證指紋在本機 **電腦 \\ My Store** 目錄中列出。

    2.  在您要設定為同盟伺服器的電腦上，開啟 [Windows PowerShell 命令] 視窗，然後執行下列命令： `$fscred = Get-Credential` 。 輸入您想要用於 federation service 帳戶的網域使用者帳號憑證，格式為網域 \\ 使用者名稱。

    3.  在相同的 Windows PowerShell 命令視窗中，執行下列命令：

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred
        ```

        若要取得 **<憑證 \_ 指紋>** 的值，請執行 `dir Cert:\LocalMachine\My` ，然後選取 SSL 憑證的指紋。 **<federation \_ service \_ 名稱>** 的值是同盟服務的名稱，例如 fs.contoso.com。

        > [!NOTE]
        > 如果這不是您第一次執行此命令，請新增 `OverwriteConfiguration` 參數。

        > [!NOTE]
        > 上述命令會建立 WID 伺服器陣列。 如果您想要建立 SQL Server 服務器陣列，您必須已安裝且可運作 SQL Server 的實例。
        >
        > 您可以使用下列命令，在新的伺服器陣列中建立第一部同盟伺服器，該伺服器使用 SQL Server 的實例： `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` 其中 **sql \_ 主控制項 \_ 名稱** 是執行 SQL Server 之伺服器的名稱，而 **sql \_ 實例 \_ 名稱** 則是 SQL Server 實例的名稱。 如果您使用 SQL Server 的預設實例，請使用「**資料來源 \=<SQL \_ 主機 \_ 名>; 整合式安全性 \= True**」的 **SQLConnectionString** 值。

        > [!IMPORTANT]
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。

## <a name="add-a-federation-server-to-an-existing-federation-server-farm"></a>新增同盟伺服器至現有的同盟伺服器陣列

> [!IMPORTANT]
> 在開始本節中的任何程式之前，請確定您已完成 [步驟3：安裝 AD FS 的角色服務](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)。

> [!IMPORTANT]
> 完成此程式之前，請確定您已取得有效的 SSL 伺服器驗證憑證。

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>透過 Active Directory Federation Service 組態精靈新增同盟伺服器至現有的同盟伺服器陣列

1.  在伺服器管理員的 [儀表板] 頁面上，按一下 [通知] 旗標，然後按一下 [在伺服器上設定同盟服務]。

    [Active Directory Federation Service 設定精靈] 隨即開啟。

2.  在 [ **歡迎使用** ] 頁面上，選取 [ **將同盟伺服器新增至同盟伺服器** 陣列]，然後按 **[下一步]**。

3.  在 [連線 **到 AD DS]** 頁面上，使用此電腦加入之 AD 網域的網域系統管理員許可權來指定帳戶，然後按 **[下一步]**。

4.  在 [ **指定伺服器** 陣列] 頁面上，于使用 WID 的伺服器陣列中提供主要同盟伺服器的名稱，或指定使用 SQL Server 之現有同盟伺服器陣列的資料庫主機名稱和資料庫實例名稱。

    > [!WARNING]
    > 在 Windows Server &reg; 2012 R2 中，有因應措施可指定 SQL Server 的預設實例。 解決辦法為不使用使用者介面。 相反地，請使用中的步驟，透過 [Windows PowerShell 在新的同盟伺服器陣列中設定第一部同盟伺服器](#to-configure-the-first-federation-server-in-a-new-federation-server-farm-via-windows-powershell)。

    > [!IMPORTANT]
    > 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。

5.  在 [ **指定 Ssl 憑證** ] 頁面上，匯入 .pfx 檔案，其中包含您先前取得的 SSL 憑證和金鑰。 此憑證是必要的服務驗證憑證。 在 [步驟2：註冊 AD FS 的 SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)時，您已取得此憑證，並將它複製到您要設定為同盟伺服器的電腦。 若要透過 wizard 匯入 .pfx 檔案，請按一下 [匯 **入** ]，然後流覽至檔案的位置。 當系統提示您時，請輸入 .pfx 檔案的密碼。

6.  在 [ **指定服務帳戶** ] 頁面上，指定您在伺服器陣列中建立第一部同盟伺服器時所設定的相同服務帳戶。 您可以使用現有群組受管理的服務帳戶或現有的網域使用者帳戶。

    > [!IMPORTANT]
    > 您指定的帳號必須與此伺服器陣列中主要同盟伺服器上使用的帳戶相同。

7.  在 [檢閱選項] 頁面上，檢查您的設定選項，然後按一下 [下一步]。

8.  在 [ **先決條件 \- 檢查** ] 頁面上，確認所有先決條件檢查都已順利完成，然後按一下 [ **設定**]。

9. 在 [ **結果** ] 頁面上，檢查結果並檢查設定是否已順利完成，然後按一下 **[完成 federation service 部署所需的後續步驟]**。 如需詳細資訊，請參閱 [完成 AD FS 安裝的後續步驟](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下 [關閉] 結束精靈。

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>透過 Windows PowerShell 新增同盟伺服器至現有的同盟伺服器陣列
您可以使用現有的 gMSA 帳戶或現有的網域使用者帳戶，將同盟伺服器新增至現有的伺服器陣列。

-   如果您想要使用現有的 gMSA 帳戶將同盟伺服器加入至伺服器陣列，請執行下列動作：

    1.  在您要設定為同盟伺服器的電腦上，確定所需的 SSL 憑證已匯入至本機 **電腦 \\ My Store** 目錄。 您可以在 Windows PowerShell 命令視窗中執行下列命令，以確認 SSL 憑證是否已匯入： `dir Cert:\LocalMachine\My` 。 憑證會依憑證指紋在本機 **電腦 \\ My Store** 目錄中列出。

    2.  在您要設定為同盟伺服器的電腦上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。

        ```
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        `<domain>\<GMSA_name>` 是您的 AD 網域，以及該網域中 gMSA 帳戶的名稱。 `<first_federation_server_hostname>` 這是此現有伺服器陣列中主要同盟伺服器的主機名稱。

        您可以藉 `<certificate_thumbprint>` 由 `dir Cert:\LocalMachine\My` 在上一個步驟中執行來取得的值。

        > [!NOTE]
        > 如果這不是您第一次執行此命令，請新增 `OverwriteConfiguration` 參數。

        > [!NOTE]
        > 上述命令會建立 WID 伺服器陣列節點。 如果您想要在執行 SQL Server 的電腦上建立伺服器陣列節點，您必須已安裝且可運作 SQL Server 的實例。
        >
        > 您可以使用下列命令，將同盟伺服器加入至使用 SQL Server 實例的現有伺服器陣列中： `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` 其中 **sql \_ 主控制項 \_ 名稱** 是執行 SQL Server 之伺服器的名稱，而 **sql \_ 實例 \_ 名稱** 則是 SQL Server 實例的名稱。 如果您使用 SQL Server 的預設實例，請使用「**資料來源 \=<SQL \_ 主機 \_ 名>; 整合式安全性 \= True**」的 **SQLConnectionString** 值。

        > [!IMPORTANT]
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。

-   如果您想要使用現有的網域使用者帳戶將同盟伺服器加入至伺服器陣列，請執行下列動作：

    1.  在您要設定為同盟伺服器的電腦上，開啟 [Windows PowerShellcommand] 視窗，然後執行下列命令： `$fscred = get-credential` 。 輸入您想要用於 federation service 帳戶的網域使用者帳號憑證，格式為網域 \\ 使用者名稱。

    2.  在您要設定為同盟伺服器的電腦上，確定所需的 SSL 憑證已匯入至本機 **電腦 \\ My Store** 目錄。 您可以在 [Windows PowerShellcommand] 視窗中執行下列命令，以確認 SSL 憑證是否已匯入： `dir Cert:\LocalMachine\My` 。 憑證會依憑證指紋在本機 **電腦 \\ My Store** 目錄中列出。

    3.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。

        ```
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        > [!NOTE]
        > 如果這不是您第一次執行此命令，請新增 `OverwriteConfiguration` 參數。

        > [!NOTE]
        > 上述命令會建立 WID 伺服器陣列節點。 如果您想要在執行 SQL Server 的電腦上建立伺服器陣列節點，您必須已安裝且可運作 SQL Server 的實例。 您可以使用下列命令，將同盟伺服器加入至現有的伺服器陣列，方法是使用 SQL Server 的實例： `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` 其中 **sql 主控制項 \_ \_ 名稱** 是 SQL Server 實例執行所在的伺服器名稱，而 **sql \_ 實例 \_ 名稱** 則是 SQL Server 實例的名稱。 如果您使用 SQL Server 的預設實例，請使用「**資料來源 \=<SQL \_ 主機 \_ 名>; 整合式安全性 \= True**」的 **SQLConnectionString** 值。

        > [!IMPORTANT]
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。

## <a name="see-also"></a>另請參閱

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)
