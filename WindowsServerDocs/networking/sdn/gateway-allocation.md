---
title: 閘道頻寬配置
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 08/22/2018
ms.openlocfilehash: 8e0709360449e1175988b14a963047ee72f26ade
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313029"
---
# <a name="gateway-bandwidth-allocation"></a>閘道頻寬配置

>適用于： Windows Server

在 Windows Server 2016 中，IPsec、GRE 和 L3 的個別通道頻寬是總閘道容量的比率。 因此，客戶會根據預期此閘道 VM 的標準 TCP 頻寬來提供閘道容量。

此外，閘道上的最大 IPsec 通道頻寬限制為（3/20）客戶所提供的\*閘道容量。 因此，例如，如果您將閘道容量設定為 100 Mbps，則 IPsec 通道容量會是 150 Mbps。 GRE 和 L3 通道的對等比例分別為1/5 和1/2。

雖然這適用于大部分的部署，但固定比例模型不適合高輸送量環境。 即使資料傳輸速率很高（例如，高於 40 Gbps），SDN 閘道通道的最大輸送量也會因為內部因素而達到上限。

在 Windows Server 2019 中，針對通道類型，最大輸送量是固定的：

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

因此，即使您的閘道主機/VM 支援具有更高輸送量的 Nic，最大可用通道輸送量也是固定的。 這項處理的另一個問題是任意過度布建閘道，這會在為閘道容量提供非常高的數目時發生。

## <a name="gateway-capacity-calculation"></a>閘道容量計算

在理想的情況下，您會將閘道輸送量容量設定為可供閘道 VM 使用的輸送量。 因此，例如，如果您有單一閘道 VM，而基礎主機 NIC 輸送量是 25 Gbps，則閘道輸送量也可以設為 25 Gbps。

如果只使用 IPsec 連線的閘道，可用的固定容量上限為 5 Gbps。 因此，例如，如果您在閘道上布建 IPsec 連線，您只能以 5 Gbps 的形式布建到匯總頻寬（傳入 + 傳出）。

如果同時使用閘道來進行 IPsec 和 GRE 連線，您可以布建最多 5 Gbps 的 IPsec 輸送量，或最多可達 15 Gbps 的 GRE 輸送量。 因此，例如，如果您布建 2 Gbps 的 IPsec 輸送量，您會有 3 Gbps 的 IPsec 輸送量留在閘道上，或剩 9 Gbps 的 GRE 輸送量。

若要將它放在更多數學詞彙中：

- 閘道的總容量 = 25 Gbps

- 可用的 IPsec 容量總計 = 5 Gbps （固定）

- 可用的 GRE 容量總計 = 15 Gbps （固定）

- 此閘道的 IPsec 輸送量比例 = 25/5 = 5 Gbps

- 此閘道的 GRE 輸送量比例 = 25/15 = 5/3 Gbps

例如，如果您將 2 Gbps 的 IPsec 輸送量配置給客戶：

閘道上的剩餘可用容量 = 閘道的總容量– IPsec 輸送量比率 * 已配置的 IPsec 輸送量（已使用的容量）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25 – 5 * 2 = 15 Gbps

您可以在閘道上配置的其餘 IPsec 輸送量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

您可以在閘道上配置的剩餘 GRE 輸送量 = 閘道/GRE 輸送量比率的剩餘容量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbps

輸送量比率會根據閘道的總容量而有所不同。 有一點要注意的是，您應該將總容量設定為閘道 VM 可用的 TCP 頻寬。 如果您在閘道上裝載多個 Vm，則必須據以調整閘道的總容量。

此外，如果閘道容量小於可用的通道容量總計，可用的通道容量總計會設定為閘道容量。 例如，如果您將閘道容量設定為 4 Gbps，則 IPsec、L3 和 GRE 的可用容量總計會設定為 4 Gbps，而每個通道的輸送量比率會保持為 1 Gbps。

## <a name="windows-server-2016-behavior"></a>Windows Server 2016 行為

適用于 Windows Server 2016 的閘道容量計算演算法會維持不變。 在 Windows Server 2016 中，最大的 IPsec 通道頻寬限制為（3/20）\*閘道上的閘道容量。 GRE 和 L3 通道的對等比例分別為1/5 和1/2。

如果您要從 Windows Server 2016 升級至 Windows Server 2019：

1.  **GRE 和 L3 通道：** 當網路控制卡節點更新為 Windows Server 2019 之後，Windows Server 2019 配置邏輯就會生效

2.  **IPSec 通道：** Windows Server 2016 閘道配置邏輯會繼續運作，直到閘道集區中的所有閘道都升級至 Windows Server 2019 為止。 針對閘道集區中的所有閘道，您必須將 Azure 閘道服務設定為 [**自動**]。

>[!NOTE]
>升級至 Windows Server 2019 之後，閘道可能會變成過度布建（因為配置邏輯會從 Windows Server 2016 變更為 Windows Server 2019）。 在此情況下，閘道上的現有連線會繼續存在。 閘道的 REST 資源會擲回閘道已過度布建的警告。 在此情況下，您應該將一些連線移到另一個閘道。

---