---
title: Windows 10 或 Windows Server 上的 Hyper-v 升級虛擬機器版本
description: 提供升級虛擬機器版本的指示和考慮
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 236e0230c30e1d0260f4a72d1735b3ba687c3fd8
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746043"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Windows 10 或 Windows Server 上的 Hyper-v 升級虛擬機器版本

>適用于： Windows 10、Windows Server 2019、Windows Server 2016、Windows Server (半年通道) 

藉由升級設定版本，讓您的虛擬機器上提供最新的 Hyper-v 功能。 請勿這樣做：

- 您將 Hyper-v 主機升級至最新版的 Windows 或 Windows Server。
- 您可以升級叢集功能等級。
- 您可以確定不需要將虛擬機器移回執行舊版 Windows 或 Windows Server 的 Hyper-v 主機。

如需詳細資訊，請參閱叢集 [作業系統輪流升級](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) 和 [在 VMM 中執行 hyper-v 主機叢集的輪流升級](/system-center/vmm/hyper-v-rolling-upgrade)。

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>步驟1：檢查虛擬機器設定版本

1. 在 Windows 桌面上，按一下 [開始] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。
2. 以滑鼠右鍵按一下 Windows PowerShell，然後選取 [以 **系統管理員身分執行**]。
3. 使用「 [取得 VM](/powershell/module/hyper-v/get-vm)」 Cmdlet。 執行下列命令以取得虛擬機器的版本。

```PowerShell
Get-VM * | Format-Table Name, Version
```

您也可以在 [Hyper-v 管理員] 中，選取虛擬機器並查看 [ **摘要** ] 索引標籤，以查看設定版本。

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>步驟2：升級虛擬機器設定版本

1. 在 [Hyper-v 管理員] 中關閉虛擬機器。
2. 選取 [動作] > 升級設定版本。 如果虛擬機器無法使用此選項，表示虛擬機器已經是 Hyper-V 主機所支援的最高組態版本。

若要使用 Windows PowerShell 升級虛擬機器設定版本，請使用 [VMVersion](/powershell/module/hyper-v/update-vmversion) Cmdlet。 執行下列命令，其中 vmname 是虛擬機器的名稱。

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>支援的虛擬機器設定版本

執行 PowerShell Cmdlet [VMHostSupportedVersion](/powershell/module/hyper-v/get-vmhostsupportedversion) ，以查看您的 hyper-v 主機支援的虛擬機器設定版本。 當您建立虛擬機器時，它會使用預設的設定版本來建立。 若要查看預設值，請執行下列命令。

```PowerShell
Get-VMHostSupportedVersion -Default
```

如果您需要建立可移至執行舊版 Windows 之 Hyper-v 主機的虛擬機器，請使用 [新的-VM](/powershell/module/hyper-v/new-vm) Cmdlet 搭配-version 參數。 例如，若要建立可移至執行 Windows Server 2012 R2 之 Hyper-v 主機的虛擬機器，請執行下列命令。 此命令會建立名為 "WindowsCV5" 的虛擬機器，其設定版本為5.0。

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>您可以匯入為執行舊版 Windows 的 Hyper-v 主機建立的虛擬機器，或從備份還原這些虛擬機器。 如果 VM 的設定版本未在下表中列為 Hyper-v 主機 OS 支援，您必須先更新 VM 設定版本，才能啟動 VM。

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>長期維護主機支援的 VM 設定版本

下表列出執行長期維護版本 Windows 的主機所支援的 VM 設定版本。

| Hyper-v 主機 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 2016 長期維護|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8。1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>半年通道主機支援的 VM 設定版本

下表列出執行目前支援的 Windows 半年通道版本之主機的 VM 設定版本。 若要取得 Windows 的半年通道版本的詳細資訊，請流覽 [Windows Server](../../../get-started-19/servicing-channels-19.md) 的下列頁面，並 [Windows 10](/windows/deployment/update/waas-overview#servicing-channels)

| Hyper-v 主機 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 2019 年5月更新 (1903 版)  |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server 版本 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server，版本 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年 10 月更新 (版本 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 1803 版|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年 4 月更新 (版本 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (版本 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (版本 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 年度更新版 (版本 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>為什麼我應該升級虛擬機器設定版本？

當您將虛擬機器移動或匯入至執行 Windows Server 2019、Windows Server 2016 或 Windows 10 上 Hyper-v 的電腦時，不會自動更新虛擬機器的設定。 這表示您可以將虛擬機器移回執行舊版 Windows 或 Windows Server 的 Hyper-v 主機。 但是，這也表示您無法使用一些新的虛擬機器功能，直到您手動更新設定版本。 您無法在升級虛擬機器設定版本之後將其降級。

虛擬機器設定版本代表虛擬機器設定的相容性、儲存的狀態，以及具有 Hyper-v 版本的快照集檔案。 當您更新設定版本時，您可以變更用來儲存虛擬機器設定和檢查點檔案的檔案結構。 您也會將設定版本更新為該 Hyper-v 主機所支援的最新版本。 升級後的虛擬機器會使用新的設定檔案格式，這種格式是為了提高讀取和寫入虛擬機器設定資料的效率而設計。 升級也會減少儲存體失敗時的資料損毀可能性。

下表列出針對新的或已升級的虛擬機器所使用的每種檔案類型的描述、副檔名和預設位置。

 |虛擬機器檔案類型 | 描述|
 |---|---|
|設定 |以二進位檔案格式儲存的虛擬機器設定資訊。 <br /> 副檔名：. >.vmcx <br /> 預設位置： C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 機器|
 |執行時間狀態|以二進位檔案格式儲存的虛擬機器執行時間狀態資訊。 <br />副檔名：. .vmrs 和. vmgs <br />預設位置： C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 機器|
|虛擬硬碟|儲存虛擬機器的虛擬硬碟。 <br /> 副檔名： .vhd 或 .vhdx <br />預設位置： C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬碟|
 |自動虛擬硬碟 |用於虛擬機器檢查點的差異磁片檔案。 <br /> 副檔名：. .avhdx <br /> 預設位置： C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬碟|
 |Checkpoint|檢查點會儲存在多個檢查點檔案中。 每個檢查點會建立設定檔案和執行階段狀態檔案。 <br /> 副檔名：. .vmrs 和. >.vmcx <br />預設位置： C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>如果我沒有升級虛擬機器設定版本，會發生什麼事？

如果您有使用舊版 Hyper-v 建立的虛擬機器，則在更新設定版本之前，較新主機 OS 上的某些功能可能無法使用這些虛擬機器。

作為一般指引，建議您在將虛擬化主機成功升級至較新版本的 Windows 之後，更新設定版本，並確信不需要復原。 當您使用叢集 [OS 輪流升級](../../../failover-clustering/cluster-operating-system-rolling-upgrade.md) 功能時，通常會在更新叢集功能等級之後進行。 如此一來，您也將受益于新功能和內部變更和優化。

>[!NOTE]
>更新 VM 設定版本之後，VM 將無法在不支援更新設定版本的主機上啟動。

下表顯示使用某些 Hyper-v 功能所需的最小虛擬機器設定版本。

|功能|最低 VM 設定版本|
|---|---|
|熱新增/移除記憶體|6.2|
|Linux VM 的安全開機|6.2|
|生產檢查點|6.2|
|PowerShell Direct |6.2|
|虛擬機器群組|6.2|
|虛擬信賴平台模組 (vTPM)|7.0|
|虛擬機器多佇列 (VMMQ) |7.1|
|XSAVE 支援|8.0|
|金鑰存放磁片磁碟機|8.0|
|以來賓虛擬安全性為基礎的安全性支援 (VBS) |8.0|
|巢狀虛擬化|8.0|
|虛擬處理器計數|8.0|
|大型記憶體 Vm|8.0|
|將虛擬裝置的預設最大數目增加到每個裝置 64 (例如網路和指派的裝置) |8.3|
|允許 Perfmon 的其他處理器功能|9.0|
|使用[核心](../manage/manage-hyper-v-scheduler-types.md#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)排程器自動公開主機上執行之 vm 的[並行多執行緒](../manage/manage-hyper-v-scheduler-types.md#background)設定|9.0|
|休眠支援|9.0|

如需這些功能的詳細資訊，請參閱 [Windows Server 上的 hyper-v 新](../What-s-new-in-Hyper-V-on-Windows.md)功能。