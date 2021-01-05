---
title: 使用 Windows PowerShell 部署 AD DHCP
description: 您可以使用本主題來部署 Windows Server 2016 網際網路通訊協定 (IP) 第4版 DHCP 伺服器，此伺服器會提供自動 IP 位址和 DHCP 選項給連線到網路上一或多個子網的 IPv4 DHCP 用戶端。
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 128c9ce3591f9603ac9085b66fc3359d85a892f2
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879777"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>使用 Windows PowerShell 部署 AD DHCP

> 適用於：Windows Server (半年度管道)、Windows Server 2016

本指南提供的指示說明如何使用 Windows PowerShell 部署網際網路通訊協定 (IP) 第4版動態主機設定通訊協定 \( DHCP \)  伺服器，此伺服器會自動指派 ip 位址和 DHCP 選項給連線到網路上一個或多個子網的 IPv4 dhcp 用戶端。

> [!NOTE]
> 若要從 TechNet 資源庫以 Word 格式下載此檔，請參閱 [使用 Windows Server 2016 中的 Windows PowerShell 部署 DHCP](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293)。

使用 DHCP 伺服器指派 IP 位址會導致系統管理額外負荷，因為您不需要針對網路上每一台電腦上的每個網路介面卡手動設定 TCP/IP v4 設定。 使用 DHCP 時，當電腦或其他 DHCP 用戶端連線到您的網路時，會自動執行 TCP/IP v4 設定。

您可以在工作組中將 DHCP 伺服器部署為獨立伺服器，或部署為 Active Directory 網域的一部分。

本指南涵蓋下列各節。

- [DHCP 部署總覽](#bkmk_overview)
- [技術概觀](#bkmk_technologies)
- [規劃 DHCP 部署](#bkmk_plan)
- [在測試實驗室中使用本指南](#bkmk_lab)
- [部署 DHCP](#bkmk_deploy)
- [確認伺服器功能](#bkmk_verify)
- [DHCP 的 Windows PowerShell 命令](#bkmk_dhcpwps)
- [本指南中的 Windows PowerShell 命令清單](#bkmk_list)

## <a name="dhcp-deployment-overview"></a><a name="bkmk_overview"></a>DHCP 部署總覽

下圖描述您可以使用本指南來部署的案例。 此案例在 Active Directory 網域中包含一部 DHCP 伺服器。 伺服器設定為在兩個不同的子網上，提供 IP 位址給 DHCP 用戶端。 子網會以已啟用 DHCP 轉送的路由器分隔。

![DHCP 網路拓撲總覽](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="technology-overviews"></a><a name="bkmk_technologies"></a>技術概觀

下列各節提供 DHCP 和 TCP/IP 的簡短概述。

### <a name="dhcp-overview"></a>DHCP 總覽

DHCP 是簡化主機 IP 設定管理的 IP 標準。 DHCP 標準提供使用 DHCP 伺服器做為管理動態 IP 位址配置的方法，以及網路上啟用 DHCP 的用戶端的其他相關設定詳細資料。

DHCP 可讓您使用 DHCP 伺服器，以動態方式將 IP 位址指派給您區域網路上的電腦或其他裝置（如印表機），而不是使用靜態 IP 位址手動設定每個裝置。

TCP/IP 網路上的每部電腦都必須擁有唯一的 IP 位址，因為 IP 位址及其相關的子網路遮罩會識別主機電腦和電腦所連線的子網路。 藉由使用 DHCP，您可以確定已設定為 DHCP 用戶端的所有電腦都會收到適合其網路位置和子網路的 IP 位址，而且藉由使用 DHCP 選項 (例如，預設閘道和 DNS 伺服器)，您可以自動為 DHCP 用戶端提供它們在您網路上正常運作所需的資訊。

若是以 TCP/IP 為基礎的網路，DHCP 可減少與設定電腦有關的複雜性和管理工作數量。

### <a name="tcpip-overview"></a>TCP/IP 概觀

根據預設，所有版本的 Windows Server 和 Windows 用戶端作業系統都有 TCP/IP 設定為從 DHCP 伺服器自動取得 IP 位址和其他資訊（稱為 DHCP 選項）的 IP 第4版網路連線。 因此，您不需要手動設定 TCP/IP 設定，除非電腦是伺服器電腦或需要手動設定之靜態 IP 位址的其他裝置。

例如，建議您手動設定 DHCP 伺服器的 IP 位址，以及 Active Directory Domain Services AD DS 執行的 DNS 伺服器和網域控制站的 IP 位址 \( \) 。

Windows Server 2016 中的 TCP/IP 如下：

- 以工業標準網路通訊協定為基礎的網路軟體。

- 支援 Windows 電腦連線到區域網路 (LAN) 與廣域網路 (WAN) 環境的可路由、企業網路通訊協定。

- 將 Windows 電腦連線到相異系統以共用資訊的核心技術與公用程式。

- 取得全域網際網路服務存取權的基礎，例如 Web 和檔案傳輸通訊協定 (FTP) 伺服器。

- 穩固、可擴充及跨平台的用戶端/伺服器架構。

TCP/IP 提供基本的 TCP/IP 公用程式，可讓 Windows 電腦與其他 Microsoft 及非 Microsoft 系統連線和共用資訊，其中包括：

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

- Apple Macintosh 系統

- IBM 主機

- UNIX 和 Linux 系統

- Open VMS 系統

- 網路就緒印表機

- 啟用有線乙太網路或無線802.11 技術的平板電腦和行動電話電話

## <a name="plan-dhcp-deployment"></a><a name="bkmk_plan"></a>規劃 DHCP 部署

安裝 DHCP 伺服器角色之前，請先執行下列重要的規劃步驟。

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>規劃 DHCP 伺服器與 DHCP 轉寄

因為 DHCP 訊息是廣播訊息，所以路由器不會在子網路之間轉寄它們。 如果您有多個子網路，而且想要為每個子網路提供 DHCP 服務，就必須執行下列其中一項：

- 在每個子網路安裝 DHCP 伺服器

- 設定路由器跨越子網路轉寄 DHCP 廣播訊息，在 DHCP 伺服器上設定多個領域，每個子網路一個領域。

在多數狀況下，設定路由器轉寄 DHCP 廣播訊息，比在網路的每個實體區段部署 DHCP 伺服器更符合成本效益。

### <a name="planning-ip-address-ranges"></a>規劃 IP 位址範圍

每個子網路都必須有它自己的唯一 IP 位址範圍。 這些範圍在 DHCP 伺服器上是以領域表示。

領域是一組電腦的 IP 位址，具有管理性質，位於使用 DHCP 服務的子網路上。 系統管理員首先為每個實體子網路建立一個領域，然後使用領域來定義用戶端所使用的參數。

領域具有下列內容：

- IP 位址範圍，從此範圍包括或排除用於 DHCP 服務租用供應項目的位址。

- 子網路遮罩，決定指定 IP 位址的子網路首碼。

- 建立領域時指派的領域名稱。

- 租用期間值，指派給 DHCP 用戶端，接收動態配置的 IP 位址。

- 為指派給 DHCP 用戶端所設定的任何 DHCP 領域選項，例如，DNS 伺服器 IP 位址以及路由器/預設閘道 IP 位址。

- 保留項目可選擇性地用來確保 DHCP 用戶端永遠接收相同的 IP 位址。

在部署伺服器之前，請列出您的子網路以及用於每個子網路的 IP 位址範圍。

### <a name="planning-subnet-masks"></a>規劃子網路遮罩

使用子網路遮罩來區別 IP 位址內的網路識別碼與主機識別碼。 每個子網路遮罩都是 32 位元數字，使用全部都是一 (1) 的連續位元群組來識別網路識別碼，使用全部都是零 (0) 的連續位元群組來識別 IP 位址的主機識別碼部分。

例如，通常搭配 IP 位址 131.107.16.200 使用的子網路遮罩，是下列 32 位元二進位數：

```
11111111 11111111 00000000 00000000
```

這個子網路遮罩數字是 16 個 1 位元加上 16 個 0 位元，表示 IP 位址的網路識別碼與主機識別碼區段的長度都是 16 位元。 一般來說，這個子網路遮罩會以小數點十進位表示法顯示為 255.255.0.0。

下表顯示網際網路位址類別的子網路遮罩。

|位址類別|子網路遮罩的位元|子網路遮罩|
|-----------------|------------------------|---------------|
|類別 A|11111111 00000000 00000000 00000000|255.0.0.0|
|類別 B|11111111 11111111 00000000 00000000|255.255.0.0|
|類別 C|11111111 11111111 11111111 00000000|255.255.255.0|

在 DHCP 中建立領域並輸入領域的 IP 位址範圍時，DHCP 會提供這些預設子網路遮罩值。 通常，大部分沒有特殊需求的網路都能接受預設的子網路遮罩值，而這其中的每個 IP 網路區段都會對應至單一實體網路。

在某些狀況下，您可以使用自訂的子網路遮罩來實作 IP 子網路。 利用 IP 子網路，您可以細分 IP 位址的預設主機識別碼部分以指定子網路，這是原始類別型網路識別碼的細分。

透過自訂子網路遮罩長度，您可以減少用於實際主機識別碼的位元數。

若要防止定址及路由問題，您應該確定網路區段上的所有 TCP/IP 電腦都使用相同的子網路遮罩，且每一部電腦或裝置都有唯一的 IP 位址。

### <a name="planning-exclusion-ranges"></a>規劃排除範圍

當您在 DHCP 伺服器上建立領域時，可以指定一個 IP 位址範圍，其中包含 DHCP 伺服器可租用給 DHCP 用戶端 (例如電腦和其他裝置) 的所有 IP 位址。 如果您接著執行並利用來自 DHCP 伺服器使用的相同 IP 位址範圍的靜態 IP 位址來設定部分伺服器和其他裝置，則您會意外造成 IP 位址衝突，您和 DHCP 伺服器會將相同的 IP 位址指派給不同的裝置。

若要解決此問題，您可以為 DHCP 領域建立排除範圍。 排除範圍是範圍 IP 位址範圍內，不允許 DHCP 伺服器使用的連續 IP 位址範圍。 如果您建立排除範圍，DHCP 伺服器就不會指派該範圍中的位址，讓您能夠手動指派這些位址，而不會產生 IP 位址衝突。

您可以透過 DHCP 伺服器建立每個領域的排除範圍，排除分配的 IP 位址。 使用靜態 IP 位址設定的所有裝置，都應該予以排除。 排除的位址應該包括手動設定給其他伺服器、非 DHCP 用戶端、無磁碟工作站或路由及遠端存取以及 PPP 用戶端的所有 IP 位址。

建議您使用額外的位址來設定排除範圍，以因應未來的網路成長。 下表針對 IP 位址範圍為 10.0.0.1 - 10.0.0.254 且子網路遮罩為 255.255.255.0 的領域，提供排除範圍範例。

|設定項目|範例值|
|-----------------------|------------------|
|排除範圍起始 IP 位址|10.0.0.1|
|排除範圍結束 IP 位址|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>規劃 TCP/IP 靜態設定

如路由器、DHCP 伺服器以及 DNS 伺服器的特定裝置都必須設定為使用靜態 IP 位址。 此外，您可能想要確定其他裝置永遠使用相同的 IP 位址，例如印表機。 列出您要為每個子網路靜態設定的裝置，然後規劃用於 DHCP 伺服器的排除範圍，以確保 DHCP 伺服器不會租用靜態設定裝置的 IP 位址。 排除範圍是領域中要從 DHCP 服務供應項目中排除的有限 IP 位址順序。 排除範圍假設伺服器不會提供這些範圍中的任何位址給網路上的 DHCP 用戶端。

例如，如果子網路的 IP 位址範圍是 192.168.0.1 至 192.168.0.254，而且您有十個裝置要設定為使用靜態 IP 位址，則可以為 192.168.0.*x* 領域建立排除範圍，使其包含十個或更多 IP 位址：192.168.0.1 至 192.168.0.15。

在這個範例中，您使用十個已排除的 IP 位址來設定伺服器與其他裝置使用靜態 IP 位址，五個額外的 IP 位址保留供您未來可能新增之裝置的靜態設定使用。 利用此排除範圍，為 DHCP 伺服器保留的位址集區為 192.168.0.16 至 192.168.0.254。

下表提供 AD DS 與 DNS 設定項目的其他範例。

|設定項目|範例值|
|-----------------------|------------------|
|網路連線繫結|乙太網路|
|DNS 伺服器設定|DC1.corp.contoso.com|
|慣用 DNS 伺服器 IP 位址|10.0.0.2|
|範圍值<p>1. 範圍名稱<br />2. 起始 IP 位址<br />3. 結束 IP 位址<br />4. 子網路遮罩<br />5. 預設閘道 (選用) <br />6. 租用持續時間|1. 主要子網<br />2. 10.0.0。1<br />3. 10.0.0.254<br />4. 255.255.255。0<br />5. 10.0.0。1<br />6. 8 天|
|IPv6 DHCP 伺服器操作模式|未啟用|

## <a name="using-this-guide-in-a-test-lab"></a><a name="bkmk_lab"></a>在測試實驗室中使用本指南

您可以使用本指南，在部署于生產環境之前，在測試實驗室中部署 DHCP。

> [!NOTE]
> 如果您不想要在測試實驗室中部署 DHCP，您可以跳到「 [部署 dhcp](#bkmk_deploy)」一節。

實驗室的需求會因您使用的是實體伺服器或虛擬機器 \( vm \) ，以及您是使用 Active Directory 網域或部署獨立 DHCP 伺服器而有所不同。

您可以使用下列資訊來決定使用本指南測試 DHCP 部署所需的最少資源。

### <a name="test-lab-requirements-with-vms"></a>使用 Vm 測試實驗室需求

若要在具有 Vm 的測試實驗室中部署 DHCP，您需要下列資源。

針對網域部署或獨立部署，您需要一部設定為 Hyper-v 主機的伺服器 \- 。

**網域部署**

此部署需要一部實體伺服器、一個虛擬交換器、兩部虛擬伺服器，以及一個虛擬用戶端：

在您的實體伺服器上，在 [Hyper-v 管理員] 中建立下列專案。

1. 一個 **內部** 虛擬交換器。 請勿建立 **外部** 虛擬交換器，因為如果您的 hyper-v \- 主機位於包含 dhcp 伺服器的子網，您的測試 vm 將會收到來自 DHCP 伺服器的 IP 位址。 此外，您部署的測試 DHCP 伺服器可能會將 IP 位址指派給安裝 Hyper-v 主機的子網上的其他電腦 \- 。
1. 一部執行 Windows Server 2016 的 VM 已設定為網域控制站，且 Active Directory Domain Services 連接到您建立的內部虛擬交換器。 為了符合本指南的規定，此伺服器必須具有靜態設定的 IP 位址10.0.0.2。 如需部署 AD DS 的詳細資訊，請參閱《 Windows Server 2016 [核心網路指南》](../../core-network-guide/core-network-guide.md#BKMK_deployADDNS01)中的 **部署 DC1** 一節。
1. 一部執行 Windows Server 2016 的 VM，您將使用本指南將其設定為 DHCP 伺服器，並連接到您建立的內部虛擬交換器。
1. 一部執行 Windows 用戶端作業系統的 VM，該作業系統會連接到您建立的內部虛擬交換器，您將使用這些 VM 來確認 DHCP 伺服器是否動態地將 IP 位址和 DHCP 選項配置給 DHCP 用戶端。

**獨立 DHCP 伺服器部署**

此部署需要一部實體伺服器、一個虛擬交換器、一個虛擬伺服器和一個虛擬用戶端：

在您的實體伺服器上，在 [Hyper-v 管理員] 中建立下列專案。

1. 一個 **內部** 虛擬交換器。 請勿建立 **外部** 虛擬交換器，因為如果您的 hyper-v \- 主機位於包含 dhcp 伺服器的子網，您的測試 vm 將會收到來自 DHCP 伺服器的 IP 位址。 此外，您部署的測試 DHCP 伺服器可能會將 IP 位址指派給安裝 Hyper-v 主機的子網上的其他電腦 \- 。
2. 一部執行 Windows Server 2016 的 VM，您將使用本指南將其設定為 DHCP 伺服器，並連接到您建立的內部虛擬交換器。
3. 一部執行 Windows 用戶端作業系統的 VM，該作業系統會連接到您建立的內部虛擬交換器，您將使用這些 VM 來確認 DHCP 伺服器是否動態地將 IP 位址和 DHCP 選項配置給 DHCP 用戶端。

### <a name="test-lab-requirements-with-physical-servers"></a>使用實體伺服器測試實驗室需求

若要在具有實體伺服器的測試實驗室中部署 DHCP，您需要下列資源。

**網域部署**

此部署需要一個中樞或交換器、兩部實體伺服器和一個實體用戶端：

1. 一個乙太網路集線器或交換器，可讓您將實體電腦與乙太網路纜線連線
2. 一部執行 Windows Server 2016 的實體電腦，設定為具有 Active Directory Domain Services 的網域控制站。 為了符合本指南的規定，此伺服器必須具有靜態設定的 IP 位址10.0.0.2。 如需部署 AD DS 的詳細資訊，請參閱《 Windows Server 2016 [核心網路指南》](../../core-network-guide/core-network-guide.md#BKMK_deployADDNS01)中的 **部署 DC1** 一節。
3. 一部執行 Windows Server 2016 的實體電腦，您將使用本指南將其設定為 DHCP 伺服器。
4. 一部執行 Windows 用戶端作業系統的實體電腦，您將使用這些電腦來確認 DHCP 伺服器是否動態地將 IP 位址和 DHCP 選項配置給 DHCP 用戶端。

> [!NOTE]
> 如果您沒有足夠的測試電腦來進行這項部署，您可以使用一部測試電腦進行 AD DS 和 DHCP-不過，不建議在生產環境中使用此設定。

**獨立 DHCP 伺服器部署**

此部署需要一個中樞或交換器、一部實體伺服器，以及一個實體用戶端：

1. 一個乙太網路集線器或交換器，可讓您將實體電腦與乙太網路纜線連線
2. 一部執行 Windows Server 2016 的實體電腦，您將使用本指南將其設定為 DHCP 伺服器。
3. 一部執行 Windows 用戶端作業系統的實體電腦，您將使用這些電腦來確認 DHCP 伺服器是否動態地將 IP 位址和 DHCP 選項配置給 DHCP 用戶端。


## <a name="deploy-dhcp"></a><a name="bkmk_deploy"></a>部署 DHCP

本節提供您可用來在一部伺服器上部署 DHCP 的範例 Windows PowerShell 命令。 在您的伺服器上執行這些範例命令之前，您必須修改命令以符合您的網路和環境。

例如，在執行命令之前，您應該將命令中的範例值取代為下列專案：

- 電腦名稱
- 您要設定的每個範圍的 IP 位址範圍 (1 個子網的範圍) 
- 您要設定之每個 IP 位址範圍的子網路遮罩
- 每個範圍的範圍名稱
- 每個範圍的排除範圍
- DHCP 選項值，例如預設閘道、功能變數名稱和 DNS 或 WINS 伺服器
- 介面名稱

> [!IMPORTANT]
> 請先檢查並修改環境的每個命令，然後再執行命令。

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>在實體電腦或 VM 上安裝 DHCP 的位置？

您可以在實體電腦或 \( \) 安裝在 hyper-v 主機上的虛擬機器 VM 上安裝 DHCP 伺服器角色 \- 。 如果您要在 VM 上安裝 DHCP，而且想要讓 DHCP 伺服器將 IP 位址指派提供給 Hyper-v 主機所連線之實體網路上的電腦，您必須將 VM 虛擬網路介面卡連接到 **外部** 的 Hyper-v 虛擬交換器。

如需詳細資訊，請參閱 [建立虛擬網路](/virtualization/hyper-v-on-windows/quick-start/connect-to-network)主題中的 **使用 Hyper-v 管理員建立虛擬交換器** 一節。

### <a name="run-windows-powershell-as-an-administrator"></a>以系統管理員身分執行 Windows PowerShell

您可以使用下列程式，以系統管理員許可權來執行 Windows PowerShell。

1. 在執行 Windows Server 2016 的電腦上，按一下 [ **開始**]，然後以滑鼠右鍵按一下 Windows PowerShell 圖示。 功能表隨即出現。

2. 在功能表中，按一下 [ **其他**]，然後按一下 [以 **系統管理員身分執行**]。 如果出現提示，請輸入在電腦上具有系統管理員許可權之帳戶的認證。 如果您用來登入電腦的使用者帳戶是系統管理員層級帳戶，您將不會收到認證提示。

3. Windows PowerShell 會以系統管理員許可權開啟。

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>重新命名 DHCP 伺服器並設定靜態 IP 位址

如果您尚未這麼做，您可以使用下列 Windows PowerShell 命令重新命名 DHCP 伺服器，並設定伺服器的靜態 IP 位址。

**設定靜態 IP 位址**

您可以使用下列命令，將靜態 IP 位址指派給 DHCP 伺服器，並使用正確的 DNS 伺服器 IP 位址來設定 DHCP 伺服器 TCP/IP 屬性。 您也必須使用您要用來設定電腦的值，取代這個範例中的介面名稱和 IP 位址。

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
```

如需這些命令的詳細資訊，請參閱下列主題。

- [新 New-netipaddress](/powershell/module/nettcpip/New-NetIPAddress)
- [設定-Set-dnsclientserveraddress](/powershell/module/dnsclient/Set-DnsClientServerAddress)

**重新命名電腦**

您可以使用下列命令來重新命名，然後重新開機電腦。

```
Rename-Computer -Name DHCP1
Restart-Computer
```

如需這些命令的詳細資訊，請參閱下列主題。

- [Rename-Computer](/powershell/module/microsoft.powershell.management/rename-computer)
- [Restart-Computer](/powershell/module/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>將電腦加入網域 \( 選用\)

如果您要將 DHCP 伺服器安裝在 Active Directory 網域環境中，就必須將電腦加入網域。 以系統管理員許可權開啟 Windows PowerShell，然後在將網域 NetBios 名稱 **CORP** 取代為適合您環境的值之後，執行下列命令。

```
Add-Computer CORP
```

出現提示時，請輸入有權將電腦加入網域之網域使用者帳戶的認證。

```
Restart-Computer
```

如需 Add-Computer 命令的詳細資訊，請參閱下列主題。

- [Add-Computer](/powershell/module/microsoft.powershell.management/add-computer?view=powershell-5.1&preserve-view=true)

### <a name="install-dhcp"></a>安裝 DHCP

電腦重新開機之後，以系統管理員許可權開啟 Windows PowerShell，然後執行下列命令以安裝 DHCP。

```
Install-WindowsFeature DHCP -IncludeManagementTools
```

如需此命令的詳細資訊，請參閱下列主題。

- [Install-](/powershell/module/servermanager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>建立 DHCP 安全性群組

若要建立安全性群組，您必須 \( 在 Windows PowerShell 中執行 Network Shell netsh \) 命令，然後重新開機 DHCP 服務，讓新的群組變成作用中狀態。

當您在 DHCP 伺服器上執行下列 netsh 命令時，會在 DHCP 伺服器上的 [**本機使用者和群組**] 中建立 **dhcp 系統管理員** 和 **dhcp 使用者** 安全性群組。

```
netsh dhcp add securitygroups
```

下列命令會重新開機本機電腦上的 DHCP 服務。

```
Restart-Service dhcpserver
```

如需這些命令的詳細資訊，請參閱下列主題。

- [網路殼層 (Netsh)](../netsh/netsh.md)
- [Restart-Service](/powershell/module/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Active Directory 選擇性將 DHCP 伺服器授權 \(\)

如果您是在網域環境中安裝 DHCP，您必須執行下列步驟，以授權 DHCP 伺服器在網域中操作。

> [!NOTE]
> 安裝在 Active Directory 網域中的未授權 DHCP 伺服器無法正常運作，且不會將 IP 位址租用給 DHCP 用戶端。 自動停用未授權的 DHCP 伺服器是一項安全性功能，可防止未經授權的 DHCP 伺服器將不正確的 IP 位址指派給網路上的用戶端。

您可以使用下列命令，將 DHCP 伺服器新增至 Active Directory 中授權 DHCP 伺服器的清單。

> [!NOTE]
> 如果您沒有網域環境，請勿執行此命令。

```
Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
```

若要確認 DHCP 伺服器已獲得 Active Directory 的授權，您可以使用下列命令。

```
Get-DhcpServerInDC
```

以下是 Windows PowerShell 中顯示的範例結果。

```
IPAddress   DnsName
---------   -------
10.0.0.3    DHCP1.corp.contoso.com
```

如需這些命令的詳細資訊，請參閱下列主題。

- [新增-DhcpServerInDC](/powershell/module/dhcpserver/add-dhcpserverindc)
- [DhcpServerInDC](/powershell/module/dhcpserver/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>通知伺服器管理員後置 \- 安裝 DHCP 設定已完成 \( 選擇性\)

當您完成後續 \- 安裝工作（例如建立安全性群組並授權 Active Directory 中的 DHCP 伺服器）之後，伺服器管理員可能仍會在使用者介面中顯示警示，說明 \- 必須使用 DHCP post 安裝設定向導完成後續安裝步驟。

您可以 \- 使用此 Windows PowerShell 命令設定下列登錄機碼，以防止在伺服器管理員中出現不必要和不正確的訊息。

```
Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
```

如需此命令的詳細資訊，請參閱下列主題。

- [Set-ItemProperty](/powershell/module/microsoft.powershell.management/set-itemproperty)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>設定伺服器層級 DNS 動態更新設定 \( 選擇性\)

如果您想要讓 DHCP 伺服器針對 DHCP 用戶端電腦執行 DNS 動態更新，您可以執行下列命令來設定此設定。 這是伺服器層級設定，而不是範圍層級設定，因此它會影響您在伺服器上設定的所有範圍。 此範例命令也會設定 DHCP 伺服器，以便在用戶端最少過期時，刪除用戶端的 DNS 資源記錄。

```
Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
```

您可以使用下列命令來設定 DHCP 伺服器用來在 DNS 伺服器上註冊或取消註冊用戶端記錄的認證。 此範例會將認證儲存在 DHCP 伺服器上。 第一個命令會使用 **Get-Credential** 來建立 **PSCredential** 物件，然後將物件儲存在 **$Credential** 變數中。 此命令會提示您輸入使用者名稱和密碼，因此請確定您為具有在 DNS 伺服器上更新資源記錄許可權的帳戶提供認證。

```
$Credential = Get-Credential
Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
```

如需這些命令的詳細資訊，請參閱下列主題。

- [Set-DhcpServerv4DnsSetting](/powershell/module/dhcpserver/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](/powershell/module/dhcpserver/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>設定公司網路範圍

完成 DHCP 安裝之後，您可以使用下列命令來設定和啟動網路功能範圍、建立範圍的排除範圍，以及設定 DHCP 選項預設閘道、DNS 伺服器 IP 位址和 DNS 功能變數名稱。

```
Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
```

如需這些命令的詳細資訊，請參閱下列主題。

- [新增-DhcpServerv4Scope](/powershell/module/dhcpserver/Add-DhcpServerv4Scope)
- [新增-DhcpServerv4ExclusionRange](/powershell/module/dhcpserver/Add-DhcpServerv4ExclusionRange)
- [設定-DhcpServerv4OptionValue](/powershell/module/dhcpserver/Set-DhcpServerv4OptionValue)

### <a name="configure-the-corpnet2-scope-optional"></a>將 Corpnet2 範圍設定為 \( 選擇性\)

如果您有第二個子網連線到具有已啟用 DHCP 轉送之路由器的第一個子網，您可以使用下列命令來新增此範例中名為 Corpnet2 的第二個範圍。 此範例也會針對預設閘道設定排除範圍和 IP 位址，以 \( Corpnet2 子網的子網作為路由器 IP 位址 \) 。

```
Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
```

如果您有此 DHCP 伺服器所服務的其他子網，您可以重複這些命令，針對所有的命令參數使用不同的值，以新增每個子網的範圍。

> [!IMPORTANT]
> 確定 DHCP 用戶端和 DHCP 伺服器之間的所有路由器都已設定 DHCP 訊息轉送。 如需有關如何設定 DHCP 轉送的詳細資訊，請參閱您的路由器檔。

## <a name="verify-server-functionality"></a><a name="bkmk_verify"></a>確認伺服器功能

若要確認 DHCP 伺服器將 IP 位址的動態配置提供給 DHCP 用戶端，您可以將另一部電腦連線到服務子網。 將乙太網路纜線連接到網路介面卡和電腦電源之後，它會向您的 DHCP 伺服器要求 IP 位址。 您可以使用 **ipconfig/all** 命令並檢查結果，或執行連線測試（例如嘗試使用瀏覽器或檔案共用搭配 Windows 檔案總管或其他應用程式）來存取 Web 資源，以驗證成功的設定。

如果用戶端未收到來自 DHCP 伺服器的 IP 位址，請執行下列疑難排解步驟。

1. 確定乙太網路纜線已插入電腦和乙太網路交換器、集線器或路由器。
2. 如果您將用戶端電腦插入網路區段（由路由器與 DHCP 伺服器分隔），請確定已將路由器設定為轉寄 DHCP 訊息。
3. 藉由執行下列命令，從 Active Directory 取出授權 DHCP 伺服器的清單，以確定 DHCP 伺服器已獲得 Active Directory 授權。 [DhcpServerInDC](/powershell/module/dhcpserver/get-dhcpserverindc)。
4. 開啟 DHCP 主控台 \( 伺服器管理員、**工具**、 **DHCP** \) 、展開伺服器樹狀目錄以檢查範圍，然後以滑鼠右鍵 \- 按一下每個範圍，以確定您的範圍已啟動。 如果產生的功能表包含選取專案 **啟用**，請按一下 [ **啟用**]。 \(如果已啟用範圍，則功能表選取專案會讀取 [ **停用**]。\)

## <a name="windows-powershell-commands-for-dhcp"></a><a name="bkmk_dhcpwps"></a>DHCP 的 Windows PowerShell 命令

下列參考提供 Windows Server 2016 的所有 DHCP 伺服器 Windows PowerShell 命令的命令說明和語法。 本主題依據命令開頭的動詞命令，以字母順序列出命令，例如 **Get** 或 **Set**。

> [!NOTE]
> 您不能使用 Windows Server 2012 R2 中的 Windows Server 2016 命令。

- [DhcpServer 模組](/powershell/module/dhcpserver/)

下列參考提供適用于 Windows Server 2012 R2 的所有 DHCP 伺服器 Windows PowerShell 命令的命令說明和語法。 本主題依據命令開頭的動詞命令，以字母順序列出命令，例如 **Get** 或 **Set**。

> [!NOTE]
> 您可以使用 Windows Server 2016 中的 Windows Server 2012 R2 命令。

- [Windows PowerShell 中的 DHCP 伺服器 Cmdlet]()

## <a name="list-of-windows-powershell-commands-in-this-guide"></a><a name="bkmk_list"></a>本指南中的 Windows PowerShell 命令清單

以下是本指南中使用的簡單命令清單和範例值。

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
Rename-Computer -Name DHCP1
Restart-Computer

Add-Computer CORP
Restart-Computer

Install-WindowsFeature DHCP -IncludeManagementTools
netsh dhcp add securitygroups
Restart-Service dhcpserver

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
```
