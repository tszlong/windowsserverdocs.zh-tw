---
title: 使用 Windows Admin Center 部署超融合式基礎結構
description: 使用 Windows Admin Center 部署超融合式基礎結構。
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 11/04/2019
ms.openlocfilehash: 1f8e8a5433b986333975f45009819a98d90b61fc
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517504"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>使用 Windows Admin Center 部署超融合式基礎結構

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

您可以使用 Windows Admin Center [1910 版](../overview.md) 或更新版本，以使用兩個或更適合的 Windows 伺服器來部署超融合式基礎結構。 這項新功能採用多階段工作流程的形式，可引導您安裝功能、設定網路功能、建立叢集，以及部署儲存空間直接存取及/或軟體定義的網路功能 (SDN) （如果選取）。

從2007版 Windows Admin Center，Windows Admin Center 支援 Azure Stack HCI 作業系統。 瞭解 [如何在 Azure Stack HCI 檔的 Windows Admin Center 中部署](/azure-stack/hci/get-started)叢集。雖然本檔著重于 Azure Stack HCI，但這些指示也大部分適用于 Windows Server 部署。

## <a name="undo-and-start-over"></a>復原並重新開始

使用這些 Windows PowerShell Cmdlet 來復原工作流程所做的變更，然後重新開始。

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>移除虛擬機器或其他叢集資源

如果您已建立任何虛擬機器或其他叢集資源（例如軟體定義網路的網路控制站），請先將其移除。

例如，若要依名稱移除資源，請使用：

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>復原儲存體步驟

如果您已啟用儲存空間直接存取，請使用下列腳本來停用它：

> [!Warning]
> 這些 Cmdlet 會永久刪除儲存空間直接存取磁片區中的任何資料。 這是無法復原的。

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>復原群集步驟

如果您已建立叢集，請使用此 Cmdlet 將它移除：

```PowerShell
Remove-Cluster -CleanUpAD
```

若要同時移除叢集驗證報告，請在屬於叢集一部分的每個伺服器上執行此 Cmdlet：

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>復原網路步驟

在屬於叢集一部分的每個伺服器上執行這些 Cmdlet。

如果您已建立 Hyper-v 虛擬交換器：

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> 此 `Remove-VMSwitch` Cmdlet 會自動移除任何虛擬介面卡，並復原實體介面卡的交換器內嵌組合。

如果您修改了網路介面卡屬性，例如名稱、IPv4 位址和 VLAN 識別碼：

> [!Warning]
> 這些 Cmdlet 會移除網路介面卡名稱和 IP 位址。 請確定您有在之後連線所需的資訊，例如從下列腳本排除的介面卡管理。 此外，也請確定您知道伺服器的連線方式，例如 MAC 位址之類的實體屬性，而不只是 Windows 中的介面卡名稱。

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

您現在已經準備好開始工作流程。

## <a name="additional-references"></a>其他參考

- [Hello，Windows Admin Center](../overview.md)
- [部署儲存空間直接存取](../../../storage/storage-spaces/deploy-storage-spaces-direct.md)
