---
title: Windows Server 上的 Hyper-V
description: 提供關於嘗試、規劃、部署和管理 Hyper-v 的重要文章連結
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 558733e3472578d1df413fe220f1846debc0f358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366839"
---
# <a name="hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-V

>適用於：Windows Server 2016、Windows Server 2019

Windows Server 中的 Hyper-v 角色可讓您建立虛擬化運算環境，您可以在其中建立和管理虛擬機器。 您可以在一部實體電腦上執行多個作業系統，並隔離彼此的作業系統。 利用這項技術，您可以改善運算資源的效率，並釋出硬體資源。

請參閱下表中的主題，以深入瞭解 Windows Server 上的 Hyper-v。

## <a name="hyper-v-resources-for-it-pros"></a>適用于 IT 專業人員的 hyper-v 資源

|工作 |資源|
|---|---|
|![核取記號和檔圖示以顯示符合需求](media/All_Symbols_MeetsRequirements.png)|**評估 Hyper-v**<br /><br />- [Hyper-v 技術總覽](Hyper-V-Technology-Overview.md)<br />- [Windows Server 上的 hyper-v 新功能](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Windows Server 上的 Hyper-v 系統需求](System-requirements-for-Hyper-V-on-Windows.md)<br />- [支援 hyper-v 的 Windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />@no__t 0[支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />[由世代和來賓 @no__t 0 功能相容性](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**規劃 Hyper-v**<br /><br />- [應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [規劃 Windows Server 中的 hyper-v 擴充性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [規劃 Windows Server 中的 hyper-v 網路](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [規劃 Windows Server 中的 hyper-v 安全性](plan/plan-hyper-v-security-in-windows-server.md)|
|![游標和放射環狀圖示](media/All_Symbols_GetStarted.png)|**開始使用 Hyper-v**<br /><br />- [下載並安裝 Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**作為虛擬機器主機的 Windows Server 2019 伺服器核心或 GUI 安裝選項**<br /><br />- [在 Windows Server 上安裝 hyper-v 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [為 hyper-v 虛擬機器建立虛擬交換器](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [在 hyper-v 中建立虛擬機器](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![個人和工具圖示](media/All_Symbols_Administrator.png)|**升級 Hyper-v 主機和虛擬機器**<br /><br />- [升級 Windows Server 叢集節點](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**設定和管理 Hyper-v**<br /><br />- [設定主機進行即時移轉，而不需要容錯移轉](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)叢集<br />- [從遠端系統管理 Nano Server](../../get-started/manage-nano-server.md)<br />- [選擇標準或生產檢查點](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [啟用或停用檢查點](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [設定 Hyper-v 複本](manage/Set-up-Hyper-V-Replica.md)|
|![交談氣泡圖示](media/All_Symbols_Chat.png)|**網路**<br /><br />查看 Microsoft 虛擬化和 Hyper-v 團隊的計畫經理、產品經理、開發人員和測試人員的最新文章。<br /><br />- [虛擬化 Blog](https://blogs.technet.com/b/virtualization/)<br />- [Windows Server Blog](https://blogs.technet.com/b/windowsserver/)<br />- [Ben Armstrong 的虛擬化 Blog](https://blogs.msdn.com/b/virtual_pc_guy/) （已封存）|
|![使用者群組圖示](media/All_Symbols_Users_Group.png)|**論壇和新聞群組**<br /><br />有任何問題嗎？ 與您的對等、Mvp 和 Hyper-v 產品小組交談。<br /><br />- [Windows Server 社區](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Windows Server Hyper-v TechNet 論壇](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>相關技術

下表列出您可能想要在虛擬化計算環境中使用的技術。

|技術|描述|
|--------------|---------------|
|[用戶端 Hyper-v](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Windows 8、Windows 8.1 和 Windows 10 隨附的虛擬化技術，可讓您透過 [**控制台**] 的 [**程式和功能**] 安裝。|
|[容錯移轉叢集](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|為 Hyper-v 主機和虛擬機器提供高可用性的 Windows Server 功能。|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|為虛擬化資料中心提供管理解決方案的 System Center 元件。 您可以設定及管理您的虛擬化主機、網路和存放裝置資源，以便建立虛擬機器和服務，並將其部署到您已建立的私人雲端。|
|[Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/)|使用 Windows Server 和 Hyper-v 容器為開發、測試和生產團隊提供標準化的環境。|