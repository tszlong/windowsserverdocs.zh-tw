---
title: Ping
description: Ping 命令的參考文章，可驗證網路連線能力。
ms.topic: reference
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: e2f2e2c042117cad94f2204e64303d01be26f249
ms.sourcegitcommit: 8c0a419ae5483159548eb0bc159f4b774d4c3d85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235785"
---
# <a name="ping"></a>Ping

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由傳送網際網路控制訊息通訊協定 (ICMP) echo 要求訊息，來驗證另一部 TCP/IP 電腦的 IP 層級連線能力。 隨即會顯示對應的回應回復訊息的回條，以及來回行程時間。 ping 是用來針對連線能力、連線能力和名稱解析進行疑難排解的主要 TCP/IP 命令。 使用時不含參數，此命令會顯示說明內容。

您也可以使用此命令來測試電腦的電腦名稱稱和 IP 位址。 如果 ping IP 位址成功，但未 ping 電腦名稱稱，您可能會遇到名稱解析問題。 在此情況下，請確定您指定的電腦名稱稱可以透過本機 Hosts 檔案解析，方法是使用網域名稱系統 (DNS) 查詢，或透過 NetBIOS 名稱解析技術。

> [!NOTE]
> 只有在網路連線的網路介面卡內容中，將網際網路通訊協定 (TCP/IP) 安裝為元件時，才能使用此命令。

## <a name="syntax"></a>語法

```
ping [/t] [/a] [/n <count>] [/l <size>] [/f] [/I <TTL>] [/v <TOS>] [/r <count>] [/s <count>] [{/j <hostlist> | /k <hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <targetname>
```

### <a name="parameters"></a>參數

| 參數 | Description |
|:--:|---|
| /t | 指定 ping 會在中斷之前，繼續傳送 echo 要求訊息到目的地。 若要中斷和顯示統計資料，請按 CTRL + ENTER。 若要中斷並關閉此命令，請按 CTRL + C。 |
| /a | 指定在目的地 IP 位址上執行反向名稱解析。 如果成功，ping 會顯示對應的主機名稱。 |
| n `<count>` | 指定要傳送的 echo 要求訊息數目。 預設值為 4。 |
| /l `<size>` | 指定 echo 要求訊息中 **資料** 欄位的長度（以位元組為單位）。 預設值為 32。 大小上限為65527。 |
| /f | 指定將 IP 標頭中的「 **請勿片段** 」旗標傳送給 echo 要求訊息，此標頭設定為 1 (僅適用于 IPv4) 。 目的地路徑中的路由器無法分割 echo 要求訊息。 此參數適用于針對 (PMTU) 問題的路徑最大傳輸單位進行疑難排解。 |
| /I `<TTL>` | 針對傳送的 echo 要求訊息，指定 IP 標頭中的存留時間 (TTL) 欄位的值。 預設值是主機的預設 TTL 值。 最大 *TTL* 為255。 |
| /v `<TOS>` | 指定 IP 標頭中的服務 (TOS) 欄位的值，以取得傳送的 echo 要求訊息 (僅適用于 IPv4) 。 預設值是 0。 將 *TOS* 指定為從0到255的十進位值。 |
| /r `<count>` | 指定 IP 標頭中的 **記錄路由** 選項，用來記錄 echo 要求訊息所採用的路徑，以及對應的 echo 回復訊息 (僅適用于 IPv4) 。 路徑中的每個躍點都會使用 **記錄路由** 選項中的專案。 可能的話，請指定等於或大於來源和目的地之間的躍點數目的 *計數* 。 *計數* 的最小值必須為1，最大值為9。 |
| /s `<count>` | 指定 IP 標頭中的 [ **Internet timestamp]** 選項用來記錄每個躍點的 echo 要求訊息和對應的回應訊息的抵達時間。 *計數* 的最小值必須為1，最大值為4。 這是連結-本機目的地位址的必要項。 |
| /j `<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的 **鬆散來源路由** 選項，以及在 *hostlist* (中指定的中繼目的地集合（僅限 IPv4) ）。 使用鬆散來源路由時，連續的中繼目的地可以用一或多個路由器來分隔。 主機清單中的位址或名稱數目上限為9。 主機清單是一系列以點分隔的十進位標記 (的 IP 位址，) 以空格分隔。 |
| /k `<hostlist>` | 指定 echo 要求訊息使用 IP 標頭中的「 **嚴格來源路由** 」選項，以及在 *hostlist* 中指定的中繼目的地組（僅適用于 IPv4 的 (）) 。 使用 strict 來源路由時，必須能夠直接連線到下一個中繼目的地 (它必須是路由器) 介面上的鄰近位置。 主機清單中的位址或名稱數目上限為9。 主機清單是一系列以點分隔的十進位標記 (的 IP 位址，) 以空格分隔。 |
| /w `<timeout>` | 以毫秒為單位，指定等待回應回復訊息對應到指定的 echo 要求訊息的時間量（以毫秒為單位）。 如果在超時時間內未收到回應回復訊息，則會顯示 [要求超時] 錯誤訊息。 預設的超時時間是 4000 (4 秒) 。 |
| /R | 指定追蹤來回路徑 (僅適用于 IPv6) 。 |
| /S `<Srcaddr>` | 指定要使用 (僅適用于 IPv6) 的來源位址。 |
| /4 | 指定用來偵測的 IPv4。 使用 IPv4 位址找出目標主機不需要此參數。 只需要依名稱識別目標主機。 |
| /6 | 指定用來偵測的 IPv6。 此參數不需要用來識別具有 IPv6 位址的目標主機。 只需要依名稱識別目標主機。 |
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

若要 ping 目的地10.0.99.221 並將10.0.99.221 解析為其主機名稱，請輸入：

```
ping /a 10.0.99.221
```

若要使用10個 echo 要求訊息來 ping 目的地10.0.99.221，其中每個都有1000個位元組的資料欄位，請輸入：

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
