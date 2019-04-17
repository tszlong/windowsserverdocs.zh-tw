---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: "Windows Server 2012 R2 AD FS 部署指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>設定聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2

您在電腦上安裝的 Active Directory 同盟服務 \(AD FS\) 角色服務之後，您就可以設定為聯盟伺服器這台電腦。 您可以執行下列其中一個動作：  
  
-   [將第一次聯盟伺服器設定中新的聯盟伺服器陣列](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [聯盟伺服器加入現有的聯盟伺服器陣列](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>將第一次聯盟伺服器設定中新的聯盟伺服器陣列  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>使用 Active Directory 同盟服務設定精靈中的新聯盟伺服器陣列設定的第一個聯盟伺服器  
  
> [!NOTE]  
> 請確定您有網域系統管理員權限或先執行此程序有可用的網域系統管理員認證。  
  
1.  在伺服器管理員**儀表板**頁面上，按一下 [**通知**標幟，然後按一下 [**設定同盟服務，伺服器上**。  
  
    **Active Directory 同盟服務設定精靈**開啟。  
  
2.  在**歡迎使用**頁面上，選取**聯盟伺服器陣列中建立的第一個聯盟伺服器**，然後按一下 [**下一步**。  
  
3.  在**連接到 AD DS**頁面上的 Active Directory \(AD\) 網域的電腦所加入，然後再按一下使用網域系統管理員權限來指定 account**下**。  
  
4.  在**指定服務屬性**頁面上，執行下列命令，，然後按**下**:  
  
    -   匯入.pfx 檔案中包含安全通訊端層 \(SSL\) 憑證，以及取得更早版本的按鍵。 在[步驟 2：註冊 AD FS SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，您已經取得此憑證，並將它複製到您想要設定為聯盟伺服器的電腦。 若要匯入透過精靈.pfx 檔案，請按一下**匯入**，然後瀏覽到檔案的位置。 當系統提示您輸入的密碼.pfx 檔案。  
  
    -   提供您同盟服務的名稱。 例如，**fs.contoso.com**。此名稱必須符合主旨或主題替代名稱憑證中的其中一個。  
  
    -   提供您同盟服務的顯示名稱。 例如，**以 Contoso Corporation**。 使用者在 Active Directory 同盟服務 \(AD FS\) sign\ 看到此名稱-頁面中。  
  
5.  在**指定服務 Account**頁面上指定的服務 account。 您可以建立，或使用現有的群組管理服務 Account \(gMSA\) 或使用現有的使用者核對。 如果您選取的選項來建立新 gMSA 帳號，指定新 account 的名稱。 如果您選擇使用現有 gMSA 或網域帳號，按**選擇**選取 account。  
  
    > [!NOTE]  
    > 使用 gMSA account 優點是其 auto\ 交涉密碼更新的功能。  
  
    > [!WARNING]  
    > 如果您想要使用 gMSA 帳號，您必須至少網域控制站在您執行 Windows Server 2012 作業系統的環境中。  
    >   
    > 如果已停用 gMSA 選項，且您看到錯誤訊息，例如**群組管理服務帳號因為尚未設定 KDS 根金鑰是無法使用**，您可以在您的網域讓 gMSA，執行下列 Windows PowerShell 命令執行 Windows Server 2012」的網域控制站或更新版本，您的 Active Directory domain: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 然後返回精靈中，按一下 [**上一步]**，，然後按一下 [**下一步**以 re\-輸入**指定服務 Account**頁面。 現在應該會支援 gMSA 選項。 您可以選取它，然後輸入您想要使用的 gMSA account 名稱。  
  
6.  在**指定設定資料庫**頁面，指定 AD FS 設定資料庫，然後按一下 [**下**。 您可以建立資料庫這台電腦上使用 Windows 內部資料庫 \(WID\)，或是您可以指定的位置，以及執行個體的 Microsoft SQL Server 名稱。  
  
    如需詳細資訊，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
    > [!IMPORTANT]  
    > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
7.  在**評論選項**頁面，確認您的設定選項，然後按一下 [**下**。  
  
8.  在**Pre\-requisite 檢查**頁面上，確認所有必要條件檢查成功完成，然後按**設定**。  
  
9. 在**結果**頁面上，檢視結果並檢查是否已成功完成設定，然後按一下**完成同盟服務部署所需的下一個步驟**。 如需詳細資訊，請查看[完成 AD FS 安裝下一個步驟](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下**關閉**以結束精靈。  
  
### <a name="BKMK_3"></a>若要設定新的聯盟伺服器陣列透過 Windows PowerShell 中的第一個聯盟伺服器  
您可以使用新的或現有 gMSA account 或現有使用者核對建立新的聯盟伺服器陣列。  
  
-   **如果您想要使用新的 gMSA account 建立新的聯盟伺服器，執行下列動作：**  
  
    > [!IMPORTANT]  
    > 您必須到的第一個聯盟伺服器建立新的聯盟伺服器陣列網域系統管理員權限。  
  
    1.  在電腦上您想要為聯盟伺服器設定，請確定所需的 SSL 憑證已匯入到**本機 Computer\\My 市集**directory。 您可以檢查是否 SSL 憑證已匯入 Windows PowerShell 命令視窗中執行下列命令：`dir Cert:\LocalMachine\My`。 憑證列在其指紋的**本機 Computer\\My 市集**directory。  
  
    2.  在您的網域控制站，開放 Windows PowerShell 命令視窗中，執行下列命令，以確認是否已在您的網域中建立 KDS 根金鑰：`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 如果該尚未建立使輸出會顯示無資訊，執行下列命令，以建立鍵：`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。  
  
    3.  在電腦上您想要設定為聯盟伺服器，開放的 Windows PowerShell 命令視窗中，並執行下列命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > `$`是必要的登入前一個命令的結尾。  
  
        若要取得的值為`<certificate_thumbprint>`、執行`dir Cert:\LocalMachine\My`，然後選取您的 SSL 憑證的指紋。 值`<federation_service_name>`是您同盟服務的名稱，例如**fs.contoso.com**。  
  
        > [!NOTE]  
        > 如果這不是執行此命令的第一次，新增`OverwriteConfiguration`的參數。  
  
        > [!NOTE]  
        > 前一個命令中建立 WID 發電廠。 如果您想要建立 SQL Server 伺服器陣列，您必須已經安裝並操作 SQL Server 的執行個體。  
        >   
        > 您可以使用下列命令來建立新的發電廠使用 SQL Server 執行個體的第一個聯盟伺服器：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`在**< SQL\_Host\_Name >**的伺服器執行的 SQL Server，名稱和**< SQL\_instance\_name >** SQL Server 的執行個體的名稱。 如果您使用預設的執行個體 SQL server，使用**SQLConnectionString**的值]**資料 Source\ = < SQL\_Host\_Name >; 整合 Security\ true**」。  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012。  
  
-   **如果您想要建立新的聯盟伺服器使用現有的使用者網域帳號，執行下列動作：**  
  
    1.  在電腦上您想要為聯盟伺服器設定，請確定所需的 SSL 憑證已匯入到**本機 Computer\\My 市集**directory。 您可以檢查是否 SSL 憑證已匯入 Windows PowerShell 命令視窗中執行下列命令：`dir Cert:\LocalMachine\My`。 憑證列在其指紋的**本機 Computer\\My 市集**directory。  
  
    2.  在電腦上您想要設定為聯盟伺服器，開放的 Windows PowerShell 命令視窗中，，然後執行下列命令：`$fscred = Get-Credential`。 輸入您想要使用的格式網域 \\ 使用者名稱同盟服務 account 網域使用者 account 認證。  
  
    3.  在同一個 Windows PowerShell 命令視窗中，執行下列命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        若要取得的值為**< certificate\_thumbprint >**、執行`dir Cert:\LocalMachine\My`，然後選取您的 SSL 憑證的指紋。 值**< federation\_service\_name >**是您同盟服務，例如 fs.contoso.com 的名稱。  
  
        > [!NOTE]  
        > 如果這不是執行此命令的第一次，新增`OverwriteConfiguration`的參數。  
  
        > [!NOTE]  
        > 前一個命令中建立 WID 發電廠。 如果您想要建立 SQL Server 陣列，您必須已經安裝並操作 SQL Server 的執行個體。  
        >   
        > 您可以使用下列命令來建立新的發電廠使用 SQL Server 執行個體的第一個聯盟伺服器：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`在**SQL\_Host\_Name**的伺服器執行的 SQL Server，名稱和**SQL\_instance\_name** SQL Server 的執行個體的名稱。 如果您使用預設的執行個體 SQL server，使用**SQLConnectionString**的值]**資料 Source\ = < SQL\_Host\_Name >; 整合 Security\ true**」。  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="BKMK_2"></a>聯盟伺服器加入現有的聯盟伺服器陣列  
  
> [!IMPORTANT]  
> 請確定您已完成[執行「步驟 3：安裝 AD FS 角色服務](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)、之前任何程序開始在本區段中。  
  
> [!IMPORTANT]  
> 具備，您有效 SSL 伺服器驗證憑證才能完成此程序。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>聯盟伺服器加入現有聯盟伺服器陣列透過 Active Directory 同盟服務設定精靈  
  
1.  在伺服器管理員**儀表板**頁面上，按一下 [**通知**標幟，然後按一下 [**設定同盟服務，伺服器上**。  
  
    **Active Directory 同盟服務設定精靈**開啟。  
  
2.  在**歡迎**頁面上，選取**新增至聯盟伺服器陣列聯盟伺服器**，然後按一下 [**下一步**。  
  
3.  在**連接到 AD DS**頁面上，使用 AD 網域的電腦所加入，然後再按一下網域系統管理員權限來指定 account**下**。  
  
4.  在**指定發電廠**頁面上，提供使用 WID 發電廠中的主要同盟伺服器的名稱，或指定資料庫主機名稱及使用 SQL Server 現有聯盟伺服器陣列資料庫執行個體名稱。  
  
    > [!WARNING]  
    > 在 Windows Server® 2012 R2，還有指定預設的執行個體 SQL server 因應措施。 因應措施是使用的使用者介面。 請改用中的步驟執行[設定新聯盟伺服器陣列透過 Windows PowerShell 中的第一個聯盟伺服器](Configure-a-Federation-Server.md#BKMK_3)。  
  
    > [!IMPORTANT]  
    > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012。  
  
5.  在**指定 SSL 憑證**頁面上，匯入.pfx 檔案中包含 SSL 憑證，以及取得先前的金鑰。 這是憑證所需的服務驗證憑證。 在[步驟 2：註冊 AD FS SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，您已經取得此憑證，並將它複製到您想要設定為聯盟伺服器的電腦。 若要匯入透過精靈.pfx 檔案，請按一下**匯入**，然後瀏覽到檔案的位置。 當系統提示您輸入的密碼.pfx 檔案。  
  
6.  在**指定服務 Account**頁面上，指定您設定當您建立的第一個聯盟伺服器發電廠相同服務 account。 現有的群組管理服務 Account 或現有的使用者網域帳號，您可以使用。  
  
    > [!IMPORTANT]  
    > 指定 account 必須是相同的帳號 account 用農場的主要聯盟伺服器上為。  
  
7.  在**評論選項**頁面，確認您的設定選項，然後按一下 [**下**。  
  
8.  在**Pre\-requisite 檢查**頁面上，確認所有必要條件檢查成功完成，然後按**設定**。  
  
9. 在**結果**頁面上，檢視結果並檢查是否已成功完成設定，然後按一下**完成同盟服務部署所需的下一個步驟**。 如需詳細資訊，請查看[完成 AD FS 安裝下一個步驟](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下**關閉**以結束精靈。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>若要新增聯盟伺服器現有聯盟伺服器陣列透過 Windows PowerShell  
您可以使用現有的 gMSA 帳號或現有使用者核對現有發電廠增加聯盟伺服器。  
  
-   如果您想要將聯盟伺服器加入發電廠使用現有的 gMSA 帳號，執行下列動作：  
  
    1.  在電腦上您想要為聯盟伺服器設定，請確定所需的 SSL 憑證已匯入到**本機 Computer\\My 市集**directory。 您可以檢查是否 SSL 憑證已匯入 Windows PowerShell 命令視窗中執行下列命令：`dir Cert:\LocalMachine\My`。 憑證列在其指紋的**本機 Computer\\My 市集**directory。  
  
    2.  在電腦上您想要設定為聯盟伺服器，打開 Windows PowerShell 命令視窗中，並執行下列命令。  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` 為您的廣告網域並 gMSA 帳號網域中的名稱。 `<first_federation_server_hostname>` 是主機現有農場中的主要同盟伺服器的名稱。  
  
        您可以取得的值為`<certificate_thumbprint>`執行`dir Cert:\LocalMachine\My`中的上一個步驟。  
  
        > [!NOTE]  
        > 如果這不是執行此命令的第一次，新增`OverwriteConfiguration`的參數。  
  
        > [!NOTE]  
        > 前一個命令中建立 WID 發電廠節點。 如果您想要建立伺服器發電廠節點執行 SQL Server 的電腦，您必須已經安裝並操作 SQL Server 的執行個體。  
        >   
        > 您可以加入現有發電廠使用 SQL Server 執行個體聯盟伺服器使用下列命令：`Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`位置**SQL\_Host\_Name**執行的 SQL Server，伺服器的名稱及**SQL\_instance\_name** SQL Server 的執行個體的名稱。 如果您使用預設的執行個體 SQL server，使用**SQLConnectionString**的值]**資料 Source\ = < SQL\_Host\_Name >; 整合 Security\ true**」。  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
-   如果您想要聯盟伺服器加入發電廠使用現有的使用者網域帳號，執行下列動作：  
  
    1.  在電腦上您想要為聯盟伺服器設定，Windows PowerShellcommand 名稱，然後執行下列命令：`$fscred = get-credential`。 輸入您想要使用的格式網域 \\ 使用者名稱同盟服務 account 網域使用者 account 認證。  
  
    2.  在電腦上您想要為聯盟伺服器設定，請確定所需的 SSL 憑證已匯入到**本機 Computer\\My 市集**directory。 您可以檢查是否 SSL 憑證已匯入 Windows PowerShellcommand 視窗中執行下列命令：`dir Cert:\LocalMachine\My`。 憑證列在其指紋的**本機 Computer\\My 市集**directory。  
  
    3.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > 如果這不是執行此命令的第一次，新增`OverwriteConfiguration`的參數。  
  
        > [!NOTE]  
        > 前一個命令中建立 WID 發電廠節點。 如果您想要建立伺服器發電廠節點執行 SQL Server 的電腦，您必須已經安裝並操作 SQL Server 的執行個體。 您可以使用下列命令新增聯盟伺服器到現有發電廠使用 SQL Server 執行個體：`Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`位置**SQL\_Host\_Name**執行的 SQL Server 的執行個體，伺服器的名稱及**SQL\_instance\_name** SQL Server 的執行個體的名稱。 如果您使用預設的執行個體 SQL server，使用**SQLConnectionString**的值]**資料 Source\ = < SQL\_Host\_Name >; 整合 Security\ true**」。  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="see-also"></a>也了 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署聯盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

