---
title: Microsoft Hyper-V Server 2016
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: kbdazure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: f5c68f260ff90a07a17a39fdbb881ddcbdb857ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853261"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-v Server 2016 是一種獨立的\-獨立產品，只包含 Windows 虛擬機器、Windows Server 驅動程式模型，以及虛擬化元件。 它提供簡單且可靠的虛擬化解決方案，可協助您改善伺服器使用率並降低成本。

Microsoft Hyper-v Server 2016 中的 Windows 虛擬機器技術與 Windows Server 2016 上的 [Hyper-v\-] 角色相同。 因此，Windows Server 2016 上的「Hyper-v」\-的大部分內容也適用于 Microsoft Hyper-v Server 2016。

## <a name="hyper-v-server-resources-for-it-pros"></a>適用于 IT 專業人員的超\-V 伺服器資源

|工作|資源|
|-|-|
|![符合需求符號](media/All_Symbols_MeetsRequirements.png)|**評估 Hyper-V**<p>-   [Hyper-v 技術總覽](hyper-v-technology-overview.md)<br />- [Windows Server 2016 上的 hyper-v 新功能](what-s-new-in-hyper-v-on-windows.md)<br />[Windows Server 2016 上的 hyper-v -   系統需求](system-requirements-for-hyper-v-on-windows.md)<br />[適用于 hyper-v 的 -   支援的 Windows 客體作業系統](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [支援的 Linux 和 FreeBSD 虛擬機器](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />透過[世代和來賓 -   功能相容性](hyper-v-feature-compatibility-by-generation-and-guest.md)<p>**規劃 Hyper-v**<p>-決定哪一種[虛擬機器產生](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)符合您的需求。 <br/>-如果您要移動或匯入虛擬機器，請決定[升級版本的](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)時機。 <br />- 的擴充[性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [網路](plan/plan-hyper-v-networking-in-windows-server.md)功能 <br />- [安全性](plan/plan-hyper-v-security-in-windows-server.md)|
|![開始使用符號](media/All_Symbols_GetStarted.png)|**開始使用 Hyper-v 伺服器**<p>[下載並安裝 Microsoft hyper-v\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)。 這會安裝 Windows 虛擬機器、Windows Server 驅動程式模型，以及虛擬化元件。 這類似于執行 Windows Server 2016 的 Server Core 安裝選項和 [Hyper-v\-V] 角色。|
|![管理符號](media/All_Symbols_Administrator.png)|**設定和管理 Hyper-v 伺服器**<p>超\-V 伺服器沒有圖形化使用者介面 \(GUI\)。 您可以使用下列工具來設定和管理 Hyper-v\-V 伺服器。<p>-   [使用 Sconfig 設定 Windows server 2016 的 Server Core 安裝](../../get-started/sconfig-on-ws2016.md)，以更新網域或工作組設定、變更 Windows Update 設定、啟用遠端系統管理等等。<br />-針對 Sconfig 中未提供的命令使用一般[命令提示](../../administration/windows-commands/windows-commands.md)字元。<br />-使用 [ [hyper-v\-v 管理員](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management)] 或 [ [Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) ] 從遠端系統管理\-hyper-v 伺服器。 若要使用 [Hyper-v\-V 管理員]，請在 Windows 10 或[Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md)[上安裝 [Hyper-v]\-V 角色](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)。<br />-如需非\-Hyper-v 專用之基本伺服器函式的其他管理選項，請參閱[安裝 Server Core](../../get-started/getting-started-with-server-core.md) 。 其中所記載的大部分管理方法也會與超\-V 伺服器搭配使用。<p>**設定和管理 Hyper-v 虛擬機器\-**<p>-   [為 hyper-v 虛擬機器建立虛擬交換器](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [在 hyper-v 中建立虛擬機器](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [在標準或生產檢查點之間選擇](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [啟用或停用檢查點](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/manage-windows-virtual-machines-with-powershell-direct.md) <p>**部署**<p>-   [設定主機以進行即時移轉，而不需要容錯移轉](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)叢集<br />- [升級 Windows Server 叢集節點](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [升級虛擬機器版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
