---
title: tracert
description: Tracert 的參考主題，其會藉由將網際網路控制訊息通訊協定（ICMP） echo 要求或 ICMPv6 訊息傳送至目的地，並以累加方式增加存留時間（TTL）域值，來決定目的地所採用的路徑。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b62558b1a2959fd441c246159c14daf0a1e9ca5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437243"
---
# <a name="tracert"></a>tracert

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由將網際網路控制訊息通訊協定（ICMP） echo 要求或 ICMPv6 訊息傳送至目的地，並以累加方式增加存留時間（TTL）域值，來決定目的地所採用的路徑。 顯示的路徑是來源主機與目的地之間路徑中路由器的近/端路由器介面清單。 近端介面是最接近路徑中傳送主機的路由器介面。 使用時不含參數，tracert 會顯示說明。

## <a name="syntax"></a>語法
```
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/d|防止**tracert**嘗試將中繼路由器的 IP 位址解析為其名稱。 這可以加速顯示**tracert**結果。|
|/h \< MaximumHops>|指定路徑中用來搜尋目標（目的地）的躍點數目上限。 預設值為30個躍點。|
|/j \< Hostlist>|指定 echo 要求訊息使用 IP 標頭中的鬆散來源路由選項，並搭配*Hostlist*中指定的中繼目的地集合。 使用鬆散來源路由，連續的中繼目的地可以由一或多個路由器隔開。 主機清單中的位址或名稱數目上限為9。 *Hostlist*是一系列以空格分隔的 IP 位址（小數點十進位標記法）。 只有在追蹤 IPv4 位址時，才使用此參數。|
|/w \< timeout>|指定所需的時間（以毫秒為單位），等待超過 ICMP 時間，或回應對應至所要接收之指定 echo 要求訊息的回復訊息。 如果未在超時時間內收到，則會顯示星號（*）。 預設的超時時間為4000（4秒）。|
|/R|指定 IPv6 路由延伸模組標頭用來傳送 echo 要求訊息到本機主機，使用目的地作為中繼目的地並測試反向路由。|
|/S \< Srcaddr>|指定要在 echo 要求訊息中使用的來源位址。 只有在追蹤 IPv6 位址時，才使用此參數。|
|/4|指定 tracert 只能針對此追蹤使用 IPv4。|
|/6|指定 tracert 只能針對此追蹤使用 IPv6。|
|\<TargetName>|指定以 IP 位址或主機名稱識別的目的地。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   此診斷工具會將具有不同存留時間（TTL）值的 ICMP echo 要求訊息傳送至目的地，以決定目的地所採用的路徑。 在轉送 IP 封包之前，路徑中的每個路由器都必須至少1個，才能將其遞減。 實際上，TTL 是最大連結計數器。 當封包上的 TTL 到達0時，路由器預期會將超過 ICMP 時間的訊息傳回給來源電腦。 tracert 會藉由傳送第一個具有 TTL 1 的 echo 要求訊息來判斷路徑，並在每次後續傳輸時將 TTL 遞增1，直到目標回應或達到躍點數的最大數目為止。 躍點的最大數目預設為30，而且可以使用 **/h**參數來指定。 路徑的判斷方式是檢查 ICMP 時間超過中繼路由器傳回的訊息，以及目的地所傳回的 echo 回復訊息。 不過，某些路由器不會針對具有過期 TTL 值的封包傳回超過的時間，而且會 invisile 到 tracert 命令。 在此情況下，會顯示該躍點的一列星號（*）。
-   若要追蹤路徑，並針對路徑中的每個路由器和連結提供網路延遲和封包遺失，請使用**pathping**命令。
-   只有當網際網路通訊協定（TCP/IP）通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。

## <a name="examples"></a>範例
若要追蹤名為 corp7.microsoft.com 之主機的路徑，請輸入：
```
tracert corp7.microsoft.com
```
若要追蹤名為 corp7.microsoft.com 之主機的路徑，並防止每個 IP 位址的名稱解析為它的名稱，請輸入：
```
tracert /d corp7.microsoft.com
```
若要追蹤名為 corp7.microsoft.com 之主機的路徑，並使用鬆散來源路由 10.12.0.1/10.29.3.1/10.1.44.1，請輸入：
```
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com
```
## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)
