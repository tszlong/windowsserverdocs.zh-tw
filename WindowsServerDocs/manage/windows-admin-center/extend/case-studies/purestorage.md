---
title: Windows 管理中心 SDK 案例研究-單純的儲存空間
description: Windows 管理中心 SDK 案例研究-單純的儲存空間
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.openlocfilehash: a79b575397c5dc139200f69a0110ab3c909d5fd5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942700"
---
# <a name="pure-storage-extension"></a>純粹儲存延伸模組

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>為 Windows 管理中心提供端對端陣列管理

[純粹儲存空間](https://www.purestorage.com/)提供企業、所有 flash 的資料儲存體解決方案，可提供以資料為中心的架構，以加速您的業務以獲得競爭優勢。  純粹是 Microsoft 的金級合作夥伴，已通過 Microsoft Windows Server 認證，並為重要的 Microsoft 解決方案（例如 Azure、Hyper-v、SQL Server、System Center、Windows PowerShell 和 Windows SMB）開發技術整合。 純粹最近宣佈了擴充功能的技術預覽，支援最新版本的 Windows 管理中心，提供單一窗格的單純 FlashArray 產品。  在此延伸模組中，使用者可以從一個工具執行監視工作、查看即時效能計量，以及管理存放磁片區與啟動器。

在早期，Windows 系統管理中心稱為「專案檀香山」，純粹發現能夠讓客戶和合作夥伴能夠從 Windows 系統管理中心提供的單一窗格中管理多個單純的儲存 FlashArrays。

當純粹開始以「專案檀香山」來研究使用案例時，他們會立即實現在 Windows 管理中心和 FlashArray 之間提供統一管理體驗的潛能。 純粹與 Windows 管理中心工程小組密切合作，協助定義功能的執行詳細資料。 純粹也能夠在 Windows 管理中心的初期階段提供意見反應，以及對 Microsoft 團隊做貢獻。

![純粹儲存延伸模組](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>「我們整合了模擬 FlashArray web 介面的功能集，可在 Windows 系統管理中心內啟用直接管理。我們的客戶和合作夥伴將受益于單一窗格，而不需要使用兩個不同的管理工具。除了單一管理優勢，客戶還可以內容管理連線到 FlashArray 的 Windows 伺服器。」</cite>
>
> --Barkz，技術總監 Microsoft 解決方案 & 整合，純粹儲存

純粹存放解決方案延伸模組中包含的功能包括：
- 連接到多個 FlashArrays。
- 查看 FlashArray 詳細資料，包括 IOPs、頻寬、延遲、資料縮減和空間管理。 這些都是您從 FlashArray 管理 GUI 取得的相同詳細資料。
- View 已設定的主機群組，用來啟用 Windows Server 主機和叢集共用磁片區的共用磁片區存取 (Csv) 。
- View Hosts —所有的連線資訊都可供使用，包括主機名稱、iSCSI 限定名稱 (Iqn) 和全球名稱 (Wwn) 。
- 管理磁片區—這包括建立和終結磁片區的能力。 一旦磁片區損毀，它就會放在 [已終結的專案] 值區中，而您必須從主要的 FlashArray 管理 GUI 中消除。
- 管理啟動器-這是其中一個最有趣的功能，可為受 Windows 管理中心部署所管理的個別伺服器提供內容。 您可以 (磁片區) 個別的 Windows 伺服器上，查看連線的磁片，檢查是否已安裝/設定了多重路徑 IO (MPIO) ，並建立/裝載新磁片區。

已建立[示範影片](https://youtu.be/IFAeCAd6V2g)，其中會顯示純儲存體解決方案延伸模組所提供的所有功能。

下列螢幕擷取畫面說明如何) 連線到特定 Windows Server 主機的 (磁片區。 除了查看連線的詳細資料之外，我們還會檢查是否已設定多重路徑 IO。

![純粹儲存延伸模組](../../media/extend-case-study-purestorage/purestorage-2.png)

除了查看磁片，您可以建立新的磁片區，並立即將其掛接到主機，而不需要使用 Windows 磁片管理工具。

![純粹儲存延伸模組](../../media/extend-case-study-purestorage/purestorage-3.png)

發行我們的 Technical Preview 之後，到目前為止所收集的客戶意見反應非常正面，同時也提供我們深入瞭解要在未來版本中新增的各種功能。

其他資源：
- [純粹的儲存延伸模組公告 blog 文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
