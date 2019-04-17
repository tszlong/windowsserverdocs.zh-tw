---
title: Windows Admin Center SDK 案例研究單純的存放裝置
description: Windows Admin Center SDK 案例研究單純的存放裝置
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995323"
---
# 進行單純的存放裝置擴充功能

## 提供 Windows Admin center 的端對端陣列管理 

[進行單純的儲存空間](https://www.purestorage.com/)可以提供企業，提供資料中心的架構，以加速您的業務的競爭優勢的全快閃資料存放裝置解決方案。  純粹的是 Microsoft 金級合作夥伴，適用於 Microsoft Windows Server，認證和開發技術整合的索引鍵的 Microsoft 解決方案，例如 Azure、 HYPER-V、 SQL Server、 System Center、 Windows PowerShell，以及 Windows SMB。 純最近已宣布支援最新版本可讓您進行單純 FlashArray 產品單一窗格檢視的 Windows Admin Center 擴充功能的技術預覽。  此延伸模組，從使用者會從一種工具進行監視工作、 檢視即時效能計量，以及管理儲存空間磁碟區和啟動器賦予。

在初期 Windows Admin Center 稱為 「 Project Honolulu 」，Pure 所見的值不能提供客戶和合作夥伴能夠管理多個單純的儲存體 FlashArrays 從單一窗格 Windows Admin Center 所提供的窗口。

當 Pure 啟動與 「 Project Honolulu 」 的使用案例研究它們立即具現化提供一致的管理體驗 Windows Admin Center 與 FlashArray 之間的可能性。 純貼近彙集與 Windows Admin Center 工程團隊，幫助定義功能的實作詳細資料。 純也能夠在 Windows Admin Center 的早期階段提供意見反應，並進行對 Microsoft 團隊的貢獻。 

![進行單純的存放裝置擴充功能](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>「 我們已整合模擬我們 FlashArray web 介面，以啟用直接管理從 Windows Admin Center 中的功能集。 我們的客戶和合作夥伴可受益於單一窗相較於需要使用這兩個不同的管理工具的窗格中。 除了管理單一存取點權益客戶將能夠清楚管理 Windows 伺服器連線至 FlashArray。 」</cite>
>
> -Barkz、 技術總監 Microsoft 解決方案與整合，單純的存放裝置

進行單純的存放裝置解決方案擴充功能中包含的功能包括：
- 連線至多個 FlashArrays。
- 檢視 FlashArray 詳細資訊，包括 IOPs、 頻寬、 延遲、 資料縮減和空間管理。 這些是您從 FlashArray 管理 GUI 取得的所有相同詳細資料。
- 檢視設定主機群組用於啟用適用於 Windows Server 主機與叢集共用磁碟區 (Csv) 的共用磁碟區的存取權。
- 檢視主機 — 連線資訊的所有可供使用包括主機名稱、 iSCSI 限定名稱 (IQNs) 和世界寬名稱 (WWNs)。
- 管理磁碟區 — 包括建立及摧毀磁碟區的能力。 一旦磁碟區會損毀它將會放在終結項目桶，而且您將需要 Eradicate 從主要 FlashArray 管理 GUI。
- 管理啟動器 — 這是提供情境給受管理的 Windows Admin Center 部署的個別伺服器最有趣的功能之一。 多重路徑 IO (MPIO) 是否安裝/設定和架設建立新的磁碟區，您可以檢視個別的 Windows 伺服器，檢查已連接的磁碟 （磁碟區）。

已建立顯示的所有功能進行單純的存放裝置解決方案擴充功能所提供的[影片示範](https://youtu.be/IFAeCAd6V2g)。 

以下螢幕擷取畫面說明檢視哪些磁碟 （磁碟區） 已連線到特定的 Windows Server 主機。 除了檢視連線詳細資料，我們會檢查是否已設定多重路徑 IO。

![進行單純的存放裝置擴充功能](../../media/extend-case-study-purestorage/purestorage-2.png)

除了檢視磁碟，新的磁碟區可以建立並立即掛接到主機不需使用 Windows 磁碟管理] 工具。

![進行單純的存放裝置擴充功能](../../media/extend-case-study-purestorage/purestorage-3.png)

自從發行我們 Technical Preview，到目前為止所收集的客戶意見反應非常正與已有也提供給我們深入了解不同的功能，以新增在未來的版本。 

其他資源：
- [進行單純的存放裝置擴充功能公告部落格文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
