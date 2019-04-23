---
title: Windows Admin Center 擴充功能
description: Windows Admin Center SDK 擴充功能 (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884999"
---
# <a name="extensions-for-windows-admin-center"></a>Windows Admin Center 擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

Windows Admin Center 建置為可延伸平台，讓合作夥伴和開發人員可以運用 Windows Admin Center 中的現有功能，與其他 IT 系統管理產品和解決方案緊密整合，並且為客戶提供額外的價值。 Windows Admin Center 中的每個解決方案及工具都是使用提供給合作夥伴與開發人員的相同的擴充性功能建置為擴充功能，因此您可以建置強大的工具，就像現今在 Windows Admin Center 中提供的那些工具一樣。

Windows Admin Center 擴充功能是使用新式 Web 技術 (包括 HTML5、CSS、Angular、TypeScript 和 jQuery) 所建置，並且可以透過 PowerShell 或 WMI 處理目標伺服器。 您也可以建置 Windows Admin Center 閘道外掛程式，透過其他通訊協定 (例如 REST) 管理目標伺服器、服務或裝置。

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>為什麼您應該考慮開發 Windows Admin Center 的擴充功能

以下是您可以藉由開發 Windows Admin Center 擴充功能，提供給您的產品和客戶的價值：

- **與 Windows Admin Center 工具整合：** 將您的產品和服務整合 Windows Admin Center 中的伺服器和叢集管理工具，並提供一致且無障礙，端對端監視、 管理、 疑難排解體驗，以您的客戶。
- **利用平台安全性、 身分識別和管理功能：** 啟用 Azure Active Directory (AAD) 支援、 多重要素驗證、 角色型存取控制 (RBAC)，記錄，稽核您的產品和服務，利用 Windows Admin Center 平台功能，以符合現今複雜需求IT 組織。
- **使用最新的 web 技術進行開發：** 快速建置令人讚嘆的使用者體驗使用新式 web 技術，包括 HTML5、 CSS、 Angular、 TypeScript 和 jQuery 中，與 Windows Admin Center SDK 中包含的豐富且功能強大的 UI 控制項。
- **擴充產品推廣：** 今年稍後成為新的 Windows Admin Center 生態系統，與我們的快速成長的客戶群和運用 Windows Server 2019 啟動趨勢電子報的延伸範圍的一部分。

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>開始使用 Windows Admin Center SDK 進行開發

開始使用 Windows Admin Center 部署很簡單 ！  範例程式碼可以找到[工具](develop-tool.md)，[解決方案](develop-solution.md)，並[閘道外掛程式](develop-gateway-plugin.md)SDK 文件中的延伸模組類型。 那里您要利用 Windows Admin Center CLI，以建立新的延伸模組專案，然後依照個別的指南，以自訂您的專案，以符合您的需求。

我們做了 Windows Admin Center [SDK 的設計工具組](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)可幫助您快速地模擬在 PowerPoint 中使用 Windows Admin Center 樣式、 控制項和頁面範本的延伸模組。 請參閱您的延伸模組可以如下所示的 Windows Admin Center 之前您開始撰寫程式碼 ！

我們也有裝載於 GitHub 上的範例程式碼：[開發人員工具](https://aka.ms/wacsdk)是範例解決方案的擴充功能，包含控制項，您可以瀏覽，並在您自己的延伸模組中使用的豐富集合。 開發人員工具是功能齊全的、可於開發人員模式側載到 Windows Admin Center 中的擴充功能。

請參閱下列主題以深入了解 SDK 並開始使用：

- [了解擴充功能的運作方式](understand-extensions.md)
- [開發延伸模組](developing-extensions.md)
- [輔助線](guides.md)
- [發行您的延伸模組](publish-extensions.md)

## <a name="partner-spotlight"></a>合作夥伴焦點

了解我們合作夥伴已開始為 Windows Admin Center 生態系統帶來的驚喜價值，並立即試用這些擴充功能。 深入了解從 Windows Admin Center [安裝擴充功能的方式](../configure/using-extensions.md)。

### <a name="dataon"></a>DataON

DataON 的必須延伸模組會將監視、 管理和端對端深入了解 DataON 的超交集基礎結構和儲存體為基礎的系統在 Windows Server 上。 必須延伸模組會將唯一的值，如歷程記錄資料的報告、 磁碟對應、 系統警示和 SAN 類似呼叫主要服務，互補的 Windows Admin Center 伺服器和超交集基礎結構管理功能，透過無縫，一致的體驗。 [深入了解 DataON 的 MUST 擴充功能及其開發經驗](case-studies/dataon.md)。

![DataON MUST 擴充功能](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Fujitsu 的 ServerView 健全狀況和 RAID 健全狀況擴充功能的 Windows Admin Center 提供深入的監視和管理重要的硬體元件，例如處理器、 記憶體、 電源和儲存體子系統 Fujitsu PRIMERGY 伺服器。 Fujitsu 已利用 Windows Admin Center UX 設計模式及 UI 控制項，帶領我們透過 Windows Admin Center 平台向端對端深入解析伺服器角色與服務、作業系統以及硬體管理的願景邁進了一大步。 [深入了解 Fujitsu 的擴充功能及其開發經驗](case-studies/fujitsu.md)。

![Fujitsu ServerView 擴充功能](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Lenovo XClarity 整合器擴充功能會順暢地整合到 Windows Admin Center 內的各種體驗帶到下一個層級的硬體管理。 XClarity 整合器解決方案提供所有 Lenovo 伺服器的高階檢視，不同的工具延伸模組提供硬體詳細資料，無論您連接到單一伺服器、 容錯移轉叢集或超交集叢集。 [深入了解 Lenovo XClarity 整合器延伸模組](case-studies/lenovo.md)。

![Lenovo 延伸模組](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

純的儲存體提供企業，提供以資料為中心的架構，以加速您的企業競爭優勢的全快閃資料儲存體解決方案。 Windows Admin Center 的單純的儲存體擴充功能提供純 FlashArray 產品的單一窗格檢視與可讓使用者進行監視工作、 檢視即時效能計量，以及管理儲存體磁碟區和起始端，透過單一 UI體驗。 [深入了解純粹的擴充功能和自己的開發經驗](case-studies/purestorage.md)。

![純的儲存體擴充功能](../media/extensibility-overview/purestorage-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up 提供以 System Center Operations Manager 為基礎且與 Azure 記錄分析、Application Insights 及其他監視解決方案整合的同等級最佳監控體驗。 [Squared Up 擴充功能](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu)將歷史效能資料和即時應用程式拓撲與相依性帶進 Windows Admin Center 所提供的伺服器及叢集管理內容中，而其可使許多來自相異來源的龐大資料形成單一體驗的實用價值已備受早期採用客戶的讚譽。 [深入了解 Squared Up 的擴充功能及其開發經驗](case-studies/squared-up.md)。

![Squared Up 擴充功能](../media/extensibility-overview/squaredup-extension.png)