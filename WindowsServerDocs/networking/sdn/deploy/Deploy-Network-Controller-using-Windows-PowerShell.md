---
title: 使用 Windows PowerShell 部署網路控制卡
description: 本主題提供有關使用 Windows PowerShell 在一個或多個電腦或執行 Windows Server 2016 的虛擬機器 (Vm) 上部署網路控制站的指示。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: d671d044896ae9e71edad8302f06f2a21fe50772
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034550"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>使用 Windows PowerShell 部署網路控制卡

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供有關使用 Windows PowerShell 在一個或多個虛擬機器 (Vm) 執行 Windows Server 2016 上部署網路控制站的指示。

>[!IMPORTANT]
>不會部署在實體主機上的網路控制卡伺服器角色。 若要部署網路控制站，您必須為 HYPER-V 虛擬機器上安裝網路控制卡伺服器角色\(VM\)安裝於 HYPER-V 主機。 在三個不同的 Hyper-v Vm 上安裝網路控制站之後\-Hyperv 主機，您必須啟用 Hyper-v\-的軟體定義網路的 HYPER-V 主機\(SDN\)加上要使用網路控制站的主機Windows PowerShell 命令**新增 NetworkControllerServer**。 如此一來，您將使 SDN 軟體負載平衡器，函式。 如需詳細資訊，請參閱 <<c0> [ 新增 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主題涵蓋下列各節。

- [安裝網路控制卡伺服器角色](#install-the-network-controller-server-role)

- [設定網路控制卡的叢集](#configure-the-network-controller-cluster)

- [設定網路控制站應用程式](#configure-the-network-controller-application)

- [網路控制站部署驗證](#network-controller-deployment-validation)

- [網路控制站的其他 Windows PowerShell 命令](#additional-windows-powershell-commands-for-network-controller)

- [網路控制站組態指令碼範例](#sample-network-controller-configuration-script)

- [非 Kerberos 部署的部署後步驟](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>安裝網路控制卡伺服器角色

您可以使用此程序的虛擬機器上安裝網路控制卡伺服器角色\(VM\)。

>[!IMPORTANT]
>不會部署在實體主機上的網路控制卡伺服器角色。 若要部署網路控制站，您必須為 HYPER-V 虛擬機器上安裝網路控制卡伺服器角色\(VM\)安裝於 HYPER-V 主機。 在三個不同的 Hyper-v Vm 上安裝網路控制站之後\-Hyperv 主機，您必須啟用 Hyper-v\-的軟體定義網路的 HYPER-V 主機\(SDN\)藉由將主機新增至網路控制站。 如此一來，您將使 SDN 軟體負載平衡器，函式。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  

>[!NOTE]
>如果您想要使用而不是 Windows PowerShell 的伺服器管理員安裝網路控制站，請參閱[安裝網路控制卡伺服器角色，使用 伺服器管理員](https://technet.microsoft.com/library/mt403348.aspx)

若要使用 Windows PowerShell 安裝網路控制站，在 Windows PowerShell 提示字元中，輸入下列命令，，然後按 ENTER 鍵。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

網路控制站的安裝需要重新啟動電腦。 若要這樣做，請輸入下列命令，並再按 ENTER 鍵。

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>設定網路控制卡的叢集

網路控制卡叢集提供高可用性和延展性，網路控制站應用程式，您可以在建立叢集之後設定和裝載在叢集之上。

>[!NOTE]
>您可以執行程序在下列各節是直接在其中安裝網路控制站，或您可以從遠端電腦正在執行的程序中使用遠端伺服器管理工具的 Windows Server 2016 VMWindows Server 2016 或 Windows 10 中。 此外中的成員資格**系統管理員**，或同等權限，才能執行此程序的最小值。 如果您安裝網路控制站 VM 的電腦已加入網域，您的使用者帳戶必須隸屬**網域使用者**。

您可以建立網路控制卡的叢集，建立節點物件，然後設定 叢集。

### <a name="create-a-node-object"></a>建立節點物件

您必須是網路控制卡叢集成員的每個 VM 建立節點物件。

若要建立節點物件時，在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您所加入的每個參數的值適用於您的部署。  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表提供每個參數的說明**新增 NetworkControllerNodeObject**命令。

|參數|描述|
|-------------|---------------|
|名稱|**名稱**參數會指定您想要新增到叢集伺服器的易記名稱|
|Server|**Server**參數會指定主機名稱、 完整格式網域名稱 (FQDN) 或您想要新增到叢集之伺服器的 IP 位址。 針對已加入網域的電腦，FQDN 是必要的。|
|FaultDomain|**FaultDomain**參數會指定您要新增到叢集伺服器的失敗網域。 此參數定義的伺服器，可能會遇到失敗，在此同時做為您要新增至叢集的伺服器。 此失敗可能是因為共用實體的相依性，例如電源和網路來源。 容錯網域通常代表這些共用相依性，可能會從容錯網域樹狀目錄中的較高點一起失敗的多部伺服器相關的階層。 在執行階段，網路控制站會考慮叢集中的容錯網域和嘗試，讓它們位在不同的容錯網域分散網路控制卡服務。 此程序有助於確保該服務和其狀態的可用性不會受到危害，任何一個容錯網域中，發生錯誤時。 階層式格式中指定容錯網域。 例如: 「 Fd: / DC1/Rack1/Host1"，其中 DC1 是資料中心名稱、 Rack1 是機架名稱而 Host1 是節點所在的主機名稱。|
|RestInterface|**RestInterface**參數指定的介面名稱在已終止 Representational State Transfer (REST) 通訊的節點上。 此網路控制器介面會接收來自網路的管理層 Northbound API 要求。|
|NodeCertificate|**NodeCertificate**參數指定網路控制站會使用進行電腦驗證的憑證。 如果您使用憑證式驗證叢集; 中的通訊，則需要憑證憑證也用於網路控制卡服務之間的流量進行加密。 憑證主體名稱必須與節點的 DNS 名稱相同。|

### <a name="configure-the-cluster"></a>設定叢集

若要設定叢集，在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您所加入的每個參數的值適用於您的部署。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表提供每個參數的說明**安裝 NetworkControllerCluster**命令。
  
|參數|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication**參數會指定用來保護節點之間的通訊，也會用於網路控制卡服務之間的流量進行加密的驗證類型。 支援的值為**Kerberos**， **X509**並**None**。 Kerberos 驗證會使用網域帳戶，並僅適用於網路控制卡節點是否已加入網域。 如果您指定 X509 為基礎的驗證，您必須提供 NetworkControllerNode 物件中的憑證。 此外，您必須以手動方式佈建憑證之前執行此命令。|
|ManagementSecurityGroup|**ManagementSecurityGroup**參數會指定安全性群組的成員，其中包含允許執行的管理 cmdlet 從遠端電腦的使用者名稱。 這才適用 ClusterAuthentication 是 Kerberos。 在本機電腦上，您必須指定網域安全性群組，而不安全群組。|
|節點|**節點**參數指定您所使用的網路控制卡節點清單**新增 NetworkControllerNodeObject**命令。|
|DiagnosticLogLocation|**DiagnosticLogLocation**參數會指定診斷記錄檔會定期上傳所在的共用位置。 如果您未指定此參數的值，會在本機記錄檔儲存每個節點上。 記錄檔會儲存在本機資料夾 %systemdrive%\windows\tracing\sdndiagnostics。 叢集記錄檔會儲存在本機資料夾 %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces。|
|LogLocationCredential|**LogLocationCredential**參數指定的認證所需的存取記錄檔儲存所在的共用位置。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate**參數指定網路控制器用來加密的認證，用來存取網路控制站的二進位檔的憑證和**LogLocationCredential**，如果指定了。 上的所有網路控制卡節點才能執行此命令時，必須佈都建憑證，必須在所有叢集節點上都註冊相同的憑證。 在生產環境中，建議使用來保護網路控制站的二進位檔和記錄檔中使用此參數。 如果沒有這個參數，會以純文字儲存的認證，並且可能會誤用任何未經授權的使用者。|
|認證|只有當您執行此命令從遠端電腦時，才需要此參數。 **認證**參數指定具有執行此命令在目標電腦上的權限的使用者帳戶。|
|憑證指紋|只有當您執行此命令從遠端電腦時，才需要此參數。 **CertificateThumbprint**參數指定的數位公開金鑰憑證 (X509) 具有目標電腦上執行此命令的權限的使用者帳戶。|
|UseSSL|只有當您執行此命令從遠端電腦時，才需要此參數。 **UseSSL**參數會指定用來連接到遠端電腦的安全通訊端層 (SSL) 通訊協定。 預設不會使用 SSL。|
|ComputerName|**ComputerName**參數指定網路控制卡節點此命令會執行。 如果您未指定此參數的值，預設會使用本機電腦。|
|LogSizeLimitInMBs|此參數會指定記錄檔大小上限，以 mb 為單位，可以儲存網路控制站。 記錄檔會儲存在循環方式。 如果提供 DiagnosticLogLocation，則此參數的預設值是 40 GB。 如果未提供 DiagnosticLogLocation，記錄會儲存在網路控制卡節點上，此參數的預設值為 15 GB。|
|LogTimeLimitInDays|此參數會指定持續時間限制，以天為單位，為其記錄會儲存。 記錄檔會儲存在循環方式。 此參數的預設值是 3 天。|

## <a name="configure-the-network-controller-application"></a>設定網路控制站應用程式
若要設定網路控制站應用程式，在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您所加入的每個參數的值適用於您的部署。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表提供每個參數的說明**安裝 NetworkController**命令。

|參數|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication**參數會指定驗證類型，用來保護 REST 與網路控制站之間的通訊。 支援的值為**Kerberos**， **X509**並**None**。 Kerberos 驗證會使用網域帳戶，並僅適用於網路控制卡節點是否已加入網域。 如果您指定 X509 為基礎的驗證，您必須提供 NetworkControllerNode 物件中的憑證。 此外，您必須以手動方式佈建憑證之前執行此命令。|
|節點|**節點**參數指定您所使用的網路控制卡節點清單**新增 NetworkControllerNodeObject**命令。|
|ClientCertificateThumbprint|只有在您的網路控制站用戶端使用憑證型驗證時，才需要此參數。 **ClientCertificateThumbprint**參數會指定已註冊至 Northbound 的圖層上的用戶端憑證的指紋。|
|ServerCertificate|**ServerCertificate**參數指定網路控制站會使用用戶端證明其識別身分的憑證。 伺服器憑證必須包含伺服器驗證目的，在 增強金鑰使用方法的擴充功能，並且必須由用戶端信任的 CA 所發行到網路控制站。|
|RESTIPAddress|您不需要指定的值**RESTIPAddress**與網路控制站的單一節點部署。 多重節點部署中， **RESTIPAddress**參數會指定 REST 端點的 IP 位址採用 CIDR 標記法。 比方說，192.168.1.10/24。 主體名稱值**ServerCertificate**的值必須解析**RESTIPAddress**參數。 當所有節點都位於相同子網路，必須指定這個參數的所有多節點網路控制卡部署。 如果節點位於不同的子網路，您必須使用**RestName**而不是使用的參數**RESTIPAddress**。|
|RestName|您不需要指定的值**RestName**與網路控制站的單一節點部署。 唯一的時候，您必須指定的值**RestName**是當多個節點部署在不同的子網路上的節點。 多重節點部署中， **RestName**參數指定網路控制卡叢集的 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup**參數指定的 Active Directory 安全性群組的成員是網路控制站用戶端的名稱。 這是必要參數只有當您使用 Kerberos 驗證**ClientAuthentication**。 安全性群組必須包含的帳戶的 REST Api 存取的來源，而且您必須建立的安全性群組，並將成員加入之前執行此命令。|
|認證|只有當您執行此命令從遠端電腦時，才需要此參數。 **認證**參數指定具有執行此命令在目標電腦上的權限的使用者帳戶。|
|憑證指紋|只有當您執行此命令從遠端電腦時，才需要此參數。 **CertificateThumbprint**參數指定的數位公開金鑰憑證 (X509) 具有目標電腦上執行此命令的權限的使用者帳戶。|
|UseSSL|只有當您執行此命令從遠端電腦時，才需要此參數。 **UseSSL**參數會指定用來連接到遠端電腦的安全通訊端層 (SSL) 通訊協定。 預設不會使用 SSL。|

完成網路控制站應用程式的組態之後，您的網路控制站的部署已完成。

## <a name="network-controller-deployment-validation"></a>網路控制站部署驗證

若要驗證您的網路控制卡部署，您可以將認證新增至網路控制站，然後擷取認證。

如果您使用 Kerberos 作為 ClientAuthentication 機制、 成員資格**ClientSecurityGroup**建立的是，至少需要執行此程序。

**程序：**

1.  用戶端電腦上，如果您使用 Kerberos 做為 ClientAuthentication 機制，登入的成員的使用者帳戶與您**ClientSecurityGroup**。

2. 開啟 Windows PowerShell，輸入下列命令將認證新增至網路控制站，然後按 ENTER。 請確定您所加入的每個參數的值適用於您的部署。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 若要擷取的認證，您已加入網路控制站，請輸入下列命令，，然後按 ENTER 鍵。 請確定您所加入的每個參數的值適用於您的部署。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. 檢視命令輸出應該類似下列的範例輸出。

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
    > 當您執行**Get NetworkControllerCredential**命令時，您可以將命令的輸出指派給變數使用點運算子來列出的認證屬性。 比方說，$cred。屬性。

## <a name="additional-windows-powershell-commands-for-network-controller"></a>網路控制站的其他 Windows PowerShell 命令

部署網路控制站之後，您可以使用 Windows PowerShell 命令來管理和修改您的部署。 以下是一些您可以對您的部署進行的變更。

- 修改網路控制卡節點、 叢集和應用程式設定

- 移除網路控制站叢集和應用程式

- 管理網路控制卡的叢集節點，包括新增、 移除、 啟用及停用節點。

下表提供的語法適用於 Windows PowerShell 命令可用來完成這些工作。

|工作|命令|語法|
|--------|-------|----------|
|修改網路控制卡的叢集設定|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站應用程式設定|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制卡節點設定|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改網路控制站診斷設定|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制站的應用程式|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|移除網路控制卡的叢集|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|將節點加入至網路控制卡的叢集|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|停用網路控制卡的叢集節點|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|啟用網路控制卡的叢集節點|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|從叢集移除網路控制卡節點|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>網路控制站的 Windows PowerShell 命令會在 TechNet Library 中[網路控制站 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。

## <a name="sample-network-controller-configuration-script"></a>網路控制站組態指令碼範例

下列範例設定指令碼會示範如何建立多節點網路控制卡叢集，並安裝網路控制站應用程式。 颾魤 ㄛ $cert 變數會從符合的主體名稱的字串"networkController.contoso.com 」 的本機電腦憑證存放區中，選取憑證。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>非 Kerberos 部署的部署後步驟

如果您不使用 Kerberos，與網路控制卡部署，您必須部署憑證。

如需詳細資訊，請參閱 <<c0> [ 網路控制站的部署後步驟](../technologies/network-controller/post-deploy-steps-nc.md)。


