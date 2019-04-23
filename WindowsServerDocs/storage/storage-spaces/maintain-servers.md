---
title: 將儲存空間直接存取伺服器離線以進行維護
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: 儲存空間直接存取, S2D, 維護
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 96ae0ad0d1def12ab68466f0a9ae60d0afcc2c17
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871219"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>將儲存空間直接存取伺服器離線以進行維護

> 適用於：Windows Server 2019，Windows Server 2016

此主題說明如何使用 [直接儲存空間](storage-spaces-direct-overview.md) 正確地重新啟動或關閉伺服器。

透過「儲存空間直接存取」將伺服器離線，也意味著將叢集中所有伺服器共用的儲存空間部分離線。 若要這樣做，您必須先暫停要離線的伺服器、將角色移至叢集中的其他伺服器，並確認叢集中其他伺服器上的所有資料皆可使用，以確保維護期間資料的安全及正常存取。

使用下列程序，在將儲存空間直接存取叢集中的伺服器離線之前，先正確將其暫停。 

   > [!IMPORTANT]
   > 若要在儲存空間直接存取叢集中安裝更新，請使用叢集感知更新 (CAU)，它會自動執行此主題中的程序，因此您不需要在安裝更新時手動執行。 如需詳細資訊，請參閱 [叢集感知更新 (CAU)](https://technet.microsoft.com/library/hh831694.aspx)。

## <a name="verifying-its-safe-to-take-the-server-offline"></a>確認伺服器是否可安全離線。

將伺服器離線以進行維護之前，確認所有磁碟區皆狀況良好。

若要這樣做，請使用系統管理權限開啟 PowerShell 工作階段，並執行下列命令以檢視磁碟區狀態︰

```PowerShell
Get-VirtualDisk 
```

此輸出可能看起來會像以下的範例：
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

確認每個磁碟區 (虛擬磁碟) 的 **\[HealthStatus\]** 屬性皆為 **\[狀況良好\]**。

若要在容錯移轉叢集管理員中執行此動作，請移至 **\[儲存\]** > **\[磁碟\]**。

確認每個磁碟區 (虛擬磁碟) 的 **\[狀態\]** 欄皆顯示 **\[線上\]**。

## <a name="pausing-and-draining-the-server"></a>暫停和清空伺服器

在重新啟動或關閉伺服器之前，先暫停和清空任何角色，例如在其中執行的虛擬機器。 這也可以讓儲存空間直接存取能正常清除和認可資料，以確保該伺服器上執行的任何應用程式皆能完全掌握關機動作。

   > [!IMPORTANT]
   > 在關閉叢集伺服器之前，請務必先將其暫停並清空。

在 PowerShell 中，(以系統管理員身分) 執行下列 Cmdlet 以將其暫停和清空。

```PowerShell
Suspend-ClusterNode -Drain
```

若要在容錯移轉叢集管理員中執行此動作，請移至 **\[節點\]**，在節點上按滑鼠右鍵，然後選取 **\[暫停\]** > **\[清空角色\]**。

![Pause-Drain](media/maintain-servers/pause-drain.png)

所有虛擬機器都將開始即時移轉至叢集中的其他伺服器。 這可能需要數分鐘。

   > [!NOTE]
   > 當您正確暫停並清空叢集結點時，Windows 會執行自動安全檢查以確保該程序可繼續執行。 如果磁碟區狀況不良，它將會停止，並提醒您繼續執行並不安全。

![Safety-Check](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>正在關閉伺服器

伺服器完成清空之後，將會在容錯移轉叢集管理員和 PowerShell 中顯示為 **\[已暫停\]**。

![已暫停](media/maintain-servers/paused.png)

您現在可以放心地像平常一樣重新啟動或將它關閉 (例如，使用 Restart-Computer 或 Stop-Computer PowerShell Cmdlet)。

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

節點正在關閉，或開始/停止叢集服務在節點上，並應該不會造成問題時，未完成 」 或 「 已降級作業的狀態是正常現象。 所有的磁碟區仍保持連線且可存取。

## <a name="resuming-the-server"></a>繼續執行伺服器

當您準備讓伺服器開始再次裝載工作負載時，請繼續執行。

在 PowerShell 中，(以系統管理員身分) 執行下列 Cmdlet 以繼續執行。

```PowerShell
Resume-ClusterNode
```

若要回復之前在此伺服器上執行的角色，請使用選擇性 **\[-Failback\]** 旗標。

```PowerShell
Resume-ClusterNode –Failback Immediate
```

若要在容錯移轉叢集管理員中執行此動作，請移至 **\[節點\]**，在節點上按滑鼠右鍵，然後選取 **\[繼續\]** > **\[容錯回復角色\]**。

![Resume-Failback](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>等候存放裝置重新同步

當伺服器恢復時，無法使用時所發生的任何新寫入需要重新同步處理。 此動作會自動執行。 使用智慧型變更追蹤時，您不需要掃描或同步處理*所有*資料，只需要針對變更部分執行這些動作。 此程序會進行調整以減少對於生產工作負載造成的影響。 根據暫停時間的長短以及寫入的新資料數量，系統可能需要數分鐘完成。

您必須等候重新同步完成才能將從叢集中的其他伺服器離線。

在 PowerShell 中，(以系統管理員身分) 執行下列 Cmdlet 以監視進度。

```PowerShell
Get-StorageJob
```
以下是一些顯示重新同步 (維修) 工作的輸出範例︰
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

**BytesTotal** 顯示需要重新同步多少儲存空間。 **PercentComplete** 顯示進度。

   > [!WARNING]
   > 請務必等候這些修復工作完成後再將另一部伺服器離線。

在此期間，您的磁碟區將繼續顯示為 **\[Warning\]**，此為正常現象。 

例如，如果您使用 `Get-VirtualDisk` Cmdlet，您可能會看到以下輸出︰
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

一旦工作完成，請使用 `Get-VirtualDisk` Cmdlet 以再次確認磁碟區顯示為 **\[Healthy\]**。 以下是一些輸出範例︰

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

您現在可以放心地暫停並重新啟動叢集中的其他伺服器。

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>如何更新儲存空間直接存取節點離線
快速使用下列步驟，以路徑儲存空間直接存取系統。 它包含排程的維護期間和關閉系統進行修補。 如果沒有快速地套用您所需要的重大安全性更新，或您可能需要確保修補完成維護視窗中，這個方法可能適合您。 此程序會關閉儲存空間直接存取叢集、 修補程式，並註冊一次將它帶。 缺點是裝載的資源的停機時間。

1. 規劃維護期間。
2. 將虛擬磁碟離線。
3. 停止叢集，以讓存放集區離線。 執行**停止叢集**cmdlet 或使用容錯移轉叢集管理員來停止叢集。
4. 將叢集服務設定為**已停用**中每個節點上的 Services.msc。 這可防止叢集服務啟動時所修正的項目。
5. 適用於 Windows Server 累計更新以及任何必要的所有節點的服務堆疊更新。 （您可以在此同時，等候叢集已關閉，因為不需要更新所有節點）。  
6. 重新啟動節點，並確定一切都沒問題。
7. 設定叢集服務將會回到**自動**每個節點上。
8. 啟動叢集。 執行**啟動叢集**cmdlet 或使用 [容錯移轉叢集管理員]。 

   稍候幾分鐘。  請確定儲存體集區的狀況良好。
9. 將帶上線的虛擬磁碟。
10. 監視執行的虛擬磁碟的狀態**Get-volume**並**Get-virtualdisk** cmdlet。


## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [叢集感知更新 (CAU)](https://technet.microsoft.com/library/hh831694.aspx)
