---
title: 任意傳播 DNS 總覽
description: 本主題提供任意傳播 DNS 的簡短總覽
manager: laurawi
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: greglin
author: greg-lindsay
ms.openlocfilehash: 2a891d2e74ec00c923808f7dde347bfd17a2b5b5
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078575"
---
# <a name="anycast-dns-overview"></a>任意傳播 DNS 總覽

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019

本主題提供有關多點傳送 DNS 運作方式的資訊。

## <a name="what-is-anycast"></a>什麼是播？

「任意傳播」是一種技術，可針對每個指派相同 IP 位址的端點群組提供多個路由路徑。 群組中的每個裝置會在網路上通告相同的位址，並使用路由通訊協定來選擇最適合的目的地。

多點傳播可讓您將多個節點放在相同的 IP 位址後面，並使用相等的多重路徑 (ECMP) 路由，將這些節點之間的流量導向，以調整無狀態服務（例如 DNS 或 HTTP）。 多點傳播與單播不同，其中每個端點都有自己的個別 IP 位址。

## <a name="why-use-anycast-with-dns"></a>為什麼要使用 DNS 的任意傳播？

有了任意傳播 DNS，您就可以啟用 DNS 伺服器或一組伺服器，以根據 DNS 用戶端的地理位置回應 DNS 查詢。 這可以增強 DNS 回應時間，並簡化 DNS 用戶端設定。 任意傳播 DNS 也提供額外的冗余層級，可協助防止 DNS 阻絕服務攻擊。

### <a name="how-anycast-dns-works"></a>任意傳播 DNS 的運作方式

任意傳播 DNS 的運作方式是使用路由通訊協定，例如邊界閘道協定 (BGP) 將 DNS 查詢傳送至慣用的 DNS 伺服器或 DNS 伺服器群組 (例如：負載平衡器所管理的一組 DNS 伺服器) 。 這可以藉由從最接近用戶端的 DNS 伺服器取得 DNS 回應，來將 DNS 通訊優化。

有了任意傳播，存在於多個地理位置的伺服器，每個都會將單一的相同 IP 位址公告到其本機閘道 (路由器) 。 當 DNS 用戶端起始對任意傳播位址的查詢時，就會評估可用的路由，並將 DNS 查詢傳送至慣用的位置。 一般而言，這是根據網路拓撲的最接近位置。 請參閱下列範例。

![任一傳播 DNS](../../media/Anycast/anycast.png)

**圖 1**：位於網路不同網站上的四部 DNS 伺服器，每個都宣佈相同的任意傳播 IP 位址， (黑色箭號) 至網路。 DNS 用戶端裝置會將要求送出至任意傳播 IP 位址。 網路裝置會分析可用的路由，並將用戶端的 DNS 查詢傳送到最接近的位置， (藍色箭號) 。

現在會使用任意傳播 DNS 來路由傳送許多全域 DNS 服務的 DNS 流量。 例如，跟 DNS 伺服器系統的高度相依于任意傳播 DNS。 多點傳播也適用于各種不同的路由通訊協定，並可專門在內部網路上使用。

## <a name="windows-server-native-bgp-anycast-demo"></a>Windows Server 原生 BGP 任意傳播示範

下列程式示範如何使用 Windows Server 上的原生 BGP 來搭配任意傳播 DNS。

### <a name="requirements"></a>需求

- 已安裝 Hyper-v 角色的一部實體裝置。
  - Windows Server 2012 R2、Windows 10 或更新版本。
- 2個用戶端 Vm (任何作業系統) 。
  - 建議您安裝 DNS 的系結工具，例如進行深入探討。
- 3部伺服器 Vm (Windows Server 2016 或 Windows Server 2019) 。
  - 如果 Windows PowerShell LoopbackAdapter 模組尚未安裝在伺服器 Vm 上 (DC001、DC002) ，則會暫時需要存取網際網路，才能安裝此模組。

### <a name="hyper-v-setup"></a>Hyper-v 安裝程式

設定您的 Hyper-v 伺服器，如下所示：

- 已設定2個私人虛擬交換器網路
  - Mock 網際網路網路 131.253.1.0/24
  - Mock 內部網路網路 10.10.10.0/24
- 2個用戶端 Vm 連接至 131.253.1.0/24 網路
- 2部伺服器 Vm 連接至 10.10.10.0/24 網路
- 1伺服器是雙重主伺服器，並同時連接至 131.253.1.0/24 和 10.10.10.0/24 網路。

### <a name="virtual-machine-network-configuration"></a>虛擬機器網路設定

使用下列設定來設定虛擬機器上的網路設定：

1.  Client1、client2
  - Client1：131.253.1。1
  - Client2：131.253.1。2
  - 子網路遮罩：255.255.255.0
  - DNS：51.51.51.51
  - 閘道：131.253.1.254
2.   (Windows Server) 的閘道
  - NIC1：131.253.1.254，子網255.255.255。0
  - NIC2：10.10.10.254，子網255.255.255。0
  - DNS：51.51.51.51
  - 閘道： 131.253.1.100 (可以忽略示範) 
3.  DC001 (Windows Server) 
  - NIC1：10.10.10。1
  - 子網：255.255.255。0
  - DNS：10.10.10。1
4.  DC002 (Windows Server) 
  - NIC1：10.10.10。2
  - 子網255.255.255。0
  - DNS：10.10.10。2

### <a name="configure-dns"></a>設定 DNS

使用伺服器管理員和 DNS 管理主控台或 Windows PowerShell 安裝下列伺服器角色，並在兩部伺服器上建立靜態 DNS 區域。

1.  DC001、DC002
  - 安裝 Active Directory Domain Services 並升級至網域控制站 (選擇性) 
  -  (需要安裝 DNS 角色) 
  - 在 DC001 和 DC002 上建立靜態區域 (非 AD 整合式) 命名為**tst**
    - 在 "TXT" 類型的區域中新增單一靜態記錄名稱**伺服器**
    - DC001 = **DC001**上 TXT 記錄的資料 (文字) 
    - DC002 = **DC002**上 TXT 記錄的資料 (文字) 

### <a name="configure-loopback-adapters"></a>設定回送介面卡

在 DC001 和 DC002 上提高許可權的 Windows PowerShell 提示字元中，輸入下列命令以設定回送介面卡。

> [!NOTE]
> **Install 模組**命令需要網際網路存取。 這可以藉由暫時將 VM 指派給 Hyper-v 中的外部網路來完成。

```PowerShell
$primary_interface = (Get-NetAdapter |?{$_.Status -eq "Up" -and !$_.Virtual}).Name
$loopback_ipv4 = '51.51.51.51'
$loopback_ipv4_length = '32'
$loopback_name = 'Loopback'
Install-Module -Name LoopbackAdapter -MinimumVersion 1.2.0.0 -Force
Import-Module -Name LoopbackAdapter
New-LoopbackAdapter -Name $loopback_name -Force
$interface_loopback = Get-NetAdapter -Name $loopback_name
$interface_main = Get-NetAdapter -Name $primary_interface
Set-NetIPInterface -InterfaceIndex $interface_loopback.ifIndex -InterfaceMetric "254" -WeakHostReceive Enabled -WeakHostSend Enabled -DHCP Disabled
Set-NetIPInterface -InterfaceIndex $interface_main.ifIndex -WeakHostReceive Enabled -WeakHostSend Enabled
Set-NetIPAddress -InterfaceIndex $interface_loopback.ifIndex -SkipAsSource $True
Get-NetAdapter $loopback_name | Set-DNSClient –RegisterThisConnectionsAddress $False
New-NetIPAddress -InterfaceAlias $loopback_name -IPAddress $loopback_ipv4 -PrefixLength $loopback_ipv4_length -AddressFamily ipv4
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_msclient
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_pacer
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_server
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_lltdio
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_rspndr
```

### <a name="virtual-machine-routing-configuration"></a>虛擬機器路由設定

在 Vm 上使用下列 Windows PowerShell 命令來設定路由。

1.  閘道
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.254” -LocalASN 8075
```

2.  DC001
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.1” -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.1 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

3.  DC002
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier "10.10.10.2" -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.2 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

### <a name="summary-diagram"></a>摘要圖表

![任一傳播 DNS](../../media/Anycast/anycast-lab.png)

**圖 2**：原生 BGP 任意傳播 DNS 示範的實驗室設定

## <a name="anycast-dns-demonstration"></a>任意傳播 DNS 示範


1.  確認閘道伺服器上的 BGP 路由

    PS C： \> Get-BgpRouteInformation

    DestinationNetwork NextHop LearnedFromPeer State LocalPref MED-V<br>
    ------------------ -------    --------------- ----- --------- ---<br>
    51.51.51.0/24 10.10.10.1 DC001 最佳<br>
    51.51.51.0/24 10.10.10.2 DC002 最佳<br>

2.  在 client1 和 client2 上，確認您可以連接到51.51.51.51

    PS C： \> ping 51.51.51.51

    使用32個位元組的資料來 ping 51.51.51.51：<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126

    51.51.51.51 的 Ping 統計資料：<br>
    封包： Sent = 4，Received = 4，遺失 = 0 (0% 遺失) ，<br>
    Milli> 秒的大約來回行程時間：<br>
    最小值 = 0 毫秒、最大值 = 0 毫秒、平均 = 0 毫秒

3.  在 client1 和 client2 上，使用 nslookup 或 [深入瞭解] 查詢 TXT 記錄。 兩者的範例如下所示。

    PS C： \> 深入瞭解 TST TXT + short<br>
    PS C： \> nslookup-type = txt server tst 51.51.51.51

    一個用戶端會顯示「DC001」，而另一個用戶端會顯示「DC002」，以確認任意傳播是否正常運作。  您也可以從閘道伺服器進行查詢。

4.  接下來，停用 DC001 上的乙太網路介面卡。

    PS C： \> (get-netadapter) 。名字<br>
    回送<br>
    Ethernet 2<br>
    PS C： \> 停用-get-netadapter "Ethernet 2"<br>
    確認<br>
    確定要執行此動作嗎？<br>
    停用-Get-netadapter 「Ethernet 2」<br>
    [Y] 是 [A] 全部都是 [N] 否 [L] 否全部 [S] 暫停 [？]Help (預設值為 "Y" ) ：<br>
    PS C： \> (get-netadapter) 。地位<br>
    Up<br>
    已停用

5.  確認先前從 DC001 接收回應的 DNS 用戶端已切換至 DC002。

    PS C： \> nslookup-type = txt server tst 51.51.51.51<br>
    伺服器：未知<br>
    位址：51.51.51.51<br>

    tst text =

    "DC001"<br>
    PS C： \> nslookup-type = txt server tst 51.51.51.51<br>
    伺服器：未知<br>
    位址：51.51.51.51<br>

    tst text =

    DC002

6.  使用閘道伺服器上的 BgpStatistics，確認 DC001 上的 BGP 會話已關閉。
7.  再次啟用 DC001 上的乙太網路介面卡，並確認已還原 BGP 會話，且用戶端會再次從 DC001 接收 DNS 回應。

> [!NOTE]
> 如果未使用負載平衡器，則個別的用戶端會使用相同的後端 DNS 伺服器（如果有的話）。 這會為用戶端建立一致的 BGP 路徑。 如需詳細資訊，請參閱 RFC4786： [相等成本路徑](https://tools.ietf.org/html/rfc4786#page-10)的4.4.3 一節。

## <a name="frequently-asked-questions"></a>常見問題集

問：任意傳播 DNS 是在內部部署 DNS 環境中使用的良好解決方案嗎？<br>
答：任意傳播 DNS 可與內部部署 DNS 服務緊密搭配運作。 不過，DNS 服務並不 *需要* 進行任何傳播，以進行調整。

問：在具有大量 (例如： >50) 的網域控制站的環境中，執行任意傳播 DNS 有何影響？ <br>
答：不會直接影響功能。 如果使用負載平衡器，則不需要在網域控制站上進行任何額外的設定。

問： Microsoft 客戶服務是否支援任意傳播 DNS 設定？<br>
答：如果您使用非 Microsoft 負載平衡器轉送 DNS 查詢，Microsoft 將會支援與 DNS 伺服器服務相關的問題。 請參閱負載平衡器廠商，以瞭解與 DNS 轉送相關的問題。

問：以大量 (例如： >50) 的網域控制站而言，任意傳播 DNS 的最佳作法為何？<br>
答：最佳作法是在每個地理位置使用負載平衡器。 負載平衡器通常是由外部廠商所提供。

問：是否有任何可傳播 DNS 和 Azure DNS 有類似的功能？<br>
答： Azure DNS 使用任意傳播。 若要使用與 Azure DNS 的任意傳播，請將負載平衡器設定為將要求轉寄到 Azure DNS 伺服器。