---
title: 遠端桌面服務 - 迎合不同類型的使用者
description: 說明不同類型的 RDS 使用者。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 11/06/2019
manager: scottman
ms.openlocfilehash: dc83d76dbfaded9dadae7c16ae9e1c8df7958a7d
ms.sourcegitcommit: d25e47a54a120b9bc26ff43597518fca58c1cc5b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73753826"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>遠端桌面服務 - 迎合不同類型的使用者

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

遠端桌面服務支援不同類型的工作負載。 根據各類使用者的預期需求調整您的部署。

## <a name="types-of-users"></a>使用者類型

### <a name="knowledge-user"></a>[知識] 使用者

知識使用者使用輕量型生產力應用程式，例如 Microsoft Word、Excel、Outlook 和 Microsoft Edge 瀏覽器。

對於知識使用者，我們建議每個虛擬 CPU (vCPU) 不要超過四位以上的使用者。

### <a name="professional-user"></a>[專業] 使用者

專業使用者除了支援更密集的工作負載 (例如開發軟體和建立多媒體內容) 之外，還可以使用網際網路瀏覽器和生產力應用程式。

對於專業使用者，我們建議每個 vCPU 不要超過兩位以上的使用者。

### <a name="power-user"></a>[進階] 使用者

進階使用者可以使用工程和圖形應用程式，例如電腦輔助設計 (CAD) 和 Adobe Photoshop 等。 對於經常使用圖形密集型程式進行影片轉譯、3D 設計和模擬的使用者而言，GPU 通常是不錯的選擇。

對於進階使用者，我們建議每個 vCPU 不要超過一位以上的使用者。

如需深入了解圖形加速，請參閱[選擇您的圖形轉譯技術](rds-graphics-virtualization.md)。

Azure 具有其他圖形加速部署選項和多個可用的 GPU VM 大小。 深入了解 [GPU 最佳化虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu)。

## <a name="test-workload"></a>測試工作負載

我們建議您同時進行壓力測試和實際使用情況模擬來負載測試部署。 您可以使用模擬工具來負載測試您的部署。 請確保系統的回應能力和彈性足以滿足使用者需求，並記得改變負載大小以避免發生意外。
