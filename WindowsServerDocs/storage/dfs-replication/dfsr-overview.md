---
Title: DFS 複寫概觀
ms.date: 03/08/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: dd381c04b02889a7f2e7b8992ff6050d1b0f078a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453057"
---
# <a name="dfs-replication-overview"></a>DFS 複寫概觀

> 適用於：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server （半年通道）

DFS 複寫是跨多部伺服器和站台可讓您有效率地複寫資料夾 （包括 DFS 命名空間路徑參照的） 的 Windows Server 中的角色服務。 DFS 複寫是一種有效率、 多重主要複寫引擎，可用來讓跨有限的頻寬網路連線的伺服器之間同步處理的資料夾。 做為複寫引擎中，DFS 命名空間，以及使用 Windows Server 2008 或更新版本的網域功能等級的網域中的 Active Directory 網域服務 (AD DS) SYSVOL 資料夾的複寫，它會取代檔案複寫服務 (FRS)。

DFS 複寫使用的壓縮演算法稱為遠端差異壓縮 (RDC)。 RDC 會偵測檔案中的資料的變更，並讓 DFS 複寫只變更的檔案區塊而非整個檔案。

如需有關如何複寫使用 DFS 複寫的 SYSVOL 的詳細資訊，請參閱[移轉至 DFS 複寫的 SYSVOL 複寫](migrate-sysvol-to-dfsr.md)。

若要使用 DFS 複寫，您必須建立複寫群組，並將複寫的資料夾新增至群組。 複寫群組、 複寫的資料夾以及成員如下圖所示。

![此複寫群組中包含兩個成員之間的連線，每個有一些複寫資料夾](media/dfsr-overview.gif)

此圖顯示複寫群組是一組伺服器，稱為成員的參與一或多個複寫資料夾的複寫。 複寫的資料夾是保持同步，每個成員上的資料夾。 在圖中，有兩個複寫的資料夾：Projects 及 Proposals。 每個複寫資料夾中的資料變更，所做的變更會複寫整個複寫群組的成員之間的連線。 所有成員之間的連線會形成複寫拓撲。
建立單一的複寫群組中的多個複寫的資料夾可簡化部署複寫的資料夾，因為拓樸、 排程和頻寬節流套用於複寫群組會套用到每個複寫資料夾的程序。 若要部署其他複寫的資料夾，您可以使用 Dfsradmin.exe 或遵循指示在精靈中定義的本機路徑和新的複寫資料夾的權限。

每個複寫的資料夾的唯一設定，例如檔案和子資料夾篩選器，以便您可以篩選出不同的檔案和每個複寫資料夾的子資料夾。

每個成員上的複寫的資料夾可以位於不同的磁碟區，在成員，並不需要複寫的資料夾是共用的資料夾或命名空間的一部分。 不過，DFS 管理嵌入式管理單元可讓您輕鬆地共用複寫的資料夾，並選擇性地將其發行現有的命名空間中。

您可以使用 DFS 管理、 DfsrAdmin 和 Dfsrdiag 命令或稱為 WMI 的指令碼來管理 DFS 複寫。

## <a name="requirements"></a>需求

在部署 DFS 複寫前，必須依照下列方式設定伺服器：

- 更新 Active Directory 網域服務 (AD DS) 結構描述，以包含 Windows Server 2003 R2 或更新版本的結構描述新增項目。 您無法使用 Windows Server 2003 R2 或較舊的結構描述新增唯讀複寫的資料夾。
- 確定複寫群組中的所有伺服器皆位於相同的樹系。 您無法在不同樹系的伺服器上啟用複寫。
- 在即將做為複寫群組成員的所有伺服器上安裝 DFS 複寫。
- 連絡您的防毒軟體廠商，確認您的防毒軟體是否與 DFS 複寫相容。
- 在以 NTFS 檔案系統格式化的磁碟區上找到您想要複寫的任意資料夾。 DFS 複寫不支援復原檔案系統 (ReFS) 或 FAT 檔案系統。 DFS 複寫也不支援複寫儲存在叢集共用磁碟區的內容。

## <a name="interoperability-with-azure-virtual-machines"></a>與 Azure 虛擬機器的互通性

在 Azure 中的虛擬機器上使用 DFS 複寫經過 Windows Server;不過，有一些限制和您必須遵循的需求。

- 使用快照或儲存的狀態來還原執行 DFS 複寫進行 SYSVOL 資料夾以外任何內容之複寫工作的伺服器，會造成 DFS 複寫失敗，因為這項工作需要特殊的資料庫復原步驟。 同樣地，請勿匯出、再製或複製虛擬機器。 如需詳細資訊，請參閱 Microsoft 知識庫的文章 [2517913](http://support.microsoft.com/kb/2517913) ，以及 [安全地虛擬化 DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)。
- 備份虛擬機器裝載的複寫資料夾中的資料時，您必須使用客體虛擬機器內的備份軟體。
- DFS 複寫需要存取實體或虛擬化網域控制站 – 它不能直接與 Azure AD 進行通訊。
- DFS 複寫需要您的內部部署複寫群組成員，與裝載在 Azure VM 中的任何成員之間的 VPN 連線。 您也需要設定內部部署路由器 (例如 Forefront Threat Management Gateway)，允許 RPC 端點對應程式 (連接埠 135) 和 49152 到 65535 之間隨機指派的連接埠通過 VPN 連線。 您可以使用 Set-dfsrmachineconfiguration cmdlet 或 Dfsrdiag 命令列工具，而不是隨機的連接埠的靜態連接埠。 如需有關如何指定 DFS 複寫的靜態連接埠詳細資訊，請參閱 [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration)。 如需開啟用於管理 Windows Server 的相關連接埠的詳細資訊，請參閱 Microsoft 知識庫文章 [832017](http://support.microsoft.com/kb/832017) 。

若要深入瞭解如何開始使用 Azure 虛擬機器，請造訪 [Microsoft Azure 網站](https://docs.microsoft.com/azure/virtual-machines/)。

## <a name="installing-dfs-replication"></a>安裝 DFS 複寫

DFS 複寫是檔案和存放服務角色的一部分。 DFS （DFS 管理、 Windows PowerShell 和命令列工具的 DFS 複寫模組） 的管理工具會分別做為遠端伺服器管理工具的一部分安裝。

使用安裝 DFS 複寫[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)，伺服器管理員或 PowerShell，在後續章節中所述。

### <a name="to-install-dfs-by-using-server-manager"></a>使用 [伺服器管理員] 安裝 DFS

1. 開啟 [伺服器管理員]，按一下 [管理]  ，然後按一下 [新增角色及功能]  。 [新增角色及功能精靈] 隨即顯示。

2. 在 [伺服器選取項目]  頁面上，選取想要安裝 DFS 的伺服器或離線虛擬機器的虛擬硬碟 (VHD)。

3. 選取您要安裝的角色服務及功能。

    - 若要安裝 DFS 複寫服務，在**伺服器角色**頁面上，選取**DFS 複寫**。

    - 若要只安裝 DFS 管理工具，請在 [功能]  頁面上，依序展開 [遠端伺服器管理工具]  、[角色管理工具]  、[檔案服務工具]  ，然後選取 [DFS 管理工具]  。

         **DFS 管理工具**會安裝 DFS 管理嵌入式管理單元，DFS 複寫及 DFS 命名空間模組的 Windows PowerShell 及命令列工具，但是它不會在伺服器上安裝任何 DFS 服務。

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>若要使用 Windows PowerShell 安裝 DFS 複寫

開啟提升權限的使用者權限，Windows PowerShell 工作階段，然後輸入下列命令，其中 < 名稱\>是角色服務或您想要安裝的功能 （請參閱下表中的相關角色服務或功能名稱的清單）：

```PowerShell
Install-WindowsFeature <name>
```

|角色服務或功能|名稱|
|---|---|
|DFS 複寫|`FS-DFS-Replication`|
|DFS 管理工具|`RSAT-DFS-Mgmt-Con`|

例如，若要安裝 [遠端伺服器管理工具] 功能的 [分散式檔案系統工具] 部分，請輸入：

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

若要安裝 DFS 複寫和遠端伺服器管理工具功能的分散式檔案系統工具部分，請輸入：

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>另請參閱

- [DFS 命名空間和 DFS 複寫概觀](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [檢查清單：部署 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [檢查清單：管理 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [部署 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [管理 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [DFS 複寫疑難排解](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
