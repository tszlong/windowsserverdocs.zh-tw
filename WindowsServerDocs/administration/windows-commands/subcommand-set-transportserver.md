---
title: 子命令 Set-transportserver
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d144ed7d461cbebcd351aa4347fde20f35736c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886599"
---
# <a name="subcommand-set-transportserver"></a>Subcommand: set-TransportServer

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定傳輸伺服器的組態設定。
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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有傳輸指定伺服器名稱，則會使用本機伺服器。|
|[/ObtainIpv4From:{Dhcp &#124; Range}]|會設定的 IPv4 位址的來源，如下所示：<br /><br />-[/start: <IP address>] 設定的 IP 位址範圍開頭。 這是必要的並只適用於此選項設定為**範圍**。<br />-[/ 結束： <IP address>] 設定的 IP 位址範圍的結尾。 這是必要的並只適用於此選項設定為**範圍**。<br />-[/ startPort: <port>] 設定的連接埠範圍的開頭。<br />-[/ EndPort: <port>] 設定的連接埠範圍的結尾。|
|[/ObtainIpv6From:Range]|指定的 IPv6 位址的來源。 此選項僅適用於 Windows Server 2008 R2 和唯一支援的值是**範圍**。<br /><br />-[/start: <IP address>] 設定的 IP 位址範圍開頭。 這是必要的並只適用於此選項設定為**範圍**。<br />-[/ 結束： <IP address>] 設定的 IP 位址範圍的結尾。 這是必要的並只適用於此選項設定為**範圍**。<br />-[/ startPort: <port>] 設定的連接埠範圍的開頭。<br />-[/ EndPort: <port>] 設定的連接埠範圍的結尾。|
|[/ 設定檔: {10Mbps &#124; 100Mbps &#124; 1Gbps&#124;自訂}]|指定要使用的網路設定檔。 此選項只適用於執行 Windows Server 2008 或 Windows Server 2003 的伺服器。|
|[/MulticastSessionPolicy]|設定多點傳送傳輸的傳輸設定。 此命令只適用於 Windows Server 2008 R2。<br /><br />-[/ 原則: {無&#124;自動中斷連線&#124;多資料流}] 會決定如何處理緩慢的用戶端。 **無**表示要保留在一個工作階段中的所有用戶端相同的速度。 **中斷**表示卸除下方指定的任何用戶端 **/Threshold**都已中斷連線。 **多資料流**表示用戶端會分成多個所指定的工作階段 **/StreamCount**。<br />-[/ 臨界值：<Speed in KBps>] 以 kbps 為單位的設定的最小傳送速率 **/Policy:AutoDisconnect**。 放在此速率下的用戶端會從多點傳送傳輸中斷連線。<br />-[/ StreamCount: {2 &#124; 3}] [/ 後援: {[是]&#124;否}] 的工作階段的數目會決定 **/Policy:Multistream**。 **2**表示兩個工作階段 （快速和低速），並**3**表示三個工作階段 (緩慢、 中、 fast)。<br />-[/ 後援: {[是] &#124; No}] 判斷是否已中斷連線的用戶端 tha 會繼續傳送使用其他方法 （如果支援用戶端）。 如果您使用的 WDS 用戶端，電腦會改為單點傳播。 Wdsmcast.exe 不支援的後援機制。 此選項也適用於不支援的用戶端**多資料流**。 在此情況下，電腦會切換回另一種方法，而不需要移動的速度較慢的傳輸工作階段。|
## <a name="BKMK_examples"></a>範例
若要設定伺服器的 IPv4 位址範圍，請輸入：
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
若要設定伺服器的 IPv4 位址範圍、 連接埠範圍和設定檔，請輸入：
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用啟用 TransportServer 命令](using-the-enable-transportserver-command.md)
 [使用 get TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： 開始 TransportServer](subcommand-start-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
