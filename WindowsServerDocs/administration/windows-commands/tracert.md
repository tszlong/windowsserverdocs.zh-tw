---
title: tracert
description: Tracert 的參考文章，藉由將網際網路控制訊息通訊協定 (ICMP) echo 要求或 ICMPv6 訊息傳送至目的地，並以累加的時間 (TTL) 域值的方式，來判斷目的地所採用的路徑。
ms.topic: reference
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 767871e9100d8b14c8a893fd9d9dbb5862810ace
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156419"
---
# <a name="tracert"></a>tracert

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這項診斷工具會藉由將網際網路控制訊息通訊協定 (ICMP) echo 要求或 ICMPv6 訊息傳送至目的地，以累加的時間 (TTL) 域值的方式，來判斷目的地所採用的路徑。 路徑上的每個路由器都必須在轉送 IP 封包中的 TTL 之前，至少先將它的 TTL 遞減1。 實際上，TTL 是最大連結計數器。 當封包的 TTL 到達0時，路由器應該會將 ICMP 時間超過的訊息傳回來源電腦。

此命令會藉由傳送第一個回應要求訊息（TTL 為1），並在每個後續傳輸上將 TTL 遞增1，直到目標回應或達到躍點數上限為止，藉此決定路徑。 躍點的最大躍點數預設為30，而且可以使用 **/h** 參數來指定。

路徑的判斷方式是檢查 ICMP 時間超過中繼路由器所傳回的訊息，以及目的地所傳回的回應回復訊息。 不過，某些路由器不會針對 TTL 值過期的封包傳回超過時間的訊息，而 **tracert** 命令看不到這些訊息。 在此情況下， `*` 會針對該躍點顯示一列星號 () 。 顯示的路徑是來源主機與目的地之間路徑中路由器的近/側路由器介面清單。 Near/側介面是最接近路徑中傳送主機的路由器介面。

> [!IMPORTANT]
> 只有在 [網路連線] 的網路介面卡內容中，將 [ (TCP/IP) 通訊協定安裝為元件時，才能使用此命令。
>
> 若要追蹤路徑並提供路徑中每個路由器和連結的網路延遲和封包遺失，請使用 [pathping 命令](pathping.md) 命令。

## <a name="syntax"></a>語法

```
tracert [/d] [/h <maximumhops>] [/j <hostlist>] [/w <timeout>] [/R] [/S <srcaddr>] [/4][/6] <targetname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /d | 停止嘗試將中繼路由器的 IP 位址解析為其名稱。 這可加速傳回結果。 |
| /h `<maximumhops>` | 指定路徑中用來搜尋目標 (目的地) 的躍點數目上限。 預設值為30個躍點。 |
| /j `<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的鬆散來源路由選項，以及中指定的中繼目的地集合 `<hostlist>` 。 使用鬆散來源路由時，連續的中繼目的地可以用一或多個路由器來分隔。 清單中的位址或名稱數目上限為 *9*。 `<hostlist>`是一系列以點分隔的十進位標記法 (的 IP 位址，) 以空格分隔。 只有在追蹤 IPv4 位址時，才使用此參數。 |
| /w `<timeout>` | 以毫秒為單位，指定要等候超過 ICMP 時間的時間量，或回應與要接收的指定回應要求訊息對應的回應回復訊息。 如果在超時時間內未收到， `*` 則會顯示星號 () 。 預設的超時時間是 4000 (4 秒) 。 |
| /R | 指定使用 IPv6 路由延伸模組標頭，將 echo 要求訊息傳送至本機主機，使用目的地作為中繼目的地並測試反向路由。 |
| /S `<srcaddr>` | 指定要在 echo 要求訊息中使用的來源位址。 只有在追蹤 IPv6 位址時，才使用此參數。 |
| /4 | 指定 tracert.exe 只能針對此追蹤使用 IPv4。 |
| /6 | 指定 tracert.exe 只能針對此追蹤使用 IPv6。 |
| `<targetname>` | 指定以 IP 位址或主機名稱識別的目的地。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要追蹤名為 *corp7.microsoft.com*之主機的路徑，請輸入：

```
tracert corp7.microsoft.com
```

若要追蹤名為 *corp7.microsoft.com* 之主機的路徑，並防止將每個 IP 位址解析為其名稱，請輸入：

```
tracert /d corp7.microsoft.com
```

若要追蹤名為 *corp7.microsoft.com* 之主機的路徑，並使用鬆散來源路由 *10.12.0.1/10.29.3.1/10.1.44.1*，請輸入：

```
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [pathping 命令](pathping.md)
