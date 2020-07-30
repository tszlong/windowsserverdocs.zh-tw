---
title: DHCP （動態主機設定通訊協定）基本概念
description: ''
ms.prod: windows-server
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: troubleshoot
author: Deland-Han
ms.author: delhan
ms.reviewer: ''
ms.openlocfilehash: 5a3247fad961f4b2d1cf6e354c29706708c8e330
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409809"
---
# <a name="dhcp-dynamic-host-configuration-protocol-basics"></a>DHCP （動態主機設定通訊協定）基本概念

動態主機設定通訊協定（DHCP）是由 RFC 1541 所定義的標準通訊協定（由 RFC 2131 取代），可讓伺服器將 IP 位址和設定資訊動態散發給用戶端。 通常 DHCP 伺服器至少會提供此基本資訊給用戶端：

- IP 位址

- 子網路遮罩

- 也可以提供預設的 GatewayOther 資訊，例如功能變數名稱服務（DNS）伺服器位址和 Windows 網際網路名稱服務（WINS）伺服器位址。 系統管理員會使用向外剖析至用戶端的選項來設定 DHCP 伺服器。

## <a name="more-information"></a>相關資訊

下列 Microsoft 產品提供 DHCP 用戶端功能：

- Windows NT Server 版本3.5、3.51 和4。0

- Windows NT 工作站版本3.5、3.51 和4。0

- Windows 95

- 適用于 MS-DOS 的 Microsoft Network Client 3.0 版

- 適用于 MS-DOS 的 Microsoft LAN Manager Client 2.2 版 c

- 適用于工作組3.11、3.11 和 3.11 b 版本的 Microsoft TCP/IP-32 for Windows

不同的 DHCP 用戶端支援可以從 DHCP 伺服器接收的不同選項。

下列 Microsoft 伺服器作業系統提供 DHCP 伺服器功能：

- Windows NT Server 版本3。5

- Windows NT Server 版本3.51

- Windows NT Server 版本4。0

當用戶端在設定為接收 DHCP 資訊後第一次初始化時，它會起始與伺服器的交談。

以下是用戶端與伺服器之間交談的摘要資料表，後面接著進程的封包層級描述：

```
Source Dest Source Dest Packet
 MAC addr MAC addr IP addr IP addr Description
 -----------------------------------------------------------------
 Client Broadcast 0.0.0.0 255.255.255.255 DHCP Discover
 DHCPsrvr Broadcast DHCPsrvr 255.255.255.255 DHCP Offer
 Client Broadcast 0.0.0.0 255.255.255.255 DHCP Request
 DHCPsrvr Broadcast DHCPsrvr 255.255.255.255 DHCP ACK
```

DHCP 用戶端和 DHCP 伺服器之間的詳細交談如下所示：

### <a name="dhcpdiscover"></a>DHCPDISCOVER

用戶端會傳送 DHCPDISCOVER 封包。 以下是網路監視器捕捉的摘錄，其中顯示 DHCPDISCOVER 封包的 IP 和 DHCP 部分。 在 [IP] 區段中，您可以看到 [目的地位址] 是255.255.255.255，而 [來源位址] 是0.0.0.0。 DHCP 區段會將封包識別為探索封包，並使用網路卡的實體位址在兩個地方識別用戶端。 請注意，[CHADDR] 欄位和 [DHCP：用戶端識別碼] 欄位中的值完全相同。

```

IP: ID = 0x0; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 0 (0x0)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x39A6
 IP: Source Address = 0.0.0.0
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: Discover (xid=21274A1D)
 DHCP: Op Code (op) = 1 (0x1)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 556223005 (0x21274A1D)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 0.0.0.0
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP Discover
 DHCP: Client-identifier = (Type: 1) 08 00 2b 2e d8 5e
 DHCP: Host Name = JUMBO-WS
 DHCP: Parameter Request List = (Length: 7) 01 0f 03 2c 2e 2f 06
 DHCP: End of this option field

```

### <a name="dhcpoffer"></a>DHCPOFFER

DHCP 伺服器會藉由傳送 DHCPOFFER 封包來回應。 在下面的 capture 摘錄的 IP 區段中，來源位址現在是 DHCP 伺服器 IP 位址，而目的地位址是廣播位址255.255.255.255。 DHCP 區段會將封包識別為供應專案。 [YIADDR] 欄位會填入伺服器提供給用戶端的 IP 位址。 請注意，CHADDR 欄位仍然包含要求用戶端的實體位址。 此外，我們也會在 DHCP 選項欄位區段中看到伺服器傳送的各種選項，以及 IP 位址。 在此情況下，伺服器會傳送子網路遮罩、預設閘道（路由器）、租用時間、WINS 伺服器位址（NetBIOS 名稱服務）和 NetBIOS 節點類型。

```

IP: ID = 0x3C30; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 15408 (0x3C30)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x2FA8
 IP: Source Address = 157.54.48.151
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: Offer (xid=21274A1D)
 DHCP: Op Code (op) = 2 (0x2)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 556223005 (0x21274A1D)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 157.54.50.5
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP Offer
 DHCP: Subnet Mask = 255.255.240.0
 DHCP: Renewal Time Value (T1) = 8 Days, 0:00:00
 DHCP: Rebinding Time Value (T2) = 14 Days, 0:00:00
 DHCP: IP Address Lease Time = 16 Days, 0:00:00
 DHCP: Server Identifier = 157.54.48.151
 DHCP: Router = 157.54.48.1
 DHCP: NetBIOS Name Service = 157.54.16.154
 DHCP: NetBIOS Node Type = (Length: 1) 04
 DHCP: End of this option field

```

### <a name="dhcprequest"></a>DHCPREQUEST

用戶端會藉由傳送 DHCPREQUEST 來回應 DHCPOFFER。 在下面的 capture 的 IP 區段中，用戶端的來源位址仍然是0.0.0.0，封包的目的地仍然是255.255.255.255。 用戶端會保留0.0.0.0，因為用戶端未收到伺服器的驗證，因此可以使用提供的位址開始。 目的地仍在廣播中，因為一部以上的 DHCP 伺服器可能已回應，而且可能會保留對用戶端的供應專案。 這可讓其他 DHCP 伺服器知道它們可以釋放其提供的位址，並將其傳回給其可用的集區。 DHCP 區段會將封包識別為要求，並使用 DHCP：要求的位址欄位來驗證提供的位址。 [DHCP：伺服器識別碼] 欄位會顯示提供租用之 DHCP 伺服器的 IP 位址。

```

IP: ID = 0x100; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 256 (0x100)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x38A6
 IP: Source Address = 0.0.0.0
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: Request (xid=21274A1D)
 DHCP: Op Code (op) = 1 (0x1)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 556223005 (0x21274A1D)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 0.0.0.0
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP Request
 DHCP: Client-identifier = (Type: 1) 08 00 2b 2e d8 5e
 DHCP: Requested Address = 157.54.50.5
 DHCP: Server Identifier = 157.54.48.151
 DHCP: Host Name = JUMBO-WS
 DHCP: Parameter Request List = (Length: 7) 01 0f 03 2c 2e 2f 06
 DHCP: End of this option field

```

### <a name="dhcpack"></a>DHCPACK

DHCP 伺服器會以 DHCPACK 回應 DHCPREQUEST，因此會完成初始化週期。 來源位址是 DHCP 伺服器 IP 位址，而目的地位址仍然是255.255.255.255。 YIADDR 欄位包含用戶端的位址，而 [CHADDR] 和 [DHCP：用戶端識別碼] 欄位是要求用戶端中網路卡的實體位址。 DHCP 選項區段會將封包識別為 ACK。

```

IP: ID = 0x3D30; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 15664 (0x3D30)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x2EA8
 IP: Source Address = 157.54.48.151
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: ACK (xid=21274A1D)
 DHCP: Op Code (op) = 2 (0x2)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 556223005 (0x21274A1D)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 157.54.50.5
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP ACK
 DHCP: Renewal Time Value (T1) = 8 Days, 0:00:00
 DHCP: Rebinding Time Value (T2) = 14 Days, 0:00:00
 DHCP: IP Address Lease Time = 16 Days, 0:00:00
 DHCP: Server Identifier = 157.54.48.151
 DHCP: Subnet Mask = 255.255.240.0
 DHCP: Router = 157.54.48.1
 DHCP: NetBIOS Name Service = 157.54.16.154
 DHCP: NetBIOS Node Type = (Length: 1) 04
 DHCP: End of this option field

```

如果用戶端先前已有 DHCP 指派的 IP 位址，且已重新開機，用戶端會特別在特殊 DHCPREQUEST 封包中要求先前租用的 IP 位址。 來源位址為0.0.0.0，而目的地為廣播位址255.255.255.255。 Microsoft 用戶端會在 DHCP 選項欄位 DHCP：要求的位址中填入先前指派的位址。 嚴格符合 RFC 規範的用戶端會將所要求的位址填入 CIADDR 欄位。 Microsoft DHCP 伺服器將接受其中一項。

```

IP: ID = 0x0; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 0 (0x0)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x39A6
 IP: Source Address = 0.0.0.0
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: Request (xid=2757554E)
 DHCP: Op Code (op) = 1 (0x1)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 660034894 (0x2757554E)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 0.0.0.0
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP Request
 DHCP: Client-identifier = (Type: 1) 08 00 2b 2e d8 5e
 DHCP: Requested Address = 157.54.50.5
 DHCP: Host Name = JUMBO-WS
 DHCP: Parameter Request List = (Length: 7) 01 0f 03 2c 2e 2f 06
 DHCP: End of this option field

```

此時，伺服器可能會或可能不會回應。 Windows NT DHCP 伺服器的行為取決於所使用的作業系統版本，以及其他因素，例如 superscoping。 如果伺服器判斷用戶端仍然可以使用該位址，它會保持無訊息或通知 DHCPREQUEST。 如果伺服器判斷用戶端不能擁有該位址，則會傳送 NACK。

```

IP: ID = 0x3F1A; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 16154 (0x3F1A)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x2CBE
 IP: Source Address = 157.54.48.151
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: NACK (xid=74A005CE)
 DHCP: Op Code (op) = 2 (0x2)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 1956644302 (0x74A005CE)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 0.0.0.0
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP NACK
 DHCP: Server Identifier = 157.54.48.151
 DHCP: End of this option field

```

接著，用戶端會開始探索程式，但 DHCPDISCOVER 封包仍然會嘗試租用相同的位址。 在許多情況下，tth 用戶端會取得相同的位址，但可能不會。

```

IP: ID = 0x100; Proto = UDP; Len: 328
 IP: Version = 4 (0x4)
 IP: Header Length = 20 (0x14)
 IP: Service Type = 0 (0x0)
 IP: Precedence = Routine
 IP: ...0.... = Normal Delay
 IP: ....0... = Normal Throughput
 IP: .....0.. = Normal Reliability
 IP: Total Length = 328 (0x148)
 IP: Identification = 256 (0x100)
 IP: Flags Summary = 0 (0x0)
 IP: .......0 = Last fragment in datagram
 IP: ......0. = May fragment datagram if necessary
 IP: Fragment Offset = 0 (0x0) bytes
 IP: Time to Live = 128 (0x80)
 IP: Protocol = UDP - User Datagram
 IP: Checksum = 0x38A6
 IP: Source Address = 0.0.0.0
 IP: Destination Address = 255.255.255.255
 IP: Data: Number of data bytes remaining = 308 (0x0134)

DHCP: Discover (xid=3ED14752)
 DHCP: Op Code (op) = 1 (0x1)
 DHCP: Hardware Type (htype) = 1 (0x1) 10Mb Ethernet
 DHCP: Hardware Address Length (hlen) = 6 (0x6)
 DHCP: Hops (hops) = 0 (0x0)
 DHCP: Transaction ID (xid) = 1053902674 (0x3ED14752)
 DHCP: Seconds (secs) = 0 (0x0)
 DHCP: Flags (flags) = 0 (0x0)
 DHCP: 0............... = No Broadcast
 DHCP: Client IP Address (ciaddr) = 0.0.0.0
 DHCP: Your IP Address (yiaddr) = 0.0.0.0
 DHCP: Server IP Address (siaddr) = 0.0.0.0
 DHCP: Relay IP Address (giaddr) = 0.0.0.0
 DHCP: Client Ethernet Address (chaddr) = 08002B2ED85E
 DHCP: Server Host Name (sname) = <Blank>
 DHCP: Boot File Name (file) = <Blank>
 DHCP: Magic Cookie = [OK]
 DHCP: Option Field (options)
 DHCP: DHCP Message Type = DHCP Discover
 DHCP: Client-identifier = (Type: 1) 08 00 2b 2e d8 5e
 DHCP: Requested Address = 157.54.51.5
 DHCP: Host Name = JUMBO-WS
 DHCP: Parameter Request List = (Length: 7) 01 0f 03 2c 2e 2f 06
 DHCP: End of this option field

```

用戶端從 DHCP 伺服器取得的 DHCP 資訊將會有相關聯的租用時間。 租用時間定義用戶端可以使用 DHCP 指派資訊的時間長度。 當租用到達特定里程碑時，用戶端會嘗試更新其 DHCP 資訊。

若要在 Windows 或 Windows 上查看工作組用戶端的 IP 資訊，請使用 IPCONFIG 公用程式。 如果用戶端是 Windows 95，請使用 WINIPCFG。

## <a name="references"></a>參考

如需有關 DHCP 的詳細資訊，請參閱 RFC1541 和 RFC2131。 Rfc 可以透過網際網路在多個網站上取得，例如： [http://www.rfc-editor.org/](http://www.rfc-editor.org/) 和[http://www.tech-nic.qc.ca/](http://www.tech-nic.qc.ca/)
