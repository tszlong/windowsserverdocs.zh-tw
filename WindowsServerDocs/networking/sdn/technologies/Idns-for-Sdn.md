---
title: 適用於 SDN 內部的 DNS 服務 (Idn)
description: 本主題解釋如何使用的整合軟體定義 Windows Server 2016 中的 [網路內部 DNS (Idn)，以您裝載的承租人工作負載提供 DNS 服務。
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
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>內部 DNS SDN 服務 \(iDNS\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

如果您使用雲端服務提供者 \(CSP\) 或企業版，並計劃部署 Windows Server 2016 中的軟體定義網路 \(SDN\)，您可以提供您裝載的承租人工作負載 DNS 服務使用內部 DNS \(iDNS\)，SDN 整合。

裝載的虛擬機器 \(VMs\) 和應用程式需要 DNS 通訊在自己的網路和網際網路上的外部資源。 與 Idn，您可以使用 DNS 名稱解析服務提供 tenants 其名稱隔離的本機空間，以及網際網路資源。

因為 Idn 服務無法承租人 Virtual 網路的存取，以外透過 Idn proxy 伺服器不會受到影響惡意承租人網路上的活動。

**功能鍵**

以下是 Idn 按鍵功能。

- 提供共用的 DNS 名稱解析承租人服務工作負載
- 授權 DNS 服務的名稱解析和 DNS 登記中承租人命名空間
- 遞迴 DNS 解析度承租人 Vm 從網際網路名稱服務。
- 如果需要的話，您可以設定同時裝載 fabric 和承租人名稱
- 具成本效益的 DNS 方案部署自己 DNS 基礎結構不需要 tenants
- Active Directory 整合，這是必要的可用性高。

這些功能，除了如果您擔心保留整合您 AD DNS 伺服器開放網際網路，您可以部署 Idn 周邊網路中的另一個遞迴解析背後的伺服器。

因為 Idn 所有 DNS 查詢中央的伺服器，CSP 或企業可以也實作承租人 DNS 防火牆、 套用篩選、 偵測惡意活動，以及稽核中央位置交易

## <a name="idns-infrastructure"></a>Idn 基礎結構
Idn 基礎結構包含 Idn 伺服器和 Idn proxy。

### <a name="idns-servers"></a>Idn 伺服器
Idn 包含一組裝載承租人特定資料，例如 VM DNS 資源記錄 DNS 伺服器。

Idn 伺服器的授權伺服器他們內部 DNS 區域，並也是在公用的名稱解析時承租人 Vm 嘗試連接到外部資源。

所有主機 Vm Virtual 網路上的名稱是 DNS 資源記錄以儲存在相同的時區。 例如您部署 Idn 名 contoso.local 區域，如果的 DNS 資源記錄 vm 在網路上的儲存在 contoso.local 區域。

承租人 VM 完整網域名稱 \(FQDNs\) 包含電腦名稱和 DNS 尾碼字串的格式 GUID Virtual 網路。 例如，如果您有房客名 TENANT1 Virtual 網路 contoso 本機，在 VM VM 的 FQDN 是 TENANT1。*vn guid*。 contoso.local，其中*vn guid*是 Virtual 網路的 DNS 尾碼字串。

>[!NOTE]
>如果您是 fabric 系統管理員，您可以使用您 CSP 或企業 DNS 的基礎結構為 Idn 伺服器，而不是部署新的 DNS 伺服器專為作為 Idn 伺服器。 無論您部署新伺服器 Idn 您使用您現有的基礎結構，Idn 依賴提供可用性 Active Directory 中。 Active Directory 因此必須整合 Idn 伺服器。

### <a name="idns-proxy"></a>Idn Proxy
Idn proxy 是每個主機上, 執行，同時，轉送承租人 Virtual 網路的 DNS 伺服器 Idn 流量的 Windows 服務。

下圖描述 DNS 傳輸路徑透過 Idn proxy 伺服器 Idn 承租人 Virtual 網路和網際網路。

![Idn 基礎結構](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>如何部署 Idn
當您在 Windows Server 2016 SDN 部署使用指令碼時，會自動將 Idn 包含在您的部署。

如需詳細資訊，下列主題。

- [部署軟體定義網路基礎結構使用指令碼，](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>了解 Idn 部署步驟
若要深入了解如何安裝 Idn 和設定部署 SDN 使用指令碼時，您可以使用此一節。

以下是摘要部署 Idn 所需的步驟。

>[!NOTE]
>如果您已透過使用指令碼部署 SDN，您不需要執行這些步驟。 提供資訊和疑難排解僅步驟。

### <a name="step-1-deploy-dns"></a>步驟 1： 部署 DNS
您可以使用 Windows PowerShell 命令下例部署的 DNS 伺服器。
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>步驟 2： 在 Network Controller 設定 Idn 資訊
這個指令碼區段格式不由系統管理員網路控制器，請至 Idn 區域設定-例如 IP 位址 iDNSServer 與用來主機 Idn 名稱區域的相關通知它的其餘部分通話。 

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
>這是區段摘要**設定 ConfigureIDns**中 SDNExpress.ps1。 如需詳細資訊，請查看[部署使用指令碼軟體定義網路基礎結構，](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-3-configure-the-idns-proxy-service"></a>步驟 3： 設定 Idn Proxy 服務
Idn Proxy 服務執行每個 HYPER-V 主機，提供的 tenants virtual 網路和實體網路的 Idn 伺服器的所在位置的橋樑上。 必須在每個 HYPER-V 主機上建立下列登錄按鍵。


**DNS 連接埠：**修正連接埠 53

- 登錄 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService 」
- 停 = 」 連接埠]
- ValueData = 53
- 值鍵入 = [Dword]
       

**DNS Proxy 連接埠：**修正連接埠 53

- 登錄 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService 」
- 停 = 」 ProxyPort 」
- ValueData = 53
- 值鍵入 = [Dword]
        
**DNS IP:**這是修正的 IP 位址設定網路介面，以方便承租人選擇要使用的 Idn 服務。

- 登錄 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService 」
- 停 = 」 IP 」
- ValueData = 」 169.254.169.254 」
- 值鍵入 = 」 字串 」

        
**Mac 位址：**媒體存取控制 DNS 伺服器位址

- 登錄 = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- 停 = 」 MAC 」
- ValueData = 」 aa-bb-cc-aa-bb-cc 」
- 值鍵入 = 」 字串 」

**IDN 伺服器的位址：**以逗號分隔的 Idn 伺服器清單。

- 登錄鍵： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- 停 = 」 轉送程式 」
- ValueData = 」 10.0.0.9 」
- 值鍵入 = 」 字串 」



>[!NOTE]
>這是區段摘要**設定 ConfigureIDnsProxy**中 SDNExpress.ps1。 如需詳細資訊，請查看[部署使用指令碼軟體定義網路基礎結構，](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>步驟 4： 重新開機網路控制器主機專員服務
若要重新開機網路控制器主機代理程式服務，您可以使用下列 Windows PowerShell 命令。
    
    Restart-Service nchostagent -Force
    
如需詳細資訊，請查看[重新開機服務](https://technet.microsoft.com/library/hh849823.aspx)。

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>讓免 DNS proxy 服務
您可以使用下列的 Windows PowerShell 命令來建立防火牆規則允許例外 proxy VM 與 Idn 伺服器通訊。
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

如需詳細資訊，請查看[讓-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx)。
    
### <a name="validate-the-idns-service"></a>驗證 Idn 服務
若要驗證 Idn 服務，您必須部署範例承租人工作負載。

如需詳細資訊，請查看[建立 VM 和連接到承租人 Virtual 網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。

如果您想要承租人 VM 使用 Idn 服務，您必須留 VM 網路介面 DNS 伺服器設定，並允許使用 DHCP 介面。 

起始 VM 的網路介面之後，它會自動接收可使用 Idn，VM 的設定，並 VM 立即開始使用 Idn 服務執行的名稱解析。

如果您設定承租人 VM 使用 Idn 服務留下空白網路介面 DNS 伺服器] 及 [其他 DNS 伺服器的資訊，Network Controller VM 提供 IP 位址，並執行代表 Idn 伺服器的 VM 的 DNS 名稱登記。 

Network Controller 也會告知 Idn proxy VM 並執行 vm 的名稱解析所需的詳細資料。 

當 VM 起始 DNS 查詢時，proxy 就像是從 Virtual 網路查詢 Idn 服務的轉寄。 

DNS proxy 也可確保承租人 VM 查詢隔離。 如果授權查詢 Idn 伺服器，Idn 伺服器回應授權回應。 如果 Idn 伺服器不適用於查詢，它會執行 DNS 遞迴解析網際網路的名稱。

>[!NOTE]
>這項資訊一節中包含**設定 AttachToVirtualNetwork**中 SDNExpressTenant.ps1。 如需詳細資訊，請查看[部署使用指令碼軟體定義網路基礎結構，](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)。

