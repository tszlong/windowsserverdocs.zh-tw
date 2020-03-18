---
title: Windows Admin Center 擴充功能
description: Windows Admin Center SDK 擴充功能 (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 90f5b670744b812769164a7a2c70fc673fe4089f
ms.sourcegitcommit: 3cb84bc0bd4be0f9333b7c85cda858c38730cb3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2020
ms.locfileid: "79432447"
---
# <a name="extensions-for-windows-admin-center"></a>Windows Admin Center 擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 建置為可延伸平台，讓合作夥伴和開發人員可以運用 Windows Admin Center 中的現有功能，與其他 IT 系統管理產品和解決方案緊密整合，並且為客戶提供額外的價值。 Windows Admin Center 中的每個解決方案及工具都是使用提供給合作夥伴與開發人員的相同的擴充性功能建置為擴充功能，因此您可以建置強大的工具，就像現今在 Windows Admin Center 中提供的那些工具一樣。

Windows Admin Center 擴充功能是使用新式 Web 技術 (包括 HTML5、CSS、Angular、TypeScript 和 jQuery) 所建置，並且可以透過 PowerShell 或 WMI 處理目標伺服器。 您也可以建置 Windows Admin Center 閘道外掛程式，透過其他通訊協定 (例如 REST) 管理目標伺服器、服務或裝置。

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>為什麼您應該考慮開發 Windows Admin Center 的擴充功能

以下是開發 Windows 管理中心延伸模組時，您可以帶入產品和客戶的價值：

- **與 Windows Admin Center 工具整合：** 將您的產品及服務與 Windows Admin Center 中的伺服器及叢集管理工具整合，並將一致且順暢的端對端監視、管理及疑難排解體驗提供給您的客戶。
- **利用平臺安全性、身分識別和管理功能：** 藉由運用 Windows 管理中心平臺功能，以符合現今 IT 組織的複雜需求，為您的產品和服務啟用 Azure Active Directory （AAD）支援、多重要素驗證、角色型存取控制（RBAC）、記錄、審核。
- **使用最新的 Web 技術進行開發：** 使用新式 Web 技術 (包括 HTML5、CSS、Angular、TypeScript 和 jQuery)，以及 Windows Admin Center SDK 中包含的豐富強大的 UI 控制項，快速建置精彩體驗。
- **擴展產品推廣範圍：** 將業務拓展到我們快速成長的客戶群，成為新興 Windows Admin Center 生態系統的一部分，並趁勢掌握今年稍後時間將推出 Windows Server 2019 的市場動向。

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>使用 Windows Admin Center SDK 開始進行開發

開始使用 Windows 管理中心開發很容易！  如需範例程式碼，請參閱 SDK 檔中的[工具](develop-tool.md)、[解決方案](develop-solution.md)和[閘道外掛程式](develop-gateway-plugin.md)擴充功能類型。 在這裡，您將利用 Windows 管理中心 CLI 來建立新的擴充功能專案，然後遵循個別指南來自訂您的專案，以符合您的需求。

我們提供了 Windows 管理中心[SDK 設計工具](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)組，可協助您使用 windows 管理中心樣式、控制項和頁面範本，在 PowerPoint 中快速模擬延伸模組。 開始撰寫程式碼之前，請先查看您的擴充功能在 Windows 系統管理中心中的樣子。

我們也有裝載于 GitHub 上的範例程式碼：[開發人員工具](https://aka.ms/wacsdk)是包含豐富控制項集合的範例解決方案延伸模組，您可以在自己的延伸模組中流覽和使用。 開發人員工具是功能齊全的、可於開發人員模式側載到 Windows Admin Center 中的擴充功能。

請參閱下列主題以深入了解 SDK 並開始使用：

- [瞭解擴充功能的工作方式](understand-extensions.md)
- [開發擴充功能](developing-extensions.md)
- [指南](guides.md)
- [發佈您的擴充功能](publish-extensions.md)

## <a name="partner-spotlight"></a>合作夥伴焦點

了解我們合作夥伴已開始為 Windows Admin Center 生態系統帶來的驚喜價值，並立即試用這些擴充功能。 深入了解從 Windows Admin Center [安裝擴充功能的方式](../configure/using-extensions.md)。

### <a name="biitops"></a>BiitOps
BiitOps 變更延伸模組會針對 Windows Server 實體/虛擬機器上的硬體、軟體和設定，提供變更追蹤。 BiitOps 變更延伸模組會精確地顯示新功能、已變更的內容，以及在單一窗格中已刪除的內容，以協助追蹤與相容性、可靠性和安全性相關的問題。 [深入瞭解 BiitOps 變更延伸](case-studies/biitops.md)模組。

![BiitOps 延伸模組](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

DataON 必須擴充功能，將監視、管理和端對端深入解析帶入以 Windows Server 為基礎的 DataON 超融合基礎結構和儲存系統。 「必須」延伸模組新增了唯一的值，例如歷程記錄資料包告、磁片對應、系統警示和類似 SAN 的呼叫家用服務、透過順暢的方式來補充 Windows Admin Center 伺服器和超融合的基礎結構管理功能。整合體驗。 [深入了解 DataON 的 MUST 擴充功能及其開發經驗](case-studies/dataon.md)。

![DataON MUST 擴充功能](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

適用于 Windows 系統管理中心的 Fujitsu ServerView 健全狀況和 RAID 健全狀況延伸模組提供了重要硬體元件的深度監視和管理，例如 Fujitsu PRIMERGY 伺服器的處理器、記憶體、電源和存放子系統。 Fujitsu 已利用 Windows Admin Center UX 設計模式及 UI 控制項，帶領我們透過 Windows Admin Center 平台向端對端深入解析伺服器角色與服務、作業系統以及硬體管理的願景邁進了一大步。 [深入了解 Fujitsu 的擴充功能及其開發經驗](case-studies/fujitsu.md)。

![Fujitsu ServerView 擴充功能](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

「聯想 XClarity 整合器延伸模組」透過與 Windows 系統管理中心內的各種體驗緊密整合，使硬體管理更上一層樓。 XClarity 整合器解決方案提供所有您的聯想伺服器的高階觀點，而不同的工具擴充功能則提供硬體詳細資料，不論您是連接到單一伺服器、容錯移轉叢集或超融合叢集。 [深入瞭解聯想 XClarity](case-studies/lenovo.md)整合器擴充功能。

![聯想延伸模組](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

純粹儲存空間提供企業、所有 flash 的資料儲存體解決方案，可提供以資料為中心的架構，以加速您的業務以獲得競爭優勢。 適用于 Windows 系統管理中心的純粹儲存延伸模組，可讓您以單一窗格的模式觀看單純的 FlashArray 產品，並可讓使用者透過單一 UI 來執行監視工作、觀看即時效能計量，以及管理儲存磁片區和啟動器遇到. [深入瞭解純粹的延伸模組及其開發體驗](case-studies/purestorage.md)。

![純粹儲存延伸模組](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

QCT 管理套件延伸模組提供實體伺服器監視和管理功能，可供 QCT Azure Stack HCI 認證系統使用，以補充 Windows 系統管理中心。 QCT 管理套件延伸模組會顯示伺服器硬體資訊，並提供直覺的 wizard UI，協助您有效率地更換實體磁片、硬體事件記錄檔工具和 S.M.A.R.T。 以預測性磁片管理為基礎。 [深入瞭解 QCT 管理套件延伸](case-studies/qct.md)模組。

![QCT 延伸模組](../media/extensibility-overview/qct-extension.png)