---
title: 遠端桌面服務的新功能
description: 提供 Windows Server 2016 中 RDS 新功能的描述。
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63711782"
---
# <a name="whats-new-in-remote-desktop-services"></a>遠端桌面服務的新功能

在 Windows Server 2016 上建置的遠端桌面服務 (RDS) 是一個虛擬化平台，可啟用各種不同的客戶案例。 整體 RDS 解決方案的改進包括遠端桌面小組和其他 Microsoft 技術合作夥伴所完成的工作。 以下是 Windows Server 2016 中全新或改進的案例與技術。

此外，請務必查看我們的 Ignite 2016 會議：[充分利用 Windows Server 2016 中的 RDS 改進功能](https://channel9.msdn.com/Events/Ignite/2016/BRK3098) \(英文\)。 在這段影片中，產品小組會檢閱遠端桌面服務中所有全新和改進的功能，包括 vGPU 支援。 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>應用程式相容性 - Windows Server 2016 與 Windows 10
Windows Server 2016 建置 Windows 10 的相同基礎上，不只具有您期望的相同外觀及操作，也可以執行許多相同的應用程式。 Windows Server 2016 搭配圖形功能 (下方) 可讓您為所有使用者提供一個更有生產力的環境。 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database - 適用於高度可用環境的新資料庫
RD 連線代理人可以在共用的 SQL 資料庫 (例如 Azure SQL 資料庫) 中儲存所有部署資訊 (例如連線狀態和使用者/主機對應)。 拋棄 SQL Server Always On 可用性群組部署手冊、抓取 Azure SQL 資料庫的連接字串，然後開始使用高度可用的環境。

其他資訊：[針對遠端桌面連線代理人的高可用性環境使用 Azure SQL DB](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>圖形 - 解決各種案例的圖形需求
歸功於 HYPER-V 不同的裝置指派，您現在可以將主機電腦上的 GPU 直接對應到 VM，以供需要 GPU 的應用程式使用。 RemoteFX vGPU 也進行了改進，包括支援 OpenGL 4.4、OpenCL 1.1、4k 解析度，以及 Windows Server 虛擬機器。

其他資訊：[不同的裝置指派](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>RD 連線代理人 - 改進大量登入期間的連線處理
透過改進的連線處理，RD 連線代理人現在可以處理超過 10,000 個並行登入要求，這個情況有時候會在「大量登入」期間看到。 改進的 RD 連線代理人也可以更快速地將伺服器新增至環境，讓部署維護更簡單。

其他資訊：[提升遠端桌面連線代理人效能](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/) \(英文\)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - 通訊協定內建的新功能
RDP 10 現在使用 H.264/AVC 444 轉碼器，適當地將影片和文字最佳化。 此版本也支援畫筆遠端處理。 透過這些功能，您的遠端工作階段開始感覺更像是本機工作階段。  

其他資訊：[Windows 10 和 Windows Server 2016 中的 RDP 10 AVC/H.264 改進功能](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/) \(英文\)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>個人工作階段桌面 - 為任何使用者提供個別的桌面
個人工作階段桌面是在雲端中裝載您自己個人桌面的新方式。 系統管理權限和專用的工作階段主機可消除裝載環境的複雜度，讓使用者就像是管理自己的桌面一樣。

其他資訊：[個人工作階段桌面](rds-personal-session-desktops.md)
