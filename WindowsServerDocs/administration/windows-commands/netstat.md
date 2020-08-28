---
title: netstat
description: Netstat 命令的參考文章，會顯示作用中的 TCP 連接、電腦正在接聽的埠、乙太網路統計資料、IP 路由表、IPv4 統計資料，以及 IPv6 統計資料。
ms.topic: reference
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d68ec2e21c4248769973b3409896ba9d5bd15e5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038799"
---
# <a name="netstat"></a>netstat

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示作用中 TCP 連線、電腦正在接聽的埠、乙太網路統計資料、IP 路由表、IP、ICMP、TCP 和 UDP 通訊協定) 的 IPv4 統計資料 (，以及 ipv6、ICMPv6、透過 IPv6 的 TCP 和 UDP 通訊協定) ipv6 統計資料 (的 IPv6 統計資料。 使用時不含參數，此命令會顯示作用中的 TCP 連接。

> [!IMPORTANT]
> 只有在 [網路連線] 的網路介面卡內容中，將 [ (TCP/IP) 通訊協定安裝為元件時，才能使用此命令。

## <a name="syntax"></a>語法

```
netstat [-a] [-b] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<interval>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -a | 顯示電腦正在接聽的所有作用中 TCP 連接和 TCP 和 UDP 埠。 |
| -b | 顯示建立每個連接或接聽埠時所牽涉到的可執行檔。 在某些情況下，已知可執行檔裝載多個獨立的元件，在這些情況下，會顯示建立連接或接聽埠所涉及的元件序列。 在此情況下，可執行檔名稱是在底部的 [] 中，在頂端是它所呼叫的元件，依此類推，直到達到 TCP/IP 為止。 請注意，此選項可能會很耗時，除非您有足夠的許可權，否則會失敗。
| -E | 顯示乙太網路統計資料，例如傳送和接收的位元組數和封包數。 此參數可以與 **-s**結合。 |
| -n | 顯示作用中的 TCP 連接，但是位址和埠號碼會以數值表示，而不會嘗試決定名稱。 |
| -o | 顯示作用中的 TCP 連接，並包含每個連接的處理序識別碼 (PID) 。 您可以根據 Windows 工作管理員中 [進程] 索引標籤上的 PID 來尋找應用程式。 這個參數可以結合 **-a**、 **-n**和 **-p**。 |
| -p `<Protocol>` | 顯示 *通訊*協定所指定的通訊協定連接。 在此情況下， *通訊協定* 可以是 tcp、udp、tcpv6 或 udpv6。 如果此參數搭配 **-s** 使用，以依通訊協定顯示統計資料，則 *通訊協定* 可以是 tcp、udp、icmp、ip、tcpv6、udpv6、icmpv6 或 ipv6。 |
| -S | 依通訊協定顯示統計資料。 根據預設，會顯示 TCP、UDP、ICMP 和 IP 通訊協定的統計資料。 如果已安裝 IPv6 通訊協定，則會顯示 TCP over IPv6 的統計資料、透過 IPv6 的 UDP、ICMPv6 和 IPv6 通訊協定。 **-P**參數可以用來指定一組通訊協定。 |
| -r | 顯示 IP 路由表的內容。 這相當於 route print 命令。 |
| `<interval>` | 每隔 *間隔* 秒重新選取所選的資訊。 按 CTRL + C 以停止重新顯示。 如果省略此參數，此命令只會列印選取的資訊一次。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Netstat**命令提供下列各項的統計資料：

    | 參數 | 描述 |
    | --------- | ----------- |
    | 原 |  (TCP 或 UDP) 的通訊協定名稱。 |
    | 本機位址 | 本機電腦的 IP 位址和使用的埠號碼。 除非指定 **-n** 參數，否則會顯示對應至 IP 位址和埠名稱的本機電腦名稱稱。 如果尚未建立埠，埠號碼會顯示為星號 ( * ) 。 |
    | 外部地址 | 通訊端所連接之遠端電腦的 IP 位址和埠號碼。 除非指定 **-n** 參數，否則會顯示對應至 IP 位址和埠的名稱。 如果尚未建立埠，埠號碼會顯示為星號 ( * ) 。 |
    | State | 指出 TCP 連接的狀態，包括：<ul><li>CLOSE_WAIT</li><li>CLOSED</li><li>建立</li><li>FIN_WAIT_1</li><li>FIN_WAIT_2</li><li>LAST_ACK</li><li>聽</li><li>SYN_RECEIVED</li><li>SYN_SEND</li><li>TIMED_WAIT</li></ul> |

### <a name="examples"></a>範例

若要顯示所有通訊協定的乙太網路統計資料和統計資料，請輸入：

```
netstat -e -s
```

若只要顯示 TCP 和 UDP 通訊協定的統計資料，請輸入：

```
netstat -s -p tcp udp
```

若要每隔5秒顯示作用中的 TCP 連接和處理序識別碼，請輸入：

```
netstat -o 5
```

若要顯示使用中的 TCP 連接和使用數值格式的處理序識別碼，請輸入：

```
netstat -n -o
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
