---
title: Windows Admin Center SDK 案例研究-純儲存體
description: Windows Admin Center SDK 案例研究-純儲存體
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81185af0bbe7bde894ccb71875497285f51531e3
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245427"
---
# <a name="pure-storage-extension"></a>純儲存體擴充功能

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>提供 Windows Admin Center 的端對端陣列管理

[純儲存體](https://www.purestorage.com/) 提供企業的全快閃資料儲存體解決方案，可提供以資料為中心的架構，以加速您的企業獲得競爭優勢。  單純是 Microsoft 的金級合作夥伴，經過 Microsoft Windows Server 認證，並為 Azure、Hyper-v、SQL Server、System Center、Windows PowerShell 和 Windows SMB 等重要 Microsoft 解決方案開發技術整合。 單純地宣佈技術預覽版支援最新版本的 Windows Admin Center，可讓您在純 FlashArray 產品中提供單一窗格的觀點。  從這個延伸模組，使用者可從一個工具進行授權，以執行監視工作、查看即時效能計量，以及管理存放磁片區和啟動器。

在早期，當 Windows Admin Center 稱為「專案 Honolulu」時，單純看到能夠提供客戶和合作夥伴從 Windows Admin Center 提供的單一窗格管理多個純儲存體 FlashArrays 的能力。

單純開始研究使用案例與「Project Honolulu」時，它們會立即發現在 Windows Admin Center 與 FlashArray 之間提供統一的管理體驗的潛力。 純粹與 Windows Admin Center 工程團隊密切合作，協助定義功能的執行詳細資料。 單純也能夠在 Windows Admin Center 的初期階段提供意見反應，並對 Microsoft 小組提出貢獻。

![純儲存體擴充功能的 [容量] 頁面螢幕擷取畫面。](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>「我們已整合模擬 FlashArray web 介面的功能集，以便從 Windows Admin Center 內進行直接管理。我們的客戶和合作夥伴將受益于單一窗格，而不需要使用兩個不同的管理工具。除了單一管理優點之外，客戶還可以內容管理連線到 FlashArray 的 Windows 伺服器。」</cite>
>
> --Barkz，技術總監 Microsoft 解決方案 & 整合，純儲存體

純儲存體解決方案延伸模組中包含的功能包括：
- 連接到多個 FlashArrays。
- 查看 FlashArray 詳細資料，包括 IOPs、頻寬、延遲、資料縮減和空間管理。 這些都是您從 FlashArray Management GUI 取得的相同詳細資料。
- 查看已設定的主機群組，用來啟用 Windows Server 主機和叢集共用磁片區的共用磁片區存取 (Csv) 。
- View Hosts-所有的連線資訊都可供使用，包括主機名稱、iSCSI 限定名稱 (Iqn) 和全球名稱 (Wwn) 。
- 管理磁片區，包括建立和終結磁片區的能力。 一旦磁片區損毀，它就會放在 [終結的專案] 值區中，您必須從主要的 FlashArray 管理 GUI 中消除。
- 管理啟動器-這是最有趣的功能之一，提供內容給 Windows Admin Center 部署所管理的個別伺服器。 您可以 (磁片區) 的磁片區查看連接的磁片，檢查是否已安裝/設定多重路徑 IO (MPIO) ，以及建立/掛接新磁片區。

已建立 [示範影片](https://youtu.be/IFAeCAd6V2g) ，其中顯示純儲存體解決方案延伸模組所提供的所有功能。

下列螢幕擷取畫面說明如何) 連接到特定 Windows Server 主機的磁片 (磁片區。 除了查看連線詳細資料，我們也會檢查是否已設定多重路徑 IO。

![螢幕擷取畫面，顯示哪些磁片已連接到特定的 Windows Server 主機。](../../media/extend-case-study-purestorage/purestorage-2.png)

除了查看磁片之外，還可以建立新的磁片區，並立即將其掛接至主機，而不需要使用 Windows 磁片管理工具。

![螢幕擷取畫面，顯示如何建立新的磁片區，並使用純儲存體主機立即將其立即掛接至主機。](../../media/extend-case-study-purestorage/purestorage-3.png)

由於發行我們的技術預覽，到目前為止所收集到的客戶意見反應非常正面，也提供我們深入解析，以在未來版本中新增的不同功能。

其他資源：
- [純粹儲存延伸模組公告的 blog 文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) 播客
