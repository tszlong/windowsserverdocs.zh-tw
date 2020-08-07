---
title: Ping
description: Ping 命令的參考文章，可確認網路連線能力。
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 999e8e80c85a85e2bbc2f933e1985c732ae85328
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884995"
---
# <a name="ping"></a>Ping

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由傳送網際網路控制訊息通訊協定 (ICMP) echo 要求訊息，驗證另一個 TCP/IP 電腦的 IP 層級連線能力。 會顯示對應回應回復訊息的回條，以及來回行程時間。 ping 是用來針對連線能力、可連線性和名稱解析進行疑難排解的主要 TCP/IP 命令。 使用時不含參數，此命令會顯示說明內容。

您也可以使用此命令來測試電腦名稱稱和電腦的 IP 位址。 如果 ping IP 位址成功，但 ping 不到電腦名稱稱，您可能會有名稱解析問題。 在此情況下，請確定您指定的電腦名稱稱可以透過本機 Hosts 檔案解析，方法是使用網域名稱系統 (DNS) 查詢，或透過 NetBIOS 名稱解析技術。

> [!NOTE]
> 只有當網際網路通訊協定 (TCP/IP) 通訊協定在網路連線的網路介面卡內容中安裝為元件時，才可使用此命令。

## <a name="syntax"></a>語法

```
ping [/t] [/a] [/n <count>] [/l <size>] [/f] [/I <TTL>] [/v <TOS>] [/r <count>] [/s <count>] [{/j <hostlist> | /k <hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <targetname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /t | 指定 ping 會繼續傳送 echo 要求訊息到目的地，直到中斷為止。 若要中斷並顯示統計資料，請按 CTRL + ENTER。 若要中斷並結束此命令，請按 CTRL + C。 |
| /a | 指定在目的地 IP 位址上執行反向名稱解析。 如果成功，ping 會顯示對應的主機名稱。 |
| /n`<count>` | 指定傳送的 echo 要求訊息數目。 預設值為 4。 |
| /l`<size>` | 指定傳送的 echo 要求訊息中，**資料**欄位的長度（以位元組為單位）。 預設值為 32。 大小上限為65527。 |
| /f | 指定傳送 echo 要求訊息時，IP 標頭中的「**請勿片段**」旗標會設定為 1 (僅適用于 IPv4 的) 。 目的地的路徑中的路由器無法分割 echo 要求訊息。 這個參數對於疑難排解路徑的最大傳輸單位 (PMTU) 問題很有用。 |
| /I`<TTL>` | 針對傳送的 echo 要求訊息，指定 IP 標頭中的 TTL 欄位的值。 預設值是主機的預設 TTL 值。 *TTL*上限為255。 |
| 停`<TOS>` | 指定 IP 標頭中的服務類型值 (TOS) 欄位，以取得僅 IPv4) 上所傳送 (可用的 echo 要求訊息。 預設值是 0。 *TOS*會指定為從0到255的十進位值。 |
| /r`<count>` | 指定 IP 標頭中的 [**記錄路由**] 選項用來記錄 echo 要求訊息所採取的路徑，以及對應的 echo 回復訊息 (僅適用于 IPv4) 。 路徑中的每個躍點都會使用 [**記錄路由**] 選項中的專案。 可能的話，請指定等於或大於來源與目的地之間的躍點數目的*計數*。 *計數*最少必須為1，最大值為9。 |
| /s`<count>` | 指定 IP 標頭中的 [**網際網路時間戳記]** 選項用來記錄 echo 要求訊息的抵達時間，以及每個躍點的對應回應訊息。 *計數*最少必須為1，最大值為4。 這是連結-本機目的地位址的必要參數。 |
| /j`<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的**鬆散來源路由**選項，並在*hostlist* (中指定的中繼目的地集，僅適用于 IPv4) 。 使用鬆散來源路由，連續的中繼目的地可以由一或多個路由器隔開。 主機清單中的位址或名稱數目上限為9。 主機清單是一系列以小數點十進位標記法 (的 IP 位址，) 以空格分隔。 |
| /k`<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的「**嚴格來源路由**」選項，搭配*hostlist* (中指定的中繼目的地集，僅適用于 IPv4) 。 使用嚴格的來源路由時，必須可以直接連線到下一個中繼目的地 (它必須是路由器) 介面上的鄰近節點。 主機清單中的位址或名稱數目上限為9。 主機清單是一系列以小數點十進位標記法 (的 IP 位址，) 以空格分隔。 |
| /w`<timeout>` | 指定要等候的回應回復訊息對應至所指定 echo 要求訊息的時間量（以毫秒為單位）。 如果未在超時時間內收到回應回復訊息，則會顯示「要求超時」錯誤訊息。 預設的超時時間為 4000 (4 秒) 。 |
| /R | 指定只追蹤反復存取路徑 (僅適用于 IPv6) 。 |
| /S`<Srcaddr>` | 指定要使用 (僅適用于 IPv6) 的來源位址。 |
| /4 | 指定使用 IPv4 來進行 ping。 使用 IPv4 位址識別目標主機時，不需要此參數。 只需要依名稱識別目標主機。 |
| /6 | 指定使用 IPv6 來進行 ping。 不需要使用此參數來識別具有 IPv6 位址的目標主機。 只需要依名稱識別目標主機。 |
| `<targetname>` | 指定目的地的主機名稱或 IP 位址。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="example-of-the-ping-command-output"></a>Ping 命令輸出的範例

```
C:\>ping example.microsoft.com
    pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:
    Reply from 192.168.239.132: bytes=32 time=101ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=100ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

### <a name="examples"></a>範例

若要 ping 目的地10.0.99.221 並將10.0.99.221 解析成其主機名稱，請輸入：

```
ping /a 10.0.99.221
```

若要 ping 包含10個 echo 要求訊息的目的地10.0.99.221，其中每個都有1000個位元組的資料欄位，請輸入：

```
ping /n 10 /l 1000 10.0.99.221
```

若要 ping 目的地10.0.99.221 並記錄4個躍點的路由，請輸入：

```
ping /r 4 10.0.99.221
```

若要 ping 目的地10.0.99.221 並指定 10.12.0.1-10.29.3.1-10.1.44.1 的鬆散來源路由，請輸入：

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
