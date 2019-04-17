---
title: 部署 Network Controller 使用 Windows PowerShell
description: 本主題提供部署 Network Controller 上一個或多個電腦正在執行 Windows Server 2016 虛擬電腦 (Vm) 使用 Windows PowerShell 指示。
manager: brianlic
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
ms.openlocfilehash: cfd06662f317381fb77bf31db5ed60c2489ff871
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-controller-using-windows-powershell"></a>部署 Network Controller 使用 Windows PowerShell

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供部署 Network Controller 一或多個虛擬在電腦上 (Vm) 是執行 Windows Server 2016 使用 Windows PowerShell 指示。

>[!IMPORTANT]
>不要部署實體主機上的 Network Controller 伺服器角色。 若要部署 Network Controller，您必須安裝網路控制站伺服器角色 HYPER-V 一樣上 \(VM\) HYPER-V 主機上安裝。 有三種不同的 Hyper\ HYPER-V 主機上 Vm 上安裝 Network Controller 之後，您必須讓 Hyper\ HYPER-V 主機的網路軟體定義 \(SDN\) 加到使用 Windows PowerShell 命令 Network Controller 的主機**新-NetworkControllerServer**。 如此一來，您會讓 SDN 軟體負載平衡器函式。 如需詳細資訊，請查看[新-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

本主題包含下列各節。

- [安裝網路控制站伺服器角色](#bkmk_role)

- [設定 Network Controller 叢集](#bkmk_configure)

- [Network Controller 應用程式設定](#bkmk_app)

- [網路控制器部署驗證](#bkmk_validation)

- [Network Controller 的其他 Windows PowerShell 命令](#bkmk_ps)

- [範例 Network Controller 設定指令碼](#bkmk_script)

- [後 Post-Deployment 步驟非 Kerberos 部署](#bkmk_nonkerb)

## <a name="bkmk_role"></a>安裝網路控制站伺服器角色

您可以使用此程序上一樣，安裝 Network Controller 伺服器角色 \(VM\)。

>[!IMPORTANT]
>不要部署實體主機上的 Network Controller 伺服器角色。 若要部署 Network Controller，您必須安裝網路控制站伺服器角色 HYPER-V 一樣上 \(VM\) HYPER-V 主機上安裝。 有三種不同的 Hyper\ HYPER-V 主機上 Vm 上安裝 Network Controller 之後，您必須讓 Hyper\ HYPER-V 主機的網路軟體定義 \(SDN\) 加到 Network Controller 的主機。 如此一來，您會讓 SDN 軟體負載平衡器函式。

資格在**系統管理員**，或相當於，才能執行此程序最小值。  

>[!NOTE]
>如果您想要使用 Windows PowerShell 而的伺服器管理員安裝網路控制器，請查看[安裝使用伺服器管理員 Network Controller 伺服器角色](https://technet.microsoft.com/library/mt403348.aspx)

若要使用 Windows PowerShell 來安裝網路控制器，請在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

安裝 Network Controller 需要重新開機。 若要這樣做，請輸入下列命令，，然後按 ENTER 鍵。

`Restart-Computer`

## <a name="bkmk_configure"></a>設定 Network Controller 叢集

Network Controller 叢集提供可用性和延展性 Network Controller 應用程式，在建立叢集之後，您可以設定和的位於叢集上方。

>[!NOTE]
>您可以執行程序下列章節直接在 VM 位置安裝網路控制器，或您可以使用遠端伺服器管理工具的 Windows Server 2016 來執行從遠端電腦是執行 Windows 10 或 Windows Server 2016 的程序。 此外，在成員資格**系統管理員**，或相當於，才能執行此程序最小值。 如果 VM 時，您可以安裝 Network Controller 的電腦已經加入網域，您的使用者帳號必須成員的**網域使用者**。

您可以建立節點物件，然後叢集的設定來建立 Network Controller 叢集。

### <a name="create-a-node-object"></a>建立節點物件

您需要為每個成員叢集 Network Controller 的 VM 建立節點物件。

若要建立節點物件的 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您新增的每個參數值適用於您的部署。  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

下表中提供的每個參數描述**新-NetworkControllerNodeObject**命令。

|參數|描述|
|-------------|---------------|
|名稱|**名稱**參數指定您要新增到叢集伺服器的易記名稱|
|伺服器|**伺服器**參數指定主機名稱、完全完整網域名稱 (FQDN) 或您想要新增到叢集伺服器的 IP 位址。 加入網域的電腦，則需要 FQDN。|
|FaultDomain|**FaultDomain**參數指定的伺服器叢集您要加入的網域失敗。 此參數定義的伺服器，可能會發生錯誤，同時為您新增到叢集伺服器。 這個錯誤可能會因為電力和來源網路共用實體相依性。 錯誤網域通常表示階層有關更多伺服器可能會失敗在一起從高點錯誤網域樹與這些共用相依性。 在執行階段 Network Controller 認為錯誤網域中叢集，並嘗試分散 Network Controller 的服務，讓它們在不同的錯誤的網域。 此程序可協助確保受到不到該 service 和其狀態的可用性、的任何一項錯誤網域故障。 錯誤網域詳列於階層格式。 例如:「Fd: / Host1 日 Rack1 DC1 日」、位置 DC1 是 datacenter 名稱、Rack1 是架名稱，以及 Host1 是放置節點主機的名稱。|
|RestInterface|**RestInterface**參數指定位置終止代表狀態傳輸（將）通訊節點上的介面的名稱。 這個 Network Controller 介面從網路管理層接收 Northbound API 要求。|
|NodeCertificate|**NodeCertificate**參數指定使用電腦驗證 Network Controller 的憑證。 如果您使用的憑證以驗證叢集; 通訊，則需要憑證憑證也可用於 Network Controller 服務間的流量加密。 憑證主體名稱必須是節點的相同 DNS 名稱。|

### <a name="configure-the-cluster"></a>設定叢集

若要設定叢集的 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您新增的每個參數值適用於您的部署。

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

下表中提供的每個參數描述**安裝-NetworkControllerCluster**命令。
  
|參數|描述|
|-------------|---------------|
|ClusterAuthentication|**ClusterAuthentication**參數指定驗證類型用於節點間通訊的保護，也可用於 Network Controller 服務間的流量加密。 支援的值為**Kerberos**，**X509**並**無**。 F:kerberos 驗證使用網域帳號，並只能加入網域 Network Controller 節點時使用。 若您指定 X509 為基礎的驗證，您必須提供中 NetworkControllerNode 物件的憑證。 此外，您必須手動提供憑證之前您執行這個命令。|
|ManagementSecurityGroup|**ManagementSecurityGroup**參數指定包含使用者可從遠端電腦上執行管理 cmdlet 安全性群組的名稱。 這只有 Kerberos ClusterAuthentication 是否適用。 您必須指定網域安全性群組並不安全性群組本機電腦上。|
|節點|**節點**參數指定清單中，使用您建立網路控制器節點**新-NetworkControllerNodeObject**命令。|
|DiagnosticLogLocation|**DiagnosticLogLocation**參數指定分享位置診斷登會定期上載。 如果您不指定的值此參數，登的每個節點上儲存在本機。 登入資料夾 %systemdrive%\windows\tracing\sdndiagnostics 儲存在本機。 叢集登入資料夾 %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces 儲存在本機。|
|LogLocationCredential|**LogLocationCredential**參數指定的認證所需的存取共用位置登的儲存位置。|
|CredentialEncryptionCertificate|**CredentialEncryptionCertificate**參數指定憑證 Network Controller 使用加密用於存取網路控制器二進位檔認證和**LogLocationCredential**，如果指定。 上的所有網路控制器節點之前執行這個命令時，您必須都提供憑證，必須相同的憑證退出所有叢集節點上。 建議使用此參數保護 Network Controller 二進位檔和登 production 環境中。 此參數，而認證儲存在明文，可以濫用任何未經授權使用者。|
|認證|這是必要參數只有當您正在執行這個命令的遠端電腦。 **認證**參數指定帳號的目標電腦上執行此命令的權限。|
|CertificateThumbprint|這是必要參數只有當您正在執行這個命令的遠端電腦。 **CertificateThumbprint**參數指定的數位公開金鑰憑證 (X509) 帳號的目標電腦上執行此命令的權限。|
|UseSSL|這是必要參數只有當您正在執行這個命令的遠端電腦。 **UseSSL**參數指定用來建立連接到遠端電腦的安全通訊端層 (SSL) 通訊協定。 根據預設，不使用 SSL。|
|電腦名稱|**電腦名稱**參數指定 Network Controller 節點是執行這個命令。 如果您不指定的值此參數，預設為使用本機電腦。|
|LogSizeLimitInMBs|此參數指定登入最大大小 (mb)，可儲存 Network Controller。 登會儲存在循環的方式。 如果提供 DiagnosticLogLocation，此參數預設值是 40 GB。 未提供 DiagnosticLogLocation，如果登會儲存到網路控制器節點和此參數預設值為 15 GB。|
|LogTimeLimitInDays|此參數指定的時間限制，天，儲存的登入。 登會儲存在循環的方式。 此參數預設值是 3 天。|

## <a name="bkmk_app"></a>Network Controller 應用程式設定
若要設定 Network Controller 應用程式的 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。 請確定您新增的每個參數值適用於您的部署。

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

下表中提供的每個參數描述**安裝-NetworkController**命令。

|參數|描述|
|-------------|---------------|
|ClientAuthentication|**ClientAuthentication**參數指定用於保護其餘和 Network Controller 間通訊的驗證類型。 支援的值為**Kerberos**，**X509**並**無**。 F:kerberos 驗證使用網域帳號，並只能加入網域 Network Controller 節點時使用。 若您指定 X509 為基礎的驗證，您必須提供中 NetworkControllerNode 物件的憑證。 此外，您必須手動提供憑證之前您執行這個命令。|
|節點|**節點**參數指定清單中，使用您建立網路控制器節點**新-NetworkControllerNodeObject**命令。|
|ClientCertificateThumbprint|您的網路控制器戶端使用憑證式驗證時，只需要此參數。 **ClientCertificateThumbprint**參數指定指紋已退出以戶端 Northbound 層上的憑證。|
|伺服器憑證|**伺服器憑證**參數指定其身份戶端用於 Network Controller 的憑證。 伺服器的憑證必須伺服器驗證目的納入增強金鑰使用方法的擴充功能，並必須發給 Network Controller 信任的樹系用 ca。|
|RESTIPAddress|您不需要指定的值為**RESTIPAddress**節點單一的部署 Network Controller。 對於多節點部署，請**RESTIPAddress**參數指定 IP 位址的其餘部分端點 CIDR 表示法中。 例如，192.168.1.10 24。 主體名稱為**伺服器憑證**必須解析的值為**RESTIPAddress**的參數。 所有節點上相同的子網路時必須此參數都指定所有多節點 Network Controller 部署。 如果節點上不同子網路，您必須使用**RestName**參數，而不要使用**RESTIPAddress**。|
|RestName|您不需要指定的值為**RestName**節點單一的部署 Network Controller。 唯一必須指定的值**RestName**當多節點部署有不同子網路上的節點。 對於多節點部署，請**RestName**參數指定叢集 Network Controller 的 FQDN。|
|ClientSecurityGroup|**ClientSecurityGroup**參數指定 Active Directory 安全性群組成員是戶端 Network Controller 的名稱。 這是必要參數只有當您使用 F:kerberos 驗證適用於**ClientAuthentication**。 安全性群組必須包含從中存取 REST Api，帳號，您必須建立安全性群組和新增成員，才能執行這個命令。|
|認證|這是必要參數只有當您正在執行這個命令的遠端電腦。 **認證**參數指定帳號的目標電腦上執行此命令的權限。|
|CertificateThumbprint|這是必要參數只有當您正在執行這個命令的遠端電腦。 **CertificateThumbprint**參數指定的數位公開金鑰憑證 (X509) 帳號的目標電腦上執行此命令的權限。|
|UseSSL|這是必要參數只有當您正在執行這個命令的遠端電腦。 **UseSSL**參數指定用來建立連接到遠端電腦的安全通訊端層 (SSL) 通訊協定。 根據預設，不使用 SSL。|

Network Controller 應用程式的設定完成之後，您的部署 Network Controller 的已完成。

## <a name="bkmk_validation"></a>網路控制器部署驗證

如果要驗證您的網路控制器部署，您可以新增 Network Controller 認證，並擷取 credential。

如果您使用 Kerberos 做為 ClientAuthentication 機制，成員資格在**ClientSecurityGroup**您建立是的最低需求才能執行此程序。

#### <a name="to-validate-deployment-of-network-controller"></a>若要驗證 Network Controller 的部署

1.  Client 的電腦上，如果您使用 Kerberos 做為 ClientAuthentication 機制，登入的使用者 account 的成員，您**ClientSecurityGroup**。

2. 打開 Windows PowerShell 輸入下列命令，以新增到網路控制器，請認證，然後按 ENTER 鍵。 請確定您新增的每個參數值適用於您的部署。

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. 若要擷取的認證，您新增到網路控制器，請輸入下列命令，，然後按 ENTER 鍵。 請確定您新增的每個參數值適用於您的部署。

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. 檢視命令的輸出之後，應該類似下列範例輸出。

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
    > 當您執行**取得-NetworkControllerCredential**命令時，您也可以使用點電信業者清單的認證屬性變數指派命令的輸出。 例如，$cred。屬性。

## <a name="bkmk_ps"></a>Network Controller 的其他 Windows PowerShell 命令

部署 Network Controller 之後，您可以使用 Windows PowerShell 命令管理和修改您的部署。 以下是一些變更，您可以讓您的部署。

- 修改 Network Controller] 節點、叢集，以及應用程式設定

- 移除 Network Controller 叢集和應用程式

- 管理網路控制器叢集節點，包括新增、移除、讓，以及停用節點。

下表會提供的語法 Windows PowerShell 命令，您可以使用完成這些工作。

|工作|命令|語法|
|--------|-------|----------|
|修改 Network Controller 叢集設定|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改 Network Controller 的應用程式設定|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改 Network Controller 節點設定|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|修改 Network Controller 診斷設定|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|移除 Network Controller 的應用程式|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|移除 Network Controller 叢集|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|將節點新增至 Network Controller 叢集|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|停用 Network Controller 叢集節點|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|讓 Network Controller 叢集節點|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|移除 Network Controller 節點從叢集|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Windows PowerShell 命令 Network Controller 的於 TechNet Library 在[網路控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。

## <a name="bkmk_script"></a>範例 Network Controller 設定指令碼

下列範例組態指令碼示範如何建立多個節點 Network Controller 叢集並安裝控制器網路應用程式。 此外，$cert 變數選取憑證從本機電腦的憑證存放區符合主體名稱字串」networkController.contoso.com」。

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="bkmk_nonkerb"></a>部署後續步驟的非-Kerberos 部署

如果您不使用 Kerberos Network Controller 部署，您必須將憑證部署。

如需詳細資訊，請查看[部署後步驟 Network Controller 的](../technologies/network-controller/post-deploy-steps-nc.md)。


