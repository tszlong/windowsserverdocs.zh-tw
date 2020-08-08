---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: 更新磁碟機韌體
ms.author: toklima
manager: dmoss
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 15e0d6dedc6bb81c0b511479ee342dbd463654e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946211"
---
# <a name="updating-drive-firmware"></a>更新磁碟機韌體
>適用于： Windows Server 2019、Windows Server 2016、Windows 10

更新磁碟機韌體向來都是可能導致停機的麻煩工作，這就是我們要改善儲存空間、Windows Server 及 Windows 10 版本 1703 和更新版本的原因。 如果您的磁碟機支援 Windows 中所含的新韌體更新機制，則可以更新生產磁碟機的磁碟機韌體，而不造成停機。 不過，如果您要更新生產磁碟機的韌體，請務必閱讀在使用這個功能強大的新功能時如何將風險降到最低的提示。

  > [!Warning]
  > 韌體更新是可能會有風險的維護作業，而且您只應該在徹底測試新的韌體映像之後再套用它們。 不受支援硬體上的新韌體可能會對可靠性和穩定性造成負面影響，甚至導致資料遺失。 系統管理員應該閱讀所指定更新隨附的版本資訊，來判斷其影響和適用性。

## <a name="drive-compatibility"></a>磁碟機相容性

若要使用 Windows Server 來更新磁碟機韌體，您必須具有支援的磁碟機。 為了確保通用裝置行為，一開始會定義適用於 SAS、SATA 和 NVMe 裝置的新和 (適用於 Windows 10 和 Windows Server 2016) 選擇性 Hardware Lab Kit (HLK) 需求。 這些需求概述 SATA、SAS 或 NVMe 裝置必須支援的命令，以用來透過這些新的 Windows 原生 PowerShell Cmdlet 進行韌體更新。 若要支援這些需求，會進行新的 HLK 測試，確認廠商產品支援正確的命令，並在未來的修訂中進行實作。

如需硬體是否支援 Windows 更新磁碟機韌體的相關資訊，請連絡解決方案廠商。
以下是各種需求的連結︰

-   SATA：[Device.Storage.Hd.Sata](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragehdsata) - 在 **[如果已實作\] 韌體下載和啟用**小節

-   SAS：[Device.Storage.Hd.Sas](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragehdsas) - 在 **[如果已實作\] 韌體下載和啟用**小節

-   NVMe：[Device.Storage.ControllerDrive.NVMe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragecontrollerdrivenvme) - 在 **5.7** 和 **5.8** 小節。

## <a name="powershell-cmdlets"></a>PowerShell Cmdlet

兩個新增至 Windows 的 Cmdlet 為：

-   Get-StorageFirmwareInformation
-   Update-StorageFirmware

第一個 Cmdlet 提供裝置功能、韌體映像和修訂的詳細資訊。 在此情況下，機器只會包含有 1 個韌體插槽的單一 SATA SSD。 以下為範例：

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

請注意，因為沒有方法可以明確查詢支援這些命令的裝置，所以 SAS 裝置一律會將 "SupportsUpdate" 報告為 "True"。

如果磁碟機支援新的韌體更新機制，則第二個 Cmdlet (Update-StorageFirmware) 可讓系統管理員使用映像檔來更新磁碟機韌體。 您應該直接從 OEM 或磁碟機廠商取得此映像檔。

  > [!Note]
  > 更新任何生產硬體之前，請測試實驗室設定中相同硬體上的特定韌體映像。

磁碟機會先將新的韌體映像載入至內部臨時區域。 發生這種情況時，通常會繼續 I/O。 映像會在下載之後啟用。 因為發生內部重設，所以磁碟機在這段時間將無法回應 I/O 命令。 這表示此磁碟機在啟用期間未提供任何資料。 除非韌體啟用完成，否則存取此磁碟機上資料的應用程式必須等待回應。 以下是作用中的 Cmdlet 範例︰

   ```powershell
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

磁碟機在啟用新的韌體映像時通常不會完成 I/O 要求。 磁碟機進行啟用所需的時間長度取決於設計和所更新的韌體類型。 我們觀察到更新時間範圍從小於 5 秒到超過 30 秒。

這個磁碟機已在 ~5.8 秒內執行韌體更新，如下所示︰

```powershell
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>更新生產環境中的磁碟機

將伺服器置入生產環境之前，強烈建議將磁碟機的韌體更新成硬體廠商或所銷售並支援您解決方案 (存放裝置機箱、磁碟機和伺服器) 之 OEM 所建議的韌體。

伺服器位於生產環境時，最好實際上對伺服器進行最少變更。 不過，有時解決方案廠商會建議您磁碟機有非常重要的韌體更新。 如果發生這種情況，以下是一些可在套用任何磁碟機韌體更新之前遵循的最佳作法︰

1. 檢閱韌體版本資訊，並確認更新可解決可能會影響您環境的問題，而且韌體不會包含任何可能會對您造成負面影響的已知問題。

2. 在實驗室中於具有相同磁碟機的伺服器上安裝韌體 (如果有相同磁碟機的多個修訂，則包含磁碟機修訂)，並使用新的韌體測試正在進行載入的磁碟機。 如需執行綜合負載測試的相關資訊，請參閱[在 Windows Server 中使用綜合工作負載測試儲存空間效能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn894707(v=ws.11))。

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>使用儲存空間直接存取的自動化韌體更新

Windows Server 2016 包含儲存空間直接存取部署的健全狀況服務 (含 Microsoft Azure Stack 解決方案)。 健全狀況服務的主要目的是為了方便監視和管理硬體部署。 在管理功能期間，它可以推出跨整個叢集的磁碟機韌體，而不會讓任何工作負載離線或發生停機。 這項功能是透過原則所驅動，並由系統管理員手動控制。

使用健全狀況服務推出整個叢集的韌體非常簡單，並且包含下列步驟︰

-   識別您預期成為儲存空間直接叢集一部分的 HDD 和 SSD 磁碟機，以及磁碟機是否支援執行韌體更新的 Windows
-   在所支援的元件 xml 檔案中列出這些磁碟機
-   在所支援的元件 xml 中，識別您預期這些磁碟機要具有的韌體版本 (包含韌體映像的位置路徑)
-   將 xml 檔案上傳至叢集資料庫

此時，健全狀況服務將會檢查和剖析 xml，以及識別任何未部署所需韌體版本的磁碟機。 它接著會繼續逐節點重新導向遠離受影響磁碟機的 I/O，並更新其上的韌體。 儲存空間直接存取叢集達成復原的方式是將資料分散到多個伺服器節點；健全狀況服務就可能找出需要更新磁碟機的整個節點。 節點更新之後，會先透過讓叢集中的所有資料複本重新彼此同步來起始儲存空間中的修復，再繼續進行下一個節點。 這對要在推出韌體時轉換為「降級」作業模式的儲存空間而言是預期且正常的作業。

為了確保新韌體映像可穩定推行並具有足夠驗證時間，則更新數部伺服器會有明顯的延遲。 根據預設，健全狀況服務將會等候 7 天，再更新第 2<sup>部</sup>伺服器。 任何後續伺服器 (第 3<sup>部</sup>、第 4<sup>部</sup>、…) 的更新都會有 1 天的延遲。 如果系統管理員發現韌體不穩定或不是所需要的，則隨時可以透過健全狀況服務停止進一步推出。 如果先前已驗證過韌體，並且想要更快速地推出，可以修改這些預設值的天、小時或分鐘。

以下是一般儲存空間直接存取叢集的所支援元件 xml 範例︰

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

若要推出在這個儲存空間直接存取叢集中啟動的新韌體，只需要將 .xml 上傳至叢集資料庫：

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

在慣用的編輯器 (例如 Visual Studio Code 或記事本) 中編輯並儲存檔案。

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

如果您想要查看作用中的健全狀況服務，並深入瞭解其推出機制，請看一下這段影片：https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>常見問題集

請參閱[對磁碟機韌體更新進行疑難排解](troubleshoot-firmware-update.md)。

### <a name="will-this-work-on-any-storage-device"></a>這種作法是否適用於任何存放裝置

這適用於在其韌體中實作正確命令的存放裝置。 Get-StorageFirmwareInformation Cmdlet 將會顯示磁碟機韌體是否確實支援正確命令 (適用於 SATA/NVMe)，而且 HLK 測試可讓廠商和 OEM 測試這種行為。

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>在我更新 SATA 磁碟機之後，報告不再支援更新機制。 磁碟機是否發生錯誤

否，除非新的韌體不再允許更新，否則磁碟機沒有問題。 您遇到已知問題，因此磁碟機快取版本功能不正確。 執行 "Update-StorageProviderCache -DiscoveryLevel Full" 將會重新列舉磁碟機功能並更新快取的複本。 因應措施是建議執行上述命令一次，再於空間直接存取叢集上起始韌體更新或完成推出。

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>是否可以透過這項機制更新 SAN 上的韌體
否 - SAN 通常會有這類維護作業的專屬公用程式和介面。 這個新的機制適用於直接連接的存放裝置，例如 SATA、SAS 或 NVMe 裝置。

### <a name="from-where-do-i-get-the-firmware-image"></a>是否從這裡取得韌體映像

您一律應該直接從 OEM、解決方案廠商或磁碟機廠商取得任何韌體，而不是從另一方進行下載。 Windows 提供取得磁碟機映像的機制，但無法確認其完整性。

### <a name="will-this-work-on-clustered-drives"></a>這是否作用於叢集磁碟機

這些 Cmdlet 也可以對叢集磁碟機執行其功能，但請記住健全狀況服務協調流程可降低執行中工作負載的 I/O 影響。 如果直接在叢集磁碟機上使用 Cmdlet，則 I/O 可能會停止。 一般而言，最好在沒有時或基礎磁碟機上只有最小工作負載時才執行磁碟機韌體更新。

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>在儲存空間上更新韌體時，會發生什麼事

在儲存空間直接存取上部署健全狀況服務的 Windows Server 2016 上，您可以執行這項作業，而不需要讓工作負載離線，並假設磁碟機支援 Windows Server 更新韌體。

### <a name="what-happens-if-the-update-fails"></a>如果更新失敗，會發生什麼事

更新可能會因各種原因而失敗，其中一部分是︰1) 磁碟機不支援讓 Windows 更新其韌體的正確命令。 在此情況下，新的韌體映像永遠不會啟用，而且磁碟機會繼續使用舊的映像運作。 2) 映像無法下載至或套用至這個磁碟機 (版本不符、影像損毀…)。 在此情況下，磁碟機會讓 activate 命令失敗。 同樣地，舊的韌體映像將會繼續使用函數。

如果磁碟機在韌體更新之後根本未回應，則您可能會在磁碟機韌體本身發現錯誤。 先測試實驗室環境中的所有韌體更新，再將它們放到生產環境中。 唯一的補救方式可能是更換磁碟機。

如需詳細資訊，請參閱[對磁碟機韌體更新進行疑難排解](troubleshoot-firmware-update.md)。

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>如何停止進行中韌體推出

透過下列方式，在 PowerShell 中停用推出︰
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>我在推出期間看到拒絕存取或找不到路徑錯誤。如何修正這個問題

確定所有叢集節點都可以存取您想要用於更新的韌體映像。 確保這種情況的最簡單方法是將它放在叢集共用磁碟區上。
