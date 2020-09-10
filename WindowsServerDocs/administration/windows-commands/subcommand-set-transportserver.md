---
title: 子命令集-TransportServer
description: 子命令集的參考文章-TransportServer，可設定傳輸伺服器的設定。
ms.topic: reference
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 59cd0a77decf2151dff1948db619692b47a75101
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637908"
---
# <a name="subcommand-set-transportserver"></a>子命令： set-TransportServer

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定傳輸伺服器的設定。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定傳輸伺服器名稱，則會使用本機伺服器。|
|[/ObtainIpv4From： {Dhcp &#124; 範圍}]|設定 IPv4 位址的來源，如下所示：<p>-[/start： <IP address> ] 設定 IP 位址範圍的開頭。 這是必要的，而且只有在此選項設定為 [ **範圍**] 時才會有效。<br />-[/End： <IP address> ] 設定 IP 位址範圍的結尾。 這是必要的，而且只有在此選項設定為 [ **範圍**] 時才會有效。<br />-[/startPort： <port> ] 設定埠範圍的開頭。<br />-[/EndPort： <port> ] 設定埠範圍的結尾。|
|[/ObtainIpv6From：範圍]|指定 IPv6 位址的來源。 此選項只適用于 Windows Server 2008 R2，且唯一支援的值是 **Range**。<p>-[/start： <IP address> ] 設定 IP 位址範圍的開頭。 這是必要的，而且只有在此選項設定為 [ **範圍**] 時才會有效。<br />-[/End： <IP address> ] 設定 IP 位址範圍的結尾。 這是必要的，而且只有在此選項設定為 [ **範圍**] 時才會有效。<br />-[/startPort： <port> ] 設定埠範圍的開頭。<br />-[/EndPort： <port> ] 設定埠範圍的結尾。|
|[/Profile： {10Mbps &#124; 100Mbps &#124; 1Gbps &#124; 自訂}]|指定要使用的網路設定檔。 此選項僅適用于執行 Windows Server 2008 或 Windows Server 2003 的伺服器。|
|[/MulticastSessionPolicy]|設定多播傳輸的傳輸設定。 此命令僅適用于 Windows Server 2008 R2。<p>-[/Policy： {None &#124; AutoDisconnect &#124; Multistream}] 決定如何處理緩慢的用戶端。 「**無**」表示將所有用戶端保持在一個會話的相同速度。 **AutoDisconnect** 表示低於指定 **/Threshold** 的任何用戶端都會中斷連線。 **Multistream** 表示用戶端將依 **/StreamCount**所指定的方式，分成多個會話。<br />-[/Threshold： <Speed in KBps> ] 設定/policy 的最小傳輸速率（KBps） **： AutoDisconnect**。 低於此速率的用戶端會與多播傳輸中斷連線。<br />-[/StreamCount： {2 &#124; 3}] [/Fallback： {Yes &#124; No}] 判斷/Policy 的會話數目 **： Multistream**。 **2** 表示兩個會話 (fast 和慢) ，而 **3** 表示三個會話 (慢、中、快速) 。<br />-[/Fallback： {Yes &#124; No}] 判斷用戶端是否已中斷連線，如果用戶端) 支援，則會使用另一個方法 (來繼續傳送。 如果您使用 WDS 用戶端，電腦將會切換回單播。 Wdsmcast.exe 不支援 fallback 機制。 此選項也適用于不支援 **Multistream**的用戶端。 在這種情況下，電腦會切換回另一種方法，而不是移至速度較慢的傳輸會話。|
## <a name="examples"></a>範例
若要設定伺服器的 IPv4 位址範圍，請輸入：
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
若要設定伺服器的 IPv4 位址範圍、埠範圍和設定檔，請輸入：
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 TransportServer 命令](using-the-disable-transportserver-command.md) 
[使用 TransportServer 命令](using-the-enable-transportserver-command.md) 
[使用 TransportServer 命令](using-the-get-transportserver-command.md) 
[子命令：啟動-TransportServer](subcommand-start-transportserver.md) 
[子命令： stop-TransportServer](subcommand-stop-transportserver.md)
