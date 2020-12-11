---
description: 深入瞭解：閘道頻寬配置
title: 閘道頻寬配置
manager: grcusanz
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 37982fbef7ee56ec65418aa087b631c3d19bec6d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044916"
---
# <a name="gateway-bandwidth-allocation"></a>閘道頻寬配置

>適用于： Windows Server

在 Windows Server 2016 中，IPsec、GRE 和 L3 的個別通道頻寬是總閘道容量的比率。 因此，客戶會根據預期閘道 VM 的標準 TCP 頻寬來提供閘道容量。

此外，閘道上的最大 IPsec 通道頻寬限制為 \* 客戶提供的 (3/20) 閘道容量。 比方說，如果您將閘道容量設定為 100 Mbps，IPsec 通道容量將會是 150 Mbps。 GRE 和 L3 通道的對等比例分別為1/5 和1/2。

雖然這在大部分的部署中都是可行的，但固定的比率模型不適合高輸送量環境。 即使資料傳輸速率很高 (比方說，在 40 Gbps) 的情況下，SDN 閘道通道的最大輸送量也是因為內部因素所致。

在 Windows Server 2019 中，針對通道類型，最大輸送量是固定的：

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

因此，即使您的閘道主機/VM 支援具有更高輸送量的 Nic，最大可用通道輸送量也是固定的。 另一個需要注意的問題是，它會自動過度布建閘道，這會在為閘道容量提供非常高的數目時發生。

## <a name="gateway-capacity-calculation"></a>閘道容量計算

在理想的情況下，您會將閘道輸送量容量設定為閘道 VM 可用的輸送量。 比方說，如果您有單一閘道 VM，而基礎主機 NIC 輸送量是 25 Gbps，則閘道輸送量也可以設定為 25 Gbps。

如果只針對 IPsec 連線使用閘道，可用的固定容量上限為 5 Gbps。 比方說，如果您在閘道上布建 IPsec 連線，您只能將 (傳入 + 傳出) 的匯總頻寬布建為 5 Gbps。

如果 IPsec 和 GRE 連線都使用閘道，您可以布建最多 5 Gbps 的 IPsec 輸送量或最多 15 Gbps 的 GRE 輸送量。 因此，舉例來說，如果您布建了 2 Gbps 的 IPsec 輸送量，就會有 3 Gbps 的 IPsec 輸送量可在閘道上布建，或剩下 9 Gbps 的 GRE 輸送量。

若要以更多數學詞彙來放置此資訊：

- 閘道的總容量 = 25 Gbps

- 可用的 IPsec 容量總計 = 5 Gbps (固定) 

- 可用的 GRE 容量總計 = 15 Gbps (固定) 

- 此閘道的 IPsec 輸送量比率 = 25/5 = 5 Gbps

- 此閘道的 GRE 輸送量比率 = 25/15 = 5/3 Gbps

例如，如果您配置 2 Gbps 的 IPsec 輸送量給客戶：

閘道上的剩餘可用容量 = 閘道的總容量– IPsec 輸送量比率 * (使用容量配置的 IPsec 輸送量) 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25– 5 * 2 = 15 Gbps

您可以在閘道上配置的其餘 IPsec 輸送量

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

您可以在閘道上配置的剩餘 GRE 輸送量 = 閘道/GRE 輸送量比率的剩餘容量

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbps

輸送量比率會根據閘道的總容量而有所不同。 有一點要注意的是，您應該將總容量設定為閘道 VM 可用的 TCP 頻寬。 如果您在閘道上裝載多個 Vm，您必須據以調整閘道的總容量。

此外，如果閘道容量小於可用的通道容量總計，可用的通道容量總計會設定為閘道容量。 例如，如果您將閘道容量設定為 4 Gbps，IPsec、L3 和 GRE 的可用容量總計會設定為 4 Gbps，而將每個通道的輸送量比率維持為 1 Gbps。

## <a name="windows-server-2016-behavior"></a>Windows Server 2016 行為

Windows Server 2016 的閘道容量計算演算法會維持不變。 在 Windows Server 2016 中，最大 IPsec 通道頻寬限制為閘道上 (3/20) \* 閘道容量。 GRE 和 L3 通道的對等比例分別為1/5 和1/2。

如果您要從 Windows Server 2016 升級到 Windows Server 2019：

1.  **GRE 和 L3 通道：** 當網路控制器節點更新至 Windows Server 2019 之後，Windows Server 2019 配置邏輯就會生效

2.  **IPSec 通道：** 除非閘道集區中的所有閘道都升級為 Windows Server 2019，否則 Windows Server 2016 閘道配置邏輯會繼續運作。 針對閘道集區中的所有閘道，您必須將 Azure 閘道服務設定為 **自動**。

>[!NOTE]
>升級至 Windows Server 2019 之後，閘道可能會隨著配置邏輯從 Windows Server 2016 變更為 Windows Server 2019) 而過度布建 (。 在此情況下，閘道上的現有連線會繼續存在。 閘道的 REST 資源會擲回一則警告，指出閘道已過度布建。 在此情況下，您應該將某些連線移至另一個閘道。

---
