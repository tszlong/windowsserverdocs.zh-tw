---
title: pathping
description: 了解如何取得網路延遲及遺失資訊使用 pathping 命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837239"
---
# <a name="pathping"></a>pathping

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

提供網路延遲和網路中斷，在來源與目的地之間的中繼躍點的相關資訊。 **pathping**會傳送多個回應要求訊息的來源和目的地之間的每個路由器經過一段時間，然後再計算根據傳回每個路由器的封包的結果。 因為**pathping**顯示程度的封包在任何指定的路由器或連結的遺失，您可以判斷哪一個路由器或子網路可能會發生網路問題。 

**pathping**執行的對等**tracert**藉由識別哪些路由器在路徑上命令。 接著定期傳送 ping 到路由器的所有指定的時間週期內，並計算每個傳回的數字為基礎的統計資料。 不含參數， **pathping**顯示說明。 

## <a name="syntax"></a>語法
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/n|防止**pathping**嘗試解析中繼路由器的 IP 位址，其名稱。 這可能會加速顯示**pathping**結果。|
|/h \<MaximumHops>|搜尋目標 （目的地） 的路徑中指定躍點的數目上限。 預設值為 30 的躍點。|
|/g \<Hostlist >|指定 echo 要求訊息使用鬆散來源路由選項中所指定的中繼目的地設定 IP 標頭*Hostlist*。 鬆散來源路由中，連續的中繼目的地可以分隔一或多個路由器。 位址或主機清單中的名稱數目上限是 9。 *Hostlist*是以空格分隔的一系列的 IP 位址 （小數點十進位表示法）。|
|/p \<Period>|指定連續的 ping 之間要等候的毫秒數。 預設為 250 毫秒 （1/4 秒）。|
|/q \<NumQueries>|指定的數目 echo 要求訊息傳送至路徑中的每個路由器。 預設值為 100 的查詢。|
|/w \<timeout>|指定要等候每個回應的毫秒數。 預設值為 3000 毫秒 （3 秒）。|
|/i \<IPaddress>|指定來源位址。|
|/4 \<IPv4>|指定該 pathping 只使用 IPv4。|
|/6 \<IPv6>|指定該 pathping 只能使用 IPv6。|
|\<TargetName>|指定的目的地，依 IP 位址或主機名稱識別。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **pathping**參數會區分大小寫。
-   若要避免網路壅塞，應該夠低的步調傳送 ping。
-   高載損失的影響降到最低，不會傳送 ping 頻率太高。
-   使用時 **/p**參數，ping 會個別傳送至每個中繼躍點。 基於這個原因，是傳送至相同的躍點的兩個 ping 的間隔*期間*乘以躍點數目。
-   使用時 **/w**參數，可以以平行方式傳送多個 ping。 因為這個緣故中, 所指定的時間量*逾時*參數不會受到中指定的時間量*期間*ping 之間等待的參數。
-   此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。

## <a name="BKMK_Examples"></a>範例

下列範例所示**pathping**命令輸出：

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
當**pathping**會執行，第一個結果列出的路徑。 這是會顯示使用的相同路徑**tracert**命令。 接下來，忙碌的訊息會顯示大約 90 秒的時間 （時間依躍點計數）。 在此期間，會收集資訊從先前所列的所有路由器和連結，兩者之間。 在這段期間結束時，會顯示測試結果。

在上述範例報表**此節點/連結**，**遺失/Sent = Pct**並**位址**資料行會顯示 172.16.87.218 與 192.168.52.1 之間的連結，正在卸除 13封包的百分比。 躍點 2 和 4 的路由器也會捨棄封包傳送給它們，但此遺失不會影響其功能轉送未定址到它們的流量。

顯示連結，以直條所識別的遺失率 (**|**) 中**位址**資料行，表示連結壅塞造成而轉送的封包遺失路徑。 顯示適用於 （依其 IP 位址識別） 的路由器的遺失率表示這些路由器可能多載。

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)