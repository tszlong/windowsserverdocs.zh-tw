---
title: Hyper-v 技術總覽
description: 說明什麼是 Hyper-v、如何取得、主要功能，以及常見的用法。
manager: dongill
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: kbdazure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 2d69a16dc49c34872d3787338a1fd130aaf7241d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997037"
---
# <a name="hyper-v-technology-overview"></a>Hyper-v 技術總覽

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

Hyper-v 是 Microsoft 的硬體虛擬化產品。 它可讓您建立及執行電腦的軟體版本，稱為「*虛擬機器*」。 每部虛擬機器的作用就像完整的電腦，執行作業系統和程式。 當您需要計算資源時，虛擬機器可提供更大的彈性、協助節省時間和金錢，而且比起在實體硬體上執行一個作業系統，更有效率的方法是使用硬體。

Hyper-v 會在自己的隔離空間中執行每部虛擬機器，這表示您可以同時在相同的硬體上執行多部虛擬機器。 您可能想要這麼做，以避免發生損毀影響其他工作負載的問題，或讓不同的人員、群組或服務存取不同的系統。

## <a name="some-ways-hyper-v-can-help-you"></a>Hyper-v 可協助您的一些方式

Hyper-v 可協助您：

- **建立或擴充私人雲端環境。** 藉由移至或擴充您的共用資源使用方式，並隨著需求的變更來調整使用率，以提供更富彈性、隨選的 IT 服務。

- **更有效率地使用您的硬體。** 將伺服器和工作負載合併到較少、功能更強大的實體電腦，以使用較少的電源和實體空間。

- **提升業務連續性。** 將工作負載的排程和非計畫停機的影響降至最低。

- **建立或擴充虛擬桌面基礎結構 (VDI)。** 透過 VDI 使用集中式桌面策略可協助您提升商業彈性和資料安全性，以及簡化法規合規性並管理桌面作業系統和應用程式。 在相同的伺服器上部署 Hyper-v 和遠端桌面虛擬主機 (RD 虛擬主機) ，讓您的使用者可以使用個人虛擬桌面或虛擬桌面集區。

- **讓開發和測試更有效率。** 重現不同的計算環境，而不需要購買或維護您只使用實體系統時所需的所有硬體。

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-v 和其他虛擬化產品

Windows 和 Windows Server 中的 hyper-v 取代了舊版硬體虛擬化產品，例如 Microsoft Virtual PC、Microsoft Virtual Server 和 Windows Virtual PC。 Hyper-v 提供這些舊產品中未提供的網路功能、效能、儲存體和安全性功能。

Hyper-v 和大部分需要相同處理器功能的協力廠商虛擬化應用程式不相容。 這是因為處理器功能（稱為硬體虛擬化延伸模組）的設計不會共用。 如需詳細資訊，請參閱[虛擬化應用程式無法與 hyper-v、Device Guard 和 Credential Guard 搭配](https://support.microsoft.com/kb/3204980)使用。

## <a name="what-features-does-hyper-v-have"></a>Hyper-v 有哪些功能？

Hyper-v 提供許多功能。 這是一項總覽，依據功能提供的內容或協助您進行的動作來分組。

**計算環境**-hyper-v 虛擬機器包含與實體電腦相同的基本元件，例如記憶體、處理器、儲存體和網路。 所有這些元件都有功能和選項，可讓您設定不同的方式來滿足不同的需求。 儲存體和網路可以視為自己的類別，因為有許多方式可以進行設定。

嚴重損壞**修復與備份**-針對嚴重損壞修復，hyper-v 複本會建立虛擬機器的複本，並將其儲存在另一個實體位置，讓您可以從複本還原虛擬機器。 針對備份，Hyper-v 提供兩種類型。 其中一個使用儲存狀態，另一個使用磁碟區陰影複製服務 (VSS) ，讓您可以針對支援 VSS 的程式進行應用程式一致備份。

**優化**-每個支援的客體作業系統都有一組自訂的服務和驅動程式，稱為*integration services*，可讓您更輕鬆地在 hyper-v 虛擬機器中使用作業系統。

可**移植性**-即時移轉、儲存體遷移和匯入/匯出等功能，讓您更輕鬆地移動或散發虛擬機器。

**遠端**連線-Hyper-v 包含虛擬機器連線，這是一種與 Windows 和 Linux 搭配使用的遠端連線工具。 與遠端桌面不同的是，這項工具可讓您進行主控台存取，因此即使作業系統尚未開機，也可以查看來賓中發生的狀況。

**安全性**安全開機和受防護的虛擬機器有助於防止惡意程式碼和其他未經授權的虛擬機器和其資料存取。

如需此版本中引進之功能的摘要，請參閱[Windows Server 上的 hyper-v 的新](What-s-new-in-Hyper-V-on-Windows.md)功能。 某些功能或部分會限制可設定的數量。 如需詳細資訊，請參閱[在 Windows Server 2016 中規劃 hyper-v 擴充性](./plan/plan-hyper-v-scalability-in-windows-server.md)。

## <a name="how-to-get-hyper-v"></a>如何取得 Hyper-v

Hyper-v 可在 Windows Server 和 Windows 中使用，做為 Windows Server x64 版本可用的伺服器角色。 如需伺服器指示，請參閱[在 Windows server 上安裝 hyper-v 角色](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。 在 Windows 上，它在某些64位版本的 Windows 中是以[功能](/virtualization/hyper-v-on-windows/index)的形式提供。 它也可作為可下載的獨立伺服器產品、 [Microsoft Hyper-v 伺服器](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019)。

## <a name="supported-operating-systems"></a>支援的作業系統

許多作業系統會在虛擬機器上執行。 一般來說，使用 x86 架構的作業系統會在 Hyper-v 虛擬機器上執行。 不過，並非所有可執行檔作業系統都受到 Microsoft 的測試和支援。 如需支援的功能清單，請參閱：

- [Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Windows Server 上的 Hyper-v 支援的 Windows 客體作業系統](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Hyper-v 的運作方式

Hyper-v 是以虛擬機器為基礎的虛擬化技術。 Hyper-v 使用的是 Windows 虛擬機器，它需要具有特定功能的實體處理器。 如需硬體詳細資料，請參閱[Windows Server 上的 Hyper-v 系統需求](System-requirements-for-Hyper-V-on-Windows.md)。

在大部分情況下，管理者會管理硬體與虛擬機器之間的互動。 這個由管理者控制的硬體存取權，讓虛擬機器在其中執行的隔離環境。 在某些設定中，虛擬機器或虛擬機器中執行的作業系統可直接存取圖形、網路或存放裝置硬體。

## <a name="what-does-hyper-v-consist-of"></a>Hyper-v 包含哪些專案？

Hyper-v 具有共同運作的必要元件，讓您可以建立和執行虛擬機器。 這些元件統稱為虛擬化平臺。 當您安裝 Hyper-v 角色時，它們會安裝成一組集合。 所需的部分包括 Windows 虛擬程式、Hyper-v 虛擬機器管理服務、虛擬化 WMI 提供者、虛擬機器匯流排 (VMbus) 、虛擬化服務提供者 (VSP) 和虛擬基礎結構驅動程式 (VID) 。

Hyper-v 也有管理和連線能力的工具。 您可以將這些安裝在 Hyper-v 角色安裝所在的同一部電腦上，以及安裝在未安裝 Hyper-v 角色的電腦上。 這些工具組括：

- Hyper-V 管理員
- [適用于 Windows PowerShell 的 hyper-v 模組](/powershell/module/hyper-v/index)
- [虛擬機器連接](./learn-more/hyper-v-virtual-machine-connect.md) \(有時稱為 VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>相關技術

以下是一些通常與 Hyper-v 搭配使用的 Microsoft 技術：

- [容錯移轉叢集](../../failover-clustering/whats-new-in-failover-clustering.md)
- [遠端桌面服務](../../remote/remote-desktop-services/welcome-to-rds.md)
- [System Center Virtual Machine Manager](/system-center/vmm/overview)

各種存放裝置技術：叢集共用磁片區、SMB 3.0、儲存空間直接存取

Windows 容器提供另一種虛擬化方法。 請參閱 MSDN 上的[Windows 容器](/virtualization/windowscontainers/index)庫。