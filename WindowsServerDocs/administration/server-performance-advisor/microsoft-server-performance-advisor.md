---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 49f6132cfe99d9d4b719aeeecf149ecb1d7b76f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382989"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下載 Microsoft Server Performance Advisor （SPA）以協助診斷 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 部署中的效能問題。 SPA 會產生完整的診斷報告和圖表，並提供建議，協助您快速分析問題及開發更正動作。

-   [伺服器效能審查程式總覽](#bkmk-aboutspa)

-   [下載 Server Performance Advisor](#bkmk-downloadspa)

-   [Server Performance Advisor 使用者指南](server-performance-advisor-users-guide.md)

-   [Server Performance Advisor 套件開發指南](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>伺服器效能審查程式總覽

伺服器效能建議程式是由兩個元件所組成： SPA 架構和 SPA Advisor 套件。

### <a name="the-server-performance-advisor-framework"></a>Server Performance Advisor 架構

負責收集 advisor 套件所指定資料的引擎，將收集的資料寫入 Microsoft SQL Server 資料庫，建立可供執行 SPA 建議程式套件腳本的 IT 易記環境，並顯示最後的報告。 您只需要在 SPA 主控台上安裝 SPA 架構。 SPA 主控台可以安裝在獨立電腦上，以從遠端存取受測試的伺服器，或安裝在受測試的伺服器上。

### <a name="server-performance-advisor-packs"></a>Server Performance Advisor 套件

SPA Advisor 套件是所有微調規則的中心，其中包含一系列的中繼資料和 SQL 腳本檔案。 SPA 隨附下列 Advisor 套件：

-   核心 OS advisor 套件會分析一般作業系統功能的效能，而不受特定伺服器角色的影響。

-   Internet Information Server （IIS） advisor 套件會追蹤 IIS 的效能。

-   Hyper-v Advisor 套件會分析 Hyper-v 伺服器角色的一般效能。

    **注意**Hyper-v Advisor 套件不會分析客體作業系統。

     

-   Active directory advisor 套件會分析 active directory 角色的一般效能。

SPA 也提供可延伸的模型，供非 Microsoft 開發人員撰寫 advisor 套件以符合其需求。

**注意**SPA 無法瞭解所有硬體和使用者案例內容。 您應該使用工具所提供的建議來協助您做出決策，並瞭解對伺服器進行任何可能變更的結果。

 

## <a href="" id="bkmk-downloadspa"></a>下載 Server Performance Advisor


使用下列連結下載適用于 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的 Server Performance Advisor：

-   [Microsoft Server Performance Advisor 3.1 （32位）](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 （64位）](https://go.microsoft.com/fwlink/p/?linkid=327752)

您可以使用下列命令來解壓縮 CAB 檔案中的檔案：

-   x86 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   若為 x64 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**注意**當您解壓縮 .cab 檔案時，SPA 必須保留階層式目錄結構，才能正常運作。 根據安裝在伺服器上的 CAB 工具而定，解壓縮可能會導致非操作目錄結構。 若要保留階層式目錄結構，您可以使用解壓縮檔案目錄結構的 CAB 解壓縮公用程式工具。

如果 CAB 解壓縮工具已正確解壓縮檔案，子資料夾將會自動出現在 [解壓縮目標] 資料夾中。

### <a name="spa-prerequisites"></a>SPA 必要條件

SPA 主控台需要安裝下列軟體：

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 或 Microsoft SQL Server 2008 R2 SP1

較新版本可能相容。 所有與 SPA 主控台的已知產品不相容都會被注明。
