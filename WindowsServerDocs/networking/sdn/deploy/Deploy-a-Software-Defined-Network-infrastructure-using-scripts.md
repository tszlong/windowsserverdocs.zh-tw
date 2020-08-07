---
title: 使用腳本部署軟體定義的網路基礎結構
description: 本主題說明如何使用 Windows Server 2016 中的腳本，在 (SDN) 基礎結構中部署 Microsoft 軟體定義網路。
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: e671421225e9fa2d5e7461c795a9f7c9789efd88
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970815"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>使用指令碼部署軟體定義網路的基礎結構

>適用于： Windows Server (半年通道) ，Windows Server 2016 ' 在本主題中，您會使用腳本來部署 Microsoft 軟體定義網路 (SDN) 基礎結構。 基礎結構包含高可用性 (HA) 網路控制卡、HA 軟體 Load Balancer (SLB) /MUX、虛擬網路，以及存取控制 Acl (相關聯的) 清單。 此外，另一個腳本會部署租使用者工作負載，以驗證您的 SDN 基礎結構。

如果您想要讓租使用者工作負載在其虛擬網路外部進行通訊，您可以設定 SLB NAT 規則、站對站閘道通道或第3層轉送，以在虛擬和實體工作負載之間進行路由。

您也可以使用 Virtual Machine Manager (VMM) 來部署 SDN 基礎結構。 如需詳細資訊，請參閱在[VMM 光纖中設定軟體定義網路 (SDN) 基礎結構](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。

## <a name="pre-deployment"></a>前置部署

> [!IMPORTANT]
> 開始部署之前，您必須規劃並設定您的主機和實體網路基礎結構。 如需詳細資訊，請參閱[規劃軟體定義網路基礎結構](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。

所有 Hyper-v 主機都必須安裝 Windows Server 2016。

## <a name="deployment-steps"></a>部署步驟
一開始請先設定 Hyper-v 主機的 (實體伺服器) Hyper-v 虛擬交換器和 IP 位址指派。 可以使用與 Hyper-v、共用或本機相容的任何儲存類型。

### <a name="install-host-networking"></a>安裝主機網路功能

1. 安裝適用于您的 NIC 硬體的最新網路驅動程式。
2. 在所有主機上安裝 Hyper-v 角色 (如需詳細資訊，請參閱[開始使用 Windows Server 2016 上的 hyper-v](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows)。

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```

3. 建立 Hyper-v 虛擬交換器。<p>針對所有主機使用相同的交換器名稱，例如**sdnSwitch**。 至少設定一個網路介面卡，如果使用 SET，請設定至少兩個網路介面卡。 使用兩個 Nic 時，會發生輸入擴散的最大值。

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```

   >[!TIP]
   >如果您有個別的管理 Nic，可以略過步驟4和5。

3. 請參閱規劃主題 ([規劃軟體定義的網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 並與您的網路系統管理員合作，以取得管理 VLAN 的 vlan ID。 將新建立之虛擬交換器的管理 vNIC 附加至管理 VLAN。 如果您的環境不使用 VLAN 標記，則可省略此步驟。

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```

4. 請參閱規劃主題 ([規劃軟體定義的網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 並與您的網路系統管理員合作，以使用 DHCP 或靜態 IP 指派，將 IP 位址指派給新建立之 VSwitch 的管理 vNIC。 下列範例顯示如何建立靜態 IP 位址，並將它指派給 vSwitch 的管理 vNIC：

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```

5. 選擇性將虛擬機器部署至主機 Active Directory Domain Services ([安裝 Active Directory Domain Services (層級 100) ](https://technet.microsoft.com/library/hh472162.aspx)和 DNS 伺服器。

    a. 將 Active Directory/DNS 伺服器虛擬機器連接到管理 VLAN：

      ```PowerShell
      Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True
      ```

   b. 安裝 Active Directory Domain Services 和 DNS。

   >[!NOTE]
   >網路控制卡同時支援 Kerberos 和 x.509 憑證來進行驗證。 本指南會針對不同的用途使用這兩種驗證機制 (雖然只有一個) 需要。

6. 將所有 Hyper-v 主機加入網域。 確定具有指派給管理網路之 IP 位址的網路介面卡的 DNS 伺服器專案指向可解析功能變數名稱的 DNS 伺服器。

   ```PowerShell
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>
   ```

   a. 以滑鼠右鍵按一下 [**開始**]，按一下 [**系統**]，然後按一下 [**變更設定**]。
   b. 按一下 [變更]****。
   c. 按一下 [**網域**]，並指定功能變數名稱。  "" "" d。 按一下 [確定]  。
   e. 出現提示時，輸入使用者名稱和密碼認證。
   f. 重新啟動伺服器。

### <a name="validation"></a>驗證
使用下列步驟來驗證主機網路設定是否正確。

1. 請確定已成功建立 VM 交換器：

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```

2. 確認 VM 交換器上的管理 vNIC 已連線至管理 VLAN：

   >[!NOTE]
   >只有在管理和租使用者流量共用相同的 NIC 時才相關。

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. 驗證所有 Hyper-v 主機和外部管理資源，例如 DNS 伺服器。<p>請使用其管理 IP 位址和（或） (FQDN) 的完整功能變數名稱，確保可透過 ping 存取它們。

   ``ping <Hyper-V Host IP>``
   ``ping <Hyper-V Host FQDN>``

4. 在部署主機上執行下列命令，並指定每部 Hyper-v 主機的 FQDN，以確保所使用的 Kerberos 認證可提供所有伺服器的存取權。

   ``winrm id -r:<Hyper-V Host FQDN>``

### <a name="nano-installation-requirements-and-notes"></a>Nano 安裝需求和注意事項

如果您使用 Nano 作為 Hyper-v 主機 (部署) 的實體伺服器，下列為額外的需求：

1. 所有 Nano 節點都必須使用語言套件安裝 DSC 套件：

   - Microsoft-NanoServer-DSC-Package.cab
   - Microsoft-NanoServer-DSC-Package_en-us.cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``

2. SDN Express 腳本必須從非 Nano 主機執行， (Windows Server Core 或 Windows Server （w/GUI) ）。 Nano 不支援 PowerShell 工作流程。

3. 若要使用 PowerShell 或 NC REST 包裝函式來叫用網路控制站 NorthBound API (其依賴 WebRequest 和 RestMethod) 必須從非 Nano 主機完成。

### <a name="run-sdn-express-scripts"></a>執行 SDN Express 腳本

1. 移至[MICROSOFT SDN GitHub 存放庫](https://github.com/Microsoft/SDN.git)以取得安裝檔案。

2. 將安裝檔案從存放庫下載到指定的部署電腦。 按一下 [**複製或下載**]，然後按一下 [**下載 ZIP**]。

   >[!NOTE]
   >指定的部署電腦必須執行 Windows Server 2016 或更新版本。

3. 展開 zip 檔案，並將**SDNExpress**資料夾複製到部署電腦的 `C:\` 資料夾。

4. 以 `C:\SDNExpress` "**SDNExpress**" 共用資料夾，**每個人都**能**讀取/寫入**。

5. 瀏覽至 `C:\SDNExpress` 資料夾。<p>您會看到下列資料夾：

   | 資料夾名稱 | 描述 |
   |--|--|
   | AgentConf | 保存每個 Windows Server 2016 Hyper-v 主機上 SDN 主機代理程式所使用的 OVSDB 架構新複本，以編寫網路原則。 |
   | 憑證 | NC 憑證檔案的暫存共用位置。 |
   | 影像 | 空白，將您的 Windows Server 2016 vhdx 映射放在這裡 |
   | 工具 | 用於疑難排解和調試的公用程式。  已複製到主機和虛擬機器。  建議您在這裡網路監視器或 Wireshark，讓它可在必要時使用。 |
   | 指令碼 | 部署腳本。<p>- **SDNExpress.ps1**<br>部署並設定網狀架構，包括網路控制站虛擬機器、SLB Mux 虛擬機器、閘道集區 () 以及對應至集區 (s) 的 HNV 閘道虛擬機器 (s) 。<br />-   **FabricConfig.psd1**<br>SDNExpress 腳本的設定檔案範本。  您會為您的環境自訂此程式。<br />-   **SDNExpressTenant.ps1**<br>在具有負載平衡 VIP 的虛擬網路上部署範例租使用者工作負載。<br>也會在服務提供者邊緣閘道上，布建一或多個 (IPSec S2S VPN、GRE、L3) 的網路連線，並連接到先前建立的租使用者工作負載。 IPSec 和 GRE 閘道可透過對應的 VIP IP 位址，以及透過對應位址集區的 L3 轉送閘道來進行連線。<br>此腳本也可以用來刪除具有 [復原] 選項的對應設定。<br />- **TenantConfig.psd1**<br>租使用者工作負載和 S2S 閘道設定的範本設定檔案。<br />- **SDNExpressUndo.ps1**<br>清除網狀架構環境，並將其重設為啟動狀態。<br />- **SDNExpressEnterpriseExample.ps1**<br>使用一個遠端存取閘道布建一或多個企業網站環境，並 (選擇性地為每個網站) 一個對應的企業虛擬機器。 IPSec 或 GRE 企業閘道會連接到服務提供者閘道的對應 VIP IP 位址，以建立 S2S 通道。 L3 轉送閘道會透過對應的對等 IP 位址進行連接。 <br> 此腳本也可以用來刪除具有 [復原] 選項的對應設定。<br />- **EnterpriseConfig.psd1**<br>適用于企業站對站閘道和用戶端 VM 設定的範本設定檔。 |
   | TenantApps | 用來部署範例租使用者工作負載的檔案。 |

6. 確認 Windows Server 2016 VHDX 檔案位於 [ **Images** ] 資料夾中。

7. 藉由變更<< 以特定值**取代 >>** 標記來自訂 SDNExpress\scripts\FabricConfig.psd1 檔案，以符合您的實驗室基礎結構，包括主機名稱、功能變數名稱、使用者名稱和密碼，以及規劃網路主題中所列網路的網路資訊。

8. 針對 NetworkControllerRestName (FQDN) 和 NetworkControllerRestIP，在 DNS 中建立主機 A 記錄。

9. 以具有網域系統管理員認證的使用者身分執行腳本：

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``

10. 若要復原所有作業，請執行下列命令：

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``

#### <a name="validation"></a>驗證

假設 SDN Express 腳本執行完成，但未回報任何錯誤，您可以執行下列步驟，以確保網狀架構資源已正確部署，而且可供租使用者部署使用。

使用[診斷工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)，以確保網路控制卡中的任何網狀架構資源都沒有錯誤。

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>使用軟體負載平衡器部署範例租使用者工作負載

現在已部署網狀架構資源，您可以藉由部署範例租使用者工作負載，端對端驗證您的 SDN 部署。 此租使用者工作負載是由兩個 (web 層和資料庫層的虛擬子網所組成) 透過存取控制清單來保護， (使用 SDN 分散式防火牆的 ACL) 規則。 Web 層的虛擬子網可透過使用虛擬 IP (VIP) 位址的 SLB/MUX 來存取。 腳本會自動部署兩個 web 層虛擬機器和一個資料庫層虛擬機器，並將其連接到虛擬子網。

1.  藉由變更<< 以特定值來**取代 >>** 標記來自訂 SDNExpress\scripts\TenantConfig.psd1 檔案 (例如： VHD 映射名稱、網路控制器 REST 名稱、vSwitch 名稱等，如先前在 FabricConfig.psd1 檔案中所定義) 

2.  執行指令碼。 例如：

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``

3.  若要復原設定，請使用**undo**參數執行相同的腳本。 例如：

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``

#### <a name="validation"></a>驗證

若要驗證租使用者部署是否成功，請執行下列動作：

1. 登入資料庫層虛擬機器並嘗試 ping 其中一個 web 層虛擬機器的 IP 位址， (確保 web 層虛擬機器) 中的 Windows 防火牆已關閉。

2. 檢查網路控制站租使用者資源是否有任何錯誤。 從任何具有第3層連線到網路控制站的 Hyper-v 主機執行下列各項：

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. 若要確認負載平衡器是否正常運作，請從任何 Hyper-v 主機執行下列程式：

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   其中 `<VIP IP address>` 是您在 TenantConfig.psd1 檔案中設定的 web 層 VIP IP 位址。

   >[!TIP]
   >搜尋 `VIPIP` TenantConfig.psd1 中的變數。

   執行此多個時間，以查看可用 Dip 之間的負載平衡器切換。 您也可以使用網頁瀏覽器來觀察此行為。 瀏覽至 `<VIP IP address>/unique.htm` 。 關閉瀏覽器並開啟新的實例，然後再流覽一次。 您會看到藍色頁面和綠色頁面替代，除非瀏覽器在快取超時前快取頁面。