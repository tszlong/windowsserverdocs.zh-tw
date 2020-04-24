---
title: DFS 複寫概觀
ms.date: 03/08/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: c17fd78a2cf726ab156d3eda09b9c0e2d4ed6a75
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "77520354"
---
# <a name="dfs-replication-overview"></a>DFS 複寫概觀

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server (半年通道)

DFS 複寫是 Windows Server 中的角色服務，可讓您在多個伺服器與站台有效率地複寫資料夾 (包括 DFS 命名空間路徑參照的資料夾)。 DFS 複寫是多重主要複寫引擎，可供您透過有限頻寬的網路連線讓伺服器之間的資料夾保持同步。 其會將檔案複寫服務 (FRS) 取代為 DFS 命名空間的複寫引擎，並且也可用來在使用 Windows Server 2008 或更新版本網域功能等級的網域中，複寫 Active Directory Domain Services (AD DS) SYSVOL 資料夾。

DFS 複寫使用的壓縮演算法稱為遠端差異壓縮 (RDC)。 RDC 會偵測到檔案中資料的變更，然後讓 DFS 複寫僅複寫變更的檔案區塊，而不是複寫整個檔案。

如需使用 DFS 複寫來複寫 SYSVOL 的詳細資訊，請參閱[將 SYSVOL 複寫遷移至 DFS 複寫](migrate-sysvol-to-dfsr.md)。

若要使用 DFS 複寫，必須建立複寫群組，並在群組中新增複寫資料夾。 下圖中說明複寫群組、複寫資料夾以及成員。

![包含兩個成員間連線的複寫群組，每個成員各有幾個已複寫的資料夾](media/dfsr-overview.gif)

該圖顯示出複寫群組是一組伺服器 (稱為成員)，這些伺服器參與了一或多個複寫資料夾的複寫。 複寫資料夾是在每個成員上保持同步的資料夾。 圖中有兩個複寫資料夾：Projects 及 Proposals。 當每個複寫資料夾中的資料發生變更時，就會透過複寫群組成員之間的連線複寫變更。 所有成員之間的連線即形成複寫拓撲。
在單一複寫群組中建立多個複寫資料夾可簡化部署複寫資料夾的程序，因為複寫群組的拓撲、排程和頻寬節流會套用到每個複寫資料夾。 若要部署其他複寫資料夾，可使用 Dfsradmin.exe 或遵循精靈的指示來定義新複寫資料夾的本機路徑和權限。

每個複寫資料夾都有其自己的設定，例如檔案和子資料夾篩選器，因此您可以針對每個複寫資料夾篩選掉不同的檔案和子資料夾。

每個成員的複寫資料夾可位於成員的不同磁碟區中，所以複寫資料夾不需要是共用資料夾或命名空間的一部分。 不過，[DFS 管理] 嵌入式管理單元讓共用複寫資料夾變得容易，而且可以選擇是否將資料夾公佈到現有命名空間。

您可以使用 DFS 管理、DfsrAdmin 與 Dfsrdiag 命令，或會呼叫 WMI 的指令碼來管理 DFS 複寫。

## <a name="requirements"></a>需求

在部署 DFS 複寫前，必須依照下列方式設定伺服器：

- 更新 Active Directory Domain Services (AD DS) 結構描述以包含 Windows Server 2003 R2 或更新版本的結構描述新增項目。 您無法在 Windows Server 2003 R2 或新增的舊版結構描述使用唯讀複寫資料夾。
- 確定複寫群組中的所有伺服器皆位於相同的樹系。 您無法在不同樹系的伺服器上啟用複寫。
- 在即將做為複寫群組成員的所有伺服器上安裝 DFS 複寫。
- 連絡您的防毒軟體廠商，確認您的防毒軟體是否與 DFS 複寫相容。
- 在以 NTFS 檔案系統格式化的磁碟區上找到您想要複寫的任意資料夾。 DFS 複寫不支援復原檔案系統 (ReFS) 或 FAT 檔案系統。 DFS 複寫也不支援複寫儲存在叢集共用磁碟區的內容。

## <a name="interoperability-with-azure-virtual-machines"></a>與 Azure 虛擬機器的互通性

在 Azure 的虛擬機器上使用 DFS 複寫已經過 Windows Server 測試，但您必須遵循一些限制和需求。

- 使用快照或儲存的狀態來還原執行 DFS 複寫進行 SYSVOL 資料夾以外任何內容之複寫工作的伺服器，會造成 DFS 複寫失敗，因為這項工作需要特殊的資料庫復原步驟。 同樣地，請勿匯出、再製或複製虛擬機器。 如需詳細資訊，請參閱 Microsoft 知識庫的文章 [2517913](https://support.microsoft.com/kb/2517913) ，以及 [安全地虛擬化 DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)。
- 備份虛擬機器裝載的複寫資料夾中的資料時，您必須使用客體虛擬機器內的備份軟體。
- DFS 複寫需要存取實體或虛擬化網域控制站 – 其不能直接與 Azure AD 通訊。
- DFS 複寫需要您的內部部署複寫群組成員，與裝載在 Azure VM 中的任何成員之間的 VPN 連線。 您也需要設定內部部署路由器 (例如 Forefront Threat Management Gateway)，允許 RPC 端點對應程式 (連接埠 135) 和 49152 到 65535 之間隨機指派的連接埠通過 VPN 連線。 您可以使用 Set-DfsrMachineConfiguration Cmdlet 或 Dfsrdiag 命令列工具來指定靜態連接連接埠，而不是隨機的連接連接埠。 如需有關如何指定 DFS 複寫的靜態連接埠詳細資訊，請參閱 [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration)。 如需開啟用於管理 Windows Server 的相關連接埠的詳細資訊，請參閱 Microsoft 知識庫文章 [832017](https://support.microsoft.com/kb/832017) 。

若要深入瞭解如何開始使用 Azure 虛擬機器，請造訪 [Microsoft Azure 網站](https://docs.microsoft.com/azure/virtual-machines/)。

## <a name="installing-dfs-replication"></a>安裝 DFS 複寫

DFS 複寫是檔案和存放服務角色的一部分。 DFS 管理工具 (DFS 管理、適用於 Windows PowerShell 的 DFS 複寫模組以及命令列工具) 是獨立安裝的，屬於遠端伺服器管理工具的一部分。

使用 [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)、伺服器管理員或 PowerShell 來安裝 DFS 複寫，如後續幾節所述。

### <a name="to-install-dfs-by-using-server-manager"></a>使用 [伺服器管理員] 安裝 DFS

1. 開啟 [伺服器管理員]，按一下 [管理]  ，然後按一下 [新增角色及功能]  。 [新增角色及功能精靈] 隨即顯示。

2. 在 [伺服器選取項目]  頁面上，選取想要安裝 DFS 的伺服器或離線虛擬機器的虛擬硬碟 (VHD)。

3. 選取您要安裝的角色服務及功能。

    - 若要安裝 DFS 複寫服務，請在 [伺服器角色]  頁面上，選取 [DFS 複寫]  。

    - 若要只安裝 DFS 管理工具，請在 [功能]  頁面上，依序展開 [遠端伺服器管理工具]  、[角色管理工具]  、[檔案服務工具]  ，然後選取 [DFS 管理工具]  。

         **DFS 管理工具**會安裝 DFS 管理嵌入式管理單元、適用於 Windows PowerShell 的 DFS 複寫和 DFS 命名空間模組以及命令列工具，但是不會在伺服器上安裝任何 DFS 服務。

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>使用 Windows PowerShell 安裝 DFS 複寫

以提高的使用者權限開啟 Windows PowerShell 工作階段，然後輸入下列命令，其中 <name\> 是想要安裝的角色服務或功能 (請參閱下表，了解相關角色服務或功能名稱的清單)：

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

若要安裝 [遠端伺服器管理工具] 功能的 [DFS 複寫] 及 [分散式檔案系統工具] 部分，請輸入：

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>另請參閱

- [DFS 命名空間和 DFS 複寫概觀](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [檢查清單：部署 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [檢查清單：管理 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [部署 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [管理 DFS 複寫](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [針對 DFS 複寫進行疑難排解](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
