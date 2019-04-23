---
title: 管理 HYPER-V 整合服務
description: 說明如何開啟和關閉的整合服務，並視需要加以安裝
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: b049efc61d5060791574f20fcdd8b369a26f0507
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890249"
---
>適用於：Windows 10、windows Server 2016 中，Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>管理 HYPER-V 整合服務

HYPER-V 整合服務增強虛擬機器效能，並提供利用與 HYPER-V 主機的雙向通訊的便利功能。 其中許多服務是一項便利措施，例如客體檔案複製，有些則是重要虛擬機器的功能，例如綜合裝置驅動程式。 這組服務與驅動程式有時稱為 「 整合元件 」。 您可以控制個別方便服務運作為任何指定的虛擬機器。 驅動程式元件不打算以手動方式提供服務。

如需每個整合服務的詳細資訊，請參閱[HYPER-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。

>[!IMPORTANT]
>您想要使用每個的服務必須能夠在主機和客體中才能運作。 Windows 客體作業系統預設會開啟 「 HYPER-V 客體服務介面 」 以外的所有整合服務。 服務可以開啟或關閉個別。 下一節中將會告訴您作法。

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>開啟或關閉使用 HYPER-V 管理員的整合服務

1. 從中間窗格中，以滑鼠右鍵按一下 虛擬機器，然後按一下**設定**。
  
2. 從左窗格**設定**] 視窗底下**管理**，按一下 [ **Integration Services**。
  
[Integration Services] 窗格會列出所有可用 HYPER-V 主機上的 integration services 和主機是否已啟用虛擬機器，以使用它們。

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>開啟或關閉使用 PowerShell 的整合服務

若要在 PowerShell 中這麼做，請使用[Enable-vmintegrationservice](https://technet.microsoft.com/library/hh848500.aspx)並[Disable-vmintegrationservice](https://technet.microsoft.com/library/hh848488.aspx)。

下列範例會示範開啟客體檔案複製整合服務開啟和關閉虛擬機器名為"demovm"。

1. 取得一份執行 integration services:
  
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

1. 開啟 客體服務介面：

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. 確認已啟用 客體服務介面：

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. 關閉客體服務介面：

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>正在檢查客體的整合服務版本
某些功能可能無法運作正確，或完全，如果不是目前的客體整合服務。 若要取得的版本資訊的 Windows 登入客體作業系統中，開啟命令提示字元中，並執行此命令：

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```
舊版客體作業系統不會有所有可用的服務。 比方說，Windows Server 2008 R2 客體不能有 「 HYPER-V 客體服務介面 」。

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>啟動和停止 Windows 客體整合服務
為了讓是功能完整的整合服務，其對應的服務必須執行內除了要在主機上啟用來賓。 在 Windows 客體，每個整合服務會列為標準的 Windows 服務。 您可以使用 [服務] 小程式，在 [控制台] 或 PowerShell 來停止和啟動這些服務。

>[!IMPORTANT]
> 停止的整合服務，可能會嚴重影響主機的能力來管理您的虛擬機器。 正常運作，則必須啟用主機和客體上的每個您想要使用的整合服務。
> 最佳做法，您應該只會控制整合服務，從 HYPER-V 使用上述指示。 在客體作業系統中相符的服務將會停止，或自動啟動，當您變更其狀態在 HYPER-V 中。
> 如果您在客體作業系統中啟動服務，但它已停用 HYPER-V 中，服務會停止。 如果您停止在 HYPER-V 中啟用客體作業系統中的服務時，HYPER-V 會最後會重新啟動它。 如果您停用客體中的服務時，HYPER-V 將無法啟動它。

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows 服務來啟動或停止在 Windows 客體整合服務

1. 開啟 [服務管理員] 執行```services.msc```身為系統管理員，或按兩下控制台中的 [服務] 圖示。

    ![螢幕擷取畫面顯示 [Windows 服務] 窗格](media/HVServices.png) 

1. 尋找開頭為 「 HYPER-V 」 的服務。 

1. 以滑鼠右鍵按一下您想要啟動或停止服務。 按一下所需的動作。


### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows PowerShell 來啟動或停止在 Windows 客體整合服務

1. 若要取得一份整合服務，請執行：

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

1. 執行下列其中一[Start-service](https://technet.microsoft.com/library/hh849825.aspx)或是[Stop-service](https://technet.microsoft.com/library/hh849790.aspx)。 例如，若要關閉 Windows PowerShell Direct，請執行：

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>啟動和停止從 Linux 客體的整合服務 

Linux 整合服務通常是透過 Linux 核心提供。 為 Linux 整合服務驅動**hv_utils**。

1.  若要找出是否**hv_utils**載入時，使用下列命令：

    ``` BASH
    lsmod | grep hv_utils
    ``` 
  
1. 輸出看起來應該如下所示：  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

1. 若要找出必要的精靈是否正在執行，使用此命令。
  
    ``` BASH
    ps -ef | grep hv
    ```
  
1. 輸出看起來應該如下所示： 
  
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

1. 若要查看可使用哪些精靈，請執行：

    ``` BASH
    compgen -c hv_
    ```
  
1. 輸出看起來應該如下所示：
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
 下面是可能會列出的整合服務精靈。 如果遺漏任何，可能不支援您的系統上，或可能不會安裝。 找到詳細資訊，請參閱 <<c0> [ 支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](https://technet.microsoft.com/library/dn531030.aspx)。  
  - **hv_vss_daemon**:若要建立即時 Linux 虛擬機器備份需要此精靈。
  - **hv_kvp_daemon**:此精靈可用於設定及查詢內建與外來的機碼值組。
  - **hv_fcopy_daemon**:此精靈可實作檔案，複製在主機與客體之間的服務。  

### <a name="examples"></a>範例

這些範例示範如何停止和啟動 KVP 精靈中，名為`hv_kvp_daemon`。

1. 使用的處理序識別碼\(PID\)停止精靈的程序。 若要尋找的 PID，看看第二個資料行的輸出，或使用`pidof`。 HYPER-V 精靈會執行為根，因此您需要根權限的範圍。

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. 若要確認所有`hv_kvp_daemon`程序都會消失，執行：

    ```
    ps -ef | hv
    ```

1. 若要再次啟動精靈，請以 root 身分執行精靈：

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. 若要確認`hv_kvp_daemon`程序會列出新的處理序識別碼，執行：

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>將 integration services 保持在最新狀態

我們建議您保留 integration services 以取得最佳的效能和最新的功能，您的虛擬機器的最新狀態。 發生這種情況的大部分的 Windows 客體預設如果它們設定為從 Windows Update 取得重要的更新。 當您更新核心時，使用目前的核心的 Linux 客體將會收到最新的整合元件。

**在 Windows 10 主機上執行的虛擬機器：**

> [!NOTE]
> 因為不再需要時，不隨附在 Windows 10 上的 HYPER-V 映像檔案 vmguest.iso。

| Guest  | 更新機制 | 附註 |
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
| Windows Server 2008 (SP 2) | Windows Update | 延伸支援僅限於 Windows Server 2016 ([閱讀更多](https://support.microsoft.com/lifecycle?p1=12925))。 |
| Windows Home Server 2011 | Windows Update | 不支援在 Windows Server 2016 ([閱讀更多](https://support.microsoft.com/lifecycle?p1=15820))。 |
| Windows Small Business Server 2011 | Windows Update | 不包含在主要支援中 ([閱讀更多](https://support.microsoft.com/lifecycle?p1=15817))。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用於 Linux 的整合服務會內建在 distro，但可能有選用的更新可用。 ******** |

\* 如果無法啟用資料交換整合服務，取得這些客體整合服務都是從[下載中心](https://support.microsoft.com/kb/3071740)為封包 (cab) 檔案。 在此有套用 cab 的指示[部落格文章](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx)。

**在 Windows 8.1 主機上執行虛擬機器：**

| Guest  | 更新機制 | 附註 |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows 7 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server 半年通道 | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用於 Linux 的整合服務會內建在 distro，但可能有選用的更新可用。 ** |


**在 Windows 8 主機上執行虛擬機器：**

| Guest  | 更新機制 | 附註 |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows 7 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Vista (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows XP (SP 2、SP 3) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2008 R2 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。|
| Windows Server 2008 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Home Server 2011 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Small Business Server 2011 | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2003 R2 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| Windows Server 2003 (SP 2) | 整合服務光碟 | 請參閱[指示](#install-or-update-integration-services)底下。 |
| - | | |
| Linux 客體 | 封裝管理員 | 適用於 Linux 的整合服務會內建在 distro，但可能有選用的更新可用。 ** |

如需 Linux 客體的更多詳細資訊，請參閱 <<c0> [ 支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。

## <a name="install-or-update-integration-services"></a>安裝或更新整合服務

主機早於 Windows Server 2016 和 Windows 10 中，您必須手動安裝或更新客體作業系統的整合服務。 
  
1.  開啟 \[Hyper-V 管理員\]。 從 工具 功能表的 伺服器管理員中，按一下**HYPER-V 管理員**。  
  
2.  連線到虛擬機器。 以滑鼠右鍵按一下 虛擬機器，然後按一下  **Connect**。  
  
3.  在 [虛擬機器連線] 的 [執行] 功能表中，按一下 [插入整合服務安裝磁片]。 這個動作會載入虛擬 DVD 光碟機中的安裝磁片。 視客體作業系統中，您可能需要手動啟動安裝。  
  
4.  安裝完成後，即可使用所有的整合服務。

這些步驟無法自動完成或在線上的虛擬機器的 Windows PowerShell 工作階段內完成。 您可以將它們套用到離線的 VHDX 映像;[請參閱此部落格文章](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/)。
