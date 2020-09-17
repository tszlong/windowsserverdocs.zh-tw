---
title: Hyper-v 技術總覽
description: 描述何謂 Hyper-v、如何取得它、主要功能，以及常見用途。
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
ms.author: benarm
author: BenjaminArmstrong
ms.date: 11/29/2016
ms.openlocfilehash: 65c3a56f2ffa9266ad2312a32c1c2d712951ec34
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746203"
---
# <a name="hyper-v-technology-overview"></a>Hyper-v 技術總覽

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

Hyper-v 是 Microsoft 的硬體虛擬化產品。 它可讓您建立及執行電腦的軟體版本，稱為「 *虛擬機器*」。 每部虛擬機器的運作方式就像是完整的電腦，執行作業系統和程式。 當您需要運算資源時，虛擬機器可提供您更多的彈性、節省時間與金錢，並且是使用硬體比在實體硬體上執行一個作業系統更有效率的方式。

Hyper-v 會在各自隔離的空間中執行每部虛擬機器，這表示您可以同時在相同硬體上執行一部以上的虛擬機器。 您可能會想要這樣做，以避免發生損毀的問題，例如影響其他工作負載的損毀，或讓不同的人員、群組或服務存取不同的系統。

## <a name="some-ways-hyper-v-can-help-you"></a>Hyper-v 可協助您的一些方式

Hyper-v 可協助您：

- **建立或擴充私人雲端環境。** 藉由移至或擴大共用資源的使用方式，並隨著需求變更來調整使用率，提供更具彈性的隨選 IT 服務。

- **更有效率地使用硬體。** 將伺服器和工作負載合併到較少、功能更強大的實體電腦，以使用較少的電源和實體空間。

- **提升業務連續性。** 將工作負載的排程和非排程停機的影響降至最低。

- **建立或擴充虛擬桌面基礎結構 (VDI)。** 使用集中式桌面策略搭配 VDI，可協助您提升企業的靈活性和資料安全性，並簡化法規合規性及管理桌面作業系統和應用程式。 在相同的伺服器上部署 Hyper-v 和遠端桌面虛擬化主機 (RD 虛擬化主機) ，讓您的使用者可以使用個人虛擬桌面或虛擬桌面集區。

- **更有效率地進行開發和測試。** 重現不同的計算環境，而不需要購買或維護您只使用實體系統時所需的所有硬體。

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-v 和其他虛擬化產品

Windows 和 Windows Server 中的 hyper-v 取代了舊版的硬體虛擬化產品，例如 Microsoft Virtual PC、Microsoft Virtual Server 和 Windows Virtual PC。 Hyper-v 提供這些舊版產品中未提供的網路功能、效能、儲存和安全性功能。

Hyper-v 和大部分需要相同處理器功能的協力廠商虛擬化應用程式都不相容。 這是因為處理器功能（稱為硬體虛擬化延伸模組）設計為不共用。 如需詳細資訊，請參閱 [虛擬化應用程式無法搭配 hyper-v、Device guard 和 Credential Guard 一起](https://support.microsoft.com/kb/3204980)使用。

## <a name="what-features-does-hyper-v-have"></a>Hyper-v 有哪些功能？

Hyper-v 提供許多功能。 這是依功能提供或協助您進行分組的總覽。

運算**環境**-hyper-v 虛擬機器包含與實體電腦相同的基本元件，例如記憶體、處理器、存放裝置和網路功能。 所有這些元件都有功能和選項，可讓您設定不同的方式來滿足不同的需求。 由於您有許多方式可以進行設定，因此每個儲存體和網路都可以被視為各自的類別。

嚴重損壞**修復和備份**-為了進行嚴重損壞修復，hyper-v 複本會建立要儲存在另一個實體位置的虛擬機器複本，讓您可以從複本還原虛擬機器。 針對備份，Hyper-v 提供兩種類型。 其中一個使用已儲存的狀態，而另一個使用磁碟區陰影複製服務 (VSS) 讓您可以針對支援 VSS 的程式進行應用程式一致的備份。

**優化** -每個支援的客體作業系統都有一組自訂的服務和驅動程式（稱為 *integration services*），可讓您更輕鬆地在 hyper-v 虛擬機器中使用作業系統。

可**移植性**-即時移轉、儲存體遷移和匯入/匯出等功能可讓您更輕鬆地移動或散發虛擬機器。

**遠端** 連線-hyper-v 包含虛擬機器連線，也就是一種與 Windows 和 Linux 搭配使用的遠端連線工具。 與遠端桌面不同的是，這項工具可讓您進行主控台存取，因此即使作業系統尚未開機，您還是可以看到來賓發生什麼事。

**安全性** 安全開機和受防護的虛擬機器有助於防止惡意程式碼和其他未經授權的虛擬機器及其資料存取。

如需此版本中引進之功能的摘要，請參閱 [Windows Server 上的 hyper-v 新](What-s-new-in-Hyper-V-on-Windows.md)功能。 某些功能或部分會限制可設定的數量。 如需詳細資訊，請參閱 [規劃 Windows Server 2016 中的 hyper-v 擴充性](./plan/plan-hyper-v-scalability-in-windows-server.md)。

## <a name="how-to-get-hyper-v"></a>如何取得 Hyper-v

Hyper-v 可在 Windows Server 和 Windows 中使用，做為適用于 x64 版本 Windows Server 的伺服器角色。 如需伺服器的指示，請參閱 [在 Windows server 上安裝 hyper-v 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。 在 Windows 中，它在某些64位版本的 Windows 中是以 [功能](/virtualization/hyper-v-on-windows/index) 的形式提供。 它也可作為可下載的獨立伺服器產品， [Microsoft Hyper-V server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019)。

## <a name="supported-operating-systems"></a>支援的作業系統

許多作業系統會在虛擬機器上執行。 一般情況下，使用 x86 架構的作業系統將會在 Hyper-v 虛擬機器上執行。 但是，並非所有可執行檔作業系統都經過 Microsoft 的測試和支援。 如需支援的功能清單，請參閱：

- [Windows 上適用于 Hyper-v 的支援 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Windows Server 上的 Hyper-v 支援的 Windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Hyper-v 的運作方式

Hyper-v 是以虛擬機器為基礎的虛擬化技術。 Hyper-v 使用的是 Windows 虛擬程式，它需要具有特定功能的實體處理器。 如需硬體詳細資料，請參閱 [Windows Server 上的 hyper-v 的系統需求](System-requirements-for-Hyper-V-on-Windows.md)。

在大多數情況下，管理程式會管理硬體與虛擬機器之間的互動。 此虛擬機器控制的硬體存取權可為虛擬機器提供執行所在的隔離環境。 在某些設定中，虛擬機器或虛擬機器中執行的作業系統可直接存取圖形、網路或存放裝置硬體。

## <a name="what-does-hyper-v-consist-of"></a>Hyper-v 包含什麼？

Hyper-v 具有共同運作的必要元件，因此您可以建立及執行虛擬機器。 這些元件統稱為虛擬化平臺。 當您安裝 Hyper-v 角色時，它們會安裝為組。 必要的部分包括 Windows 虛擬程式、Hyper-v 虛擬機器管理服務、虛擬化 WMI 提供者、虛擬機器匯流排 (VMbus) 、虛擬化服務提供者 (.VSP) 和虛擬基礎結構驅動程式 (VID) 。

Hyper-v 也有管理和連線能力的工具。 您可以在安裝 Hyper-v 角色的同一部電腦上，以及未安裝 Hyper-v 角色的電腦上安裝這些元件。 這些工具組括：

- Hyper-V 管理員
- [Windows PowerShell 的 hyper-v 模組](/powershell/module/hyper-v/index)
- [虛擬機器連線](./learn-more/hyper-v-virtual-machine-connect.md) \(有時稱為 VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>相關技術

這些是 Microsoft 的一些經常與 Hyper-v 搭配使用的技術：

- [容錯移轉叢集](../../failover-clustering/whats-new-in-failover-clustering.md)
- [遠端桌面服務](../../remote/remote-desktop-services/welcome-to-rds.md)
- [System Center Virtual Machine Manager](/system-center/vmm/overview)

各種儲存技術：叢集共用磁片區、SMB 3.0、儲存空間直接存取

Windows 容器提供另一種虛擬化方法。 請參閱 MSDN 上的 [Windows 容器](/virtualization/windowscontainers/index) 庫。