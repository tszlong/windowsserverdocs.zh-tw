---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.topic: article
ms.openlocfilehash: bd359e71cfb48ecd8aab24a8538369622dd1d271
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627708"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下載 Microsoft Server Performance Advisor (SPA) ，以協助診斷 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 部署中的效能問題。 SPA 會產生全方位的診斷報表和圖表，並提供建議以協助您快速分析問題及開發矯正措施。

-   [Server Performance Advisor 總覽](#bkmk-aboutspa)

-   [下載 Server Performance Advisor](#bkmk-downloadspa)

-   [Server Performance Advisor 使用者指南](server-performance-advisor-users-guide.md)

-   [Server Performance Advisor 套件開發指南](server-performance-advisor-pack-development-guide.md)

## <a name="overview-of-server-performance-advisor"></a><a href="" id="bkmk-aboutspa"></a>Server Performance Advisor 總覽

伺服器效能建議程式是由兩個部分所組成： SPA 架構和 SPA Advisor 套件。

### <a name="the-server-performance-advisor-framework"></a>Server Performance Advisor 架構

負責收集 advisor 套件所指定之資料的引擎，將收集的資料寫入 Microsoft SQL Server 資料庫中，建立一個方便使用的環境來執行 SPA Advisor 套件的腳本，並顯示最終報表。 您只需要在 SPA 主控台上安裝 SPA 架構。 SPA 主控台可安裝在獨立電腦上，以從遠端存取受測試的伺服器，或安裝在受測試的伺服器上。

### <a name="server-performance-advisor-packs"></a>Server Performance Advisor 套件

SPA Advisor 套件是所有調整規則的中心，其中包含一連串的中繼資料和 SQL 腳本檔案。 SPA 隨附下列 Advisor 套件：

-   Core OS advisor 套件會分析一般作業系統功能的效能，而不受特殊伺服器角色的影響。

-   Internet Information Server (IIS) advisor 套件會追蹤 IIS 的效能。

-   Hyper-v Advisor 套件會分析 Hyper-v 伺服器角色的一般效能。

    **注意** Hyper-v Advisor 套件不會分析客體作業系統。



-   Active directory advisor 套件會分析 active directory 角色的一般效能。

SPA 也為非 Microsoft 開發人員提供可延伸的模型，以撰寫 advisor 套件以符合其需求。

**注意** SPA 無法瞭解所有硬體和使用者案例內容。 您應使用工具提供的建議，協助您進行決策，並瞭解對伺服器所做之任何可能變更的結果。



## <a name="download-server-performance-advisor"></a><a href="" id="bkmk-downloadspa"></a>下載 Server Performance Advisor


您可以使用下列連結，下載適用于 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的 Server Performance Advisor：

-   [Microsoft Server Performance Advisor 3.1 (32 位) ](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 (64 位) ](https://go.microsoft.com/fwlink/p/?linkid=327752)

您可以使用下列命令，在 CAB 檔案中解壓縮檔案：

-   x86 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   針對 x64 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**注意** 當您解壓縮 .cab 檔案時，SPA 必須保留階層式目錄結構才能正確運作。 視您伺服器上安裝的 CAB 工具而定，解壓縮可能會導致非操作目錄結構。 若要保留階層式目錄結構，您可以使用可解壓縮檔案目錄結構的 CAB 解壓縮公用程式工具。

如果 CAB 解壓縮工具已正確解壓縮檔案，子資料夾會自動出現在 [解壓縮目標] 資料夾中。

### <a name="spa-prerequisites"></a>SPA 必要條件

SPA 主控台需要安裝下列軟體：

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 或 Microsoft SQL Server 2008 R2 SP1

較新的版本可能會相容。 所有與 SPA 主控台的已知產品不相容。
