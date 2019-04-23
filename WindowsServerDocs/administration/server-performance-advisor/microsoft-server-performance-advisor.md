---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856329"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

下載 Microsoft Server Performance Advisor (SPA) 以協助診斷效能問題，在 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 部署。 SPA 會產生完整的診斷報告和圖表，並提供建議來協助您快速分析問題，以及開發的修正動作。

-   [Server Performance Advisor 概觀](#bkmk-aboutspa)

-   [下載伺服器效能警告](#bkmk-downloadspa)

-   [Server Performance Advisor 使用者指南](server-performance-advisor-users-guide.md)

-   [伺服器效能警告器組件開發指南](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Server Performance Advisor 概觀

Server Performance Advisor 是由兩個部分，SPA 架構和 SPA Advisor 組件所組成。

### <a name="the-server-performance-advisor-framework"></a>Server Performance Advisor framework

負責收集 advisor 組件所指定的資料、 將收集的資料寫入到 Microsoft SQL Server 資料庫、 建立適合 IT 環境的 SPA Advisor 組件，執行指令碼和顯示最終的報告引擎。 您只需要安裝 SPA 架構，SPA 主控台上。 SPA 主控台可以在樹系中安裝遠端存取受測試的伺服器，或安裝在受測試的伺服器上的獨立電腦上。

### <a name="server-performance-advisor-packs"></a>Server Performance Advisor 套件

SPA Advisor 組件是所有的調整規則，一系列的中繼資料和 SQL 指令碼檔案所組成。 SPA 隨附下列 Advisor 組件：

-   Core OS advisor 套件分析效能的一般作業系統功能，獨立的特製化的伺服器角色。

-   Internet Information Server (IIS) advisor 套件 IIS 的效益。

-   HYPER-V Advisor 組件會分析伺服器的 HYPER-V 伺服器角色的整體效能。

    **請注意**HYPER-V Advisor 組件不會分析客體作業系統。

     

-   Active directory advisor 組件會分析 active directory 角色的整體效能。

SPA 也提供可擴充之模型的非 Microsoft 開發人員撰寫 advisor 組件，以符合其需求。

**請注意**SPA 無法了解所有硬體和使用者案例的內容。 您應該使用此工具可協助您做出的決策，並了解任何可能的變更對伺服器的結果所提供的建議。

 

## <a href="" id="bkmk-downloadspa"></a>下載伺服器效能警告


您可以使用下列連結來下載伺服器效能警告器的 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008:

-   [Microsoft Server Performance Advisor 3.1 （32 位元）](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 （64 位元）](https://go.microsoft.com/fwlink/p/?linkid=327752)

您可以使用下列命令擷取的 CAB 檔案中的檔案：

-   適用於 x86 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   適用於 x64 版本： `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**注意：** SPA 時您解壓縮.cab 檔案，必須保留的階層式的目錄結構，才能正確運作。 根據您的伺服器所安裝的封包工具，擷取可能會導致無法運作的目錄結構。 若要保留的階層式的目錄結構，您可以使用封包擷取公用程式工具，它會擷取檔案的目錄結構。

如果封包擷取工具正確解壓縮檔案，則子資料夾會自動顯示在擷取目標資料夾中。

### <a name="spa-prerequisites"></a>SPA 的必要條件

SPA 主控台需要安裝下列軟體：

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 or Microsoft SQL Server 2008 R2 SP1

較新版本可能相容。 使用 SPA 主控台的任何已知的產品不相容狀況會註明。
