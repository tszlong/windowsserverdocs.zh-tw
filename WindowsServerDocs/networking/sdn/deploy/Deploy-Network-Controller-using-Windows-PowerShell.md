---
title: 使用 Windows PowerShell 部署網路控制卡
description: 本主題提供在執行 Windows Server 2016 的一部或多部電腦或虛擬機器（Vm）上，使用 Windows PowerShell 部署網路控制站的指示。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: ee3aa93c02419667b05a987f548ef4d14285231d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313076"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>使用 Windows PowerShell 部署網路控制卡

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題提供在執行 Windows Server 2016 的一或多部虛擬機器（Vm）上，使用 Windows PowerShell 部署網路控制站的指示。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制卡伺服器角色。 若要部署網路控制站，您必須在 hyper-v 虛擬機器上安裝網路控制站伺服器角色，\(安裝在 Hyper-v 主機上的 VM\)。 在三部不同的\-Hyper-v 主機上的 Vm 上安裝網路控制站之後，您必須使用 Windows PowerShell 命令**NetworkControllerServer**，將主機新增至網路控制站，以啟用軟體定義網路的超\-主機 \(SDN\)。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。 如需詳細資訊，請參閱[NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主題包含下列各節。

- [安裝網路控制卡伺服器角色](#install-the-network-controller-server-role)

- [設定網路控制站叢集](#configure-the-network-controller-cluster)

- [設定網路控制站應用程式](#configure-the-network-controller-application)

- [網路控制站部署驗證](#network-controller-deployment-validation)

- [適用于網路控制卡的其他 Windows PowerShell 命令](#additional-windows-powershell-commands-for-network-controller)

- [範例網路控制站設定腳本](#sample-network-controller-configuration-script)

- [非 Kerberos 部署的部署後步驟](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>安裝網路控制卡伺服器角色

您可以使用此程式，在虛擬機器上安裝網路控制卡伺服器角色 \(VM\)。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制卡伺服器角色。 若要部署網路控制站，您必須在 hyper-v 虛擬機器上安裝網路控制站伺服器角色，\(安裝在 Hyper-v 主機上的 VM\)。 在三部不同的\-Hyper-v 主機上的 Vm 上安裝網路控制站之後，您必須將主機新增至網路控制站，以啟用軟體定義網路的超\-主機 \(SDN\)。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  

>[!NOTE]
>如果您想要使用伺服器管理員而非 Windows PowerShell 來安裝網路控制卡，請參閱[使用伺服器管理員安裝網路控制卡伺服器角色](https://technet.microsoft.com/library/mt403348.aspx)

若要使用 Windows PowerShell 安裝網路控制卡，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

您必須重新開機電腦，才能安裝網路控制站。 若要這麼做，請輸入下列命令，然後按 ENTER。

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>設定網路控制站叢集

網路控制站叢集可為網路控制站應用程式提供高可用性和擴充性，您可以在建立叢集之後設定此叢集，並將其裝載在叢集上。

>[!NOTE]
>您可以直接在安裝網路控制站的 VM 上執行下列各節中的程式，也可以使用 Windows Server 2016 的遠端伺服器管理工具，從執行的遠端電腦執行程式Windows Server 2016 或 Windows 10。 此外，若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。 如果您安裝網路控制卡的電腦或 VM 已加入網域，則您的使用者帳戶必須是**網域使用者**的成員。

您可以藉由建立節點物件，然後設定叢集，來建立網路控制站叢集。

### <a name="create-a-node-object"></a>建立 node 物件

您必須為網路控制站叢集成員的每個 VM 建立節點物件。

若要建立 node 物件，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER 鍵。 請確定您為每個適用于您的部署的參數新增值。  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表提供**NetworkControllerNodeObject**命令之每個參數的描述。

|參數|描述|
|-------------|---------------|
|名稱|**Name**參數會指定您想要新增至叢集之伺服器的易記名稱|
|Server|**伺服器**參數指定您想要新增至叢集之伺服器的主機名稱、完整功能變數名稱（FQDN）或 IP 位址。 若為已加入網域的電腦，則需要 FQDN。|
|FaultDomain|**FaultDomain**參數會指定您要新增至叢集之伺服器的失敗網域。 此參數會定義與您要新增至叢集的伺服器同時發生失敗的伺服器。 這項失敗可能是因為共用實體相依性（例如電源和網路來源）所致。 容錯網域通常代表與這些共用相依性相關的階層，而且有更多伺服器可能會與容錯網域樹狀結構中較高的點一起失敗。 在執行時間期間，網路控制卡會考慮叢集中的容錯網域，並嘗試散佈網路控制站服務，使其位於不同的容錯網域中。 此程式有助於確保任何一個容錯網域失敗時，該服務的可用性及其狀態不會受到危害。 容錯網域是以階層格式來指定。 例如： "Fd：/DC1/Rack1/Host1"，其中 DC1 是資料中心名稱，Rack1 是機架名稱，Host1 是放置節點所在的主機名稱。|
|RestInterface|**RestInterface**參數會在具像狀態傳輸（REST）通訊終止的節點上指定介面的名稱。 此網路控制站介面會從網路的管理層接收 Northbound 的 API 要求。|
|NodeCertificate|**NodeCertificate**參數會指定網路控制站用來進行電腦驗證的憑證。 如果您針對叢集中的通訊使用以憑證為基礎的驗證，則需要憑證;憑證也會用於網路控制站服務之間的流量加密。 憑證主體名稱必須與節點的 DNS 名稱相同。|

### <a name="configure-the-cluster"></a>設定叢集

若要設定叢集，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER。 請確定您為每個適用于您的部署的參數新增值。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表提供**NetworkControllerCluster**命令之每個參數的描述。
  
|參數|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication**參數會指定用來保護節點間通訊的驗證類型，也可用來加密網路控制站服務之間的流量。 支援的值為**Kerberos**、 **X509**和**None**。 Kerberos 驗證會使用網域帳戶，而且只有在網路控制卡節點已加入網域時才能使用。 如果您指定以 X509 為基礎的驗證，則必須在 NetworkControllerNode 物件中提供憑證。 此外，您必須先手動布建憑證，才能執行此命令。|
|ManagementSecurityGroup|**ManagementSecurityGroup**參數會指定安全性群組的名稱，其中包含允許從遠端電腦執行管理 Cmdlet 的使用者。 只有在 ClusterAuthentication 為 Kerberos 時，才適用這種情況。 您必須指定網域安全性群組，而不是本機電腦上的安全性群組。|
|節點|**Node**參數會指定您使用**NetworkControllerNodeObject**命令建立的網路控制卡節點清單。|
|DiagnosticLogLocation|**DiagnosticLogLocation**參數會指定要定期上傳診斷記錄的共用位置。 如果您未指定這個參數的值，記錄檔會儲存在本機的每個節點上。 記錄檔會儲存在本機資料夾%systemdrive%\Windows\tracing\SDNDiagnostics。 叢集記錄檔會儲存在本機的資料夾%systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces。|
|LogLocationCredential|**LogLocationCredential**參數會指定存取記錄儲存所在之共用位置所需的認證。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate**參數會指定網路控制站用來加密用來存取網路控制卡二進位檔和**LogLocationCredential**之認證的憑證（如果有指定的話）。 在您執行此命令之前，必須先在所有網路控制站節點上布建憑證，而且必須在所有叢集節點上註冊相同的憑證。 建議在生產環境中使用此參數來保護網路控制卡二進位檔和記錄檔。 若沒有此參數，認證會以純文字儲存，並可由任何未經授權的使用者誤用。|
|Credential|只有當您從遠端電腦執行此命令時，才需要此參數。 **Credential**參數指定具有在目的電腦上執行此命令之許可權的使用者帳戶。|
|CertificateThumbprint|只有當您從遠端電腦執行此命令時，才需要此參數。 **CertificateThumbprint**參數會指定具有在目的電腦上執行此命令之許可權的使用者帳戶的數位公開金鑰憑證（X509）。|
|UseSSL|只有當您從遠端電腦執行此命令時，才需要此參數。 **UseSSL**參數會指定用來建立與遠端電腦之連接的安全通訊端層（SSL）通訊協定。 預設不會使用 SSL。|
|ComputerName|**ComputerName**參數會指定用來執行此命令的網路控制站節點。 如果您未指定這個參數的值，預設會使用本機電腦。|
|LogSizeLimitInMBs|此參數會指定網路控制站可以儲存的最大記錄檔大小（以 MB 為單位）。 記錄會以迴圈方式儲存。 如果提供 DiagnosticLogLocation，此參數的預設值為 40 GB。 如果未提供 DiagnosticLogLocation，記錄會儲存在網路控制卡節點上，而此參數的預設值為 15 GB。|
|LogTimeLimitInDays|這個參數會指定儲存記錄的持續時間限制（以天為單位）。 記錄會以迴圈方式儲存。 此參數的預設值為3天。|

## <a name="configure-the-network-controller-application"></a>設定網路控制站應用程式
若要設定網路控制站應用程式，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER。 請確定您為每個適用于您的部署的參數新增值。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表提供**NetworkController**命令之每個參數的描述。

|參數|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication**參數會指定用來保護 REST 和網路控制站之間通訊的驗證類型。 支援的值為**Kerberos**、 **X509**和**None**。 Kerberos 驗證會使用網域帳戶，而且只有在網路控制卡節點已加入網域時才能使用。 如果您指定以 X509 為基礎的驗證，則必須在 NetworkControllerNode 物件中提供憑證。 此外，您必須先手動布建憑證，才能執行此命令。|
|節點|**Node**參數會指定您使用**NetworkControllerNodeObject**命令建立的網路控制卡節點清單。|
|ClientCertificateThumbprint|只有當您針對網路控制卡用戶端使用以憑證為基礎的驗證時，才需要此參數。 **ClientCertificateThumbprint**參數會指定向 Northbound 層上的用戶端註冊之憑證的指紋。|
|ServerCertificate|**ServerCertificate**參數會指定網路控制站用來向用戶端證明其身分識別的憑證。 伺服器憑證必須在增強金鑰使用方式延伸中包含伺服器驗證目的，而且必須由用戶端信任的 CA 發行至網路控制卡。|
|RESTIPAddress|您不需要使用網路控制站的單一節點部署來指定**RESTIPAddress**的值。 針對多節點部署， **RESTIPAddress**參數會以 CIDR 標記法指定 REST 端點的 IP 位址。 例如，192.168.1.10/24。 **ServerCertificate**的 [主體名稱] 值必須解析為**RESTIPAddress**參數的值。 當所有節點都位於相同的子網時，必須為所有多節點網路控制站部署指定此參數。 如果節點位於不同的子網，您必須使用**RestName**參數，而不是使用**RESTIPAddress**。|
|RestName|您不需要使用網路控制站的單一節點部署來指定**RestName**的值。 只有當多個節點的部署具有不同子網的節點時，您才必須指定**RestName**的值。 針對多節點部署， **RestName**參數會指定網路控制卡叢集的 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup**參數會指定成員為網路控制站用戶端的 Active Directory 安全性群組的名稱。 只有當您針對**ClientAuthentication**使用 Kerberos 驗證時，才需要此參數。 安全性群組必須包含用來存取 REST Api 的帳戶，而且您必須先建立安全性群組並新增成員，才能執行此命令。|
|Credential|只有當您從遠端電腦執行此命令時，才需要此參數。 **Credential**參數指定具有在目的電腦上執行此命令之許可權的使用者帳戶。|
|CertificateThumbprint|只有當您從遠端電腦執行此命令時，才需要此參數。 **CertificateThumbprint**參數會指定具有在目的電腦上執行此命令之許可權的使用者帳戶的數位公開金鑰憑證（X509）。|
|UseSSL|只有當您從遠端電腦執行此命令時，才需要此參數。 **UseSSL**參數會指定用來建立與遠端電腦之連接的安全通訊端層（SSL）通訊協定。 預設不會使用 SSL。|

完成網路控制站應用程式的設定之後，您的網路控制站部署就已完成。

## <a name="network-controller-deployment-validation"></a>網路控制站部署驗證

若要驗證您的網路控制站部署，您可以將認證新增至網路控制卡，然後取得認證。

如果您使用 Kerberos 做為 ClientAuthentication 機制，則您所建立之**ClientSecurityGroup**中的成員資格是執行此程式的最低需求。

**步**

1.  在用戶端電腦上，如果您使用 Kerberos 做為 ClientAuthentication 機制，請使用屬於您**ClientSecurityGroup**成員的使用者帳戶登入。

2. 開啟 [Windows PowerShell]，輸入下列命令以將認證新增至網路控制卡，然後按 ENTER。 請確定您為每個適用于您的部署的參數新增值。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 若要取出您新增至網路控制卡的認證，請輸入下列命令，然後按 ENTER。 請確定您為每個適用于您的部署的參數新增值。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. 檢查命令輸出，其應該類似下列範例輸出。

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > 當您執行**NetworkControllerCredential**命令時，可以使用點運算子來列出認證的屬性，以將命令的輸出指派給變數。 例如，$cred。屬性.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>適用于網路控制卡的其他 Windows PowerShell 命令

部署網路控制站之後，您可以使用 Windows PowerShell 命令來管理和修改您的部署。 以下是您可以對部署進行的一些變更。

- 修改網路控制卡節點、叢集和應用程式設定

- 移除網路控制卡叢集和應用程式

- 管理網路控制站叢集節點，包括新增、移除、啟用和停用節點。

下表提供您可以用來完成這些工作的 Windows PowerShell 命令語法。

|工作|命令|語法|
|--------|-------|----------|
|修改網路控制站叢集設定|設定-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制卡應用程式設定|設定-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站節點設定|設定-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站診斷設定|設定-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制卡應用程式|卸載-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制卡叢集|卸載-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|將節點新增至網路控制站叢集|新增-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|停用網路控制站叢集節點|停用-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|啟用網路控制卡叢集節點|啟用-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|從叢集中移除網路控制卡節點|移除-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>網路控制卡的 Windows PowerShell 命令位於[網路控制卡 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)的 TechNet Library 中。

## <a name="sample-network-controller-configuration-script"></a>範例網路控制站設定腳本

下列範例設定腳本顯示如何建立多節點網路控制站叢集，並安裝網路控制卡應用程式。 此外，$cert 變數會從 [本機電腦] 憑證存放區選取符合主體名稱字串 "networkController.contoso.com" 的憑證。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>非 Kerberos 部署的部署後步驟

如果您的網路控制站部署未使用 Kerberos，就必須部署憑證。

如需詳細資訊，請參閱[網路控制卡的部署後步驟](../technologies/network-controller/post-deploy-steps-nc.md)。


