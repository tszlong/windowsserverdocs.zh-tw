---
title: 對 Windows Server 軟體定義網路堆疊進行疑難排解
description: 此 Windows Server 快速入門會檢查軟體定義網路 (SDN) 的常見錯誤和失敗案例，並概述利用可用的診斷工具的疑難排解工作流程。
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: eeb0c335e4afd3c6835a04421a15073aeab6cdc6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446245"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>對 Windows Server 軟體定義網路堆疊進行疑難排解

>適用於：Windows Server （半年通道），Windows Server 2016

本指南會檢查軟體定義網路 (SDN) 的常見錯誤和失敗案例，並概述利用可用的診斷工具的疑難排解工作流程。  

如需有關 Microsoft 的軟體定義網路功能的詳細資訊，請參閱 <<c0> [ 軟體定義網路](../../sdn/Software-Defined-Networking--SDN-.md)。  

## <a name="error-types"></a>錯誤類型  
下列清單代表在市場中生產環境部署 Windows Server 2012 R2 中 HYPER-V 網路虛擬化 (HNVv1) 最常出現的問題類別，並在許多方面伴隨相同類型的 Windows Server 2016 中的問題使用新的軟體定義網路 (SDN) 堆疊 HNVv2。  

大多數錯誤可以歸類成較少的類別：   
* **無效或不受支援的組態**  
   不正確或無效的原則，則使用者會叫用即 NorthBound API。   

* **在原則的應用程式中的錯誤**  
     從網路控制卡的原則未傳遞至 HYPER-V 主機時，大幅延遲和/或不是最新 （比方說，在即時移轉） 之後的所有 HYPER-V 主機上。  
* **設定漂移或軟體 bug**  
  已卸除的封包中所產生的資料路徑相關問題。  

* **外部錯誤與 NIC 硬體 / 驅動程式或為網路網狀架構**  
  異常工作卸載 （例如 VMQ) 或為網路網狀架構設定錯誤 （例如 MTU)   

  這份疑難排解指南會檢查每個這些錯誤類別目錄，並建議最佳作法和診斷工具可用來識別並修正錯誤。  

## <a name="diagnostic-tools"></a>診斷工具  

之前討論的疑難排解工作流程的每個這些類型的錯誤，讓我們來檢查可用的診斷工具。   

若要使用的網路控制站 （控制路徑） 的診斷工具，您必須先安裝 RSAT NetworkController 功能並匯入``NetworkControllerDiagnostics``模組：  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

若要使用 HNV 診斷 （資料路徑） 的診斷工具，您必須匯入``HNVDiagnostics``模組：

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>網路控制站診斷  
這些 cmdlet 會記載在 TechNet 上[網路控制站診斷 Cmdlet 主題](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/)。 它們幫助識別控制路徑之間網路控制卡節點以及在網路控制站和 HYPER-V 主機上執行之 NC 主機代理程式的網路原則一致性的問題。

 _偵錯 ServiceFabricNodeStatus_並_Get NetworkControllerReplica_必須從一部網路控制卡節點虛擬機器中執行 cmdlet。 所有其他 NC 診斷 cmdlet 可以執行從已連線到網路控制站和在網路控制站管理安全性群組 (Kerberos) 或具有存取 X.509 憑證來管理網路控制站的任何主機。 

### <a name="hyper-v-host-diagnostics"></a>HYPER-V 主機診斷  
這些 cmdlet 會記載在 TechNet 上[HYPER-V 網路虛擬化 (HNV) 診斷 Cmdlet 主題](https://docs.microsoft.com/powershell/module/hnvdiagnostics/)。 它們可協助識別租用戶虛擬機器 （東/西） 之間的資料路徑中的問題和透過 SLB VIP （北/南） 的輸入流量。 

_偵錯 VirtualMachineQueueOperation_， _Get CustomerRoute_， _Get PACAMapping_， _Get ProviderAddress_， _Get VMNetworkAdapterPortId_， _Get VMSwitchExternalPortId_，和_測試 EncapOverheadSettings_都可以從任何 Hyper-v 主機執行的所有本機測試。 其他指令程式會叫用透過網路控制站的資料路徑測試，並因此需要存取網路控制站指定為 descried 上方。

### <a name="github"></a>GitHub
[Microsoft/SDN GitHub 存放庫](https://github.com/microsoft/sdn)有一些範例指令碼和這些內建 cmdlet 為基礎的工作流程。 特別是，在找到診斷的指令碼[診斷](https://github.com/Microsoft/sdn/diagnostics)資料夾。 請協助我們對這些指令碼，提交提取要求。

## <a name="troubleshooting-workflows-and-guides"></a>疑難排解工作流程與指南  

### <a name="hoster-validate-system-health"></a>[主機服務提供者]驗證系統的健康情況
沒有名為內嵌的資源_組態狀態_中數個網路控制站的資源。 組態狀態提供包括網路控制站的組態和 HYPER-V 主機上的實際 （執行） 狀態的一致性的系統健康情況相關資訊。 

若要檢查設定狀態，從執行下列任何 Hyper-v 主機連線到網路控制站。

>[!NOTE] 
>值*NetworkController*參數應該是根據 X.509 主體名稱的 FQDN 或 IP 位址 > 為網路控制站建立的憑證。
>
>*認證*參數，只需要指定 （在 VMM 部署一般） 的 Kerberos 驗證時，是否要使用網路控制站。 此認證必須是網路控制器管理安全性群組的使用者。

```none
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

如下所示的範例設定狀態訊息：

```none
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
> 有一個錯誤是其中 SLB Mux 傳輸 VM NIC 的網路介面資源處於失敗狀態發生錯誤 「 虛擬交換器-主機未連接到控制器 」 系統中。 如果從傳輸邏輯網路的 IP 集區中 VM 的 NIC 資源的 IP 組態設定為 IP 位址，就可以放心地忽略此錯誤。 處於失敗狀態，錯誤為 「 虛擬交換器-PortBlocked 」 中的閘道 HNV 提供者 VM Nic 的網路介面資源所在的系統中沒有第二個 bug。 這項錯誤也可以安全地忽略如果在 VM NIC 資源的 IP 組態設定為 null （依設計）。


下表顯示錯誤碼、 訊息和後續追蹤動作，才會根據所觀察到的組態狀態的清單。


| **Code**| **Message**| **動作**|  
|--------|-----------|----------|  
| 不明| 未知的錯誤| |  
| HostUnreachable                       | 找不到主機電腦 | 檢查網路控制卡和主機之間的管理網路連線 |  
| PAIpAddressExhausted                  | PA Ip 位址已用盡 | 增加 HNV 提供者的邏輯子網路的 IP 集區大小 |  
| PAMacAddressExhausted                 | PA Mac 位址已用盡 | 增加 Mac 集區範圍 |  
| PAAddressConfigurationFailure         | 可連接至主機的 PA 位址失敗 | 檢查網路控制卡和主機之間的管理網路連線。 |  
| CertificateNotTrusted                 | 憑證不受信任  |修正用來與主機通訊的憑證。 |  
| CertificateNotAuthorized              | 未獲授權的憑證 | 修正用來與主機通訊的憑證。 |  
| PolicyConfigurationFailureOnVfp       | 在 設定 VFP 原則失敗 | 這是執行階段失敗。  沒有明確的因應措施中。 收集記錄檔。 |  
| PolicyConfigurationFailure            | 將原則推送到主機，因為通訊失敗或其他失敗 NetworkController 時發生錯誤。| 沒有明確的動作。  這是因為在處理中的網路控制卡模組的目標狀態的失敗。 收集記錄檔。 |  
| HostNotConnectedToController          | 主機尚未連線到網路控制站 | 無法連線到網路控制站不會套用在主機或主機的通訊埠設定檔。 驗證 HostID 登錄機碼符合伺服器資源的執行個體識別碼 |  
| MultipleVfpEnabledSwitches            | 有多個 VFp 在主機上啟用切換  | 由於網路控制站主機代理程式只支援一個 vSwitch VFP 擴充功能已啟用，請刪除其中一個參數， |  
| PolicyConfigurationFailure            | 因為憑證錯誤或連線錯誤是因為 VmNic 的推送 VNet 原則失敗  | 檢查 是否已部署適當的憑證 （憑證主體名稱必須符合主機的 FQDN）。 也請驗證網路控制站的主機連線 |  
| PolicyConfigurationFailure            | 因為憑證錯誤或連線錯誤是因為 VmNic 的推送 vSwitch 原則失敗  | 檢查 是否已部署適當的憑證 （憑證主體名稱必須符合主機的 FQDN）。 也請驗證網路控制站的主機連線|
| PolicyConfigurationFailure            | 無法將因為憑證錯誤或連線錯誤是因為 VmNic 的發送防火牆原則 | 檢查 是否已部署適當的憑證 （憑證主體名稱必須符合主機的 FQDN）。 也請驗證網路控制站的主機連線|
| DistributedRouterConfigurationFailure | 無法設定主機 vNic 上的分散式路由器設定                          | TCPIP 堆疊時發生錯誤。 可能需要清除回報此錯誤發生在伺服器上的 PA 和 DR 主機 Vnic |
| DhcpAddressAllocationFailure          | DHCP 位址配置失敗是因為 VMNic                                                    | 檢查是否 NIC 資源上設定靜態 IP 位址屬性 |  
| CertificateNotTrusted<br>CertificateNotAuthorized | 無法連線至 Mux，因為網路或憑證錯誤 | 請檢查錯誤訊息的程式碼中提供的數字代碼： 這會對應至 winsock 錯誤程式碼。 憑證錯誤是細微 (比方說，您無法驗證憑證，憑證未獲授權，依此類推。) |  
| HostUnreachable                       | MUX 狀況不良 （常見的案例是 BGPRouter 中斷連線） | RRAS （BGP 虛擬機器） 或的機架頂端 (ToR) 交換器上的 BGP 對等體成功是無法連線到或無法對等互連。 檢查軟體負載平衡器多工器的資源和 BGP 對等互連 （ToR 或 RRAS 虛擬機器） 上的 BGP 設定 |  
| HostNotConnectedToController          | SLB 主機代理程式未連線  | 檢查 SLB 主機代理程式服務正在執行;請參閱 SLB 主機代理程式記錄檔 （自動執行），原因為何，萬一 SLBM (NC) 已拒絕執行的主機代理程式所呈現的憑證狀態將會顯示細微的資訊  |  
| PortBlocked                           | VFP 連接埠遭到封鎖，因為缺乏 VNET / ACL 規則 | 檢查是否有任何其他錯誤，可能會造成未設定的原則。 |  
| 超載                            | 多載的負載平衡器 MUX  | MUX 效能問題 |  
| RoutePublicationFailure               | 負載平衡器 MUX 未連接至 BGP 路由器 | 檢查是否 MUX 具有 BGP 路由器與該 BGP 對等互連的連線已正確設定 |  
| VirtualServerUnreachable              | 負載平衡器 MUX 未連線到 SLB 管理員 | 檢查 SLBM 和 MUX 之間的連線 |  
| QosConfigurationFailure               | 無法設定 QOS 原則 | 查看是否有足夠的頻寬適用於所有 VM 的 QOS 保留項目是使用 |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>請檢查網路連線之間的網路控制卡和 HYPER-V 主機 （NC 主機代理程式服務）
執行*netstat*下方的命令，以驗證有三個 NC 主機代理程式與網路控制卡節點和一個 HYPER-V 主機上的接聽通訊端之間的 ESTABLISHED 連線
- 在 HYPER-V 主機 （NC 主機代理程式服務） 上的連接埠 TCP:6640 上接聽
- 兩個建立的連接，從 HYPER-V 主機上連接埠 6640 NC 節點 ip 暫時連接埠 (> 32000) 上的 IP
- 其中一個建立的暫時連接埠上的 HYPER-V 主機 IP 連線到網路控制站 REST IP 連接埠 6640

>[!NOTE]
>只能有兩個 HYPER-V 主機上建立的連接是否部署在該特定的主機上沒有租用戶虛擬機器。

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>請檢查主機代理程式服務
網路控制站會在 HYPER-V 主機上的兩個主機代理程式服務與通訊：SLB 主機代理程式和 NC 主機代理程式。 可以，一或兩種服務未執行。 檢查其狀態並重新啟動，如果不執行。

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>檢查網路控制站的健全狀況
如果有不三個 ESTABLISHED 連線或網路控制卡似乎沒有回應，請檢查所有節點和服務模組，使用下列 cmdlet 也都已啟動並執行。 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
網路控制站服務模組有︰
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

檢查為 ReplicaStatus**就緒**而且 HealthState **[確定]** 。

在生產環境中部署已與多節點網路控制站，您也可以查看每項服務的主要在哪一個節點，以及其個別的複本狀態。

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
檢查複本狀態是否已準備好針對每個服務。

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>檢查有對應的 HostIDs 和網路控制站與每一部 HYPER-V 主機之間的憑證 
在 HYPER-V 主機上，執行下列命令來檢查 HostID 對應網路控制站上的伺服器資源的執行個體識別碼

```none
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

*補救*如果使用 SDNExpress 指令碼或手動部署，更新 HostId 金鑰在登錄中，以符合伺服器資源的執行個體識別碼。 如果使用 VMM 從 VMM 刪除 HYPER-V 伺服器，請重新啟動 HYPER-V 主機 （實體伺服器） 上的網路控制站主機代理程式，並移除 HostId 登錄機碼。 然後，重新新增到 VMM 伺服器。


檢查 (SouthBound) 之間進行通訊的 HYPER-V 主機 （NC 主機代理程式服務） 和網路控制站節點 （主機名稱會是憑證的主體名稱） 的 HYPER-V 主機所使用的 X.509 憑證的指紋相同。 也請檢查網路控制站的 REST 憑證有主體名稱*CN =<FQDN or IP>* 。

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

您也可以檢查下列參數的每個憑證，並確定主體名稱是什麼是預期 （主機名稱或 NC REST FQDN 或 IP），該憑證尚未過期，，和憑證鏈結中的所有憑證授權單位會都包含在受信任的根授權單位。

- 主體名稱  
- 到期日  
- 受信任的根授權單位  

*補救*如果多個憑證有相同的主體名稱為 HYPER-V 主機上，網路控制站主機代理程式會隨機選擇要呈現給網路控制站。 這可能不符合已知網路控制站的伺服器資源的憑證指紋。 在此情況下，刪除其中一個使用相同的主體名稱，在 HYPER-V 主機上的憑證，然後再重新啟動網路控制站主機代理程式服務。 如果可以仍然連接，請刪除具有相同的主體名稱，在 HYPER-V 主機上的其他憑證，並刪除對應的伺服器資源，在 VMM 中。 然後，重新建立伺服器資源，進而產生新的 X.509 憑證，並將它安裝在 HYPER-V 主機上的 VMM 中。


#### <a name="check-the-slb-configuration-state"></a>檢查 SLB 組態狀態
SLB 設定狀態可以判斷要偵錯 NetworkController cmdlet 的輸出的一部分。 此 cmdlet 也會輸出目前的網路控制站在 JSON 檔案中，從每個 HYPER-V 主機 （伺服器） 的所有 IP 組態，然後從主機代理程式的資料庫資料表的本機網路原則的資源集。 

根據預設，會收集額外的追蹤。 若要收集追蹤，將新增-IncludeTraces: $false 參數。

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>預設輸出位置是 < working_directory > \NCDiagnostics\ 目錄。 可以變更預設的輸出目錄使用`-OutputDirectory`參數。 

SLB 設定狀態的資訊可在_診斷 slbstateResults.Json_此目錄中的檔案。

此 JSON 檔案可以分成下列各節：
 * 網狀架構
   * SlbmVips-此區段會列出可由網路控制站與 coodinate 組態和 SLB Mux 和 SLB 主機代理程式之間的健全狀況的 SLB 管理員 VIP 位址的 IP 位址。
   * MuxState-此區段會列出一個值提供狀態的 mux 部署每個 SLB Mux 的
   * 路由器設定為此區段會列出上游路由器 （BGP 對等） 自發系統編號 (ASN)、 傳輸的 IP 位址和識別碼。 它也會列出 SLB Mux ASN 與轉送 IP。
   * 連線主機資訊-本章節將的清單管理 IP 位址的所有 HYPER-V 主機可執行負載平衡工作負載。
   * Vip 範圍-這一節會列出公用和私用 VIP 的 IP 集區範圍。 SLBM VIP 將會納入來自這些範圍的其中一個配置的 IP。 
   * Mux 路由-此區段會包含所有該特定 mux 的路由通告每個 SLB Mux 部署的列出一個值。
 * 租用戶
   * VipConsolidatedState-此區段會列出連線狀態每個租用戶 VIP 包括通告的路由前置詞、 HYPER-V 主機和 DIP 端點。

> [!NOTE]
> SLB 狀態可以是直接使用 reflection [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)上可用的指令碼[Microsoft SDN GitHub 存放庫](https://github.com/microsoft/sdn)。 

#### <a name="gateway-validation"></a>閘道驗證

**從網路控制卡：**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**從閘道 VM:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**從 Top of Rack (ToR) 交換器：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP Router**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

除了這些之外，從我們到目前為止看到 （特別是在 SDNExpress 型部署） 的問題未取得 GW Vm 上設定的租用戶區間的最常見原因似乎 FabricConfig.psd1 GW 容量是小於比較項目事實人嘗試指派 TenantConfig.psd1 中的網路連線 （S2S 通道）。 這可以輕鬆地藉由比較下列命令的輸出：

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[主機服務提供者]驗證資料平面
已部署網路控制站，租用戶虛擬網路和子網路建立之後，Vm 已附加至虛擬子網路之後，就可以執行其他的網狀架構層級測試由主機服務提供者檢查租用戶的連線。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>檢查 HNV 提供者邏輯網路連線
之後第一個客體 VM 的 HYPER-V 主機上執行已連線至租用戶虛擬網路，網路控制站會將兩個的 HNV 提供者 IP 位址 （PA IP 位址） 指派給 HYPER-V 主機。 這些 Ip 會來自 HNV 提供者邏輯網路的 IP 集區，且受網路控制站。  若要了解這兩個的 HNV IP 位址的

```none
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

這些 HNV 提供者 IP 位址 (PA Ip) 指派給個別的 TCPIP 網路區間中建立的乙太網路介面卡，而有配接器名稱_VLANX_其中 X 是指派給 HNV 提供者 （傳輸） 的邏輯網路的 VLAN。

使用邏輯網路可藉由使用其他的區間 ping 的 HNV 提供者的兩個 HYPER-V 主機之間的連線 (-c Y) 當中 Y 是在其中建立 PAhostVNICs TCPIP 網路區間的參數。 這個區間可決定藉由執行：

```none
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
> PA 主機 vNIC 介面卡不會用於資料路徑，因此不需要指派給 「 vEthernet (PAhostVNic) 介面卡 」 的 IP。

比方說，假設 HYPER-V 主機 1 和 2 有 HNV 提供者 (PA) IP 位址：

|-HYPER-V 主機-|-PA IP 位址 1|-PA IP 位址 2|
|---             |---            |---             |
|主控件 1 | 10.10.182.64 | 10.10.182.65 |
|主控件 2 | 10.10.182.66 | 10.10.182.67 |

我們可以使用下列命令來檢查 HNV 提供者邏輯網路連線兩者之間進行 ping。

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*補救*如果 HNV 提供者偵測未運作，請檢查您實體網路連線，包括 VLAN 設定。 每個 HYPER-V 主機上的實體 Nic 應在主幹模式中沒有指派的特定 vlan。 管理主機 vNIC 應管理邏輯網路的 VLAN 來隔離。

```none
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

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>檢查 HNV 提供者邏輯網路上的 MTU 和巨大框架支援

HNV 提供者邏輯網路中另一個常見的問題是實體的網路連接埠和/或乙太網路卡沒有夠大的 MTU 設定為處理從 VXLAN （或 NVGRE） 封裝的額外負荷。 
>[!NOTE]
> 某些乙太網路卡和驅動程式支援新 * EncapOverhead 關鍵字，將會自動設定網路控制站主機代理程式的值為 160。 此值接著會加入的值 * 其總和當做已公告的 MTU JumboPacket 關鍵字。
> 例如 * EncapOverhead = 160 和 * JumboPacket = 1514年 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要測試是否 HNV 提供者邏輯網路支援大型 MTU 大小端對端，使用_測試 LogicalNetworkSupportsJumboPacket_ cmdlet:
```none
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

# TODO: Success Results aftering updating MTU on physical switch ports
```

*補救*
* 調整的實體交換器連接埠，以最少的 MTU 大小 1674B （包括 14B 乙太網路標頭和結尾）
* 如果您的 NIC 卡不支援 EncapOverhead 關鍵字，調整為至少 JumboPacket 關鍵字 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>檢查租用戶 VM NIC 連線
指派給客體虛擬機器的每個 VM NIC 具有私用的客戶位址 (CA) 和 HNV 提供者位址 (PA) 空間之間的 CA-PA 對應。 這些對應會保留在 OVSDB server 資料表，每個 HYPER-V 主機上，而且可以藉由執行下列 cmdlet 中找到。

```none
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
> 如果您預期的 CA-PA 對應不會在指定的租用戶 VM 的輸出，請檢查網路控制站上的 VM NIC 和 IP 組態的資源_Get NetworkControllerNetworkInterface_ cmdlet。 而且請檢查 NC 主機代理程式與網路控制卡節點之間建立的連接。

利用此資訊，租用戶 VM ping 現在可由起始從網路控制站使用的主機服務提供者_測試 VirtualNetworkConnection_ cmdlet。

## <a name="specific-troubleshooting-scenarios"></a>特定的疑難排解案例

下列各節會提供針對特定案例進行疑難排解的指引。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>兩個租用戶虛擬機器之間沒有網路連線

1.  [租用戶]請確定租用戶虛擬機器中的 Windows 防火牆未封鎖流量。  
2.  [租用戶]檢查 IP 位址有執行指派給租用戶虛擬機器_ipconfig_。 
3.  [主機服務提供者]執行**測試 VirtualNetworkConnection**從 HYPER-V 主機，以驗證有問題的兩個租用戶虛擬機器之間的連線。 

>[!NOTE]
>VSID 指的是虛擬的子網路識別碼。 如果是 VXLAN，這是 VXLAN 網路識別碼 (VNI)。 您可以找到此值，藉由執行**Get PACAMapping** cmdlet。

#### <a name="example"></a>範例

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

建立 CA-ping 之間 「 綠色 Web VM 1 」 的主機上的 192.168.1.4 SenderCA ip"sa18n30-2.sa18.nttest.microsoft.com"ListenerCA ip 10.127.132.153 Mgmt ip 的同時附加到虛擬子網路 (VSID) 4114 192.168.1.5。

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

正在啟動 CA 空間 ping 測試啟動追蹤工作階段 Ping 192.168.1.5 成功從位址 192.168.1.4 Rtt = 0 毫秒


CA 路由資訊：

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

PA 路由資訊：

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip> ...

4. [租用戶]檢查有任何分散式的防火牆原則會指定虛擬子網路或 VM 上會封鎖流量的網路介面。    

查詢在 sa18.nttest.microsoft.com 網域 sa18n30nc 在示範環境中找到網路控制站的 REST API。

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>查看 IP 組態和參考這個 ACL 的虛擬子網路

1. [主機服務提供者]執行``Get-ProviderAddress``在這兩個 HYPER-V 主機裝載兩個租用戶中問題的虛擬機器，然後執行``Test-LogicalNetworkConnection``或``ping -c <compartment>``來驗證連線的 HNV 提供者邏輯網路上的 HYPER-V 主機
2.  [主機服務提供者]請確定 MTU 設定 HYPER-V 主機上正確和任何第 2 層切換裝置之間的 HYPER-V 主機。 執行``Test-EncapOverheadValue``有問題的所有 HYPER-V 主機上。 也請檢查之間的所有第 2 層交換器有 MTU 設定為最低 1674 位元組，以負責 160 個位元組的最大的額外負荷。  
3.  [主機服務提供者]如果 PA IP 位址不存在和/或 CA 連線已中斷，請檢查以確保已經收到網路原則。 執行``Get-PACAMapping``查看如果封裝規則和建立重疊虛擬網路所需的 CA-PA 對應正確建立。  
4.  [主機服務提供者]請確認網路控制站主機代理程式連線到網路控制站。 執行``netstat -anp tcp |findstr 6640``以查看是否   
5.  [主機服務提供者]主應用程式識別碼在 HKLM 中的核取 / 符合裝載租用戶虛擬機器的伺服器資源的執行個體識別碼。  
6. [主機服務提供者]請檢查連接埠設定檔識別碼相符的租用戶虛擬機器的 VM 網路介面的執行個體識別碼。  

## <a name="logging-tracing-and-advanced-diagnostics"></a>記錄、 追蹤及進階的診斷

下列各節提供記錄和追蹤的進階診斷資訊。

### <a name="network-controller-centralized-logging"></a>網路控制站的集中式記錄 

網路控制站可自動收集偵錯工具記錄檔，並將它們儲存在集中位置。 當您第一次，或稍後部署網路控制站時，可以啟用記錄檔收集。 記錄檔會收集從網路控制站，而且網路受網路控制卡的項目： 裝載機器、 軟體負載平衡器 (SLB) 和閘道機器。 

這些記錄包含網路控制卡叢集、 網路控制站應用程式、 閘道記錄檔、 SLB、 虛擬網路和分散式的防火牆的偵錯記錄檔。 每當新主機/SLB/閘道新增到網路控制站時，記錄會啟動這些機器上。 同樣地，從網路控制卡移除主機/SLB/閘道時，記錄會停止這些機器上。

#### <a name="enable-logging"></a>啟用記錄

當您安裝網路控制卡的叢集使用時，都會自動啟用記錄**安裝 NetworkControllerCluster** cmdlet。 根據預設，本機收集記錄在網路控制卡節點上 *%systemdrive%\SDNDiagnostics*。 很**強烈建議**您變更這個位置是在遠端檔案共用 （不是本機的）。 

網路控制卡的叢集記錄檔會儲存在 *%programData%\Windows Fabric\log\Traces*。 您可以指定記錄檔集合的集中式的位置**DiagnosticLogLocation**具有這也是建議的參數是在遠端檔案共用。 

如果您想要限制存取到這個位置，您可以提供具有存取認證**LogLocationCredential**參數。 如果您提供認證以存取記錄檔位置，您也應該提供**CredentialEncryptionCertificate**參數，用來加密在網路控制卡節點上本機儲存的認證。  

使用預設設定，它會建議您將至少要有 75 GB 中的中央位置和 25 GB 的本機節點上的可用空間 （如果未使用的中央位置） 3 個節點網路控制卡叢集。

#### <a name="change-logging-settings"></a>變更記錄設定

您可以變更記錄設定，在任何時候使用``Set-NetworkControllerDiagnostic``cmdlet。 可以變更下列設定：

- **集中式記錄檔位置**。  您可以變更的位置來儲存所有記錄檔，與``DiagnosticLogLocation``參數。  
- **若要存取記錄檔位置的認證**。  您可以變更認證，才能存取記錄檔位置，與``LogLocationCredential``參數。  
- **移至本機記錄**。  如果您已提供集中的位置來儲存記錄檔，您可以移回至本機登入，在網路控制卡節點上``UseLocalLogLocation``參數 （由於大型的磁碟空間需求不建議）。  
- **記錄範圍**。  根據預設，會收集所有記錄檔。 您可以變更的範圍，來收集唯一的網路控制卡叢集記錄檔。  
- **記錄層級**。  預設記錄層級是資訊。 您可以將它變更錯誤、 警告或詳細資訊。  
- **記錄過時時間**。  記錄檔會儲存在循環方式。 不論您是使用本機記錄或集中式的記錄，您會根據預設，有 3 天的記錄資料。 您可以變更此時間限制，而且具有**LogTimeLimitInDays**參數。  
- **記錄過時大小**。  根據預設，您必須記錄資料的最大 75GB 如果使用集中式的記錄和 25 GB，如果使用本機的記錄。 您可以變更此限制，而且具有**LogSizeLimitInMBs**參數。

#### <a name="collecting-logs-and-traces"></a>收集的記錄檔和追蹤

VMM 部署網路控制站依預設使用集中式的記錄。 部署網路控制卡服務範本時，會指定這些記錄檔的檔案共用位置。

如果檔案位置尚未指定，本機記錄會在網路控制站的每個節點上與記錄檔儲存在 C:\Windows\tracing\SDNDiagnostics 搭配使用。 這些記錄檔會儲存使用下列階層：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- 追蹤

網路控制站會使用 (Azure) 的 Service Fabric。 疑難排解特定問題時，可能需要 Service Fabric 記錄檔。 這些記錄檔都位於 C:\ProgramData\Microsoft\Service 網狀架構在每個網路控制卡節點。

如果使用者已執行_偵錯 NetworkController_ cmdlet，額外的記錄檔會提供已使用伺服器資源，網路控制卡中指定每個 HYPER-V 主機上。 這些記錄檔 （和追蹤，如果啟用） 會保留在 C:\NCDiagnostics

### <a name="slb-diagnostics"></a>SLB 診斷

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>SLBM 網狀架構錯誤 （裝載服務提供者動作）

1.  檢查正常軟體負載平衡器管理員 (SLBM) 和協調流程層可以來彼此通訊：SLBM]-> [SLB Mux 和 SLBM]-> [SLB 主機代理程式。 執行[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)從使用網路控制站的 REST 端點的存取權的任何節點。  
2.  驗證*SDNSLBMPerfCounters*其中一個網路控制卡節點 Vm （最好是主要網路控制卡節點-Get NetworkControllerReplica） 上的 PerfMon 中：
    1.  已連線至 SLBM 的負載平衡器 (LB) 引擎？ (*SLBM LBEngine 組態總計*> 0)  
    2.  不需自己端點至少知道 SLBM 嗎？ (*VIP 端點總數*> = 2)  
    3.  HYPER-V (DIP) 主機連線到 SLBM 嗎？ (*連線的 HP 用戶端*= = num 伺服器)   
    4.  連接到多工器 SLBM 嗎？ (*多工連線* == *多工器上狀況良好 SLBM* == *多工器回報運作正常*= # SLB Mux Vm)。  
3.  請確定設定的 BGP 路由器已成功對等互連與 SLB MUX  
    1.  如果使用 RRAS 的遠端存取 （也就是 BGP 虛擬機器）：  
        1.  取得 Bgp 應該會顯示已連線  
        2.  取得 BgpRouteInformation 應該會顯示至少一個路由，如 SLBM VIP 本身  
    2.  如果使用實體頂端機架 (ToR) 交換器 BGP 對等體，請參閱您的文件  
        1.  例如: # 顯示 bgp 執行個體  
4.  驗證*SlbMuxPerfCounters*並*SLBMUX*中 SLB Mux VM 上的 PerfMon 計數器
5.  檢查設定狀態和軟體負載平衡器管理員資源中的 VIP 範圍  
    1.  取得 NetworkControllerLoadBalancerConfiguration ConnectionUri < https://<FQDN or IP>| convertto json-深度 8 (檢查 VIP 範圍內 IP 集區，並確定 SLBM 自助 VIP (*LoadBalanacerManagerIPAddress*) 以及任何租用戶面向 Vip 是在這些範圍內）  
        1. 取得 NetworkControllerIpPool NetworkId"< 公用/私人 VIP 邏輯網路的資源識別碼 > 」-SubnetId 」 < 公用/私人 VIP 邏輯子網路的資源識別碼 > 」-ResourceId"<IP Pool Resource Id>"-ConnectionUri $uri | convertto json-深度 8 
    2.  Debug-NetworkControllerConfigurationState -  

如果任何一項以上失敗的檢查，租用戶 SLB 狀態也會在失敗模式。  

**補救**   
根據提供的下列診斷資訊，來修正下列：  
* 請確定連線 SLB Multiplexers  
  * 修正憑證問題  
  * 修正網路連線問題  
* 請確定已成功設定 BGP 對等互連的資訊  
* 請確定在登錄中的主應用程式識別碼符合 Server 資源的伺服器執行個體識別碼 (參考附錄*HostNotConnected*錯誤碼)  
* 收集記錄檔  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>SLBM 租用戶錯誤 （裝載服務提供者和租用戶動作）

1.  [主機服務提供者]請檢查*偵錯 NetworkControllerConfigurationState*任何負載平衡器資源是否處於錯誤狀態。 請嘗試下列動作項目 「 附錄 」 中的資料表來降低。   
    1.  請檢查 VIP 端點存在且通告路由  
    2.  檢查 已探索多少的 DIP 端點的 VIP 端點  
2.  [租用戶]驗證已正確指定負載平衡器資源  
    1.  驗證 DIP 端點會在 SLBM 中註冊要用來裝載租用戶虛擬機器，這會對應到負載平衡器後端位址集區的 IP 組態  
3.  [主機服務提供者]如果未探索到的 DIP 端點或連接：   
    1.  檢查*偵錯 NetworkControllerConfigurationState*  
        1.  驗證該 NC 和 SLB 主機代理程式已順利連線到網路控制站事件協調者使用 ``netstat -anp tcp |findstr 6640)``  
    2.  檢查*HostId*中*nchostagent*服務 regkey (參考*HostNotConnected* 「 附錄 」 中的錯誤程式碼) 與對應的伺服器資源執行個體識別碼 （相符``Get-NCServer |convertto-json -depth 8``)  
    3.  檢查虛擬機器通訊埠的連接埠設定檔識別碼符合對應虛擬機器 NIC 資源的執行個體識別碼   
4.  [主控提供者]收集記錄檔   

#### <a name="slb-mux-tracing"></a>SLB Mux 追蹤

從軟體負載平衡器多工的資訊也可以透過事件檢視器來決定。 
1. 在 [事件檢視器檢視] 功能表按一下 [顯示分析與偵錯記錄檔]
2. 瀏覽至 應用程式和服務記錄檔"> Microsoft > Windows > SlbMuxDriver > 在事件檢視器中追蹤 
3. 以滑鼠右鍵按一下它，然後選取 [啟用記錄]

>[!NOTE]
>建議您只有此一小段時間啟用，而您嘗試重現問題的記錄

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 追蹤

從任何 HYPER-V 主機裝載的客體 VM 附加至租用戶虛擬網路，您可以收集 VFP 追蹤，以判斷問題可能位於其中。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
