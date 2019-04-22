---
title: SDN 的內部 DNS 服務 (iDNS)
description: 本主題說明如何使用整合了 Windows Server 2016 中的軟體定義網路功能的內部 DNS (Idn)，為您託管的租用戶工作負載提供 DNS 服務。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824079"
---
# <a name="internal-dns-service-idns-for-sdn"></a>SDN 的內部 DNS 服務 (iDNS)

>適用於：Windows Server （半年通道），Windows Server 2016

如果您為雲端服務提供者\(CSP\)或計劃部署軟體定義網路的企業\(SDN\)在 Windows Server 2016 中，您可以提供 DNS 服務，為您託管的租用戶工作負載使用內部 DNS \(Idn\)，已向 SDN。

裝載虛擬機器\(Vm\)和應用程式需要在自己的網路中，以及與網際網路上的外部資源進行通訊的 DNS。 使用的 iDNS，您可以使用 DNS 名稱解析服務提供租用戶，其隔離、 本機的命名空間以及網際網路資源。

IDNS 服務不能從租用戶虛擬網路存取，因為以外透過 Idn 的 proxy，伺服器不會受到影響的租用戶網路上的惡意活動。

**主要功能**

以下是重要為 Idn 功能。

- 提供共用的 DNS 名稱解析服務，租用戶工作負載
- 名稱解析和租用戶名稱空間中的 DNS 登錄的權威 DNS 服務
- 從租用戶 Vm 的網際網路名稱解析的遞迴 DNS 服務。
- 如有需要，您可以設定同時裝載的網狀架構和租用戶名稱
- 符合成本效益的 DNS 解決方案-租用戶不需要部署他們自己的 DNS 基礎結構
- 使用 Active Directory 整合，這是必要的高可用性。

除了這些功能，如果您擔心如何保持您的 AD 整合的 DNS 伺服器開啟到網際網路，您可以部署在周邊網路中的另一個遞迴解析程式背後的 Idn 伺服器。

IDNS 是集中式的伺服器所有的 DNS 查詢，因為 CSP 或企業也可以實作租用戶 DNS 防火牆、 套用篩選、 偵測惡意活動，和稽核在中央位置的交易

## <a name="idns-infrastructure"></a>iDNS 基礎結構
IDNS 基礎結構包含 Idn 伺服器和 Idn proxy。

### <a name="idns-servers"></a>iDNS 伺服器
iDNS 包含一組裝載租用戶專屬的資料，例如 VM 的 DNS 資源記錄的 DNS 伺服器。

iDNS 伺服器都是其內部的 DNS 區域，授權伺服器，而且也可做為 公用名稱的解析程式當租用戶 Vm 會嘗試連接到外部資源。

所有虛擬網路上 Vm 的主機名稱會儲存為 DNS 資源記錄，在相同的區域。 比方說，如果您部署的區域，名為 contoso.local 的 Idn，該網路上 Vm 的 DNS 資源記錄會儲存 [contoso.local] 區域中。

租用戶 VM 完整網域名稱\(Fqdn\)包含 GUID 格式的虛擬網路的電腦名稱和 DNS 後置詞字串。 例如，如果您有一個租用戶 VM 位於虛擬網路的本機，contoso 的 TENANT1 VM 的 FQDN 會是 TENANT1。*vn guid*。 contoso.local，其中*vn guid*是虛擬網路的 DNS 後置詞字串。

>[!NOTE]
>如果您是網狀架構系統管理員，您可以使用 CSP 或企業的 DNS 基礎結構，如為 Idn 使用 Idn 的伺服器，而不需要部署新的 DNS 伺服器，專為伺服器。 無論您部署新的 Idn 的伺服器，或使用現有的基礎結構，Idn 會依賴 Active Directory，以提供高可用性。 因此必須與 Active Directory 整合您 Idn 的伺服器。

### <a name="idns-proxy"></a>iDNS Proxy
iDNS proxy 是在每個主機上，執行和其租用戶虛擬網路 DNS 將流量轉送至 Idn 伺服器 Windows 服務。

下圖說明從透過 Idn 伺服器的 Idn proxy 的租用戶虛擬網路和網際網路的 DNS 流量路徑。

![iDNS 基礎結構](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 Idn
當您使用指令碼來部署 Windows Server 2016 中的 SDN 時，Idn 會自動包含在您的部署。

如需詳細資訊，請參閱下列主題。

- [部署軟體定義網路基礎結構使用指令碼](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>了解 Idn 的部署步驟
您可以使用本節來了解如何安裝和設定，當您部署使用指令碼的 SDN Idn。

以下是部署 Idn 所需步驟的摘要。

>[!NOTE]
>如果您使用指令碼部署 SDN，您不需要執行任何步驟執行。 提供的步驟資訊和疑難排解的用途。

### <a name="step-1-deploy-dns"></a>步驟 1：部署 DNS
您可以使用下列的範例 Windows PowerShell 命令，以部署的 DNS 伺服器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>步驟 2：設定網路控制卡中的 iDNS 資訊
此指令碼區段是 REST 呼叫所做的系統管理員對網路控制站，通知需 Idn 的區域設定-例如 iDNSServer 和區域是用來裝載 Idn 名稱的 IP 位址。 

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
>這是摘錄自一節**組態 ConfigureIDns** SDNExpress.ps1 中。 如需詳細資訊，請參閱 <<c0> [ 部署軟體定義網路基礎結構使用指令碼](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>步驟 3：設定 Idn Proxy 服務
IDNS Proxy 服務會執行每個 HYPER-V 主機，提供租用戶虛擬網路與 Idn 伺服器的所在位置的實體網路之間的橋樑。 必須在每個 Hyper-v 主機上建立下列登錄機碼。


**DNS 連接埠：** 固定的連接埠 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**DNS Proxy 連接埠：** 固定的連接埠 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP:** 網路介面上設定，如果租用戶選擇使用 Idn 服務的固定的 IP 位址

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType ="String"

        
**Mac 位址：** DNS 伺服器的媒體存取控制位址

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = “aa-bb-cc-aa-bb-cc”
- ValueType ="String"

**IDNS 伺服器位址：** 以逗號分隔 Idn 的伺服器的清單。

- 登錄機碼：HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Forwarders"
- ValueData = “10.0.0.9”
- ValueType ="String"



>[!NOTE]
>這是摘錄自一節**組態 ConfigureIDnsProxy** SDNExpress.ps1 中。 如需詳細資訊，請參閱 <<c0> [ 部署軟體定義網路基礎結構使用指令碼](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步驟 4：重新啟動網路控制站主機代理程式服務
您可以使用下列 Windows PowerShell 命令重新啟動網路控制卡主機代理程式服務。
    
    Restart-Service nchostagent -Force
    
如需詳細資訊，請參閱 < [Restart-service](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>啟用 DNS proxy 服務的防火牆規則
您可以使用下列 Windows PowerShell 命令來建立防火牆規則，可讓例外狀況，要與 VM 和 Idn 伺服器進行通訊的 proxy。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

如需詳細資訊，請參閱 < [Enable-netfirewallrule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>驗證服務的 iDNS
若要驗證 Idn 服務，您必須部署範例租用戶工作負載。

如需詳細資訊，請參閱 <<c0> [ 建立 VM 並連線到租用戶虛擬網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果您想的租用戶 VM 使用 Idn 服務時，您必須將 VM 網路介面的 DNS 伺服器設定保留為空白，並允許使用 DHCP 的介面。 

起始與這類網路介面的 VM 之後，它會自動接收組態，以允許使用 Idn，VM 和 VM 會立即開始使用 Idn 服務執行名稱解析。

如果您設定的租用戶 VM 使用 Idn 服務藉由將網路介面的 DNS 伺服器與替代的 DNS 伺服器資訊留為空白時，網路控制站提供 IP 位址的 VM，並代表使用 iDNS 伺服器 VM 中執行的 DNS 名稱登錄. 

網路控制站也會通知 Idn proxy vm 執行名稱解析的 VM 和必要的詳細資料。 

當 VM 起始 DNS 查詢時，proxy 會做為 Idn 服務從虛擬網路之查詢的轉寄站。 

DNS proxy 也可確保租用戶 VM 查詢隔離。 如果 Idn 的伺服器是查詢中，授權 Idn 伺服器以 「 權威的回應。 如果 Idn 伺服器不授權查詢，它會執行解析網際網路名稱的 DNS 遞迴函式。

>[!NOTE]
>此資訊會包含一節**組態 AttachToVirtualNetwork** SDNExpressTenant.ps1 中。 如需詳細資訊，請參閱 <<c0> [ 部署軟體定義網路基礎結構使用指令碼](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

