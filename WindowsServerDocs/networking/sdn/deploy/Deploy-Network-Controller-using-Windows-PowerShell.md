---
title: 使用 Windows PowerShell 部署網路控制卡
description: 本主題提供的指示說明如何使用 Windows PowerShell 在一或多部電腦上部署網路控制站，或在執行 Windows Server 2019 或 2016 (Vm) 的虛擬機器上部署網路控制站。
ms.topic: how-to
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: anpaul
author: AnirbanPaul
manager: grcusanz
ms.date: 08/23/2018
ms.openlocfilehash: 3e7e020dfa5567608c53d41c4478a54ba89fb720
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716224"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>使用 Windows PowerShell 部署網路控制卡

>適用於：Windows Server 2019、Windows Server 2016

本主題提供有關使用 Windows PowerShell 在一或多部虛擬機器上部署網路控制站的指示， (Vm) 執行 Windows Server 2019 或2016。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制站伺服器角色。 若要部署網路控制站，您必須在 \( \) 安裝于 hyper-v 主機上的 hyper-v 虛擬機器 VM 上安裝網路控制站伺服器角色。 在三部不同的 Hyper-v 主機上的 Vm 上安裝網路控制站之後 \- ，您必須 \- \( \) 使用 Windows PowerShell 命令 **NetworkControllerServer**，將主機新增至網路控制站，以啟用軟體定義網路 SDN 的 hyper-v 主機。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。 如需詳細資訊，請參閱 [NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主題包含下列各節。

- [安裝網路控制站伺服器角色](#install-the-network-controller-server-role)

- [設定網路控制卡叢集](#configure-the-network-controller-cluster)

- [設定網路控制站應用程式](#configure-the-network-controller-application)

- [網路控制站部署驗證](#network-controller-deployment-validation)

- [網路控制站的其他 Windows PowerShell 命令](#additional-windows-powershell-commands-for-network-controller)

- [範例網路控制站設定腳本](#sample-network-controller-configuration-script)

- [非 Kerberos 部署的部署後步驟](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>安裝網路控制站伺服器角色

您可以使用此程式在虛擬機器 VM 上安裝網路控制站伺服器角色 \( \) 。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制站伺服器角色。 若要部署網路控制站，您必須在 \( \) 安裝于 hyper-v 主機上的 hyper-v 虛擬機器 VM 上安裝網路控制站伺服器角色。 在三部不同的 Hyper-v 主機上的 Vm 上安裝網路控制站之後 \- ，您必須 \- \( \) 將主機新增至網路控制站，以啟用軟體定義網路 SDN 的 hyper-v 主機。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

>[!NOTE]
>如果您想要使用伺服器管理員而不是 Windows PowerShell 安裝網路控制站，請參閱[使用伺服器管理員安裝網路控制站伺服器角色](../technologies/network-controller/install-the-network-controller-server-role-using-server-manager.md)。

若要使用 Windows PowerShell 安裝網路控制站，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER 鍵。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

您必須重新開機電腦，才能安裝網路控制卡。 若要這樣做，請輸入下列命令，然後按 ENTER。

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>設定網路控制卡叢集

網路控制站叢集可為網路控制站應用程式提供高可用性和擴充性，您可以在建立叢集之後設定該應用程式，並將其裝載在叢集之上。

>[!NOTE]
>您可以直接在已安裝網路控制卡的 VM 上執行下列各節中的程式，也可以使用 Windows Server 2016 的遠端伺服器管理工具，從執行 Windows Server 2016 或 Windows 10 的遠端電腦執行程式。 此外，若要執行此程式，至少需要 **Administrators** 的成員資格或同等許可權。 如果您安裝網路控制卡的電腦或 VM 已加入網域，您的使用者帳戶必須是 **網域使用者** 的成員。

您可以建立節點物件，然後設定叢集，藉以建立網路控制站叢集。

### <a name="create-a-node-object"></a>建立 node 物件

您必須為每個屬於網路控制卡叢集成員的 VM 建立節點物件。

若要建立 node 物件，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER 鍵。 確定您為部署所適用的每個參數新增值。

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表提供 **NetworkControllerNodeObject** 命令的每個參數的描述。

|參數|描述|
|-------------|---------------|
|Name|**Name** 參數指定您想要新增至叢集之伺服器的易記名稱|
|伺服器|**Server** 參數指定您要新增至叢集之伺服器的主機名稱、完整功能變數名稱 (FQDN) 或 IP 位址。 若為已加入網域的電腦，則需要 FQDN。|
|FaultDomain|**FaultDomain** 參數會指定您要新增至叢集之伺服器的失敗網域。 此參數會定義在您要新增至叢集的伺服器時，可能會遇到失敗的伺服器。 這項失敗的原因可能是共用實體相依性，例如電源和網路來源。 容錯網域通常代表與這些共用相依性相關的階層，而更多伺服器可能會在容錯網域樹狀結構中的較高點一起故障。 在執行時間期間，網路控制站會考慮叢集中的容錯網域，並嘗試將網路控制站服務分散在不同的容錯網域中。 在任何一個容錯網域發生錯誤時，此程序有助確保該服務和其狀態的可用性沒有遭到破壞。 容錯網域會以階層格式來指定。 例如： "Fd：/DC1/Rack1/Host1"，其中 DC1 是資料中心名稱，Rack1 是機架名稱，而 Host1 是放置節點的主機名稱。|
|RestInterface|**RestInterface** 參數會指定節點上的介面名稱，具象狀態傳輸 (REST) 通訊終止。 此網路控制站介面會從網路的管理層接收 Northbound API 要求。|
|NodeCertificate|**NodeCertificate** 參數會指定網路控制站用來進行電腦驗證的憑證。 如果您使用以憑證為基礎的驗證來進行叢集內的通訊，就需要憑證;憑證也用來加密網路控制站服務之間的流量。 憑證的主體名稱必須與節點的 DNS 名稱相同。|

### <a name="configure-the-cluster"></a>設定叢集

若要設定叢集，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER 鍵。 確定您為部署所適用的每個參數新增值。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表提供 **NetworkControllerCluster**  命令的每個參數的描述。

|參數|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication** 參數會指定用來保護節點間通訊安全的驗證類型，也會用於網路控制站服務之間的流量加密。 支援的值為 **Kerberos**、 **X509** 及 **None**。 Kerberos 驗證會使用網域帳戶，而且只有在網路控制站節點已加入網域時才能使用。 如果您指定 X509 型驗證，則必須在 NetworkControllerNode 物件中提供憑證。 此外，您必須在執行此命令之前，手動布建憑證。|
|ManagementSecurityGroup|**ManagementSecurityGroup** 參數會指定安全性群組的名稱，此安全性群組包含允許從遠端電腦執行管理 Cmdlet 的使用者。 只有在 ClusterAuthentication 為 Kerberos 時才適用。 您必須指定網域安全性群組，而不是本機電腦上的安全性群組。|
|節點|**Node** 參數指定您使用 **NetworkControllerNodeObject** 命令建立的網路控制卡節點清單。|
|DiagnosticLogLocation|**DiagnosticLogLocation** 參數會指定定期上傳診斷記錄的共用位置。 如果您未指定此參數的值，記錄檔會儲存在本機的每個節點上。 記錄會儲存在本機資料夾%systemdrive%\Windows\tracing\SDNDiagnostics。 叢集記錄會儲存在本機的%systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces. 資料夾中。|
|LogLocationCredential|**LogLocationCredential** 參數會指定存取儲存記錄檔之共用位置所需的認證。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate** 參數會指定網路控制站用來加密用來存取網路控制站二進位檔和 **LogLocationCredential** 的認證（如有指定）的憑證。 您必須在所有網路控制卡節點上布建憑證，才能執行此命令，而且必須在所有叢集節點上註冊相同的憑證。 建議在生產環境中使用此參數來保護網路控制站二進位檔和記錄檔。 如果沒有這個參數，認證會以純文字儲存，並可供任何未經授權的使用者誤用。|
|認證|只有當您從遠端電腦執行此命令時，才需要此參數。 **Credential** 參數指定具有在目的電腦上執行此命令之許可權的使用者帳戶。|
|CertificateThumbprint|只有當您從遠端電腦執行此命令時，才需要此參數。 **CertificateThumbprint** 參數會指定具有在目的電腦上執行此命令之許可權的使用者帳戶的數位公開金鑰憑證 (X509) 。|
|UseSSL|只有當您從遠端電腦執行此命令時，才需要此參數。 **UseSSL** 參數會指定用來建立遠端電腦連線的安全通訊端層 (SSL) 通訊協定。 預設不會使用 SSL。|
|ComputerName|**ComputerName** 參數會指定執行此命令的網路控制器節點。 如果您未指定此參數的值，預設會使用本機電腦。|
|LogSizeLimitInMBs|此參數指定網路控制站可以儲存的最大記錄檔大小（以 MB 為單位）。 記錄會以迴圈方式儲存。 如果提供 DiagnosticLogLocation，此參數的預設值為 40 GB。 如果未提供 DiagnosticLogLocation，則記錄會儲存在網路控制站節點上，而此參數的預設值為 15 GB。|
|LogTimeLimitInDays|此參數會指定儲存記錄的持續時間限制（以天為單位）。 記錄會以迴圈方式儲存。 此參數的預設值為3天。|

## <a name="configure-the-network-controller-application"></a>設定網路控制站應用程式
若要設定網路控制站應用程式，請在 Windows PowerShell 命令提示字元中輸入下列命令，然後按 ENTER 鍵。 確定您為部署所適用的每個參數新增值。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表提供 **NetworkController** 命令的每個參數的描述。

|參數|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication** 參數會指定用來保護 REST 和網路控制站之間通訊的驗證類型。 支援的值為 **Kerberos**、 **X509** 及 **None**。 Kerberos 驗證會使用網域帳戶，而且只有在網路控制站節點已加入網域時才能使用。 如果您指定 X509 型驗證，則必須在 NetworkControllerNode 物件中提供憑證。 此外，您必須在執行此命令之前，手動布建憑證。|
|節點|**Node** 參數指定您使用 **NetworkControllerNodeObject** 命令建立的網路控制卡節點清單。|
|ClientCertificateThumbprint|只有當您使用網路控制站用戶端的憑證型驗證時，才需要此參數。 **ClientCertificateThumbprint** 參數會指定在 Northbound 層上註冊至用戶端之憑證的指紋。|
|ServerCertificate|**ServerCertificate** 參數會指定網路控制站用來向用戶端證明其身分識別的憑證。 伺服器憑證必須在增強金鑰使用方法延伸中包含伺服器驗證目的，且必須由用戶端信任的 CA 發行至網路控制卡。|
|RESTIPAddress|您不需要使用網路控制站的單一節點部署來指定 **RESTIPAddress** 的值。 針對多節點部署， **RESTIPAddress** 參數會以 CIDR 標記法指定 REST 端點的 IP 位址。 例如，192.168.1.10/24。 **ServerCertificate** 的主體名稱值必須解析為 **RESTIPAddress** 參數的值。 當所有節點都位於相同的子網上時，必須為所有多重節點的網路控制站部署指定此參數。 如果節點位於不同的子網，您必須使用 **RestName** 參數，而不是使用 **RESTIPAddress**。|
|RestName|您不需要使用網路控制站的單一節點部署來指定 **RestName** 的值。 只有當多節點部署的節點位於不同的子網上時，才必須指定 **RestName** 的值。 若為多節點部署， **RestName** 參數會指定網路控制站叢集的 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup** 參數會指定其成員為網路控制站用戶端的 Active Directory 安全性群組的名稱。 只有當您針對 **ClientAuthentication** 使用 Kerberos 驗證時，才需要此參數。 安全性群組必須包含用來存取 REST Api 的帳戶，而您必須先建立安全性群組並新增成員，才能執行此命令。|
|認證|只有當您從遠端電腦執行此命令時，才需要此參數。 **Credential** 參數指定具有在目的電腦上執行此命令之許可權的使用者帳戶。|
|CertificateThumbprint|只有當您從遠端電腦執行此命令時，才需要此參數。 **CertificateThumbprint** 參數會指定具有在目的電腦上執行此命令之許可權的使用者帳戶的數位公開金鑰憑證 (X509) 。|
|UseSSL|只有當您從遠端電腦執行此命令時，才需要此參數。 **UseSSL** 參數會指定用來建立遠端電腦連線的安全通訊端層 (SSL) 通訊協定。 預設不會使用 SSL。|

完成網路控制站應用程式的設定之後，您的網路控制站部署就會完成。

## <a name="network-controller-deployment-validation"></a>網路控制站部署驗證

若要驗證您的網路控制站部署，您可以將認證新增至網路控制站，然後取得認證。

如果您使用 Kerberos 作為 ClientAuthentication 機制，則您所建立之 **ClientSecurityGroup** 的成員資格是執行此程式的最低需求。

**程式：**

1.  在用戶端電腦上，如果您使用 Kerberos 作為 ClientAuthentication 機制，請使用屬於 **ClientSecurityGroup** 成員的使用者帳戶登入。

2. 開啟 Windows PowerShell，輸入下列命令以將認證新增至網路控制站，然後按 ENTER。 確定您為部署所適用的每個參數新增值。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 若要抓取您新增到網路控制站的認證，請輸入下列命令，然後按 ENTER。 確定您為部署所適用的每個參數新增值。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1
    ```

4. 檢查命令輸出，其應類似下列範例輸出。

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
    > 當您執行 **NetworkControllerCredential** 命令時，您可以使用點運算子列出認證的屬性，將命令的輸出指派給變數。 例如，$cred。性能。

## <a name="additional-windows-powershell-commands-for-network-controller"></a>網路控制站的其他 Windows PowerShell 命令

部署網路控制站之後，您可以使用 Windows PowerShell 命令來管理和修改您的部署。 以下是您可以對部署進行的一些變更。

- 修改網路控制站節點、叢集和應用程式設定

- 移除網路控制卡叢集和應用程式

- 管理網路控制站叢集節點，包括新增、移除、啟用和停用節點。

下表提供您可用來完成這些工作之 Windows PowerShell 命令的語法。

|Task|Command|Syntax|
|--------|-------|----------|
|修改網路控制站叢集設定|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站應用程式設定|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站節點設定|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站診斷設定|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制站應用程式|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制卡叢集|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|將節點新增至網路控制站叢集|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|停用網路控制站叢集節點|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|啟用網路控制站叢集節點|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|從叢集中移除網路控制站節點|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>網路控制站的 Windows PowerShell 命令位於 TechNet Library 的 [Network Controller Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)中。

## <a name="sample-network-controller-configuration-script"></a>範例網路控制站設定腳本

下列範例設定腳本示範如何建立多節點網路控制站叢集，以及如何安裝網路控制站應用程式。 此外，$cert 變數會從 [本機電腦] 憑證存放區選取符合主體名稱字串 "networkController.contoso.com" 的憑證。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>非 Kerberos 部署的部署後步驟

如果您的網路控制站部署未使用 Kerberos，則必須部署憑證。

如需詳細資訊，請參閱 [網路控制站的部署後步驟](../technologies/network-controller/post-deploy-steps-nc.md)。