---
title: 在 Windows Server 上的 hyper V
description: 提供有關試用、 規劃、 部署及管理 HYPER-V 的主要文件的連結
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 1a658b611b68d7ecde64bdf0f8a318cc1af2804c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880259"
---
# <a name="hyper-v-on-windows-server"></a>在 Windows Server 上的 hyper V

>適用於：Windows Server 2016、windows Server 2019

HYPER-V 角色，在 Windows Server 可讓您建立的虛擬化的運算環境，您可以在其中建立和管理虛擬機器。 您可以在一部實體電腦上執行多個作業系統，並找出彼此的作業系統。 使用這項技術，您可以改善您的運算資源的效率，並釋出您的硬體資源。

請參閱下表，以深入了解 Windows Server 上的 HYPER-V 中的主題。

## <a name="hyper-v-resources-for-it-pros"></a>適用於 IT 專業人員的 HYPER-V 資源

|工作 |資源|
|---|---|
|![核取記號和文件圖示以顯示符合需求](media/All_Symbols_MeetsRequirements.png)|**評估 HYPER-V**<br /><br />- [HYPER-V 技術概觀](Hyper-V-Technology-Overview.md)<br />- [在 Windows Server 上的 HYPER-V 中最新消息](What-s-new-in-Hyper-V-on-Windows.md)<br />- [適用於 Windows Server 上的 HYPER-V 系統需求](System-requirements-for-Hyper-V-on-Windows.md)<br />- [支援的 Windows 客體作業系統的 HYPER-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- [世代與客體各自的功能相容性](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**適用於 HYPER-V 的計劃**<br /><br />- [應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [規劃 Windows Server 中的 HYPER-V 延展性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [規劃的 Windows Server 中的 HYPER-V 網路功能](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [規劃 Windows Server 中的 HYPER-V 安全性](plan/plan-hyper-v-security-in-windows-server.md)|
|![資料指標和放射環狀圖圖示](media/All_Symbols_GetStarted.png)|**開始使用 HYPER-V**<br /><br />- [下載並安裝 Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**Server Core 或 GUI 安裝選項的 Windows Server 2019 做虛擬機器主機**<br /><br />- [Windows Server 上安裝 HYPER-V 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [建立 HYPER-V 虛擬機器的虛擬交換器](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [在 HYPER-V 中建立虛擬機器](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![人員和工具的圖示](media/All_Symbols_Administrator.png)|**升級 HYPER-V 主機和虛擬機器**<br /><br />- [升級 Windows Server 叢集節點](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**設定及管理 HYPER-V**<br /><br />- [設定即時移轉，而不容錯移轉叢集的主機](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [從遠端管理 Nano Server](../../get-started/manage-nano-server.md)<br />- [選擇 [標準] 或 [生產檢查點](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [啟用或停用檢查點](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [設定 HYPER-V 複本](manage/Set-up-Hyper-V-Replica.md)|
|![交談 （泡泡） 圖示](media/All_Symbols_Chat.png)|**部落格**<br /><br />簽出的最新文章的程式經理、 產品經理、 開發人員和測試人員的 Microsoft 虛擬化和 HYPER-V 的小組中。<br /><br />- [虛擬化部落格](https://blogs.technet.com/b/virtualization/)<br />- [Windows Server 部落格](https://blogs.technet.com/b/windowsserver/)<br />- [Ben Armstrong 的虛擬化部落格](https://blogs.msdn.com/b/virtual_pc_guy/)（已封存）|
|![使用者群組圖示](media/All_Symbols_Users_Group.png)|**論壇和新聞群組**<br /><br />有問題嗎？ 與同業、 Mvp 和 HYPER-V 產品小組。<br /><br />- [Windows Server 社群](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Windows Server HYPER-V 的 TechNet 論壇](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>相關技術

下表列出您可能想要使用虛擬化運算環境中的技術。

|技術|描述|
|--------------|---------------|
|[用戶端 Hyper-v](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|隨附於 Windows 8、 Windows 8.1 和 Windows 10，您可以透過安裝虛擬化技術**程式和功能**中**控制台**。|
|[容錯移轉叢集](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Windows Server 為 HYPER-V 主機和虛擬機器提供高可用性的功能。|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|System Center 元件，提供虛擬化的資料中心的管理解決方案。 您可以設定及管理您的虛擬化主機、 網路和儲存體資源，因此您可以建立並部署到您已建立的私人雲端的虛擬機器和服務。|
|[Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/)|您可以使用 Windows Server 和 HYPER-V 容器提供標準化的環境，適用於開發、 測試和生產環境的小組。|