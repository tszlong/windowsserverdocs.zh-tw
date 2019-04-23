---
title: HYPER-V 技術概觀
description: 描述 HYPER-V 的是，如何取得、 重要功能，以及常見的用法。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 9ae3c9dce36ad7d67a19ce167c9cb875b3c91810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864309"
---
# <a name="hyper-v-technology-overview"></a>HYPER-V 技術概觀

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

HYPER-V 是 Microsoft 硬體虛擬化產品。 它可讓您建立和執行軟體版本的電腦，稱為*虛擬機器*。 每個虛擬機器就像是完整的電腦，執行的作業系統和程式。 當您需要運算資源時，虛擬機器讓您更大的彈性、 節省時間與金錢，而且更有效率的方式，使用比只在實體硬體上執行一種作業系統的硬體。

HYPER-V 會在它自己的隔離空間，這表示您可以在相同硬體上執行多個虛擬機器，同時執行每個虛擬機器。 您可能想要這麼做以避免問題，例如會影響其他工作負載，損毀，或讓不同系統的不同人員、 群組或服務的存取。

## <a name="some-ways-hyper-v-can-help-you"></a>某些方面 HYPER-V 可協助您

HYPER-V 可以幫助您：

- **建立或擴充私人雲端環境。** 透過移至或擴充使用共用的資源提供更有彈性、 依需求的 IT 服務，並調整使用率，視需求變更。

- **更有效地使用您的硬體。** 將伺服器和工作負載較少、 更強大的實體電腦，可以使用較少的電源和實體空間合併。

- **提升商務持續性。** 您的工作負載的排程和非排程停機時間的影響降到最低。

- **建立或擴充虛擬桌面基礎結構 (VDI)。** 使用 vdi 的集中式桌面策略可以協助您提升業務靈活度和資料安全性，以及簡化法規相符性與管理桌面作業系統和應用程式。 若要讓您的使用者提供個人虛擬桌面或虛擬桌面集區相同的伺服器上部署 HYPER-V 和遠端桌面虛擬主機 （RD 虛擬主機）。

- **可讓您開發和測試更有效率。** 重現不同的運算環境，而不必購買或維護需要如果您只使用實體系統的所有硬體。

## <a name="hyper-v-and-other-virtualization-products"></a>HYPER-V 和其他虛擬化產品

Windows 和 Windows Server 中的 HYPER-V 會取代較舊的硬體虛擬化產品，例如 Microsoft Virtual PC、 Microsoft Virtual Server 和 Windows Virtual PC。 HYPER-V 提供這些較舊的產品中無法使用的網路功能、 效能、 儲存體和安全性功能。

HYPER-V 並需要相同的處理器功能的大部分協力廠商虛擬化應用程式不相容。 這是因為處理器功能，稱為硬體虛擬化延伸模組，專為不共用。 如需詳細資訊，請參閱 <<c0> [ 虛擬化應用程式無法運作搭配 HYPER-V、 Device Guard 和 Credential Guard](https://support.microsoft.com/kb/3204980)。

## <a name="what-features-does-hyper-v-have"></a>HYPER-V 有哪些功能？

HYPER-V 提供許多功能。 這是依功能的功能提供或協助您執行的概觀。

**運算環境**-HYPER-V 虛擬機器包含為實體電腦，例如記憶體、 處理器、 儲存體和網路功能相同的基本組件。 所有這些組件有功能和選項，您可以設定不同的方式，以滿足不同需求。 儲存體和網路可以每個被視為自己的類別，因為許多的方式設定。

**災害復原與備份**-針對災害復原，HYPER-V 複本會建立要儲存在另一個實體位置，因此您可以從複本還原虛擬機器的虛擬機器的複本。 備份中，HYPER-V 會提供兩種類型。 其中一個使用儲存的狀態和其他使用磁碟區陰影複製服務 (VSS)，讓您可以將支援 VSS 之程式的應用程式一致備份

**最佳化**-每個支援的客體作業系統都有一組自訂的服務和驅動程式，稱為*integration services*，，請使用作業系統的 HYPER-V 虛擬機器中的工作變得更容易。

**可攜性**-功能，例如即時移轉、 存放裝置移轉，並匯入/匯出讓您更輕鬆地移動，或將虛擬機器。

**遠端連線**-Hyper V 包括虛擬機器連線，適用於 Windows 和 Linux 的遠端連線工具。 與遠端桌面主控台存取權，此工具可讓不同的是您所見的作業系統不尚未啟動，即使發生客體中。

**安全性**-安全開機和受防護的虛擬機器協助防範惡意程式碼和其他未經授權的存取權的虛擬機器和其資料。

如需此版本中引進的功能的摘要，請參閱 <<c0> [ 的 Windows Server 上的 HYPER-V 新功能](What-s-new-in-Hyper-V-on-Windows.md)。 有些功能或組件有多少可以設定限制。 如需詳細資訊，請參閱 <<c0> [ 規劃 Windows Server 2016 中的 HYPER-V 延展性](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。

## <a name="how-to-get-hyper-v"></a>如何取得 HYPER-V

HYPER-V 是適用於 Windows Server 和 Windows，做為伺服器角色適用於 x64 的 Windows Server 版本。 伺服器的指示，請參閱[Windows Server 上安裝 HYPER-V 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。 在 Windows，它是以提供[功能](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)一些 64 位元版本的 Windows 中。 它也是可下載，獨立的伺服器產品，以提供[Microsoft HYPER-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019)。

## <a name="supported-operating-systems"></a>支援的作業系統

許多作業系統會在虛擬機器上執行。 一般情況下，作業系統，會使用的 x86 架構將 HYPER-V 虛擬機器執行。 測試及 Microsoft 支援，不過並非所有可執行的作業系統。 如需支援的功能清單，請參閱：

- [Windows 上 HYPER-V 的受支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [支援的 Windows 客體作業系統的 Windows Server 上的 HYPER-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>HYPER-V 的運作方式

HYPER-V 是以 hypervisor 為基礎的虛擬化技術。 HYPER-V 會使用 Windows hypervisor，這需要一個實體處理器有特定的功能。 硬體的詳細資訊，請參閱 <<c0> [ 適用於 Windows Server 上的 HYPER-V 系統需求](System-requirements-for-Hyper-V-on-Windows.md)。

在大部分情況下，hypervisor 會管理硬體與虛擬機器之間的互動。 這個 hypervisor 控制存取的硬體可讓虛擬機器執行所在的隔離的環境。 在某些組態中，虛擬機器或虛擬機器中執行的作業系統有直接存取圖形、 網路或儲存體硬體。

## <a name="what-does-hyper-v-consist-of"></a>功能沒有 HYPER-V 組成？

HYPER-V 需要一起運作，因此您可以建立並執行虛擬機器的部分。 在一起，這些組件稱為虛擬化平台。 當您安裝 HYPER-V 角色，他們會安裝成一組。 必要的組件包含 Windows hypervisor、 HYPER-V 虛擬機器管理服務、 虛擬化 WMI 提供者、 虛擬機器匯流排 (VMbus)、 虛擬化服務提供者 (VSP) 和虛擬基礎結構驅動程式 (VID)。

HYPER-V 也有適用於管理和連線工具。 您可以將它們安裝 HYPER-V 角色已安裝的和電腦上安裝了 HYPER-V 角色不在相同電腦上。 這些工具包括：

- Hyper-V 管理員
- [適用於 Windows PowerShell 的 HYPER-V 模組](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [虛擬機器連線](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect)\(有時也稱為 VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>相關技術

這些是 Microsoft 一些技術，通常會搭配 Hyper-v:

- [容錯移轉叢集](../../failover-clustering/whats-new-in-failover-clustering.md)
- [遠端桌面服務](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

各種不同的儲存體技術： 叢集共用磁碟區，SMB 3.0 中，儲存空間直接存取

Windows 容器，提供虛擬化的另一種方法。 請參閱[Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/index)MSDN 上的程式庫。
