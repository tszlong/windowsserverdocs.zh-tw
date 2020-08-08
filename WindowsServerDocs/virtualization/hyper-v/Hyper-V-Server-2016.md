---
title: Microsoft Hyper-V Server 2016
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: kbdazure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: e99750caea4948f51f5f660f3b6f4b42766e833d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997066"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-v Server 2016 是一種獨立 \- 產品，只包含 windows 虛擬機器、Windows Server 驅動程式模型和虛擬化元件。 它提供簡單且可靠的虛擬化解決方案，可協助您改善伺服器使用率並降低成本。

Microsoft Hyper-v Server 2016 中的 Windows 虛擬機器技術與 \- Windows Server 2016 上的 hyper-v 角色相同。 因此，Windows Server 2016 上的 Hyper-v 角色所提供的大部分內容 \- 也適用于 Microsoft Hyper-v Server 2016。

## <a name="hyper-v-server-resources-for-it-pros"></a>\-適用于 IT 專業人員的 Hyper-v 伺服器資源

|工作|資源|
|-|-|
|![符合需求符號](media/All_Symbols_MeetsRequirements.png)|**評估 Hyper-V**<p>-   [Hyper-v 技術總覽](hyper-v-technology-overview.md)<br />- [Windows Server 2016 上的 Hyper-v 新功能](what-s-new-in-hyper-v-on-windows.md)<br />-   [Windows Server 2016 上的 Hyper-v 系統需求](system-requirements-for-hyper-v-on-windows.md)<br />-   [Hyper-v 支援的 Windows 客體作業系統](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [支援的 Linux 和 FreeBSD 虛擬機器](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [層代和來賓的功能相容性](hyper-v-feature-compatibility-by-generation-and-guest.md)<p>**規劃 Hyper-v**<p>-決定哪一種[虛擬機器產生](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)符合您的需求。 <br/>-如果您要移動或匯入虛擬機器，請決定[升級版本的](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)時機。 <br />- [延展性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [連](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [安全級](plan/plan-hyper-v-security-in-windows-server.md)|
|![開始使用符號](media/All_Symbols_GetStarted.png)|**開始使用 Hyper-v 伺服器**<p>[下載並安裝 Microsoft hyper-v \-V 伺服器 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)。 這會安裝 Windows 虛擬機器、Windows Server 驅動程式模型，以及虛擬化元件。 這類似于執行 Windows Server 2016 和 Hyper-v 角色的 Server Core 安裝選項 \- 。|
|![管理符號](media/All_Symbols_Administrator.png)|**設定和管理 Hyper-v 伺服器**<p>Hyper-v \- 伺服器沒有圖形化使用者介面 \( GUI \) 。 您可以使用下列工具來設定和管理 Hyper-v \- 伺服器。<p>-   [使用 Sconfig 設定 Windows server 2016 的 Server Core 安裝](../../get-started/sconfig-on-ws2016.md)，以更新網域或工作組設定、變更 Windows Update 設定、啟用遠端系統管理等等。<br />-針對 Sconfig 中未提供的命令使用一般[命令提示](../../administration/windows-commands/windows-commands.md)字元。<br />-使用[Hyper-v \- 管理員](./manage/remotely-manage-hyper-v-hosts.md)或[Virtual Machine Manager](/system-center/vmm)從遠端系統管理 hyper-v \- 伺服器。 若要使用 Hyper-v \- 管理員，請[ \- 在 Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)或[Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md)上安裝 hyper-v 角色。<br />-如需非 Hyper-v 專用之基本伺服器函式的其他管理選項，請參閱[安裝 Server Core](../../get-started/getting-started-with-server-core.md) \- 。 其中所記載的大部分管理方法也適用于 Hyper-v \- 伺服器。<p>**設定和管理 Hyper-v \- 虛擬機器**<p>-   [為 Hyper-v 虛擬機器建立虛擬交換器](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [在 Hyper-v 中建立虛擬機器](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [選擇標準或生產檢查點](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [啟用或停用檢查點](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/manage-windows-virtual-machines-with-powershell-direct.md) <p>**部署**<p>-   [設定主機以進行即時移轉而不需要容錯移轉叢集](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [升級 Windows Server 叢集節點](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [升級虛擬機器版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|