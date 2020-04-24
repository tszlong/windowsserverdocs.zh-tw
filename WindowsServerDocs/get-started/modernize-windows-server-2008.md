---
title: 升級 Windows Server 2008 與 Windows Server 2008 R2
description: Windows Server 2008 與 2008 R2 即將終止服務。 了解如何升級內部部署或重新裝載至 Azure。
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 3d2c55430a78eaabfe55b764275c6e61fa80368a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826211"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>升級 Windows Server 2008 與 Windows Server 2008 R2

Windows Server 2008 和 Windows Server 2008 R2 的延伸支援即將於 2020 年 1 月 14 結束。 有兩個現代化路徑：內部部署升級或藉由在 Azure 中重新裝載來進行移轉。 **如果您在 Azure 中重新裝載，您可以免費移轉您現有的 Server 映像。**

![說明從 Windows Server 2008 升級路徑的流程圖](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>內部部署升級
如果您想要將伺服器保留在內部部署，且您執行的是 Windows Server 2008 或 Windows Server 2008 R2，則您必須先[升級為 Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2)，才能[升級為 Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016)。 當您升級時，您仍可選擇透過重新裝載的方式移轉至 Azure。

請參閱[從 Windows Server 2008 R2 或 Windows Server 2008 升級](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008)，以了解內部部署升級選項的詳細資訊。

如果您執行的是 Windows Server 2003，您必須[升級為 Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)) (英文)。 請參閱 [Windows Server 2008 升級路徑](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) (英文)，以了解內部部署升級選項的詳細資訊。


## <a name="migrate-to-azure"></a>移轉至 Azure
您可以將內部部署的 Windows Server 2008 與 Windows Server 2008 R2 伺服器移轉至 Azure，在此您可以繼續在虛擬機器上執行這些伺服器。 在 Azure 中，您將持續符合規範、變得更安全，並在您的工作中新增雲端創意。 移轉至 Azure 的優點包括：

- Azure 中的安全性更新。
- 取得超過三年的 Windows Server 2008 R2 或 2008 重大和重要安全性更新 (免費隨附)。 
- Azure 中的免費升級。
- 當您準備好時採用更多的雲端服務。
- 透過將 SQL Server 移轉至 Azure 受管理執行個體或 VM，您可以得到超過三年的 Windows Server 2008 R2 或 2008 重大安全性更新 (免費隨附)。 
- 運用現有的 SQL Server 與 Windows Server 授權來達到只有 Azure 才有的雲端節省。

[![開始使用特製化映像來遷移至 Azure](./media/WS08-image-banner-small.png)](uploading-specialized-WS08-image-to-azure.md)

若要開始移轉，請參閱[將 Windows Server 2008/2008 R2 專用映像上傳至 Azure](uploading-specialized-WS08-image-to-azure.md)。

為了幫助您了解如何分析現有 IT 資源、評估您擁有的項目，以及瞭解將特定服務與應用程式移至雲端、將工作負載保留在內部部署，或升級為最新版 Windows Server 的優點，請參閱 [Windows Server 移轉手冊](https://go.microsoft.com/fwlink/?linkid=872689) (英文)。

## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>與 Windows Server 平行升級 SQL Server 2008/2008 R2

![SQL Server 標誌](media/sqlr2.jpg)

如果您執行的是 SQL Server 2008/2008 R2，您可以升級為 SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) 或 [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017)。


## <a name="additional-resources"></a>其他資源
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)