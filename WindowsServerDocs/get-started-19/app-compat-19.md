---
title: Windows Server 2019 和 Microsoft Server 應用程式相容性
description: Windows Server 2019 和 Microsoft server 應用程式的相容性表格
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
ms.openlocfilehash: e8bb8cda003ca151216e3b86d5884dfd05e73e6d
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256941"
---
# Windows Server 2019 和 Microsoft Server 應用程式相容性

>適用於︰Windows Server 2019

此表格列出支援 Window Server 2019 的安裝和功能的 Microsoft server 應用程式。 這項資訊適用於快速參考，並不是要取代個別產品規格、需求、宣告或每個個別伺服器應用程式的一般通訊。 請參閱每項產品的官方文件，完全了解相容性和選項。

如果您是軟體廠商合作夥伴，尋找 Windows Server 與非 Microsoft 應用程式相容性的詳細資訊，請瀏覽[商業應用程式認證入口網站](https://commercialappcertification.microsoft.com/)。
| **產品**                                                  | **支援在 Server Core**             |   | **具備桌面體驗的伺服器上支援** | **發行與否？** |   | **產品網頁連結**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是           |   | [Exchange Server 系統需求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016，CU3                            | 是                                      |   | 是                                             | 否            |   | [主機 Integration Server 系統需求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Yes\ *                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Yes\ *                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Yes\ *                                    |   | 是                                             | 是           |   | [安裝 SQL Server 2014 的硬體與軟體需求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Yes\ *                                    |   | 是                                             | 是           |   | [硬體與軟體需求安裝 SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Yes\ *                                    |   | 是                                             | 是           |   | [硬體與軟體需求安裝 SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager （版本 1806年） | 是做為受管理的用戶端，不做為站台伺服器 |   | 是做為受管理的用戶端，不做為站台伺服器        | 是           |   | [版本 1806 System Center Configuration Manager 中的新功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Yes\ *                                    |   | 是                                             | 是           |   | [System Center Operations Manager 的系統需求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Yes\ *                                    |   | 是                                             | 是           |   | [系統需求適用於 System Center 虛擬機器 Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 是           |   | [針對 System Center Data Protection Manager 準備您的環境](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2016 的硬體與軟體需求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2019 的硬體與軟體需求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [專案 Server 2019 的軟體需求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| 2019 商務用 Skype                                      | 否                                       |   | 是                                             | 是           |   | [安裝適用於 Business Server Skype 的先決條件](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*May 有限制，或可能需要[Server Core 應用程式相容性功能上隨選安裝 (FOD)。 ](install-fod-19.md)
請參閱特定產品或 FOD 文件。
