---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: 進階重複資料刪除設定
ms.prod: windows-server-threshold
ms.technology: storage-deduplication
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: af977519b5e77eb768fdf8de1e6a34f7c8274666
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447247"
---
# <a name="advanced-data-deduplication-settings"></a>進階重複資料刪除設定

> 適用於 Windows Server (半年度管道)、Windows Server 2016

本文件說明如何修改進階[重複資料刪除](overview.md)設定。 針對[建議的工作負載](install-enable.md#enable-dedup-candidate-workloads)，預設設定應已足夠。 修改這些設定的主要原因，是為了改進重複資料刪除搭配其他工作負載類型時的效能。

## <a id="modifying-job-schedules"></a>修改重複資料刪除作業排程
[預設重複資料刪除工作排程](understand.md#job-info)是專為搭配建議的工作負載運作並盡可能不干擾工作而設計 (不包括為[**備份**使用類型](understand.md#usage-type-backup)啟用的「優先順序最佳化」  工作)。 當工作負載有大型資源需求時，它可以確保工作僅在閒置時間執行，或是減少或增加允許重複資料刪除工作使用的系統資源量。

### <a id="modifying-job-schedules-change-schedule"></a>變更重複資料刪除排程
重複資料刪除工作是透過 Windows 工作排程器排程，並且可以在 Microsoft\Windows\Deduplication 路徑下檢視及編輯。 重複資料刪除包括數個可讓您輕鬆排程的 Cmdlet。
* [`Get-DedupSchedule`](https://technet.microsoft.com/library/hh848446.aspx) 顯示目前的排程的工作。
* [`New-DedupSchedule`](https://technet.microsoft.com/library/hh848445.aspx) 建立新的排程的工作。
* [`Set-DedupSchedule`](https://technet.microsoft.com/library/hh848447.aspx) 修改現有的排程的工作。
* [`Remove-DedupSchedule`](https://technet.microsoft.com/library/hh848451.aspx) 移除排程的工作。

變更重複資料刪除工作執行時間最常見的原因，是為了確保工作在非尖峰時間執行。 以下的逐步範例示範如何針對「陽光普照的一天」  案例修改重複資料刪除排程：有一部超交集 Hyper-V 主機會在週末以及工作日下午 7:00 之後閒置。 為了變更排程，請使用系統管理員身分執行下列 PowerShell Cmdlet。

1. 停用排定的每小時[最佳化](understand.md#job-info-optimization)工作。  
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. 移除目前排定的[記憶體回收](understand.md#job-info-gc)和[完整性清除](understand.md#job-info-scrubbing)工作。
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. 建立夜間最佳化工作，它會在下午 7:00 執行，具有高優先順序，並使用系統上所有可用的 CPU 和記憶體。
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]  
    > 為 `-Start` 提供之 `System.Datetime` 中的「日期」  部分是無關的 (只要是過去的日期即可)，但「時間」  部分會指定工作應該開始的時間。
4. 建立每週的記憶體回收工作，它會在星期六上午 7:00 執行，具有高優先順序，並使用系統上所有可用的 CPU 和記憶體。
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. 建立每週的完整性清除工作，它會在星期日上午 7 時執行，具有高優先順序，並使用系統上所有可用的 CPU 和記憶體。
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a id="modifying-job-schedules-available-settings"></a>可用的全工作設定
您可以針對新的或是排定的重複資料刪除工作切換下列設定：

<table>
    <thead>
        <tr>
            <th style="min-width:125px">參數名稱</th>
            <th>定義</th>
            <th>接受的值</th>
            <th>為何要設定此值？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>類型</td>
            <td>應排定的工作類型</td>
            <td>
                <ul>
                    <li>Optimization</li>
                    <li>GarbageCollection</li>
                    <li>擦選</li>
                </ul>
            </td>
            <td>此值為必要，因為它是您要排定的工作類型。 在工作排定之後，您就無法變更這個值。</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>已排定工作的系統優先順序</td>
            <td>
                <ul>
                    <li>高</li>
                    <li>中等</li>
                    <li>低</li>
                </ul>
            </td>
            <td>這個值有助於系統判斷如何配置 CPU 時間。 <em>High</em> 將使用較多的 CPU 時間，<em>Low</em> 則會使用較少的 CPU 時間。</td>
        </tr>
        <tr>
            <td>日</td>
            <td>排定執行工作的日子</td>
            <td>0-6 的整數陣列，代表星期幾：<ul>
                <li>0 = 星期日</li>
                <li>1 = 星期一</li>
                <li>2 = 星期二</li>
                <li>3 = 星期三</li>
                <li>4 = 星期四</li>
                <li>5 = 星期五</li>
                <li>6 = 星期六</li>
            </ul></td>
            <td>排定的工作必須至少在其中一天執行。</td>
        </tr>
        <tr>
            <td>Cores</td>
            <td>工作應使用系統上之核心的百分比</td>
            <td>整數 0-100 (表示百分比)</td>
            <td>控制工作將對系統上的計算資源產生的影響程度</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>允許工作執行的時數上限</td>
            <td>正整數</td>
            <td>若要避免工作和工作負載時遇到&#39;s 非閒置時間</td>
        </tr>
        <tr>
            <td>Enabled</td>
            <td>是否將執行工作</td>
            <td>True/false</td>
            <td>停用工作而不移除工作</td>
        </tr>
        <tr>
            <td>完整</td>
            <td>用於排程完整記憶體回收工作</td>
            <td>參數 (true/false)</td>
            <td>根據預設，每第四個工作為完整記憶體回收工作。 使用此參數，您就可以排程完整記憶體回收以更頻繁地執行。</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>指定套用至工作的輸出/輸入量節流設定</td>
            <td>整數 0-100 (表示百分比)</td>
            <td>節流設定可確保該作業 don&#39;t 會干擾其他 O-大量程序。</td>
        </tr>
        <tr>
            <td>記憶體</td>
            <td>系統上工作應使用之記憶體的百分比</td>
            <td>整數 0-100 (表示百分比)</td>
            <td>控制工作將對系統上的記憶體資源產生的影響程度</td>
        </tr>
        <tr>
            <td>名稱</td>
            <td>已排程工作的名稱</td>
            <td>字串</td>
            <td>工作必須有可唯一識別的名稱。</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>指示清除工作程序，並報告它所發現的損毀，但不執行任何修復動作</td>
            <td>參數 (true/false)</td>
            <td>您可以手動還原位於磁碟損壞區段的檔案。</td>
        </tr>
        <tr>
            <td>開始時間</td>
            <td>指定工作應該開始的時間</td>
            <td><code>System.DateTime</code></td>
            <td><em>日期</em>屬於<code>System.Datetime</code>提供給<em>開始</em>無關 (只要它&#39;中過去)，但<em>時間</em>部分會指定工作應該開始.</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>指定重複資料刪除是否應該在系統忙碌時停止</td>
            <td>參數 (true/false)</td>
            <td>這個參數可讓您控制重複資料刪除的行為--如果您想要在工作負載並非閒置時執行重複資料刪除，這特別重要。</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-volume-settings"></a>修改重複資料刪除全磁碟區設定
### <a id="modifying-volume-settings-how-to-toggle"></a>切換磁碟區設定
您可以透過在啟用磁碟區重複資料刪除時所選取的[使用類型](understand.md#usage-type)，來設定全磁碟區預設設定。 重複資料刪除包括可輕鬆編輯全磁碟區設定的 Cmdlet：

* [`Get-DedupVolume`](https://technet.microsoft.com/library/hh848448.aspx)
* [`Set-DedupVolume`](https://technet.microsoft.com/library/hh848438.aspx)

從所選使用類型修改磁碟區設定的主要原因，是為了改進特定檔案 (例如多媒體或其他已壓縮的檔案類型) 的讀取效能，或微調重複資料刪除以針對您的特定工作負載達到更好的最佳化。 下列範例示範如何針對最類似於一般用途檔案伺服器工作負載，但使用經常變更之大型檔案的工作負載，修改重複資料刪除磁碟區設定。

1. 查看叢集共用磁碟區 1 的目前磁碟區設定。
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. 啟用叢集共用磁碟區 1 上的 OptimizePartialFiles，讓 MinimumFileAge 原則套用至檔案的區段，而非整個檔案。 這樣可確保即使檔案的區段經常變更，檔案的大部分仍可獲得最佳化。
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a id="modifying-volume-settings-available-settings"></a>可用的全磁碟區設定
<table>
    <thead>
        <tr>
            <th style="min-width:125px">設定名稱</th>
            <th>定義</th>
            <th>接受的值</th>
            <th>為何要修改此值？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>區塊在複製到區塊存放區的熱點區段之前，區塊被參考的次數。 熱點區段的值是，所謂&quot;熱&quot;經常參考的區塊有多個存取路徑，以改善存取時間。</td>
            <td>正整數</td>
            <td>修改此數字的主要原因是為了增加具有高度重複資料磁碟區的儲存速率。 一般情況下，預設值 (100) 是建議的設定，以及您應該不&#39;需要修改這個。</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>要在最佳化時排除的檔案類型</td>
            <td>副檔名的陣列</td>
            <td>某些檔案類型，特別是多媒體或是已壓縮的檔案，最佳化之後並不會有太多好處。 此設定可讓您設定要排除的類型。</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>指定不應考量進行最佳化的資料夾路徑</td>
            <td>資料夾路徑的陣列</td>
            <td>如果您想要改善效能，或是不要讓特定路徑中的內容被最佳化，您可以將磁碟區上的特定路徑排除在最佳化的考量之外。</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>指定後續處理工作期間重複資料刪除要在磁碟區上使用的 IO 平行處理層級 (IO 佇列)</td>
            <td>正整數，範圍 1-36</td>
            <td>修改此值的主要原因是藉由限制允許重複資料刪除在磁碟區上使用的 IO 佇列數目，減少對高 IO 工作負載的效能影響。 請注意，修改此設定的預設值可能會導致重複資料刪除&#39;s 的後置處理作業執行速度變慢。</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>檔案建立之後，在被視為需要依原則進行最佳化之前的天數。</td>
            <td>正整數 (包括零)</td>
            <td><strong>預設</strong>和 <strong>HyperV</strong> 使用類型會將此值設定為 3，來將熱門或最近所建立檔案的效能最佳化。 如果您想要更積極進行重複資料刪除，或如果您不在意因為重複資料刪除而產生相關聯的額外延遲，您可以修改此值。</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>檔案在被視為需要依原則進行最佳化時的最小檔案大小</td>
            <td>大於 32 KB的正整數 (位元組)</td>
            <td>變更此值的主要原因是為了排除最佳化價值有限的小型檔案，以節省運算時間。</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>區塊是否應在放入區塊存放區之前壓縮</td>
            <td>True/False</td>
            <td>某些類型的檔案 (特別是多媒體檔案和已壓縮的檔案類型) 可能無法妥善壓縮。 此設定可讓您針對磁碟區上的所有檔案關閉壓縮。 如果您正在最佳化的資料集包含許多已壓縮的檔案，這會是理想的設定。</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>區塊進入區塊存放區之前，其區塊不應被壓縮的檔案類型</td>
            <td>副檔名的陣列</td>
            <td>某些類型的檔案 (特別是多媒體檔案和已壓縮的檔案類型) 可能無法妥善壓縮。 此設定可針對那些檔案關閉壓縮，以節省 CPU 資源。</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>啟用時，具有作用中控制代碼的檔案本身會被視為需要依原則進行最佳化。</td>
            <td>True/false</td>
            <td>如果您的工作負載持續開啟檔案很長的時間，請啟用此設定。 如果未啟用此設定，檔案就永遠不會進行最佳化工作負載是否開啟的控制代碼，即使它&#39;s 偶爾才會附加在結尾的資料。</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>啟用時，MinimumFileAge 值會套用至檔案的區段，而非整個檔案。</td>
            <td>True/false</td>
            <td>如果您的工作負載會使用大型、經常編輯的檔案，而其中大部分的檔案內容不會更動，請啟用此設定。 如果未啟用此設定，即使大部分的檔案內容已準備好最佳化，這些檔案也將永遠不會進行最佳化，因為它們持續在變更。</td>
        </tr>
        <tr>
            <td>驗證</td>
            <td>啟用時，如果區塊的雜湊和我們在區塊存放區中已經擁有的區塊相符，就會逐個位元組比較該區塊以確保它們完全相同。</td>
            <td>True/false</td>
            <td>這是一項完整性功能，可確保比較區塊的雜湊演算法在比較實際上不同但擁有相同雜湊的兩個資料區塊時，不會發生錯誤。 實際上，這幾乎不太可能發生。 啟用驗證功能會對最佳化工作增加相當大的負擔。</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-dedup-system-settings"></a>修改重複資料刪除全系統設定
重複資料刪除有其他的全系統設定，可透過[登錄編輯程式](https://technet.microsoft.com/library/cc755256(v=ws.11).aspx)來設定。 這些設定會套用到在系統上執行的所有工作和磁碟區。 編輯登錄時，請務必非常小心。

例如，您可能想要停用完整記憶體回收。 如需關於這為何可能對您的案例有用的詳細資訊，請參閱[常見問題集](#faq-why-disable-full-gc)。 若要使用 PowerShell 編輯登錄：

* 如果重複資料刪除正在叢集中執行：
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* 如果重複資料刪除未在叢集中執行：
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a id="modifying-dedup-system-settings-available-settings"></a>可用的全系統設定
<table>
    <thead>
        <tr>
            <th style="min-width:125px">設定名稱</th>
            <th>定義</th>
            <th>接受的值</th>
            <th>為什麼要變更此值？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>此設定可允許工作使用比重複資料刪除判斷的實際可用記憶體量更多的記憶體。 例如，設定為 300 表示工作使用的記憶體量必須達到所指派記憶體的三倍，才會取消工作。</td>
            <td>正整數 (300 的值表示 300% 或 3 倍)</td>
            <td>如果您有另一項工作會在重複資料刪除使用更多記憶體時停止</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>此設定可設定一般記憶體回收工作變成<a href="advanced-settings.md#faq-full-v-regular-gc" data-raw-source="[full Garbage Collection jobs](advanced-settings.md#faq-full-v-regular-gc)">完整記憶體回收工作</a>的間隔。 設定為 n 代表每第 n<sup></sup> 次工作將會是完整記憶體回收工作。 請注意，完整記憶體回收對<a href="understand.md#usage-type-backup" data-raw-source="[Backup Usage Type](understand.md#usage-type-backup)">備份使用類型</a>的磁碟區永遠停用（無論登錄值）。 <code>Start-DedupJob -Type GarbageCollection -Full</code> 如果可以使用完整記憶體回收需要備份的磁碟區上。</td>
            <td>整數（-1 指出已停用）</td>
            <td>請參閱<a href="advanced-settings.md#faq-why-disable-full-gc" data-raw-source="[this frequently asked question](advanced-settings.md#faq-why-disable-full-gc)">此常見問題集</a></td>
        </tr>
    </tbody>
</table>

## <a id="faq"></a>常見問題集
<a id="faq-use-responsibly"></a>**我變更重複資料刪除設定，和現在工作速度很慢或無法完成，或我的工作負載效能降低。為什麼？**  
這些設定提供您許多強大的功能來控制重複資料刪除的執行方式。 請謹慎使用這些設定，並[監視效能](run.md#monitoring-dedup)。

<a id="faq-running-dedup-jobs-manually"></a>**我想要立即執行重複資料刪除工作，但我不想要建立新的排程，要怎麼做呢？**  
可以，[所有的工作均可手動執行](run.md#running-dedup-jobs-manually)。

<a id="faq-full-v-regular-gc"></a>**完整和一般記憶體回收之間的差異為何？**  
[記憶體回收](understand.md#job-info-gc)有兩種類型：

- 「一般記憶體回收」  會使用統計演算法來尋找符合特定條件 (記憶體與 IOPS 不足) 的大型未被參考區塊。 一般記憶體回收只有在未被參考區塊的百分比達到下限時，才會壓縮區塊存放區容器。 相較於完整記憶體回收，此類型記憶體回收的執行速度更快且使用的資源較少。 一般記憶體回收工作的預設排程是一星期執行一次。
- 「完整記憶體回收」  會執行更完整的工作，以尋找未參考區塊並釋放更多磁碟空間。 完整記憶體回收會壓縮每一個容器，即使容器內只有單一區塊未被參考。 如果在最佳化工作期間曾發生當機或電源中斷，完整記憶體回收也會釋放曾經使用的空間。 完整記憶體回收工作將會 100% 復原重複資料刪除磁碟區上可復原的可用空間，代價則是需要比執行一般記憶體回收工作時更多的時間與系統資源。 與一般記體回收工作相比，完整記憶體回收工作通常會尋找並釋放最多多 5% 的額外未被參考資料。 完整記憶體回收工作的預設排程是在每排程四次記憶體回收時執行。

<a id="faq-why-disable-full-gc"></a>**為什麼要停用完整記憶體回收？**  
- 記憶體回收可能會對磁碟區的存留期陰影複製和增量備份大小產生不利影響。 執行完整記憶體回收工作時，可能會看到高區塊或 I/O 密集性工作負載的效能降低。           
- 如果您知道系統當機，您可以從 PowerShell 手動執行完整記憶體回收工作來清除流失的部分。
