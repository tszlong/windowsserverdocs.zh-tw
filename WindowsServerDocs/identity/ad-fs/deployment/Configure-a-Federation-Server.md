---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Windows Server 2012 R2 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847379"
---
# <a name="configure-a-federation-server"></a>設定同盟伺服器

>適用於：Windows Server 2016, Windows Server 2012 R2

安裝 Active Directory Federation Services 之後\(AD FS\)角色服務在您的電腦，您已準備好將此電腦成為同盟伺服器設定。 您可以執行下列其中一個步驟：  
  
-   [在 新的同盟伺服器陣列中設定第一部同盟伺服器](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [將同盟伺服器新增至現有的同盟伺服器陣列](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>在 新的同盟伺服器陣列中設定第一部同盟伺服器  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>若要設定新的同盟伺服器陣列中的第一部同盟伺服器使用 Active Directory Federation Service 組態精靈  
  
> [!NOTE]  
> 請確定您具有網域系統管理員權限或擁有可用的網域系統管理員認證，才能執行此程序。  
  
1.  在 [伺服器管理員] 的 [儀表板] 頁面上，按一下 [通知] 旗標，然後按一下 [設定伺服器上的 Federation Service]。  
  
    [Active Directory Federation Service 設定精靈]  隨即開啟。  
  
2.  在 [歡迎] 頁面上，選取 [在同盟伺服器陣列中建立第一部同盟伺服器]，然後按一下 [下一步]。  
  
3.  在上**連線到 AD DS**頁面上，指定 Active directory 使用網域系統管理員權限的帳戶\(AD\)這部電腦已加入的網域，然後再按一下**下一步**.  
  
4.  在 [指定服務內容]  頁面上執行下列動作，然後按一下 [下一步] ：  
  
    -   匯入.pfx 檔案，其中包含安全通訊端層\(SSL\)憑證和您稍早取得的金鑰。 在 [步驟 2:註冊適用於 AD FS 的 SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，您已取得此憑證，並將它複製到您想要設定為同盟伺服器的電腦上。 若要匯入.pfx 檔案，透過精靈，請按一下**匯入**，然後瀏覽至檔案的位置。 系統會提示您時，輸入.pfx 檔案的密碼。  
  
    -   提供您的 federation service 的名稱。 例如， **fs.contoso.com**。 此名稱必須符合其中一個主旨或憑證中的主旨替代名稱。  
  
    -   提供您的 federation service 顯示名稱。 例如， **Contoso Corporation**。 使用者會看到此名稱在 Active Directory Federation Services \(AD FS\)號\-頁面中。  
  
5.  在 **指定服務帳戶**頁面上，指定服務帳戶。 您可以建立或使用現有群組受控服務帳戶\(gMSA\)或使用現有的網域使用者帳戶。 如果您選取要建立新的 gMSA 帳戶的選項，指定新帳戶的名稱。 如果您選取使用現有的 gMSA 或網域帳戶的選項，請按一下**選取**選取帳戶。  
  
    > [!NOTE]  
    > 使用 gMSA 帳戶的優點是其自動\-交涉的密碼更新功能。  
  
    > [!WARNING]  
    > 如果您想要使用 gMSA 帳戶，您必須至少一個網域控制站執行 Windows Server 2012 作業系統的環境中。  
    >   
    > 如果 gMSA 選項已停用，而且您看到錯誤訊息，例如**群組受控服務帳戶不可以使用，因為未設定 KDS 根金鑰**，您也可以執行下列 Windows 網域中啟用 gMSA網域控制站，執行 Windows Server 2012 或更新版本上，您的 Active Directory 網域中的 PowerShell 命令： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 然後返回精靈中，按一下**上一步**，然後按一下**下一步**若要重新\-輸入**指定服務帳戶**頁面。 應該已啟用 gMSA 選項。 您可以選取它，並輸入您想要使用 gMSA 帳戶名稱。  
  
6.  在 [**指定設定資料庫**頁面上，指定 AD FS 設定資料庫，然後按一下**下一步]**。 您可以建立一個資料庫在此電腦上使用 Windows Internal Database \(WID\)，或者您可以指定的位置和 Microsoft SQL server 執行個體名稱。  
  
    如需詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
    > [!IMPORTANT]  
    > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
7.  在 [檢閱選項] 頁面上，檢查您的設定選項，然後按一下 [下一步]。  
  
8.  在  **Pre\-必要檢查**頁面上，確認所有先決條件檢查都順利完成，然後按一下**設定**。  
  
9. 在 **結果**頁面上，檢閱結果並檢查設定是否成功完成，然後按一下**完成 federation service 部署所需的後續步驟**。 如需詳細資訊，請參閱 <<c0> [ 後續步驟以完成 AD FS 安裝](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下 **[關閉]** 以結束精靈。  
  
### <a name="BKMK_3"></a>若要設定透過 Windows PowerShell 的新同盟伺服器陣列中的第一部同盟伺服器  
您可以使用新的或現有的 gMSA 帳戶或現有的網域使用者帳戶，以建立新的同盟伺服器陣列。  
  
-   **如果您想要使用新的 gMSA 帳戶建立新的同盟伺服器，執行下列作業：**  
  
    > [!IMPORTANT]  
    > 您必須建立新的同盟伺服器陣列中的第一部同盟伺服器的網域系統管理員權限。  
  
    1.  在您想要設定為同盟伺服器的電腦，請確定所需的 SSL 憑證已匯入至**本機電腦\\我的儲存體**目錄。 您可以驗證 SSL 憑證是否已匯入 Windows PowerShell 命令視窗中執行下列命令： `dir Cert:\LocalMachine\My`。 憑證依照在其憑證指紋**本機電腦\\我的儲存體**目錄。  
  
    2.  網域控制站，開啟 Windows PowerShell 命令視窗並執行下列命令來確認是否已在您的網域中建立 KDS 根金鑰： `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。 如果它尚未建立，因此輸出會顯示任何資訊，請執行下列命令來建立機碼： `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`。  
  
    3.  在您想要設定為同盟伺服器的電腦，開啟 Windows PowerShell 命令視窗中，並執行下列命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > `$`號結尾的前一個命令是必要的。  
  
        若要取得的值`<certificate_thumbprint>`，請執行`dir Cert:\LocalMachine\My`，然後選取 SSL 憑證的指紋。 值`<federation_service_name>`是您的同盟服務名稱，例如**fs.contoso.com**。  
  
        > [!NOTE]  
        > 如果這不是第一次執行此命令，新增`OverwriteConfiguration`參數。  
  
        > [!NOTE]  
        > 前一個命令會建立 WID 伺服器陣列。 如果您想要建立 SQL Server 伺服器陣列時，您必須已安裝且可運作的 SQL Server 的執行個體。  
        >   
        > 您可以使用下列命令來建立第一部同盟伺服器使用的 SQL Server 執行個體的新伺服陣列中：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`何處 **< SQL\_主機\_名稱 >** 是在 SQL server 名稱伺服器正在執行，並 **< SQL\_執行個體\_名稱 >** 是 SQL Server 執行個體的名稱。 如果您使用 SQL Server 的預設執行個體時，使用**SQLConnectionString**的值 」**資料來源\=< SQL\_主機\_名稱 >; Integrated Security\=，則為 True**".  
  
        > [!IMPORTANT]  
        > 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
-   **如果您想要使用現有的網域使用者帳戶建立新的同盟伺服器，執行下列作業：**  
  
    1.  在您想要設定為同盟伺服器的電腦，請確定所需的 SSL 憑證已匯入至**本機電腦\\我的儲存體**目錄。 您可以驗證 SSL 憑證是否已匯入 Windows PowerShell 命令視窗中執行下列命令： `dir Cert:\LocalMachine\My`。 憑證依照在其憑證指紋**本機電腦\\我的儲存體**目錄。  
  
    2.  在上您想要設定為同盟伺服器的電腦，開啟 Windows PowerShell 命令視窗，然後執行下列命令： `$fscred = Get-Credential`。 輸入您想要使用格式的網域中的同盟服務帳戶的網域使用者帳戶認證\\使用者名稱。  
  
    3.  在相同的 Windows PowerShell 命令視窗中，執行下列命令：  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        若要取得的值 **< 憑證\_指紋 >**，請執行`dir Cert:\LocalMachine\My`，然後選取 SSL 憑證的指紋。 值 **< 同盟\_服務\_名稱 >** 是同盟服務，例如 fs.contoso.com 的名稱。  
  
        > [!NOTE]  
        > 如果這不是第一次執行此命令，新增`OverwriteConfiguration`參數。  
  
        > [!NOTE]  
        > 前一個命令會建立 WID 伺服器陣列。 如果您想要建立 SQL Server 伺服器陣列，您必須已安裝且可運作的 SQL Server 執行個體。  
        >   
        > 您可以使用下列命令來建立第一部同盟伺服器使用的 SQL Server 執行個體的新伺服陣列中：`Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`何處**SQL\_主機\_名稱**是 SQL Server 所在的伺服器名稱執行，並**SQL\_執行個體\_名稱**是 SQL Server 執行個體的名稱。 如果您使用 SQL Server 的預設執行個體時，使用**SQLConnectionString**的值 」**資料來源\=< SQL\_主機\_名稱 >; Integrated Security\=，則為 True**".  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="BKMK_2"></a>將同盟伺服器新增至現有的同盟伺服器陣列  
  
> [!IMPORTANT]  
> 請確定您已完成[步驟 3:安裝 AD FS 角色服務](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)，然後才能在您啟動任何程序這一節。  
  
> [!IMPORTANT]  
> 請確定您已取得有效的 SSL 伺服器驗證憑證才能完成此程序。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>若要將同盟伺服器新增至現有的同盟伺服器陣列透過 Active Directory Federation Service 組態精靈  
  
1.  在 [伺服器管理員] 的 [儀表板] 頁面上，按一下 [通知] 旗標，然後按一下 [設定伺服器上的 Federation Service]。  
  
    [Active Directory Federation Service 設定精靈]  隨即開啟。  
  
2.  在上**歡迎**頁面上，選取**新增至同盟伺服器陣列的同盟伺服器**，然後按一下**下一步**。  
  
3.  在 [**連線到 AD DS**頁面上，指定帳戶使用網域系統管理員權限適用於 AD 這台電腦已加入的網域，然後按一下**下一步]**。  
  
4.  在 **指定的伺服器陣列**頁面上，提供使用 WID 伺服器陣列中的主要同盟伺服器的名稱，或指定的資料庫主機名稱和現有的同盟伺服器陣列，並使用 SQL Server 資料庫執行個體名稱。  
  
    > [!WARNING]  
    > 在 Windows Server® 2012 R2 中，沒有因應措施若要指定 SQL Server 的預設執行個體。 因應措施是不使用使用者介面。 相反地，使用中的步驟[若要設定透過 Windows PowerShell 的新同盟伺服器陣列中的第一部同盟伺服器](Configure-a-Federation-Server.md#BKMK_3)。  
  
    > [!IMPORTANT]  
    > 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
5.  在 **指定 SSL 憑證**頁面上，匯入.pfx 檔案，其中包含 SSL 憑證與您先前取得的金鑰。 此憑證是必要的服務驗證憑證。 在 [步驟 2:註冊適用於 AD FS 的 SSL 憑證](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md)，您已取得此憑證，並將它複製到您想要設定為同盟伺服器的電腦。 若要匯入.pfx 檔案，透過精靈，請按一下**匯入**瀏覽至檔案的位置。 系統會提示您時，輸入.pfx 檔案的密碼。  
  
6.  在 **指定服務帳戶**頁面上，指定您建立伺服器陣列中的第一部同盟伺服器時所設定的相同服務帳戶。 您可以使用現有群組受控服務帳戶或現有的網域使用者帳戶。  
  
    > [!IMPORTANT]  
    > 您所指定的帳戶必須與此伺服器陣列中主要同盟伺服器使用的帳戶相同的帳戶。  
  
7.  在 [檢閱選項] 頁面上，檢查您的設定選項，然後按一下 [下一步]。  
  
8.  在  **Pre\-必要檢查**頁面上，確認所有先決條件檢查都順利完成，然後按一下**設定**。  
  
9. 在 **結果**頁面上，檢閱結果並檢查設定是否成功完成，然後按一下**完成 federation service 部署所需的後續步驟**。 如需詳細資訊，請參閱 <<c0> [ 後續步驟以完成 AD FS 安裝](https://go.microsoft.com/fwlink/p/?LinkId=286704)。 按一下 **[關閉]** 以結束精靈。  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>若要將同盟伺服器新增至現有的同盟伺服器陣列透過 Windows PowerShell  
您可以新增至現有的伺服陣列的同盟伺服器，使用現有的 gMSA 帳戶或現有的網域使用者帳戶。  
  
-   如果您想要將同盟伺服器加入至伺服器陣列，使用現有的 gMSA 帳戶，執行下列作業：  
  
    1.  在您想要設定為同盟伺服器的電腦，請確定所需的 SSL 憑證已匯入至**本機電腦\\我的儲存體**目錄。 您可以驗證 SSL 憑證是否已匯入 Windows PowerShell 命令視窗中執行下列命令： `dir Cert:\LocalMachine\My`。 憑證依照在其憑證指紋**本機電腦\\我的儲存體**目錄。  
  
    2.  在您想要設定為同盟伺服器的電腦，開啟 Windows PowerShell 命令視窗中，並執行下列命令。  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` 您的 AD 網域和該網域中 gMSA 帳戶的名稱。 `<first_federation_server_hostname>` 為這個現有的伺服陣列中的主要同盟伺服器的主機名稱。  
  
        您可以取得的值`<certificate_thumbprint>`藉由執行`dir Cert:\LocalMachine\My`上一個步驟。  
  
        > [!NOTE]  
        > 如果這不是第一次執行此命令，新增`OverwriteConfiguration`參數。  
  
        > [!NOTE]  
        > 前一個命令會建立 WID 伺服器陣列節點。 如果您想要建立伺服器陣列節點的執行 SQL Server 的電腦，您必須已安裝且可運作的 SQL Server 執行個體。  
        >   
        > 您可以使用下列命令，將同盟伺服器新增至現有的伺服器陣列所使用的 SQL Server 執行個體：`Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`何處**SQL\_主機\_名稱**是 SQL Server 所在的伺服器名稱執行，並**SQL\_執行個體\_名稱**是 SQL Server 執行個體的名稱。 如果您使用 SQL Server 的預設執行個體時，使用**SQLConnectionString**的值 」**資料來源\=< SQL\_主機\_名稱 >; Integrated Security\=，則為 True**".  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
-   如果您想要將同盟伺服器加入至伺服器陣列，使用現有的網域使用者帳戶，執行下列作業：  
  
    1.  在您想要設定為同盟伺服器的電腦，開啟 Windows PowerShellcommand 視窗，然後再執行下列命令： `$fscred = get-credential`。 輸入您想要使用格式的網域中的同盟服務帳戶的網域使用者帳戶認證\\使用者名稱。  
  
    2.  在您想要設定為同盟伺服器的電腦，請確定所需的 SSL 憑證已匯入至**本機電腦\\我的儲存體**目錄。 您可以驗證 SSL 憑證是否已匯入 Windows PowerShellcommand 視窗中執行下列命令： `dir Cert:\LocalMachine\My`。 憑證依照在其憑證指紋**本機電腦\\我的儲存體**目錄。  
  
    3.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > 如果這不是第一次執行此命令，新增`OverwriteConfiguration`參數。  
  
        > [!NOTE]  
        > 前一個命令會建立 WID 伺服器陣列節點。 如果您想要建立伺服器陣列節點的執行 SQL Server 的電腦，您必須已安裝且可運作的 SQL Server 執行個體。 您可以使用下列命令來新增至現有的伺服陣列的同盟伺服器所使用的 SQL Server 執行個體：`Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`何處**SQL\_主機\_名稱**所在的伺服器名稱的 SQL 執行個體伺服器正在執行，並**SQL\_執行個體\_名稱**是 SQL Server 執行個體的名稱。 如果您使用 SQL Server 的預設執行個體時，使用**SQLConnectionString**的值 」**資料來源\=< SQL\_主機\_名稱 >; Integrated Security\=，則為 True**".  
  
        > [!IMPORTANT]  
        > 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

