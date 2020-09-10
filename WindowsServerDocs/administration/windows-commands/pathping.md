---
title: pathping
description: Pathping 命令的參考文章，可取得來源與目的地之間中繼躍點的網路延遲和網路遺失的相關資訊。
ms.topic: reference
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: ed63467c8eba2d65a6408e08762357275dfef7f8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628635"
---
# <a name="pathping"></a>pathping

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

提供來源和目的地之間中繼躍點的網路延遲和網路遺失的相關資訊。 此命令會在一段時間內，將多個 echo 要求訊息傳送至來源和目的地之間的每個路由器，然後根據每個路由器傳回的封包來計算結果。 因為此命令會顯示任何指定路由器或連結的封包遺失程度，所以您可以判斷哪些路由器或子網可能發生網路問題。 使用時不含參數，此命令會顯示說明。

> [!NOTE]
> 只有在 [網路連線] 的網路介面卡內容中，將 [ (TCP/IP) 通訊協定安裝為元件時，才能使用此命令。
>
> 此外，此命令會識別路徑上有哪些路由器，與使用 [tracert 命令](tracert.md)相同。 Howevever，此命令也會在指定的一段時間內，定期傳送 ping 給所有路由器，並根據每個所傳回的數目來計算統計資料。

## <a name="syntax"></a>語法

```
pathping [/n] [/h <maximumhops>] [/g <hostlist>] [/p <Period>] [/q <numqueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<targetname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /n | 防止 **pathping** 嘗試將中繼路由器的 IP 位址解析為其名稱。 這可能會加速顯示 **pathping** 結果。 |
| /h `<maximumhops>` | 指定路徑中用來搜尋目標 (目的地) 的躍點數目上限。 預設值為30個躍點。 |
| /g `<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的 **鬆散來源路由** 選項與 *hostlist*中指定的中繼目的地集合。 使用鬆散來源路由時，連續的中繼目的地可以用一或多個路由器來分隔。 主機清單中的位址或名稱數目上限為 **9**。 *Hostlist*是一系列以點分隔的十進位標記法 (的 IP 位址，) 以空格分隔。 |
| /p `<period>` | 指定連續 ping 之間等待的毫秒數。 預設值為250毫秒 (1/4 秒) 。 此參數會將個別的 ping 傳送至每個中繼躍點。 因此，傳送至相同躍點的兩個 ping 之間的間隔會是 *句點* 乘以躍點數目。 |
| 一起 `<numqueries>` | 指定傳送至路徑中每個路由器的 echo 要求訊息數目。 預設值為100查詢。 |
| /w `<timeout>` | 指定等候每個回復的毫秒數。 預設值為3000毫秒 (3 秒) 。 此參數會以平行方式傳送多個 ping。 因此， *timeout* 參數中所指定的時間量不會受限於 *period* 參數中所指定的時間量，以等候偵測之間的時間。 |
| /i `<IPaddress>` | 指定來源地址。 |
| /4 `<IPv4>` | 指定 pathping 只使用 IPv4。 |
| /6 `<IPv6>` | 指定只有使用 IPv6 的 pathping。 |
| `<targetname>` | 指定以 IP 位址或主機名稱識別的目的地。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 所有參數會區分大小寫。

- 為了避免網路擁塞，以及將高載遺失的影響降到最低，應該以夠慢的步調來傳送 ping。

### <a name="example-of-the-pathping-command-output"></a>Pathping 命令輸出的範例

```
D:\>pathping /n contoso1
Tracing route to contoso1 [10.54.1.196]
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

當執行 **pathping** 時，第一個結果會列出路徑。 接下來，大約90秒會顯示一則忙碌的訊息， (時間因躍點計數) 而有所不同。 在這段期間，會從先前列出的所有路由器和它們之間的連結收集資訊。 在這段期間結束時，會顯示測試結果。

在上述範例報表中， **此節點/連結**[ **遺失/傳送 = Pct** ] 和 [ **位址** ] 欄顯示 *172.16.87.218* 與 *192.168.52.1* 之間的連結正在卸載13% 的封包。 躍點2和4的路由器也會捨棄定址的封包，但這種遺失並不會影響其轉送未定址流量的能力。

針對連結所顯示的遺失率（識別為分隔號 (**|**) 在 [ **位址** ] 欄中，表示導致在路徑上轉送的封包遺失的連結擁塞。 針對路由器所顯示的遺失率 (由其 IP 位址識別) 指出這些路由器可能已超載。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [tracert 命令](tracert.md)
