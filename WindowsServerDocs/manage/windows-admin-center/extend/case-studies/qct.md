---
title: Windows Admin Center SDK 案例研究-QCT
description: Windows Admin Center SDK 案例研究-QCT
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: b0c8ad554103db068defcc4438fd4790c2101ef9
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245369"
---
# <a name="qct-management-suite-extension"></a>QCT 管理套件擴充功能

## <a name="a-simple-path-to-server-infrastructure-management"></a>伺服器基礎結構管理的簡單路徑

適用于 Windows Admin Center 的 QCT Management Suite 擴充功能提供單一窗格，可用來監視系統組態以及管理 [QCT Azure Stack HCI 認證系統](https://go.qct.io/solutions/enterprise-private-cloud/qxstack-windows-server-cloud-ready-appliances/windows-server-software-defined-solution-wssd/) 的伺服器健康狀態： [QuantaGrid D52BQ-2U](https://www.qct.io/product/index/Server/rackmount-server/2U-Rackmount-Server/QuantaGrid-D52BQ-2U)、 [QuantaGrid D52T-1ULH](https://www.qct.io/product/index/Storage/Storage-Server/1U-Storage-Server/QuantaGrid-D52T-1ULH) 和 [QuantaPlex T21P-4u](https://www.qct.io/product/index/Storage/Storage-Server/4U-Storage-Server/QuantaPlex-T21P-4U)。

依客戶的現有監視和管理難題來推動，QCT 提供專有、互補的特色和功能，包括系統事件記錄檔、監視驅動程式和硬體元件健全狀況的總覽，以增強整體管理體驗。

![QCT 延伸模組之 [磁片] 索引標籤的螢幕擷取畫面。](../../media/extend-case-study-qct/D52T_DarkMode_Disk-Detail-General.PNG)

QCT Management Suite 擴充了 Windows Admin Center 的功能與下列主要功能：
- 單鍵 **專用硬體管理**-直覺的使用者介面會顯示硬體資訊，包括型號名稱、處理器、記憶體和 BIOS。 IT 系統管理員可以透過簡單的單鍵 UI 重新開機 BMC。

![QCT 延伸模組單鍵專用硬體管理功能的螢幕擷取畫面。](../../media/extend-case-study-qct/D52T_Overview.PNG)

- **有效服務支援的磁片對應和 LED 識別** -QCT Management SUITE wizard UI 設計會顯示每個所選磁片的位置，其中包含磁片設定檔的總覽，以及所選磁片的 LED 燈控制項以進行有效率的取代。

![QCT 延伸模組之磁片更換功能的螢幕擷取畫面。](../../media/extend-case-study-qct/T21P_disk_mapping.png)

- 適用于硬體事件記錄檔和健康狀態的容易使用監視工具。

![QCT 延伸模組中包含的硬體事件記錄檔和健全狀況狀態監視工具的螢幕擷取畫面。](../../media/extend-case-study-qct/D52T_event_log.PNG)

- **預測磁片管理** -使用的資訊和狀況不良的通知來評估系統狀況，讓組織能夠在總失敗發生前採取動作。

![QCT 延伸模組中包含的資訊和狀況不良通知的螢幕擷取畫面。](../../media/extend-case-study-qct/T21P_SMART.PNG)

深入瞭解適用于 Windows Admin Center 的 QCT Management Suite：
- [QCT Management Suite 網頁](https://go.qct.io/solutions/enterprise-private-cloud/qxstack-windows-server-cloud-ready-appliances/)
- [QCT Management Suite 資料工作表](https://go.qct.io/wp-content/uploads/2019/04/WAC-data-sheet_v04222019.pdf)