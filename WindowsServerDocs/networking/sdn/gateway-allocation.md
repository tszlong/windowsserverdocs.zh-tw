---
title: 閘道頻寬配置
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850429"
---
# <a name="gateway-bandwidth-allocation"></a>閘道頻寬配置

>適用於：Windows Server

在 Windows Server 2016 中，個別的通道頻寬 IPsec、 GRE 和 L3 會是總閘道容量的比例。 因此，客戶會提供依照預期這超出閘道 VM 的一般 TCP 頻寬的閘道容量。

此外，在閘道上的最大 IPsec 通道頻寬限於 (3/20)\*客戶所提供的閘道容量。 因此，比方說，如果您設定閘道容量為 100 Mbps 時，則 IPsec 通道容量會 150 Mbps。 GRE 和 L3 通道的對等比例為 1/5 和 1/2，分別。

雖然這任職大多數部署時，固定的比例模型不是適用於高輸送量的環境。 即使當資料傳輸費率相當高 （比方說，高於 40 Gbps），因為內部因素設定上限的 SDN 閘道通道的最大輸送量。

在 Windows Server 2019，通道類型，最大輸送量被固定：

-   IPsec = 5 Gbps，

-   GRE = 15 Gbps

-   L3 = 5 Gbps

因此，即使您的閘道主機 /VM 支援 Nic 與更高的輸送量，被固定的最大的可用通道輸送量。 此程式碼的另一個問題任意過度佈建閘道，為閘道容量提供極高的數字時所發生這種情況。

## <a name="gateway-capacity-calculation"></a>閘道容量計算

在理想情況下，您可以設定閘道的輸送量容量為閘道 VM 可用的輸送量。 因此，比方說，如果您有單一閘道 VM，而基礎主機 NIC 輸送量是 25 Gbps，閘道輸送量可以設以及 25 Gbps。

如果只使用 IPsec 連線的閘道，最大的可用固定的容量是 5 Gbps。 因此，比方說，如果您佈建在閘道上的 IPsec 連線，您只能佈建的彙總頻寬 （連入 + 外寄） 為 5 Gbps。

如果使用 IPsec 與 GRE 連線的閘道，您可以佈建 5 的 IPsec Gbps 輸送量最大值或最大的 15 Gbps 的 GRE 輸送量。 因此，比方說，如果您佈建 2 Gbps 的 IPsec 輸送量，您必須保留佈建閘道或保留的 9 的 GRE Gbps 輸送量的 3 Gbps 的 IPsec 輸送量。

若要將這更數學術語中：

- 閘道的容量總計 = 25 Gbps

- 可用的 IPsec 容量總計 = 5 Gbps （固定）

- 可用的 GRE 容量總計 = （固定） 為 15 Gbps

- 此閘道的 IPsec 輸送量比例 = 25/5 = 5 Gbps

- 此閘道的 GRE 輸送量比例 = 25/15 = 5/3 Gbps

比方說，如果您配置給客戶的 2 Gbps 的 IPsec 輸送量：

在閘道上的可用容量的剩餘 = 閘道 – IPsec 輸送量比例的容量總計 * IPsec 輸送量配置 （已使用的容量）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25–5*2 = 15 Gbps

您可以在閘道配置的剩餘 IPsec 輸送量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

您可以在閘道配置的剩餘 GRE 輸送量 = 閘道/GRE 輸送量比例的剩餘容量 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15*3/5 = 9 Gbps

輸送量比例取決於閘道的總容量。 要注意的一點是，您應該設定的總容量為 TCP 可用的頻寬給閘道 VM。 如果您有多個閘道上裝載的 Vm，您必須據以調整閘道的總容量。

此外，如果閘道容量是總可用通道容量小於，總可用通道容量設為閘道容量。 比方說，如果您設定閘道容量為 4 Gbps 時，IPsec、 L3，以及 GRE 可用總容量設為 4 Gbps，留給 1 Gbps 輸送量比率，每個通道。

## <a name="windows-server-2016-behavior"></a>Windows Server 2016 的行為

閘道容量計算演算法，適用於 Windows Server 2016 會維持不變。 在 Windows Server 2016 中，最大 IPsec 通道頻寬限於 (3/20)\*閘道上的閘道容量。 對等費率計費，GRE 和 L3 通道是 1/5 和 1/2，分別。

如果您從 Windows Server 2016 升級至 Windows Server 2019:

1.  **GRE 和 L3 通道：** Windows Server 2019 配置邏輯網路控制卡節點更新為 Windows Server 2019 之後才會生效

2.  **IPSec 通道：** Windows Server 2016 閘道配置邏輯，會繼續運作直到閘道集區中的所有閘道都升級為 Windows Server 2019。 閘道集區中的所有閘道，您必須將 Azure 閘道服務**自動**。

>[!NOTE]
>可以，升級到 Windows Server 2019 之後，閘道會變成過度佈建 （如配置邏輯會從 Windows Server 2016 變成 Windows Server 2019）。 在此情況下，在閘道上現有的連線繼續存在。 閘道 REST 資源會擲回警告，則閘道會是過度佈建。 在此情況下，您應該將某些連線移到另一個閘道。

---