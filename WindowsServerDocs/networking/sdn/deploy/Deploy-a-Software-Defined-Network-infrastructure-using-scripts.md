---
title: 部署軟體定義的網路基礎結構使用指令碼，
description: 本主題涵蓋如何部署 Microsoft 軟體定義網路 (SDN) 基礎結構使用 Windows Server 2016 指令碼。
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>部署軟體定義的網路基礎結構使用指令碼，

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題涵蓋如何部署 Microsoft 軟體定義網路 (SDN) 基礎結構使用指令碼。 基礎結構包含可用性 (HA) 網路控制器，哈軟體負載平衡器 (SLB) 日 MUX，virtual 網路和相關存取控制清單 (Acl)。 此外，另一個指令碼部署承租人工作負載驗證您的 SDN 基礎結構。  
  
如果您想外他們 virtual 網路通訊您承租人工作負載，您可以設定 SLB NAT 規則、網站-閘道的通道，或層級 3 轉寄 virtual 與實體工作負載之間傳送。  
  
您也可以將使用一樣 Manager (VMM) SDN 基礎結構部署。 如需詳細資訊，請查看[設定中 VMM fabric 軟體定義網路 (SDN) 基礎架構](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。  
  
## <a name="pre-deployment"></a>預先部署  
  
> [!IMPORTANT]  
> 部署在您開始之前，您必須計畫並設定您的主機和實體網路基礎結構。 如需詳細資訊，請查看[計劃軟體定義網路基礎架構](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。  
  
所有 HYPER-V 主機必須都已安裝 Windows Server 2016。  
  
## <a name="deployment-steps"></a>部署步驟  
開始設定 HYPER-V 主機（實體伺服器）HYPER-V virtual 切換及 IP 位址設定。 可使用任何相容於 HYPER-V，共用或本機存放裝置類型。  
### <a name="install-host-networking"></a>安裝主機的網路  
1. 安裝最新的網路驅動程式可供您 NIC 的硬體。  
2. 在所有主機上安裝 HYPER-V 角色 (如需詳細資訊，請查看[開始使用 Windows Server 2016 上 HYPER-V](https://technet.microsoft.com/en-us/library/mt126159.aspx)。   
  
   從 Windows PowerShellcommand 提升權限提示：  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    a。 建立 HYPER-V virtual 切換（使用相同的所有主機切換名稱。 For example: **sdnSwitch**)。 設定一個以上的網路介面卡，或使用切換 Embedded 小組，如果設定至少兩部網路介面卡。 使用兩個 Nic 時，最大值輸入分配就會發生。  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >如果您有不同管理 Nic，您可以跳過步驟 3、4。

3. 規劃主題參考 ([計劃軟體定義網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md))，可以使用您的網路管理員以取得 VLAN ID 管理 VLAN。 管理 VLAN 附加管理但 vNIC 的新建立的 Virtual 切換。 如果您的環境並不會使用 VLAN 標記可以忽略此步驟。  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. 規劃主題參考 ([計劃軟體定義網路基礎結構](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md))，可以使用您的網路系統管理員使用「DHCP 或將 IP 位址指派給新建立的 vSwitch 的管理但 vNIC 靜態 IP 設定。 下例示範如何建立靜態 IP 位址，並將它指派給的 vSwitch 管理但 vNIC:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [選擇性]部署一樣主機 Active Directory Domain Services ([安裝 Active Directory Domain Services (層級 100)](https://technet.microsoft.com/library/hh472162.aspx)和 [DNS 伺服器。  
   
    a。 若要管理 VLAN 連接 Active Directory 日 DNS 伺服器一樣：
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b。 Active Directory Domain Services 和 DNS 安裝。  
      >[!NOTE]
      >網路控制器驗證支援 X.509 和 Kerberos 憑證。 本指南會針對不同用途使用這兩個驗證機制（但只能有一個是必要的）。  
        
6. 加入網域的所有 HYPER-V 主機。 確定已指派給管理網路點可以網域名稱解析 DNS 伺服器的 IP 位址的網路介面卡的 DNS 伺服器項目。 例如：

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   a。 以滑鼠右鍵按一下**[開始]**，按一下 [**系統**，然後按一下 [**變更設定**。  
   b。 按一下**變更**。  
   c。 按一下**網域**，然後指定的網域名稱。  
   d。 按一下**[確定]**。  
   e。 輸入使用者名稱和密碼認證出現提示時。  
   f。 重新開機伺服器。  
  
### <a name="validation"></a>驗證  
使用下列步驟，以驗證網路該主機設定正確。  
1. 確定已順利建立 VM 切換：  
      
    ``Get-VMSwitch "<switch name>"``  
2. 確認上 VM 開關切換至管理但 vNIC 連接至管理 VLAN:  
    >[!NOTE]
    >只有當管理和承租人流量共用相同 NIC 相關    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. 驗證您的所有 HYPER-V 主機 (和外部管理資源，例如：DNS 伺服器) 可透過 ping 使用他們管理 IP 位址和/或完整的網域名稱 (FQDN)。   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. 執行下列命令部署主機上，然後指定以確定用 Kerberos 認證每個 HYPER-V 主機的 FQDN 可讓您存取所有伺服器。  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Nano 安裝需求和筆記  
如果您使用 Nano 為您的 HYPER-V 主機（實體伺服器）部署，以下的額外需求：  
1. 所有 Nano 節點都需要 DSC 套件安裝語言套件：  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. 必須非 Nano 主機（Windows Server Core 或 GUI 與 Windows Server）上執行的 SDN 快速指令碼。 Nano 不支援 PowerShell 工作流程。  
3.  網路控制器 NorthBound API 叫用使用 PowerShell 或 NC 其餘包裝函式（依賴 Invoke-WebRequest 和 Invoke-RestMethod），必須完成從非 Nano 主機。  
   
         
### <a name="run-sdn-express-scripts"></a>執行 SDN 快速指令碼  
  
1.  GitHub 上位於將安裝檔案。 下載 zip 檔案從[Microsoft SDN GitHub 存放庫](https://github.com/Microsoft/SDN.git)。 在 Microsoft SDN 存放庫頁面上，按一下 [**複製或下載**，然後按一下 [**下載 ZIP**。  
  
2.  為您的部署電腦指定一部電腦。  這台電腦必須執行 Windows Server 2016。 展開 [zip 檔案和複製**SDNExpress**資料夾，以部署電腦的`C:\`資料夾。  
  
3.  分享`C:\SDNExpress`資料夾]**SDNExpress**」的權限**每個人都**以**讀取/寫入**。  
  
4.  瀏覽至`C:\SDNExpress`資料夾。

 您會看到下列資料夾：  

|資料夾名稱|描述|  
|---------------|---------------|  
|AgentConf|保留 OVSDB 結構描述 SDN 主機代理程式網路原則每個 Windows Server 2016 HYPER-V 主機上所使用的全新複本。|  
|憑證|暫時共用的 NC 憑證檔案的位置。|  
|影像|清空、以下放入您的 Windows Server 2016 vhdx 影像|  
|工具|疑難排解與 windows 偵錯的公用程式。  複製到主機和虛擬的電腦。  我們建議您將網路監視器或 Wireshark 以下，並視需要使用。|  
|指令碼|部署指令碼。<br /><br />-   **SDNExpress.ps1**<br />    部署，並設定 fabric，包括網路控制器虛擬電腦、SLB Mux 虛擬電腦、閘道集區與 HNV 閘道虛擬機器對應至集區。<br />-   **FabricConfig.psd1**<br />    設定檔範本 SDNExpress 指令碼。  您將會為您的環境自訂此設定。<br />-   **SDNExpressTenant.ps1**<br />    部署 virtual 負載平衡 VIP 與網路上的範例承租人工作負載。<br />    也在連接到先前建立的承租人工作負載的服務提供者 edge 閘道 provisions 一或多個網路連接 IPSec S2S VPN、GRE（L3）。 IPSec 和 GRE 閘道可供連接到對應的 VIP IP 位址，以及 L3 轉接閘道到對應的地址集區。<br />    此指令碼，可以用於也 delete 相對應的組態的復原選項。<br />-   **TenantConfig.psd1**<br />    範本組態承租人工作負載和 S2S 閘道設定檔。<br />-   **SDNExpressUndo.ps1**<br />    清除 fabric 環境並將它重設開始狀態。<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Provisions 一或多個企業網站環境一個遠端存取閘道與（選擇性）一個對應企業一樣每個網站。 IPSec 或 GRE 企業閘道器連接至建立 S2S 可愛的服務提供者閘道對應的 VIP IP 位址。 L3 轉寄閘道器連接到對應的 IP 位址等。 <br />            此指令碼，可以用於也 delete 相對應的組態的復原選項。<br />-   **EnterpriseConfig.psd1**<br />    適用於企業網站-閘道和 Client VM 設定範本設定檔。|  
|TenantApps|用來部署範例承租人工作負載的檔案。|  
  
5.  確認 Windows Server 2016 VHDX 檔案**影像**資料夾。  
  
6. 變更來自訂 SDNExpress\scripts\FabricConfig.psd1 檔案**<< 取代 >>**標籤與特定值，可符合您 lab 的基礎結構包括主機名稱、網域名稱、使用者名稱和密碼，以及網路資訊的網路列出規劃網路主題中。  
7. 建立 NetworkControllerRestIP NetworkControllerRestName (FQDN) 和 DNS 主機 A 記錄。  
8. 網域系統管理員認證的使用者身分執行指令碼：  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  若要復原所有作業，請執行下列命令：  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>驗證  
假設 SDN 快速指令碼執行完成不報告出現任何錯誤，您可以執行下列步驟來確保正確部署及可供部署承租人 fabric 資源。  

- 使用[診斷工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)以確定您不有任何錯誤任何 fabric 資源網路控制器。  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>部署範例承租人工作負載的軟體負載平衡器  
    
既然已部署 fabric 資源，您可以藉由部署範例承租人工作負載驗證您 SDN 部署的端點。 這個承租人工作負載包含兩個 virtual 子網路（web 層和資料庫層）使用 SDN 散發防火牆存取控制清單 (ACL) 規則透過受保護。 透過使用 Virtual IP (VIP) 位址 SLB 日 MUX 存取的網頁層 virtual 子網路。 指令碼自動部署兩個 web 層虛擬電腦和一個資料庫層一樣，並連接這些 virtual 子網路。  
  
1.  變更來自訂 SDNExpress\scripts\TenantConfig.psd1 檔案**<< 取代 >>**標籤使用特定的值 (例如：VHD 影像名稱、控制器其餘的網路名稱、vSwitch 名稱、等上文所述 FabricConfig.psd1 檔案)  
2.  執行指令碼。 例如：  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  復原設定，來執行相同的指令碼的**復原**的參數。 例如：  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>驗證  
若要驗證承租人部署成功，執行下列動作：
1.  登入資料庫層一樣，然後嘗試 ping 一個（確保 web 層虛擬電腦中為關閉 Windows 防火牆）的網頁層虛擬電腦的 IP 位址。  
2.  檢查有任何錯誤網路控制器承租人資源。 從執行下列任何 HYPER-V 主機的層級 3 連接至網路控制器：  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. 若要確認正確執行負載平衡器，執行下列命令從任何 HYPER-V 主機：
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   其中`<VIP IP address>`是 web 層您設定在 TenantConfig.psd1 檔案 VIP IP 位址。 搜尋`VIPIP`在 TenantConfig.psd1 變數。

   執行這個 muliple 次看到負載平衡器提供 DIPs 之間切換。 您也可以觀察使用網頁瀏覽器此行為。 瀏覽] `<VIP IP address>/unique.htm`。 關閉瀏覽器開放新執行個體並再試一次瀏覽。 您將會看到藍色頁面替代，除了在瀏覽器快取網頁快取逾時前遺漏頁面。