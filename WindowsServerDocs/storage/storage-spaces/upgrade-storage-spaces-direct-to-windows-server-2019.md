---
title: 將儲存空間直接存取叢集升級至 Windows Server 2019
description: 如何將儲存空間直接存取叢集升級至 Windows Server 2019-不論 Vm 是在執行中或在停止的狀態下執行。
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9966ee0fd3c0a2d1df0180bf177dec03343efc14
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867184"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>將儲存空間直接存取叢集升級至 Windows Server 2019

本主題說明如何將儲存空間直接存取叢集升級至 Windows Server 2019。 有四種方法可將儲存空間直接存取叢集從 Windows Server 2016 升級至 Windows Server 2019，使用叢集[OS 輪流升級](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)程式，這兩個步驟會牽涉到執行中的 vm，另外兩個則牽涉到停止所有 vm。 每種方法都有不同的優點和缺點，因此請選取最符合您組織需求的方式：

- 就地升級：在叢集中的每部伺服器上執行**vm 時**，此選項不會產生任何 VM 停機時間，但您必須等候儲存體作業（鏡像修復）在每部伺服器升級後完成。

- 當 Vm 在叢集中的每部伺服器上執行**時，會進行全新的作業系統安裝**—此選項不會產生任何 VM 停機時間，但您必須等到每部伺服器升級之後，才能完成儲存體作業（鏡像修復），而且您必須設定每部伺服器及其所有應用程式和角色。

- 就地升級：在叢集中的每部伺服器上**停止 vm 時**，此選項會造成 VM 停機，但您不需要等候儲存體作業（鏡像修復），因此速度會更快。

- 清理-在叢集中的每部伺服器上**停止 vm 時安裝**：此選項會導致 VM 停機，但您不需要等候儲存體作業（鏡像修復），因此速度會更快。

## <a name="prerequisites-and-limitations"></a>必要條件和限制

繼續進行升級之前：

- 如果升級程式期間發生任何問題，請檢查您是否有可用的備份。

- 檢查您的硬體廠商是否有適用于您伺服器的 BIOS、固件和驅動程式，可在 Windows Server 2019 上支援。

要注意的升級程式有一些限制：

- 若要啟用2019版176693.292 之前的 Windows Server 組建儲存空間直接存取，客戶可能需要聯繫 Microsoft 支援服務，以取得可啟用儲存空間直接存取和軟體定義網路功能的登錄機碼。 如需詳細資訊，請參閱 Microsoft 知識庫[文章 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)。

- 升級具有 ReFS 磁片區的叢集時，有幾個限制：

- ReFS 磁片區上的升級受到完整支援，不過，升級的磁片區不能受益于 Windows Server 2019 中的 ReFS 增強功能。 這些優點（例如，鏡像加速同位的增加效能）需要新建立的 Windows Server 2019 ReFS 磁片區。 換句話說，您必須使用`New-Volume` Cmdlet 或伺服器管理員建立新的磁片區。 以下是一些新磁片區會取得的 ReFS 增強功能：

    - **對應記錄檔略過**： ReFS 中的效能改善僅適用于叢集（儲存空間直接存取）系統，不適用於獨立儲存集區。

    - **壓縮**Windows Server 2019 中多個復原磁片區特有的效率改良功能。

- 升級 Windows Server 2016 儲存空間直接存取叢集伺服器之前，建議您將伺服器置於儲存體維護模式。 如需詳細資訊，請參閱[疑難排解儲存空間直接存取](troubleshooting-storage-spaces.md)的事件5120一節。 雖然 Windows Server 2016 已修正此問題，但建議您最好在升級期間將每部儲存空間直接存取伺服器放入儲存體維護模式。

- 使用 SET 參數的軟體定義網路環境有已知的問題。 此問題牽涉到從 Windows Server 2019 到 Windows Server 2016 的 Hyper-v VM 即時移轉（即時移轉至舊版作業系統）。 為確保成功的即時移轉，建議您在從 Windows Server 2019 即時移轉到 Windows Server 2016 的 Vm 上，變更 VM 網路設定。 此問題已針對 2019-01D 修補匯總套件中的 Windows Server 2019 （也稱為組建17763.292）修正。 如需詳細資訊，請參閱 Microsoft 知識庫[文章 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)。

由於上述已知問題，有些客戶可能會考慮建立新的 Windows Server 2019 叢集並從舊叢集複製資料，而不是使用下列四個程式的其中一個來升級其 Windows Server 2016 叢集。

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>執行 Vm 時的就地升級

此選項不會產生任何 VM 停機時間，但您必須等候存放裝置作業（鏡像修復）在每部伺服器升級後完成。 雖然在升級程式期間會依序重新開機個別伺服器，但叢集中的其餘伺服器和所有 Vm 都將維持執行狀態。

1. 檢查叢集中的所有伺服器是否已安裝最新的 Windows 更新。 如需詳細資訊，請參閱[windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安裝 Microsoft 知識庫[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2019年2月19日）。 組建編號（請參閱`ver`命令）應該是14393.2828 或更高版本。

2. 如果您使用已設定參數的軟體定義網路功能，請開啟已提升許可權的 PowerShell 會話，然後執行下列命令以停用叢集上所有 Vm 的 VM 即時移轉驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次在一部叢集伺服器上執行下列步驟：

   1. 使用 Hyper-v VM 即時移轉，將執行中的 Vm 移出您即將升級的伺服器。

   2. 使用下列 PowerShell 命令暫停叢集伺服器-請注意，部分內部群組是隱藏的。 我們建議您務必小心，如果您尚未在伺服器上即時移轉 Vm，此 Cmdlet 會為您執行此動作，因此您可以視需要略過上一個步驟。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 藉由執行下列 PowerShell 命令，將伺服器置於儲存體維護模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 執行下列 Cmdlet 來檢查**OperationalStatus**值是否處於**維護模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. 執行**setup.exe**並使用 [保留個人檔案和應用程式] 選項，在伺服器上執行 Windows Server 2019 的升級安裝。 安裝完成之後，伺服器會保留在叢集中，而且叢集服務會自動啟動。

   6. 檢查新升級的伺服器是否有最新的 Windows Server 2019 更新。 如需詳細資訊，請參閱[windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。 組建編號（請參閱`ver`命令）應該是17763.292 或更高版本。

   7. 使用下列 PowerShell 命令，從存放裝置維護模式移除伺服器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. 使用下列 PowerShell 命令來繼續伺服器：

       ```PowerShell
       Resume-ClusterNode
       ```

   9. 等待存放裝置修復作業完成，並讓所有磁片恢復正常狀態。 這可能需要相當長的時間，視伺服器升級期間執行的 Vm 數目而定。 以下是要執行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級叢集中的下一個伺服器。

5. 所有伺服器都已升級至 Windows Server 2019 之後，請使用下列 PowerShell Cmdlet 來更新叢集功能等級。

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我們建議您儘快更新叢集功能等級，但在技術上，您最多可以有四周的時間來執行此動作。

6. 更新叢集功能等級之後，請使用下列 Cmdlet 來更新存放集區。 此時，新的 Cmdlet （例如`Get-ClusterPerf` ）會在叢集中的任何伺服器上完全運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 藉由停止每個 vm、使用`Update-VMVersion` Cmdlet，然後再次啟動 vm，即可選擇性地升級 vm 設定層級。

8. 如果您使用軟體定義的網路功能，並設定參數並停用 VM 即時移轉檢查，如上所述，請使用下列 Cmdlet 重新啟用 VM 即時驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. 確認升級的叢集如預期般運作。 角色應正確容錯移轉，且如果在叢集上使用 VM 即時移轉，Vm 應該會順利進行即時移轉。

10. 執行叢集驗證（`Test-Cluster`）並檢查叢集驗證報告，以驗證叢集。

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>執行虛擬機器的全新 OS 安裝

此選項不會產生任何 VM 停機時間，但您必須等候存放裝置作業（鏡像修復）在每部伺服器升級後完成。 雖然在升級程式期間會依序重新開機個別伺服器，但叢集中的其餘伺服器和所有 Vm 都將維持執行狀態。

1. 檢查叢集中的所有伺服器都執行最新的更新。 如需詳細資訊，請參閱[windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安裝 Microsoft 知識庫[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2019年2月19日）。 組建編號（請參閱`ver`命令）應該是14393.2828 或更高版本。

2. 如果您使用已設定參數的軟體定義網路功能，請開啟已提升許可權的 PowerShell 會話，然後執行下列命令以停用叢集上所有 Vm 的 VM 即時移轉驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次在一部叢集伺服器上執行下列步驟：

   1. 使用 Hyper-v VM 即時移轉，將執行中的 Vm 移出您即將升級的伺服器。

   2. 使用下列 PowerShell 命令暫停叢集伺服器-請注意，部分內部群組是隱藏的。 我們建議您務必小心，如果您尚未在伺服器上即時移轉 Vm，此 Cmdlet 會為您執行此動作，因此您可以視需要略過上一個步驟。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 藉由執行下列 PowerShell 命令，將伺服器置於儲存體維護模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 執行下列 Cmdlet 來檢查**OperationalStatus**值是否處於**維護模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  執行下列 PowerShell 命令，從叢集收回伺服器：  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. 在伺服器上執行 Windows Server 2019 的全新安裝：將系統磁片磁碟機格式化，執行**setup.exe 並使用**[無] 選項。 在安裝程式完成且伺服器重新開機之後，您必須設定伺服器身分識別、角色、功能和應用程式。

   5. 在伺服器上安裝 hyper-v 角色和容錯移轉叢集功能（您可以使用`Install-WindowsFeature` Cmdlet）。

   6. 為您的伺服器製造商核准的硬體安裝最新的存放裝置和網路驅動程式，以便與儲存空間直接存取搭配使用。

   7. 檢查新升級的伺服器是否有最新的 Windows Server 2019 更新。 如需詳細資訊，請參閱[windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。 組建編號（請參閱`ver`命令）應該是17763.292 或更高版本。

   8. 使用下列 PowerShell 命令，將伺服器重新加入叢集：

       ```PowerShell
       Add-ClusterNode
       ```

   9. 使用下列 PowerShell 命令，從存放裝置維護模式移除伺服器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. 等待存放裝置修復作業完成，並讓所有磁片恢復正常狀態。 這可能需要相當長的時間，視伺服器升級期間執行的 Vm 數目而定。 以下是要執行的命令:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. 升級叢集中的下一個伺服器。

5. 所有伺服器都已升級至 Windows Server 2019 之後，請使用下列 PowerShell Cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我們建議您儘快更新叢集功能等級，但在技術上，您最多可以有四周的時間來執行此動作。

6. 更新叢集功能等級之後，請使用下列 Cmdlet 來更新存放集區。 此時，新的 Cmdlet （例如`Get-ClusterPerf` ）會在叢集中的任何伺服器上完全運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 藉由停止每個 vm 並使用`Update-VMVersion`指令程式，然後再次啟動 vm，即可選擇性地升級 vm 設定層級。

8. 如果您使用軟體定義的網路功能，並設定參數並停用 VM 即時移轉檢查，如上所述，請使用下列 Cmdlet 重新啟用 VM 即時驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. 確認升級的叢集如預期般運作。 角色應正確容錯移轉，且如果在叢集上使用 VM 即時移轉，Vm 應該會順利進行即時移轉。

10. 執行叢集驗證（`Test-Cluster`）並檢查叢集驗證報告，以驗證叢集。

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>在 Vm 停止時執行就地升級

此選項會產生 VM 停機時間，但所需的時間可能比您在升級期間保留執行的 Vm 還短，因為您不需要等到每一部伺服器升級之後，才等待儲存作業（鏡像修復）完成。 雖然在升級過程中會依序重新開機個別伺服器，但叢集中的其餘伺服器仍會繼續執行。

1. 檢查叢集中的所有伺服器都執行最新的更新。 如需詳細資訊，請參閱[windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安裝 Microsoft 知識庫[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2019年2月19日）。 組建編號（請參閱`ver`命令）應該是14393.2828 或更高版本。

2. 停止在叢集上執行的 Vm。

3. 一次在一部叢集伺服器上執行下列步驟：

   1. 使用下列 PowerShell 命令暫停叢集伺服器-請注意，部分內部群組是隱藏的。 我們建議您謹慎執行此步驟。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. 藉由執行下列 PowerShell 命令，將伺服器置於儲存體維護模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. 執行下列 Cmdlet 來檢查**OperationalStatus**值是否處於**維護模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. 執行**setup.exe**並使用 [保留個人檔案和應用程式] 選項，在伺服器上執行 Windows Server 2019 的升級安裝。  
   安裝完成之後，伺服器會保留在叢集中，而且叢集服務會自動啟動。

   5.  檢查新升級的伺服器是否有最新的 Windows Server 2019 更新。  
   如需詳細資訊，請參閱[windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。
   組建編號（請參閱`ver`命令）應該是17763.292 或更高版本。

   6.  使用下列 PowerShell 命令，從存放裝置維護模式移除伺服器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  使用下列 PowerShell 命令來繼續伺服器：

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  等待存放裝置修復作業完成，並讓所有磁片恢復正常狀態。  
   這應該相對快速，因為 Vm 不在執行中。 以下是要執行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級叢集中的下一個伺服器。
5. 所有伺服器都已升級至 Windows Server 2019 之後，請使用下列 PowerShell Cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我們建議您儘快更新叢集功能等級，但在技術上，您最多可以有四周的時間來執行此動作。

6. 更新叢集功能等級之後，請使用下列 Cmdlet 來更新存放集區。  
   此時，新的 Cmdlet （例如`Get-ClusterPerf` ）會在叢集中的任何伺服器上完全運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 啟動叢集上的 Vm，並檢查它們是否正常運作。

8. 藉由停止每個 vm 並使用`Update-VMVersion`指令程式，然後再次啟動 vm，即可選擇性地升級 vm 設定層級。

9. 確認升級的叢集如預期般運作。  
   角色應正確容錯移轉，且如果在叢集上使用 VM 即時移轉，Vm 應該會順利進行即時移轉。

10. 執行叢集驗證（`Test-Cluster`）並檢查叢集驗證報告，以驗證叢集。

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>在 Vm 停止時執行全新的作業系統安裝

此選項會產生 VM 停機時間，但所需的時間可能比您在升級期間保留執行的 Vm 還短，因為您不需要等到每一部伺服器升級之後，才等待儲存作業（鏡像修復）完成。 雖然在升級過程中會依序重新開機個別伺服器，但叢集中的其餘伺服器仍會繼續執行。

1. 檢查叢集中的所有伺服器都執行最新的更新。  
   如需詳細資訊，請參閱[windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
   至少要安裝 Microsoft 知識庫[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2019年2月19日）。 組建編號（請參閱`ver`命令）應該是14393.2828 或更高版本。

2. 停止在叢集上執行的 Vm。

3. 一次在一部叢集伺服器上執行下列步驟：

   2. 使用下列 PowerShell 命令暫停叢集伺服器-請注意，部分內部群組是隱藏的。  
      我們建議您謹慎執行此步驟。

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. 藉由執行下列 PowerShell 命令，將伺服器置於儲存體維護模式：

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. 執行下列 Cmdlet 來檢查**OperationalStatus**值是否處於**維護模式**：

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. 執行下列 PowerShell 命令，從叢集收回伺服器：  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. 在伺服器上執行 Windows Server 2019 的全新安裝：將系統磁片磁碟機格式化，執行**setup.exe 並使用**[無] 選項。  
      在安裝程式完成且伺服器重新開機之後，您必須設定伺服器身分識別、角色、功能和應用程式。

   7. 在伺服器上安裝 hyper-v 角色和容錯移轉叢集功能（您可以使用`Install-WindowsFeature` Cmdlet）。

   8. 為您的伺服器製造商核准的硬體安裝最新的存放裝置和網路驅動程式，以便與儲存空間直接存取搭配使用。

   9. 檢查新升級的伺服器是否有最新的 Windows Server 2019 更新。  
      如需詳細資訊，請參閱[windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。
      組建編號（請參閱`ver`命令）應該是17763.292 或更高版本。

   10. 使用下列 PowerShell 命令，將伺服器重新加入叢集：

      ```PowerShell
      Add-ClusterNode
      ```

   11. 使用下列 PowerShell 命令，從存放裝置維護模式移除伺服器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. 等待存放裝置修復作業完成，並讓所有磁片恢復正常狀態。  
       這可能需要相當長的時間，視伺服器升級期間執行的 Vm 數目而定。 以下是要執行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級叢集中的下一個伺服器。

5. 所有伺服器都已升級至 Windows Server 2019 之後，請使用下列 PowerShell Cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我們建議您儘快更新叢集功能等級，但在技術上，您最多可以有四周的時間來執行此動作。

6. 更新叢集功能等級之後，請使用下列 Cmdlet 來更新存放集區。  
   此時，新的 Cmdlet （例如`Get-ClusterPerf` ）會在叢集中的任何伺服器上完全運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 啟動叢集上的 Vm，並檢查它們是否正常運作。

8. 藉由停止每個 vm 並使用`Update-VMVersion`指令程式，然後再次啟動 vm，即可選擇性地升級 vm 設定層級。

9. 確認升級的叢集如預期般運作。  
   角色應正確容錯移轉，且如果在叢集上使用 VM 即時移轉，Vm 應該會順利進行即時移轉。

10. 執行叢集驗證（`Test-Cluster`）並檢查叢集驗證報告，以驗證叢集。
