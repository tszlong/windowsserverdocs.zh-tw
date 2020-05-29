---
title: 調整容錯移轉基準網路閾值
description: 本文介紹調整容錯移轉叢集網路閾值的解決方案。
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: c0e2f0309049f0271a223c2a23012eb2efa8d843
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150166"
---
# <a name="iaas-with-sql-alwayson---tuning-failover-cluster-network-thresholds"></a>IaaS 與 SQL AlwaysOn - 微調容錯移轉叢集網路閾值

本文介紹調整容錯移轉叢集網路閾值的解決方案。

## <a name="symptom"></a>徵狀

使用 SQL Server AlwaysOn 在 IaaS 中執行 Windows 容錯移轉叢集節點時，建議您將叢集設定變更為較寬鬆的監視狀態。 現成的叢集設定會受到限制，而且可能會造成不必要的中斷。 預設設定是專為對內部部署網路進行高度調整而設計，並不會考慮因多租使用者環境（例如 Windows Azure （IaaS））而造成延遲的可能性。

Windows Server 容錯移轉叢集會持續監視 Windows 叢集中節點的網路連線和健全狀況。  如果無法透過網路連線到節點，則會採取復原動作來復原，並讓叢集中另一個節點上的應用程式和服務恢復連線。 叢集節點之間通訊的延遲可能會導致下列錯誤：  

> 錯誤1135（系統事件記錄檔）

叢集節點**1**叢集已從作用中的容錯移轉叢集成員資格中移除。 此節點上的叢集服務可能已停止。 這也可能是因為節點與容錯移轉叢集中的其他使用中節點失去通訊。 執行 [驗證設定向導] 來檢查您的網路設定。 如果此狀況持續發生，請檢查與此節點上網路介面卡相關的硬體或軟體錯誤。 另請檢查節點所連接的任何其他網路元件是否發生失敗，例如集線器、交換器或橋接器。

Cluster .log 範例：

```console
0000ab34.00004e64::2014/06/10-07:54:34.099 DBG   [NETFTAPI] Signaled NetftRemoteUnreachable event, local address 10.xx.x.xxx:3343 remote address 10.x.xx.xx:3343
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] got event: Remote endpoint 10.xx.xx.xxx:~3343~ unreachable from 10.xx.x.xx:~3343~
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Marking Route from 10.xxx.xxx.xxxx:~3343~ to 10.xxx.xx.xxxx:~3343~ as down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] Checking to see if all routes for route (virtual) local fexx::xxx:5dxx:xxxx:3xxx:~0~ to remote xxx::cxxx:xxxd:xxx:dxxx:~0~ are down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] All routes for route (virtual) local fxxx::xxxx:5xxx:xxxx:3xxx:~0~ to remote fexx::xxxx:xxxx:xxxx:xxxx:~0~ are down
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [CORE] Node 8: executing node 12 failed handlers on a dedicated thread
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: Cleaning up connections for n12.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [Nodename] Clearing 0 unsent and 15 unacknowledged messages.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: n12 node object is closing its connections
0000ab34.00008b68::2014/06/10-07:54:34.099 INFO  [DCM] HandleNetftRemoteRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 1: Old: 05.936, Message: Response, Route sequence: 150415, Received sequence: 150415, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:28.000, Ticks since last sending: 4
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: closing n12 node object channels
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 2: Old: 06.434, Message: Request, Route sequence: 150414, Received sequence: 150402, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.665, Ticks since last sending: 36
0000ab34.0000a8ac::2014/06/10-07:54:34.099 INFO  [DCM] HandleRequest: dcm/netftRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 3: Old: 06.934, Message: Response, Route sequence: 150414, Received sequence: 150414, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.165, Ticks since last sending: 4
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 4: Old: 07.434, Message: Request, Route sequence: 150413, Received sequence: 150401, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:26.664, Ticks since last sending: 36
```

```console
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realLocal>10.xxx.xx.xxx:~3343~</realLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realRemote>10.xxx.xx.xxx:~3343~</realRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualLocal>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualRemote>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Delay>1000</Delay>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Threshold>5</Threshold>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Priority>140481</Priority>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Attributes>2147483649</Attributes>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO  </struct mscs::FaultTolerantRoute>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO   removed
```

```console
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: Lost quorum (3 4 5 6 7 8)
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: goingAway: 0, core.IsServiceShutdown: 0
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   lost quorum (status = 5925)
```

## <a name="cause"></a>原因

有兩個設定可用來設定叢集的連線能力健全狀況。

**延遲**–這會定義在節點之間傳送叢集信號的頻率。  延遲是傳送下一個信號的秒數。  在相同的叢集中，相同子網上的節點與位於不同子網的節點之間，可能會有不同的延遲。

**閾值**–這會定義在叢集採取復原動作之前，所遺漏的「心跳」數目。  臨界值是一些心跳。  在相同的叢集中，相同子網上的節點與位於不同子網的節點之間，可能會有不同的閾值。

根據預設，Windows Server 2016 會將**SameSubnetThreshold**設定為10，並將**SameSubnetDelay**設為1000毫秒。 例如，如果連線監視失敗10秒，就會達到容錯移轉閾值，導致無法連線到該節點從叢集成員資格中移除。 這會導致資源移到叢集上的另一個可用節點。 系統會回報叢集錯誤，包括回報叢集錯誤1135（上方）。

## <a name="resolution"></a>解決方案

在 IaaS 環境中，放寬叢集網路設定。

### <a name="steps-to-verify-current-configuration"></a>確認目前設定的步驟

檢查目前的叢集網路設定，使用的是 cluster 命令：

```powershell
C:\Windows\system32> get-cluster | fl *subnet*
```

每個支援作業系統的預設、最小值、最大值和建議值

|   |作業系統|最小值|最大值|預設|建議|
|---|---|---|---|---|---|
|CrossSubnetThreshold|2008 R2|3|20|5|20|
|CrossSubnet 臨界值|2012|3|120|5|20|
|CrossSubnet 臨界值|2012 R2|3|120|5|20|
|CrossSubnet 臨界值|2016|3|120|20|20|
|SameSubnet 臨界值|2008 R2|3|10|5|10|
|SameSubnet 臨界值|2012|3|120|5|10
|SameSubnet 臨界值|2012 R2|3|120|5|10|
|SameSubnetThreshold|2016|3|120|10|10|
|||||||

[閾值] 的值會反映目前關於部署範圍的建議，如下列文章所述：

[微調 Windows Server 2012 R2 中的容錯移轉叢集網路閾值](https://support.microsoft.com/en-us/help/3153887/fine-tuning-failover-cluster-network-thresholds-in-windows-server-2012)

**閾值**會定義在叢集採取復原動作之前，會遺漏的心跳數。  臨界值是一些心跳。  在相同的叢集中，同一個子網上的節點與位於不同子網的節點之間，可能會有不同的閾值。

## <a name="recommendations-for-changing-to-more-relaxed-settings-for-multi-tenant-environments-like-azure-iaas"></a>針對多租使用者環境（例如 Azure （IaaS））變更為更寬鬆設定的建議

> [!NOTE]
> 藉由調整叢集網路設定值來增加叢集環境的復原能力，可能會導致停機時間增加。 如需詳細資訊，請參閱[調整容錯移轉叢集網路閾值](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834)。

1. 修改為更寬鬆的設定：

    > [!NOTE]
    > 變更叢集閾值會立即生效，您不需要重新開機叢集或任何資源。

    針對 AlwaysOn 可用性群組的相同子網和跨區域部署，建議使用下列設定。

    ```powershell
    C:\Windows\system32> (get-cluster).SameSubnetThreshold = 20
    ```

    ```powershell
    C:\Windows\system32> (get-cluster).CrossSubnetThreshold = 20
    ```

2. 驗證變更：

    ```powershell
    C:\Windows\system32> get-cluster | fl *subnet*
    ```

    :::image type="content" source="media/iaas-sql-failover-cluster/cmd.png" alt-text="cmd" border="false":::

## <a name="references"></a>參考

如需微調 Windows 叢集網路設定的詳細資訊，請參閱[調整容錯移轉叢集網路閾值](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834)。

如需使用 cluster.exe 調整 Windows 叢集網路設定的相關資訊，請參閱[如何設定容錯移轉叢集的叢集網路](/previous-versions/office/exchange-server-2007/bb690953(v=exchg.80)?redirectedfrom=MSDN)。
