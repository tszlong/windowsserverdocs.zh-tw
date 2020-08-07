---
title: DFS 命名空間概觀
ms.author: jgerend
manager: daveba
ms.topic: article
author: jasongerend
ms.date: 06/07/2019
description: 本主題說明 DFS 命名空間，這是 Windows Server 中的角色服務，可讓您將位於不同伺服器上的共用資料夾，分組成一個或多個邏輯結構命名空間。
ms.openlocfilehash: 54f26a605c15ab683dbe51f768e82bce2c00a290
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936188"
---
# <a name="dfs-namespaces-overview"></a>DFS 命名空間概觀

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server (半年通道)

DFS 命名空間是 Windows Server 中的角色服務，可讓您將位於不同伺服器上的共用資料夾，分組成一個或多個邏輯結構命名空間。 因此可以為使用者提供共用資料夾的虛擬檢視，其中單一路徑即可通向位於多部伺服器上的檔案，如下圖所示：

![DFS 命名空間技術項目](media/dfs-overview.png)

以下說明構成 DFS 命名空間的元素：

- **命名空間伺服器** - 命名空間伺服器會裝載一個命名空間。 命名空間伺服器可以是成員伺服器或網域控制站。
- **命名空間根目錄** - 命名空間根目錄是命名空間的起始點。 在上圖中，根的名稱是公用的，而命名空間路徑是 \\ \\ Contoso \\ Public。 這類命名空間稱為網域型命名空間，因為其開頭為網域名稱 (例如 Contoso)，而其中繼資料存放在 Active Directory 網域服務 (AD DS) 中。 雖然上圖顯示的是單一命名空間伺服器，但網域型命名空間還是可以裝載於多部命名空間伺服器來提高命名空間的可用性。
- **資料夾** - 沒有資料夾目標的資料夾會將結構和階層新增至命名空間，而有資料夾目標的資料夾則會提供實際內容給使用者。 當使用者瀏覽命名空間中有資料夾目標的資料夾時，用戶端電腦會收到明確地將用戶端電腦重新導向至其中一個資料夾目標的轉介。
- **資料夾目標** - 資料夾目標是共用資料夾的 UNC 路徑或其他與命名空間中資料夾相關聯的命名空間。 資料夾目標是儲存資料及內容所在的位置。 在上圖中，名為 Tools 的資料夾有兩個資料夾目標，一個在倫敦，另一個在紐約，而名為 Training Guides 的資料夾有一個在紐約的單一資料夾目標。 流覽 \\ \\ Contoso \\ 公開軟體工具的使用者 \\ \\ ，會以透明方式重新導向至共用資料夾 \\ \\ LDN-SVR-01 \\ tools 或 \\ \\ NYC-SVR-01 \\ tools，視使用者目前所在的網站而定。

本主題討論如何安裝 DFS、新功能，以及尋找評估和部署資訊的位置。

您可以使用 DFS Management、[Windows PowerShell 中的 DFS 命名空間 (DFSN) Cmdlet](/powershell/module/dfsn/?view=win10-ps)、**DfsUtil** 命令或是呼叫 WMI 的指令碼來管理命名空間。

## <a name="server-requirements-and-limits"></a>伺服器需求與限制

執行 DFS 管理或使用 DFS 命名空間不需要額外的硬體或軟體。

命名空間伺服器是裝載命名空間的網域控制站或成員伺服器。 您可在伺服器上裝載的命名空間數目取決於命名空間伺服器上執行的作業系統。

執行下列作業系統的伺服器除了單一獨立命名空間之外，還可以裝載多個網域型命名空間。

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter 和 Enterprise Edition
- Windows Server (半年通道)

執行下列作業系統的伺服器可以裝載單一獨立命名空間：

- Windows Server 2008 R2 Standard

下表說明選擇裝載命名空間的伺服器時所要考量的其他因素。

| 裝載獨立命名空間的伺服器 | 裝載網域型命名空間的伺服器 |
| ---                                   |        ---                                |
| 必須包含 NTFS 磁碟區才能裝載命名空間。|必須包含 NTFS 磁碟區才能裝載命名空間。 |
| 可以是成員伺服器或網域控制站。|必須是設定命名空間所在網域中的成員伺服器或網域控制站 (這項需求適用於裝載指定之網域型命名空間的每一個命名空間伺服器)。 |
| 可以由容錯移轉叢集裝載來提高命名空間可用性。|命名空間不可以是容錯移轉叢集中的叢集化資源。 不過，如果您將命名空間設定為只使用同樣於容錯移轉叢集中做為節點之伺服器的本機資源，則可以在該伺服器上找到命名空間。 |

## <a name="installing-dfs-namespaces"></a>安裝 DFS 命名空間

DFS 命名空間與 DFS 複寫是檔案和存放服務角色中的一部分。 DFS 管理工具 (DFS 管理、適用於 Windows PowerShell 的 DFS 命名空間模組以及命令列工具) 是獨立安裝的，屬於遠端伺服器管理工具的一部分。

使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)、伺服器管理員或 POWERSHELL 安裝 DFS 命名空間，如下一節所述。

### <a name="to-install-dfs-by-using-server-manager"></a>使用 [伺服器管理員] 安裝 DFS

1. 開啟 [伺服器管理員]，按一下 [管理] ，然後按一下 [新增角色及功能] 。 [新增角色及功能精靈] 隨即顯示。

2. 在 [伺服器選取項目]  頁面上，選取想要安裝 DFS 的伺服器或離線虛擬機器的虛擬硬碟 (VHD)。

3. 選取您要安裝的角色服務及功能。

    - 若要安裝 DFS 命名空間服務，請選取 **\[伺服器角色\]** 頁面上的 **\[DFS 命名空間\]**。

    - 若要只安裝 DFS 管理工具，請在 [功能]  頁面上，依序展開 [遠端伺服器管理工具] 、[角色管理工具] 、[檔案服務工具] ，然後選取 [DFS 管理工具] 。

         [DFS 管理工具]**** 會安裝 DFS 管理嵌入式管理單元、適用於 Windows PowerShell 的 DFS 命名空間模組及命令列工具，但是不會在伺服器上安裝任何 DFS 服務。

### <a name="to-install-dfs-by-using-windows-powershell"></a>使用 Windows PowerShell 安裝 DFS

以提高的使用者權限開啟 Windows PowerShell 工作階段，然後輸入下列命令，其中 <name\> 是想要安裝的角色服務或功能 (請參閱下表，了解相關角色服務或功能名稱的清單)：

```PowerShell
Install-WindowsFeature <name>
```

| 角色服務或功能 | 名稱 |
| ----------------------- | ---- |
| DFS 命名空間          | `FS-DFS-Namespace` |
| DFS 管理工具    | `RSAT-DFS-Mgmt-Con` |

例如，若要安裝 [遠端伺服器管理工具] 功能的 [分散式檔案系統工具] 部分，請輸入：

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

若要安裝 [遠端伺服器管理工具] 功能的 [DFS 命名空間] 及 [分散式檔案系統工具] 部分，請輸入：

```PowerShell
Install-WindowsFeature "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```

## <a name="interoperability-with-azure-virtual-machines"></a>與 Azure 虛擬機器的互通性

已經測試過在 Microsoft Azure 的虛擬機器上使用 DFS 命名空間，但是有一些限制和您必須遵循的需求。

- 您無法在 Azure 虛擬機器中叢集獨立命名空間。

- 您可以在 Azure 虛擬機器中裝載網域型命名空間，包括具有 Azure Active Directory 的環境。

若要深入了解如何開始使用 Azure 虛擬機器，請參閱 [Azure 虛擬機器文件](/azure/virtual-machines/)。

## <a name="additional-references"></a>其他參考資料

如需其他相關資訊，請參閱下列資源。

| 內容類型        | 參考 |
| ------------------  | ----------------|
| **產品評估** | [Windows Server 中的 DFS 命名空間和 DFS 複寫的新功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn281957(v=ws.11)) |
| **部署**    | [DFS 命名空間延展性考量](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) |
| **作業**    | [DFS 命名空間：常見問題集](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee404780(v=ws.10)) |
| **社群資源** | [檔案服務和儲存體 TechNet 論壇](https://docs.microsoft.com/answers/topics/windows-server-storage.html) |
| **通訊協定**        | [Windows Server 中的檔案服務通訊協定](/openspecs/windows_protocols/MS-WINPROTLP/df36f95e-6a6b-48d6-a3ae-35a17674f546) (已淘汰)  |
| **相關技術** | [容錯移轉叢集](../../failover-clustering/failover-clustering-overview.md)|
| **支援** | [Windows IT 專業人員支援](https://www.microsoft.com/itpro/windows/support)|
