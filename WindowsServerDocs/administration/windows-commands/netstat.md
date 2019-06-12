---
title: netstat
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06684eb73639e7480b5bad39d4d679739682800
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437154"
---
# <a name="netstat"></a>netstat

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示使用中 TCP 連線，在電腦所在接聽、 乙太網路統計資料、 IP 路由表、 IPv4 統計資料 （IP、 ICMP、 TCP 和 UDP 通訊協定），以及 IPv6 統計資料 （適用於 IPv6，ICMPv6，透過 IPv6，TCP 和 UDP 透過 IPv6 通訊協定） 的連接埠。 不含參數， **netstat**顯示使用中 TCP 連線。 

## <a name="syntax"></a>語法
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>參數

|   參數   |                                                                                                                                              描述                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   會顯示所有作用中的 TCP 連線以及電腦所接聽的 TCP 和 UDP 連接埠。                                                                                                   |
|      -e       |                                                                                 顯示 乙太網路統計資料，例如傳送和接收的封包和位元組數目。 這個參數可以結合 **-s**。                                                                                  |
|      -n       |                                                                               顯示使用中 TCP 連線，不過，位址和連接埠號碼以數值表示，不會嘗試判斷名稱。                                                                               |
|      -o       |                          顯示使用中 TCP 連線，並包含每個連線的程序識別碼 (PID)。 您可以找到 Windows 工作管理員 中的處理序 索引標籤上 PID 為基礎的應用程式。 這個參數可以結合 **-a**， **-n**，並 **-p**。                           |
| -p <Protocol> |               顯示指定的通訊協定的連接*通訊協定*。 在此情況下，*通訊協定*可以是 tcp、 udp、 tcpv6-已或 udpv6。 如果這個參數搭配 **-s**通訊協定，所顯示的統計資料*通訊協定*可以是 tcp、 udp、 icmp、 ip、 tcpv6-已、 udpv6、 icmpv6 或 ipv6。                |
|      -s       | 通訊協定，會顯示統計資料。 根據預設，會顯示統計資料，TCP、 UDP、 ICMP 及 IP 通訊協定。 如果未安裝 IPv6 通訊協定，則會顯示統計資料為 TCP 透過 IPv6、 UDP IPv6、 ICMPv6，以及 IPv6 通訊協定。 **-P**參數可以用來指定一組通訊協定。 |
|      -r       |                                                                                                     顯示 IP 路由表的內容。 這是相當於 route print 命令。                                                                                                     |
|  <Interval>   |                                                        選取的資訊會重新顯示每個*間隔*秒。 按下 CTRL + C 來停止在重新顯示。 如果省略這個參數，則**netstat**會列印一次選取的資訊。                                                         |
|      /?       |                                                                                                                                 在命令提示字元顯示說明。                                                                                                                                  |

## <a name="remarks"></a>備註
-   此命令使用的參數必須在前面加上連字號 ( **-** ) 而不是斜線 ( **/** )。
-   **netstat**提供下列統計資料：
    -   Proto 通訊協定 （TCP 或 UDP） 的名稱。
    -   本機位址的 IP 位址在本機電腦和所使用的連接埠號碼。 本機電腦的 IP 位址對應的名稱和連接埠的名稱會顯示除非 **-n**指定參數。 如果尚未建立的連接埠，連接埠號碼會顯示為星號 （*）。
    -   外部位址的 IP 位址和連接埠號碼的通訊端所連接的遠端電腦。 對應至 IP 位址和連接埠的名稱會顯示，除非 **-n**指定參數。 如果尚未建立的連接埠，連接埠號碼會顯示為星號 （*）。
    -   （狀態）表示 TCP 連線的狀態。 可能的狀態如下所示：CLOSE_WAIT 關閉建立 FIN_WAIT_1 FIN_WAIT_2 LAST_ACK 接聽 SYN_RECEIVED SYN_SEND timeD_WAIT，如需有關狀態的 TCP 連線，請參閱 Rfc 793。
-   此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。

## <a name="BKMK_Examples"></a>範例
若要顯示的乙太網路統計資料和所有的通訊協定的統計資料，請輸入：
```
netstat -e -s
```
若要顯示的統計資料，只有 TCP 和 UDP 通訊協定，請輸入：
```
netstat -s -p tcp udp
```
若要顯示使用中 TCP 連線和處理序識別碼每隔 5 秒的資訊，請輸入：
```
netstat -o 5
```
若要顯示作用中的 TCP 連線和處理序識別碼使用數字格式，輸入：
```
netstat -n -o
```

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
