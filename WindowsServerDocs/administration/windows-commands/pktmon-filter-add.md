---
title: pktmon 篩選準則新增
description: Pktmon filter add 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: e1e69bbad5433eedd582b1d6218e462b4c70f71d
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241060"
---
# <a name="pktmon-filter-add"></a>pktmon 篩選準則新增

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

Pktmon 篩選準則新增可讓您新增篩選器來控制要報告的封包。 針對要報告的封包，它必須符合至少一個篩選準則中指定的所有條件。 一次最多可以有八個篩選準則可供使用。

## <a name="syntax"></a>Syntax

```
pktmon filter add <name> [-m mac [mac2]] [-v vlan] [-d { IPv4 | IPv6 | number }]
                         [-t { TCP [flags...] | UDP | ICMP | ICMPv6 | number }]
                         [-i ip [ip2]] [-p port [port2]] [-e [port]]
```

您可以提供篩選的選擇性名稱或描述。

  > [!NOTE]
  > 當兩個 Mac (-m) 、Ip (-i) 或埠 (-p) 指定時，篩選準則會比對包含兩者的封包。 它不會區分來源或目的地的目的。

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-m、--mac [-address]** | 符合來源或目的地 MAC 位址。 請參閱上述注意事項。 |
| **-v、--vlan** | 比對 802.1 Q 標頭中的 VLAN ID (VID) 。 |
| **-d、--data-link [-protocol]、--ethertype** |  (第2層) 通訊協定的資料連結相符。 可以是 IPv4、IPv6、ARP 或通訊協定編號。 |
| **-t，--transport [-protocol]，--ip-通訊協定** | 依傳輸 (第4層) 通訊協定比對。 可以是 TCP、UDP、ICMP、ICMPv6 或通訊協定編號。 若要進一步篩選 TCP 封包，可以提供選擇性的 TCP 旗標清單來進行比對。 支援的旗標為 FIN、SYN、RST、PSH、ACK、URG、ECE 和 CWR。 |
| **-i，--ip [-address]** | 符合來源或目的地 IP 位址。 請參閱上述注意事項。 若要依子網進行比對，請使用 CIDR 標記法搭配前置長度。 |
| **-p、--port** | 符合來源或目的地埠號碼。 請參閱上述注意事項。 |
| **-e，--encap** | 除了外部封包之外，此篩選也適用于封裝的內部封包。 支援的封裝方法有 VXLAN、GRE、NVGRE 和 IP IP。 自訂 VXLAN 埠是選擇性的，預設值為4789。 |

## <a name="examples"></a>範例

下列一組篩選器會從 IP 位址10.0.0.10 捕捉任何 ICMP 流量，以及埠53上的任何流量。

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t icmp
C:\Test> pktmon filter add -p 53
```

下列篩選器會捕獲 IP 位址10.0.0.10 所傳送或接收的所有 SYN 封包：

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t tcp syn
```

下列篩選器會使用 ICMP 通訊協定來呼叫 **MyPing** ping 10.10.10.10：

```PowerShell
C:\Test> pktmon filter add MyPing -i 10.10.10.10 -t ICMP
```

下列稱為 **MySmbSyb** 的篩選器會捕捉 TCP 同步處理的 SMB 流量：

```PowerShell
C:\Test> pktmon filter add MySmbSyn -i 10.10.10.10 -t TCP SYN -p 445
```

下列稱為 **>mysubnet** 的篩選器會以子網路遮罩255.255.255.0 或/24 （CIDR 標記法）來捕捉流量：

```PowerShell
C:\Test> pktmon filter add MySubnet -i 10.10.10.0/24
```

## <a name="other-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
