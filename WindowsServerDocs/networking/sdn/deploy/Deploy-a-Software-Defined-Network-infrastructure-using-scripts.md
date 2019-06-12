---
title: 部署軟體定義的網路基礎結構使用指令碼
description: 本主題說明如何部署 Windows Server 2016 中使用指令碼的 Microsoft 軟體定義網路 (SDN) 基礎結構。
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: ce88659f6065ddd0957b95831a4e3065f09dc9e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446370"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>使用指令碼部署軟體定義網路的基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您可以部署使用指令碼的 Microsoft 軟體定義網路 (SDN) 基礎結構。 基礎結構包括高可用性 (HA) 網路控制站，HA 軟體負載平衡器 (SLB) / MUX、 虛擬網路，以及相關聯的存取控制清單 (Acl)。 此外，另一個指令碼會部署您驗證您的 SDN 基礎結構的租用戶工作負載。  

如果您想您的租用戶工作負載，其虛擬網路外部通訊，您可以設定 SLB NAT 規則、 站對站閘道的通道，或 Layer 3 Forwarding 虛擬和實體工作負載之間路由傳送。  

您也可以部署 SDN 基礎結構使用 Virtual Machine Manager (VMM)。 如需詳細資訊，請參閱 <<c0> [ 設定 VMM 光纖中軟體定義網路 (SDN) 基礎結構](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。  


## <a name="pre-deployment"></a>部署前  

> [!IMPORTANT]  
> 在開始部署之前，您必須規劃並設定您的主機和實體網路基礎結構。 如需詳細資訊，請參閱[規劃軟體定義網路基礎結構](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。  

所有的 HYPER-V 主機必須已安裝 Windows Server 2016。  

## <a name="deployment-steps"></a>部署步驟  
開始設定 HYPER-V 主機 （實體伺服器） 對 HYPER-V 虛擬交換器和 IP 位址指派。 適用於 HYPER-V、 共用或本機任何儲存體類型都可用。  

### <a name="install-host-networking"></a>安裝主機網路功能  

1. 安裝最新的網路驅動程式可供您的 NIC 硬體。  
2. 在 所有主機上安裝 HYPER-V 角色 (如需詳細資訊，請參閱 <<c0> [ 開始使用 Windows Server 2016 上的 HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows)。   

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  

3. 建立 HYPER-V 虛擬交換器。<p>例如，使用相同的交換器名稱，適用於所有的主機**sdnSwitch**。 設定至少一個網路介面卡，或者，如果使用的集合，設定至少兩個網路介面卡。 使用兩個 Nic 時，最大輸入分配就會發生。  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >如果您有個別的管理 Nic，您可以略過步驟 4 和 5。

3. 請參閱規劃主題 ([規劃軟體定義網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 以及使用您的網路系統管理員，以取得管理 VLAN 的 VLAN 識別碼。 將新建立的虛擬交換器管理 vNIC 連結至 管理 VLAN。 如果您的環境不會使用 VLAN 標記，則可以省略此步驟。  

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  

4. 請參閱規劃主題 ([規劃軟體定義網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 以及使用您的網路系統管理員，才能使用是 DHCP 或靜態 IP 指派，以將 IP 位址指派給新建立的管理 vNICvSwitch。 下列範例示範如何建立靜態 IP 位址，並將它指派給管理 vNIC vSwitch 的：  

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  

5. [選用]將虛擬機器部署到主機 Active Directory 網域服務 ([安裝 Active Directory 網域服務 (技術等級 100)](https://technet.microsoft.com/library/hh472162.aspx)和 DNS 伺服器。  

    a. 將 Active Directory/DNS 伺服器虛擬機器連線到管理 VLAN:

       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. 安裝 Active Directory 網域服務和 DNS。  

   >[!NOTE]
   >網路控制站支援 Kerberos 和 X.509 憑證進行驗證。 本指南會針對不同用途使用這兩種驗證機制 （雖然只有一個是必要的）。  

6. 加入網域的所有 HYPER-V 主機。 請確定指派至管理網路點可以解析網域名稱的 DNS 伺服器的 IP 位址的網路介面卡的 DNS 伺服器項目。 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```

   a. 以滑鼠右鍵按一下**開始**，按一下**系統**，然後按一下**變更設定**。  
   b. 按一下 [變更]  。  
   c. 按一下 **網域**和指定網域名稱。  
   d. 按一下 [確定]  。  
   e. 輸入的使用者名稱和密碼認證出現提示時。  
   f. 重新啟動伺服器。  

### <a name="validation"></a>驗證  
使用下列步驟來驗證該網路的主機已正確設定。  

1. 請確定已成功建立 VM 交換器：  

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. 確認管理 vNIC 上 VM 交換器已連線至管理 VLAN:  

   >[!NOTE]
   >僅適用於管理和租用戶流量共用相同的 nic。    

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. 驗證所有的 HYPER-V 主機和外部管理資源，例如 DNS 伺服器。<p>請確定它們是透過使用其管理 IP 位址和/或完整的網域名稱 (FQDN) 的 ping 存取。   

   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. 在部署主機上執行下列命令，並指定每一部 HYPER-V 主機，以確保所使用的 Kerberos 認證的 FQDN 提供所有伺服器的存取權。  

   ``winrm id -r:<Hyper-V Host FQDN>``  

### <a name="nano-installation-requirements-and-notes"></a>Nano 安裝需求和注意事項  

如果您使用做為您的 HYPER-V 主機 （實體伺服器） 的 Nano 部署，以下還有其他需求：  

1. Nano 的所有節點都必須已安裝語言套件的 DSC 套件：  

   - Microsoft-NanoServer-DSC-Package.cab  
   - Microsoft-NanoServer-DSC-Package_en-us.cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. SDN Express 指令碼必須從非 Nano 主機 （Windows Server Core 或 Windows Server 含 GUI） 中執行。 在 Nano 上不支援 PowerShell 工作流程。  

3. 叫用網路控制器 NorthBound API 使用 PowerShell 或 NC REST 包裝函式 （這些服務依賴 Invoke-webrequest 和 Invoke-restmethod） 必須透過非 Nano 主機。  


### <a name="run-sdn-express-scripts"></a>執行 SDN Express 指令碼  

1. 移至[Microsoft SDN GitHub 存放庫](https://github.com/Microsoft/SDN.git)安裝檔案。

2. 從儲存機制下載安裝檔案，以指定的部署電腦。 按一下 **複製或下載**，然後按一下**下載 ZIP**。  

   >[!NOTE]
   >在指定的部署電腦必須執行 Windows Server 2016 或更新版本。

3. 展開的 zip 檔案，並複製**SDNExpress**資料夾部署的電腦`C:\`資料夾。  

4. 共用`C:\SDNExpress`與資料夾 」**SDNExpress**「 具有權限的**每個人都**來**讀取/寫入**。  

5. 瀏覽至`C:\SDNExpress`資料夾。<p>您會看到下列資料夾：  


   | 資料夾名稱 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |  AgentConf  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   保留 OVSDB SDN 主機代理程式，到程式的網路原則的每個 Windows Server 2016 HYPER-V 主機上所使用的結構描述的最新複本。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |    憑證    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         暫存 NC 憑證檔案的共用的位置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |   映像    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         空的將您的 Windows Server 2016 vhdx 映像放在此處                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |    工具    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            適用於疑難排解和偵錯的公用程式。  複製到主機和虛擬機器。  我們建議您將放在網路監視器 」 或 「 Wireshark 此處因此如有需要時才可用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |   指令碼   | 部署指令碼。<br /><br />-   **SDNExpress.ps1**<br />    部署並設定網狀架構，包括網路控制站虛擬機器、 SLB Mux 虛擬機器、 閘道集區和 HNV 閘道虛擬機器的集區對應。<br />-   **FabricConfig.psd1**<br />    SDNExpress 指令碼的設定檔範本。  您要自訂這為您的環境。<br />-   **SDNExpressTenant.ps1**<br />    部署虛擬網路與負載平衡 VIP 上的範例租用戶工作負載。<br />    也會一或多個網路連線 （IPSec S2S VPN，GRE，L3） 佈建服務提供者邊緣閘道連線到先前建立的租用戶工作負載。 IPSec 與 GRE 閘道可供連線對應的 VIP IP 位址和 L3 轉寄閘道上對應的位址集區。<br />    此指令碼可用來刪除對應的組態，使用 [復原] 選項。<br />-   **TenantConfig.psd1**<br />    租用戶工作負載和 S2S 閘道組態範本設定檔。<br />-   **SDNExpressUndo.ps1**<br />    清除 網狀架構環境，並將它重設為起始的狀態。<br />-   **SDNExpressEnterpriseExample.ps1**<br />    佈建一個遠端存取閘道與每個網站 （選擇性） 一個對應 enterprise 虛擬機器的一或多個企業站台環境。 IPSec 或 GRE 企業閘道器連接到對應的 VIP IP 位址，以建立 S2S 通道的服務提供者閘道。 L3 轉送閘道會透過對應的對等體 IP 位址連線。 <br />            此指令碼可用來刪除對應的組態，使用 [復原] 選項。<br />-   **EnterpriseConfig.psd1**<br />    範本會設定企業站對站閘道器和用戶端 VM 組態檔。 |
   | TenantApps  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             用來部署範例租用戶工作負載的檔案。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

   ---

6. 確認 Windows Server 2016 VHDX 檔案位於**映像**資料夾。  

7. 藉由變更自訂 SDNExpress\scripts\FabricConfig.psd1 檔案 **<< 取代 >>** 標記以特定的值，以符合您的實驗室基礎結構，包括主機名稱、 網域名稱、 使用者名稱和密碼，以及規劃網路 > 主題中列出的網路的網路資訊。  

8. 在 DNS 中建立主機 A 記錄，NetworkControllerRestName (FQDN) 和 NetworkControllerRestIP。  

9. 具有網域系統管理員認證的使用者身分執行指令碼：  

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

10. 若要復原的所有作業，請執行下列命令：  

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

#### <a name="validation"></a>驗證  

假設 SDN Express 指令碼執行完成時未報告任何錯誤，您可以執行下列步驟，以確保已正確部署的網狀架構資源，並可供租用戶部署。  

使用[診斷工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)以確保沒有任何錯誤上任何網狀架構資源的網路控制卡中。  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>部署軟體負載平衡器的範例租用戶工作負載  

既然已部署了網狀架構資源，您可以藉由部署範例租用戶工作負載來驗證您 SDN 部署端對端。 此租用戶工作負載都包含兩個虛擬子網路 （web 層和資料庫層） 透過使用 SDN 分散式防火牆的存取控制清單 (ACL) 規則保護。 透過使用虛擬 IP (VIP) 位址 SLB/MUX 存取 web 層的虛擬子網路。 指令碼自動部署兩個 web 層虛擬機器和資料庫層虛擬機器，並連接到虛擬子網路。  

1.  藉由變更自訂 SDNExpress\scripts\TenantConfig.psd1 檔案 **<< 取代 >>** 具有特定值的標記 (例如：VHD 映像名稱，網路控制卡 REST 名稱、 vSwitch 名稱，如先前 FabricConfig.psd1 檔案中定義等）  

2.  執行指令碼。 例如:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  若要復原的組態，執行具有相同的指令碼**復原**參數。 例如:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>驗證  

若要驗證的租用戶部署已順利完成，執行下列作業：

1. 登入資料庫層虛擬機器，然後再次嘗試 ping 的其中一個 （請確定 Windows 防火牆已關閉 web 層虛擬機器） 的 web 層虛擬機器的 IP 位址。  

2. 請檢查有任何錯誤的網路控制站的租用戶資源。 執行下列命令從任何 HYPER-V 主機與第 3 層連線到網路控制站：  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. 若要確認負載平衡器正常執行，從任何 Hyper-v 主機執行下列命令：

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   其中`<VIP IP address>`是 web 層您 TenantConfig.psd1 檔案中設定的 VIP IP 位址。 

   >[!TIP]
   >搜尋`VIPIP`TenantConfig.psd1 中使用者定義變數。

   執行此多時間，以查看可用的 Dip 之間切換的負載平衡器。 您也可以觀察此行為，使用網頁瀏覽器。 瀏覽至 `<VIP IP address>/unique.htm`。 關閉瀏覽器中開啟的新執行個體和瀏覽一次。 您會看到 [藍色] 頁面和綠色替代，除了當瀏覽器快取的頁面快取逾時之前的頁面。

---