---
title: Windows Server 2016 中的歡迎使用遠端桌面服務
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
ms.openlocfilehash: cd00f92254f9e55f83442f5e68e344e0aa7579a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855499"
---
# <a name="welcome-to-remote-desktop-services"></a>歡迎使用遠端桌面服務 

遠端桌面服務 (RDS) 是建置虛擬化解決方案，針對每個終端客戶需求，包括提供個別的虛擬化應用程式，提供安全行動和遠端桌面的存取，以及提供使用者所選擇的平台若要從雲端執行其應用程式和桌面的能力。

![遠端桌面服務概觀](.\media\rds-overview.png)

RDS 提供部署彈性，成本效率和擴充性 — 透過不同部署選項，包括 Windows Server 2016 內部部署、 雲端部署的 Microsoft Azure 和協力廠商的強固陣列的所有已傳遞解決方案。

根據您的環境和喜好設定，您可以設定工作階段為基礎的虛擬化的 RDS 解決方案為虛擬桌面基礎結構 (VDI)，或兩者的組合：

- **工作階段為基礎的虛擬化**:利用 Windows Server 的計算能力，提供符合成本效益的多重工作階段環境，來驅動您的使用者的日常工作負載
- **VDI**:利用 Windows 用戶端，以提供高效能和應用程式相容性，也都是您的使用者的 Windows 桌面體驗的熟悉度。

在這些虛擬化環境中，您會有額外的彈性，在您發佈給使用者：

- **桌上型電腦**:讓使用者使用各種不同的應用程式，您安裝和管理完整桌面體驗。 適用於依賴這些電腦為其主要的工作站，或隨附精簡型用戶端，例如使用 MultiPoint 服務使用者。
- **Remoteapp**:指定裝載/執行虛擬機器上的個別應用程式，但出現如同它們就像是本機應用程式的使用者的桌面上正在執行。 應用程式有自己的工作列項目和可調整大小，並在監視器之間移動。 適用於部署及管理安全的遠端環境中的重要應用程式，同時允許使用者從工作及自訂他們的電腦。

對於其中符合成本效益很重要，而且您想要部署完整的桌面工作階段為基礎的虛擬化環境中的優點延伸的環境，您可以使用[MultiPoint 服務](../multipoint-services/multipoint-services.md)以提供最佳的價值。 

使用這些選項和組態，您可以彈性地部署桌面及應用程式，您的使用者必須在遠端，安全，並符合成本效益的方式。

## <a name="next-steps"></a>後續步驟

以下是幫助您更加了解的 RDS 和即使在開始部署您自己的環境的一些後續步驟：
-   了解[支援的組態](rds-supported-config.md)適用於各種 Windows 和 Windows Server 版本的 RDS
-   [規劃和設計](rds-plan-and-design.md)RDS 環境，以因應各種需求，例如高可用性和多重要素驗證。
-   檢閱[遠端桌面服務架構模型](desktop-hosting-logical-architecture.md)，最適用於您所需的環境。
-   若要開始[部署 RDS 環境 ARM 與 Azure Marketplace](rds-in-azure.md)。
