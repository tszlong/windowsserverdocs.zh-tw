---
title: 在 Windows 10 或 Windows Server 上的 HYPER-V 中的虛擬機器版本升級
description: 提供指示和升級虛擬機器的版本時的考量
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 160adc0e838cb732ba792cbdd7fd9fa200c68794
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810506"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>在 Windows 10 或 Windows Server 上的 HYPER-V 中的虛擬機器版本升級

>適用於：Windows 10、windows Server 2019，Windows Server 2016、windows Server （半年通道）

最新的 HYPER-V 功能在您的虛擬機器上藉由提供升級設定版本。 不這麼做之前：

- 您升級至最新版的 Windows 或 Windows Server HYPER-V 主機。
- 您升級叢集功能等級。
- 您確定，您不需要將虛擬機器移回執行舊版 Windows 或 Windows Server 的 HYPER-V 主機。

如需詳細資訊，請參閱 <<c0> [ 叢集作業系統輪流升級](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)並[在 VMM 中執行的 HYPER-V 主機叢集的輪流升級](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade)。

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>步驟 1：檢查虛擬機器設定版本

1. 在 Windows 桌面上，按一下 \[開始\] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。
2. 以滑鼠右鍵按一下 Windows PowerShell，然後選取**系統管理員身分執行**。
3. 使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet。 執行下列命令，以取得您的虛擬機器的版本。

```PowerShell
Get-VM * | Format-Table Name, Version
```

您也可以查看設定版本，在 [HYPER-V 管理員選取虛擬機器，然後看看**摘要**] 索引標籤。

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>步驟 2：升級虛擬機器設定版本

1. 關閉 HYPER-V 管理員中的虛擬機器。
2. 選取動作 > 升級設定版本。 如果此選項不適用於虛擬機器，則它已被最高組態版本所支援的 HYPER-V 主機。

若要使用 Windows PowerShell 來升級虛擬機器設定版本，請使用[更新 VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet。 執行下列命令，其中 vmname 是虛擬機器的名稱。

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>支援的虛擬機器設定版本

執行 PowerShell cmdlet [Get VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion)以查看您的 HYPER-V 主機支援哪些虛擬機器設定版本。 當您建立虛擬機器時，它會建立預設設定版本。 若要查看預設值，執行下列命令。

```PowerShell
Get-VMHostSupportedVersion -Default
```

如果您要建立虛擬機器，您可以移至 HYPER-V 主機執行較舊版本的 Windows，使用[NEW-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet 與-version 參數。 例如，若要建立虛擬機器，您可以移至執行 Windows Server 2012 R2 的 HYPER-V 主機，請執行下列命令。 此命令會建立名為"WindowsCV5 」 組態版本 5.0。

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>您可以匯入已建立 HYPER-V 主機執行較舊版本的 Windows 虛擬機器，或從備份還原它們。 如果未列出的 VM 設定版本，因為您在下表中的 HYPER-V 主機作業系統支援，您必須更新 VM 設定版本，才能啟動 VM。

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>支援的長期服務主機的 VM 組態版本

下表列出支援執行長期的服務版本的 Windows 主機的 VM 組態版本。

| HYPER-V 主機的 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企業版 2016 長期維護|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>半年度通道主機支援的 VM 組態版本

下表列出執行目前支援的半年度通道版本的 Windows 主機上的 VM 組態版本。 若要取得在半年度的通道版本的 Windows 上的詳細資訊，請瀏覽下列頁面[Windows Server](../../../get-started-19/servicing-channels-19.md)和[Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| HYPER-V 主機的 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 可能 2019年更新 （版本 1903年） |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server 版 1903， |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server 版 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 年 10 月 2018 Update （版本 1809年）|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 版本 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 April 2018 Update （版本 1803年）|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update （1709年版）|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update （1703年版）|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 年度更新版 （版本 1607年）|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>為什麼應該升級虛擬機器設定版本？

當您移動或匯入到 Windows Server 2019、 Windows Server 2016 或 Windows 10 中，虛擬機器執行 HYPER-V 的電腦的虛擬機器 」 的組態不會自動更新。 這表示，您可以回到虛擬機器執行舊版 Windows 或 Windows Server 的 HYPER-V 主機。 但是，這也表示您無法使用的一些新的虛擬機器功能直到您手動更新組態版本。 已升級之後，您無法降級虛擬機器設定版本。

將虛擬機器設定版本代表虛擬機器的組態，以儲存狀態，以及快照集檔案的 HYPER-V 版本的相容性。 當您更新的設定版本時，您可以變更用來儲存虛擬機器組態和檢查點檔案的檔案結構。 您也會更新至最新版的 HYPER-V 主機所支援的設定版本。 升級後的虛擬機器會使用新的設定檔案格式，這種格式是為了提高讀取和寫入虛擬機器設定資料的效率而設計。 升級也會減少儲存體失敗時的資料損毀可能性。

下表列出描述、 檔案名稱副檔名，以及每個用於新的或升級虛擬機器的檔案類型的預設位置。

 |虛擬機器的檔案類型 | 描述|
 |---|---|
|組態 |儲存在二進位檔案格式的虛擬機器組態資訊。 <br /> 檔案名稱副檔名：.vmcx <br /> 預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |執行階段狀態|以二進位檔案格式儲存虛擬機器執行階段狀態資訊。 <br />檔案名稱副檔名： 為.vmrs 和.vmgs <br />預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|虛擬硬碟|儲存虛擬機器的虛擬硬碟。 <br /> 檔案名稱副檔名：.vhd 或.vhdx <br />預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |自動虛擬硬碟 |用於虛擬機器檢查點的差異磁碟檔案。 <br /> 檔案名稱副檔名：.avhdx <br /> 預設位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Checkpoint|檢查點會儲存在多個檢查點檔案中。 每個檢查點會建立設定檔案和執行階段狀態檔案。 <br /> 檔案名稱副檔名： 為.vmrs 和.vmcx <br />預設位置：C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>如果不升級虛擬機器設定版本，則會發生什麼事？

如果您有使用舊版的 HYPER-V 建立虛擬機器時，某些功能，可用於較新 OS 可能不適用於這些虛擬機器，直到更新設定版本的主機上。

一般來說，我們建議您更新的設定版本，一旦您已成功升級至較新版本的 Windows 虛擬化主機，並確信您不需要回復。 當您使用[叢集 OS 輪流升級](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade)功能，這通常是在更新叢集功能等級之後。 如此一來，您會受益的新功能和內部的變更以及最佳化。

>[!NOTE]
>VM 設定版本更新之後，VM 將無法在不支援更新的設定版本的主機上啟動。

下表顯示使用 HYPER-V 的功能所需的最小的虛擬機器設定版本。

|功能|最小的 VM 設定版本|
|---|---|
|熱新增/移除記憶體|6.2|
|Linux VM 的安全開機|6.2|
|生產檢查點|6.2|
|PowerShell Direct |6.2|
|虛擬機器群組|6.2|
|虛擬信賴平台模組 (vTPM)|7.0|
|虛擬機器的多個佇列 (VMMQ)|7.1|
|XSAVE 支援|8.0|
|金鑰的儲存體磁碟機|8.0|
|來賓虛擬化型安全性支援 (VBS)|8.0|
|巢狀虛擬化|8.0|
|虛擬處理器計數|8.0|
|大量記憶體的 Vm|8.0|
|增加預設數目的上限為 64，每個裝置 （例如網路並成功指派的裝置） 的虛擬裝置|8.3|
|Perfmon 允許額外的處理器功能|9.0|
|自動公開[同時進行多執行緒處理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background)使用的主機上執行的 Vm 組態[核心排程器](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|休眠支援|9.0|

如需這些功能的詳細資訊，請參閱[的 Windows Server 上的 HYPER-V 新功能](../What-s-new-in-Hyper-V-on-Windows.md)。

