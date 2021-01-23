---
title: 對 Windows Server 軟體定義網路堆疊進行疑難排解
description: 本 Windows Server 指南探討常見的軟體定義網路 (SDN) 錯誤和失敗案例，並概述利用可用診斷工具的疑難排解工作流程。
manager: grcusanz
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/14/2018
ms.openlocfilehash: 8069bee3933e64e47a9bc9fc3259201dda68c669
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716634"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>對 Windows Server 軟體定義網路堆疊進行疑難排解

>適用於：Windows Server 2019、Windows Server 2016

本指南探討常見的軟體定義網路 (SDN) 錯誤和失敗案例，並概述利用可用診斷工具的疑難排解工作流程。

如需 Microsoft 的軟體定義網路功能的詳細資訊，請參閱 [軟體定義的網路](../software-defined-networking.md)功能。

## <a name="error-types"></a>錯誤類型
下列清單表示最常在 Hyper-v 網路虛擬 (化中看到的問題類別) HNVv1 Windows Server 2012 R2 中的生產環境內部部署，並以使用新的軟體定義網路 (SDN) Stack 的 Windows Server 2016 HNVv2 中的相同類型問題，以許多方式一致。

大部分的錯誤都可以分類成一組小型的類別：
* **無效或不支援的** 設定使用者以不正確的方式叫用 NorthBound API，或使用不正確原則。

* **原則應用程式發生錯誤** 來自網路控制站的原則未傳遞至 Hyper-v 主機，在所有 Hyper-v 主機上明顯延遲及/或不是最新 (例如，在即時移轉) 之後。
* 設定 **漂移或軟體錯誤** 資料路徑問題導致捨棄的封包。

* **與 NIC 硬體/驅動程式或基礎網路網狀架構相關的外部錯誤** 不正常的工作卸載 (例如 VMQ) 或基礎網路網狀架構設定錯誤的 (例如 MTU) 

  這份疑難排解指南會檢查每個錯誤類別，並建議可用來找出並修正錯誤的最佳作法和診斷工具。

## <a name="diagnostic-tools"></a>診斷工具

在討論這些錯誤類型的疑難排解工作流程之前，讓我們來檢查可用的診斷工具。

若要使用網路控制站) 診斷工具 (的控制路徑，您必須先安裝 RSAT-NetworkController 功能，並匯入 ``NetworkControllerDiagnostics`` 模組：

```powershell
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools
Import-Module NetworkControllerDiagnostics
```

若要使用 HNV 診斷 (資料路徑) 診斷工具，您必須匯入 ``HNVDiagnostics`` 模組：

```powershell
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics
```

### <a name="network-controller-diagnostics"></a>網路控制站診斷
這些 Cmdlet 記載于 TechNet 上的 [Network Controller Diagnostics Cmdlet 主題](/powershell/module/networkcontrollerdiagnostics/)中。 它們有助於找出網路控制站節點之間的網路原則一致性問題，以及在 Hyper-v 主機上執行的網路控制站和 NC 主機代理程式之間的網路原則一致性問題。

 ServiceFabricNodeStatus 和 _NetworkControllerReplica_ 指令 _程式_ 必須從其中一個網路控制站節點虛擬機器執行。 所有其他 NC 診斷 Cmdlet 都可以從任何可連線到網路控制站的主機執行，也可以在網路控制卡管理安全性群組 (Kerberos) 或可存取 x.509 憑證來管理網路控制站。

### <a name="hyper-v-host-diagnostics"></a>Hyper-v 主機診斷
這些 Cmdlet 記載于 TechNet 上的 [Hyper-v 網路虛擬化 (HNV) 診斷 Cmdlet 主題](/powershell/module/hnvdiagnostics/)中。 它們有助於找出租使用者虛擬機器 (東部/西部) 的資料路徑中的問題，以及透過 SLB VIP (北/南) 的輸入流量。

_VirtualMachineQueueOperation_、 _CustomerRoute_、 _PACAMapping_、 _ProviderAddress_、 _VMNetworkAdapterPortId_ _、VMSwitchExternalPortId_ 和 _Test-_ EncapOverheadSettings 都是可從任何 hyper-v 主機執行的本機測試，都可以執行。 其他 Cmdlet 會透過網路控制站叫用資料路徑測試，因此需要存取網路控制站，如上述 descried 所示。

### <a name="github"></a>GitHub
[Microsoft/SDN github](https://github.com/microsoft/sdn)存放庫有幾個範例腳本和工作流程，這些腳本和工作流程都是以這些現成的 Cmdlet 為基礎。 尤其 [是診斷資料夾中](https://github.com/Microsoft/sdn/diagnostics) 可以找到診斷腳本。 請藉由提交提取要求來協助我們參與這些腳本。

## <a name="troubleshooting-workflows-and-guides"></a>疑難排解工作流程和指南

### <a name="hoster-validate-system-health"></a>Hoster.config驗證系統健康情況
有幾個網路控制站資源中有一個名為 [設定 _狀態_ ] 的內嵌資源。 設定狀態提供系統健康情況的相關資訊，包括網路控制站設定與 Hyper-v 主機上) 狀態的實際 (之間的一致性。

若要檢查設定狀態，請從任何連線到網路控制站的 Hyper-v 主機執行下列各項。

>[!NOTE]
>*NetworkController* 參數的值應該是以針對網路控制站所建立之 x.509 >憑證的主體名稱為基礎的 FQDN 或 IP 位址。
>
>如果網路控制站使用 Kerberos 驗證 (典型的 VMM 部署) 中，就必須指定 *Credential* 參數。 認證必須是網路控制站管理安全性群組中的使用者。

```
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

範例設定狀態訊息如下所示：

```
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> 系統中有一個錯誤，即 SLB Mux 傳輸 VM NIC 的網路介面資源處於失敗狀態，併發生錯誤「虛擬交換器-主機未連線至控制器」。 如果 VM NIC 資源中的 IP 設定是從傳輸邏輯網路的 IP 集區設定為 IP 位址，則可以放心地忽略此錯誤。
> 系統中的第二個錯誤是閘道 HNV 提供者 VM Nic 的網路介面資源處於失敗狀態，併發生錯誤「虛擬交換器-PortBlocked」。 如果 VM NIC 資源中的 IP 設定 (由設計) 設定為 null，也可以安全地忽略此錯誤。

下表根據觀察到的設定狀態，顯示要採取的錯誤碼、訊息和後續動作的清單。

| **程式碼** | **訊息** | **動作** |
|--|--|--|
| Unknown | 未知的錯誤 |  |
| HostUnreachable | 無法連線到主機電腦 | 檢查網路控制站與主機之間的管理網路連線能力 |
| PAIpAddressExhausted | PA Ip 位址已用盡 | 增加 HNV 提供者邏輯子網的 IP 集區大小 |
| PAMacAddressExhausted | PA Mac 位址已耗盡 | 增加 Mac 集區範圍 |
| PAAddressConfigurationFailure | 無法將 PA 位址檢測至主機 | 檢查網路控制站與主機之間的管理網路連線能力。 |
| CertificateNotTrusted | 憑證不受信任 | 修正與主機通訊時所使用的憑證。 |
| CertificateNotAuthorized | 未授權憑證 | 修正與主機通訊時所使用的憑證。 |
| PolicyConfigurationFailureOnVfp | 設定 VFP 原則失敗 | 這是執行時間失敗。  沒有明確的解決辦法。 收集記錄檔。 |
| PolicyConfigurationFailure | 因為通訊失敗或 NetworkController 中的其他錯誤，導致無法將原則推送至主機。 | 沒有明確的動作。  這是因為網路控制器模組中的目標狀態處理失敗。 收集記錄檔。 |
| HostNotConnectedToController | 主機尚未連線到網路控制站 | 無法從網路控制卡存取主機或主機上未套用的通訊埠設定檔。 驗證 HostID 登錄機碼與伺服器資源的實例識別碼相符 |
| MultipleVfpEnabledSwitches | 主機上有多個已啟用 VFp 的交換器 | 刪除其中一個參數，因為網路控制站主機代理程式只支援一個已啟用 VFP 擴充功能的 vSwitch |
| PolicyConfigurationFailure | 因為發生憑證錯誤或連線錯誤，所以無法推送 VmNic 的 VNet 原則 | 檢查是否已部署正確的憑證 (憑證主體名稱必須符合主機) 的 FQDN。 也請確認主機與網路控制站的連線能力 |
| PolicyConfigurationFailure | 因為發生憑證錯誤或連線錯誤，所以無法推送 VmNic 的 vSwitch 原則 | 檢查是否已部署正確的憑證 (憑證主體名稱必須符合主機) 的 FQDN。 也請確認主機與網路控制站的連線能力 |
| PolicyConfigurationFailure | 因為發生憑證錯誤或連線錯誤，所以無法推送 VmNic 的防火牆原則 | 檢查是否已部署正確的憑證 (憑證主體名稱必須符合主機) 的 FQDN。 也請確認主機與網路控制站的連線能力 |
| DistributedRouterConfigurationFailure | 無法設定主機 vNic 上的分散式路由器設定 | TCPIP 堆疊錯誤。 可能需要清除報告此錯誤之伺服器上的 PA 和 DR 主機 Vnic |
| DhcpAddressAllocationFailure | VMNic 的 DHCP 位址配置失敗 | 檢查是否已在 NIC 資源上設定靜態 IP 位址屬性 |
| CertificateNotTrusted<br>CertificateNotAuthorized | 因為網路或憑證錯誤而無法連線到 Mux | 檢查錯誤訊息程式碼中提供的數值代碼：這會對應到 winsock 錯誤碼。 憑證錯誤是細微的 (例如，憑證無法驗證、憑證未獲授權等 )  |
| HostUnreachable | MUX 狀況不良 (常見案例是 BGPRouter 中斷連線)  | RRAS 上的 BGP 對等 (BGP 虛擬機器) 或機架頂端 (ToR) 切換無法連線或未成功地對等互連。 檢查軟體 Load Balancer 多工器資源和 BGP 對等 (ToR 或 RRAS 虛擬機器上的 BGP 設定)  |
| HostNotConnectedToController | SLB 主機代理程式未連線 | 確認 SLB 主機代理程式服務正在執行;請參閱 SLB 主機代理程式記錄 (自動執行) 的原因，例如，當 SLBM (NC) 拒絕主機代理程式執行狀態所顯示的憑證時，會顯示差別細微資訊 |
| PortBlocked | 因為缺少 VNET/ACL 原則，所以已封鎖 VFP 埠 | 檢查是否有任何其他錯誤，這可能會導致未設定原則。 |
| 超載 | 負載平衡器 MUX 已多載 | MUX 的效能問題 |
| RoutePublicationFailure | Loadbalancer MUX 未連線至 BGP 路由器 | 檢查 MUX 是否與 BGP 路由器連線，而且已正確設定 BGP 對等互連 |
| VirtualServerUnreachable | Loadbalancer MUX 未連線至 SLB 管理員 | 檢查 SLBM 和 MUX 之間的連線能力 |
| QosConfigurationFailure | 無法設定 QOS 原則 | 查看是否有足夠的頻寬可供所有 VM 使用（如果使用 QOS 保留） |

#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>檢查網路控制站與 Hyper-v 主機 (NC 主機代理程式服務之間的網路連線) 
執行下面的 *netstat* 命令，以驗證 NC 主機代理程式與網路控制站節點之間有三個已建立的連線 (s) 和 hyper-v 主機上的一個接聽通訊端
- 在 Hyper-v 主機上接聽埠 TCP： 6640 (NC 主機代理程式服務) 
- 已建立兩個從埠6640上的 Hyper-v 主機 IP 到暫時埠上 NC 節點 IP 的連線 ( # A0 32000) 
- 在埠6640上，有一個從 Hyper-v 主機 IP 以暫時埠連線到網路控制站 REST IP 的連接

>[!NOTE]
>如果該特定主機上沒有部署任何租使用者虛擬機器，Hyper-v 主機上可能只會有兩個已建立的連接。

```
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```

#### <a name="check-host-agent-services"></a>檢查主機代理程式服務
網路控制站會與 Hyper-v 主機上的兩個主機代理程式服務進行通訊： SLB 主機代理程式和 NC 主機代理程式。 其中一個或兩個服務都不在執行中。 檢查其狀態，並在未執行時重新開機。

```
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>檢查網路控制站的健全狀況
如果沒有三個已建立的連線，或網路控制站沒有回應，請使用下列 Cmdlet 查看所有節點和服務模組是否已啟動並執行。

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>

```
網路控制站服務模組如下：
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService
```

Check that ReplicaStatus is **Ready** and HealthState is **Ok**.

In a production deployment is with a multi-node Network Controller, you can also check which node each service is primary on and its individual replica status.

```powershell
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready
```

檢查每個服務的複本狀態是否已備妥。

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>檢查網路控制站與每部 Hyper-v 主機之間的對應 HostIDs 和憑證
在 Hyper-v 主機上，執行下列命令以檢查 HostID 是否對應到網路控制卡上伺服器資源的實例識別碼。

```powershell
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

*補救* 如果使用 SDNExpress 腳本或手動部署，請更新登錄中的 HostId 機碼，以符合伺服器資源的實例識別碼。 如果您使用 VMM，請在 Hyper-v 主機 (實體伺服器上重新開機網路控制站主機代理程式) 如果使用 VMM，請從 VMM 刪除 Hyper-v 伺服器，並移除 HostId 登錄機碼。 然後，透過 VMM 重新新增伺服器。

檢查 Hyper-v 主機 (主機名稱所使用的 x.509 憑證指紋是否為憑證的主體名稱)  (SouthBound) Hyper-v 主機 (NC 主機代理程式服務) 和網路控制器節點之間的通訊。 也請檢查網路控制站的 REST 憑證是否有 *CN = <FQDN or IP>* 的主體名稱。

```
# On Hyper-V Host
dir cert:\\localmachine\my

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```

您也可以檢查每個憑證的下列參數，以確定主體名稱是預期的 (主機名稱或 NC REST FQDN 或 IP) 、憑證尚未過期，而且憑證鏈中的所有憑證授權單位單位都包含在受信任的根授權單位中。

- 主體名稱
- 到期日
- 受根授權單位信任

*補救* 如果 Hyper-v 主機上有多個憑證具有相同的主體名稱，則網路控制站代理程式會隨機播放一個以顯示到網路控制站。 這可能不符合網路控制站已知的伺服器資源指紋。 在此情況下，請刪除 Hyper-v 主機上具有相同主體名稱的其中一個憑證，然後重新開機網路控制站主機代理程式服務。 如果仍然無法進行連線，請在 Hyper-v 主機上刪除具有相同主體名稱的其他憑證，並在 VMM 中刪除對應的伺服器資源。 然後，在 VMM 中重新建立伺服器資源，以產生新的 x.509 憑證，並將它安裝在 Hyper-v 主機上。

#### <a name="check-the-slb-configuration-state"></a>檢查 SLB 設定狀態
您可以在 Debug-NetworkController Cmdlet 的輸出中判斷 SLB 設定狀態。 此 Cmdlet 也會輸出 JSON 檔案中的目前網路控制站資源集、每一部 Hyper-v 主機 (伺服器的所有 IP 設定) 和主機代理程式資料庫資料表中的區域網路原則。

預設會收集其他追蹤。 若不要收集追蹤，請新增-IncludeTraces： $false 參數。

```
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>預設輸出位置將會是 <working_directory> \NCDiagnostics\ 目錄。 您可以使用參數來變更預設的輸出目錄 `-OutputDirectory` 。

您可以在此目錄中的檔案 _diagnostics-slbstateResults.Js_ 找到 SLB 設定狀態資訊。

此 JSON 檔案可細分為下列區段：

 * 網狀架構
   * SlbmVips-此區段會列出 SLB 管理員 VIP 位址的 IP 位址，此位址是網路控制站用來 coodinate SLB Mux 與 SLB 主機代理程式之間的設定和健全狀況。
   * MuxState-此區段會針對每個已部署的 SLB Mux （提供 Mux 狀態）列出一個值
   * 路由器設定-此區段會列出上游路由器的 (BGP 對等) 自發系統編號 (ASN) 、傳輸 IP 位址和識別碼。 它也會列出 SLB Mux ASN 和傳輸 IP。
   * 連線的主機資訊-此區段會列出管理 IP 位址，所有可用來執行負載平衡工作負載的 Hyper-v 主機。
   * Vip 範圍-此區段會列出公用和私人 VIP IP 集區範圍。 從下列其中一個範圍中，會將 SLBM VIP 包含為配置的 IP。
   * Mux 路由-此區段會針對每個已部署的 SLB Mux （包含該特定 Mux 的所有路由公告）列出一個值。
 * 租用戶
   * VipConsolidatedState-此區段會列出每個租使用者 VIP 的線上狀態，包括公告的路由首碼、Hyper-v 主機和 DIP 端點。

> [!NOTE]
> 您可以使用[MICROSOFT SDN GitHub 存放庫](https://github.com/microsoft/sdn)上提供的[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)腳本，直接確認 SLB 狀態。

#### <a name="gateway-validation"></a>閘道驗證

**從網路控制卡：**

```powershell
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**從閘道 VM：**

```powershell
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**從機架頂端 (ToR) 切換：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP 路由器**

```powershell
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

除此之外，從我們到目前為止所看到的問題 (特別是在以 SDNExpress 為基礎的部署) 中，在 GW Vm 上未設定租使用者區間的最常見原因，似乎是 FabricConfig.psd1 中的 GW 容量相較于) 1 中 (S2S 通道 TenantConfig.psd的使用者指派給網路連接的情況。 您可以藉由比較下列命令的輸出來輕鬆檢查這一點：

```powershell
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>Hoster.config驗證 Data-Plane
部署網路控制站之後，已建立租使用者虛擬網路和子網，且 Vm 已連結至虛擬子網，主機服務提供者可以執行額外的網狀架構層級測試，以檢查租使用者的連線能力。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>檢查 HNV 提供者邏輯網路連線能力
在 Hyper-v 主機上執行的第一個來賓 VM 連接到租使用者虛擬網路之後，網路控制站會將兩個 HNV 提供者 IP 位址指派給 Hyper-v 主機)  (PA IP 位址。 這些 Ip 會來自 HNV 提供者邏輯網路的 IP 集區，並由網路控制站管理。  若要瞭解這兩個 HNV 的 IP 位址是什麼

```powershell
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

這些 HNV 提供者 IP 位址 (PA IP) 會指派給在個別 TCPIP 網路區間中建立的乙太網路介面卡，而且介面卡名稱為 _VLANX_ ，其中 X 是指派給 HNV 提供者 (傳輸) 邏輯網路的 VLAN。

使用 HNV 提供者邏輯網路的兩個 Hyper-v 主機之間的連線，可以透過具有額外區間 (-c Y) 參數的 ping 來完成，其中 Y 是 PAhostVNICs 建立所在的 TCPIP 網路區間。 您可以執行下列程式來判斷此區間：

```
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> PA 主機 vNIC 介面卡不會用在資料路徑中，因此沒有指派給 "vEthernet (PAhostVNic) adapter" 的 IP。

比方說，假設 Hyper-v 主機1和2具有 HNV 提供者 (PA) IP 位址的 IP 位址：

| Hyper-V 主機 | PA IP 位址1 | PA IP 位址2 |
|--|--|--|
| 主機1 | 10.10.182.64 | 10.10.182.65 |
| 主機2 | 10.10.182.66 | 10.10.182.67 |

我們可以使用下列命令來檢查 HNV 提供者邏輯網路連線能力，以在兩者之間進行 ping。

```
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*補救* 如果 HNV 提供者 ping 無法運作，請檢查您的實體網路連線（包括 VLAN 設定）。 每一部 Hyper-v 主機上的實體 Nic 都應該在主幹模式中，而且不會指派特定的 VLAN。 管理主機 vNIC 應該隔離到管理邏輯網路的 VLAN。

```powershell
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...
```

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>檢查 HNV 提供者邏輯網路上的 MTU 和大型框架支援

HNV 提供者邏輯網路中另一個常見的問題是，實體網路埠和/或 Ethernet 卡沒有足夠的 MTU，無法設定來處理 VXLAN (或 NVGRE) 封裝的額外負荷。

>[!NOTE]
> 某些乙太網路卡和驅動程式支援新的 * EncapOverhead 關鍵字，它會自動由網路控制站主機代理程式設定為160的值。 然後，此值會加入至 * JumboPacket 關鍵字的值，其總和會當做公告的 MTU 使用。
> 例如 * EncapOverhead = 160 and * JumboPacket = 1514 => MTU = 1674B

```
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要測試 HNV 提供者邏輯網路是否支援端對端較大的 MTU 大小，請使用 _LogicalNetworkSupportsJumboPacket_ Cmdlet：

```powershell
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
```

*補救*
* 將實體交換器埠上的 MTU 大小調整為至少 1674B (包括14B 乙太網路標頭和尾端) 
* 如果您的 NIC 卡不支援 EncapOverhead 關鍵字，請將 JumboPacket 關鍵字調整為至少1674B

#### <a name="check-tenant-vm-nic-connectivity"></a>檢查租使用者 VM 的 NIC 連線能力
指派給來賓 VM 的每個 VM NIC 都有私人客戶位址 (CA) 與 HNV 提供者位址 (PA) 空間之間的 CA PA 對應。 這些對應會保留在每部 Hyper-v 主機上的 OVSDB 伺服器資料表中，並可透過執行下列 Cmdlet 來找到。

```
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66
```

>[!NOTE]
> 如果您預期的 CA-PA 對應不會輸出給指定的租使用者 VM，請使用 _NetworkControllerNetworkInterface 指令程式_ 檢查網路控制站上的 VM NIC 和 IP 設定資源。 此外，也請檢查 NC 主機代理程式和網路控制站節點之間的已建立連接。

有了這項資訊，便可使用 _VirtualNetworkConnection_ 指令程式，從網路控制站的主機服務提供者起始租使用者 VM 偵測。

## <a name="specific-troubleshooting-scenarios"></a>特定的疑難排解案例

下列各節提供針對特定案例進行疑難排解的指引。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>兩部租使用者虛擬機器之間沒有網路連線能力

1.  租確定租使用者虛擬機器中的 Windows 防火牆不會封鎖流量。
2.  租藉由執行 _ipconfig_，檢查是否已將 IP 位址指派給租使用者虛擬機器。
3.  Hoster.config從 Hyper-v 主機執行 **測試 VirtualNetworkConnection** ，以驗證兩個有問題的租使用者虛擬機器之間的連線能力。

>[!NOTE]
>VSID 是指虛擬子網識別碼。 在 VXLAN 的案例中，這是 VXLAN 網路識別碼 (VNI) 。 您可以藉由執行 **PACAMapping** Cmdlet 來尋找此值。

#### <a name="example"></a>範例

```powershell
$password = ConvertTo-SecureString -String "password" -AsPlainText -Force
$cred = New-Object pscredential -ArgumentList (".\administrator", $password)
```

在主機 "sa18n30-2.sa18.nttest.microsoft.com" 上具有 SenderCA IP of 192.168.1.4 的「綠色 Web VM 1」之間建立 CA 偵測，並將10.127.132.153 的管理 ip 附加至虛擬子網 (LISTENERCA) 4114。

```powershell
Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114
Test-VirtualNetworkConnection at command pipeline position 1

Starting CA-space ping test
Starting trace session
Ping to 192.168.1.5 succeeded from address 192.168.1.4
Rtt = 0 ms

CA Routing Information:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

PA Routing Information:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip> ...
```

1. 租請檢查虛擬子網或 VM 網路介面上未指定任何會封鎖流量的分散式防火牆原則。

在 sa18.nttest.microsoft.com 網域中 sa18n30nc 的示範環境中，查詢網路控制站 REST API 找到。

```
$uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
Get-NetworkControllerAccessControlList -ConnectionUri $uri
```

## <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>查看參考此 ACL 的 IP 設定和虛擬子網

1. Hoster.config ``Get-ProviderAddress`` 在兩部裝載有問題的租使用者虛擬機器的 hyper-v 主機上執行，然後 ``Test-LogicalNetworkConnection`` ``ping -c <compartment>`` 從 hyper-v 主機執行，以驗證 HNV 提供者邏輯網路上的連線能力
2.  Hoster.config確定 hyper-v 主機和 Hyper-v 主機之間的任何第2層切換裝置上的 MTU 設定都是正確的。 在 ``Test-EncapOverheadValue`` 所有有問題的 hyper-v 主機上執行。 此外，請檢查所有的第2層切換之間是否已將 MTU 設定為最少1674位元組，以考慮160位元組的最大額外負荷。
3.  Hoster.config如果 PA IP 位址不存在且/或 CA 連線中斷，請檢查以確定已收到網路原則。 執行 ``Get-PACAMapping`` 以查看建立重迭虛擬網路所需的封裝規則和 CA PA 對應是否已正確建立。
4.  Hoster.config檢查網路控制站主機代理程式是否已連線到網路控制站。 執行 ``netstat -anp tcp |findstr 6640`` 以查看是否有
5.  Hoster.config檢查 HKLM/中的主機識別碼是否符合裝載租使用者虛擬機器之伺服器資源的實例識別碼。
6. Hoster.config檢查通訊埠設定檔識別碼是否符合租使用者虛擬機器之 VM 網路介面的實例識別碼。

## <a name="logging-tracing-and-advanced-diagnostics"></a>記錄、追蹤和 advanced diagnostics

下列各節提供有關 advanced diagnostics、記錄和追蹤的資訊。

### <a name="network-controller-centralized-logging"></a>網路控制站的集中式記錄

網路控制站可以自動收集偵錯工具記錄，並將它們儲存在集中的位置。 當您第一次或稍後部署網路控制站時，可以啟用記錄收集。 記錄是從網路控制站收集，以及由網路控制站管理的網路元素：主機電腦、軟體負載平衡器 (SLB) 和閘道機器。

這些記錄包括網路控制站叢集、網路控制站應用程式、閘道記錄、SLB、虛擬網路和分散式防火牆的偵錯工具記錄。 每當新的主機/SLB/閘道新增至網路控制站時，就會在這些電腦上開機記錄。
同樣地，從網路控制站移除主機/SLB/閘道時，會在這些電腦上停止記錄。

#### <a name="enable-logging"></a>啟用記錄

當您使用  **NetworkControllerCluster 指令程式** 安裝網路控制站叢集時，會自動啟用記錄功能。 根據預設，記錄檔會在 *%systemdrive%\SDNDiagnostics* 的網路控制站節點本機上收集。 **強烈建議** 您將此位置變更為遠端檔案共用 (非本機) 。

網路控制站叢集記錄會儲存在 *%ProgramData%\Windows Fabric\log\Traces*。 您可以使用 **DiagnosticLogLocation** 參數指定記錄檔收集的集中位置，建議您也可以使用遠端檔案共用。

如果您想要限制此位置的存取權，您可以使用 **LogLocationCredential** 參數提供存取認證。 如果您提供存取記錄檔位置的認證，您也應該提供 **CredentialEncryptionCertificate** 參數，此參數是用來加密儲存在網路控制站節點本機上的認證。

使用預設設定時，建議您在中央位置至少有 75 GB 的可用空間，而在本機節點上至少有 25 GB 的可用空間 (如果沒有針對3節點的網路控制站叢集使用中央位置) 。

#### <a name="change-logging-settings"></a>變更記錄設定

您可以使用 Cmdlet 隨時變更記錄設定 ``Set-NetworkControllerDiagnostic`` 。 您可以變更下列設定：

- **集中式記錄檔位置**。  您可以使用參數變更要儲存所有記錄檔的位置 ``DiagnosticLogLocation`` 。
- **存取記錄檔位置的認證**。  您可以使用參數來變更要存取記錄檔位置的認證 ``LogLocationCredential`` 。
- **移至本機記錄**。  如果您已提供集中位置來儲存記錄檔，您可以使用參數將其移回本機的網路控制卡節點上的登入， ``UseLocalLogLocation`` (不建議的原因是) 的大型磁碟空間需求。
- **記錄範圍**。  預設會收集所有記錄檔。 您可以將範圍變更為只收集網路控制站叢集記錄。
- **記錄層級**。  預設記錄層級為 [資訊]。 您可以將它變更為 [錯誤]、[警告] 或 [詳細資訊]。
- **記錄過時時間**。  記錄會以迴圈方式儲存。 根據預設，您會有3天的記錄資料，無論您是使用本機記錄或集中式記錄。 您可以使用 **LogTimeLimitInDays** 參數來變更此時間限制。
- **記錄過時的大小**。  根據預設，如果使用集中式記錄，您將會有最大 75 GB 的記錄資料，如果使用本機記錄則為 25 GB。 您可以使用 **LogSizeLimitInMBs** 參數來變更此限制。

#### <a name="collecting-logs-and-traces"></a>收集記錄和追蹤

VMM 部署預設會使用網路控制站的集中式記錄。 部署網路控制站服務範本時，會指定這些記錄檔的檔案共用位置。

如果未指定檔案位置，則會在每個網路控制器節點上使用本機記錄，並將記錄儲存在 C:\Windows\tracing\SDNDiagnostics。 這些記錄會使用下列階層儲存：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- 效能計數器
- SDNDiagnostics
- 追蹤

網路控制站會使用 (的 Azure) Service Fabric。 針對特定問題進行疑難排解時，可能需要 Service Fabric 記錄檔。 這些記錄可以在 C:\ProgramData\Microsoft\Service 網狀架構的每個網路控制站節點上找到。

如果使用者已執行 NetworkController 指令 _程式_ ，則會在每個已使用網路控制卡中的伺服器資源指定的 hyper-v 主機上提供其他記錄。 如果啟用的) 保留在 C:\NCDiagnostics 中，這些記錄 (和追蹤

### <a name="slb-diagnostics"></a>SLB 診斷

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>裝載服務提供者動作 (的 SLBM 網狀架構錯誤) 

1.  檢查軟體 Load Balancer 管理員 (SLBM) 是否正常運作，而且協調流程層可以彼此通訊： SLBM-> SLB Mux 和 SLBM > SLB 主機代理程式。 從可存取網路控制站 REST 端點的任何節點執行 [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) 。
2.  在其中一個網路控制站節點 Vm 上驗證 PerfMon 中的 *SDNSLBMPerfCounters* ， (最好是主要網路控制站節點-NetworkControllerReplica) ：
    1.  Load Balancer (LB) 引擎是否已連接到 SLBM？  (的 *SLBM LBEngine 設定總計* > 0) 
    2.  對於它自己的端點，是否至少要知道它呢？  (*VIP 端點總計* >= 2 ) 
    3.  Hyper-v (DIP) 主機是否已連線到 SLBM？ 連接的 (*HP 用戶端*  = = num server) 
    4.  SLBM 是否已連線到 Mux？  (*mux 連線*  ==  *mux 在 SLBM*  ==  *mux 報告狀況* 良好 = # SLB mux vm) 上狀況良好。
3.  確定已設定 BGP 路由器成功與 SLB MUX 的對等互連
    1.  如果使用 RRAS 搭配遠端存取 (即 BGP 虛擬機器) ：
        1.  Get-BgpPeer 應該會顯示 [已連線]
        2.  Get-BgpRouteInformation 應至少顯示一個適用于 SLBM self VIP 的路由
    2.  如果使用實體最上層 (ToR) 交換器作為 BGP 對等，請參閱您的檔
        1.  例如： # show bgp 實例
4.  驗證 SLB Mux VM 上 PerfMon 中的 *SlbMuxPerfCounters* 和 *SLBMUX* 計數器
5.  檢查軟體 Load Balancer Manager 資源中的設定狀態和 VIP 範圍
    1.  Get-NetworkControllerLoadBalancerConfiguration-ConnectionUri <HTTPs:// <FQDN or IP> | convertto-json-深度 8 (檢查 IP 集區中的 VIP 範圍，並 (確保) 和任何租使用者對應的 vip 都在這些範圍內) 
        1. Get-NetworkControllerIpPool-NetworkId "<公用/私人 VIP 邏輯網路資源識別碼>"-SubnetId "<公用/私人 VIP 邏輯子網資源識別碼>"-ResourceId " <IP Pool Resource Id> "-ConnectionUri $uri | convertto-json-深度8
    2.  Debug-NetworkControllerConfigurationState-

如果上述任何一項檢查失敗，則租使用者 SLB 狀態也會處於失敗模式。

**補救** 根據提供的下列診斷資訊，修正下列問題：
* 確定已連接 SLB Multiplexers
  * 修正憑證問題
  * 修正網路連線問題
* 確定已成功設定 BGP 對等互連資訊
* 請確定登錄中的主機識別碼符合伺服器資源 (參考附錄中的伺服器實例識別碼，以瞭解 *HostNotConnected* 錯誤碼) 
* 收集記錄

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a> (主機服務提供者和租使用者動作) 的 SLBM 租使用者錯誤

1.  Hoster.config檢查 *NetworkControllerConfigurationState* ，查看是否有任何 LoadBalancer 資源處於錯誤狀態。 請依照附錄中的「動作專案」表格來嘗試減輕問題。
    1.  檢查 VIP 端點是否存在並公告路由
    2.  檢查已探索 VIP 端點的 DIP 端點數目
2.  租確認已正確指定 Load Balancer 資源
    1.  驗證在 SLBM 中註冊的 DIP 端點，其裝載的租使用者虛擬機器對應至 LoadBalancer 後端位址集區 IP 設定
3.  Hoster.config如果未探索或連接 DIP 端點：
    1.  檢查 *NetworkControllerConfigurationState*
        1.  驗證 NC 和 SLB 主機代理程式是否已成功連接到網路控制站事件協調器，並使用 ``netstat -anp tcp |findstr 6640)``
    2.  檢查 *nchostagent* service 登錄中的 *HostId* (參考 *HostNotConnected* 附錄中的錯誤碼) 符合對應伺服器資源的實例識別碼 (``Get-NCServer |convertto-json -depth 8``) 
    3.  檢查虛擬機器埠的通訊埠設定檔識別碼符合對應的虛擬機器 NIC 資源實例識別碼
4.  [主控提供者]收集記錄檔

#### <a name="slb-mux-tracing"></a>SLB Mux 追蹤

您也可以透過事件檢視器來判斷來自軟體 Load Balancer Mux 的資訊。
1. 按一下事件檢視器的 [View] 功能表底下的 [顯示分析和調試記錄]
2. 流覽至 [應用程式及服務記錄檔] > Microsoft > Windows > SlbMuxDriver > 追蹤事件檢視器
3. 以滑鼠右鍵按一下它，然後選取 [啟用記錄]

>[!NOTE]
>建議您在嘗試重現問題時，只在短時間啟用此記錄

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 追蹤

從任何裝載連接至租使用者虛擬網路之來賓 VM 的 Hyper-v 主機，您都可以收集 VFP 追蹤，以判斷問題可能發生在哪裡。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch
netsh trace stop
netsh trace convert .\vfp.etl ov=yes
```