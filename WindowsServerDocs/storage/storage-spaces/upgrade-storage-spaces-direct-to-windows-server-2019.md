---
title: 將儲存空間直接存取叢集升級到 Windows Server 2019
description: 如何將儲存空間直接存取叢集升級到 Windows Server 2019-可能是同時執行的 Vm，或它們要停止時。
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9db92aa33cde9b2beed11149dae06bb3af2b5a03
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455416"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>將儲存空間直接存取叢集升級到 Windows Server 2019

本主題描述如何將儲存空間直接存取叢集升級到 Windows Server 2019。 若要從 Windows Server 2016 的儲存空間直接存取叢集升級為使用的 Windows Server 2019，有四種[叢集 OS 輪流升級程序](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)— 涉及保持執行，Vm 的兩個資料表及涉及的兩個停止所有的虛擬機器。 每一種方法具有不同的優點和缺點，因此，請選取其中最適合您組織的需求：

- **就地升級執行 Vm 時**叢集中的每部伺服器上，此選項時，會產生任何 VM 停機時間，但您必須等待完成每一部伺服器升級後的儲存體工作 （鏡像修復）。

- **Vm 正在執行時的清除作業系統安裝**叢集中的每部伺服器上，此選項時，會產生任何 VM 停機時間，但您必須等待每一部伺服器和所有的儲存體工作 （鏡像修復） 完成後的每個伺服器已升級，您必須設定它的應用程式和角色一次。

- **就地升級會停止 Vm 時**叢集中的每部伺服器上 — 這個選項不會產生 VM 停機時間，但是您不需要等待儲存體作業 （鏡像修復） 進行，因此更快速。

- **清除作業系統安裝的 Vm 都停止時**叢集中的每部伺服器上 — 這個選項不會產生 VM 停機時間，但是您不需要等待儲存體作業 （鏡像修復），因此它的速度進行。

## <a name="prerequisites-and-limitations"></a>必要條件和限制

然後繼續執行升級：

- 請檢查您有可用的備份，以防在升級過程中有任何問題。

- 檢查您的硬體廠商有 BIOS、 韌體與驅動程式支援在 Windows Server 2019 的伺服器。

有一些限制需要注意的升級程序：

- 若要啟用儲存空間直接存取與 Windows Server 2019 組建 176693.292 之前，客戶可能需要連絡 Microsoft 支援服務啟用儲存空間直接存取和以軟體定義網路功能的登錄機碼。 如需詳細資訊，請參閱 Microsoft Knowledge Base[文章 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)。

- 升級時具有 ReFS 磁碟區的叢集中，有一些限制：

- ReFS 磁碟區上完全支援升級，不過，升級後的磁碟區不會受益於 ReFS 中 Windows Server 2019 的增強功能。 這些權益，例如鏡像加速同位檢查的效能提升會需要新建立的 Windows Server 2019 ReFS 磁碟區。 換句話說，您必須建立新的磁碟區使用`New-Volume`cmdlet 或 伺服器管理員。 以下是一些新的磁碟區會得到的 ReFS 增強功能：

    - **對應記錄-略過**: ReFS 的效能改善，僅適用於叢集 （儲存空間直接存取） 的系統，也不適用於獨立的存放集區。

    - **壓縮**專屬於多具有恢復功能的磁碟區的效率提升在 Windows Server 2019。

- 在升級之前的 Windows Server 2016 儲存空間直接存取叢集伺服器，建議您將伺服器放入存放裝置維護模式。 如需詳細資訊，請參閱 > 一節事件 5120[疑難排解儲存空間直接存取](troubleshooting-storage-spaces.md)。 雖然 Windows Server 2016 中已修正此問題，我們建議最佳做法在升級期間讓每個儲存空間直接存取伺服器進入存放裝置維護模式。

- 沒有使用參數集的軟體定義網路環境的已知的問題。 這個問題牽涉到 HYPER-V VM 即時移轉，從 Windows Server 2019 以 Windows Server 2016 （即時移轉到較早的作業系統）。 若要確保成功的即時移轉，建議您變更正在進行即時移轉的 Windows Server 2019 到 Windows Server 2016 的 Vm 上的 VM 網路設定。 針對 Windows Server 2019 在 2019年 01 D hotfix 彙總套件修正此問題，也就是建置 17763.292。 如需詳細資訊，請參閱 Microsoft Knowledge Base[文章 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)。

因為上述已知問題，有些客戶可能會考慮建立新的 Windows Server 2019 叢集，並將資料複製從舊叢集，而不是升級其使用其中一個如下所述的四個處理程序的 Windows Server 2016 叢集。

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Vm 正在執行時，執行就地升級

此選項時，會產生任何 VM 停機時間，但您必須等候完成每一部伺服器升級後的儲存體作業 （鏡像修復）。 雖然個別的伺服器將會重新啟動循序升級程序期間，剩餘的伺服器叢集，以及所有 Vm，將會繼續執行。

1. 檢查叢集中的所有伺服器都已都安裝最新的 Windows 更新。 如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 最少，安裝 Microsoft Knowledge Base[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 組建編號 (請參閱`ver`命令) 應該是 14393.2828 或更高版本。

2. 如果您使用軟體定義網路設定參數，開啟提升權限的 PowerShell 工作階段，並執行下列命令來停用叢集上的所有 Vm 上的 VM 即時移轉驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次，一部叢集伺服器上執行下列步驟：

   1. 使用 HYPER-V VM 即時移轉移動執行中 Vm 關閉您即將升級的伺服器。

   2. 暫停叢集伺服器，使用下列 PowerShell 命令，請注意，會隱藏某些內部的群組。 我們建議此步驟，請特別小心，如果您沒有已上市，將 Vm 移轉，因此您可以跳過上一個步驟如果想要執行此 cmdlet 會在伺服器關閉。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 請將伺服器放在存放裝置維護模式中，執行下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 執行下列 cmdlet 來檢查所**OperationalStatus**值是**處於維護模式**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. 藉由執行在伺服器上執行升級安裝的 Windows Server 2019 **setup.exe**和使用 [保留個人檔案和應用程式] 選項。 安裝完成後，伺服器仍會保留在叢集和叢集服務會自動啟動。

   6. 檢查新升級的伺服器有最新的 Windows Server 2019 更新。 如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。 組建編號 (請參閱`ver`命令) 應該是 17763.292 或更高版本。

   7. 移除存放裝置維護模式的伺服器使用下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. 使用下列 PowerShell 命令，繼續執行伺服器：

       ```PowerShell
       Resume-ClusterNode
       ```

   9. 等候儲存體修復作業完成並恢復狀況良好狀態的所有磁碟。 這可能需要相當長的時間，根據 Vm 的數目在伺服器升級期間執行。 以下是要執行的命令：

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級在叢集中的下一步 的伺服器。

5. 所有伺服器已都升級至 Windows Server 2019 之後，請使用下列 PowerShell cmdlet 來更新叢集功能等級。

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我們建議您更新叢集功能等級，盡雖然技術上您若要這樣做，會在有最多四週時。

6. 更新叢集功能等級之後，使用下列 cmdlet 來更新存放集區。 此時，新的指令程式，例如`Get-ClusterPerf`會在叢集中的任何伺服器上完全正常運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 選擇升級停止每個 VM 的 VM 設定層級使用`Update-VMVersion`cmdlet，然後再重新啟動 Vm。

8. 如果您使用軟體定義網路設定參數，而且已停用的 VM 即時移轉檢查上述的指示來使用下列 cmdlet 來重新啟用 VM 即時驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. 確認升級後的叢集運作如預期般運作。 角色應該容錯移轉正確，如果在叢集上使用 VM 即時移轉，則 Vm 應該成功即時移轉。

10. 執行叢集驗證來驗證叢集 (`Test-Cluster`) 並檢查叢集驗證報告。

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Vm 正在執行時，請執行全新的作業系統安裝

此選項時，會產生任何 VM 停機時間，但您必須等候完成每一部伺服器升級後的儲存體作業 （鏡像修復）。 雖然個別的伺服器將會重新啟動循序升級程序期間，剩餘的伺服器叢集，以及所有 Vm，將會繼續執行。

1. 檢查在叢集中的所有伺服器都執行最新的更新。 如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 最少，安裝 Microsoft Knowledge Base[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 組建編號 (請參閱`ver`命令) 應該是 14393.2828 或更高版本。

2. 如果您使用軟體定義網路設定參數，開啟提升權限的 PowerShell 工作階段，並執行下列命令來停用叢集上的所有 Vm 上的 VM 即時移轉驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次，一部叢集伺服器上執行下列步驟：

   1. 使用 HYPER-V VM 即時移轉移動執行中 Vm 關閉您即將升級的伺服器。

   2. 暫停叢集伺服器，使用下列 PowerShell 命令，請注意，會隱藏某些內部的群組。 我們建議此步驟，請特別小心，如果您沒有已上市，將 Vm 移轉，因此您可以跳過上一個步驟如果想要執行此 cmdlet 會在伺服器關閉。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 請將伺服器放在存放裝置維護模式中，執行下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 執行下列 cmdlet 來檢查所**OperationalStatus**值是**處於維護模式**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  收回叢集的伺服器執行下列 PowerShell 命令：  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. 在伺服器上執行全新安裝的 Windows Server 2019： 格式的系統磁碟機中，執行**setup.exe** ，並使用 「 虛無 」 選項。 您必須設定伺服器身分識別、 角色、 功能，並在安裝完成後的應用程式和伺服器重新啟動。

   5. 在伺服器上安裝 HYPER-V 角色和容錯移轉叢集功能 (您可以使用`Install-WindowsFeature`cmdlet)。

   6. 安裝最新儲存體和網路硬體驅動程式與儲存空間直接存取搭配使用伺服器製造商所核准。

   7. 檢查新升級的伺服器有最新的 Windows Server 2019 更新。 如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。 組建編號 (請參閱`ver`命令) 應該是 17763.292 或更高版本。

   8. 使用下列 PowerShell 命令，重新加入至叢集的伺服器：

       ```PowerShell
       Add-ClusterNode
       ```

   9. 移除存放裝置維護模式的伺服器使用下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. 等候儲存體修復作業完成並恢復狀況良好狀態的所有磁碟。 這可能需要相當長的時間，根據 Vm 的數目在伺服器升級期間執行。 以下是要執行的命令：

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. 升級在叢集中的下一步 的伺服器。

5. 所有伺服器已都升級至 Windows Server 2019 之後，請使用下列 PowerShell cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我們建議您更新叢集功能等級，盡雖然技術上您若要這樣做，會在有最多四週時。

6. 更新叢集功能等級之後，使用下列 cmdlet 來更新存放集區。 此時，新的指令程式，例如`Get-ClusterPerf`會在叢集中的任何伺服器上完全正常運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 選擇性地將 VM 設定層級升級停止每個 VM，並使用`Update-VMVersion`cmdlet，然後再重新啟動 Vm。

8. 如果您使用軟體定義網路設定參數，而且已停用的 VM 即時移轉檢查上述的指示來使用下列 cmdlet 來重新啟用 VM 即時驗證檢查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. 確認升級後的叢集運作如預期般運作。 角色應該容錯移轉正確，如果在叢集上使用 VM 即時移轉，則 Vm 應該成功即時移轉。

10. 執行叢集驗證來驗證叢集 (`Test-Cluster`) 並檢查叢集驗證報告。

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>雖然 Vm 已停止執行就地升級

此選項會產生 VM 停機時間，但是可能需要較少的時間比您保留在升級期間執行，因為您不需要等待完成每一部伺服器升級後的儲存體工作 （鏡像修復） 的 Vm。 雖然個別的伺服器將會重新啟動循序升級程序期間，在叢集中剩餘的伺服器繼續執行。

1. 檢查在叢集中的所有伺服器都執行最新的更新。 如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 最少，安裝 Microsoft Knowledge Base[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 組建編號 (請參閱`ver`命令) 應該是 14393.2828 或更高版本。

2. 停止叢集上執行的 Vm。

3. 一次，一部叢集伺服器上執行下列步驟：

   1. 暫停叢集伺服器，使用下列 PowerShell 命令，請注意，會隱藏某些內部的群組。 我們建議此步驟，請務必謹慎。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. 請將伺服器放在存放裝置維護模式中，執行下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. 執行下列 cmdlet 來檢查所**OperationalStatus**值是**處於維護模式**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. 藉由執行在伺服器上執行升級安裝的 Windows Server 2019 **setup.exe**和使用 [保留個人檔案和應用程式] 選項。  
   安裝完成後，伺服器仍會保留在叢集和叢集服務會自動啟動。

   5.  檢查新升級的伺服器有最新的 Windows Server 2019 更新。  
   如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。
   組建編號 (請參閱`ver`命令) 應該是 17763.292 或更高版本。

   6.  移除存放裝置維護模式的伺服器使用下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  使用下列 PowerShell 命令，繼續執行伺服器：

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  等候儲存體修復作業完成並恢復狀況良好狀態的所有磁碟。  
   這應該是相當快速，因為 Vm 未執行。 以下是要執行的命令：

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級在叢集中的下一步 的伺服器。
5. 所有伺服器已都升級至 Windows Server 2019 之後，請使用下列 PowerShell cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我們建議您更新叢集功能等級，盡雖然技術上您若要這樣做，會在有最多四週時。

6. 更新叢集功能等級之後，使用下列 cmdlet 來更新存放集區。  
   此時，新的指令程式，例如`Get-ClusterPerf`會在叢集中的任何伺服器上完全正常運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 在叢集上啟動 Vm，並檢查它們正常運作。

8. 選擇性地將 VM 設定層級升級停止每個 VM，並使用`Update-VMVersion`cmdlet，然後再重新啟動 Vm。

9. 確認升級後的叢集運作如預期般運作。  
   角色應該容錯移轉正確，如果在叢集上使用 VM 即時移轉，則 Vm 應該成功即時移轉。

10. 執行叢集驗證來驗證叢集 (`Test-Cluster`) 並檢查叢集驗證報告。

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>雖然 Vm 已停止執行全新的作業系統安裝

此選項會產生 VM 停機時間，但是可能需要較少的時間比您保留在升級期間執行，因為您不需要等待完成每一部伺服器升級後的儲存體工作 （鏡像修復） 的 Vm。 雖然個別的伺服器將會重新啟動循序升級程序期間，在叢集中剩餘的伺服器繼續執行。

1. 檢查在叢集中的所有伺服器都執行最新的更新。  
   如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2016 更新歷程記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
   最少，安裝 Microsoft Knowledge Base[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 組建編號 (請參閱`ver`命令) 應該是 14393.2828 或更高版本。

2. 停止叢集上執行的 Vm。

3. 一次，一部叢集伺服器上執行下列步驟：

   2. 暫停叢集伺服器，使用下列 PowerShell 命令，請注意，會隱藏某些內部的群組。  
      我們建議此步驟，請務必謹慎。

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. 請將伺服器放在存放裝置維護模式中，執行下列 PowerShell 命令：

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. 執行下列 cmdlet 來檢查所**OperationalStatus**值是**處於維護模式**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. 收回叢集的伺服器執行下列 PowerShell 命令：  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. 在伺服器上執行全新安裝的 Windows Server 2019： 格式的系統磁碟機中，執行**setup.exe** ，並使用 「 虛無 」 選項。  
      您必須設定伺服器身分識別、 角色、 功能，並在安裝完成後的應用程式和伺服器重新啟動。

   7. 在伺服器上安裝 HYPER-V 角色和容錯移轉叢集功能 (您可以使用`Install-WindowsFeature`cmdlet)。

   8. 安裝最新儲存體和網路硬體驅動程式與儲存空間直接存取搭配使用伺服器製造商所核准。

   9. 檢查新升級的伺服器有最新的 Windows Server 2019 更新。  
      如需詳細資訊，請參閱 < [Windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619/windows-10-update-history)。
      組建編號 (請參閱`ver`命令) 應該是 17763.292 或更高版本。

   10. 使用下列 PowerShell 命令，重新加入至叢集的伺服器：

      ```PowerShell
      Add-ClusterNode
      ```

   11. 移除存放裝置維護模式的伺服器使用下列 PowerShell 命令：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. 等候儲存體修復作業完成並恢復狀況良好狀態的所有磁碟。  
       這可能需要相當長的時間，根據 Vm 的數目在伺服器升級期間執行。 以下是要執行的命令：

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升級在叢集中的下一步 的伺服器。

5. 所有伺服器已都升級至 Windows Server 2019 之後，請使用下列 PowerShell cmdlet 來更新叢集功能等級。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我們建議您更新叢集功能等級，盡雖然技術上您若要這樣做，會在有最多四週時。

6. 更新叢集功能等級之後，使用下列 cmdlet 來更新存放集區。  
   此時，新的指令程式，例如`Get-ClusterPerf`會在叢集中的任何伺服器上完全正常運作。

   ```PowerShell
   Update-StoragePool
   ```

7. 在叢集上啟動 Vm，並檢查它們正常運作。

8. 選擇性地將 VM 設定層級升級停止每個 VM，並使用`Update-VMVersion`cmdlet，然後再重新啟動 Vm。

9. 確認升級後的叢集運作如預期般運作。  
   角色應該容錯移轉正確，如果在叢集上使用 VM 即時移轉，則 Vm 應該成功即時移轉。

10. 執行叢集驗證來驗證叢集 (`Test-Cluster`) 並檢查叢集驗證報告。
