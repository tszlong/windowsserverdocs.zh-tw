---
title: Windows Server 2019 和 Microsoft Server 應用程式相容性
description: Windows Server 2019 和 Microsoft Server 應用程式的相容性表格
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 40c90b179321062949af5eea6d92f6238fb9b460
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391995"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Windows Server 2019 和 Microsoft Server 應用程式相容性

>適用於：Windows Server Standard 2012 R2

這個表格列出支援 Window Server 2019 安裝和功能的 Microsoft Server 應用程式。 這項資訊適用於快速參考，並不是要取代個別產品規格、需求、宣告或每個個別伺服器應用程式的一般通訊。 請參閱每項產品的官方文件，完全了解相容性和選項。

如果您是尋找 Windows Server 與非 Microsoft 應用程式相容性詳細資訊的軟體廠商合作夥伴，請瀏覽 [Commercial App Certification 入口網站](https://commercialappcertification.microsoft.com/)。

| **產品**                                                  | **在 Server Core 上受到支援**             |   | **在含有桌面體驗的伺服器上受到支援** | **發行與否？** |   | **產品 Web 連結**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是           |   | [Exchange Server 系統需求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016 CU3                            | 是                                      |   | 是                                             | 是            |   | [Host Integration Server 系統需求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | 是\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | 是\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | 是\*                                    |   | 是                                             | 是           |   | [安裝 SQL Server 2014 的硬體與軟體需求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | 是\*                                    |   | 是                                             | 是           |   | [安裝 SQL Server 2016 的硬體與軟體需求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | 是\*                                    |   | 是                                             | 是           |   | [安裝 SQL Server 2017 的硬體與軟體需求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (1806 版) | 作為受控用戶端則為是，作為站台伺服器則為否 |   | 作為受控用戶端則為是，作為站台伺服器則為否        | 是           |   | [System Center Configuration Manager 1806 版的新功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | 是\*                                    |   | 是                                             | 是           |   | [System Center Operations Manager 的系統需求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | 是\*                                    |   | 是                                             | 是           |   | [System Center Virtual Machine Manager 的系統需求](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 是           |   | [準備 System Center Data Protection Manager 環境](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2016 的硬體與軟體需求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2019 的硬體與軟體需求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2019 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| 商務用 Skype 2019                                      | 否                                       |   | 是                                             | 是           |   | [安裝商務用 Skype Server 的必要條件](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*可能有限制，或可能需要 [Server Core 應用程式相容性功能隨選安裝 (FOD)](install-fod-19.md)。
請參閱特定產品或 FOD 文件。
