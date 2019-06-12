---
title: Windows Server 2019 和 Microsoft Server 應用程式相容性
description: Windows Server 2019 和 Microsoft 伺服器應用程式的相容性資料表
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 8dcaff6ab8a296790158f59035bd4a5c1a093cbd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442377"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Windows Server 2019 和 Microsoft Server 應用程式相容性

>適用於：Windows Server 2019

下表列出支援於視窗 Server 2019 的安裝和功能的 Microsoft 伺服器應用程式。 這項資訊適用於快速參考，並不是要取代個別產品規格、需求、宣告或每個個別伺服器應用程式的一般通訊。 請參閱每項產品的官方文件，完全了解相容性和選項。

如果您是尋找更多有關 Windows Server 與非 Microsoft 應用程式相容性的軟體廠商合作夥伴，請瀏覽[商業應用程式憑證入口網站](https://commercialappcertification.microsoft.com/)。

| **Product**                                                  | **Server Core 上支援**             |   | **在 含有桌面體驗的伺服器上支援** | **發行？** |   | **產品網頁連結**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是           |   | [Exchange Server 系統需求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016 CU3                            | 是                                      |   | 是                                             | 是            |   | [主機整合伺服器系統需求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | [是]\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | [是]\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | [是]\*                                    |   | 是                                             | 是           |   | [硬體和 Software Requirements for Installing SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | [是]\*                                    |   | 是                                             | 是           |   | [硬體和 Software Requirements for Installing SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | [是]\*                                    |   | 是                                             | 是           |   | [硬體和 Software Requirements for Installing SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager （版本 1806年） | 是為受管理的用戶端，無法與站台伺服器 |   | 是為受管理的用戶端，無法與站台伺服器        | 是           |   | [什麼是版本 1806年的 System Center Configuration Manager 的新功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | [是]\*                                    |   | 是                                             | 是           |   | [System Center Operations Manager 的系統需求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | [是]\*                                    |   | 是                                             | 是           |   | [針對 System Center Virtual Machine Manager 的系統需求](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 是           |   | [針對 System Center Data Protection Manager 準備您的環境](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [適用於 SharePoint Server 2016 的硬體和軟體需求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2019 的硬體和軟體需求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2019 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| 2019 商務用 Skype                                      | 否                                       |   | 是                                             | 是           |   | [安裝必要的 skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*可能會有限制，也可能需要[Server Core 應用程式相容性功能上 on Demand (FOD)](install-fod-19.md)。
請參閱特定產品或 FOD 文件。
