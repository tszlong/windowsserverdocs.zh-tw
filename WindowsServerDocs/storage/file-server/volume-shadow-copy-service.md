---
title: 磁碟區陰影複製服務
ms.date: 01/30/2019
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 44f0db935e50bf7976612edc4317b4212818f84d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950742"
---
# <a name="volume-shadow-copy-service"></a>磁碟區陰影複製服務

適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 10、Windows 8.1、Windows 8、Windows 7

備份和還原重要商務資料可能會因為下列問題而非常複雜：

  - 您通常需要在產生資料的應用程式仍在執行時備份資料。 這表示某些資料檔案可能已開啟，或可能處於不一致的狀態。

  - 如果資料集很大，要一次備份所有檔案可能很困難。

若要正確執行備份及還原作業，備份應用程式、要備份的企業營運應用程式，以及儲存管理硬體和軟體之間都需要密切協調。 Windows Server®2003 中引進的磁碟區陰影複製服務 (VSS) 可協助這些元件進行交談，使其更完美地搭配運作。 當所有元件都支援 VSS 時，您就可以使用這些元件來備份應用程式資料，而不需要使應用程式離線。

VSS 會針對要備份的資料，協調用於建立一致陰影複製 (也稱為快照或時間點複本) 時所需的動作。 陰影複製可以依現狀使用，也可以在下列情況下使用：

  - 您想要備份應用程式資料和系統狀態資訊，包括將資料封存到另一個硬碟、磁帶或其他卸載式媒體。

  - 您正在進行資料採礦。

  - 您正在執行磁碟到磁碟的備份。

  - 為了快速復原遺失的資料，您需要將資料還原到原始 LUN 或還原至全新的 LUN 以取代失敗的原始 LUN。


使用 VSS 的 Windows 功能和應用程式包含下列各項：

  - [Windows Server Backup](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkId=180891)

  - [共用資料夾的陰影複製](https://go.microsoft.com/fwlink/?linkid=142874) (https://go.microsoft.com/fwlink/?LinkId=142874)

  - [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=180892) (https://go.microsoft.com/fwlink/?LinkId=180892)

  - [系統還原](https://go.microsoft.com/fwlink/?linkid=180893) (https://go.microsoft.com/fwlink/?LinkId=180893)


## <a name="how-volume-shadow-copy-service-works"></a>磁碟區陰影複製服務的運作方式

完整的 VSS 解決方案需要下列所有基本元件：

**VSS 服務**   Windows 作業系統的一部分，用來確保其他元件可以適當地彼此通訊，並一起運作。

**VSS 要求者**   要求實際建立陰影複製 (或是匯入或刪除陰影複製等其他高階作業) 的軟體。 一般來說，這是指備份應用程式。 Windows Server Backup 公用程式和 System Center Data Protection Manager 應用程式都是 VSS 要求者。 非 Microsoft® VSS 要求者包含幾乎所有在 Windows 上執行的備份軟體。

**VSS 寫入器**   該元件可確保我們有一致的資料集可進行備份。 此元件通常會在企業營運應用程式中提供，例如 SQL Server®或 Exchange Server。 適用於各種 Windows 元件 (例如登錄) 的 VSS 寫入器會隨附於 Windows 作業系統中。 許多需要在備份期間保證資料一致性的 Windows 版應用程式都包含非 Microsoft VSS 寫入器。

**VSS 提供者**   用於建立和維護陰影複製的元件。 這可能會出現在軟體或硬體中。 Windows 作業系統包含使用「寫入時複製」的 VSS 提供者。 如果您使用存放區域網路 (SAN)，請務必安裝適用於 SAN 的 VSS 硬體提供者 (如果有提供的話)。 硬體提供者會從主機作業系統卸載陰影複製的建立和維護工作。

下圖說明 VSS 服務如何與要求者、寫入器和提供者合作，以建立磁碟區的陰影複製。

![磁碟區陰影複製服務的架構圖](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**圖 1**   磁碟區陰影複製服務的架構圖

### <a name="how-a-shadow-copy-is-created"></a>陰影複製的建立方式

本節藉由列出建立陰影複製所需採取的步驟，將要求者、寫入器和提供者的各種角色放入內容中。 下圖顯示磁碟區陰影複製服務如何控制要求者、寫入器和提供者的整體協調工作。

![磁碟區陰影複製服務運作方式的圖表](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**圖 2** 陰影複製建立程序

為建立陰影複製，要求者、寫入器和提供者會執行下列動作：

1.  要求者會要求磁碟區陰影複製服務列舉寫入器、收集寫入器中繼資料，以及準備建立陰影複製。

2.  每個寫入器都會針對需要備份的元件和資料存放區建立 XML 描述，並將其提供給磁碟區陰影複製服務。 寫入器也會定義所有元件適用的還原方法。 磁碟區陰影複製服務會將寫入器的描述提供給要求者，讓其選取要備份的元件。

3.  磁碟區陰影複製服務會通知所有寫入器準備其資料，以進行陰影複製。

4.  每個寫入器都會視情況準備資料，例如完成所有開啟的交易、更新交易記錄及清除快取。 當資料準備好要進行陰影複製時，寫入器會通知磁碟區陰影複製服務。

5.  磁碟區陰影複製服務會指示寫入器暫時將應用程式的寫入 I/O 要求凍結幾秒鐘 (讀取 I/O 要求仍可繼續)，這是建立磁碟區陰影複製的必要動作。 應用程式凍結的時間不能超過 60 秒。 磁碟區陰影複製服務會清空檔案系統緩衝區，然後凍結檔案系統，以確保檔案系統中繼資料已正確記錄，而要進行陰影複製的資料會以一致的順序寫入。

6.  磁碟區陰影複製服務會指示提供者建立陰影複製。 陰影複製的建立時間不會超過 10 秒，在這段期間，所有對檔案系統進行的寫入 I/O 要求都會維持在凍結狀態。

7.  磁碟區陰影複製服務會釋出檔案系統的寫入 I/O 要求。

8.  VSS 會指示寫入器解除凍結應用程式的寫入 I/O 要求。 此時，應用程式就可以繼續將資料寫入正在陰進行影複製的磁碟。


> [!NOTE]
> 如果寫入器保持在凍結狀態超過 60 秒，或如果提供者認可陰影複製的時間超過 10 秒，則陰影複製的建立作業可能會中止。
<br>

9. 要求者可以重試該程序 (返回步驟1)，或通知系統管理員稍後再試一次。

10. 如果已成功建立陰影複製，則磁碟區陰影複製服務會將陰影複製的位置資訊傳回給要求者。 在某些情況下，陰影複製可以暫時作為讀寫磁碟區使用，讓 VSS 和一個或多個應用程式可以在陰影複製完成前修改陰影複製內容。 VSS 和應用程式進行變更之後，陰影複製就會變成唯讀狀態。 此階段稱為「自動復原」，用來在陰影複製磁碟區上復原陰影複製建立之前未完成的任何檔案系統交易或應用程式交易。


### <a name="how-the-provider-creates-a-shadow-copy"></a>提供者建立陰影複製的方式

硬體或軟體陰影複製提供者會使用下列其中一種方法來建立陰影複製：

**完整複製**   此方法會在指定時間點建立原始磁碟區的完整複本 (稱為「完整複本」或「再製」)。 此複本為唯讀狀態。

**寫入時複製**   此方法不會複製原始磁碟區。 而是複製指定時間點之後磁碟區上的所有變更 (已完成的寫入 I/O 要求) 來建立差異複本。

**寫入時重新導向**   此方法不會複製原始磁碟區，也不會在指定時間點之後對原始磁碟區進行任何變更。 而是會將所有變更重新導向不同的磁碟區，藉此來建立差異複本。

## <a name="complete-copy"></a>完整副本

完整副本通常是藉由建立「分割鏡像」來建立，如下所示：

1. 原始磁碟區和陰影複製磁碟區是一組鏡像磁碟區。

2. 陰影複製磁碟區會與原始磁碟區分開。 此動作會中斷鏡像連結。


鏡像連結中斷之後，原始磁碟區和陰影複製磁碟區就會各自獨立。 原始磁碟區會繼續接受所有變更 (寫入 I/O 要求)，而陰影複製磁碟區在中斷連結時仍是原始資料的完整唯讀複本。

### <a name="copy-on-write-method"></a>寫入時複製方法

在「寫入時複製」方法中，如果原始磁碟區發生變更 (但必須在寫入 I/O 要求完成之前)，系統就會讀取要修改的每個區塊，然後將其寫入磁碟區的陰影複製存放區域 (也稱為「相異區域」)。 陰影複製存放區域可以位於相同的磁碟區或不同的磁碟區上。 這會將資料區塊複本保留在原始磁碟區上，直到變更將其覆寫。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>時間</th>
<th>來源資料 (狀態和資料)</th>
<th>陰影複製 (狀態和資料)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始資料：1 2 3 4 5</p></td>
<td><p>無複本：—</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>快取中的資料變更：3 變成 3'</p></td>
<td><p>已建立陰影複製 (僅限差異部分)：3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>已覆寫原始資料：1 2 3' 4 5</p></td>
<td><p>陰影複製上儲存的差異和索引：3</p></td>
</tr>
</tbody>
</table>

**表 1**   建立陰影複製的寫入時複製方法

「寫入時複製」方法是建立陰影複製的快速方法，因為其只會複製已變更的資料。 相異區域中的已複製區塊可以與原始磁碟區上的已變更資料結合，以便將磁碟區還原到進行任何變更之前的狀態。 如果有許多變更，則「寫入時複製方法」的成本可能會變高。

### <a name="redirect-on-write-method"></a>寫入時重新導向方法

在「寫入時重新導向」方法中，如果原始磁碟區收到變更 (寫入 I/O 要求)，則此變更不會套用至原始磁碟區。 相反地，變更會寫入另一個磁碟區的陰影複製存放區域。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>時間</th>
<th>來源資料 (狀態和資料)</th>
<th>陰影複製 (狀態和資料)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始資料：1 2 3 4 5</p></td>
<td><p>無複本：—</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>快取中的資料變更：3 變成 3'</p></td>
<td><p>已建立陰影複製 (僅限差異部分)：3'</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>原始資料未變更：1 2 3 4 5</p></td>
<td><p>陰影複製上儲存的差異和索引：3'</p></td>
</tr>
</tbody>
</table>

**表 2**   建立陰影複製的寫入時重新導向方法

如同「寫入時複製」方法，「寫入時重新導向」方法也是建立陰影複製的快速方法，因為其只會複製資料中已變更的部分。 相異區域中已複製的區塊可以與原始磁碟區上未變更的資料結合，以建立完整的最新資料複本。 如果有許多讀取 I/O 要求，則「寫入時重新導向」方法的成本可能會變高。

## <a name="shadow-copy-providers"></a>陰影複製提供者

陰影複製提供者有兩種類型：硬體型提供者和軟體型提供者。 另外還有一個系統提供者，也就是 Windows 作業系統內建的軟體提供者。

### <a name="hardware-based-providers"></a>硬體型提供者

硬體型陰影複製提供者可與硬體儲存裝置介面卡或控制器搭配使用，藉此作為磁碟區陰影複製服務與硬體層級之間的介面。 建立和維護陰影複製的工作是由儲存裝置陣列執行。

硬體提供者一律取用整個 LUN 的陰影複製，但是磁碟區陰影複製服務只會公開所要求磁碟區的陰影複製。

硬體型陰影複製提供者會使用磁碟區陰影複製服務功能來定義時間點、允許資料同步、管理陰影複製，以及提供備份應用程式的通用介面。 不過，磁碟區陰影複製服務不會指定硬體型提供者用以產生和維護陰影複製的基礎機制。

### <a name="software-based-providers"></a>軟體型提供者

軟體型陰影複製提供者通常會在檔案系統和磁碟區管理員軟體之間，攔截和處理軟體層中的讀取和寫入 I/O 要求。

這些提供者會實作為使用者模式 DLL 元件，以及至少一個核心模式裝置驅動程式 (通常是儲存體篩選驅動程式)。 與硬體型提供者不同的是，軟體型提供者會在軟體層級建立陰影複製，而不是硬體層級。

軟體型陰影複製提供者必須藉由存取資料集來維護磁碟區的「時間點」檢視，以重新建立陰影複製建立前的磁碟區狀態。 例如，系統提供者的「寫入時複製」技術。 不過，磁碟區陰影複製服務不會限制軟體型提供者使用什麼技術來建立和維護陰影複製。

比起硬體型提供者，軟體型提供者可用於更廣泛的儲存平台，而且同樣可與基本磁碟或邏輯磁碟區搭配使用。 (邏輯磁碟區是結合兩個或多個磁碟可用空間所建立的磁碟區)。與硬體陰影複製相比，軟體提供者會耗用作業系統資源來維護陰影複製。

如需有關基本磁碟的詳細資訊，請參閱 TechNet 上的[什麼是基本磁碟和磁碟區？](https://go.microsoft.com/fwlink/?linkid=180894) (https://go.microsoft.com/fwlink/?LinkId=180894) 。

### <a name="system-provider"></a>系統提供者

Windows 作業系統中會提供一個陰影複製提供者 (系統提供者)。 雖然 Windows 中提供了預設的提供者，但其他廠商也可以自由提供針對其儲存體硬體和軟體應用程式進行最佳化的實作項目。

為了維護陰影複製中所包含的磁碟區「時間點」檢視，系統提供者會使用「寫入時複製」技術。 開始建立陰影複製之後，磁碟區上已修改的區塊複本都會儲存在陰影複製存放區域中。

系統提供者可以公開生產磁碟區，以便正常寫入和讀取。 需要陰影複製時，其會以邏輯方式將差異套用到生產磁碟區上的資料，以公開完整的陰影複製。

系統提供者的陰影複製存放區域必須位於 NTFS 磁碟區上。 要進行陰影複製的磁碟區不需要是 NTFS 磁碟區，但是系統上掛接的磁碟區必須至少有一個是 NTFS 磁碟區。

組成系統提供者的元件檔案是 swprv.dll 和 volsnap.sys。

### <a name="in-box-vss-writers"></a>內建 VSS 寫入器

Windows 作業系統包含一組 VSS 寫入器，負責列舉各種 Windows 功能所需的資料。

如需這些寫入器的詳細資訊，請參閱下列Microsoft Docs 網頁：

- [內建 VSS 寫入器](/windows/win32/vss/in-box-vss-writers) (https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers)


## <a name="how-shadow-copies-are-used"></a>陰影複製的使用方式

除了備份應用程式資料和系統狀態資訊之外，陰影複製也可以用於許多用途，包括下列各項：

  - 還原 LUN (LUN 重新同步和 LUN 交換)

  - 還原個別檔案 (共用資料夾陰影複製)

  - 使用可轉移的陰影複製來進行資料採礦


### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>還原 LUN (LUN 重新同步和 LUN 交換)

在 Windows Server 2008 R2 和 Windows 7 中，VSS 要求者可以使用名為 LUN 重新同步 (LUN resynchronization 或 LUN resync) 的硬體陰影複製提供者功能。 這是一種快速復原配置，可讓應用程式系統管理員將陰影複製中的資料還原到原始 LUN 或新的 LUN。

陰影複製可以是完整再製或差異陰影複製。 不論是哪一種情況，在重新同步作業結束時，目的地 LUN 的內容都會與陰影複製 LUN 相同。 在重新同步作業期間，陣列會執行從陰影複製到目的地 LUN 的區塊層級複製。


> [!NOTE]
> 陰影複製必須是可轉移的硬體陰影複製。
<br>


在重新同步作業開始之後，大部分的陣列都會讓生產 I/O 作業在短時間內繼續。 重新同步作業進行時，讀取要求會重新導向至陰影複製 LUN，並將要求寫入目的地 LUN。 這可讓陣列復原非常大型的資料集，並在數秒內恢復正常作業。

LUN 重新同步與 LUN 交換不同。 LUN 交換是在 Windows Server 2003 SP1 之後，VSS 所支援的快速復原案例。 LUN 交換的過程中會匯入陰影複製，然後轉換成讀寫磁碟區。 此轉換是無法復原的作業，而且轉換之後，磁碟區和基礎 LUN 將無法使用 VSS API 來控制。 下列清單說明 LUN 重新同步與 LUN 交換的比較：

  - 在 LUN 重新同步中，陰影複製並不會改變，因此可以使用數次。 在 LUN 交換中，只能使用一次陰影複製來進行復原。 對於最重視安全性的系統管理員來說，這一點很重要。 使用 LUN 重新同步時，如果第一次發生錯誤，要求者可以重試整個還原作業。

  - 在 LUN 交換結束時，您會針對生產 I/O 要求使用陰影複製 LUN。 基於這個理由，陰影複製 LUN 必須使用與原始生產 LUN 相同的儲存體品質，以確保效能在復原作業之後不受影響。 如果改用 LUN 重新同步，硬體提供者可以在儲存體上維護陰影複製，這比具生產品質的儲存空間便宜。

  - 如果目的地 LUN 無法使用且需要重新建立，LUN 交換可能會比較實惠，因為其不需要目的地 LUN。


> [!WARNING]
> 列出的所有作業都是 LUN 層級作業。 如果您嘗試使用 LUN 重新同步來復原特定磁碟區，您就不一定要還原所有共用 LUN 的其他磁碟區。
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>還原個別檔案 (共用資料夾陰影複製)

共用資料夾陰影複製會使用磁碟區陰影複製服務來提供檔案的時間點複本，這些檔案位於共用網路資源 (例如檔案伺服器) 上。 透過共用資料夾陰影複製，使用者可以快速復原網路上已刪除或已變更的檔案。 由於他們可以在沒有系統管理員協助的情況下這麼做，所以共用資料夾陰影複製可以提高生產力並降低管理成本。

如需共用資料夾陰影複製的詳細資訊，請參閱 TechNet 上的 [共用資料夾陰影複製](https://go.microsoft.com/fwlink/?linkid=180898) (https://go.microsoft.com/fwlink/?LinkId=180898) 。

### <a name="data-mining-by-using-transportable-shadow-copies"></a>使用可轉移的陰影複製來進行資料採礦

透過為搭配磁碟區陰影複製服務而設計的硬體提供者，您可以建立可傳送的陰影複製，以便匯入至相同子系統內的伺服器 (例如 SAN)。 這些陰影複製可用來植入具有唯讀資料的生產安裝或測試安裝，以用於資料採礦。

透過磁碟區陰影複製服務，以及具有硬體提供者 (為搭配磁碟區陰影複製服務而設計) 的儲存裝置陣列，您就可以在一部伺服器上建立來源資料磁碟區的陰影複製，然後將陰影複製匯入到其他伺服器 (或回到相同伺服器)。 此程序會在幾分鐘內完成，且不論資料的大小為何。 轉移程序會透過一系列步驟來完成，其中使用了支援可轉移陰影複製的陰影複製要求者 (儲存管理應用程式)。

## <a name="to-transport-a-shadow-copy"></a>若要轉移陰影複製

1.  在伺服器上建立來源資料的可轉移陰影複製。

2.  將陰影複製匯入到連線至 SAN 的伺服器 (您可以匯入至不同伺服器或相同伺服器)。

3.  資料現在已可供使用。

![如何在兩個伺服器之間傳輸陰影複製的圖表](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**圖 3**   在兩部伺服器之間建立和轉移陰影複製


> [!NOTE]
> 在 Windows Server 2003 上建立的可轉移陰影複製無法匯入執行 Windows Server 2008 或 Windows Server 2008 R2 的伺服器。 在 Windows Server 2008 或 Windows Server 2008 R2 上建立的可轉移陰影複製無法匯入執行 Windows Server 2003 的伺服器。 但是，在 Windows Server 2008 上建立的陰影複製可以匯入執行 Windows Server 2008 R2 的伺服器，反之亦然。
<br>


陰影複製是唯讀的。 如果您想要將陰影複製轉換成讀取/寫入 LUN，除了磁碟區陰影複製服務以外，您還可以使用虛擬磁碟服務型的儲存管理應用程式 (包括某些要求者)。 藉由使用此應用程式，您可以從磁碟區陰影複製服務管理中移除陰影複製，並將其轉換成讀取/寫入 LUN。

在執行 Windows Server 2003 Enterprise Edition、Windows Server 2003 Datacenter Edition、Windows Server 2008 或 Windows Server 2008 R2 的電腦上，磁碟區陰影複製服務轉移是一個進階的解決方案。 只有在儲存裝置陣列上有硬體提供者時，此服務才會運作。 陰影複製轉移可以用於許多用途，包括磁帶備份、資料採礦和測試。

## <a name="frequently-asked-questions"></a>常見問題集

此常見問題集會為系統管理員回答有關磁碟區陰影複製服務 (VSS) 的問題。 如需有關 VSS 應用程式開發介面的詳細資訊，請參閱 Windows 開發人員中心程式庫中的[磁碟區陰影複製服務](https://go.microsoft.com/fwlink/?linkid=180899) (https://go.microsoft.com/fwlink/?LinkId=180899) 。

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>磁碟區陰影複製服務在何時引進？ 有哪些 Windows 作業系統版本可使用該服務？

VSS 是在 Windows XP 中引進的。 其適用於 Windows XP、Windows Server 2003、Windows Vista®、Windows Server 2008、Windows 7 和 Windows Server 2008 R2。

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>陰影複製和備份有何不同？

在硬碟備份的情況下，建立陰影複製也是備份。 您可以從陰影複製中複製資料來進行還原，或使用陰影複製來快速復原，例如 LUN 重新同步或 LUN 交換。

將資料從陰影複製中複製到磁帶或其他卸載式媒體時，儲存在媒體上的內容會構成備份。 複製資料之後，您就可以刪除陰影複製本身。

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>磁碟區陰影複製服務支援的最大磁碟區大小為何？

磁碟區陰影複製服務支援高達 64 TB 的磁碟區大小。

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>我在 Windows Server 2008 上進行了備份。 我可以在 Windows Server 2008 R2 上將其還原嗎？

這取決於您所使用的備份軟體。 大部分的備份程式都可在資料上支援此案例，但不適用於系統狀態備份。

在這兩個 Windows 版本上建立的陰影複製都可以用在另一個版本上。

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>我在 Windows Server 2003 上進行了備份。 我可以在 Windows Server 2008 上將其還原嗎？

這取決於您所使用的備份軟體。 您無法在 Windows Server 2008 上使用您在 Windows Server 2003 上建立的陰影複製。 同樣的，您無法在 Windows Server 2003 上還原您在 Windows Server 2008 上建立的陰影複製。

### <a name="how-can-i-disable-vss"></a>如何停用 VSS？

您可以使用 Microsoft Management Console 來停用磁碟區陰影複製服務。 不過，您不應該這麼做。 停用 VSS 會對依附其使用的任何軟體 (例如系統還原和 Windows Server Backup) 造成不良影響。

若需相關資訊，請參閱下列 Microsoft TechNet 網站：

- [系統還原](https://go.microsoft.com/fwlink/?linkid=157113) (https://go.microsoft.com/fwlink/?LinkID=157113)

- [Windows Server Backup](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkID=180891)


### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>我可以從陰影複製中排除檔案來節省空間嗎？

VSS 的設計目的是要建立整個磁碟區的陰影複製。 系統會自動在陰影複製中省略暫存檔案 (例如分頁檔案) 來節省空間。

若要從陰影複製中排除特定檔案，請使用下列登錄機碼：**FilesNotToSnapshot**。


> [!NOTE]
> <STRONG>FilesNotToSnapshot</STRONG> 登錄機碼僅供應用程式使用。 嘗試使用此登錄機碼的使用者會遇到下列限制：
> <br>
> <UL>
> <LI>無法在 Windows Server 上使用舊版功能建立的陰影複製中刪除檔案。<BR><BR>
> <LI>無法從共用資料夾陰影複製中刪除檔案。<BR><BR>
> <LI>可以從使用 <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](../../administration/windows-commands/diskshadow.md)">Diskshadow</a> 公用程式所建立的陰影複製中刪除檔案，但無法從使用 <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](../../administration/windows-commands/vssadmin.md)">Vssadmin</a> 公用程式所建立的陰影複製中刪除檔案。<BR><BR>
> <LI>檔案會盡可能以各種方式從陰影複製中刪除。 但這表示我們無法保證能將其刪除。<BR><BR></LI></UL>


如需詳細資訊，請參閱 MSDN 上的[從陰影複製排除檔案](https://go.microsoft.com/fwlink/?linkid=180904) (https://go.microsoft.com/fwlink/?LinkId=180904) 。

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>我的非 Microsoft 備份程式失敗，發生 VSS 錯誤。 我該怎麼處理？

在建立備份程式的公司網站上查看 [產品支援] 區段。 那邊可能會有用來修正問題的產品更新供您下載並安裝。 如果沒有，請聯絡公司的產品支援部門。

系統管理員可以使用下列 Microsoft TechNet 程式庫網站上的 VSS 疑難排解資訊，收集 VSS 相關問題的診斷資訊。

如需詳細資訊，請參閱 TechNet 上的[磁碟區陰影複製服務](https://go.microsoft.com/fwlink/?linkid=180905) (https://go.microsoft.com/fwlink/?LinkId=180905) 。

### <a name="what-is-the-diff-area"></a>什麼是「相異區域」？

陰影複製存放區域 (或「相異區域」) 用於儲存系統軟體提供者建立的陰影複製資料。

### <a name="where-is-the-diff-area-located"></a>相異區域位於何處？

相異區域可以位於任何本機磁碟區上。 不過，其必須位於有足夠空間讓其儲存的 NTFS 磁碟區上。

### <a name="how-is-the-diff-area-location-determined"></a>如何決定相異區域位置？

系統會依此順序評估下列準則，進而決定相異區域的位置：

  - 如果磁碟區已有現有的陰影複製，則會使用該位置。

  - 如果原始磁碟區與陰影複製磁碟區位置之間有預先設定的手動關聯，則會使用該位置。

  - 如果前兩個準則未能提供位置，則陰影複製服務會根據可用空間來選擇位置。 如果有多個磁碟區正在進行陰影複製，則陰影複製服務會根據可用空間的大小 (以遞減順序)，建立可能的快照集位置清單。 提供的位置數目等於進行陰影複製的磁碟區數目。

  - 如果要進行陰影複製的磁碟區是其中一個可能的位置，則會建立本機關聯。 否則，將會與具有最多可用空間的磁碟區建立關聯。


### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS 是否可以建立非 NTFS 磁碟區的陰影複製？

是。 不過，持續性陰影複製只能針對 NTFS 磁碟區進行。 此外，系統上掛接的磁碟區必須至少有一個是 NTFS 磁碟區。

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>一次最多可以建立幾個陰影複製？

在一組陰影複製中，可進行陰影複製的磁碟區數目上限為 64 個。 請注意，這與陰影複製的數目不同。

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>針對單一磁碟區，我最多可以維護多少個系統提供者建立的軟體陰影複製？

每個磁碟區的軟體陰影複製數目上限為 512 個。 不過，根據預設，您只能維護 64 個「共用資料夾陰影複製」功能所使用的陰影複製。 若要變更「共用資料夾陰影複製」功能的限制，請使用下列登錄機碼：**MaxShadowCopies**。

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>如何控制用於陰影複製儲存空間的空間？

輸入 **vssadmin resize shadowstorage**命令。

如需詳細資訊，請參閱 TechNet 上的 [Vssadmin resize shadowstorage](https://go.microsoft.com/fwlink/?linkid=180906) (https://go.microsoft.com/fwlink/?LinkId=180906) 。

### <a name="what-happens-when-i-run-out-of-space"></a>當空間用完時，會發生什麼事？

系統會刪除磁碟區的陰影複製 (從最舊的陰影複製開始)。

## <a name="volume-shadow-copy-service-tools"></a>磁碟區陰影複製服務工具

Windows 作業系統提供下列工具來使用 VSS：

  - [DiskShadow](https://go.microsoft.com/fwlink/?linkid=180907) (https://go.microsoft.com/fwlink/?LinkId=180907)

  - [VssAdmin](https://go.microsoft.com/fwlink/?linkid=84008) (https://go.microsoft.com/fwlink/?LinkId=84008)


### <a name="diskshadow"></a>DiskShadow

DiskShadow 是 VSS 要求者，可讓您用來管理系統上可以擁有的所有硬體和軟體快照集。 DiskShadow 包含如下所示的命令：

  - **list**：列出 VSS 寫入器、VSS 提供者和陰影複製

  - **create**：建立新的陰影複製

  - **import**：匯入可轉移的陰影複製

  - **expose**：公開持續性的陰影複製 (例如磁碟機代號)

  - **revert**：將磁碟區還原回指定的陰影複製


這項工具主要提供給 IT 專業人員使用，但是開發人員在測試 VSS 寫入器或 VSS 提供者時，可能也會發現該工具很實用。

DiskShadow 僅適用於 Windows Server 作業系統。 其無法在 Windows 用戶端作業系統上使用。

### <a name="vssadmin"></a>VssAdmin

VssAdmin 可用來建立、刪除和列出陰影複製的相關資訊。 也可以用來調整陰影複製存放區域的大小 (「相異區域」)。

VssAdmin 包含如下所示的命令：

  - **create shadow**：建立新的陰影複製

  - **delete shadows**：刪除陰影複製

  - **list providers**：列出所有已註冊的 VSS 提供者

  - **list writers**：列出所有已訂閱的 VSS 寫入器

  - **resize shadowstorage**：變更陰影複製存放區域的大小上限


VssAdmin 只能用來管理系統軟體提供者所建立的陰影複製。

VssAdmin 可在 Windows 用戶端和 Windows Server 作業系統版本上使用。

## <a name="volume-shadow-copy-service-registry-keys"></a>磁碟區陰影複製服務的登錄機碼

下列登錄機碼可與 VSS 搭配使用：

  - **VssAccessControl**

  - **MaxShadowCopies**

  - **MinDiffAreaFileSize**


### <a name="vssaccesscontrol"></a>VssAccessControl

此機碼可用來指定哪些使用者有權存取陰影複製。

如需詳細資訊，請參閱 MSDN 網站的下列項目：

  - [寫入器的安全性考量](https://go.microsoft.com/fwlink/?linkid=157739) (https://go.microsoft.com/fwlink/?LinkId=157739)

  - [要求者的安全性考量](https://go.microsoft.com/fwlink/?linkid=180908) (https://go.microsoft.com/fwlink/?LinkId=180908)


### <a name="maxshadowcopies"></a>MaxShadowCopies

此機碼會指定最多可以在電腦中每個磁碟區上儲存多少用戶端可存取的陰影複製。 共用資料夾陰影複製會使用用戶端可存取的陰影複製。

如需詳細資訊，請參閱 MSDN 網站的下列項目：

[用於備份與還原的登錄機碼](https://go.microsoft.com/fwlink/?linkid=180909) (https://go.microsoft.com/fwlink/?LinkId=180909) 底下的 **MaxShadowCopies**

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

此機碼會指定陰影複製存放區域的最小初始大小 (以 MB 為單位)。

如需詳細資訊，請參閱 MSDN 網站的下列項目：

[用於備份與還原的登錄機碼](https://go.microsoft.com/fwlink/?linkid=180910) (https://go.microsoft.com/fwlink/?LinkId=180910) 底下的 **MinDiffAreaFileSize**

### <a name="supported-operating-system-versions"></a>支援的作業系統版本

下表列出 VSS 功能支援的最低作業系統版本。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>VSS 功能</th>
<th>最低支援的用戶端</th>
<th>最低支援的伺服器</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>LUN 重新同步</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong> 登錄機碼</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>可轉移的陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003 SP1</p></td>
</tr>
<tr class="even">
<td><p>硬體陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>先前的 Windows Server 版本</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>使用 LUN 交換快速復原</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003 SP1</p></td>
</tr>
<tr class="odd">
<td><p>硬體陰影複製的多重匯入</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>注意</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>這是可多次匯入陰影複製的功能。 一次只能執行一個匯入作業。
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>共用資料夾陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>可轉移的自動復原陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>並行備份工作階段 (最多 64 個)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>單一還原工作階段與備份並行</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 SP2</p></td>
</tr>
<tr class="even">
<td><p>最多 8 個還原工作階段與備份並行</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="additional-references"></a>其他參考資料

[Windows 開發人員中心內的磁碟區陰影複製服務](/windows/desktop/vss/volume-shadow-copy-service-overview)
