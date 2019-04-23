---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839369"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft HYPER-V Server 2016 是獨立\-單獨的產品，其中包含 Windows hypervisor、 Windows Server 驅動程式模型，以及虛擬化元件。 它提供簡單又可靠的虛擬化解決方案，可協助您提高伺服器使用率並降低成本。

Microsoft HYPER-V Server 2016 中的 Windows hypervisor 技術是相同的 Hyper-v 功能\-V 角色 Windows Server 2016 上的。 因此，有許多內容適用於 Hyper-v\-V 角色 Windows Server 2016 上的也適用於 Microsoft HYPER-V Server 2016。

## <a name="hyper-v-server-resources-for-it-pros"></a>超\-V Server IT 專業人員資源

|工作|資源|
|-|-|
|![符合需求的符號](media/All_Symbols_MeetsRequirements.png)|**評估 HYPER-V**<br /><br />-   [HYPER-V 技術概觀](hyper-v-technology-overview.md)<br />- [在 Windows Server 2016 上的 HYPER-V 中最新消息](what-s-new-in-hyper-v-on-windows.md)<br />-   [適用於 Windows Server 2016 上的 HYPER-V 系統需求](system-requirements-for-hyper-v-on-windows.md)<br />-   [支援的 Windows 客體作業系統的 HYPER-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [支援的 Linux 和 FreeBSD 虛擬機器](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [世代與客體各自的功能相容性](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**適用於 HYPER-V 的計劃**<br /><br />-決定哪一種[虛擬機器世代](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)符合您的需求。 <br/>-如果您要移動或匯入虛擬機器，決定何時[版本升級](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。 <br />- [延展性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [網路功能](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [安全性](plan/plan-hyper-v-security-in-windows-server.md)|
|![取得開始使用的符號](media/All_Symbols_GetStarted.png)|**開始使用 HYPER-V Server**<br /><br />[下載並安裝 Microsoft Hyper-v\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)。 這會安裝 Windows hypervisor、 Windows Server 驅動程式模型，以及虛擬化元件。 類似於執行 Server Core 安裝選項的 Windows Server 2016 和 Hyper-v\-V 角色。|
|![管理符號](media/All_Symbols_Administrator.png)|**設定及管理 HYPER-V 伺服器**<br /><br />超\-V 伺服器沒有圖形化使用者介面\(GUI\)。 您可以使用下列工具來設定及管理 Hyper-v\-V Server。<br /><br />-   [以 Sconfig.cmd 設定 Server Core 安裝的 Windows Server 2016](../../get-started/sconfig-on-ws2016.md)更新網域或工作群組設定，變更 Windows 更新的設定、 啟用遠端管理，和更多功能。<br />-使用一般[命令提示字元](../../administration/windows-commands/windows-commands.md)命令在 Sconfig 中無法使用。<br />-使用[Hyper-v\-管理員](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management)或是[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm)遠端管理 Hyper-v\-V Server。 若要使用 Hyper-v\-V 管理員 中，[安裝 Hyper-v\-V 角色在 Windows 10 上的](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)或是[Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md)。<br />-請參閱[安裝 Server Core](../../get-started/getting-started-with-server-core.md)如需非 Hyper-v 特定的基本的伺服器功能的額外的管理選項\-V。 大部分那里記載的管理方法也會使用 Hyper-v\-V Server。<br /><br />**設定及管理 Hyper-v\-V 虛擬機器**<br /><br />-   [建立 HYPER-V 虛擬機器的虛擬交換器](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [在 HYPER-V 中建立虛擬機器](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [選擇 [標準] 或 [生產檢查點](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [啟用或停用檢查點](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**部署**<br /><br />-   [設定即時移轉，而不容錯移轉叢集的主機](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [升級 Windows Server 叢集節點](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [升級虛擬機器版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
