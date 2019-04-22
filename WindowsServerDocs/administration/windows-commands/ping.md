---
title: ping
description: 使用 ping 來確認網路連線。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816749"
---
# <a name="ping"></a>ping

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

**Ping**命令會驗證 IP 層級連線能力，與其他 TCP/IP 電腦傳送網際網路控制訊息通訊協定 (ICMP) 回應要求訊息。 對應回應的回覆訊息，連同一起顯示的來回行程時間的回條。 ping 會用來疑難排解連線、 連線能力，以及名稱解析的主要 TCP/IP 命令。 不含參數， **ping**顯示說明。

## <a name="syntax"></a>語法

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|/t|指定 ping 會繼續將 echo 要求訊息傳送至目的地之前中斷。 若要中斷，並顯示統計資料，請按 CTRL + break。 若要中斷並結束**ping**，按 CTRL + C。|
|/a|指定的目的地 IP 位址上執行的反向名稱解析。 如果成功，則 ping 會顯示對應的主機名稱。|
|/n \<Count\>|指定的數目 echo 要求訊息傳送。 預設值是 4。|
|/l \<Size\>|指定的長度，以位元組為單位，在回應中的資料欄位的要求傳送的訊息。 預設值為 32。 大小上限是 65,527。|
|/f|指定會 echo 要求訊息會傳送不片段旗標設為 1 （適用於僅限 IPv4） IP 標頭中的。 目的地路徑中，路由器，不分段 echo 要求訊息。 此參數可用於疑難排解路徑最大傳輸單位 (PMTU) 問題。|
|/I \<TTL\>|回應的 IP 標頭中指定的 TTL 欄位的值要求傳送的訊息。 預設為主機的預設 TTL 值。 最大值*TTL*為 255。|
|/v \<TOS\>|回應的 IP 標頭中指定的 Service (TOS) 欄位的型別值要求訊息傳送 （適用於僅限 IPv4）。 預設值為 0。 *TOS*指定從 0 到 255 之間的十進位值。|
|/r \<Count\>|指定的 IP 標頭中的記錄路由選項用來記錄 echo 要求訊息所採取的路徑，以及對應回應 （適用於僅限 IPv4） 的回覆訊息。 在路徑中的每個躍點會使用記錄路由選項中的項目。 可能的話，請指定*計數*等於或大於來源與目的地之間的躍點數目。 *計數*必須最少 1 個且最大為 9。|
|/s \<Count\>|指定的 IP 標頭中的 [網際網路] 時間戳記選項用來記錄 echo 要求訊息到達的時間，以及對應回應每個躍點的回覆訊息。 *計數*必須最少 1 個且最多 4 個。 這是必要的連結-本機目的地位址。|
|/j \<Hostlist\>|指定 echo 要求訊息使用鬆散來源路由選項中所指定的中繼目的地設定 IP 標頭*Hostlist* （適用於僅限 IPv4）。 鬆散來源路由中，連續的中繼目的地可以分隔一或多個路由器。 位址或主機清單中的名稱數目上限是 9。 主機清單是以空格分隔的一系列的 IP 位址 （小數點十進位表示法）。|
|/k \<Hostlist\>|指定 echo 要求訊息使用嚴格的來源路由選項中所指定的中繼目的地設定 IP 標頭*Hostlist* （適用於僅限 IPv4）。 透過嚴格的來源路徑下, 一步 的中繼目的地必須是直接連接 （它必須是路由器介面上的芳鄰）。 位址或主機清單中的名稱數目上限是 9。 主機清單是以空格分隔的一系列的 IP 位址 （小數點十進位表示法）。|
|/w \<timeout\>|指定的時間，以毫秒為單位，等候 「 回應回覆訊息中對應至要接收指定的回應要求訊息。 如果在逾時內未收到回應回覆訊息，會顯示 「 要求已逾時 」 錯誤訊息。 預設逾時值為 4000 （4 秒）。|
|/R|指定的反覆存取的路徑追蹤 （適用於僅 IPv6）。|
|/S \<Srcaddr\>|指定要使用 （適用於僅 IPv6） 的來源位址。|
|/4|指定 IPv4 用來執行 ping 命令。 這不是必要參數來識別目標主機的 IPv4 位址。 它是只需要識別目標主機名稱。|
|/6|指定 IPv6 用來執行 ping 命令。 這不是必要參數來識別目標主機的 IPv6 位址。 它是只需要識別目標主機名稱。|
|\<TargetName\>|指定的主機名稱或 IP 位址的目的地。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   您可以使用**ping**測試的電腦名稱和電腦的 IP 位址。 如果 ping 的 IP 位址成功，但 ping 的電腦名稱不是，您可能有名稱解析問題。 在此情況下，請確認您要指定可以解析本機主機檔案中，透過使用網域名稱系統 (DNS) 查詢，或透過 NetBIOS 名稱解析技巧的電腦名稱。
-   此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。

## <a name="BKMK_Examples"></a>範例

下列範例所示**ping**命令輸出：

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Ping 目的地 10.0.99.221 和 10.0.99.221 解析為其主機名稱，請輸入：

```
ping /a 10.0.99.221
```

若要 ping 的目的地 10.0.99.221 10 echo 要求訊息，每個都有 1000 個位元組的資料欄位，輸入：

```
ping /n 10 /l 1000 10.0.99.221
```

若要 ping 目的地 10.0.99.221，並記錄 4 個躍點的路由，請輸入：

```
ping /r 4 10.0.99.221
```

Ping 目的地 10.0.99.221，並指定 10.12.0.1-10.29.3.1-10.1.44.1 的鬆散來源路由，請輸入：

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
