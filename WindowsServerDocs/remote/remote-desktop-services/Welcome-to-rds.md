---
title: 歡迎使用 Windows Server 2016 中的遠端桌面服務
description: 提供遠端桌面服務的概觀
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: 1fcb3fb7e2989399d908a1b5bed7bd21240efeab
ms.sourcegitcommit: 2ec5e779c00481b13f186e2c56d207007897cfd4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021647"
---
# <a name="welcome-to-remote-desktop-services"></a>歡迎使用遠端桌面服務 

遠端桌面服務 (RDS) 是針對每個終端客戶需求，建置虛擬化解決方案的首選平台，包括提供個別的虛擬化應用程式、提供安全行動和遠端桌面存取，並讓使用者能夠從雲端執行其應用程式和桌面。

![遠端桌面服務概觀](./media/rds-overview.png)

RDS 可提供部署彈性、成本效益以及擴充性，且全部都可以透過各種部署選項，包括 Windows Server 2016 (用於內部部署)、Microsoft Azure (用於雲端部署)，以及一系列強大的合作夥伴解決方案提供。

根據您的環境和喜好設定，您可以為工作階段型虛擬化設定 RDS 解決方案，以作為虛擬桌面基礎結構 (VDI)，或作為兩者的組合：

- **工作階段型虛擬化**：利用 Windows Server 的運算能力，提供符合成本效益的多重工作階段環境，以提升使用者的日常工作負載。
- **VDI**：利用 Windows 用戶端提供使用者對於 Windows 桌面體驗所期望的高效能、應用程式相容性，以及熟悉度。

在這些虛擬化環境中，您可以更靈活地向使用者發佈內容：

- **桌面**：利用您安裝和管理的各種應用程式，為使用者提供完整的桌面體驗。 適用於依賴這些電腦作為其主要工作站的使用者，或來自精簡型用戶端的使用者，例如使用 MultiPoint 服務。
- **RemoteApps**：指定在虛擬化電腦上裝載/執行，但看起來如同本機應用程式般，在使用者桌面上執行的個別應用程式。 應用程式有自己的工作列項目，而且可以調整大小並在監視器之間移動。 適用於在安全的遠端環境中部署及管理重要的應用程式，同時允許使用者在其中工作並自訂自己的桌面。

對於符合成本效益至關重要，而且您想要擴展在工作階段型虛擬化環境中部署完整桌面優點的環境，您可以使用 [MultiPoint 服務](../multipoint-services/multipoint-services.md)來提供最佳的價值。 

您可以使用這些選項和設定，靈活地以遠端、安全，且符合成本效益的方式，部署使用者所需的桌面及應用程式。

## <a name="next-steps"></a>後續步驟

以下是協助您更加了解 RDS，甚至是開始部署自己環境的一些後續步驟：
-   針對各種 Windows 和 Windows Server 版本的 RDS，了解[支援的設定](rds-supported-config.md)
-   [規劃和設計](rds-plan-and-design.md) RDS 環境，以因應各種需求，例如高可用性和多重要素驗證。
-   檢閱最適用於您所需環境的[遠端桌面服務架構模型](desktop-hosting-logical-architecture.md)。
-   開始[使用 ARM 和 Azure Marketplace 部署 RDS 環境](rds-in-azure.md)。
