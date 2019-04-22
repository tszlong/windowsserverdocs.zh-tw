---
title: 在 遠端桌面服務中最新消息
description: 提供的新功能 Windows Server 2016 中的 RDS 的描述。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816309"
---
# <a name="whats-new-in-remote-desktop-services"></a>在 遠端桌面服務中最新消息

遠端桌面服務 (RDS) 建置在 Windows Server 2016 上是啟用各種不同的客戶案例的虛擬化平台。 改善整體的 RDS 方案中加入了遠端桌面小組和其他 microsoft 的技術合作夥伴所完成的工作。 下列案例與技術是新增或改善 Windows Server 2016 中。

也請務必查看我們的工作階段，來自 Ignite 2016:[充分利用 Windows Server 2016 中的 RDS 改進](https://channel9.msdn.com/Events/Ignite/2016/BRK3098)。 在這段影片中，產品小組會檢閱所有全新和改進功能，遠端桌面服務，包括 vGPU 支援。 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>應用程式相容性-Windows Server 2016 和 Windows 10
建置 Windows 10 的相同基礎上，Windows Server 2016 不只具有相同的外觀及操作您覺得桌面，但也可以執行許多相同的應用程式。 Windows Server 2016 搭配 （下方） 的圖形功能可讓您提高產能的所有使用者的環境。 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database-新的資料庫高可用性的環境
RD 連線代理人是能夠將共用的 SQL 資料庫，例如 Azure SQL database 中儲存的所有部署資訊 （例如連線狀態和使用者/主控件對應）。 從 SQL Server Always On 可用性群組部署手冊、 擷取到 Azure SQL database 中，連接字串和開始使用高度可用的環境。

其他資訊：[針對遠端桌面連線代理人高可用性環境使用 Azure SQL DB](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>圖形-解決跨各種案例的圖形的需求
由於 HYPER-V 的不連續的裝置指派，您現在可以直接至 VM，以供其 GPU 需要應用程式主機電腦上對應 Gpu。 也進行了改進的 RemoteFX vGPU，包括支援 OpenGL 4.4，OpenCL 1.1、 4 k 解析度，以及 Windows Server 虛擬機器。

其他資訊：[不連續的裝置指派](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>RD 連線代理人-改善處理期間登入大量的連線
處理改善的連線，RD 連線代理人現在就可以處理超過 10,000 的並行登入要求，在 「 登入風暴 」 期間，有時候會出現。 改善的 RD 連線代理人也會維護部署更簡單，因為能夠更快速地將伺服器新增至環境。

其他資訊：[更佳的遠端桌面連線代理人效能](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10-通訊協定內建的新功能
H.264/AVC 444 轉碼器，影片和文字之間適當地最佳化，現在會使用 RDP 10。 此版本中，也支援畫筆遠端處理。 使用這些功能，您的遠端工作階段啟動感覺更像是本機工作階段。  

其他資訊：[在 Windows 10 和 Windows Server 2016 的 RDP 10 AVC/H.264 增強功能](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>個人的工作階段桌面-提供給任何使用者的個別桌面上
個人的工作階段桌面是在雲端中裝載您自己個人桌面的新方式。 系統管理權限和專用的工作階段主機移除裝載在使用者想要管理像桌面環境的複雜度是其本身。

其他資訊：[個人的工作階段桌面](rds-personal-session-desktops.md)
