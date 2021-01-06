---
title: 調整容錯移轉基準網路閾值
description: 本文介紹調整容錯移轉叢集網路之臨界值的解決方案。
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.topic: troubleshooting
ms.openlocfilehash: 39469ef6b36af20a2fd8fdab8f3400d5991c2446
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948554"
---
# <a name="iaas-with-sql-alwayson---tuning-failover-cluster-network-thresholds"></a>IaaS 與 SQL AlwaysOn - 微調容錯移轉叢集網路閾值

本文介紹調整容錯移轉叢集網路之臨界值的解決方案。

## <a name="symptom"></a>徵狀

在具有 SQL Server AlwaysOn 的 IaaS 中執行 Windows 容錯移轉叢集節點時，建議將叢集設定變更為較寬鬆的監視狀態。 現成的叢集設定有限制，而且可能會造成不必要的中斷。 預設設定是專為高度調整的內部部署網路而設計，因此不會考慮因多租使用者環境（例如 Windows Azure (IaaS) ）而造成的延遲。

Windows Server 容錯移轉叢集不斷地監視 Windows 叢集中節點的網路連線和健全狀況。  如果無法透過網路連線到節點，則會採取復原動作來復原，並讓叢集中另一個節點上的應用程式和服務恢復連線。 叢集節點之間的通訊延遲可能會導致下列錯誤：

> 錯誤 1135 (系統事件記錄檔) 

叢集節點節點 **1** 已從使用中的容錯移轉叢集成員資格中移除。 此節點上的叢集服務可能已停止。 這也可能是因為節點與容錯移轉叢集中其他使用中節點的通訊中斷。 執行 [驗證設定] 嚮導以檢查您的網路設定。 如果此狀況持續發生，請檢查此節點上與網路介面卡相關的硬體或軟體錯誤。 也請檢查節點所連接之任何其他網路元件的失敗，例如中樞、交換器或橋接器。

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

有兩個設定可用來設定叢集的連線健康情況。

**Delay** –這會定義在節點之間傳送叢集的頻率。  延遲是傳送下一個心跳之前的秒數。  在相同的叢集中，相同子網上的節點與位於不同子網上的節點之間可能會有不同的延遲。

**閾值** –這會定義在叢集採取復原動作之前遺漏的心跳數目。  閾值是一些心跳。  在相同的叢集中，相同子網上的節點與位於不同子網上的節點之間可能會有不同的閾值。

依預設，Windows Server 2016 會將 **SameSubnetThreshold** 設定為10，並將 **SameSubnetDelay** 設定為1000毫秒。 例如，如果連線監視的10秒失敗，就會到達容錯移轉閾值，導致無法連線到叢集成員資格中的節點。 這會導致資源移至叢集中的另一個可用節點。 系統會報告叢集錯誤，包括回報上述) 的叢集錯誤 1135 (。

## <a name="resolution"></a>解決方案

在 IaaS 環境中，放寬叢集網路設定設定。

### <a name="steps-to-verify-current-configuration"></a>驗證目前設定的步驟

檢查目前的叢集網路設定是否使用 cluster 命令：

```powershell
C:\Windows\system32> get-cluster | fl *subnet*
```

每個支援 OS 的預設值、最小值、最大值和建議值

| 描述 | OS | 最小值 | 最大值 | 預設 | 建議 |
|--|--|--|--|--|--|
| CrossSubnetThreshold | 2008 R2 | 3 | 20 | 5 | 20 |
| CrossSubnet 閾值 | 2012 | 3 | 120 | 5 | 20 |
| CrossSubnet 閾值 | 2012 R2 | 3 | 120 | 5 | 20 |
| CrossSubnet 閾值 | 2016 | 3 | 120 | 20 | 20 |
| SameSubnet 閾值 | 2008 R2 | 3 | 10 | 5 | 10 |
| SameSubnet 閾值 | 2012 | 3 | 120 | 5 | 10 |
| SameSubnet 閾值 | 2012 R2 | 3 | 120 | 5 | 10 |
| SameSubnetThreshold | 2016 | 3 | 120 | 10 | 10 |

閾值的值會反映目前關於部署範圍的建議，如下列文章所述：

[微調 Windows Server 2012 R2 中的容錯移轉叢集網路閾值](https://support.microsoft.com/en-us/help/3153887/fine-tuning-failover-cluster-network-thresholds-in-windows-server-2012)

**閾值** 會定義在叢集採取修復動作之前，會遺漏的心跳數目。  閾值是一些心跳。  在相同的叢集中，相同子網上的節點與位於不同子網上的節點之間可能會有不同的閾值。

## <a name="recommendations-for-changing-to-more-relaxed-settings-for-multi-tenant-environments-like-azure-iaas"></a>針對多租使用者環境變更為更寬鬆設定的建議，例如 Azure (IaaS) 

> [!NOTE]
> 藉由調整叢集網路設定來提高叢集環境的復原能力，可能會導致停機時間增加。 如需詳細資訊，請參閱 [調整容錯移轉叢集網路閾值](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834)。

1. 修改為更寬鬆的設定：

    > [!NOTE]
    > 變更叢集閾值會立即生效，您不需要重新開機叢集或任何資源。

    針對相同的子網和 AlwaysOn 可用性群組的跨區域部署，建議使用下列設定。

    ```powershell
    C:\Windows\system32> (get-cluster).SameSubnetThreshold = 20
    ```

    ```powershell
    C:\Windows\system32> (get-cluster).CrossSubnetThreshold = 20
    ```

2. 確認變更：

    ```powershell
    C:\Windows\system32> get-cluster | fl *subnet*
    ```

    :::image type="content" source="media/iaas-sql-failover-cluster/cmd.png" alt-text="cmd" border="false":::

## <a name="references"></a>參考資料

如需微調 Windows 叢集網路設定的詳細資訊，請參閱 [調整容錯移轉叢集網路閾值](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834)。

如需使用 cluster.exe 來調整 Windows 叢集網路設定的詳細資訊，請參閱 [如何設定容錯移轉叢集的叢集網路](/previous-versions/office/exchange-server-2007/bb690953(v=exchg.80)?redirectedfrom=MSDN)。
