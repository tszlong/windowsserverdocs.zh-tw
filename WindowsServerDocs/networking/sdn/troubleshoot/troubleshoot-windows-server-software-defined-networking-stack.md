---
title: 疑難排解 Windows Server 軟體定義的網路堆疊
description: Windows Server 本文檢查軟體定義網路 (SDN) 常見的錯誤和失敗案例，並概述運用可診斷工具疑難排解工作流程。
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>疑難排解 Windows Server 軟體定義的網路堆疊

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本指南檢查軟體定義網路 (SDN) 常見的錯誤和失敗案例和概述運用可診斷工具疑難排解工作流程。  

如需 Microsoft 軟體定義網路的相關資訊，請查看[軟體定義網路](../../sdn/Software-Defined-Networking--SDN-.md)。  

## <a name="error-types"></a>錯誤類型  
以下清單代表 Windows Server 2012 R2 的市場中 production 部署種最常看到的 HYPER-V 網路模擬 (HNVv1) 的問題，並具有相同的新的軟體定義網路 (SDN) 堆疊與 Windows Server 2016 HNVv2 中看到的問題類型相合許多種方式。  

大部分的錯誤可分為一小組：   
* **無效的或不支援的設定**  
   錯誤或不正確的原則，使用者會叫用 NorthBound API。   

* **原則應用程式中錯誤**  
     Network Controller 的原則未傳遞至 HYPER-V 主機，大幅延遲和/或不（例如，在動態移轉）之後所有 HYPER-V 主機上最新狀態。  
* **設定積雪或軟體問題**  
 資料路徑問題會導致捨棄封包。  

* **NIC 硬體相關的外部錯誤 / 驅動程式或底圖網路 fabric**  
 發生錯誤工作卸載（例如 VMQ) 或底圖網路 fabric 設定錯誤（例如 MTU)   

 本疑難排解指南檢查每個類的錯誤和建議最佳做法，並找出並修正錯誤診斷工具。  

## <a name="diagnostic-tools"></a>診斷工具  

再為每個這類的錯誤的疑難排解工作流程，讓我們檢查可用的診斷工具。   
  
若要使用的網路控制器（控制項路徑）診斷工具，必須先安裝 RSAT-NetworkController 功能和匯入``NetworkControllerDiagnostics``模組：  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

若要使用診斷工具 HNV 診斷（路徑資料），您必須匯入``HNVDiagnostics``模組：
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>網路控制器診斷  
這些 cmdlet 所記載上的[網路控制器診斷 Cmdlet 主題](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/)。 它們協助找出控制路徑 Network Controller 節點間 Network Controller and HYPER-V 主機上執行 NC 主機代理程式及間的網路原則一致性的問題。

 _偵錯-ServiceFabricNodeStatus_和_取得-NetworkControllerReplica_的其中一個 Network Controller 節點虛擬電腦必須執行 cmdlet。 所有其他 NC 診斷 cmdlet 可執行的任何主機已連接到 Network Controller 並在網路控制器管理安全性群組 (Kerberos) 中或管理 Network Controller X.509 憑證的存取。 
   
### <a name="hyper-v-host-diagnostics"></a>HYPER-V 主機診斷]  
這些 cmdlet 所記載上的[HYPER-V 網路模擬 (HNV) 診斷 Cmdlet 主題](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/)。 它們協助找出問題資料路徑之間承租人虛擬電腦（西東日）中的並輸入流量透過 SLB VIP（北日南）。 

_偵錯-VirtualMachineQueueOperation_，_取得-CustomerRoute_，_取得-PACAMapping_、_取得-ProviderAddress_，_取得-VMNetworkAdapterPortId_，_取得-VMSwitchExternalPortId_，和_測試-EncapOverheadSettings_都可以從任何 HYPER-V 主機執行的所有區域測試。 其他 cmdlet 叫用透過網路控制器資料路徑測試，因此必須設為上述 descried Network Controller 的存取。
 
### <a name="github"></a>GitHub
[Microsoft 日 SDN GitHub 存放庫](https://github.com/microsoft/sdn)範例指令碼或工作流程自動化組建這些附隨 cmdlet 上方的號碼。 尤其是診斷指令碼位於[診斷](https://github.com/Microsoft/sdn/diagnostics)資料夾。 請協助我們貢獻提交提取要求，這些指令碼。

## <a name="troubleshooting-workflows-and-guides"></a>疑難排解工作流程和指南  

### <a name="hoster-validate-system-health"></a>[主]驗證系統健康
還有 embedded 的資源名為_設定狀態_在幾個 Network Controller 的資源。 設定狀態提供資訊包括網路 controller 的設定和 HYPER-V 主機上的實際（執行）狀態之間的一致性系統健康。 

若要檢查設定狀態，執行從任何 HYPER-V 主機連接至網路控制器。

>[!NOTE] 
>值為*NetworkController*參數應該 FQDN 或 IP 位址根據 X.509 主體名稱 > 建立 Network Controller 的憑證。
>
>*認證*參數只需要指定控制器網路是否使用 F:kerberos 驗證（一般 VMM 部署）。 網路控制器管理安全性群組中的使用者必須認證。

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

範例設定狀態訊息如下所示：

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
> 有是錯誤，在系統 SLB Mux 傳輸 VM 而的網路介面資源何處失敗狀態的「Virtual 切換-主機無法連接到控制器」錯誤。 如果 IP 設定，在 VM NIC 資源轉送邏輯網路的 IP 集區中設定為 [IP 位址可以放心地忽略此錯誤。 有系統閘道 HNV 提供者 VM Nic 的網路介面資源何處失敗狀態發生錯誤」切換 Virtual-PortBlocked」中是第二個錯誤。 這個錯誤，可也放心地忽略設定 IP 設定，在 VM NIC 資源至空值（來設計）。


下表顯示錯誤碼、簡訊及後續動作上觀察到的組態狀態才會根據的清單。

  
| **程式碼**| **訊息**| **控制項目**|  
|:--------:|:-----------:|----------:|  
| 不明| 未知的錯誤| |  
| HostUnreachable                       | 不到主機 | 檢查網路控制器和主機間管理網路連接 |  
| PAIpAddressExhausted                  | 用盡 PA Ip 位址 | 增加 HNV 提供者邏輯子網路的 IP 集區大小 |  
| PAMacAddressExhausted                 | 用盡 PA Mac 位址 | 增加 Mac 集區範圍 |  
| PAAddressConfigurationFailure         | 無法配管到主機 PA 地址 | 檢查網路控制器和主機間管理網路連接。 |  
| CertificateNotTrusted                 | 不受信任的憑證  |修正用於與主機通訊的憑證。 |  
| CertificateNotAuthorized              | 未取得授權的憑證 | 修正用於與主機通訊的憑證。 |  
| PolicyConfigurationFailureOnVfp       | 在 [設定原則 VFP 失敗 | 這是執行階段失敗。  未明確工作替。 會收集登。 |  
| PolicyConfigurationFailure            | 在主機，因為通訊失敗或其他具備原則失敗 NetworkController 時發生錯誤。| 未明確的動作。  這是因為目標狀態處理 Network Controller 單元失敗。 會收集登。 |  
| HostNotConnectedToController          | 主機尚未連接至網路控制器 | 不會套用主機或主機連接埠設定檔不到網路控制器。 驗證 HostID 登錄符合執行個體 ID 伺服器資源 |  
| MultipleVfpEnabledSwitches            | 有多個 VFp 主機上支援選項  | Delete 其中一個選項，因為網路控制器主機代理程式僅支援一個 vSwitch 與 VFP 擴充功能 |  
| PolicyConfigurationFailure            | 無法 VmNic 因為憑證錯誤或連接錯誤的推播 VNet 原則  | 檢查是否已部署適當的憑證（憑證主體名稱必須符合的主機 FQDN）。 也請確認 Network Controller 的主機連接 |  
| PolicyConfigurationFailure            | 無法 VmNic 因為憑證錯誤或連接錯誤的推播 vSwitch 原則  | 檢查是否已部署適當的憑證（憑證主體名稱必須符合的主機 FQDN）。 也請確認 Network Controller 的主機連接|
| PolicyConfigurationFailure            | 無法 VmNic 因為憑證錯誤或連接錯誤的推播防火牆原則 | 檢查是否已部署適當的憑證（憑證主體名稱必須符合的主機 FQDN）。 也請確認 Network Controller 的主機連接|
| DistributedRouterConfigurationFailure | 無法在主機但 vNic 設定分散路由器設定                          | TCPIP 堆疊時發生錯誤。 可能需要在伺服器的此錯誤 PA 和 DR 主機 vNICs 清理 |
| DhcpAddressAllocationFailure          | 失敗 VMNic DHCP 位址配置                                                    | 檢查是否已在 NIC 資源靜態 IP 位址屬性 |  
| CertificateNotTrusted<br>CertificateNotAuthorized | 因為網路或憑證錯誤連接 Mux 失敗 | 請提供錯誤碼的訊息中的數字的程式碼：這 winsock 錯誤碼對應。 憑證錯誤的細微 (例如，無法驗證憑證，未取得授權的憑證，。) |  
| HostUnreachable                       | MUX 是 Unhealthy（常見是 BGPRouter 中斷） | BGP 等 RRAS（BGP 一樣）或切換上架 (ToR) 就無法存取不等成功。 檢查 BGP 上軟體負載平衡器多工器資源 BGP 等（ToR 或 RRAS 一樣）的設定 |  
| HostNotConnectedToController          | 未連接 SLB 主機代理程式  | 檢查服務 SLB 主機代理程式正在執行;參考 SLB 主機代理程式登（自動執行）原因為何，以方便 SLBM (NC) 拒絕出示主機代理程式執行憑證狀態將會顯示 nuanced 的資訊  |  
| PortBlocked                           | 封鎖 VFP 連接埠，因為 VNET 缺乏日 ACL 原則 | 檢查是否有任何其他錯誤，可能會導致原則不會設定此。 |  
| 多載                            | 為多載 Loadbalancer MUX  | MUX 效能問題 |  
| RoutePublicationFailure               | 未連接 Loadbalancer MUX BGP 路由器 | 檢查是否 MUX 連接 BGP 路由器和的 BGP 外面正確設定 |  
| VirtualServerUnreachable              | Loadbalancer MUX 未連接 SLB 管理員 | 檢查 SLBM 之間 MUX 連接 |  
| QosConfigurationFailure               | 若要設定 QOS 原則失敗 | 查看是否有可用的所有 VM 如果 QOS 保留的項目使用不足頻寬 |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>檢查網路連接之間網路控制器上並 HYPER-V 主機（NC 主機專員服務）
執行*netstat*下方驗證有三種 ESTABLISHED 連接之間 NC 主機代理程式及 Network Controller 節點 HYPER-V 主機上的一個聆聽通訊端命令
- 連接埠 TCP:6640 HYPER-V 主機（NC 主機代理程式服務）上聆聽
- 有兩個來自建立的 HYPER-V 主機連接埠 6640 NC 節點 IP 暫時連接埠 (> 32000) 上的 IP
- 其中一個建立連接 HYPER-V 主機 IP 暫時連接埠上從網路控制器其餘 IP 6640 連接埠

>[!NOTE]
>可能只有兩種 HYPER-V 主機上建立的連接是否有任何承租人虛擬機器該特定主機上部署。

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>檢查服務主機代理程式
HYPER-V 主機上的兩個主機專員服務通訊的網路控制器：SLB 主機代理程式和 NC 主機代理程式。 很可能的一個或兩個這些服務無法執行。 檢查其狀態，並重新開機如果他們無法執行。

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>查看網路控制器的健康
是否有不三個 ESTABLISHED 連接或會出現無法回應時應網路控制器，請檢查所有節點和服務模組，使用下列 cmdlet 也都會恢復並執行。 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
網路控制器服務模組︰
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

檢查 ReplicaStatus 的**準備**HealthState 是**[確定]**。

在 production 部署是多節點網路控制器，您也可以查看每個服務的主要在哪一個節點，其個人複本狀態。

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
檢查複本狀態是準備為每個服務。
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>檢查有對應 HostIDs 和網路控制器與每個 HYPER-V 主機之間的憑證 
HYPER-V 主機，執行下列命令，查看 HostID 的伺服器上的資源 Network Controller 的執行個體 id 對應

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

*補救*如果使用 SDNExpress 指令碼或手動部署，更新符合伺服器資源的執行個體 Id 登錄 HostId 機。 如果使用 VMM，從 VMM delete HYPER-V Server 重新開機 HYPER-V 主機（實體伺服器）上的網路控制器主機代理程式，並移除 HostId 登錄金鑰。 然後，重新新增到 VMM 伺服器。


檢查 x.509 HYPER-V 主機（主機會憑證的主體名稱）用於 (SouthBound) 通訊 HYPER-V 主機（NC 主機專員服務）和 Network Controller 節點之間的憑證碼相同。 同時檢查 [Network Controller 的其餘部分憑證已主體名稱*DATA-CN =<FQDN or IP>*。

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

您也可以查看下列的每個憑證，以確定主體名稱是參數（主機 NC 其他 FQDN 或 IP），如預期般尚未到期憑證，並的受信任的根授權中所包含的所有憑證授權單位」中的憑證鏈結。

- 主體名稱  
- 到期  
- 信任的根授權  

*補救*多個憑證有相同的主體名稱 HYPER-V 主機上，如果網路控制器主機代理程式會隨機閃爍的問題選擇一個來向網路控制器。 這可能不符合指紋的資源已知的網路控制站伺服器。 在這種情形下，其中一個相同主體名稱 HYPER-V 主機上的憑證 delete，然後重新開始網路控制器主機代理程式服務。 如果可以仍然連接，delete 相同主體名稱 HYPER-V 主機上的另一個憑證，並 delete 對應伺服器資源 VMM 中。 然後，重新建立 VMM 將產生 X.509 新的憑證，並 HYPER-V 主機上安裝在伺服器資源。
  

#### <a name="check-the-slb-configuration-state"></a>檢查 SLB 設定狀態
可以判斷 SLB 設定狀態做為輸出 Debug-NetworkController cmdlet 的一部分。 這個 cmdlet 也會輸出目前 Network Controller 資源 JSON 檔案、所有 IP 組態的每個 HYPER-V 主機（伺服器）的區域網路原則從主機代理程式資料庫表格中的設定。 

根據預設，將會收集其他追蹤。 不會收集追蹤來新增-IncludeTraces: $false 參數。

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>預設的輸出位置將會 < working_directory > \NCDiagnostics\ directory。 變更預設的輸出 directory 可以使用`-OutputDirectory`的參數。 

中找到 SLB 設定狀態的資訊_診斷-slbstateResults.Json_在此 directory 中的檔案。

這個 JSON 檔案可分成下列的區段：
 * Fabric
   * SlbmVips-本節列出使用 coodinate 設定和 SLB Muxes 之間 SLB 主機代理程式健康 Network Controller 的 SLB 管理員 VIP 地址的 IP 位址。
   * MuxState-本節會列出一個值的提供的狀態 mux 每個 SLB Mux 部署
   * 路由器設定的這個區段會列出上游路由器的（BGP 等）獨立系統 (ASN)，傳送的 IP 位址和 id。 它也會列出 SLB Muxes ASN 與傳輸 IP。
   * 連接主機資訊的這個區段會清單管理 IP 地址所有可用來執行負載平衡工作負載 HYPER-V 主機。
   * Vip 範圍-本章節也會列出的公開和私人 VIP IP 集區範圍。 SLBM VIP 將作為從這些範圍的其中一個 IP 配置。 
   * Mux 路徑-本節會列出一個值的包含所有的該特定 mux 路由廣告的每個 SLB Mux 部署。
 * 承租人
   * VipConsolidatedState-本章節也會列出連接狀態的每個承租人 VIP，包括廣告的路由首碼、HYPER-V 主機和 DIP 端點。
    
> [!NOTE]
> SLB 狀態都可以利用直接確定[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)上的指令碼[Microsoft SDN GitHub 存放庫](https://github.com/microsoft/sdn)。 

#### <a name="gateway-validation"></a>閘道驗證

**從 Network Controller:**
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

**從最上面的架 (ToR) 切換：**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP 路由器**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

除了這些項目，我們已經看過為止（尤其是根據 SDNExpress 部署），在問題的承租人區間 GW Vm 上尚未取得設定的最常見原因似乎的 FabricConfig.psd1 GW 容量較少比較他人嘗試指派給網路連接（S2S 通道）TenantConfig.psd1 的事實。 這都可以比較下列命令的輸出輕鬆地檢查：

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[主]驗證資料平面
部署 Network Controller、建立承租人 virtual 網路和子網路，並附加到 virtual 子網路 Vm 之後，可以執行其他 fabric 層級測試，以檢查承租人連接項。

#### <a name="check-hnv-provider-logical-network-connectivity"></a>檢查 HNV 提供者邏輯網路連接
之後的第一個來賓執行 HYPER-V 主機上 VM 已連接到承租人 virtual 網路，Network Controller 將 HYPER-V 主機指派兩個 HNV 提供者 IP 位址（PA IP 位址）。 這些程式將來自 HNV 提供者邏輯網路的 IP 集區與受網路控制器。  若要找出這兩個 HNV IP 位址的

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

這些 HNV 提供者 IP 位址 (PA IPs) 已指派給建立不同的 TCPIP 網路區間乙太網路卡和的介面卡名稱_VLANX_ X 是 VLAN 指派給 HNV 提供者（傳輸）邏輯網路。

連接兩個 HYPER-V 主機使用 HNV 提供者可以使用其他區間 ping 進行邏輯網路之間 (-c Y) 參數，Y 所在中建立 PAhostVNICs TCPIP 網路區間。 可以判斷這區間執行：

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
> PA 主機但 vNIC 介面卡未使用的資料路徑中，所以不需要 IP 指派給「vEthernet (PAhostVNic) 介面卡]。

例如，假設 HYPER-V 主機 1 到 2 有 HNV 提供者 (PA) IP 位址：

|-HYPER-V 主機-|-PA IP 位址 1|-PA IP 位址 2|
|---             |---            |---             |
|主機 1 | 10.10.182.64 | 10.10.182.65 |
|主機 2 | 10.10.182.66 | 10.10.182.67 |

我們可以使用下列命令查看 HNV 提供者邏輯網路連接兩個之間 ping。

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

*補救*如果 HNV 提供者 ping 無法運作，檢查您的實體網路連接包括 VLAN 設定。 主幹模式，應該是每個 HYPER-V 主機上的實體 Nic 有任何特定的 VLAN 指派。 管理主機但 vNIC 應該和管理邏輯網路 VLAN 分離。

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>檢查 MTU 和巨大框架 HNV 提供者邏輯網路支援

另一個常見邏輯 HNV 提供者網路中的問題會的實體網路連接埠和/或乙太網路卡不需要設定為處理 VXLAN（或 NVGRE）封裝負擔大 MTU。 
>[!NOTE]
> 有些乙太網路卡和驅動程式支援的新 * EncapOverhead 關鍵字，將會自動設定為 160 主機的網路控制器代理程式。 這個值，然後會新增至的值 * JumboPacket 關鍵字總和其做為 MTU 通知。
> 例如 * EncapOverhead = 160 和 * JumboPacket = 1514 年 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

若要測試是否 HNV 提供者邏輯網路支援大 MTU 大小的端點使用_測試-LogicalNetworkSupportsJumboPacket_ cmdlet:
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

*修復*
* 調整大小 MTU 實體切換連接埠，至少會在 1674B（包括 14B 乙太網路標頭和結尾）
* 如果您的卡片 NIC 不支援的 EncapOverhead 關鍵字，調整至少為 JumboPacket 關鍵字 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>檢查承租人 VM NIC 連接
指派給來賓 VM 每個 VM NIC 有私人客戶地址 (CA) 和 HNV 提供者地址 (PA) 空間之間的 CA-PA 對應。 這些對應會保持在每個 HYPER-V 主機 OVSDB 伺服器表格中，執行下列 cmdlet 可以找到。

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
> 如果指定房客 VM 的不輸出您預期的 CA-PA 對應，請檢查 VM 網路介面卡、IP 設定資源 Network Controller 使用_取得-NetworkControllerNetworkInterface_ cmdlet。 此外，檢查 NC 主機代理程式和網路控制器節點間建立的連接。

使用此資訊，請承租人 VM ping 可以立即將車載機起始來從網路控制器使用項_測試-VirtualNetworkConnection_ cmdlet。

## <a name="specific-troubleshooting-scenarios"></a>特定疑難排解案例

下列章節提供指導方針進行疑難排解特定的案例。

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>有兩個承租人虛擬電腦之間不網路連接

1.  [承租人]確定 Windows 防火牆承租人虛擬電腦中不會封鎖流量。  
2.  [承租人]檢查您的 IP 位址已指派給承租人一樣執行_ipconfig_。 
3.  [主]執行**測試-VirtualNetworkConnection** HYPER-V 主機驗證有問題的兩個承租人虛擬電腦之間連接。 

>[!NOTE]
>VSID 指的是 Virtual 子網路編號。 如果是 VXLAN，這是 VXLAN 網路識別碼 (VNI)。 您可以找到這個值執行**取得-PACAMapping** cmdlet。

#### <a name="example"></a>範例

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

建立的主機上 [sa18n30-2.sa18.nttest.microsoft.com」管理 IP 的 10.127.132.153 ListenerCA ip 192.168.1.5 這兩個附加到 Virtual 子網路 (VSID) 4114 的 192.168.1.4 SenderCA IP CA-ping 之間」遺漏 Web VM 1」。

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

開始 CA 空間 ping 測試從開始到 192.168.1.5 追蹤工作階段 Ping 成功電子郵件地址 192.168.1.4 Rtt = 0 ms


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

4.  [承租人]檢查有 virtual 子網路或封鎖流量 VM 網路介面指定不分散式的防火牆原則。    

查詢 sa18.nttest.microsoft.com 網域中找到 sa18n30nc 示範環境中網路控制器 REST API。

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>尋找 IP 設定，這參考此 ACL Virtual 子網路。

1. [主]執行``Get-ProviderAddress``在兩個 HYPER-V 主機裝載兩個承租人虛擬有問題的電腦，然後執行``Test-LogicalNetworkConnection``或``ping -c <compartment>``HYPER-V 主機驗證連接 HNV 提供者的邏輯網路上的
2.  [主]確保 MTU 設定正確 HYPER-V 主機上，以及任何層級 2 切換空行 HYPER-V 主機裝置。 執行``Test-EncapOverheadValue``上所有 HYPER-V 主機有問題。 也請查看所有層級 2 切換中的有 MTU 為最低 1674 位元組的最大的 160 位元組的費用。  
3.  [主]如果未出現 PA IP 位址和/或 CA 連接損壞，檢查以確定已收到的網路原則。 執行``Get-PACAMapping``以查看是否封裝規則 CA-PA 對應建立覆疊 virtual 網路所需的正確建立及。  
4.  [主]檢查網路控制器主機代理程式連接至網路控制器。 執行``netstat -anp tcp |findstr 6640``以查看是否   
5.  [主]檢查主機 ID 中 HKLM / 符合伺服器資源裝載承租人虛擬電腦的執行個體來電顯示。  
6. [主]連接埠設定檔 ID 符合 VM 網路介面承租人虛擬電腦的執行個體 ID 檢查。  

## <a name="logging-tracing-and-advanced-diagnostics"></a>登入，追蹤進階診斷]

下列章節登入，和追蹤進階診斷提供的資訊。

### <a name="network-controller-centralized-logging"></a>網路控制器集中登入 
 
Network Controller 可自動登偵錯工具會收集並將它們儲存在中央位置。 當您第一次，或任何時候稍後部署 Network Controller 時，可登入的收藏。 登的網路控制器，請從所收集和網路由 Network Controller 的項目︰ 裝載電腦、軟體負載平衡器 (SLB) 和閘道電腦。 

這些登包含 Network Controller 叢集、網路控制器應用程式、閘道登、SLB、virtual 網路和分散式的防火牆偵錯登。 只要新增了一個新主機日 SLB 日閘道 Network Controller，登入被開始在這些電腦上。 同樣地，從網路控制器移除主機日 SLB 日閘道之後，登入停止在這些電腦上。

#### <a name="enable-logging"></a>讓登入

安裝 Network Controller 叢集使用時，會自動支援登入**安裝-NetworkControllerCluster** cmdlet。 根據預設，登會在本機上收集 Network Controller 節點在*%systemdrive%\SDNDiagnostics*。 這是**建議在**您變更是（不是本機）遠端檔案共用此位置。 

Network Controller 叢集登會儲存在*%programData%\Windows Fabric\log\Traces*。 您可以指定中央的位置登入收集使用**DiagnosticLogLocation**與建議，這也是參數會遠端檔案共用。 

如果您想要限制這位置的存取，您可以存取憑證的**LogLocationCredential**的參數。 如果您提供的認證存取位置登入，您也應該提供**CredentialEncryptionCertificate**用來儲存在本機上 Network Controller 節點認證加密的參數。  

使用預設設定，建議您有 [本機] 節點中中央位置 25 GB 可用空間至少 75 GB（如果未使用的中央位置）叢集 3 個節點 Network Controller 的。

#### <a name="change-logging-settings"></a>變更登入設定

您可以變更在任何時間使用的登入設定``Set-NetworkControllerDiagnostic``cmdlet。 變更下列設定：

- **中央位置登入**。  您可以變更所有登，儲存的位置``DiagnosticLogLocation``的參數。  
- **若要存取的位置登入認證**。  您可以變更的憑證存取登入的位置，以``LogLocationCredential``的參數。  
- **移到本機登入**。  如果您有提供的集中的登的存放位置，您可以移至本機 Network Controller 的節點上登入``UseLocalLogLocation``（不建議因為大型磁碟空間需求）的參數。  
- **登入範圍**。  根據預設，所有登會都收集。 您可以變更收集只 Network Controller 叢集登範圍。  
- **登入層級**。  預設值登入為資訊。 您可以變更錯誤、警告，或詳細資訊。  
- **過時的時間登入**。  登會儲存在循環的方式。 無論您使用登入本機集中登入，您將必須預設 3 天的登入的資料。 您可以變更此時間限制，使用**LogTimeLimitInDays**的參數。  
- **過時大小登入**。  根據預設，您將可以登入資料的最大 75 GB 如果使用的登入本機使用打造登入和 25 GB。 您可以變更此限制的**LogSizeLimitInMBs**的參數。

#### <a name="collecting-logs-and-traces"></a>收集登和追蹤

VMM 部署使用預設 Network Controller 集中登入。 部署 Network Controller 服務範本時指定檔案共用這些登的位置。

如果檔案的位置已指定的本機登入將每個 Network Controller 節點上使用儲存在 C:\Windows\tracing\SDNDiagnostics 登的。 使用下面階層儲存這些登：

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- 追蹤

Network Controller 使用 (Azure) 服務 Fabric。 特定的問題進行疑難排解時，可能需要服務 Fabric 登。 這些登上可找到 C:\ProgramData\Microsoft\Service Fabric 在每個 Network Controller 節點。

如果使用者執行_偵錯-NetworkController_ cmdlet，其他登將會提供指定的伺服器中的資源 Network Controller 的每個 HYPER-V 主機上。 這些登（和追蹤如果支援）會保持在 C:\NCDiagnostics

### <a name="slb-diagnostics"></a>SLB 診斷

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>SLBM Fabric 錯誤（裝載服務提供者動作）

1.  檢查運作的軟體負載平衡器管理員 (SLBM) 和，協調流程層級可以才能互相溝通：SLBM SLB Mux]-> [與 SLBM]-> [SLB 主機代理程式。 執行[DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1)的存取權的網路控制器其餘 Endpoint 任何節點。  
2.  確認*SDNSLBMPerfCounters*中效能在其中的網路控制器節點 Vm（最好的主要 Network Controller 節點-Get-NetworkControllerReplica）：
    1.  已連接到 SLBM 負載平衡器 (LB)」引擎嗎？ (*SLBM LBEngine 設定總計*> 0)  
    2.  SLBM 至少知道自己的端點嗎？ (*VIP 端點總計*> = 2)  
    3.  連接到 SLBM HYPER-V (DIP) 主機嗎？ (*HP 戶端連接*= num 伺服器)   
    4.  已連接到 Muxes SLBM 嗎？ (*Muxes 連接* == *上 SLBM 健康 Muxes* == *Muxes 報告健康*= # SLB Muxes Vm)。  
3.  請確定 SLB MUX 使用已成功對等 BGP 路由器設定  
    1.  如果您使用遠端存取權（也就是 BGP 一樣）RRAS:  
        1.  Get-BgpPeer 應該會顯示在連接  
        2.  Get-BgpRouteInformation 應該會顯示 SLBM 至少路由自我 VIP  
    2.  如果使用實體頂端的位架 (ToR) 切換為 BGP 等，請洽詢您的文件  
        1.  例如: # 顯示 bgp 執行個體  
4.  確認*SlbMuxPerfCounters*和*SLBMUX*中效能 SLB Mux VM 上計數器
5.  檢查設定狀態和 VIP 範圍軟體負載平衡器管理員資源  
    1.  Get-NetworkControllerLoadBalancerConfiguration-ConnectionUri < https://<FQDN or IP>|convertto-json-深度 8 (檢查 VIP 範圍 IP 集區中的，確定 SLBM 自我-VIP (*LoadBalanacerManagerIPAddress*)，在這些範圍任何承租人面向 Vip)  
        1. Get-NetworkControllerIpPool NetworkId」< 公開私密金鑰日 VIP 邏輯網路資源 ID > [-SubnetId」< 公開私密金鑰日 VIP 邏輯子網路資源 ID > [預設-]<IP Pool Resource Id>「-ConnectionUri $uri | convertto-json-深度 8 
    2.  Debug-NetworkControllerConfigurationState-  

如果有任何上述無法檢查、承租人 SLB 狀態也會失敗模式。  

**修復**   
依據所顯示的下列診斷資訊，修正下列動作：  
* 確定已連接 SLB Multiplexers  
  * 修正憑證問題  
  * 修正網路連接的問題  
* 請確定已成功設定 BGP 等資訊  
* 確保主機 ID 登錄符合伺服器資源.執行個體 (參考附錄適用於*HostNotConnected*錯誤碼)  
* 會收集登  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>SLBM 承租人錯誤（裝載服務提供者和承租人動作）

1.  [主]查看*偵錯-NetworkControllerConfigurationState*以查看是否任何 LoadBalancer 資源發生錯誤。 請嘗試下列待辦事項表格附錄來降低。   
    1.  檢查有與廣告路徑 VIP 端點  
    2.  檢查端點 VIP 發現多少 DIP 端點  
2.  [承租人]驗證正確指定負載平衡器資源  
    1.  驗證 DIP 裝載承租人虛擬機器對應至 LoadBalancer 後端地址集區 IP 設定，這登記 SLBM 中的端點  
3.  [主]如果發現不會或連接 DIP 端點：   
    1.  查看*偵錯-NetworkControllerConfigurationState*  
        1.  驗證該 NC 和 SLB 主機代理程式成功是連接到使用網路控制器事件協調器 ``netstat -anp tcp |findstr 6640)``  
    2.  查看*HostId*在*nchostagent*服務 regkey (參考*HostNotConnected*中附錄錯誤碼) 符合對應伺服器資源的執行個體 Id (``Get-NCServer |convertto-json -depth 8``)  
    3.  檢查一樣連接埠連接埠設定檔 id 符合對應一樣 NIC 資源的執行個體 Id   
4.  [裝載提供者]會收集登   

#### <a name="slb-mux-tracing"></a>SLB Mux 追蹤

也可以透過事件檢視器判斷軟體負載平衡器 Muxes 資訊。 
1. 按一下 [顯示分析及偵錯登」在事件檢視器檢視功能表
2. 瀏覽到 [應用程式與服務登「> Microsoft > Windows > SlbMuxDriver > 沿著湖邊繪製事件檢視器 
3. 在其上按一下滑鼠右鍵，然後選取 [讓登入]

>[!NOTE]
>我們建議您只需要一段時間當您嘗試重現問題支援此登入

### <a name="vfp-and-vswitch-tracing"></a>VFP 和 vSwitch 追蹤

從任何 HYPER-V 主機的裝載來賓 VM 附加到承租人 virtual 網路，您也可以收集 VFP 追蹤以判斷位置座落問題可能。

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
