---
title: 使用 Windows 管理中心部署超大範圍基礎結構
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: 62bf21dd0afcb99aa77cff8a733e80fc4cffe2fb
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587228"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>使用 Windows 管理中心部署超大範圍基礎結構

> 適用于： Windows 系統管理中心、Windows 系統管理中心預覽

您可以使用 Windows 管理中心[1910 版](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)或更新版本，使用兩個或更多適合的 Windows 伺服器來部署超大範圍基礎結構。 這項新功能採用多階段工作流程的形式，引導您安裝功能、設定網路、建立叢集，以及部署儲存空間直接存取和/或軟體定義的網路功能（SDN）（若已選取）。

![叢集建立工作流程的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/deployment-workflow-screenshot.png)

  > [!Important]
  > 這項功能正處於開發階段。 它在預覽中提供，讓您可以提早試用並分享您的意見反應。

## <a name="preview-the-workflow"></a>預覽工作流程

### <a name="1-prerequisites"></a>1. 必要條件

Windows 管理中心的叢集建立工作流程不會執行裸機作業系統安裝，因此您必須先在每部伺服器上安裝 Windows Server。 支援的版本為 Windows Server 2016、Windows Server 2019 和 Windows Server Insider preview。 您也必須將每部伺服器聯結到與 Windows 管理中心執行所在的相同 Active Directory 網域，然後再啟動工作流程。

### <a name="2-install-windows-admin-center"></a>2. 安裝 Windows 系統管理中心
 
請依照指示[下載並安裝](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)最新版的 Windows 管理中心。

### <a name="3-install-the-cluster-creation-extension"></a>3. 安裝叢集建立擴充功能

叢集建立工作流程會透過 Windows 管理中心延伸模組摘要傳遞和更新。 如此一來，我們就可以解決您的意見反應，並在預覽階段進行更快速的改進。

若要安裝此擴充功能，請啟動 Windows 系統管理中心，然後：

1.  在右上方選取 [設定] 圖示
2.  流覽至延伸模組
3.  尋找叢集建立擴充功能
4.  選取安裝

![安裝叢集建立延伸模組的四個步驟的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/how-to-install.png)

安裝之後，從左上方的下拉式清單中選取 [叢集建立] 延伸模組：

![啟動叢集建立擴充功能的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/how-to-launch.png)

## <a name="limitations-of-the-preview-release"></a>預覽版本的限制

叢集建立工作流程正在進行開發，換言之，尚未完成。

以下是目前預覽版本的一些限制：

1.  **網域需求。** 工作流程目前要求您新增的每一部伺服器都必須已加入與 Windows Admin Center 執行所在的相同 Active Directory 網域。
2.  **顯示腳本。** 使用目前的預覽版本時，您無法使用 Windows 管理中心的 [顯示腳本] 功能來查看工作流程執行的腳本，也無法下載最後發生之所有專案的完整報告。 我們知道這對許多使用者來說都很重要–敬請期待。 在此同時，請使用以下提供的 Cmdlet 來[查看發生什麼情況](#see-whats-happening)。
3.  **繼續或重新開機。** 雖然您可以透過工作流程結束中途，但目前的預覽版本並不會處理從您離開的位置繼續進行，也不能在開始之前完全清除。 如果您想要在相同的伺服器上重複部署以進行測試，請使用以下提供的 Cmdlet 來[復原和啟動](#undo-and-start-over)。
4.  **遠端直接記憶體存取（RDMA）。** 目前的預覽版本無法啟用 RDMA 網路，這通常與超大範圍基礎結構搭配使用。 現在，請在不啟用 RDMA 的情況下完成工作流程，然後使用其他工具來啟用 RDMA，就像您在其他情況下所做的一樣。

除了解決這些限制以外，未來的版本中可能會加入無數的潛在功能：套用 Windows 更新、設定叢集仲裁的見證、匯入虛擬機器等等。 為協助我們設定您想要的功能優先順序，請[分享意見](#feedback)反應，如下所述。

## <a name="see-whats-happening"></a>查看發生的情況

請使用這些 Windows PowerShell Cmdlet 來遵循，並查看工作流程的作用。

若要查看已安裝的 Windows 功能，請使用 `Get-WindowsFeature` Cmdlet。 例如：

```PowerShell
Get-WindowsFeature "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "BitLocker"
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-1.png)

  > [!Note]
  > 安裝的功能取決於您選取的叢集類型。

若要查看網路介面卡及其屬性，例如名稱、IPv4 位址和 VLAN ID：

```PowerShell
Get-NetAdapter | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
Get-NetAdapter | Where Status -Eq "Up" | Get-NetIPAddress -AddressFamily IPv4 -ErrorAction SilentlyContinue | Sort InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-2.png)

若要查看 Hyper-v 虛擬交換器，以及實體網路介面卡的組合方式：

```PowerShell
Get-VMSwitch
Get-VMSwitchTeam
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-3.png)

若要查看主機虛擬網路介面卡（如果有的話），請執行：

```PowerShell
Get-VMNetworkAdapter -ManagementOS | Format-Table Name, IsManagementOS, SwitchName
Get-VMNetworkAdapterTeamMapping -ManagementOS | Format-Table NetAdapterName, ParentAdapter
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-4.png)

若要查看目前的容錯移轉叢集和其成員節點，請執行：

```PowerShell
Get-Cluster
Get-ClusterNode
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-5.png)

若要查看是否已啟用儲存空間直接存取，以及自動建立的預設儲存集區：

```PowerShell
Get-ClusterStorageSpacesDirect
Get-StoragePool -IsPrimordial $False
```

![PowerShell 輸出的螢幕擷取畫面](../media/deploy-hyperconverged-infrastructure/script-out-6.png)

## <a name="undo-and-start-over"></a>復原並從頭開始

使用這些 Windows PowerShell Cmdlet 來復原工作流程所做的變更，並從頭開始。

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>移除虛擬機器或其他叢集資源

如果您已建立任何虛擬機器或其他叢集資源（例如軟體定義網路的網路控制站），請先將它們移除。

例如，若要依名稱移除資源，請使用：

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>復原儲存體步驟

如果您已啟用儲存空間直接存取，請使用下列腳本將它停用：

> [!Warning]
> 這些 Cmdlet 會永久刪除儲存空間直接存取磁片區中的任何資料。 這無法復原。

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>復原叢集步驟

如果您已建立叢集，請使用此 Cmdlet 將它移除：

```PowerShell
Remove-Cluster -CleanUpAD
```

若也要移除叢集驗證報告，請在屬於叢集一部分的每部伺服器上執行此 Cmdlet：

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>復原網路步驟

在屬於叢集一部分的每一部伺服器上執行這些 Cmdlet。

如果您已建立 Hyper-v 虛擬交換器：

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> `Remove-VMSwitch` Cmdlet 會自動移除任何虛擬配接器，並復原實體介面卡的交換器內嵌小組。

如果您修改了網路介面卡內容，例如 [名稱]、[IPv4 位址] 和 [VLAN ID]：

> [!Warning]
> 這些 Cmdlet 會移除網路介面卡名稱和 IP 位址。 請確定您擁有連線所需的資訊，例如從下面的腳本排除的介面卡管理。 此外，也請確定您知道伺服器如何連接到 MAC 位址之類的實體屬性，而不只是 Windows 中的介面卡名稱。

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

您現在已經準備好開始工作流程。

## <a name="feedback"></a>意見反應

此預覽版本是您的意見反應。 以下是您可以觸及小組的一些方式：

- [在 UserVoice 上提交和投票功能要求](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft Tech 社區的 Windows 管理中心論壇](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 電子郵件 hci-部署 [at] microsoft.com
- 推文至[@servermgmt](https://twitter.com/servermgmt)

## <a name="report-an-issue"></a>回報問題

使用上面列出的通道來回報叢集建立工作流程的問題。

可能的話，請包含下列資訊，以協助我們快速重現並解決您的問題：

- 您選取的叢集類型（例如： *"超大範圍"* ）
- 您遇到此問題的步驟（範例： *"3.2 Create cluster"* ）
- 叢集建立擴充功能的版本。 移至 [**設定**] > **延伸**模組 > **已安裝的擴充**功能，並查看**版本**資料行（範例： *"1.0.30"* ）。
- 錯誤訊息，不論是在畫面上，還是在瀏覽器主控台中，您可以按**F12**開啟。
- 您環境的任何其他相關資訊 

## <a name="see-also"></a>請參閱

- [Hello，Windows 系統管理中心](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [部署儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
