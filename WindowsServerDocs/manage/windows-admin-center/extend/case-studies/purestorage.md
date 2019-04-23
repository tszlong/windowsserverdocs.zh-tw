---
title: Windows Admin Center SDK 案例研究-純粹的儲存體
description: Windows Admin Center SDK 案例研究-純粹的儲存體
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849919"
---
# <a name="pure-storage-extension"></a>純的儲存體擴充功能

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>提供 Windows Admin Center 的端對端陣列的管理 

[純的儲存體](https://www.purestorage.com/)提供企業，提供以資料為中心的架構，以加速您的企業競爭優勢的全快閃資料儲存體解決方案。  純粹的是 Microsoft 金級合作夥伴，Microsoft Windows Server、 認證和開發技術的整合，重要的 Microsoft 解決方案 （例如 Azure) 的 HYPER-V、 SQL Server、 System Center、 Windows PowerShell 和 Windows SMB。 純粹最近宣布支援最新版的 Windows Admin Center 提供單一窗格檢視，轉換為純 FlashArray 產品的擴充功能的技術預覽。  從這個延伸模組，使用者就能從某個用來執行監視工作、 檢視即時效能計量，以及管理儲存體磁碟區和啟動器工具。

早 Windows Admin Center 稱為 「 專案檀香山"，純粹所看到的值要能夠提供客戶和合作夥伴能夠管理多個純虛擬的儲存體 FlashArrays Windows Admin Center 提供的半透明效果的單一窗格。

純粹開始研究 」 專案檀香山 」 使用案例時它們立即發現可能會提供統一的管理體驗 Windows Admin Center 與 FlashArray 之間。 純粹緊密共同作業與 Windows Admin Center 工程小組，協助定義功能的實作詳細資料。 純粹也是能夠在初期階段的 Windows Admin Center 提供意見反應，並為 Microsoft 小組做出貢獻。 

![純的儲存體擴充功能](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>「 我們已整合，以模擬我們 FlashArray web 介面，可讓從 Windows Admin Center 內的直接管理一組功能。我們的客戶和合作夥伴將會從與需要使用兩個不同的管理工具的單一窗格中獲益。除了單點管理優點的客戶將能夠內容管理 Windows 伺服器連線到 FlashArray。 」</cite>
>
> -Barkz、 技術總監 Microsoft 解決方案和整合，純粹的儲存體

純的儲存體解決方案延伸模組中包含的功能包括：
- 連接到多個 FlashArrays。
- 檢視 FlashArray 詳細資料，包括 IOPs、 頻寬、 延遲、 減少資料和空間管理。 這些是您從 FlashArray 管理 GUI 取得的所有相同詳細資料。
- 檢視用來啟用 Windows Server 主機和叢集共用磁碟區 (Csv) 的共用磁碟區存取設定的主機群組。
- 檢視主機，所有連線資訊都可包括主機名稱，iSCSI 限定名稱 (Iqn) 和全球名稱 (Wwn)。
- 這包括能夠建立和摧毀磁碟區管理磁碟區 —。 一旦磁碟區損毀它就會放在終結項目貯體，您將需要 Eradicate 從主要的 FlashArray 管理 GUI。
- 管理啟動器 — 這是其中一個最有趣的功能提供給個別的伺服器所管理的 Windows Admin Center 部署的內容。 多重路徑 IO (MPIO) 是否已安裝/設定和建立/掛接新磁碟區，您可以檢視個別的 Windows 伺服器，檢查已連接的磁碟 （磁碟區）。

A[觀看示範影片](https://youtu.be/IFAeCAd6V2g)已建立，會顯示所有純虛擬的儲存體解決方案延伸模組提供的功能。 

以下螢幕擷取畫面說明如何檢視哪些磁碟 （磁碟區） 連接到特定的 Windows Server 主機。 除了檢視連線詳細資料，我們會檢查是否已設定多重路徑 IO。

![純的儲存體擴充功能](../../media/extend-case-study-purestorage/purestorage-2.png)

除了檢視磁碟，可以建立並立即掛接至主機，而不需要使用 Windows 磁碟管理工具的新磁碟區。

![純的儲存體擴充功能](../../media/extend-case-study-purestorage/purestorage-3.png)

因為我們的技術預覽版本，客戶的意見反應收集到目前為止已經非常正面，而且也提供了我們深入了解不同的功能，將在未來的版本。 

其他資源：
- [純的儲存體擴充功能公告部落格文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
