---
title: 使用 Windows 管理中心部署超大範圍基礎結構
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 11/04/2019
ms.openlocfilehash: a7c15bd07754d48b7fbffe2cd95edaa871c9bde3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964444"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>使用 Windows 管理中心部署超大範圍基礎結構

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

您可以使用 Windows 管理中心[1910 版](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)或更新版本，使用兩個或更多適合的 Windows 伺服器來部署超大範圍基礎結構。 這項新功能會採用多階段工作流程的形式，引導您安裝功能、設定網路功能、建立叢集，以及部署儲存空間直接存取和/或軟體定義的網路 (SDN) （如果已選取）。

從 Windows 系統管理中心2007版，Windows 系統管理中心支援 Azure Stack HCI OS。 如需瞭解[如何在 Windows 系統管理中心部署叢集，請參閱 AZURE STACK HCI](https://docs.microsoft.com/azure-stack/hci/getting-started)檔。Althought 本檔著重于 Azure Stack HCI，指示也適用于 Windows Server 部署。

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
> `Remove-VMSwitch`Cmdlet 會自動移除任何虛擬配接器，並復原實體介面卡的交換器內嵌小組。

如果您修改了網路介面卡內容，例如 [名稱]、[IPv4 位址] 和 [VLAN ID]：

> [!Warning]
> 這些 Cmdlet 會移除網路介面卡名稱和 IP 位址。 請確定您擁有連線所需的資訊，例如從下面的腳本排除的介面卡管理。 此外，也請確定您知道伺服器如何連接到 MAC 位址之類的實體屬性，而不只是 Windows 中的介面卡名稱。

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

您現在已經準備好開始工作流程。

## <a name="additional-references"></a>其他參考資料

- [Hello，Windows 系統管理中心](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [部署儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
