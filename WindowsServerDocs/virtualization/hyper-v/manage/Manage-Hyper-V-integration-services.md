---
title: 管理 Hyper-v Integration Services
description: 描述如何開啟和關閉 integration services，並視需要加以安裝
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: bcf8109530043f5e0a6d141c484233c4364fb307
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169568"
---
>適用于： Windows 10、Windows Server 2012、Windows Server 2012R2、Windows Server 2016、Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>管理 Hyper-v Integration Services

Hyper-v Integration Services 藉由利用與 Hyper-v 主機的雙向通訊，加強虛擬機器效能並提供便利的功能。 其中許多服務都是便利的，例如來賓檔案複製，有些則對虛擬機器的功能很重要，例如綜合設備磁碟機。 這組服務和驅動程式有時稱為「整合元件」。 您可以控制個別的便利性服務是否會針對任何指定的虛擬機器運作。 驅動程式元件不適合以手動方式服務。

如需每個整合服務的詳細資訊，請參閱[hyper-v Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。

> [!IMPORTANT]
> 您要使用的每個服務都必須同時在主機和來賓中啟用，才能正常運作。 除了「Hyper-v 來賓服務介面」以外的所有整合服務，預設會在 Windows 客體作業系統上開啟。 服務可以個別開啟和關閉。 下一節將為您示範作法。

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>使用 Hyper-v 管理員開啟或關閉整合服務

1. 在中央窗格中，以滑鼠右鍵按一下虛擬機器，然後按一下 [**設定**]。
  
2. 從 [**設定**] 視窗的左窗格中，按一下 [**管理**] 底下的 [ **Integration Services**]。
  
[Integration Services] 窗格會列出 Hyper-v 主機上所有可用的整合服務，以及主機是否已啟用虛擬機器來使用它們。

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>使用 PowerShell 開啟或關閉整合服務

若要在 PowerShell 中執行這項操作，請使用[enable-vmintegrationservice](https://technet.microsoft.com/library/hh848500.aspx)和[Disable-enable-vmintegrationservice](https://technet.microsoft.com/library/hh848488.aspx)。

下列範例示範如何針對名為 "demovm" 的虛擬機器，開啟和關閉來賓檔案複製整合服務。

1. 取得執行中整合服務的清單：
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. 輸出應該看起來像這樣：

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

1. 確認已啟用 [來賓服務介面]：

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. 關閉來賓服務介面：

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>檢查來賓的 integration services 版本
某些功能可能無法正常運作，或如果來賓的整合服務不是最新的，則完全無法使用。 若要取得 Windows 的版本資訊，請登入客體作業系統，開啟命令提示字元，然後執行此命令：

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

較舊的客體作業系統將不會有所有可用的服務。 例如，Windows Server 2008 R2 來賓不能有「Hyper-v 來賓服務介面」。

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>從 Windows 來賓啟動和停止整合服務
為了讓整合服務能夠完全運作，其對應的服務必須在來賓內執行，以及在主機上啟用。 在 Windows 來賓中，每個整合服務都會列為標準 Windows 服務。 您可以使用 [控制台] 中的 [服務] 小程式或 PowerShell 來停止和啟動這些服務。

> [!IMPORTANT]
> 停止整合服務可能會嚴重影響主機管理虛擬機器的能力。 若要正確運作，您必須同時在主機和來賓上啟用每個您想要使用的整合服務。
> 最佳做法是，您應該只使用上述指示，從 Hyper-v 控制整合服務。 當您在 Hyper-v 中變更其狀態時，客體作業系統中的比對服務將會自動停止或啟動。
> 如果您在客體作業系統中啟動服務，但在 Hyper-v 中是停用的，服務將會停止。 如果您在 Hyper-v 中啟用的客體作業系統上停止服務，Hyper-v 最後會重新開機它。 如果您停用來賓中的服務，Hyper-v 將無法啟動它。

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows 服務啟動或停止 Windows 來賓中的整合服務

1. 以系統管理員身分執行 ```services.msc```，或按兩下 [控制台] 中的 [服務] 圖示，以開啟 [服務管理員]。

    ![顯示 [Windows 服務] 窗格的螢幕擷取畫面](media/HVServices.png) 

1. 尋找以 "Hyper-v" 開頭的服務。 

1. 以滑鼠右鍵按一下您要啟動或停止的服務。 按一下想要的動作。

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows PowerShell 啟動或停止 Windows 來賓內的整合服務

1. 若要取得整合服務的清單，請執行：

    ```
    Get-Service -Name vm*
    ```

1.  輸出看起來應該如下所示：

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

1. 執行 [[啟動-服務](https://technet.microsoft.com/library/hh849825.aspx)] 或 [[停止服務](https://technet.microsoft.com/library/hh849790.aspx)]。 例如，若要關閉 Windows PowerShell Direct，請執行：

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>從 Linux 來賓啟動和停止整合服務 

Linux 整合服務通常是透過 Linux 核心提供。 Linux integration services 驅動程式名為**hv_utils**。

1. 若要找出是否已載入**hv_utils** ，請使用此命令：

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. 輸出看起來應該如下所示：  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. 若要找出所需的守護程式是否正在執行，請使用此命令。
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. 輸出看起來應該如下所示： 
  
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
  
6. 輸出看起來應該如下所示：
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   可能列出的整合服務守護套裝程式括下列各項。 如果遺失，可能是您的系統不支援它們，或者它們可能尚未安裝。 尋找詳細資料，請參閱[Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](https://technet.microsoft.com/library/dn531030.aspx)。  
   - **hv_vss_daemon**：建立即時 Linux 虛擬機器備份時，必須要有此 daemon。
   - **hv_kvp_daemon**：此背景程式可讓您設定及查詢內建和外來機碼值組。
   - **hv_fcopy_daemon**：此後台處理常式會在主機和來賓之間執行檔案複製服務。  

### <a name="examples"></a>範例

這些範例示範如何停止和啟動 KVP daemon，名為 `hv_kvp_daemon`。

1. 使用處理序識別碼 \(PID\) 來停止 daemon 的進程。 若要尋找 PID，請查看輸出的第二個數據行，或使用 `pidof`。 Hyper-v 守護程式是以 root 身分執行，因此您需要根許可權。

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. 若要確認所有 `hv_kvp_daemon` 進程都已消失，請執行：

    ```
    ps -ef | hv
    ```

1. 若要再次啟動此 daemon，請以 root 身分執行背景程式：

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. 若要確認已使用新的處理序識別碼列出 `hv_kvp_daemon` 進程，請執行：

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>保持整合服務的最新狀態

我們建議您將 integration services 保持在最新狀態，以便為您的虛擬機器取得最佳效能和最新功能。 根據預設，大部分的 Windows 來賓都會發生這種情況，如果它們設定為從 Windows Update 取得重要更新。 當您更新核心時，使用目前核心的 Linux 來賓將會收到最新的整合元件。

**針對在 Windows 10/Windows Server 2016/2019 主機上執行的虛擬機器：**

> [!NOTE]
> Windows 10/Windows Server 2016/2019 上的 Hyper-v 並未隨附影像檔 vmguest.iso，因為不再需要此檔案。

| 來賓  | 更新機制 | 注意事項 |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | 需要「資料交換」整合服務。* |
| Windows 7 | Windows Update | 需要「資料交換」整合服務。* |
| Windows Vista (SP 2) | Windows Update | 需要「資料交換」整合服務。* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server 半年通道 | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | 需要「資料交換」整合服務。* |
| Windows Server 2008 R2 (SP 1) | Windows Update | 需要「資料交換」整合服務。* |
| Windows Server 2008 (SP 2) | Windows Update | 僅限 Windows Server 2016 中的擴充支援（[閱讀更多](https://support.microsoft.com/lifecycle?p1=12925)）。 |
| Windows Home Server 2011 | Windows Update | Windows Server 2016 中將不支援（[閱讀更多](https://support.microsoft.com/lifecycle?p1=15820)）。 |
| Windows Small Business Server 2011 | Windows Update | 不包含在主要支援中 ([閱讀更多](https://support.microsoft.com/lifecycle?p1=15817))。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 已內建于散發版本中，但可能有選擇性的更新可供使用。 ******** |

\* 如果無法啟用資料交換整合服務，則可以從[下載中心](https://support.microsoft.com/kb/3071740)以封包（cab）檔案的形式取得這些來賓的整合服務。 此[blog 文章](https://techcommunity.microsoft.com/t5/virtualization/integration-components-available-for-virtual-machines-not/ba-p/382247)提供套用 cab 的指示。

**針對在 Windows 8.1/Windows Server 2012R2 主機上執行的虛擬機器：**

| 來賓  | 更新機制 | 注意事項 |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows 8 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows 7 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server 半年通道 | Windows Update | |
| Windows Server 2012 R2 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2012 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 已內建于散發版本中，但可能有選擇性的更新可供使用。 ** |


**針對在 Windows 8/Windows Server 2012 主機上執行的虛擬機器：**

| 來賓  | 更新機制 | 注意事項 |
|:---------|:---------|:---------|
| Windows 8.1 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows 8 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows 7 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2012 R2 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2012 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。|
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱下面的[指示](#install-or-update-integration-services)。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用于 Linux 的 Integration services 已內建于散發版本中，但可能有選擇性的更新可供使用。 ** |

如需 Linux 來賓的詳細資訊，請參閱[支援的 linux 和 FreeBSD 虛擬機器（適用于 Windows 上的 hyper-v](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)）。

## <a name="install-or-update-integration-services"></a>安裝或更新 integration services

> [!NOTE]
> 對於早于 Windows Server 2016 和 Windows 10 的主機，您必須**手動安裝或更新**客體作業系統中的整合服務。 

手動安裝或更新整合服務的程式：

1.  開啟 [Hyper-V 管理員]。 從伺服器管理員的 [工具] 功能表中，按一下 [ **Hyper-v 管理員**]。  
  
2.  連接到虛擬機器。 以滑鼠右鍵按一下虛擬機器，然後按一下 **[連線]** 。  
  
3.  在虛擬機器連線的 [執行] 功能表中，按一下 [插入整合服務安裝磁片]。 這個動作會載入虛擬 DVD 光碟機中的安裝磁片。 視客體作業系統而定，您可能需要手動啟動安裝。  
  
4.  安裝完成後，即可使用所有的整合服務。

> [!NOTE]
> 在**線上**虛擬機器的 Windows PowerShell 會話中，**無法自動**或完成這些步驟。
> 您可以將它們套用到**離線**VHDX 映射;請參閱[如何在虛擬機器未執行時安裝 integration services](https://docs.microsoft.com/virtualization/community/team-blog/2013/20130418-how-to-install-integration-services-when-the-virtual-machine-is-not-running)。
> 您也可以透過與 Vm**連線**的**Configuration Manager**來自動化整合服務的部署，但必須在安裝結束時重新開機 vm;請參閱[使用 Config Manager 和 DISM 將 hyper-v Integration Services 部署至 vm](https://docs.microsoft.com/archive/blogs/manageabilityguys/deploying-hyper-v-integration-services-to-vms-using-config-manager-and-dism)
