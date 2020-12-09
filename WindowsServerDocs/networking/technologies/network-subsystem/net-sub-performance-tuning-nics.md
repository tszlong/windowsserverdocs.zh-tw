---
title: 效能調整網路介面卡
description: 本主題是 Windows Server 2016 的網路子系統效能微調指南的一部分。
audience: Admin - CI ID 111485 - CSSTroubleshoot
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 12/23/2019
ms.openlocfilehash: 4b3c32a6382e7538ccfb1fab22b4d2be9495b3b0
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866047"
---
# <a name="performance-tuning-network-adapters"></a>效能調整網路介面卡

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

使用本主題中的資訊，針對執行 Windows Server 2016 和更新版本的電腦調整效能網路介面卡。 如果您的網路介面卡提供微調選項，您可以使用這些選項來優化網路輸送量和資源使用量。

您網路介面卡的正確調整設定取決於下列變數：

- 網路介面卡及其功能集
- 伺服器執行的工作負載類型
- 伺服器硬體和軟體資源
- 您的伺服器效能目標

以下各節說明一些效能調整選項。

##  <a name="enabling-offload-features"></a><a name="bkmk_offload"></a> 啟用卸載功能

調整網路介面卡卸載功能通常會有好處。 不過，網路介面卡的功能可能不足以處理高輸送量的卸載功能。

> [!IMPORTANT]
> 請勿使用卸載功能 **IPsec** 工作卸載或 **TCP 煙囪** 卸載。 這些技術在 Windows Server 2016 中已被取代，而且可能會對伺服器和網路效能造成負面影響。 此外，Microsoft 未來可能不支援這些技術。

例如，假設有一個具有有限硬體資源的網路介面卡。
在此情況下，啟用分割卸載功能可能會降低介面卡的最大持續性輸送量。 但是，如果可以接受減少的輸送量，您應該先啟用分割卸載功能。

> [!NOTE]
> 有些網路介面卡會要求您針對傳送和接收路徑個別啟用卸載功能。

##  <a name="enabling-receive-side-scaling-rss-for-web-servers"></a><a name="bkmk_rss_web"></a> 啟用 web 伺服器的接收端調整 (RSS) 

當伺服器上的網路介面卡比邏輯處理器少時，RSS 可以改善 Web 延展性和效能。 當所有 web 流量都通過支援 RSS 的網路介面卡時，伺服器可以跨不同的 Cpu 同時處理來自不同連接的傳入 web 要求。

> [!IMPORTANT]
> 避免在同一部伺服器上同時使用非 RSS 網路介面卡和支援 RSS 的網路介面卡。 因為 RSS 和超文字傳輸通訊協定中的負載分配邏輯 (HTTP) ，所以如果不支援 RSS 的網路介面卡在具有一或多個支援 RSS 的網路介面卡的伺服器上接受網路流量，效能可能會嚴重降低。 在這種情況下，您應該使用支援 RSS 的網路介面卡，或是在網路介面卡內容 [進階內容] 索引標籤上停用 RSS。
>
> 若要判斷網路介面卡是否支援 RSS，您可以檢視網路介面卡內容 [進階內容] 索引標籤上的 RSS 資訊。

### <a name="rss-profiles-and-rss-queues"></a>RSS 設定檔和 RSS 佇列

預設 RSS 預先定義的設定檔是 **NUMAStatic**，與舊版 Windows 所使用的預設值不同。 開始使用 RSS 設定檔之前，請先檢查可用的設定檔，以瞭解它們的優點，以及它們如何套用至您的網路環境和硬體。

例如，如果您開啟工作管理員並檢查伺服器上的邏輯處理器，而且看起來似乎使用量過低而無法接收流量，您可以嘗試將 RSS 佇列的數目從預設的2增加到網路介面卡支援的最大值。 您的網路介面卡可能會在驅動程式中提供變更 RSS 佇列數目的選項。

##  <a name="increasing-network-adapter-resources"></a><a name="bkmk_resources"></a> 增加網路介面卡資源

針對可讓您手動設定資源（例如接收和傳送緩衝區）的網路介面卡，您應該增加已配置的資源。

某些網路介面卡會將其接收緩衝區設低以節省來自主機的配置記憶體。 低的值會導致丟棄封包和降低效能。 因此，對於大量接收的情況，建議您將接收緩衝區的值增加到最大。

> [!NOTE]
> 如果網路介面卡未公開手動資源設定，則可能會動態地設定資源，或將資源設定為無法變更的固定值。

### <a name="enabling-interrupt-moderation"></a>啟用中斷仲裁

為了控制中斷仲裁，某些網路介面卡會公開不同的中斷仲裁層級，不同的緩衝區聯合參數 (有時個別用於傳送和接收緩衝區) 或兩者。

您應考慮針對 CPU 系結工作負載進行中斷仲裁。 使用「中斷仲裁」時，請考慮主機 CPU 節省和延遲之間的取捨，以及增加的主機 CPU 節省率，因為有更多的中斷和較低的延遲。 如果網路介面卡不會執行中斷仲裁，但是它會公開緩衝區聯合，您可以藉由增加合併的緩衝區數目來提升效能，以允許每個傳送或接收的緩衝區更多。

##  <a name="performance-tuning-for-low-latency-packet-processing"></a><a name="bkmk_low"></a> 低延遲封包處理的效能微調

許多網路介面卡都有提供將作業系統引起的延遲最佳化的選項。 延遲係指網路驅動程式處理連入封包到網路驅動程式將封包傳送回去之間的經歷時間。 此時間通常是以微秒為單位。 相較之下，用於長距離封包傳輸的傳輸時間則通常是以毫秒 (量級較大) 為單位。 這項調整不會縮短封包在傳輸上所花的時間。

以下是一些適用於精細度到微秒之網路的效能調整建議。

- 將定電腦 BIOS 設定成 [高效能]，且停用 C-State。 不過，請注意，這與系統和 BIOS 相依，某些系統在讓作業系統控制電源管理的情況下可以提供較高的效能。 您可以從 **設定** 或使用 **powercfg** 命令檢查及調整電源管理設定。 如需詳細資訊，請參閱 [Powercfg Command-Line 選項](/windows-hardware/design/device-experiences/powercfg-command-line-options)。

- 將作業系統電源管理設定檔設定為 [高效能系統]。
   > [!NOTE]
   > 如果系統 BIOS 已設定為停用電源管理的作業系統控制，此設定將無法正常運作。

- 啟用靜態卸載。 例如，啟用 UDP 總和檢查碼、TCP 總和檢查碼，然後將大型卸載 (LSO) 設定。

- 如果流量有多重資料流程處理，例如接收大量多播流量時，請啟用 RSS。

- 針對要求最低可能延遲的網路卡驅動程式，請停用 [插斷仲裁] 設定。 請記住，此設定可能會使用更多的 CPU 時間，它代表取捨。

- 在與處理封包的程式 (使用者執行緒) 所使用之核心共用 CPU 快取的核心處理器上，處理網路介面卡插斷和 DPC。 CPU 親和性調整可用來將處理程序導向特定邏輯處理器並搭配 RSS 設定來完成這項作業。 將相同的核心用於插斷、DPC 及使用者模式執行緒會讓效能隨著負載增加而變差，因為 ISR、DPC 及執行緒會爭相使用該核心。

##  <a name="system-management-interrupts"></a><a name="bkmk_smi"></a> 系統管理中斷

許多硬體系統都會針對各種維護功能使用系統管理中斷 (SMI-S) ，例如報告錯誤修正程式碼 (ECC) 記憶體錯誤、維持舊版 USB 相容性、控制風扇，以及管理 BIOS 控制的電源設定。

SMI-S 是系統上的最高優先順序的中斷，會將 CPU 置於管理模式。 當 SMI-S 執行插斷服務常式（通常包含在 BIOS 中）時，此模式會 shutdown 所有其他活動。

可惜的是，這種行為可能會導致延遲尖峰或超過100微秒。

如果您需要達到最低延遲，您應該向硬體提供者要求一個可將 SMI 盡可能降到最低限度的 BIOS 版本。 這些 BIOS 版本通常稱為「低延遲 BIOS」或「SMI-S 免費 BIOS」。 在某些情況下，硬體平台不可能將 SMI 活動全部消除，因為會使用它來控制必要的功能 (例如冷卻風扇)。

> [!NOTE]
> 作業系統無法控制 SMIs，因為邏輯處理器正在以特殊維護模式執行，以防止作業系統介入。

##  <a name="performance-tuning-tcp"></a><a name="bkmk_tcp"></a> 效能調整 TCP

 您可以使用下列專案來調整 TCP 效能。

###  <a name="tcp-receive-window-autotuning"></a><a name="bkmk_tcp_params"></a> TCP 接收視窗自動優化

在 Windows Vista、Windows Server 2008 和更新版本的 Windows 中，Windows 網路堆疊會使用名為「 *tcp 接收視窗* 自動調整」層級的功能來協調 tcp 接收視窗大小。 這項功能可以在 TCP 交握期間，為每個 TCP 通訊協調已定義的接收視窗大小。

在舊版的 Windows 中，Windows 網路堆疊使用固定大小的接收視窗 (65535 個位元組) ，而這些位元組會限制連接的整體可能輸送量。 TCP 連線的可達成總輸送量可能會限制網路使用方式。 TCP 接收視窗自動優化可讓這些案例完全使用網路。

若為具有特定大小的 TCP 接收視窗，您可以使用下列方程式來計算單一連接的總輸送量。

> 可達成 *的總輸送量（以位元組為單位）*  = *TCP 接收視窗大小（位元組）* \* (1/*連接延遲（秒）*) 

例如，對於延遲為10毫秒的連接，可達成的總輸送量只是 51 Mbps。 此值對於大型公司網路基礎結構而言是合理的。 不過，藉由使用自動調整來調整接收視窗，連線可以達到 1 Gbps 連接的完整行速率。

某些應用程式會定義 TCP 接收視窗的大小。 如果應用程式未定義接收視窗大小，連結速度會決定大小，如下所示：

- 每秒小於 1 mb (Mbps) ： 8 kb (KB) 
- 1 mbps 至 100 Mbps： 17 KB
- 每秒 100 Mbps 至 10 gb (Gbps) ： 64 KB
- 10 Gbps 或更快： 128 KB

例如，在已安裝 1 Gbps 網路介面卡的電腦上，視窗大小應為 64 KB。

這項功能也可充分利用其他功能來改善網路效能。 這些功能包括 [RFC 1323](https://tools.ietf.org/html/rfc1323)中所定義的其他 TCP 選項。 藉由使用這些功能，Windows 電腦可以協調較小但以定義值調整的 TCP 接收視窗大小，視設定而定。 此行為可讓您更容易處理網路裝置的大小。

> [!NOTE]
> 您可能會遇到網路裝置與 [ **TCP 視窗調整] 選項** 不相容的問題，如 [RFC 1323](https://tools.ietf.org/html/rfc1323) 中所定義，因此不支援縮放比例。 在這種情況下，請參閱此 [KB 934430，當您嘗試在防火牆裝置後方使用 Windows Vista 時，網路連線會失敗，](https://support.microsoft.com/help/934430/network-connectivity-fails-when-you-try-to-use-windows-vista-behind-a) 或洽詢網路裝置廠商的支援小組。

#### <a name="review-and-configure-tcp-receive-window-autotuning-level"></a>檢查和設定 TCP 接收視窗自動調諧層級

您可以使用 netsh 命令或 Windows PowerShell Cmdlet 來檢查或修改 TCP 接收視窗自動優化層級。

> [!NOTE]
> 不同于 Windows 10 或 Windows Server 2019 之前的 Windows 版本，您無法再使用登錄來設定 TCP 接收視窗大小。 如需已淘汰設定的詳細資訊，請參閱已 [淘汰的 TCP 參數](#deprecated-tcp-parameters)。

> [!NOTE]
> 如需可用自動優化層級的詳細資訊，請參閱自動 [優化層級](#autotuning-levels)。

**使用 netsh 來檢查或修改自動調諧層級**

若要檢查目前的設定，請開啟命令提示字元視窗，然後執行下列命令：

```cmd
netsh interface tcp show global
```

此命令的輸出應如下所示：

```
Querying active state...

TCP Global Parameters
-----
Receive-Side Scaling State : enabled
Chimney Offload State : disabled
Receive Window Auto-Tuning Level : normal
Add-On Congestion Control Provider : default
ECN Capability : disabled
RFC 1323 Timestamps : disabled
Initial RTO : 3000
Receive Segment Coalescing State : enabled
Non Sack Rtt Resiliency : disabled
Max SYN Retransmissions : 2
Fast Open : enabled
Fast Open Fallback : enabled
Pacing Profile : off
```

若要修改設定，請在命令提示字元中執行下列命令：

```cmd
netsh interface tcp set global autotuninglevel=<Value>
```

> [!NOTE]
> 在上述命令中， \<*Value*> 表示自動調整層級的新值。

如需此命令的詳細資訊，請參閱 [介面傳輸控制通訊協定的 Netsh 命令](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731258(v=ws.10))。

**使用 Powershell 來檢查或修改自動調諧層級**

若要檢查目前的設定，請開啟 PowerShell 視窗並執行下列 Cmdlet。

```PowerShell
Get-NetTCPSetting | Select SettingName,AutoTuningLevelLocal
```

此 Cmdlet 的輸出應如下所示。

```
SettingName           AutoTuningLevelLocal
-----------          --------------------
Automatic
InternetCustom       Normal
DatacenterCustom     Normal
Compat               Normal
Datacenter           Normal
Internet             Normal
```

若要修改設定，請在 PowerShell 命令提示字元中執行下列 Cmdlet。

```PowerShell
Set-NetTCPSetting -AutoTuningLevelLocal <Value>
```

> [!NOTE]
> 在上述命令中， \<*Value*> 表示自動調整層級的新值。

如需這些 Cmdlet 的詳細資訊，請參閱下列文章：

- [NetTCPSetting](/powershell/module/nettcpip/get-nettcpsetting)
- [設定-NetTCPSetting](/powershell/module/nettcpip/set-nettcpsetting)

#### <a name="autotuning-levels"></a>自動調諧層級

您可以將接收視窗自動優化設定為五個層級的任一個。 預設層級為 [ **正常**]。 下表描述這些層級。

|層級 |十六進位值 |註解 |
| --- | --- | --- |
|標準 (預設) |0x8 (比例因數 8)  |將 TCP 接收視窗設定為 [成長]，以容納幾乎所有的案例。 |
|已停用 |沒有任何可用的擴展因數 |設定 TCP 接收視窗的預設值。 |
|受限制 |0x4 (4) 的比例因數 |設定 TCP 接收視窗，使其成長超過預設值，但在某些情況下限制這類成長。 |
|高度限制 |0x2 (比例因數 2)  |設定 TCP 接收視窗，使其成長超過預設值，但請謹慎進行。 |
|實驗 |0xE (擴展因數 14)  |設定要成長以容納極端案例的 TCP 接收視窗。 |

如果您使用應用程式來捕獲網路封包，應用程式應該會針對不同的視窗自動優化層級設定報告類似下列內容的資料。

- 自動優化層級： **一般 (預設狀態)**

   ```
   Frame: Number = 492, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2667, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60975, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=4075590425, Ack=0, Win=64240 ( Negotiating scale factor 0x8 ) = 64240
   SrcPort: 60975
   DstPort: Microsoft-DS(445)
   SequenceNumber: 4075590425 (0xF2EC9319)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x8 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x8 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 8 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- 自動調諧層級： **已停用**

   ```
   Frame: Number = 353, Captured Frame Length = 62, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2576, Total IP Length = 48
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60956, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2315885330, Ack=0, Win=64240 ( ) = 64240
   SrcPort: 60956
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2315885330 (0x8A099B12)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 112 (0x70)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( ) = 64240 ----------------------------------------> TCP Receive Window set as 64K as per NIC Link bitrate. Note there is no Scale Factor defined. In this case, Scale factor is not being sent as a TCP Option, so it will not be used by Windows.
   Checksum: 0x817E, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- 自動優化層級： **限制**

   ```
   Frame: Number = 3, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2319, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60890, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1966088568, Ack=0, Win=64240 ( Negotiating scale factor 0x4 ) = 64240
   SrcPort: 60890
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1966088568 (0x75302178)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x4 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x4 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 4 -------------------------------> Scale factor, defined by AutoTuningLevel.
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- 自動調諧層級： **高度受限**

   ```
   Frame: Number = 115, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2388, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60903, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1463725706, Ack=0, Win=64240 ( Negotiating scale factor 0x2 ) = 64240
   SrcPort: 60903
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1463725706 (0x573EAE8A)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x2 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x2 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 2 ------------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- 自動優化層級：**實驗** 性

   ```
   Frame: Number = 238, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2490, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60933, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2095111365, Ack=0, Win=64240 ( Negotiating scale factor 0xe ) = 64240
   SrcPort: 60933
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2095111365 (0x7CE0DCC5)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0xe ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0xe Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 14 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

#### <a name="deprecated-tcp-parameters"></a>已淘汰的 TCP 參數

下列來自 Windows Server 2003 的登錄設定已不再受支援，並會在較新的版本中予以忽略。

- **TcpWindowSize**
- **NumTcbTablePartitions**
- **MaxHashTableSize**

所有這些設定都位於下列登錄子機碼中：

> **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters**

###  <a name="windows-filtering-platform"></a><a name="bkmk_wfp"></a> Windows 篩選平台

Windows Vista 和 Windows Server 2008 引進了 (WFP) 的 Windows 篩選平台。 WFP 將 Api 提供給非 Microsoft 獨立軟體廠商 (Isv) 來建立封包處理篩選。 範例包括防火牆和防毒軟體。

> [!NOTE]
> 寫入不當的 WFP 篩選可能會大幅降低伺服器的網路效能。 如需詳細資訊，請參閱 Windows 開發人員中心中的 [將 Packet-Processing 驅動程式和應用程式移植到 WFP](/windows-hardware/drivers/network/porting-packet-processing-drivers-and-apps-to-wfp) 。

如需本指南中所有主題的連結，請參閱 [網路子系統效能微調](net-sub-performance-top.md)。
