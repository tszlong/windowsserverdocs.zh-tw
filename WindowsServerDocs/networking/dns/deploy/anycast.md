---
title: 任意傳播 DNS 總覽
description: 本主題提供任意傳播 DNS 的簡要總覽
manager: laurawi
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: greglin
author: greg-lindsay
ms.openlocfilehash: 2f91ace398cf236967fadde21db7ea0957640995
ms.sourcegitcommit: c4f30b1617571fe434c7fe054695d163e73506b8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88048890"
---
# <a name="anycast-dns-overview"></a>任意傳播 DNS 總覽

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019

本主題提供多點傳播 DNS 運作方式的相關資訊。

## <a name="what-is-anycast"></a>什麼是任意傳播？

「任意傳播」是一種技術，可提供多個路由路徑給每個端點群組，分別指派相同的 IP 位址。 群組中的每個裝置都會在網路上通告相同的位址，而路由通訊協定則是用來選擇哪一個是最佳的目的地。

「可傳播」可讓您藉由將多個節點放在相同的 IP 位址後方，並使用相同成本的多重路徑，來調整無狀態服務（例如 DNS 或 HTTP） (ECMP) 路由，以在這些節點之間直接傳輸流量。 任意傳播與單播不同，每個端點都有自己的個別 IP 位址。 

## <a name="why-use-anycast-with-dns"></a>為什麼要使用 DNS 的任意傳播？

使用任意傳播 DNS，您可以讓 DNS 伺服器或一組伺服器，根據 DNS 用戶端的地理位置回應 DNS 查詢。 這可以增強 DNS 回應時間，並簡化 DNS 用戶端設定。 任一傳播 DNS 也提供額外一層的冗余，有助於防範 DNS 阻絕服務攻擊。 

### <a name="how-anycast-dns-works"></a>多點傳播 DNS 的運作方式

任一傳播 DNS 的運作方式是使用路由通訊協定，例如邊界閘道協定 (BGP) 將 DNS 查詢傳送至慣用的 DNS 伺服器或 DNS 伺服器群組 (例如：負載平衡器所管理的 DNS 伺服器群組) 。 這可以藉由從最接近用戶端的 DNS 伺服器取得 DNS 回應，來優化 DNS 通訊。

使用任意傳播時，存在於多個地理位置的伺服器，每個都會將單一且相同的 IP 位址公告到其本機閘道 (路由器) 。 當 DNS 用戶端起始對任一傳播位址的查詢時，就會評估可用的路由，並將 DNS 查詢傳送至慣用的位置。 一般來說，這是以網路拓撲為基礎的最接近位置。 請參閱下列範例。

![任意傳播 DNS](../../media/Anycast/anycast.png)

**圖 1**：位於網路上不同網站的四部 DNS 伺服器，各自宣告相同的任意傳播 IP 位址 (黑色箭號) 到網路。 DNS 用戶端裝置會將要求送出到任意傳播 IP 位址。 網路裝置會分析可用的路由，並將用戶端的 DNS 查詢傳送到最接近的位置 (藍色箭號) 。 

目前常用來路由傳送許多全域 DNS 服務之 DNS 流量的 DNS。 例如，根 DNS 伺服器系統會大量依賴任意傳播 DNS。 「任意播」也適用于各種路由通訊協定，而且可以專門用於內部網路。

## <a name="windows-server-native-bgp-anycast-demo"></a>Windows Server 原生 BGP 的任意傳播示範

下列程式示範 Windows Server 上的原生 BGP 如何搭配任意傳播 DNS 來使用。  

### <a name="requirements"></a>需求

- 已安裝 Hyper-v 角色的一部實體裝置。
  - Windows Server 2012 R2、Windows 10 或更新版本。
- 2 (任何作業系統) 的用戶端 Vm。
  - 建議您安裝 DNS 的 BIND 工具，例如進行發掘。
- 3部伺服器 Vm (Windows Server 2016 或 Windows Server 2019) 。
  - 如果伺服器 Vm 上尚未安裝 Windows PowerShell LoopbackAdapter 模組 (DC001、DC002) ，則會暫時需要網際網路存取以安裝此模組。

### <a name="hyper-v-setup"></a>Hyper-v 設定

設定 Hyper-v 伺服器，如下所示：

- 2已設定私人虛擬交換器網路
  - Mock 網際網路網路 131.253.1.0/24
  - Mock 近端內部網路 10.10.10.0/24
- 2用戶端 Vm 已連接到 131.253.1.0/24 網路
- 2個伺服器 Vm 已連接到 10.10.10.0/24 網路
- 1部伺服器是雙重主目錄，並同時連接到 131.253.1.0/24 和 10.10.10.0/24 網路。

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
  - 閘道：可以忽略示範的 131.253.1.100 () 
3.  DC001 (Windows Server) 
  - NIC1：10.10.10。1
  - 子網：255.255.255。0
  - DNS：10.10.10。1
4.  DC002 (Windows Server) 
  - NIC1：10.10.10。2
  - 子網255.255.255。0
  - DNS：10.10.10。2

### <a name="configure-dns"></a>設定 DNS

使用伺服器管理員和 DNS 管理主控台或 Windows PowerShell 來安裝下列伺服器角色，並在兩部伺服器上建立靜態 DNS 區域。

1.  DC001、DC002
  - 安裝 Active Directory Domain Services 並升級至網域控制站 (選擇性) 
  -  (需要安裝 DNS 角色) 
  - 建立靜態區域 (DC001 和 DC002 上名為**tst**的非 AD 整合式) 
    - 在類型為 "TXT" 的區域中新增單一靜態記錄名稱**伺服器**
    - DC001 = **DC001**上 TXT 記錄的資料 (文字) 
    - DC002 = **DC002**上 TXT 記錄的資料 (文字) 

### <a name="configure-loopback-adapters"></a>設定回送介面卡

在 DC001 和 DC002 上提高許可權的 Windows PowerShell 提示字元中輸入下列命令，以設定回送介面卡。 

> [!NOTE]
> **Install-Module**命令需要網際網路存取。 這可以藉由暫時將 VM 指派給 Hyper-v 中的外部網路來完成。

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

![任意傳播 DNS](../../media/Anycast/anycast-lab.png)

**圖 2**：原生 BGP 的的實驗室設定 DNS 示範

## <a name="anycast-dns-demonstration"></a>任意傳播 DNS 示範


1.  確認閘道伺服器上的 BGP 路由

    PS C： \> Get-BgpRouteInformation

    DestinationNetwork NextHop LearnedFromPeer State LocalPref MED-V<br>
    ------------------ -------    --------------- ----- --------- ---<br>
    51.51.51.0/24 10.10.10.1 DC001 最佳<br>
    51.51.51.0/24 10.10.10.2 DC002 最佳<br>

2.  在 client1 和 client2 上，確認您可以連線到51.51.51.51

    PS C： \> ping 51.51.51.51

    使用32位元組的資料 ping 51.51.51.51：<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126<br>
    從51.51.51.51 回復：位元組 = 32 時間<1 毫秒 TTL = 126

    51.51.51.51 的 Ping 統計資料：<br>
    封包：已傳送 = 4，Received = 4，遺失 = 0 (0% 遺失) ，<br>
    Milli> 秒內的大約來回行程時間：<br>
    最小值 = 0 毫秒，最大值 = 0 毫秒，平均值 = 0 毫秒

3.  在 client1 和 client2 上，使用 nslookup 或 [深入探索] 來查詢 TXT 記錄。 兩者的範例如下所示。

    PS C： \> 發掘伺服器. TST TXT + short<br>
    PS C： \> nslookup-type = txt server. zone. tst 51.51.51.51

    其中一個用戶端會顯示「DC001」，另一個用戶端將會顯示「DC002」，確認任意傳播是否正常運作。  您也可以從閘道伺服器進行查詢。

4.  接下來，停用 DC001 上的乙太網路介面卡。

    PS C： \> (get-netadapter) 。檔案名<br>
    回送<br>
    Ethernet 2<br>
    PS C： \> Disable-get-netadapter "Ethernet 2"<br>
    確認<br>
    您確定要執行此動作嗎？<br>
    停用-Get-netadapter ' Ethernet 2 '<br>
    [Y] 是 [A] 全部都是 [N] 否 [L] 否全部 [S] 暫停 [？][說明 (] 預設為 "Y" ) ：<br>
    PS C： \> (get-netadapter) 。狀態<br>
    Up<br>
    停用

5.  確認先前從 DC001 接收回應的 DNS 用戶端已切換至 DC002。

    PS C： \> nslookup-type = txt server. zone. tst 51.51.51.51<br>
    伺服器：不明<br>
    位址：51.51.51.51<br>

    tst text =

    "DC001"<br>
    PS C： \> nslookup-type = txt server. zone. tst 51.51.51.51<br>
    伺服器：不明<br>
    位址：51.51.51.51<br>

    tst text =

    DC002

6.  使用閘道伺服器上的 Get-bgpstatistics，確認已在 DC001 上關閉 BGP 會話。
7.  再次在 DC001 上啟用 Ethernet 介面卡，並確認已還原 BGP 會話，而且用戶端再次收到來自 DC001 的 DNS 回應。

> [!NOTE]
> 如果未使用負載平衡器，則個別的用戶端將會使用相同的後端 DNS 伺服器（如果有的話）。 這會為用戶端建立一致的 BGP 路徑。 如需詳細資訊，請參閱 4.4.3 of RFC4786：[均等-成本路徑](https://tools.ietf.org/html/rfc4786#page-10)。

## <a name="frequently-asked-questions"></a>常見問題集

問：任意傳播 DNS 是否為在內部部署 DNS 環境中使用的絕佳解決方案？<br>
答：任意內容的 DNS 與內部部署 DNS 服務緊密搭配運作。 不過，DNS 服務無法進行擴充，因此不*需要*任何傳播。
 
問：在具有大量 (例如： >50) 網域控制站的環境中，執行任意傳播 DNS 的影響為何？ <br>
答：對功能沒有直接的影響。 如果使用負載平衡器，則不需要在網域控制站上進行其他設定。
 
問： Microsoft 客戶服務支援的任意傳播 DNS 設定嗎？<br>
答：如果您使用非 Microsoft 的負載平衡器來轉送 DNS 查詢，Microsoft 將會支援與 DNS 伺服器服務相關的問題。 請參閱負載平衡器廠商，以取得與 DNS 轉送相關的問題。 
 
問：具有大量 (（例如： >50) 網域控制站的多點傳播 DNS 的最佳做法為何？<br>
答：最佳作法是在每個地理位置使用負載平衡器。 負載平衡器通常是由外部廠商所提供。 

問：任何傳播 DNS 和 Azure DNS 有類似的功能嗎？<br>
答： Azure DNS 使用任意傳播。 若要使用 Azure DNS 的任意傳播，請將負載平衡器設定為將要求轉送至 Azure DNS 伺服器。 