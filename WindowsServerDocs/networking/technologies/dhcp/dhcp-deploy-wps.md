---
title: 使用 Windows PowerShell 部署 AD DHCP
description: 您可以使用本主題將提供自動的 IP 位址及 DHCP 選項來連線到您網路上的一個或多個子網路的 IPv4 DHCP 用戶端的 Windows Server 2016 網際網路通訊協定 (IP) 第 4 版 DHCP 伺服器。
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849089"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>使用 Windows PowerShell 部署 AD DHCP

>適用於：Windows Server （半年通道），Windows Server 2016

本指南說明如何使用 Windows PowerShell 來部署網際網路通訊協定 (IP) 第 4 版動態主機設定通訊協定\(DHCP\) IPv4 dhcp 的伺服器，會自動指派 IP 位址及 DHCP 選項連線到您網路上的一個或多個子網路的用戶端。

>[!NOTE]
>若要從 TechNet 組件庫下載這份文件以 Word 格式，請參閱[部署 DHCP 使用 Windows PowerShell 在 Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293)。

使用 DHCP 伺服器指派 IP 位址會將儲存在系統管理額外負荷，因為您不需要以手動方式設定每個網路介面卡的 TCP/IP v4 設定您的網路上的每一部電腦。 使用 DHCP 的電腦時自動執行 TCP/IP v4 設定，或其他的 DHCP 用戶端連線到網路。

您可以部署 DHCP 伺服器在工作群組中的，為獨立伺服器，或做為 Active Directory 網域的一部分。

本指南涵蓋下列各節。

- [DHCP 部署概觀](#bkmk_overview)
- [技術概觀](#bkmk_technologies)
- [規劃 DHCP 部署](#bkmk_plan)
- [使用本指南中的測試實驗室](#bkmk_lab)
- [部署 DHCP](#bkmk_deploy)
- [確認伺服器功能](#bkmk_verify)
- [DHCP 的 Windows PowerShell 命令](#bkmk_dhcpwps)
- [本指南中的 Windows PowerShell 命令清單](#bkmk_list)

## <a name="bkmk_overview"></a>DHCP 部署概觀

下圖說明您可以使用本指南來部署的案例。 此案例包括一部 DHCP 伺服器在 Active Directory 網域中。 伺服器被設定為兩個不同的子網路上 DHCP 用戶端提供 IP 位址。 已啟用 DHCP 轉寄的路由器以分隔的子網路。

![DHCP 網路拓撲概觀](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>技術概觀

下列各節提供 DHCP 與 TCP/IP 的簡短的概觀。

### <a name="dhcp-overview"></a>DHCP 概觀

DHCP 是簡化主機 IP 設定管理的 IP 標準。 DHCP 標準提供使用 DHCP 伺服器做為管理動態 IP 位址配置的方法，以及網路上啟用 DHCP 的用戶端的其他相關設定詳細資料。

DHCP 可讓您以動態方式指派給電腦或其他裝置，例如印表機，在您的區域網路，而不是手動使用靜態 IP 位址設定每個裝置上的 IP 位址使用 DHCP 伺服器。

TCP/IP 網路上的每部電腦都必須擁有唯一的 IP 位址，因為 IP 位址及其相關的子網路遮罩會識別主機電腦和電腦所連線的子網路。 藉由使用 DHCP，您可以確定已設定為 DHCP 用戶端的所有電腦都會收到適合其網路位置和子網路的 IP 位址，而且藉由使用 DHCP 選項 (例如，預設閘道和 DNS 伺服器)，您可以自動為 DHCP 用戶端提供它們在您網路上正常運作所需的資訊。

針對 TCP 架構網路，DHCP 可減少複雜度和相關設定電腦的系統管理工作的數量。

### <a name="tcpip-overview"></a>TCP/IP 概觀

根據預設，所有版本的 Windows Server 和 Windows 用戶端作業系統都已設定為自動取得 IP 位址和其他資訊，稱為 DHCP 選項，從 DHCP 伺服器的 IP 版本 4 的網路連線的 TCP/IP 設定。 基於這個原因，您不需要手動設定 TCP/IP 設定，除非電腦是伺服器電腦或其他需要手動設定的靜態 IP 位址的裝置。 

例如，建議您手動設定 DHCP 伺服器的 IP 位址和 DNS 伺服器和執行 Active Directory 網域服務的網域控制站的 IP 位址\(AD DS\)。

Windows Server 2016 中的 TCP/IP 如下：

-   以工業標準網路通訊協定為基礎的網路軟體。

-   支援 Windows 電腦連線到區域網路 (LAN) 與廣域網路 (WAN) 環境的可路由、企業網路通訊協定。

-   將 Windows 電腦連線到相異系統以共用資訊的核心技術與公用程式。

-   取得全域網際網路服務，例如 Web 和檔案傳輸通訊協定 (FTP) 伺服器的存取權之基礎。

-   穩固、可擴充及跨平台的用戶端/伺服器架構。

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

- 網路印表機

- 平板電腦和行動電話與有線乙太網路或無線的 802.11 技術啟用

## <a name="bkmk_plan"></a>規劃 DHCP 部署

下面是之前安裝 DHCP 伺服器角色的主要規劃步驟。

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>規劃 DHCP 伺服器與 DHCP 轉寄

因為 DHCP 訊息是廣播訊息，所以路由器不會在子網路之間轉寄它們。 如果您有多個子網路，而且想要為每個子網路提供 DHCP 服務，就必須執行下列其中一項：

-   在每個子網路安裝 DHCP 伺服器

-   設定路由器跨越子網路轉寄 DHCP 廣播訊息，在 DHCP 伺服器上設定多個領域，每個子網路一個領域。

在多數狀況下，設定路由器轉寄 DHCP 廣播訊息，比在網路的每個實體區段部署 DHCP 伺服器更符合成本效益。

### <a name="planning-ip-address-ranges"></a>規劃 IP 位址範圍

每個子網路都必須有它自己的唯一 IP 位址範圍。 這些範圍在 DHCP 伺服器上是以領域表示。

領域是一組電腦的 IP 位址，具有管理性質，位於使用 DHCP 服務的子網路上。 系統管理員首先為每個實體子網路建立一個領域，然後使用領域來定義用戶端所使用的參數。

領域具有下列內容：

-   IP 位址範圍，從此範圍包括或排除用於 DHCP 服務租用提供的位址。

-   子網路遮罩，決定指定 IP 位址的子網路首碼。

-   建立領域時指派的領域名稱。

-   租用期間值，指派給 DHCP 用戶端，接收動態配置的 IP 位址。

-   為指派給 DHCP 用戶端所設定的任何 DHCP 領域選項，例如，DNS 伺服器 IP 位址以及路由器/預設閘道 IP 位址。

-   保留項目可選擇性地用來確保 DHCP 用戶端永遠接收相同的 IP 位址。

在部署伺服器之前，請列出您的子網路以及用於每個子網路的 IP 位址範圍。

### <a name="planning-subnet-masks"></a>規劃子網路遮罩

使用子網路遮罩來區別 IP 位址內的網路識別碼與主機識別碼。 每個子網路遮罩都是 32 位元數字，使用全部都是一 (1) 的連續位元群組來識別網路識別碼，使用全部都是零 (0) 的連續位元群組來識別 IP 位址的主機識別碼部分。

例如，通常搭配 IP 位址 131.107.16.200 使用的子網路遮罩，是下列 32 位元二進位數：

```
11111111 11111111 00000000 00000000
```

此子網路遮罩數字是 16 一個位元加上 16 0 位元，指出此 IP 位址的網路識別碼與主機識別碼區段長度這兩個 16 位元。 一般來說，這個子網路遮罩會以小數點十進位表示法顯示為 255.255.0.0。

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

若要解決此問題，您可以為 DHCP 領域建立排除範圍。 排除範圍是連續的 IP 位址範圍的 DHCP 伺服器不允許使用的範圍的 IP 位址範圍內。 如果您建立排除範圍，DHCP 伺服器就不會指派該範圍中的位址，讓您能夠手動指派這些位址，而不會產生 IP 位址衝突。

您可以透過 DHCP 伺服器建立每個領域的排除範圍，排除分配的 IP 位址。 使用靜態 IP 位址設定的所有裝置，都應該予以排除。 排除的位址應該包括手動設定給其他伺服器、非 DHCP 用戶端、無磁碟工作站或路由及遠端存取以及 PPP 用戶端的所有 IP 位址。

建議您使用額外的位址來設定排除範圍，以因應未來的網路成長。 下表提供排除範圍範例的範圍，IP 位址範圍為 10.0.0.1-10.0.0.254 且子網路遮罩為 255.255.255.0。

|設定項目|範例值|
|-----------------------|------------------|
|排除範圍起始 IP 位址|10.0.0.1|
|排除範圍結束 IP 位址|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>規劃 TCP/IP 靜態設定

如路由器、DHCP 伺服器以及 DNS 伺服器的特定裝置都必須設定為使用靜態 IP 位址。 此外，您可能想要確定其他裝置永遠使用相同的 IP 位址，例如印表機。 列出您要為每個子網路靜態設定的裝置，然後規劃用於 DHCP 伺服器的排除範圍，以確保 DHCP 伺服器不會租用靜態設定裝置的 IP 位址。 排除範圍是領域中要從 DHCP 服務提供中排除的有限 IP 位址順序。 排除範圍假設伺服器不會提供這些範圍中的任何位址給網路上的 DHCP 用戶端。

例如，如果子網路的 IP 位址範圍是 192.168.0.1 至 192.168.0.254，而且您有十個您想要使用靜態 IP 位址設定的裝置，您可以建立為 192.168.0 排除範圍。*x*包含十個或多個 IP 位址範圍：192.168.0.1 至 192.168.0.15。

在這個範例中，您使用十個已排除的 IP 位址來設定伺服器與其他裝置使用靜態 IP 位址，五個額外的 IP 位址保留供您未來可能新增之裝置的靜態設定使用。 利用此排除範圍，為 DHCP 伺服器保留的位址集區為 192.168.0.16 至 192.168.0.254。

下表提供其他範例的 AD DS 與 DNS 的設定項目。

|設定項目|範例值|
|-----------------------|------------------|
|網路連線繫結|Ethernet|
|DNS 伺服器設定|DC1.corp.contoso.com|
|慣用 DNS 伺服器 IP 位址|10.0.0.2|
|範圍值<br /><br />1.領域名稱<br />2.起始 IP 位址<br />3.結束 IP 位址<br />4.子網路遮罩<br />5.預設閘道 (選擇性)<br />6.租用期間|1.主要子網路<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6.8 天|
|IPv6 DHCP 伺服器操作模式|未啟用|

## <a name="bkmk_lab"></a>使用本指南中的測試實驗室

若要在生產環境中部署之前，將 DHCP 部署測試實驗室中，您可以使用本指南。 

>[!NOTE]
>如果您不想將 DHCP 部署測試實驗室中，您可以跳到節[部署 DHCP](#bkmk_deploy)。

您的實驗室的需求不同，取決於您使用實體伺服器或虛擬機器\(Vm\)，以及您為使用 Active Directory 網域或部署獨立 DHCP 伺服器。 

您可以使用下列資訊來判斷您要測試 DHCP 部署使用本指南的最少資源。

### <a name="test-lab-requirements-with-vms"></a>使用 Vm 的測試實驗室需求

若要部署 Vm 的測試實驗室中的 DHCP，您需要下列資源。

對於網域部署或獨立部署中，您需要一部伺服器設定為 Hyper-v\-主機。

**網域部署**

此部署需要一部實體伺服器、 一個虛擬交換器、 兩個虛擬伺服器和一部虛擬用戶端：

在您的實體伺服器，在 HYPER-V 管理員 中，建立下列項目。

1. 一**內部**虛擬交換器。 不會建立**外部**的虛擬交換器，因為如果您的 Hyper\-主機位於子網路的 DHCP 伺服器上，您的測試 Vm 會接收從 DHCP 伺服器的 IP 位址。 此外，您部署的測試 DHCP 伺服器時，可能到子網路上其他電腦指派 IP 位址，Hyper\-主機已安裝。
1. 您建立一部執行 Windows Server 2016 與連線到內部虛擬交換器的 Active Directory 網域服務的網域控制站設定的 VM。 若要符合本指南，此伺服器必須靜態設定的 IP 位址 10.0.0.2。 如需部署 AD DS 的詳細資訊，請參閱下節**部署 DC1**在 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 其中一個執行 Windows Server 2016，您將使用本指南，並設定為 DHCP 伺服器的 VM 連線到內部虛擬交換器您所建立。 
1. VM 執行 Windows 用戶端作業系統，連線到內部虛擬交換器您所建立，並使用驗證，您的 DHCP 伺服器是以動態方式配置 IP 位址及 DHCP 選項給 DHCP 用戶端。

**獨立 DHCP 伺服器部署**

此部署需要一部實體伺服器、 一個虛擬交換器、 一部虛擬伺服器和一部虛擬用戶端：

在您的實體伺服器，在 HYPER-V 管理員 中，建立下列項目。

1. 一**內部**虛擬交換器。 不會建立**外部**的虛擬交換器，因為如果您的 Hyper\-主機位於子網路的 DHCP 伺服器上，您的測試 Vm 會接收從 DHCP 伺服器的 IP 位址。 此外，您部署的測試 DHCP 伺服器時，可能到子網路上其他電腦指派 IP 位址，Hyper\-主機已安裝。
1. 其中一個執行 Windows Server 2016，您將使用本指南，並設定為 DHCP 伺服器的 VM 連線到內部虛擬交換器您所建立。
1. VM 執行 Windows 用戶端作業系統，連線到內部虛擬交換器您所建立，並使用驗證，您的 DHCP 伺服器是以動態方式配置 IP 位址及 DHCP 選項給 DHCP 用戶端。

### <a name="test-lab-requirements-with-physical-servers"></a>測試實驗室使用實體伺服器的需求

若要部署實體伺服器的測試實驗室中的 DHCP，您需要下列資源。

**網域部署**

此部署需要一個集線器或交換器、 兩部實體伺服器和一部實體用戶端：

1. 一個乙太網路集線器或交換器，您可以使用乙太網路纜線連接的實體電腦
1. 執行 Windows Server 2016 與 Active Directory 網域服務的網域控制站設定的一部實體電腦。 若要符合本指南，此伺服器必須靜態設定的 IP 位址 10.0.0.2。 如需部署 AD DS 的詳細資訊，請參閱下節**部署 DC1**在 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)。
1. 執行 Windows Server 2016 使用本指南將為 DHCP 伺服器設定的一部實體電腦。 
1. 一部執行 Windows 用戶端作業系統，您會使用它來確認您的 DHCP 伺服器的實體電腦以動態方式配置 IP 位址及 DHCP 選項給 DHCP 用戶端。

>[!NOTE]
>如果您沒有足夠的測試電腦，此部署，您可以使用其中一部測試電腦，用於 AD DS 與 DHCP-但這項設定不建議用於生產環境即可。

**獨立 DHCP 伺服器部署**

此部署需要一個集線器或交換器、 一部實體伺服器和一部實體用戶端：

1. 一個乙太網路集線器或交換器，您可以使用乙太網路纜線連接的實體電腦
2. 執行 Windows Server 2016 使用本指南將為 DHCP 伺服器設定的一部實體電腦。 
3. 一部執行 Windows 用戶端作業系統，您會使用它來確認您的 DHCP 伺服器的實體電腦以動態方式配置 IP 位址及 DHCP 選項給 DHCP 用戶端。


## <a name="bkmk_deploy"></a>部署 DHCP

本節提供可用來將 DHCP 部署一部伺服器上的範例 Windows PowerShell 命令。 在您的伺服器上執行這些命令的範例之前，您必須修改以符合您的網路和環境的命令。 

比方說，您執行命令之前，您應該取代下列項目的命令中的範例值：

- 電腦名稱
- 您想要設定 （1 個範圍，每個子網路） 的每個範圍的 IP 位址範圍
- 您想要設定每個 IP 位址範圍的子網路遮罩
- 每個領域的領域名稱
- 每個領域的排除範圍
- DHCP 選項值，例如預設閘道、 網域名稱和 DNS 或 WINS 伺服器
- 介面名稱

>[!IMPORTANT]
>檢查並修改每個命令，為您的環境，然後再執行命令。

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>實體電腦或 VM 上安裝 DHCP-何處？

您可以在實體電腦上或在虛擬機器上安裝 DHCP 伺服器角色\(VM\)安裝於 Hyper-v\-主機。 如果您在 VM 上安裝 DHCP，而且您想要在 HYPER-V 主機所連接的實體網路上提供給電腦的 IP 位址指派之 DHCP 伺服器，您必須連接 VM 的虛擬網路介面卡到 HYPER-V 虛擬交換器**外部**。

如需詳細資訊，請參閱**使用 HYPER-V 管理員建立虛擬交換器**主題中的 <<c4> [ 建立虛擬網路](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)。

### <a name="run-windows-powershell-as-an-administrator"></a>系統管理員身分執行 Windows PowerShell

您可以使用下列程序，以系統管理員權限執行 Windows PowerShell。

1. 在執行 Windows Server 2016 的電腦，按一下 **啟動**，然後以滑鼠右鍵按一下 Windows PowerShell 圖示。 此時會出現一個功能表。 

2. 在功能表中，按一下**更多**，然後按一下**系統管理員身分執行**。 如果出現提示，請輸入電腦具有系統管理員權限帳戶的認證。 如果系統管理員層級的帳戶與您登入電腦的使用者帳戶，您不會收到認證提示。

3. Windows PowerShell 會開啟以系統管理員權限。

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>重新命名的 DHCP 伺服器，並設定靜態 IP 位址

如果您尚未這樣做，您可以使用下列 Windows PowerShell 命令來重新命名的 DHCP 伺服器，並設定伺服器的靜態 IP 位址。

**設定靜態 IP 位址**

要將靜態 IP 位址指派給 DHCP 伺服器上，並設定正確的 DNS 伺服器 IP 位址的 DHCP 伺服器的 TCP/IP 內容，您可以使用下列命令。 您也必須使用您要用來設定電腦的值，取代這個範例中的介面名稱和 IP 位址。

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

如需有關這些命令的詳細資訊，請參閱下列主題。

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**重新命名電腦**

您可以使用下列命令重新命名，然後重新啟動電腦。

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

如需有關這些命令的詳細資訊，請參閱下列主題。

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>將電腦加入網域\(選擇性\)

如果您要安裝 DHCP 伺服器在 Active Directory 網域環境中，您必須將電腦加入網域。 以系統管理員的權限開啟 Windows PowerShell，然後執行下列命令，取代 網域 NetBios 名稱後**CORP**適用於您環境的值。

    Add-Computer CORP

出現提示時，輸入網域使用者帳戶具有將電腦加入網域的權限的認證。 

    Restart-Computer

如需有關 Add-computer 命令的詳細資訊，請參閱下列主題。

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>安裝 DHCP

在電腦重新啟動之後，請以系統管理員的權限開啟 Windows PowerShell，並執行下列命令，以安裝 DHCP。

    Install-WindowsFeature DHCP -IncludeManagementTools

如需有關此命令的詳細資訊，請參閱下列主題。

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>建立 DHCP 安全性群組

若要建立的安全性群組，您必須執行的網路殼層\(netsh\)命令在 Windows PowerShell 中，並使新的群組則會變成作用中，然後重新啟動 DHCP 服務。

當您在 DHCP 伺服器上，執行下列的 netsh 命令**DHCP 系統管理員**並**DHCP Users**中建立安全性群組**本機使用者和群組**於 DHCP伺服器。

    netsh dhcp add securitygroups

下列命令會重新啟動本機電腦上的 DHCP 服務。

    Restart-service dhcpserver

如需有關這些命令的詳細資訊，請參閱下列主題。

- [網路殼層 (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>授權 DHCP 伺服器在 Active Directory 中的\(選擇性\)

如果您要在網域環境中安裝 DHCP，您必須執行下列步驟來授權 DHCP 伺服器在網域中運作。

>[!NOTE]
>未經授權的 DHCP 伺服器是安裝在 Active Directory 網域中無法正常運作，並不租用給 DHCP 用戶端的 IP 位址。 未經授權的 DHCP 伺服器自動停用是不正確的 IP 位址指派給您的網路上的用戶端時，防止未經授權的 DHCP 伺服器的安全性功能。

您可以使用下列命令，將 DHCP 伺服器新增到 Active Directory 中授權 DHCP 伺服器的清單。 

>[!NOTE]
>如果您沒有網域環境，不會執行此命令。

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

若要確認已在 Active Directory 中授權 DHCP 伺服器，您可以使用下列命令。

    Get-DhcpServerInDC

以下是在 Windows PowerShell 中顯示的範例結果。

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

如需有關這些命令的詳細資訊，請參閱下列主題。

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>通知伺服器管理員張貼\-已完成安裝 DHCP 設定\(選擇性\)

您已完成文章之後\-安裝的工作，例如建立安全性群組，以及授權 DHCP 伺服器在 Active Directory 中，伺服器管理員仍可能會指出該貼文的使用者介面中顯示警示\-必須使用 DHCP 安裝後的設定精靈 完成安裝步驟。

您現在可以防止\-藉由設定下列登錄機碼使用下列 Windows PowerShell 命令不會出現在 [伺服器管理員] 中的不必要的和不正確的訊息。

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

如需有關此命令的詳細資訊，請參閱下列主題。

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>設定伺服器層級 DNS 動態更新組態設定\(選擇性\)

如果您想為 DHCP 用戶端電腦執行 DNS 動態更新的 DHCP 伺服器，您可以執行下列命令來設定此設定。 這是伺服器層級設定，不的範圍層級設定，因此它會影響您在伺服器設定的所有範圍。 此範例命令也會設定用戶端至少到期時，刪除 DNS 資源記錄用戶端的 DHCP 伺服器。

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

您可以使用下列命令來設定 DHCP 伺服器用來登錄或取消註冊的 DNS 伺服器上的用戶端記錄的認證。 此範例會將認證儲存在 DHCP 伺服器上。 第一個命令會使用**Get-credential**來建立**PSCredential**物件，然後再將儲存在物件 **$Credential**變數。 此命令會提示您輸入使用者名稱和密碼，因此請確定您提供可更新資源記錄，DNS 伺服器上的權限的帳戶認證。

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

如需有關這些命令的詳細資訊，請參閱下列主題。

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>設定公司網路範圍

DHCP 安裝完成之後，您可以使用下列命令來設定和啟用公司領域、 建立排除範圍的範圍，及設定 DHCP 選項的預設閘道、 DNS 伺服器 IP 位址和 DNS 網域名稱。

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

如需有關這些命令的詳細資訊，請參閱下列主題。

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>設定 Corpnet2 領域\(選擇性\)

如果您有第二個子網路連線到路由器的第一個子網路啟用 DHCP 轉寄的位置，您可以使用下列命令來新增第二個範圍，此範例中名為 Corpnet2。 此範例也會設定排除範圍和預設閘道的 IP 位址\(子網路上的路由器 IP 位址\)Corpnet2 子網路。

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

如果您有其他子網路，由這個 DHCP 伺服器，您可以重複這些命令，為所有命令的參數，都使用不同的值，以新增為每個子網路的範圍。

>[!IMPORTANT]
>確定您的 DHCP 用戶端和 DHCP 伺服器之間的所有路由器已都設定為 DHCP 訊息轉送。 有關如何設定 DHCP 轉寄，請參閱您的路由器文件的資訊。

## <a name="bkmk_verify"></a>確認伺服器功能

若要確認 DHCP 伺服器提供給 DHCP 用戶端的動態配置的 IP 位址，您可以連接至服務的子網路的另一部電腦。 您連接到網路介面卡和電源的電腦上的乙太網路纜線之後，它就會從 DHCP 伺服器要求 IP 位址。 您可以使用，以確認成功設定組態**ipconfig /all**命令並檢查結果，或藉由執行連線能力測試，例如嘗試存取 Web 資源與您的瀏覽器或檔案共用，使用 Windows檔案總管 或其他應用程式。

如果用戶端不會收到來自 DHCP 伺服器的 IP 位址，請執行下列疑難排解步驟。

1. 請確定乙太網路纜線已插入同時在電腦和乙太網路交換器、 集線器或路由器。
1. 如果用戶端電腦插入路由器透過分開 DHCP 伺服器的網路區段上，確定路由器已設定成轉寄 DHCP 訊息。
1. 請確定 DHCP 伺服器權在 Active Directory 中，執行下列命令，以從 Active Directory 擷取授權的 DHCP 伺服器的清單。 [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. 請確定您的範圍，會啟用開啟 DHCP 主控台\(伺服器管理員 中，**工具**， **DHCP**\)，展開伺服器樹狀目錄中，若要檢閱的領域，然後以滑鼠右鍵\-按一下每個範圍。 如果產生的功能表包含選取項目**Activate**，按一下**Activate**。 \(如果已啟用的領域，功能表選取項目讀取**停用**。\) 

## <a name="bkmk_dhcpwps"></a>DHCP 的 Windows PowerShell 命令

下列參考提供命令說明和語法的所有 DHCP 伺服器的 Windows PowerShell 命令適用於 Windows Server 2016。 本主題列出以字母順序，例如根據命令，開頭的動詞命令的命令**取得**或是**設定**。

>[!NOTE]
>您可以使用 Windows Server 2012 R2 中的 Windows Server 2016 命令。

- [DhcpServer Module](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

下列參考提供命令說明和語法的所有 DHCP 伺服器的 Windows PowerShell 命令適用於 Windows Server 2012 R2。 本主題列出以字母順序，例如根據命令，開頭的動詞命令的命令**取得**或是**設定**。

>[!NOTE]
>Windows Server 2016 中，您可以使用 Windows Server 2012 R2 的命令。

- [Windows PowerShell 中的 DHCP 伺服器 Cmdlet](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>本指南中的 Windows PowerShell 命令清單

以下是簡單的命令和本指南中所使用的範例值清單。

    
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
    


