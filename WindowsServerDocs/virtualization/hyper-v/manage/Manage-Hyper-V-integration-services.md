---
title: 管理 Hyper-v Integration Services
description: 描述如何開啟和關閉 integration services，並視需要進行安裝
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/20/2016
ms.topic: article
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: f82ccc4e6dc2dbd7a34d829c0bc753fc6533f8dd
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866107"
---
# <a name="manage-hyper-v-integration-services"></a>管理 Hyper-v Integration Services

> 適用于： Windows 10、Windows Server 2012、Windows Server 2012R2、Windows Server 2016、Windows Server 2019

Hyper-v Integration Services 利用與 Hyper-v 主機之間的雙向通訊，增強虛擬機器的效能，並提供便利的功能。 其中許多服務都是便利性的，例如來賓檔案複製，有些則對虛擬機器的功能很重要，例如綜合設備磁碟機。 這組服務和驅動程式有時稱為「整合元件」。 您可以控制是否要針對任何指定的虛擬機器操作個別的便利性服務。 驅動程式元件無法以手動方式服務。

如需每個整合服務的詳細資訊，請參閱 [hyper-v Integration Services](/virtualization/hyper-v-on-windows/reference/integration-services)。

> [!IMPORTANT]
> 您要使用的每個服務都必須同時在主機和來賓中啟用，才能運作。 除了 "Hyper-V 客體服務介面" 以外的所有整合服務預設都在 Windows 客體作業系統上開啟。 服務可以個別開啟和關閉。 下一節將為您示範作法。

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>使用 Hyper-v 管理員開啟或關閉整合服務

1. 在中央窗格中，以滑鼠右鍵按一下虛擬機器，然後按一下 [ **設定**]。

2. 在 [ **設定** ] 視窗的左窗格中，按一下 [ **管理**] 底下的 [ **Integration Services**]。

[Integration Services] 窗格會列出 Hyper-v 主機上所有可用的整合服務，以及主機是否已啟用虛擬機器來使用它們。

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>使用 PowerShell 開啟或關閉整合服務

若要在 PowerShell 中執行這項操作，請使用 [get-vmintegrationservice](/powershell/module/hyper-v/enable-vmintegrationservice) 和 [停用-get-vmintegrationservice](/powershell/module/hyper-v/disable-vmintegrationservice)。

下列範例示範如何開啟和關閉名為 "demovm" 之虛擬機器的來賓檔案複製整合服務。

1. 取得正在執行的 integration services 清單：

    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. 輸出看起來應該會像下面這樣：

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. 開啟來賓服務介面：

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. 確認已啟用 Guest 服務介面：

   ```
   Get-VMIntegrationService -VMName "DemoVM"
   ```

1. 關閉來賓服務介面：

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```

## <a name="checking-the-guests-integration-services-version"></a>檢查來賓的 integration services 版本
某些功能可能無法正常運作，或在來賓的 integration services 不是最新的情況時進行。 若要取得 Windows 的版本資訊，請登入客體作業系統，開啟命令提示字元，然後執行此命令：

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

舊版的客體作業系統將不會有所有可用的服務。 例如，Windows Server 2008 R2 來賓不能有「Hyper-V 客體服務介面」。

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>從 Windows 來賓啟動及停止整合服務
為了讓整合服務能夠完全正常運作，除了在主機上啟用之外，它的對應服務還必須在來賓內執行。 在 Windows 來賓中，每個整合服務都會列為標準的 Windows 服務。 您可以使用主控台或 PowerShell 中的 [服務] 小程式來停止和啟動這些服務。

> [!IMPORTANT]
> 停止整合服務可能會嚴重影響主機管理虛擬機器的能力。 若要正確運作，您必須同時在主機和來賓上啟用您想要使用的每個整合服務。
> 基於最佳作法，您應該只使用上述指示，從 Hyper-v 控制 integration services。 當您在 Hyper-v 中變更其狀態時，客體作業系統中的比對服務將會自動停止或啟動。
> 如果您在客體作業系統中啟動服務，但在 Hyper-v 中停用該服務，服務將會停止。 如果您在 Hyper-v 中啟用的客體作業系統中停止服務，Hyper-v 最終將會重新開機它。 如果您停用來賓中的服務，Hyper-v 將無法啟動它。

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows 服務啟動或停止 Windows 來賓內的整合服務

1. ```services.msc```以系統管理員身分執行，或按兩下主控台中的 [服務] 圖示，以開啟 [服務管理員]。

    ![顯示 [Windows 服務] 窗格的螢幕擷取畫面](media/HVServices.png)

1. 尋找以 "Hyper-v" 開頭的服務。

1. 以滑鼠右鍵按一下您要啟動或停止的服務。 按一下所需的動作。

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows PowerShell 來啟動或停止 Windows 來賓內的整合服務

1. 若要取得 integration services 清單，請執行：

    ```
    Get-Service -Name vm*
    ```

1.  輸出應類似以下的內容：

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. 執行 [啟動服務](/powershell/module/microsoft.powershell.management/start-service?view=powershell-7) 或 [停止服務](/powershell/module/microsoft.powershell.management/stop-service?view=powershell-7)。 例如，若要關閉 Windows PowerShell Direct，請執行：

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>從 Linux 來賓啟動及停止整合服務

Linux 整合服務通常是透過 Linux 核心提供。 Linux integration services 驅動程式的名稱為 **hv_utils**。

1. 若要找出 **hv_utils** 是否已載入，請使用此命令：

   ``` BASH
   lsmod | grep hv_utils
   ```

2. 輸出應類似以下的內容：

    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. 若要瞭解所需的守護程式是否正在執行，請使用此命令。

    ``` BASH
    ps -ef | grep hv
    ```

4. 輸出應類似以下的內容：

    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv
    ```

5. 若要查看可使用哪些精靈，請執行：

    ``` BASH
    compgen -c hv_
    ```

6. 輸出應類似以下的內容：

    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon
    ```

   可能列出的整合服務守護套裝程式括下列各項。 如果有任何遺漏，可能是您的系統不支援或未安裝。 尋找詳細資料，請參閱 [Windows 上適用于 hyper-v 的支援 Linux 和 FreeBSD 虛擬機器](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)。
   - **hv_vss_daemon**：需要此 daemon 才能建立即時的 Linux 虛擬機器備份。
   - **hv_kvp_daemon**：此背景程式允許設定和查詢內建和內建的索引鍵值組。
   - **hv_fcopy_daemon**：此 daemon 會在主機與來賓之間執行檔案複製服務。

### <a name="examples"></a>範例

這些範例示範如何停止和啟動 KVP daemon，名為 `hv_kvp_daemon` 。

1. 使用處理序識別碼 \( PID \) 來停止 daemon 的進程。 若要尋找 PID，請查看輸出的第二個數據行，或使用 `pidof` 。 Hyper-v 守護程式以 root 的形式執行，因此您需要根許可權。

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. 若要確認所有 `hv_kvp_daemon` 進程都已消失，請執行：

    ```
    ps -ef | hv
    ```

1. 若要重新開機此 daemon，請以 root 的身份執行 daemon：

    ``` BASH
    sudo hv_kvp_daemon
    ```

1. 若要確認 `hv_kvp_daemon` 進程是否以新的處理序識別碼列出，請執行：

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>將 integration services 保持在最新狀態

建議您將整合服務保持在最新狀態，以取得最佳效能，以及最新的虛擬機器功能。 如果大部分的 Windows 來賓設定為從 Windows Update 取得重要的更新，則會發生這種情況。 使用目前核心的 Linux 來賓會在您更新核心時收到最新的整合元件。

**針對 Windows 10/Windows Server 2016/2019 主機上執行的虛擬機器：**

> [!NOTE]
> Windows 10/Windows Server 2016/2019 上的 Hyper-v 未隨附影像檔案 vmguest.iso，因為不再需要該檔案。

| 來賓  | 更新機制 | 注意 |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | 需要資料交換整合服務。* |
| Windows 7 | Windows Update | 需要資料交換整合服務。* |
| Windows Vista (SP 2) | Windows Update | 需要資料交換整合服務。* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server 半年通道 | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | 需要資料交換整合服務。* |
| Windows Server 2008 R2 (SP 1) | Windows Update | 需要資料交換整合服務。* |
| Windows Server 2008 (SP 2) | Windows Update | 僅限 Windows Server 2016 中的延伸支援 ([進一步瞭解](https://support.microsoft.com/lifecycle?p1=12925)) 。 |
| Windows Home Server 2011 | Windows Update | 在 Windows Server 2016 中將不受支援 ([閱讀更多](https://support.microsoft.com/lifecycle?p1=15820)) 。 |
| Windows Small Business Server 2011 | Windows Update | 不在主流支援下 ([進一步瞭解](https://support.microsoft.com/lifecycle?p1=15817)) 。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 內建在發行版本中，但可能有選用的更新可用。 ******** |

\* 如果無法啟用「資料交換」整合服務，則可從 [下載中心](https://support.microsoft.com/kb/3071740) 取得這些來賓的整合服務，做為封包 (cab) 檔。 此 [blog 文章](https://techcommunity.microsoft.com/t5/virtualization/integration-components-available-for-virtual-machines-not/ba-p/382247)中有套用 cab 的指示。

**針對 Windows 8.1/Windows Server 2012R2 主機上執行的虛擬機器：**

| 來賓  | 更新機制 | 注意 |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows 8 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows 7 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server 半年通道 | Windows Update | |
| Windows Server 2012 R2 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2012 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 內建在發行版本中，但可能有選用的更新可用。 ** |


**針對 Windows 8/Windows Server 2012 主機上執行的虛擬機器：**

| 來賓  | 更新機制 | 注意 |
|:---------|:---------|:---------|
| Windows 8.1 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows 8 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows 7 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2012 R2 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2012 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。|
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱下方的 [指示](#install-or-update-integration-services)。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 內建在發行版本中，但可能有選用的更新可用。 ** |

如需 Linux 來賓的詳細資訊，請參閱 [Windows 上適用于 hyper-v 的支援 Linux 和 FreeBSD 虛擬機器](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)。

## <a name="install-or-update-integration-services"></a>安裝或更新 integration services

> [!NOTE]
> 若是早于 Windows Server 2016 和 Windows 10 的主機，您必須在客體作業系統中 **手動安裝或更新** integration services。

手動安裝或更新 integration services 的程式：

1.  開啟 Hyper-V 管理員。 從伺服器管理員的 [工具] 功能表中，按一下 [ **Hyper-v 管理員**]。

2.  連接至虛擬機器。 以滑鼠右鍵按一下虛擬機器，然後按一下 **[連線]**。

3.  在虛擬機器連線的 [執行] 功能表中，按一下 [插入整合服務安裝磁片]。 這個動作會載入虛擬 DVD 光碟機中的安裝磁片。 視客體作業系統而定，您可能需要手動開始安裝。

4.  安裝完成後，即可使用所有的整合服務。

> [!NOTE]
> 這些步驟 **無法** 在 **線上** 虛擬機器的 Windows PowerShell 會話內自動執行或完成。
> 您可以將它們套用至 **離線** VHDX 映射; [當虛擬機器未執行時，請參閱如何安裝 integration services](/virtualization/community/team-blog/2013/20130418-how-to-install-integration-services-when-the-virtual-machine-is-not-running)。
> 您也可以透過 **Configuration Manager** 與 vm **連線**，將 integration services 的部署自動化，但您必須在安裝結束時重新開機 vm;請參閱 [使用 Config Manager 和 DISM 將 hyper-v Integration Services 部署至 vm](/archive/blogs/manageabilityguys/deploying-hyper-v-integration-services-to-vms-using-config-manager-and-dism)
