---
description: 深入瞭解： Microsoft Hyper-V Server 2016
title: Microsoft Hyper-V Server 2016
ms.topic: how-to
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
ms.author: benarm
author: BenjaminArmstrong
ms.date: 07/26/2017
ms.openlocfilehash: a894b4d8bf9055c96f4d391521f1bdf798090e64
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948134"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 是獨立 \- 產品，其中只包含 windows 管理程式、Windows Server 驅動程式模型，以及虛擬化元件。 它提供簡單且可靠的虛擬化解決方案，可協助您改善伺服器的使用，並降低成本。

Microsoft Hyper-V Server 2016 中的 Windows 虛擬程式技術與 \- Windows Server 2016 上的 hyper-v 角色相同。 因此，Windows Server 2016 上的 Hyper-v 角色有許多可用的內容 \- 也適用于 Microsoft Hyper-V Server 2016。

## <a name="hyper-v-server-resources-for-it-pros"></a>\-IT 專業人員適用的 Hyper-v 伺服器資源

|工作|資源|
|-|-|
|![符合需求符號](media/All_Symbols_MeetsRequirements.png)|**評估 Hyper-V**<p>-   [Hyper-v 技術總覽](hyper-v-technology-overview.md)<br />- [Windows Server 2016 上的 Hyper-v 新功能](what-s-new-in-hyper-v-on-windows.md)<br />-   [Windows Server 2016 上的 Hyper-v 系統需求](system-requirements-for-hyper-v-on-windows.md)<br />-   [Hyper-v 支援的 Windows 客體作業系統](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [支援的 Linux 和 FreeBSD 虛擬機器](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [產生和來賓的功能相容性](hyper-v-feature-compatibility-by-generation-and-guest.md)<p>**規劃 Hyper-v**<p>-決定哪一種 [虛擬機器世代](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)  符合您的需求。 <br/>-如果您要移動或匯入虛擬機器，請決定 [升級版本的](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)時機。 <br />- [可 伸縮 性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [網路](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [安全](plan/plan-hyper-v-security-in-windows-server.md)|
|![開始使用符號](media/All_Symbols_GetStarted.png)|**開始使用 Hyper-v 伺服器**<p>[下載並安裝 Microsoft hyper-v \-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)。 這會安裝 Windows 虛擬程式、Windows Server 驅動程式模型，以及虛擬化元件。 這與執行 Windows Server 2016 和 Hyper-v 角色的 Server Core 安裝選項類似 \- 。|
|![管理符號](media/All_Symbols_Administrator.png)|**設定和管理 Hyper-v 伺服器**<p>Hyper-v \- 伺服器沒有圖形化使用者介面 \( GUI \) 。 您可以使用下列工具來設定及管理 Hyper-v \- Server。<p>-   [使用 Sconfig 設定 Windows server 2016 的 Server Core 安裝](../../get-started/sconfig-on-ws2016.md) ，以更新網域或工作組設定、變更 Windows Update 設定、啟用遠端系統管理等等。<br />-針對 Sconfig 中無法使用的命令使用一般 [命令提示](../../administration/windows-commands/windows-commands.md) 字元。<br />-使用 [ [Hyper-v \- 管理員](./manage/remotely-manage-hyper-v-hosts.md) ] 或 [Virtual Machine Manager](/system-center/vmm) 從遠端系統管理 hyper-v \- 伺服器。 若要使用 Hyper-v \- 管理員，請在 Windows 10 或[Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md) [ \- 上安裝 hyper-v 角色](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)。<br />-請參閱 [安裝 Server Core](../../get-started/getting-started-with-server-core.md) ，以取得非 hyper-v 專用之基本伺服器函式的其他管理選項 \- 。 其中所記載的大部分管理方法也適用于 Hyper-v \- Server。<p>**設定和管理 Hyper-v \- 虛擬機器**<p>-   [建立 Hyper-v 虛擬機器的虛擬交換器](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [在 Hyper-v 中建立虛擬機器](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [在標準或生產檢查點之間選擇](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [啟用或停用檢查點](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/manage-windows-virtual-machines-with-powershell-direct.md) <p>**部署**<p>-   [設定主機進行即時移轉，而不需要容錯移轉叢集](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [升級 Windows Server 叢集節點](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [升級虛擬機器版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
