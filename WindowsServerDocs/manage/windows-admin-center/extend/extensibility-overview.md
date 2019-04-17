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
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001840"
---
# Windows Admin Center 擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 建置為可延伸平台，讓合作夥伴和開發人員可以運用 Windows Admin Center 中的現有功能，與其他 IT 系統管理產品和解決方案緊密整合，並且為客戶提供額外的價值。 Windows Admin Center 中的每個解決方案及工具都是使用提供給合作夥伴與開發人員的相同的擴充性功能建置為擴充功能，因此您可以建置強大的工具，就像現今在 Windows Admin Center 中提供的那些工具一樣。

Windows Admin Center 擴充功能是使用新式 Web 技術 (包括 HTML5、CSS、Angular、TypeScript 和 jQuery) 所建置，並且可以透過 PowerShell 或 WMI 處理目標伺服器。 您也可以建置 Windows Admin Center 閘道外掛程式，透過其他通訊協定 (例如 REST) 管理目標伺服器、服務或裝置。

## 為什麼您應該考慮開發 Windows Admin Center 的擴充功能

以下是您可以藉由開發 Windows Admin Center 擴充功能，提供給您的產品和客戶的價值：

- **與 Windows Admin Center 工具整合：** 將您的產品及服務與 Windows Admin Center 中的伺服器及叢集管理工具整合，並將一致且順暢的端對端監視、管理及疑難排解體驗提供給您的客戶。
- **運用平台安全性、身分識別和管理功能：** 利用 Windows Admin Center 平台功能滿足現今 IT 組織的複雜需求，為您的產品和服務啟用 Azure Active Directory (AAD) 支援、多重要素驗證、角色型存取控制 (RBAC) 記錄、稽核等功能。
- **使用最新的 Web 技術進行開發：** 使用新式 Web 技術 (包括 HTML5、CSS、Angular、TypeScript 和 jQuery)，以及 Windows Admin Center SDK 中包含的豐富強大的 UI 控制項，快速建置精彩體驗。
- **擴展產品推廣範圍：** 將業務拓展到我們快速成長的客戶群，成為新興 Windows Admin Center 生態系統的一部分，並趁勢掌握今年稍後時間將推出 Windows Server 2019 的市場動向。

## 開始使用 Windows Admin Center SDK 開發

開始使用 Windows Admin Center 開發是簡單的 ！  範例程式碼可以找到我們 SDK 的文件中的[工具](develop-tool.md)、[解決方案](develop-solution.md)和[閘道外掛程式](develop-gateway-plugin.md)擴充功能類型。 那里您將會利用 Windows Admin Center CLI 建置新的延伸模組專案，然後依照來自訂您的專案，以符合您需求的個別的指南。

我們已將 Windows Admin Center [SDK 設計工具組](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)可協助您快速模擬 PowerPoint 中的擴充功能使用 Windows Admin Center 樣式、 控制項和網頁範本。 請參閱您的擴充功能可以看起來像 Windows Admin Center 中開始撰寫程式碼之前 ！

我們也有裝載在 GitHub 上的範例程式碼：[開發人員工具](https://aka.ms/wacsdk)是包含豐富集合的控制項，您可以瀏覽，並使用您自己的延伸模組中的範例解決方案擴充功能。 開發人員工具是功能齊全的、可於開發人員模式側載到 Windows Admin Center 中的擴充功能。

請參閱下列主題以深入了解 SDK 並開始使用：

- [了解擴充功能的運作方式](understand-extensions.md)
- [開發擴充功能](developing-extensions.md)
- [指南](guides.md)
- [發佈您的擴充功能](publish-extensions.md)

## 合作夥伴焦點

了解我們合作夥伴已開始為 Windows Admin Center 生態系統帶來的驚喜價值，並立即試用這些擴充功能。 深入了解從 Windows Admin Center [安裝擴充功能的方式](../configure/using-extensions.md)。

### DataON

DataON 的 MUST 擴充功能帶來監視、 管理和端對端深入了解 DataON 的超融合式基礎結構和儲存體系統以 Windows Server 為基礎。 MUST 擴充功能會新增例如歷程記錄資料報告、 磁碟對應、 系統警示及 SAN 類似通報服務，Windows Admin Center 伺服器及超融合式基礎結構管理功能，透過順暢且，可補充的唯一值一致的體驗。 [深入了解 DataON 的 MUST 擴充功能及其開發經驗](case-studies/dataon.md)。

![DataON MUST 擴充功能](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

適用於 Windows Admin Center 的 Fujitsu 的 ServerView Health 和 RAID Health 擴充功能提供對 Fujitsu PRIMERGY 伺服器重要硬體元件 (例如處理器、記憶體、電源及儲存空間子系統) 的深入監控與管理。 Fujitsu 已利用 Windows Admin Center UX 設計模式及 UI 控制項，帶領我們透過 Windows Admin Center 平台向端對端深入解析伺服器角色與服務、作業系統以及硬體管理的願景邁進了一大步。 [深入了解 Fujitsu 的擴充功能及其開發經驗](case-studies/fujitsu.md)。

![Fujitsu ServerView 擴充功能](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Lenovo 的 XClarity 整合商延伸模組會順暢地整合到 Windows Admin Center 中的各種體驗帶到下一個關卡的硬體管理。 XClarity 整合商方案提供高階檢視您的 Lenovo 部伺服器，並不同的工具擴充功能提供硬體的詳細資訊，是否您已連線到單一伺服器、 容錯移轉叢集或超融合式叢集。 [深入了解有關 Lenovo XClarity 整合商擴充功能](case-studies/lenovo.md)。

![Lenovo 擴充功能](../media/extensibility-overview/lenovo-extension.png)

### 進行單純的存放裝置

進行單純的儲存空間可以提供企業，提供資料中心的架構，以加速您的業務的競爭優勢的全快閃資料存放裝置解決方案。 Windows Admin Center 的純存放裝置擴充功能可讓您進行單純 FlashArray 產品單一窗格檢視和來提供使用者進行監視工作、 檢視即時績效衡量指標，並管理儲存空間磁碟區和透過單一 UI 啟動器體驗。 [深入了解 Pure 的擴充功能及其開發經驗](case-studies/purestorage.md)。

![進行單純的存放裝置擴充功能](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

Squared Up 提供以 System Center Operations Manager 為基礎且與 Azure 記錄分析、Application Insights 及其他監視解決方案整合的同等級最佳監控體驗。 [Squared Up 擴充功能](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu)將歷史效能資料和即時應用程式拓撲與相依性帶進 Windows Admin Center 所提供的伺服器及叢集管理內容中，而其可使許多來自相異來源的龐大資料形成單一體驗的實用價值已備受早期採用客戶的讚譽。 [深入了解 Squared Up 的擴充功能及其開發經驗](case-studies/squared-up.md)。

![Squared Up 擴充功能](../media/extensibility-overview/squaredup-extension.png)