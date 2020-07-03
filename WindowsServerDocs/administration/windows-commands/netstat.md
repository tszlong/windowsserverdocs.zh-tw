---
title: netstat
description: Netstat 命令的參考文章，會顯示作用中的 TCP 連線、電腦正在接聽的埠、乙太網路統計資料、IP 路由表、IPv4 統計資料，以及 IPv6 統計資料。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c53ac83c1037d5f4998bb6efa43d66b418119df8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934799"
---
# <a name="netstat"></a>netstat

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示使用中的 TCP 連線、電腦正在接聽的埠、乙太網路統計資料、IP 路由表、IPv4 統計資料（適用于 IP、ICMP、TCP 和 UDP 通訊協定），以及 IPv6 統計資料（適用于 IPv6、ICMPv6、透過 IPv6 的 TCP 和 UDP 通訊協定）。 使用時不含參數，此命令會顯示作用中的 TCP 連線。

> [!IMPORTANT]
> 只有當網際網路通訊協定（TCP/IP）通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。

## <a name="syntax"></a>語法

```
netstat [-a] [-b] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<interval>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -a | 顯示所有作用中的 TCP 連線，以及電腦在其上接聽的 TCP 和 UDP 埠。 |
| -b | 顯示建立每個連接或接聽埠時所牽涉到的可執行檔。 在某些情況下，已知的可執行檔會裝載多個獨立元件，而在這些情況下，會顯示與建立連線或接聽埠相關的元件順序。 在此情況下，可執行檔名稱位於底部的 [] 中，頂端是它所呼叫的元件，依此類推，直到達到 TCP/IP 為止。 請注意，此選項可能非常耗時，除非您有足夠的許可權，否則將會失敗。
| -E | 顯示乙太網路統計資料，例如傳送和接收的位元組數目和封包數。 這個參數可以與 **-s**結合。 |
| -n | 顯示使用中的 TCP 連線，但位址和埠號碼會以數位表示，而且不會嘗試判斷名稱。 |
| -o | 顯示使用中的 TCP 連線，並包含每個連接的處理序識別碼（PID）。 您可以在 Windows 工作管理員的 [進程] 索引標籤上，根據 PID 來尋找應用程式。 這個參數可以與 **-a**、 **-n**和 **-p**結合。 |
| -p`<Protocol>` | 顯示*通訊協定*所指定之通訊協定的連接。 在此情況下，*通訊協定*可以是 tcp、udp、tcpv6 或 udpv6。 如果搭配 **-s**使用此參數來依通訊協定顯示統計資料，*通訊協定*可以是 tcp、udp、icmp、ip、tcpv6、udpv6、icmpv6 或 ipv6。 |
| -S | 依通訊協定顯示統計資料。 根據預設，會顯示 TCP、UDP、ICMP 和 IP 通訊協定的統計資料。 如果已安裝 IPv6 通訊協定，則會顯示 TCP over IPv6、UDP over IPv6、ICMPv6 和 IPv6 通訊協定的統計資料。 **-P**參數可以用來指定一組通訊協定。 |
| -r | 顯示 IP 路由表的內容。 這相當於 route print 命令。 |
| `<interval>` | 每隔*間隔*秒重新計算選取的資訊。 按 CTRL + C 停止重新顯示。 如果省略此參數，此命令只會列印選取的資訊一次。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Netstat**命令提供下列各項的統計資料：

    | 參數 | 說明 |
    | --------- | ----------- |
    | Proto | 通訊協定（TCP 或 UDP）的名稱。 |
    | 本機位址 | 本機電腦的 IP 位址，以及所使用的埠號碼。 除非指定 **-n**參數，否則會顯示對應至 IP 位址和埠名稱的本機電腦名稱稱。 如果尚未建立埠，埠號碼會顯示為星號（*）。 |
    | 外部地址 | 通訊端連接之遠端電腦的 IP 位址和埠號碼。 除非指定 **-n**參數，否則會顯示對應至 IP 位址和埠的名稱。 如果尚未建立埠，埠號碼會顯示為星號（*）。 |
    | State | 指出 TCP 連接的狀態，包括：<ul><li>CLOSE_WAIT</li><li>CLOSED</li><li>建成</li><li>FIN_WAIT_1</li><li>FIN_WAIT_2</li><li>LAST_ACK</li><li>接聽</li><li>SYN_RECEIVED</li><li>SYN_SEND</li><li>TIMED_WAIT</li></ul> |

### <a name="examples"></a>範例

若要顯示所有通訊協定的 Ethernet 統計資料和統計資料，請輸入：

```
netstat -e -s
```

若只要顯示 TCP 和 UDP 通訊協定的統計資料，請輸入：

```
netstat -s -p tcp udp
```

若要每隔5秒顯示作用中的 TCP 連線和處理序識別碼，請輸入：

```
netstat -o 5
```

若要使用數值形式顯示作用中的 TCP 連線和處理序識別碼，請輸入：

```
netstat -n -o
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
