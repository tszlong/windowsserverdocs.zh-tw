---
title: pathping
description: 瞭解如何使用 pathping 命令取得網路延遲和遺失資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 8be1897b241871bcb65126b39f201769f82f50bf
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436813"
---
# <a name="pathping"></a>pathping

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

提供來源與目的地之間中繼躍點的網路延遲和網路遺失的相關資訊。 **pathping**會在一段時間內，將多個 echo 要求訊息傳送至來源與目的地之間的每個路由器，然後根據每個路由器傳回的封包來計算結果。 因為**pathping**會在任何指定的路由器或連結上顯示封包遺失的程度，您可以判斷哪些路由器或子網可能有網路問題。

**pathping**會藉由識別路徑上的路由器，來執行**tracert**命令的對等項。 然後，它會在指定的時間週期內，定期將 ping 傳送至所有路由器，並根據每個傳回的數位來計算統計資料。 使用時不含參數， **pathping**會顯示說明。

## <a name="syntax"></a>語法
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/n|防止**pathping**嘗試將中繼路由器的 IP 位址解析為其名稱。 這可能會加快顯示**pathping**結果的速度。|
|/h \< MaximumHops>|指定路徑中用來搜尋目標（目的地）的躍點數目上限。 預設值為30個躍點。|
|/g \< Hostlist>|指定 echo 要求訊息使用 IP 標頭中的鬆散來源路由選項，並搭配*Hostlist*中指定的中繼目的地集合。 使用鬆散來源路由，連續的中繼目的地可以由一或多個路由器隔開。 主機清單中的位址或名稱數目上限為9。 *Hostlist*是一系列以空格分隔的 IP 位址（小數點十進位標記法）。|
|/p \< Period>|指定連續 ping 之間要等候的毫秒數。 預設值為250毫秒（1/4 秒）。|
|/q \< NumQueries>|指定傳送至路徑中每個路由器的 echo 要求訊息數目。 預設值為100查詢。|
|/w \< timeout>|指定等待每個回復的毫秒數。 預設值為3000毫秒（3秒）。|
|/i \< IPaddress>|指定來源地址。|
|/4 \< IPv4>|指定 pathping 僅使用 IPv4。|
|/6 \< IPv6>|指定 pathping 僅使用 IPv6。|
|\<TargetName>|指定以 IP 位址或主機名稱識別的目的地。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **pathping**參數會區分大小寫。
-   若要避免網路擁塞，應以夠慢的步調傳送 ping。
-   若要將高載遺失的影響降至最低，請不要頻繁地傳送 ping。
-   使用 **/p**參數時，會將 ping 個別傳送到每個中繼躍點。 因此，傳送至相同躍點的兩個 ping 之間的*間隔會乘以*躍點數目。
-   使用 **/w**參數時，可以平行傳送多個 ping。 因此， *timeout*參數中指定的時間量不會受*Period*參數中指定的時間長度限制，以等待 ping 之間的等候。
-   只有當網際網路通訊協定（TCP/IP）通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。

## <a name="examples"></a>範例

若要顯示**pathping**命令輸出：

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```
執行**pathping**時，第一個結果會列出路徑。 這是使用**tracert**命令所顯示的相同路徑。 接下來，會顯示大約90秒的忙碌訊息（時間因躍點計數而有所不同）。 在這段期間，會從先前列出的所有路由器，以及從它們之間的連結收集資訊。 在這段期間結束時，會顯示測試結果。

在上述的範例報表中，**此節點/連結**[**遺失/傳送 = 百分比**] 和 [**位址**] 資料行顯示172.16.87.218 和192.168.52.1 之間的連結正在卸載13% 的封包。 躍點2和4的路由器也會捨棄處理的封包，但這種遺失並不會影響其轉送未定址流量的能力。

針對連結顯示的遺失率（ **|** 在 [**位址**] 欄中識別為分隔號（））表示連結擁塞，這會導致在路徑上轉送的封包遺失。 針對路由器顯示的遺失率（由其 IP 位址識別）表示這些路由器可能超載。

## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)