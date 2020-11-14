---
title: Pktmon 命令語法和格式
description: 使用此頁面來瞭解 Windows Vibranium 組建的 pktmon 語法、命令、格式化和輸出。
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 3e4c73af3b00f42301b34e59493f25b6d2ccb95c
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632457"
---
# <a name="pktmon-command-syntax-and-formatting"></a>Pktmon 命令語法和格式

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視器 (Pktmon) 是適用于 Windows 的現成、跨元件網路診斷工具。 它可用於封包捕獲、封包捨棄偵測、封包篩選和計數。 此工具在虛擬化案例（例如容器網路和 SDN）中特別有用，因為它可在網路堆疊中提供可見度。 封包監視器可透過 Vibranium OS 上的 pktmon.exe 命令 (組建 19041) 取得。 您可以使用本主題來瞭解如何瞭解 pktmon 語法、命令格式設定和輸出。

## <a name="quick-start"></a>快速入門

您可以使用下列步驟，在一般案例中開始著手：

1. 識別捕捉所需的封包類型，也就是與封包相關聯的特定 IP 位址、埠或通訊協定。 然後，檢查語法以套用捕獲篩選器，並套用上一個步驟中所識別之封包的篩選器。

   ```PowerShell
   C:\Test> pktmon filter add help
   C:\Test> pktmon filter add <filters>
   ```

2. 啟動 capture 並啟用封包記錄。

   ```PowerShell
   C:\Test> pktmon start --etw
   ```

3. 重現正在診斷的問題。 查詢計數器以確認預期的流量是否存在，以及取得如何在機器中流動流量的高階觀點。

   ```PowerShell
   C:\Test> pktmon counters
   ```

4. 停止捕獲並取出 txt 格式的記錄以進行分析。

   ```PowerShell
   C:\Test> pktmon stop
   C:\Test> pktmon format <etl file>
   ```

請參閱 [分析封包監視器輸出](#analyze-packet-monitor-output) ，以取得分析 txt 輸出的指示。

## <a name="capture-filters"></a>捕獲篩選

強烈建議您在開始任何封包捕獲之前套用篩選，因為當您將焦點放在封包的單一資料流程時，對特定目的地的連線能力會更容易進行。 捕捉 *所有* 網路流量可能會導致輸出太雜訊而無法分析。 針對要報告的封包，它必須符合至少一個篩選準則中指定的所有條件。 一次最多可支援32個篩選準則。

例如，下列篩選器集合將會從 IP 位址10.0.0.10 和埠53上的任何流量，取得任何 ICMP 流量。

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t icmp
C:\Test> pktmon filter add -p 53
```

### <a name="filtering-capability"></a>篩選功能

- 封包監視器支援依 MAC 位址、IP 位址、埠、EtherType、傳輸通訊協定和 VLAN 識別碼進行篩選。 

- 當來源或目的地到達 MAC 位址、IP 位址或埠篩選器時，封包監視將不會區分來源或目的地。 

- 若要進一步篩選 TCP 封包，可以提供選擇性的 TCP 旗標清單來進行比對。 支援的旗標為 FIN、SYN、RST、PSH、ACK、URG、ECE 和 CWR。

  - 例如，下列篩選器會捕獲 IP 位址10.0.0.10 所傳送或接收的所有 SYN 封包：

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t tcp syn
```

- 如果已將 [-e] 旗標新增到任何篩選，則封包監視器可將篩選套用至封裝的內部封包，以及外部封包。 支援的封裝方法有 VXLAN、GRE、NVGRE 和 IP IP。 自訂 VXLAN 埠是選擇性的，預設值為4789。

### <a name="pktmon-filters-syntax"></a>Pktmon 篩選語法

```PowerShell
C:\Test> pktmon filter help
pktmon filter { list | add | remove } [OPTIONS | help]

Commands
    list      Display active packet filters.
    add       Add a filter to control which packets are reported.
    remove    Removes all filters.

help
    Show help text for a command.

C:\Test> pktmon filter add help
pktmon filter add <name> [-m mac [mac2]] [-v vlan] [-d { IPv4 | IPv6 | number }]
                         [-t { TCP [flags...] | UDP | ICMP | ICMPv6 | number }]
                         [-i ip [ip2]] [-p port [port2]] [-e [port]]
    Add a filter to control which packets are reported. For a packet to be
    reported, it must match all conditions specified in at least one filter.
    Up to 8 filters can be active at once.

    NOTE1: When two MACs (-m), IPs (-i), or ports (-p) are specified, the filter
           matches packets that contain both. It will not distinguish between source
           or destination for this purpose.

name
    Optional name or description of the filter.

Ethernet frame
    -m, --mac[-address]
        Match source or destination MAC address. See NOTE1 above.

    -v, --vlan
        Match by VLAN Id (VID) in the 802.1Q header.

    -d, --data-link[-protocol], --ethertype
        Match by data link (layer 2) protocol. Can be IPv4, IPv6, ARP, or
        a protocol number.

IP header
    -t, --transport[-protocol], --ip-protocol
        Match by transport (layer 4) protocol. Can be TCP, UDP, ICMP, ICMPv6, or
        a protocol number.
        To further filter TCP packets, an optional list of TCP flags to match can
        be provided. Supported flags are FIN, SYN, RST, PSH, ACK, URG, ECE, and CWR.

    -i, --ip[-address]
        Match source or destination IP address. See NOTE1 above.
        To match by subnet, use CIDR notation with the prefix length.

TCP/UDP header
    -p, --port
        Match source or destination port number. See NOTE1 above.

Encapsulation
    -e, --encap
        This filter also applies to encapsulated inner packets, in addition to the outer
        packet. Supported encapsulation methods are VXLAN, GRE, NVGRE, and IP-in-IP.
        Custom VXLAN port is optional, and defaults to 4789.

Example 1: Ping filter
        pktmon filter add MyPing -i 10.10.10.10 -t ICMP

Example 2: TCP SYN filter for SMB traffic
    pktmon filter add MySmbSyn -i 10.10.10.10 -t TCP SYN -p 445

Example 3: Subnet filter
    pktmon filter add MySubnet -i 10.10.10.0/24
```

## <a name="packet-capture-and-logging"></a>封包捕獲和記錄

封包監視器可以使用或不使用封包記錄來捕捉網路流量。 如需在不使用封包記錄的情況下捕獲流量的詳細資訊，請參閱下面的封包計數器一節 若要捕獲和記錄封包，請將 **[--etw]** 參數新增至 start 命令。

選取要透過 **[-c]** 參數監視的元件。 它可以是所有元件、僅限 Nic 或元件識別碼清單。 預設值是在所有元件上捕獲流量。 只使用 [-d] 參數監視卸載的封包。 預設值是捕捉流入和丟棄的封包。

例如，下列命令將會捕捉所有網路介面卡的封包計數器，而不會記錄封包：

```PowerShell
C:\Test> pktmon start -c nics
```

但是，下列命令只會捕捉經過元件4和5的已捨棄封包，並加以記錄：

```PowerShell
C:\Test> pktmon start --etw -c 4,5 -d
```

### <a name="packet-logging-capability"></a>封包記錄功能

- 封包監視器支援多種記錄模式：

  - 迴圈：當達到檔案大小上限時，新的封包會覆寫最舊的封包。 這是預設的記錄模式。
  - 多檔案：達到檔案大小上限時，就會建立新的記錄檔。 記錄檔會依序編號： PktMon1、PktMon2 和 etl 等等。套用此記錄模式以保留所有記錄，但請留意儲存體的使用方式。 注意：使用每個記錄檔的檔案建立時間戳記作為捕捉中特定時間範圍的指示。
  - 即時：封包會即時顯示在畫面上。 未建立任何記錄檔。 使用 Ctrl + C 停止監視。
  - Memory：事件會寫入至圓形記憶體緩衝區。 緩衝區大小是透過 **[-s]** 參數指定。 緩衝區內容會在停止捕捉之後寫入記錄檔。 針對非常雜訊的案例使用此記錄模式，以在記憶體緩衝區中非常短的時間內捕獲大量的流量。 使用任何其他記錄模式時，可能會遺失某些流量。

- 指定要透過 **[-p]** 參數記錄多少封包。 將參數設定為0，以記錄每個封包的整個封包（不論其大小為何）。 預設值為128個位元組，其中應該包含大部分封包的標頭。

- 透過 **[-s]** 參數指定記錄檔的大小。 在封包監視器開始以較新的封包覆寫較舊的封包之前，這會是迴圈記錄模式中的檔案大小上限。 在 [封包監視] 建立新檔案以記錄下一個封包之前，這也會是多檔案記錄模式中每個檔案的大小上限。 您也可以使用這個參數來設定記憶體記錄模式的緩衝區大小。

### <a name="pktmon-start-syntax"></a>Pktmon 開始語法

```PowerShell
C:\Test> pktmon start help
pktmon start [-c { all | nics | [ids...] }] [-d] [--etw [-p size] [-k keywords]]
             [-f] [-s] [--log-mode {circular | multi-file | real-time | memory}]
    Start packet monitoring.

-c, --components
    Select components to monitor. Can be all components, NICs only, or a
    list of component ids. Defaults to all.

-d, --drop-only
    Only report dropped packets. By default, successful packet propagation
    is reported as well.

ETW Logging
    --etw
        Start a logging session for packet capture.

    -p, --packet-size
        Number of bytes to log from each packet. To always log the entire
        packet, set this to 0. Default is 128 bytes.

    -k, --keywords
        Hexadecimal bitmask (i.e. sum of the below flags) that controls
        which events are logged. Default is 0x012.

        Flags:
        0x001 - Internal Packet Monitor errors.
        0x002 - Information about components, counters and filters.
                This information is added to the end of the log file.
        0x004 - Source and destination information for the first
                packet in NET_BUFFER_LIST group.
        0x008 - Select packet metadata from NDIS_NET_BUFFER_LIST_INFO
                enumeration.
        0x010 - Raw packet, truncated to the size specified in
                [--packet-size] parameter.

    -f, --file-name
        .etl log file. Default is PktMon.etl.

    -s, --file-size
        Maximum log file size in megabytes. Default is 512 MB.

    -l, --log-mode
        Select logging mode. Default is circular.

        circular
            New events overwrite the oldest ones when
            when the maximum file size is reached.

        multi-file
            A new log file is created when the maximum file size is reached.
            Log files are sequentially numbered. PktMon1.etl, PktMon2.etl, etc.

        real-time
            Display events and packets on screen at real time. No log file is created.
            Press Ctrl+C to stop monitoring.

        memory
            Events are written to a circular memory buffer.
            Buffer size is specified in [--file-size] parameter.
            Buffer contents is written to a log file during stop operation.

Example: pktmon start --etw -l real-time
```

## <a name="packet-analysis-and-formatting"></a>封包分析和格式化

封包監視器會以 ETL 格式產生記錄檔。 有多種方式可格式化 ETL 檔案以進行分析：

-  (預設選項) 將記錄轉換成文字格式，然後使用 TextAnalysisTool.NET 之類的文字編輯器工具加以分析。 封包資料會以 TCPDump 格式顯示。 遵循下列指南，以瞭解如何分析文字檔中的輸出。
- 將記錄轉換成 pcapng 格式，以使用[Wireshark](pktmon-pcapng-support.md)進行分析*
- 使用[網路監視器](pktmon-netmon-support.md)開啟 ETL 檔案*

>[!NOTE]
>* 使用上面的超連結，瞭解如何在 **Wireshark** 和 **網路監視器** 中剖析及分析封包監視器記錄。

### <a name="pktmon-format-syntax"></a>Pktmon 格式語法

```PowerShell
C:\Test> pktmon format help
pktmon format log.etl [-o log.txt] [-b] [-v [level]] [-x] [-e] [-l [port]
    Convert log file to text format.

-o, --out
    Name of the formatted text file.

-s, --stats-only
    Display log file statistical information.

Network packet formatting options

    -b, --brief
        Abbreviated packet format.

    -v, --verbose
        Verbosity level [1..3].

    -x, --hex
        Hexadecimal format.

    -e, --no-ethernet
        Don't print ethernet header.

    -l, --vxlan
        Custom VXLAN port.


Example: pktmon format C:\tmp\PktMon.etl

C:\Test> pktmon pcapng help
pktmon pcapng log.etl [-o log.pcapng]
    Convert log file to pcapng format.
    Dropped packets are not included by default.

-o, --out
    Name of the formatted pcapng file.

-d, --drop-only
    Convert dropped packets only.

-c, --component-id
    Filter packets by a specific component ID.

Example: pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

### <a name="analyze-packet-monitor-output"></a>分析封包監視器輸出

封包監視器會依網路堆疊的每個元件捕獲封包的快照。 因此，在下圖中，每個封包的多個快照集 (會以藍色方塊) 的線條來表示。
這些封包快照集會以幾行表示， (紅和綠方塊) 。 至少有一行包含從時間戳記開始的封包實例相關資料。 之後，下圖中至少有一行 (粗體，) 以文字格式顯示剖析的原始封包， (不含時間戳記) ;如果封裝封包，則可以是多行，例如綠色方塊中的封包。

<center>

:::image type="content" source="media/pktmon-log-example.png" alt-text="封包監視器的 txt 記錄輸出範例" border="false":::

</center>

若要相互關聯相同封包的所有快照集，請監視 PktGroupId 和 PktNumber 值 (在黃色) 中反白顯示。相同封包的所有快照集都必須具有這兩個共通的值。 [外觀] 值 (以藍色反白顯示) 作為相同封包的每個後續快照集的計數器。 例如，封包的第一個快照集 (第一次出現在網路堆疊中的封包) 值為1，下一個快照的值為2，依此類推。

每個封包快照集都有元件識別碼 (上圖中加上底線，) 表示與快照集相關聯的元件。 若要解析元件名稱和參數，請在記錄檔底部的元件清單中搜尋此元件識別碼。 元件資料表的一部分顯示在下列影像中，以黃色顯示「元件1」 (這是上一次捕捉上述上一個快照的元件) 。
具有2個邊緣的元件會在每個邊緣報告2個快照集 (例如，在上述影像中的快照集，例如具有3和外觀4的快照集) 。

在每個記錄檔的底部，會顯示如下圖所示的 [篩選] 清單， (以藍色) 反白顯示。 每個篩選器會在) 下列範例中顯示) 指定 (通訊協定 ICMP (的參數，而其餘的參數則顯示零。

<center>

:::image type="content" source="media/pktmon-log-example-components.png" alt-text="封包監視器的 txt 記錄檔輸出元件範例":::

</center>

若為捨棄的封包，就會在任何行的任何一行之前出現 "drop"，這些行代表封包捨棄的快照集。 每個丟棄的封包也會提供 dropReason 值。 這個 dropReason 參數提供封包捨棄原因的簡短描述;例如，MTU 不符、已篩選的 VLAN 等等。

<center>

:::image type="content" source="media/dropped-packet-log-example.png" alt-text="已卸載的封包記錄範例":::

</center>

## <a name="packet-counters"></a>封包計數器

封包監視器計數器可在整個網路堆疊中提供網路流量的高階觀點，而不需要分析記錄檔，這可能是相當昂貴的進程。 啟動封包監視捕捉之後，使用 **pktmon 計數器** 查詢封包計數器來檢查流量模式。 使用 **pktmon 重設** ，將計數器重設為零，或使用 **pktmon stop** 停止整體監視。

- 計數器是由具有網路介面卡的系結堆疊和底部的通訊協定來排列。
- Tx/Rx：計數器會分成兩個數據行，以傳送 (德克薩斯) ，並接收 (Rx) 方向。  
- 邊緣：當封包跨越元件界限 (edge) 時，元件會報告封包傳播。 每個元件可能會有一或多個邊緣。 微型埠驅動程式通常具有單一最上層，通訊協定具有單一較低的邊緣，而篩選器驅動程式具有上下邊緣。  
- 捨棄：在相同的資料表中報告封包卸載計數器。

在下列範例中，已啟動新的捕獲，然後使用 **pktmon 計數器** 命令來查詢計數器，然後才停止捕捉。 這些計數器會顯示一個封包，讓它從網路堆疊中取出，從通訊協定層一直到實體網路介面卡，而其回應是在另一方向回來。 如果偵測到或回應遺失，很容易就能透過計數器偵測到。

<center>

:::image type="content" source="media/pktmon-counters-with-perfect-flow.png" alt-text="具有完美流程的封包計數器範例" border="false":::

</center>

在下一個範例中，會在 "Counter" 資料行下回報 drop。 使用 **pktmon 的計數器（json 格式）** 要求 json 格式的計數器資料，以抓取每個元件的最後一個捨棄原因，或分析輸出記錄檔以取得詳細資訊。

<center>

:::image type="content" source="media/pktmon-counters-drop-example.png" alt-text="具有已捨棄封包的封包計數器範例" border="false":::

</center>

如這些範例所示，計數器可以透過圖表提供許多資訊，只要快速查看就能進行分析。

### <a name="pktmon-counters-syntax"></a>Pktmon 計數器語法

```PowerShell
C:\Test> pktmon counters help
pktmon [comp] counters [-t { all | drop | flow }] [-z] [--json]
    Display current per-component counters.

-t, --counter-type
    Select which types of counters to show.
    Supported values are all counters (default), drops only, or flows only.

-z, --show-zeros
    Show counters that are zero in both directions.

-i, --show-hidden
    Show components that are hidden by default.

--json
    Output the counters in JSON format.
```

## <a name="networking-stack-layout"></a>網路堆疊版面配置

透過命令 **pktmon 清單** 來檢查網路堆疊版面配置。

:::image type="content" source="media/pktmon-networking-stack-example.png" alt-text="網路堆疊版面配置範例" border="false":::

此命令會顯示網路元件 (驅動程式) 由介面卡系結排列。

典型的系結是由下列各項所組成：

- 單一網路介面卡 (NIC) 
- 有幾個 (可能有零個) 篩選器驅動程式
- 一或多個通訊協定驅動程式 (TCP/IP 或其他) 

每個元件都是以封包監視器元件識別碼來唯一識別，此識別碼是用來以個別元件為目標來進行監視。

>[!NOTE]
>識別碼不是持續性的，而且可能會在重新開機時變更，並隨著封包監視器的驅動程式重新開機

### <a name="pktmon-list-syntax"></a>Pktmon 清單語法

```powershell
C:\Test> pktmon list help
pktmon [comp] list
    List all active components.

-i, --show-hidden
    Show components that are hidden by default.

--json
    Output the list in JSON format.
```