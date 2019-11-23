---
title: SDN 的內部 DNS 服務 (iDNS)
description: 本主題說明如何使用內部 DNS （Idn）（與 Windows Server 2016 中的軟體定義網路整合），為您的託管租使用者工作負載提供 DNS 服務。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a7e5aa9e1ae7442c706c1bdbdb56d65234fe5ae8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405969"
---
# <a name="internal-dns-service-idns-for-sdn"></a>SDN 的內部 DNS 服務 (iDNS)

>適用於：Windows Server (半年通道)、Windows Server 2016

如果您工作的雲端服務提供者 \(CSP\) 或企業計畫要在 Windows Server 2016 中部署軟體定義網路 \(SDN\)，您可以使用與 SDN 整合的內部 DNS \(Idn\)，為裝載的租使用者工作負載提供 DNS 服務。

裝載的虛擬機器 \(Vm\) 和應用程式需要 DNS 在自己的網路內和網際網路上的外部資源進行通訊。 透過 Idn，您可以為租使用者提供其隔離的本機命名空間和網際網路資源的 DNS 名稱解析服務。

由於 Idn 服務無法從租使用者虛擬網路存取，而不是透過 Idn proxy，因此伺服器不容易遭受租使用者網路上的惡意活動。

**主要功能**

以下是 Idn 的主要功能。

- 為租使用者工作負載提供共用的 DNS 名稱解析服務
- 在租使用者命名空間內進行名稱解析和 DNS 註冊的授權 DNS 服務
- 用於解析來自租使用者 Vm 之網際網路名稱的遞迴 DNS 服務。
- 如有需要，您可以設定同時裝載網狀架構和租使用者名稱
- 符合成本效益的 DNS 解決方案-租使用者不需要部署自己的 DNS 基礎結構
- 具有 Active Directory 整合的高可用性，這是必要的。

除了這些功能之外，如果您擔心將 AD 整合式 DNS 伺服器保持在網際網路上開啟，您可以將 Idn 伺服器部署在周邊網路中的另一個遞迴解析程式之後。

因為 Idn 是適用于所有 DNS 查詢的集中式伺服器，所以 CSP 或企業也可以執行租使用者 DNS 防火牆、套用篩選、偵測惡意活動，以及在中央位置審核交易。

## <a name="idns-infrastructure"></a>Idn 基礎結構
Idn 基礎結構包含 Idn 伺服器和 Idn proxy。

### <a name="idns-servers"></a>Idn 伺服器
Idn 包含一組裝載租使用者特定資料的 DNS 伺服器，例如 VM DNS 資源記錄。

Idn 伺服器是其內部 DNS 區域的授權伺服器，當租使用者 Vm 嘗試連線到外部資源時，也可作為公用名稱的解析程式。

虛擬網路上 Vm 的所有主機名稱都會儲存為相同區域下的 DNS 資源記錄。 例如，如果您為名為 contoso 的區域部署 Idn，該網路上 Vm 的 DNS 資源記錄會儲存在 contoso. 本機區域中。

租使用者 VM 的完整功能變數名稱 \(Fqdn\) 由電腦名稱稱和虛擬網路的 DNS 尾碼字串（GUID 格式）組成。 例如，如果您有一個名為 TENANT1 的租使用者 VM （位於虛擬網路 contoso，local），則 VM 的 FQDN 會是 TENANT1。*vn-guid.empty*，其中*vn-guid*是虛擬網路的 DNS 尾碼字串。

>[!NOTE]
>如果您是網狀架構系統管理員，您可以使用您的 CSP 或企業 DNS 基礎結構作為 Idn 伺服器，而不是專門部署新的 DNS 伺服器作為 Idn 伺服器使用。 無論您為 Idn 部署新的伺服器，或是使用現有的基礎結構，Idn 都依賴 Active Directory 來提供高可用性。 因此，您的 Idn 伺服器必須與 Active Directory 整合。

### <a name="idns-proxy"></a>Idn Proxy
Idn proxy 是一種 Windows 服務，它會在每部主機上執行，並將租使用者虛擬網路 DNS 流量轉送至 Idn 伺服器。

下圖說明透過 Idn proxy，將租使用者虛擬網路到 Idn 伺服器和網際網路的 DNS 流量路徑。

![Idn 基礎結構](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 Idn
當您使用腳本在 Windows Server 2016 中部署 SDN 時，Idn 會自動包含在您的部署中。

如需詳細資訊，請參閱下列主題。

- [使用腳本部署軟體定義的網路基礎結構](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>瞭解 Idn 部署步驟
當您使用腳本部署 SDN 時，您可以使用本節來瞭解如何安裝和設定 Idn。

以下是部署 Idn 所需步驟的摘要。

>[!NOTE]
>如果您已使用腳本部署 SDN，則不需要執行任何這些步驟。 僅針對資訊和疑難排解目的提供這些步驟。

### <a name="step-1-deploy-dns"></a>步驟1：部署 DNS
您可以使用下列範例 Windows PowerShell 命令來部署 DNS 伺服器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>步驟2：設定網路控制卡中的 Idn 資訊
此腳本區段是由系統管理員對網路控制站進行的 REST 呼叫，會通知它關於 Idn 區域設定-例如 iDNSServer 的 IP 位址，以及用來裝載 Idn 名稱的區域。 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>這是來自 SDNExpress 的 設定**ConfigureIDns**一節的摘錄。 如需詳細資訊，請參閱[使用腳本部署軟體定義網路基礎結構](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>步驟3：設定 Idn Proxy 服務
Idn Proxy 服務會在每個 Hyper-v 主機上執行，提供租使用者的虛擬網路與 Idn 伺服器所在的實體網路之間的橋樑。 您必須在每個 Hyper-v 主機上建立下列登錄機碼。


**DNS 埠：** 固定通訊埠53

- 登錄機碼 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService」
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**DNS Proxy 埠：** 固定通訊埠53

- 登錄機碼 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService」
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP：** 固定在網路介面上設定的 IP 位址（如果租使用者選擇使用 Idn 服務）

- 登錄機碼 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService」
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Mac 位址：** DNS 伺服器的媒體存取控制位址

- 登錄機碼 = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "aa-bb-cc-aa-bb-cc"
- ValueType = "String"

**Idn 伺服器位址：** 以逗號分隔的 Idn 伺服器清單。

- 登錄機碼： HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "轉寄站"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>這是來自 SDNExpress 的 設定**ConfigureIDnsProxy**一節的摘錄。 如需詳細資訊，請參閱[使用腳本部署軟體定義網路基礎結構](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步驟4：重新開機網路控制站主機代理程式服務
您可以使用下列 Windows PowerShell 命令來重新開機網路控制站主機代理程式服務。
    
    Restart-Service nchostagent -Force
    
如需詳細資訊，請參閱[重新開機服務](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>啟用 DNS proxy 服務的防火牆規則
您可以使用下列 Windows PowerShell 命令來建立防火牆規則，以允許 proxy 的例外狀況與 VM 和 Idn 伺服器進行通訊。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

如需詳細資訊，請參閱[Enable-get-netfirewallrule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>驗證 Idn 服務
若要驗證 Idn 服務，您必須部署範例租使用者工作負載。

如需詳細資訊，請參閱[建立 VM 並聯機至租使用者虛擬網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果您想要讓租使用者 VM 使用 Idn 服務，則必須讓 VM 網路介面的 DNS 伺服器設定保留空白，並允許介面使用 DHCP。 

在啟動具有這類網路介面的 VM 之後，它會自動接收允許 VM 使用 Idn 的設定，而 VM 會使用 Idn 服務立即開始執行名稱解析。

如果您將 [網路介面 DNS 伺服器] 和 [其他 DNS 伺服器資訊] 保留空白，則設定讓租使用者 VM 使用 Idn 服務時，網路控制卡會提供具有 IP 位址的 VM，並使用 Idn 伺服器來代表 VM 執行 DNS 名稱註冊. 

網路控制站也會通知 Idn proxy 關於 VM，以及執行 VM 名稱解析所需的詳細資料。 

當 VM 起始 DNS 查詢時，proxy 會作為從虛擬網路到 Idn 服務之查詢的轉寄站。 

DNS proxy 也可確保租使用者 VM 查詢已隔離。 如果 Idn 伺服器是查詢的授權，則 Idn 伺服器會以授權回應回應。 如果 Idn 伺服器不是查詢的授權，它會執行 DNS 遞迴來解析網際網路名稱。

>[!NOTE]
>這項資訊包含在 SDNExpressTenant 的 設定**AttachToVirtualNetwork**一節中。 如需詳細資訊，請參閱[使用腳本部署軟體定義網路基礎結構](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

