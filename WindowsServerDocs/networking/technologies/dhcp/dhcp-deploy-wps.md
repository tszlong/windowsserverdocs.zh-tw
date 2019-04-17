---
title: 部署 DHCP 使用 Windows PowerShell
description: 您可以使用此主題部署 Windows Server 2016 網際網路通訊協定 (IP) 版本 4 DHCP 伺服器連接您網路上的一或多個子網路 IPv4 DHCP 戶端提供自動 IP 位址和 DHCP 選項。
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>部署 DHCP 使用 Windows PowerShell

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本指南提供如何使用 Windows PowerShell 部署自動將 IP 位址和 DHCP 選項指派給您網路上的一或多個子網路連接 IPv4 DHCP 戶端網際網路通訊協定」(IP) 版本 4 動態主機設定通訊協定 \(DHCP\) 伺服器上的指示。

>[!NOTE]
>從 TechNet 庫下載 Word 格式這份文件，請查看[部署 DHCP 使用 Windows PowerShell 中的 Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293)。

若要指定 IP 使用 DHCP 伺服器位址儲存管理費用因為您不需要手動設定 TCP/IP v4 設定中的針對所有網路介面卡，在網路上每一部電腦。 使用 DHCP，當電腦自動執行 v4 的 TCP/IP 設定，或其他 DHCP client 已連接到您的網路。

為獨立伺服器，或做為 Active Directory domain 的一部分，您可以部署 DHCP 伺服器工作群組中。

本指南包含下列各節。

- [DHCP 部署概觀](#bkmk_overview)
- [技術概觀](#bkmk_technologies)
- [規劃 DHCP 部署](#bkmk_plan)
- [本指南使用實驗室測試](#bkmk_lab)
- [部署 DHCP](#bkmk_deploy)
- [請確認伺服器功能](#bkmk_verify)
- [Windows PowerShell 命令 DHCP](#bkmk_dhcpwps)
- [本指南 Windows PowerShell 命令清單](#bkmk_list)

## <a name="bkmk_overview"></a>DHCP 部署概觀

下圖描述案例，您可以使用此快速入門部署。 Active Directory domain 案例包含一個 DHCP 伺服器。 伺服器提供給 DHCP 戶端兩個不同的子網路上的 IP 位址設定。 子網路分隔路由器的 DHCP 轉寄支援。

![DHCP 網路拓撲概觀](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>技術概觀

下列章節提供 DHCP 和 TCP/IP 簡短的概觀。

### <a name="dhcp-overview"></a>DHCP 概觀

DHCP 是標準簡化主機 IP 設定的管理的 IP。 標準 DHCP 提供使用 DHCP 伺服器管理 DHCP 式戶端，您網路上的 IP 位址動態配置及其他設定的相關詳細資料的方式。

DHCP 可讓您使用 DHCP 伺服器動態指派的電腦或其他裝置，例如印表機，請在您的區域網路，而不是以手動方式與靜態 IP 位址設定每部裝置的 IP 位址。

每個 TCP/IP 網路上的電腦必須唯一的 IP 位址，因為的 IP 位址，其相關子網路遮罩找出主機電腦和電腦連接的子網路。 使用 DHCP，您可以確保所有電腦設定為 DHCP 戶端都獲得適當的網路位置的子網路的 IP 位址，使用 DHCP 選項，例如預設閘道和 DNS 伺服器，您可以自動提供 DHCP 戶端正確運作，您網路上所需的資訊。

TCP 型網路，它可以減少參與設定電腦的系統管理工作量與複雜。

### <a name="tcpip-overview"></a>TCP/IP 概觀

根據預設，所有版本的 Windows Server 與 Windows Client 作業系統都有設定為自動取得 IP 位址和其他資訊，稱為 DHCP 選項，DHCP 伺服器的 IP 版本 4 的網路連接 TCP/IP 設定。 因此，您不需要手動設定 TCP/IP 設定，除非電腦伺服器電腦或其他裝置，需要手動設定、靜態 IP 位址。 

例如，建議您手動設定 DHCP 伺服器的 IP 位址，以及執行 Active Directory Domain Services \(AD DS\) 網域控制站的 DNS 伺服器的 IP 位址。

以下是在 Windows Server 2016 TCP/IP:

-   網路根據業界標準網路通訊協定的軟體。

-   路由企業網路通訊協定支援 windows 電腦的區域網路（區域網路）和寬區域 (WAN) 環境連接。

-   核心技術與公共事業適用於 windows 的電腦連接的不同系統，以分享的資訊。

-   適用於通用網際網路服務，例如檔案傳輸通訊協定（檔案）伺服器存取基本知識。

-   穩定，延展性跨平台，client 日伺服器架構。

TCP/IP 提供基本 TCP/IP 公用程式，可讓 Windows 電腦連接並分享的資訊和其他 Microsoft、非 Microsoft 系統，包括：

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- 網際網路主機

- 蘋果 Macintosh 系統

- 大型 IBM 主機

- UNIX 和 Linux 系統

- 開放 VM 系統

- 網路準備印表機

- 平板電腦和行動電話有線乙太網路或 wireless 802.11 的技術支援

## <a name="bkmk_plan"></a>規劃 DHCP 部署

以下是金鑰規劃步驟之前，請先安裝 DHCP 伺服器角色。

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>規劃伺服器 DHCP 和 DHCP 轉接

由於 DHCP 訊息廣播的訊息，它們不轉送之間路由器子網路。 如果您有多個子網路，並想要提供 DHCP 為每個子網路的服務，您必須執行下列其中一個動作：

-   安裝每個子網路 DHCP 伺服器

-   設定路由器 DHCP 廣播的郵件轉寄子網路上並設定多個領域子網路每一個範圍 DHCP 伺服器上。

在大部分案例中，設定，將 DHCP 廣播的郵件轉寄路由器有更多成本效益的比 DHCP 伺服器每個區段實體網路上的部署。

### <a name="planning-ip-address-ranges"></a>規劃 IP 位址範圍

每個子網路中必須有它自己獨特的 IP 位址。 這些範圍表示範圍 DHCP 伺服器上。

領域是使用 DHCP 服務子網路上的電腦的 IP 位址管理群組。 系統管理員會先建立的每個實體的子網路的範圍，然後使用範圍定義用所使用的參數。

領域具有下列屬性：

-   IP 位址，包括或排除使用 DHCP 服務租用提供的地址。

-   子網路遮罩] 判斷子網路首碼指定 IP 位址。

-   指派建立時領域名稱。

-   租用期間值，已指派給 DHCP 戶端接收動態配置的 IP 位址。

-   指派給 DHCP 戶端，例如 DNS 伺服器的 IP 位址和路由器] / [預設閘道 IP 位址設定的任何 DHCP 範圍選項。

-   保留也會用來確保 DHCP client 收到相同的 IP 位址。

部署之前您的伺服器，會列出您子網路和您想要使用的每個子網路的 IP 位址範圍。

### <a name="planning-subnet-masks"></a>子網路遮罩計劃

使用 [子網路遮罩分辨網路 Id 和主機 Id 中 IP 位址。 每個子網路遮罩是 32 位元數字使用連續元群組的所有的網路找出 (1) ID 和所有零 (0) 找出主機 ID 部分的 IP 位址。

例如，常用的 IP 位址 131.107.16.200 子網路遮罩是下列 32 位元二進位數字：

```
11111111 11111111 00000000 00000000
```

此子網路遮罩數字是 16 一位元後面 16 零位元，指出這個 IP 位址的網路 ID 和主機 ID 區段這兩個 16 位元的長度。 一般而言，這個子網路遮罩小數點標記中顯示為 255.255.0.0。

下表顯示子網路遮罩網際網路位址類別。

|地址課|子網路遮罩的位元|子網路遮罩|
|-----------------|------------------------|---------------|
|A 課|11111111 00000000 00000000 00000000|255.0.0.0|
|B|11111111 11111111 00000000 00000000|255.255.0.0|
|C 課|11111111 11111111 11111111 00000000|255.255.255.0|

當您建立領域 DHCP 中，輸入 ip 範圍 DHCP 提供這些預設子網路遮罩值。 一般而言，子網路遮罩的預設值是適用於任何特殊需求大部分網路與其中每個 IP 網路區段對應單一實體網路。

有時候，您可以使用 [自訂子網路遮罩實作 IP 子網路。 IP 子網路，您也可以細分預設主機 ID 部分指定的原始課程為基礎的網路 ID 量度子網路的 IP 位址

自訂子網路遮罩長度，您可以減少用於實際主機收到的位元

若要防止地址和路由問題，您應該確定區段網路上的所有 TCP/IP 電腦都使用相同的子網路遮罩與每個電腦或裝置具有獨特的 IP 位址。

### <a name="planning-exclusion-ranges"></a>規劃範圍排除項目

當您建立範圍 DHCP 伺服器時，您可以指定 IP 位址範圍包含所有的租用 DHCP 戶端，例如電腦與其他裝置以允許 DHCP 伺服器的 IP 位址。 如果您然後並手動設定某些伺服器與其他裝置靜態相同的 IP 位址範圍使用 DHCP 伺服器的 IP 位址，您不小心可以建立 IP 位址衝突，有您和 DHCP 伺服器兩指派相同的 IP 位址不同的裝置。

若要解開這個問題，您可以建立 DHCP 範圍排除項目範圍。 排除項目範圍是介於領域的 IP 位址 DHCP 伺服器不受允許使用連續 IP 位址。 如果您建立排除範圍，DHCP 伺服器不會指派範圍，讓您以手動方式將這些位址指派而不需要建立 IP 位址衝突中的位址。

您可以從排除 IP 位址 distribution DHCP 伺服器建立的每個領域排除範圍。 適用於所有裝置與靜態 IP 位址設定，您應該使用排除項目。 排除的位址應該會包含所有伺服器，非 DHCP 戶端、無磁碟工作站，或其他路由並遠端存取和 PPP 手動指派的 IP 位址。

建議您在使用額外的地址，以配合未來網路成長設定您的範圍排除項目。 下表 ip 10.0.0.1-10.0.0.254 和 255.255.255.0 子網路遮罩領域提供的範例排除範圍。

|設定項目|範例值|
|-----------------------|------------------|
|排除項目範圍開始 IP 位址|10.0.0.1|
|排除項目範圍結束 IP 位址|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>規劃靜態的 TCP/IP 設定

特定裝置，例如路由器、DHCP 伺服器和 DNS 伺服器，必須使用靜態 IP 位址設定。 此外，您可能有其他裝置，例如印表機、想要確保永遠具有相同的 IP 位址。 列出的裝置，您靜態想要設定的每個子網路，並規劃排除範圍您想要使用 DHCP 伺服器上，以確保 DHCP 伺服器不會租用靜態設定裝置的 IP 位址。 排除項目範圍是有限的一連串中排除 DHCP 服務方案的範圍的 IP 位址。 排除項目範圍確保給您網路上的 DHCP 戶端伺服器不提供任何這些範圍中的位址。

例如，如果子網路的 IP 位址範圍是透過 192.168.0.254 192.168.0.1 10 個裝置您想要使用靜態 IP 位址設定，您可以建立排除項目範圍 192.168.0 的。*x*包含十部或多個 IP 位址的範圍：192.168.0.1 透過 192.168.0.15。

在此範例中，使用 10 排除 IP 位址伺服器和其他裝置設定成靜態 IP 位址，還有適用的新裝置，您可能想要新增未來靜態設定五個其他的 IP 位址。 使用這個排除項目範圍，透過 192.168.0.254 192.168.0.16 位址集區與剩餘 DHCP 伺服器。

下表中提供 AD DS 和 DNS 範例額外的設定項目。

|設定項目|範例值|
|-----------------------|------------------|
|網路連接繫結|乙太網路|
|DNS 伺服器設定|DC1.corp.contoso.com|
|慣用的 DNS 伺服器的 IP 位址|10.0.0.2|
|範圍值<br /><br />1.範圍名稱<br />2.開始 IP 位址<br />3.結束 IP 位址<br />4.子網路遮罩<br />5.預設閘道（選擇性）<br />6.租用期間|1.主要子網路<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6.8 天|
|IPv6 DHCP 伺服器操作模式|不支援|

## <a name="bkmk_lab"></a>本指南使用實驗室測試

您可以使用此指南 production 環境中部署之前，DHCP 部署在實驗室測試。 

>[!NOTE]
>如果您不想在實驗室測試組織中部署 DHCP，您可以跳過一節[部署 DHCP](#bkmk_deploy)。

實驗室您的需求會根據您正在使用實體伺服器或 \(VMs\) 虛擬電腦，以及您是否會使用 Active Directory domain 或部署獨立 DHCP 伺服器。 

您可以使用下列資訊，以判斷您需要測試使用本指南 DHCP 部署的最低資源。

### <a name="test-lab-requirements-with-vms"></a>測試與 Vm Lab 需求

若要部署 DHCP 實驗室測試的 Vm 中，您需要下列資源。

網域部署或獨立部署，您將需要設定為 Hyper\ HYPER-V 主機伺服器。

**網域部署**

這個部署需要一個實體伺服器、一個 virtual 切換、兩個 virtual 伺服器，以及一個 virtual client:

您所在的伺服器，在 [HYPER-V 管理員建立下列項目。

1. 一個**內部**virtual 切換。 不會建立**外部**virtual 切換，因為您的測試 Vm 子網路，其中包含 DHCP 伺服器上 Hyper\ HYPER-V 主機時，將會從 DHCP 伺服器收到 IP 位址。 此外，您要部署的測試 DHCP 伺服器可能指派到其他電腦上安裝 Hyper\ HYPER-V 主機的位置的子網路的 IP 位址。
1. 您建立一個 VM 執行為網域控制站的 Active Directory Domain Services 連接到內部 virtual 切換設定 Windows Server 2106。 本指南再比對，此伺服器必須 10.0.0.2 靜態設定的 IP 位址。 部署 AD DS 資訊，會看到一節**部署 DC1** Windows Server 2016 中[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 其中執行，您將會設定為 DHCP 伺服器及本指南使用的 Windows Server 2106 VM 已連接到 virtual 內部切換您建立。 
1. 執行 Windows client 作業系統連接到 virtual 內部一個 VM 切換所建立，而且您將會使用驗證，DHCP 伺服器動態配置 IP 位址和 DHCP 選項來 DHCP 戶端。

**獨立 DHCP 伺服器部署**

此部署需要一個實體伺服器、一 virtual 切換，一個 isp 與一個 virtual client:

您所在的伺服器，在 [HYPER-V 管理員建立下列項目。

1. 一個**內部**virtual 切換。 不會建立**外部**virtual 切換，因為您的測試 Vm 子網路，其中包含 DHCP 伺服器上 Hyper\ HYPER-V 主機時，將會從 DHCP 伺服器收到 IP 位址。 此外，您要部署的測試 DHCP 伺服器可能指派到其他電腦上安裝 Hyper\ HYPER-V 主機的位置的子網路的 IP 位址。
1. 其中執行，您將會設定為 DHCP 伺服器及本指南使用的 Windows Server 2106 VM 已連接到 virtual 內部切換您建立。
1. 執行 Windows client 作業系統連接到 virtual 內部一個 VM 切換所建立，而且您將會使用驗證，DHCP 伺服器動態配置 IP 位址和 DHCP 選項來 DHCP 戶端。

### <a name="test-lab-requirements-with-physical-servers"></a>使用實體伺服器測試 Lab 需求

若要在使用實體伺服器實驗室測試部署 DHCP，您需要下列資源。

**網域部署**

此部署需要一個中樞或切換、兩個實體伺服器和一個實體 client:

1. 一乙太網路中樞或切換的可以連接實體電腦的纜乙太網路
1. 實體電腦上執行為網域控制站 Active Directory Domain Services 與設定 Windows Server 2106。 本指南再比對，此伺服器必須 10.0.0.2 靜態設定的 IP 位址。 部署 AD DS 資訊，會看到一節**部署 DC1** Windows Server 2016 中[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 實體電腦上執行 Windows Server 2106，您將會使用此快速入門設定為 DHCP 伺服器。 
1. 實體電腦上執行 Windows client 作業系統，您將會使用，以確認您的 DHCP 伺服器動態配置 IP 位址和 DHCP 選項來 DHCP 戶端。

>[!NOTE]
>如果您不需要此部署不足，無法測試的電腦，可用於某部測試 AD DS 和 DHCP-不過 production 環境不建議使用此設定。

**獨立 DHCP 伺服器部署**

此部署需要一個中樞或切換、實體伺服器、和一個實體 client:

1. 一乙太網路中樞或切換的可以連接實體電腦的纜乙太網路
2. 實體電腦上執行 Windows Server 2106，您將會使用此快速入門設定為 DHCP 伺服器。 
3. 實體電腦上執行 Windows client 作業系統，您將會使用，以確認您的 DHCP 伺服器動態配置 IP 位址和 DHCP 選項來 DHCP 戶端。


## <a name="bkmk_deploy"></a>部署 DHCP

本章節提供的範例，您可以用來部署 DHCP 伺服器上的 Windows PowerShell 命令。 之前，請先執行這些範例命令伺服器上，您必須修改以符合您的網路及環境的命令。 

例如您執行的命令之前，您應該會取代範例值在下列項目的命令中：

- 電腦名稱
- IP 位址領域範圍為每個您想要設定（每個子網路 1 範圍）的
- 子網路遮罩每個您想要設定的 IP 位址範圍
- 每個領域範圍名稱
- 每個範圍排除項目範圍
- DHCP 選項值，例如預設閘道、網域名稱和或 WINS DNS 伺服器
- 介面名稱

>[!IMPORTANT]
>檢查並修改您的環境中的每個命令之前您執行的命令。

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>安裝 DHCP-要實體的電腦上 VM 的位置？

您所在的電腦上或一樣安裝 DHCP 伺服器角色 \(VM\) Hyper\ HYPER-V 主機上安裝。 如果您正在安裝 DHCP VM 上您想要提供實體網路的 HYPER-V 主機已連接到電腦的 IP 位址指派 DHCP 伺服器，您必須連接 VM virtual 網路介面卡是 HYPER-V Virtual 切換到**外部**。

如需詳細資訊，請查看區段**Virtual 切換建立 HYPER-V 管理員與**主題中的[建立 virtual 網路](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)。

### <a name="run-windows-powershell-as-an-administrator"></a>系統管理員身分執行 Windows PowerShell

您可以使用下列程序，才能執行 Windows PowerShell 以系統管理員權限。

1. 在執行 Windows Server 2016 的電腦，請按一下**[開始]**，然後以滑鼠右鍵按一下 [Windows PowerShell 圖示。 會出現功能表。 

2. 功能表中，按一下 [**更多**，然後按一下 [**以系統管理員身分執行**。 如果出現提示，請輸入認證帳號在電腦上的系統管理員權限。 如果系統管理員等級 account 與您的登入電腦的使用者帳號，您將不會收到 credential 提示。

3. Windows PowerShell 開啟以系統管理員權限。

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>重新命名 DHCP 伺服器，並設定靜態 IP 位址

如果已經執行此動作，您可以使用下列的 Windows PowerShell 命令重新命名 DHCP 伺服器，並設定伺服器的靜態 IP 位址。

**設定靜態 IP 位址**

您可以使用下列命令，將靜態 IP 位址指派給 DHCP 伺服器，並且以正確 DNS 伺服器的 IP 位址設定 DHCP 伺服器 TCP/IP 屬性。 您必須也取代介面名稱與 IP 位址，在此範例中您想要設定您的電腦使用的值。

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

如需下列命令，查看下列主題。

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**將電腦重新命名**

您可以使用下列命令，以重新命名，然後重新開機。

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

如需下列命令，查看下列主題。

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>將電腦加入網域 \(Optional\)

如果您在 Active Directory domain 環境來安裝 DHCP 伺服器，您必須將電腦加入的網域。 Windows PowerShell 開放的系統管理員權限，並更換網域名稱 NetBios 後執行下列命令**CORP**適用於您的環境的值。

    Add-Computer CORP

出現提示時，輸入認證使用者核對有權限加入網域的電腦。 

    Restart-Computer

如需有關 Add-Computer 命令，查看下列主題。

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>安裝 DHCP

電腦重新開機之後，Windows PowerShell 開放的系統管理員權限，並執行下列命令，然後安裝 DHCP。

    Install-WindowsFeature DHCP -IncludeManagementTools

如需有關這個命令的詳細資訊，請查看下列主題。

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>建立 DHCP 安全性群組

若要建立安全性群組，您必須執行 Windows PowerShell、網路殼層 \(netsh\) 命令，並使變成作用中的新群組重新 DHCP 服務。

當您在 DHCP 伺服器上，執行下列 netsh 命令**DHCP 系統管理員**和**DHCP 使用者**中建立安全性群組**本機使用者和群組**DHCP 伺服器上。

    netsh dhcp add securitygroups

下列命令重新開機 DHCP 服務本機電腦上。

    Restart-service dhcpserver

如需下列命令，查看下列主題。

- [網路 Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>授權在 Active Directory \(Optional\) DHCP 伺服器

如果您正在安裝 DHCP 網域環境中，您必須執行下列步驟來授權 DHCP 伺服器網域中運作。

>[!NOTE]
>在 Active Directory 網域中安裝未經授權的 DHCP 伺服器無法正常運作，並不租用 DHCP 戶端 IP 位址。 未經授權 DHCP 伺服器自動停用是正確的 IP 位址指派給您網路上的戶端可防止未經授權的 DHCP 伺服器的安全性功能。

您可以使用下列命令新增 DHCP 伺服器的在 Active Directory 授權 DHCP 伺服器清單。 

>[!NOTE]
>如果您不網域環境，不會執行這個命令。

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

若要驗證 DHCP 伺服器，在 Active Directory 授權，您可以使用下列命令。

    Get-DhcpServerInDC

以下是範例 Windows PowerShell 中顯示的結果。

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

如需下列命令，查看下列主題。

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>通知伺服器管理員該 post\ 安裝 DHCP 設定已完成 \(Optional\)

當您完成 post\-安裝工作，例如建立安全性群組和授權 Active Directory 中的 DHCP 伺服器伺服器管理員可能仍會顯示在使用者介面這部，必須使用 DHCP 文章安裝設定精靈完成 post\ 安裝步驟警示。

您可以防止顯示在伺服器管理員中，設定下列使用此 Windows PowerShell 命令機碼此 now\ 不必要並不正確的訊息。

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

如需有關這個命令的詳細資訊，請查看下列主題。

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>設定伺服器層級 DNS 動態更新設定設定 \(Optional\)

若要使用 DHCP 伺服器 DHCP client 電腦的執行 DNS 動態更新，您可以執行下列命令，此設定。 這是設定，不範圍層級設定，讓它會影響您設定在伺服器上的所有範圍伺服器層級。 這個命令範例也設定 DHCP 伺服器時 DNS 資源記錄戶端的 client 至少到期。

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

您可以使用下列命令 DHCP 伺服器登記或移除 client 記錄 DNS 伺服器使用憑證的設定。 此範例儲存認證 DHCP 伺服器上。 使用第一個命令**取得認證**來建立**PSCredential**物件、，然後儲存中的物件**$Credential**變數。 命令提示您輸入使用者名稱和密碼，所以請確定您所提供的認證帳號，已更新資源記錄 DNS 伺服器上的權限。

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

如需下列命令，查看下列主題。

- [設定-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>設定依舊套用範圍

DHCP 安裝完成後，您可以使用下列命令來設定和啟動依舊套用範圍，建立範圍，排除項目範圍設定 DHCP 選項預設閘道、DNS 伺服器的 IP 位址，以及 DNS 網域名稱。

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

如需下列命令，查看下列主題。

- [新增 DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [新增 DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [設定-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>設定 Corpnet2 範圍 \(Optional\)

如果您有第二個子網路連接到第一個子網路路由器 DHCP 轉接功能的位置，您可以使用下列命令新增第二個範圍，名 Corpnet2 針對此範例。 此範例中也設定排除範圍，以及預設閘道 IP 位址 \（路由器上的 IP 位址 subnet\）Corpnet2 子網路。

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

如果您有其他的子網路的服務，此 DHCP 伺服器，您可以重複這些指令，若要新增的每個子網路的範圍所有命令的參數，使用不同的值。

>[!IMPORTANT]
>請確定 DHCP 郵件轉寄所有路由器 DHCP 戶端和 DHCP 伺服器之間的都設定。 如何設定 DHCP 轉接查看您路由器的文件的資訊。

## <a name="bkmk_verify"></a>請確認伺服器功能

若要確認您 DHCP 伺服器，會提供給 DHCP 戶端的動態配置的 IP 位址，您可以子網路服務連接另一部電腦。 您的網路介面卡和電腦上的電源連接乙太網路電纜之後，它將會從您的 DHCP 伺服器要求 IP 位址。 您可以檢查成功設定使用**ipconfig /all**命令和檢查結果，或透過執行連接測試，例如嘗試存取您的瀏覽器或檔案共用的 Windows 檔案總管或其他應用程式使用 Web 資源。

如果 client 不會收到 DHCP 伺服器的 IP 位址，請執行下列疑難排解步驟。

1. 請確定乙太網路電纜插入都在電腦與乙太網路切換、中心] 或路由器。
1. 如果您 client 電腦插入路由器分開 DHCP 伺服器網路區段，確定路由器設定向前 DHCP 訊息。
1. 請確定 DHCP 伺服器授權 Active Directory 中，執行下列命令來擷取 Active Directory 授權 DHCP 伺服器清單。 [取得-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)。
1. 確保的範圍啟動打開 DHCP 主控台 \ (伺服器管理員中，**工具**，**DHCP**\)，展開伺服器樹檢視範圍，然後 right\ 按一下每個範圍。 如果接下來的功能表包括選擇**Activate**，按一下 [ **Activate**。 \ (如果便會已經觸動範圍，功能表選取範圍就會顯示**停用**。\) 

## <a name="bkmk_dhcpwps"></a>Windows PowerShell 命令 DHCP

下列參考提供命令描述和語法所有 DHCP 伺服器 Windows PowerShell 命令的 Windows Server 2016。 此主題列出的命令依字母順序根據動詞開頭命令，例如**取得**或**設定**。

>[!NOTE]
>在 Windows Server 2012 R2，您無法使用 Windows Server 2016 的命令。

- [DhcpServer 模組](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

下列參考提供命令描述和語法所有 DHCP 伺服器 Windows PowerShell 命令的 Windows Server 2012 R2。 此主題列出的命令依字母順序根據動詞開頭命令，例如**取得**或**設定**。

>[!NOTE]
>Windows Server 2016 中，您可以使用 Windows Server 2012 R2 的命令。

- [Windows PowerShell 中的 DHCP 伺服器 Cmdlet](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>本指南 Windows PowerShell 命令清單

以下是簡單的指令和範例值本指南使用的清單。

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


