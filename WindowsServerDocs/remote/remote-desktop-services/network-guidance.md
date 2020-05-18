---
title: 網路指導方針
description: 遠端桌面部署的頻寬建議。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/12/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: ba084c58e725627e838c07b5b5b9849d131b2038
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203542"
---
# <a name="network-guidelines"></a>網路指導方針

使用遠端 Windows 工作階段時，您網路的可用頻寬會大幅影響您的體驗品質。 不同的應用程式和顯示器解析度需要不同的網路組態，因此請務必確定您的網路已妥善設定而符合您的需求。

>[!NOTE]
>下列建議適用於遺失率低於 0.1% 的網路。 無論您在虛擬機器 (VM) 上裝載多少工作階段，都適用這些建議。

## <a name="applications"></a>應用程式

下表列出順暢使用者體驗的最低建議頻寬。 這些建議以[遠端桌面工作負載](remote-desktop-workloads.md)中的指導方針為基礎。

| 工作負載類型   | 建議的頻寬 |
|-----------------|-----------------------|
| 輕量型           | 1.5 Mbps              |
| 中          | 3 Mbps                |
| 大量           | 5 Mbps                |
| 電源           | 15 Mbps               |

請記住，網路承受的壓力取決於應用程式工作負載的輸出畫面播放速率和顯示器解析度。 如果畫面播放速率或顯示器解析度增加，頻寬需求也會隨之上升。 例如，使用高解析度顯示器執行輕量工作負載時，所需的可用頻寬會使用一般或低解析度執行輕量工作負載時更高。

其他案例可能會根據您的使用方式而有不同的頻寬需求，例如：

- 語音或視訊會議
- 即時通訊
- 4k 串流視訊

請務必使用 Login VSI 之類的模擬工具，在您的部署中進行這些案例的負載測試。 請會改變負載大小、執行壓力測試，並在遠端工作階段中測試常見的使用者案例，以進一步了解您的網路需求。

## <a name="display-resolutions"></a>顯示器解析度

不同的顯示器解析度需要不同的可用頻寬。 下表列出我們建議的頻寬，這在採用一般顯示器解析度、畫面播放速率 (fps) 為每秒 30 個畫面時，可提供的流暢使用者體驗。 這些建議適用於單一和多使用者案例。 請記住，畫面播放速率低於 30 fps 的案例 (例如讀取靜態文字) 中，所需的可用頻寬較低。

| 30 fps 時的一般顯示器解析度    | 建議的頻寬 |
|------------------------------------------|-----------------------|
| 約 1024 × 768 像素                      | 1.5 Mbps              |
| 約 1280 × 720 像素                      | 3 Mbps                |
| 約 1920 × 1080 像素                     | 5 Mbps                |
| 約 3840 × 2160 像素 (4K)                | 15 Mbps               |

## <a name="additional-resources"></a>其他資源

您所在的 Azure 區域可能會對使用者體驗產生與網路條件同等的影響。 若要深入了解，請參閱 [Windows 虛擬桌面體驗評估工具](https://azure.microsoft.com/services/virtual-desktop/assessment/)。
