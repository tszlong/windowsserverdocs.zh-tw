---
title: 升級 Windows 10 或 Windows Server 上的 Hyper-v 中的虛擬機器版本
description: 提供升級虛擬機器版本的指示和考慮
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 96678dfab2a3d5b6f503d8ce9d00850a3c437b35
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392939"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>升級 Windows 10 或 Windows Server 上的 Hyper-v 中的虛擬機器版本

>適用於：Windows 10、Windows Server 2019、Windows Server 2016、Windows Server （半年通道）

藉由升級設定版本，將最新的 Hyper-v 功能提供給您的虛擬機器。 請不要執行此動作，直到：

- 您會將 Hyper-v 主機升級至最新版本的 Windows 或 Windows Server。
- 您升級叢集功能等級。
- 您可以確定不需要將虛擬機器移回執行舊版 Windows 或 Windows Server 的 Hyper-v 主機。

如需詳細資訊，請參閱叢集[作業系統輪流升級](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)和[在 VMM 中執行 hyper-v 主機叢集的輪流升級](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade)。

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>步驟 1:檢查虛擬機器設定版本

1. 在 Windows 桌面上，按一下 \[開始\] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。
2. 以滑鼠右鍵按一下 [Windows PowerShell]，然後選取 [**以系統管理員身分執行**]。
3. 使用[Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)Cmdlet。 執行下列命令來取得虛擬機器的版本。

```PowerShell
Get-VM * | Format-Table Name, Version
```

您也可以在 [Hyper-v 管理員] 中查看設定版本，方法是選取虛擬機器，然後查看 [**摘要**] 索引標籤。

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>步驟 2:升級虛擬機器設定版本

1. 在 [Hyper-v 管理員] 中關閉虛擬機器。
2. 選取 [動作] > [升級設定版本]。 如果虛擬機器無法使用此選項，則它已是 Hyper-v 主機所支援的最高設定版本。

若要使用 Windows PowerShell 升級虛擬機器設定版本，請使用[VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) Cmdlet。 執行下列命令，其中 vmname 是虛擬機器的名稱。

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>支援的虛擬機器設定版本

執行 PowerShell Cmdlet [VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) ，以查看您的 hyper-v 主機支援的虛擬機器設定版本。 當您建立虛擬機器時，它會使用預設設定版本來建立。 若要查看預設值，請執行下列命令。

```PowerShell
Get-VMHostSupportedVersion -Default
```

如果您需要建立可移至執行舊版 Windows 之 Hyper-v 主機的虛擬機器，請使用[新的-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) Cmdlet 搭配-version 參數。 例如，若要建立可移至執行 Windows Server 2012 R2 之 Hyper-v 主機的虛擬機器，請執行下列命令。 此命令將會建立名為 "WindowsCV5" 的虛擬機器，其設定版本為5.0。

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>您可以匯入已針對執行舊版 Windows 的 Hyper-v 主機所建立的虛擬機器，或從備份還原它們。 如果 VM 的設定版本在下表中未列為您的 Hyper-v 主機 OS 支援，您必須先更新 VM 設定版本，才能啟動 VM。

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>支援長期維護主機的 VM 設定版本

下表列出執行 Windows 長期維護版本的主機所支援的 VM 設定版本。

| Hyper-v 主機 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server Standard 2012 R2|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 2016 長期維護|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>半年通道主機支援的 VM 設定版本

下表列出執行目前支援的 Windows 半年通道版本之主機的 VM 設定版本。 若要取得有關半年通道版本 Windows 的詳細資訊，請造訪[Windows Server](../../../get-started-19/servicing-channels-19.md)和[windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)的下列頁面

| Hyper-v 主機 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 5 月2019更新（版本1903） |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server 版本 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, 版本1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年10月更新（版本1809）|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 版本 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年4月更新（版本1803）|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 秋季建立者更新（版本1709）|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 建立者更新（版本1703）|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 年度更新版（版本1607）|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>為什麼要升級虛擬機器設定版本？

當您將虛擬機器移動或匯入至在 Windows Server 2019、Windows Server 2016 或 Windows 10 上執行 Hyper-v 的電腦時，不會自動更新虛擬機器的設定。 這表示您可以將虛擬機器移回執行舊版 Windows 或 Windows Server 的 Hyper-v 主機。 但是，這也表示您無法使用某些新的虛擬機器功能，直到您手動更新設定版本。 您無法在升級虛擬機器設定版本之後將其降級。

虛擬機器設定版本代表虛擬機器設定的相容性、儲存的狀態，以及具有 Hyper-v 版本的快照集檔案。 當您更新設定版本時，您會變更用來儲存虛擬機器設定和檢查點檔案的檔案結構。 您也會將設定版本更新為該 Hyper-v 主機所支援的最新版本。 升級後的虛擬機器會使用新的設定檔案格式，這種格式是為了提高讀取和寫入虛擬機器設定資料的效率而設計。 升級也會減少儲存體失敗時的資料損毀可能性。

下表列出用於新的或升級的虛擬機器之每種檔案類型的描述、副檔名和預設位置。

 |虛擬機器檔案類型 | 描述|
 |---|---|
|組態 |以二進位檔案格式儲存的虛擬機器設定資訊。 <br /> 副檔名：. .vmcx <br /> 預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |執行時間狀態|以二進位檔案格式儲存的虛擬機器執行時間狀態資訊。 <br />副檔名：. .vmrs 和. vmgs <br />預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|虛擬硬碟|儲存虛擬機器的虛擬硬碟。 <br /> 副檔名： .vhd 或 .vhdx <br />預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬碟|
 |自動虛擬硬碟 |用於虛擬機器檢查點的差異磁片檔案。 <br /> 副檔名：. .avhdx <br /> 預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬碟|
 |Checkpoint|檢查點會儲存在多個檢查點檔案中。 每個檢查點會建立設定檔案和執行階段狀態檔案。 <br /> 副檔名：. .vmrs 和. .vmcx <br />預設位置：C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>如果我不升級虛擬機器設定版本，會發生什麼事？

如果您有使用舊版 Hyper-v 所建立的虛擬機器，則在更新設定版本之前，較新主機 OS 上的某些可用功能可能無法與這些虛擬機器搭配使用。

作為一般指引，建議您在將虛擬化主機成功升級至較新版本的 Windows 之後更新設定版本，並確信不需要復原。 當您使用叢集[OS 輪流升級](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade)功能時，通常會在更新叢集功能等級之後進行。 如此一來，您就可以從新功能和內部變更和優化中獲益。

>[!NOTE]
>更新 VM 設定版本後，VM 將無法在不支援更新設定版本的主機上啟動。

下表顯示使用某些 Hyper-v 功能所需的最低虛擬機器設定版本。

|功能|最低 VM 設定版本|
|---|---|
|熱新增/移除記憶體|6.2|
|Linux VM 的安全開機|6.2|
|生產檢查點|6.2|
|PowerShell Direct |6.2|
|虛擬機器群組|6.2|
|虛擬信賴平台模組 (vTPM)|7.0|
|虛擬機器多個佇列（VMMQ）|7.1|
|XSAVE 支援|8.0|
|金鑰存放磁片磁碟機|8.0|
|以來賓虛擬化為基礎的安全性支援（VBS）|8.0|
|嵌套虛擬化|8.0|
|虛擬處理器計數|8.0|
|大型記憶體 Vm|8.0|
|將每個裝置的虛擬裝置的預設最大數目增加到64（例如網路和指派的裝置）|8.3|
|允許 Perfmon 的其他處理器功能|9.0|
|使用[核心](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)排程器自動公開在主機上執行之 vm 的[同步多執行緒](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background)設定|9.0|
|休眠支援|9.0|

如需這些功能的詳細資訊，請參閱[Windows Server 上的 hyper-v 的新功能](../What-s-new-in-Hyper-V-on-Windows.md)。

