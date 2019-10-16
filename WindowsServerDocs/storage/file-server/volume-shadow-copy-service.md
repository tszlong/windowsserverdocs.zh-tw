---
title: 磁碟區陰影複製服務
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 19e07504dad49c5e23cc49630015529e2a746aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394454"
---
# <a name="volume-shadow-copy-service"></a>磁碟區陰影複製服務

適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2、Windows Server 2008、Windows 10、Windows 8.1、Windows 8、Windows 7

備份和還原重要的商務資料可能會因為下列問題而非常複雜：

  - 資料通常需要在產生資料的應用程式仍在執行時進行備份。 這表示某些資料檔案可能已開啟，或可能處於不一致的狀態。  
      
  - 如果資料集很大，就很難以一次備份所有檔案。  
      

正確執行備份和還原作業需要在備份應用程式、正在備份的企業營運應用程式，以及存放裝置管理硬體和軟體之間進行密切協調。 Windows Server®2003中引進的磁碟區陰影複製服務（VSS）可協助這些元件之間的交談，使其能夠更完美地搭配使用。 當所有元件都支援 VSS 時，您可以使用它們來備份您的應用程式資料，而不需要讓應用程式離線。

VSS 會協調建立所要備份之資料的一致陰影複製（也稱為快照集或時間點複本）所需的動作。 陰影複製可以依自己的方式使用，也可以在下列情況下使用：

  - 您想要備份應用程式資料和系統狀態資訊，包括將資料封存到另一個硬碟、磁帶或其他卸載式媒體。  
      
  - 您是資料採礦。  
      
  - 您正在執行磁片到磁片備份。  
      
  - 您需要將資料還原到原始 LUN 或全新的 LUN，以取代失敗的原始 LUN，以快速復原資料遺失。  
      

使用 VSS 的 Windows 功能和應用程式包含下列各項：

  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891)(http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [共用資料夾的陰影複製](http://go.microsoft.com/fwlink/?linkid=142874)(http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892)(http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [系統還原](http://go.microsoft.com/fwlink/?linkid=180893)(http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>磁碟區陰影複製服務的運作方式

完整的 VSS 解決方案需要下列所有基本元件：

   Windows 作業系統的 VSS 服務部分，可確保其他元件可以適當地彼此通訊，並一起運作。

**VSS**要求軟體提出實際的陰影複製建立（或其他高階作業，例如匯入或刪除它們）。    一般來說，這是備份應用程式。 Windows Server Backup 公用程式和 System Center Data Protection Manager 應用程式都是 VSS 要求者。 非 Microsoft® VSS 要求者包含幾乎所有在 Windows 上執行的備份軟體。

**VSS writer**    會保證我們具有一致的資料集來進行備份的元件。 這通常會在企業營運應用程式中提供，例如 SQL Server®或 Exchange Server。 適用于各種 Windows 元件（例如登錄）的 VSS 寫入器隨附于 Windows 作業系統中。 Windows 的許多應用程式都包含非 Microsoft VSS 寫入器，需要在備份期間保證資料的一致性。

**VSS 提供者**   會建立及維護陰影複製的元件。 這可能會發生在軟體或硬體中。 Windows 作業系統包含使用「寫入時複製」的 VSS 提供者。 如果您使用存放區域網路（SAN），請務必安裝適用于 SAN 的 VSS 硬體提供者（如果有提供的話）。 硬體提供者會從主機作業系統卸載建立和維護陰影複製的工作。

下圖說明 VSS 服務如何與要求者、寫入器和提供者協調，以建立磁片區的陰影複製。

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**圖 1**    磁碟區陰影複製服務的架構圖

### <a name="how-a-shadow-copy-is-created"></a>陰影複製的建立方式

本節藉由列出建立陰影複製所需採取的步驟，將要求者、寫入器和提供者的各種角色放入內容中。 下圖顯示磁碟區陰影複製服務如何控制要求者、寫入器和提供者的整體協調。

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**圖 2**陰影複製建立程式

若要建立陰影複製，要求者、寫入器和提供者會執行下列動作：

1.  要求者會詢問磁碟區陰影複製服務列舉寫入器、收集寫入器中繼資料，以及準備建立陰影複製。  
      
2.  每個寫入器都會針對需要備份的元件和資料存放區建立 XML 描述，並將其提供給磁碟區陰影複製服務。 寫入器也會定義用於所有元件的還原方法。 磁碟區陰影複製服務會將寫入器的描述提供給要求者，以選取要備份的元件。  
      
3.  磁碟區陰影複製服務會通知所有寫入器準備其資料以進行陰影複製。  
      
4.  每個寫入器都會適當地準備資料，例如完成所有開啟的交易、復原交易記錄，以及清除快取。 當資料準備好要進行陰影複製時，寫入器會通知磁碟區陰影複製服務。  
      
5.  在建立磁片區陰影複製所需的幾秒內，磁碟區陰影複製服務會告訴寫入器暫時凍結應用程式寫入 i/o 要求（讀取 i/o 要求仍然可能）。 應用程式凍結的時間不能超過60秒。 磁碟區陰影複製服務會排清檔案系統緩衝區，然後凍結檔案系統，以確保檔案系統中繼資料已正確錄製，而要陰影複製的資料則是以一致的順序寫入。  
      
6.  磁碟區陰影複製服務會告訴提供者建立陰影複製。 陰影複製建立期間的持續時間不會超過10秒，在這段期間，所有對檔案系統的寫入 i/o 要求都會維持凍結狀態。  
      
7.  磁碟區陰影複製服務會釋放檔案系統寫入 i/o 要求。  
      
8.  VSS 會指示寫入器解除凍結應用程式寫入 i/o 要求。 此時，應用程式可以繼續將資料寫入正在陰影複製的磁片。  
      

> [!NOTE]
> 如果寫入器保持在凍結狀態超過60秒，或如果提供者所花費的時間超過10秒，無法認可陰影複製，則可以中止陰影複製建立。 
<br>

9. 要求者可以重試進程（返回步驟1），或通知系統管理員稍後再試一次。  
      
10. 如果已成功建立陰影複製，則磁碟區陰影複製服務會將陰影複製的位置資訊傳回給要求者。 在某些情況下，陰影複製可以暫時做為讀寫磁片區使用，讓 VSS 和一或多個應用程式可以在陰影複製完成之前，修改陰影複製的內容。 VSS 和應用程式進行變更之後，陰影複製就會變成隻讀。 這個階段稱為「自動復原」，它是用來復原在陰影複製建立之前未完成之陰影複製磁片區上的任何檔案系統或應用程式交易。  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>提供者如何建立陰影複製

硬體或軟體陰影複製提供者會使用下列其中一種方法來建立陰影複製：

**完成複製**   此方法會在指定的時間點，建立原始磁片區的完整複本（稱為「完整複本」或「複製」）。 此複本為唯讀。

**寫入時複製**此方法不會複製原始磁片區   。 相反地，它會藉由複製在給定時間點之後對磁片區進行的所有變更（完成寫入 i/o 要求）來建立差異複本。

   「重新導向-寫入」此方法不會複製原始磁片區，而且不會在指定的時間點之後對原始磁片區進行任何變更。 相反地，它會將所有變更重新導向至不同的磁片區，藉此建立差異複本。

## <a name="complete-copy"></a>完成複製

完整的複製通常是藉由建立「分割鏡像」來建立，如下所示：

1.  原始磁片區和陰影複製磁片區是一組鏡像磁片區。  
      
2.  陰影複製磁片區會與原始磁片區隔開。 這會中斷鏡像連接。  
      

鏡像連接中斷之後，原始磁片區和陰影複製磁片區是獨立的。 原始磁片區會繼續接受所有變更（寫入 i/o 要求），而陰影複製磁片區在中斷時仍然是原始資料的確切唯讀複本。

### <a name="copy-on-write-method"></a>寫入時複製方法

在寫入時複製方法中，如果原始磁片區發生變更（但在寫入 i/o 要求完成之前），則會讀取要修改的每個區塊，然後將其寫入磁片區的陰影複製存放區域（也稱為其「差異區域」）。 陰影複製存放區域可以位於相同的磁片區或不同的磁片區上。 這會在變更覆寫之前，保留原始磁片區上的資料區塊複本。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>來源資料（狀態和資料）</th>
<th>陰影複製（狀態和資料）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始資料：1 2 3 4 5</p></td>
<td><p>無複本：—</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>快取中的資料變更：3到 3 '</p></td>
<td><p>已建立陰影複製（僅限差異）：3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>覆寫原始資料：1 2 3 ' 4 5</p></td>
<td><p>陰影複製上儲存的差異和索引：3</p></td>
</tr>
</tbody>
</table>

**表 1**    建立陰影複製的「寫入時複製」方法

「寫入時複製」方法是建立陰影複製的快速方法，因為它只會複製已變更的資料。 差異區域中複製的區塊可以與原始磁片區上的已變更資料結合，以便在進行任何變更之前，將磁片區還原到其狀態。 如果有許多變更，則寫入時複製方法可能會變得昂貴。

### <a name="redirect-on-write-method"></a>重新導向-寫入方法

在重新導向寫入方法中，每當原始磁片區收到變更（寫入 i/o 要求）時，此變更就不會套用至原始磁片區。 相反地，變更會寫入另一個磁片區的陰影複製存放區域。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>來源資料（狀態和資料）</th>
<th>陰影複製（狀態和資料）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始資料：1 2 3 4 5</p></td>
<td><p>無複本：—</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>快取中的資料變更：3到 3 '</p></td>
<td><p>已建立陰影複製（僅限差異）：第</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>原始資料未變更：1 2 3 4 5</p></td>
<td><p>陰影複製上儲存的差異和索引：第</p></td>
</tr>
</tbody>
</table>

**表 2**    建立陰影複製的重新導向寫入方法

如同複製時寫入方法，重新導向寫入方法是建立陰影複製的快速方法，因為它只會複製資料的變更。 差異區域中複製的區塊可以與原始磁片區上未變更的資料結合，以建立完整的最新資料複本。 如果有許多讀取 i/o 要求，則重新導向寫入方法可能會變得昂貴。

## <a name="shadow-copy-providers"></a>陰影複製提供者

陰影複製提供者有兩種類型：以硬體為基礎的提供者和以軟體為基礎的提供者。 另外還有一個系統提供者，也就是 Windows 作業系統內建的軟體提供者。

### <a name="hardware-based-providers"></a>以硬體為基礎的提供者

以硬體為基礎的陰影複製提供者可搭配硬體儲存介面卡或控制器使用，做為磁碟區陰影複製服務與硬體層級之間的介面。 建立和維護陰影複製的工作是由存放裝置陣列執行。

硬體提供者一律會採用整個 LUN 的陰影複製，但是磁碟區陰影複製服務只會公開所要求磁片區的陰影複製。

以硬體為基礎的陰影複製提供者會使用定義時間點的磁碟區陰影複製服務功能，允許資料同步處理、管理陰影複製，以及提供備份應用程式的通用介面。 不過，磁碟區陰影複製服務不會指定以硬體為基礎的提供者產生和維護陰影複製的基礎機制。

### <a name="software-based-providers"></a>以軟體為基礎的提供者

以軟體為基礎的陰影複製提供者通常會在檔案系統和磁片區管理員軟體之間，攔截和處理軟體層中的讀取和寫入 i/o 要求。

這些提供者會實作為使用者模式 DLL 元件，以及至少一個核心模式設備磁碟機，通常是儲存體篩選器驅動程式。 與硬體型提供者不同的是，以軟體為基礎的提供者會在軟體層級建立陰影複製，而不是在硬體層級。

以軟體為基礎的陰影複製提供者必須藉由存取可在陰影複製建立時間之前用來重新建立磁片區狀態的資料集，來維持「時間點」的查看量。 例如，系統提供者的「寫入時複製」技術。 不過，磁碟區陰影複製服務不會限制以軟體為基礎的提供者用來建立和維護陰影複製的技術。

軟體提供者適用于更廣泛的儲存平臺，而不是硬體型提供者，而且也應該同樣地使用基本磁碟或邏輯卷。 （邏輯磁片區是結合兩個或多個磁片的可用空間所建立的磁片區）。與硬體陰影複製相較之下，軟體提供者會耗用作業系統資源來維護陰影複製。

如需有關基本磁碟的詳細資訊，請參閱[什麼是基本磁碟和磁片區？](http://go.microsoft.com/fwlink/?linkid=180894) （ http://go.microsoft.com/fwlink/?LinkId=180894) TechNet 上的。

### <a name="system-provider"></a>系統提供者

Windows 作業系統中提供一個陰影複製提供者（系統提供者）。 雖然 Windows 中提供了預設的提供者，但其他廠商也可以免費提供針對其儲存體硬體和軟體應用程式優化的執行。

為了維護陰影複製中所包含之磁片區的「時間點」視圖，系統提供者會使用寫入時複製的技術。 在陰影複製建立開始後，磁片區上已修改的區塊複本會儲存在陰影複製存放區域中。

系統提供者可以公開生產磁片區，這可以正常寫入和讀取。 當需要陰影複製時，它會以邏輯方式將差異套用到生產磁片區上的資料，以公開完整的陰影複製。

針對系統提供者，陰影複製存放區域必須位於 NTFS 磁片區上。 要陰影複製的磁片區不需要是 NTFS 磁片區，但是在系統上至少掛接一個磁片區必須是 NTFS 磁片區。

組成系統提供者的元件檔是 swprv 和 volsnap。

### <a name="in-box-vss-writers"></a>內建 VSS 寫入器

Windows 作業系統包含一組 VSS 寫入器，負責列舉各種 Windows 功能所需的資料。

如需這些寫入器的詳細資訊，請參閱下列 Microsoft 網站：

  - [內建 VSS 寫入](http://go.microsoft.com/fwlink/?linkid=180895)器(http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [適用于 Windows Server 2008 和 Windows VISTA SP1 的全新內建 VSS 寫入](http://go.microsoft.com/fwlink/?linkid=180896)器(http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [適用于 Windows Server 2008 R2 和 windows 7 的全新內建 VSS 寫入](http://go.microsoft.com/fwlink/?linkid=180897)器(http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>如何使用陰影複製

除了備份應用程式資料和系統狀態資訊之外，陰影複製也可以用於許多用途，包括下列各項：

  - 還原 Lun （LUN 重新同步處理和 LUN 交換）  
      
  - 還原個別檔案（共用資料夾陰影複製）  
      
  - 使用可轉移的陰影複製來進行資料採礦  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>還原 Lun （LUN 重新同步處理和 LUN 交換）

在 Windows Server 2008 R2 和 Windows 7 中，VSS 要求者可以使用名為 LUN 重新同步處理（或「LUN 重新同步」）的硬體陰影複製提供者功能。 這是一種快速復原配置，可讓應用程式系統管理員將陰影複製的資料還原到原始 LUN 或新的 LUN。

陰影複製可以是完整複製或差異陰影複製。 不論是哪一種情況，在重新同步作業結束時，目的地 LUN 的內容都會與陰影複製 LUN 相同。 在重新同步作業期間，陣列會從陰影複製執行區塊層級的複製到目的地 LUN。


> [!NOTE]
> 陰影複製必須是可轉移的硬體陰影複製。 
<br>


在重新同步作業開始之後，大部分的陣列都允許生產 i/o 作業很快就會繼續。 重新同步作業進行時，會將讀取要求重新導向至陰影複製 LUN，並將要求寫入目的地 LUN。 這可讓陣列復原非常大型的資料集，並在數秒內恢復正常作業。

LUN 重新同步處理與 LUN 交換不同。 LUN 交換是一種快速復原案例，在 Windows Server 2003 SP1 之後支援 VSS。 在 LUN 交換中，陰影複製會匯入，然後轉換成讀寫磁片區。 轉換是無法復原的作業，而且磁片區和基礎 LUN 在該之後無法與 VSS Api 控制。 下列清單說明 LUN 重新同步處理與 LUN 交換的比較方式：

  - 在 LUN 重新同步處理中，陰影複製不會改變，因此可以使用數次。 在 LUN 交換中，陰影複製只能使用一次進行復原。 對於最重視安全性的系統管理員來說，這一點很重要。 使用 LUN 重新同步處理時，如果第一次發生錯誤，要求者可以重試整個還原作業。  
      
  - 在 LUN 交換結束時，陰影複製 LUN 會用於生產 i/o 要求。 基於這個理由，陰影複製 LUN 必須使用與原始生產 LUN 相同的儲存體品質，以確保效能在復原操作之後不受影響。 如果改用 LUN 重新同步處理，硬體提供者可以在儲存體上維護陰影複製，這比生產品質的儲存空間便宜。  
      
  - 如果目的地 LUN 無法使用且需要重新建立，LUN 交換可能會更實惠，因為它不需要目的地 LUN。  
      


> [!WARNING]
> 列出的所有作業都是 LUN 層級作業。 如果您嘗試使用 LUN 重新同步處理來復原特定的磁片區，您就不一定會還原所有共用 LUN 的其他磁片區。 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>還原個別檔案（共用資料夾陰影複製）

共用資料夾陰影複製使用磁碟區陰影複製服務來提供檔案的時間點複本，這些檔案位於共用網路資源上，例如檔案伺服器。 透過共用資料夾陰影複製，使用者可以快速復原已刪除或變更儲存在網路上的檔案。 因為他們可以在沒有系統管理員協助的情況下這麼做，所以共用資料夾陰影複製可以提高生產力並降低管理成本。

如需共用資料夾陰影複製的詳細資訊，[請參閱共用資料夾陰影複製](http://go.microsoft.com/fwlink/?linkid=180898)（ http://go.microsoft.com/fwlink/?LinkId=180898) TechNet 上的。

### <a name="data-mining-by-using-transportable-shadow-copies"></a>使用可轉移的陰影複製來進行資料採礦

透過專為搭配磁碟區陰影複製服務使用而設計的硬體提供者，您可以建立可傳送的陰影複製，以便匯入至相同子系統內的伺服器（例如 SAN）。 這些陰影複製可用來植入生產或測試安裝，並提供資料採礦的唯讀資料。

有了磁碟區陰影複製服務和存放裝置陣列，其中具有專為搭配磁碟區陰影複製服務使用而設計的硬體提供者，就可以在一部伺服器上建立來源資料磁片區的陰影複製，然後將陰影複製匯入到其他伺服器 （或回到相同的伺服器）。 此程式會在幾分鐘內完成，不論資料的大小為何。 傳輸程式是透過一系列的步驟來完成，使用支援可轉移陰影複製的陰影複製要求者（儲存管理應用程式）。

## <a name="to-transport-a-shadow-copy"></a>傳輸陰影複製

1.  在伺服器上建立來源資料的可傳送陰影複製。

2.  將陰影複製匯入到連接至 SAN 的伺服器（您可以匯入至不同的伺服器或相同的伺服器）。

3.  資料現在已可供使用。

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**[圖 3**    ] 在兩部伺服器之間建立和傳輸陰影複製


> [!NOTE]
> 在 Windows Server 2003 上建立的可轉移陰影複製，無法匯入執行 Windows Server 2008 或 Windows Server 2008 R2 的伺服器上。 在 Windows Server 2008 或 Windows Server 2008 R2 上建立的可轉移陰影複製，無法匯入執行 Windows Server 2003 的伺服器上。 不過，在 Windows Server 2008 上建立的陰影複製，可以匯入執行 Windows Server 2008 R2 的伺服器上，反之亦然。 
<br>


陰影複製是唯讀的。 如果您想要將陰影複製轉換成讀取/寫入 LUN，除了磁碟區陰影複製服務以外，您還可以使用虛擬磁碟服務型儲存管理應用程式（包括某些要求者）。 藉由使用此應用程式，您可以從磁碟區陰影複製服務管理移除陰影複製，並將其轉換成讀取/寫入 LUN。

在執行 Windows Server 2003 Enterprise Edition、Windows Server 2003 Datacenter Edition、Windows Server 2008 或 Windows Server 2008 R2 的電腦上，磁碟區陰影複製服務傳輸是一個先進的解決方案。 只有在存放裝置陣列上有硬體提供者時，它才會運作。 陰影複製傳輸可以用於許多用途，包括磁帶備份、資料採礦和測試。

## <a name="frequently-asked-questions"></a>常見問題集

此常見問題會回答有關系統管理員磁碟區陰影複製服務（VSS）的問題。 如需 VSS 應用程式開發介面的詳細資訊，請參閱[磁碟區陰影複製服務](http://go.microsoft.com/fwlink/?linkid=180899)（ http://go.microsoft.com/fwlink/?LinkId=180899) 在 Windows 開發人員中心程式庫中）。

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>何時磁碟區陰影複製服務引進？ 有哪些可用的 Windows 作業系統版本？

VSS 是在 Windows XP 中引進。 其適用于 Windows XP、Windows Server 2003、Windows Vista®、Windows Server 2008、Windows 7 和 Windows Server 2008 R2。

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>陰影複製和備份有何不同？

在硬碟備份的情況下，建立的陰影複製也是備份。 您可以從陰影複製複製資料進行還原，或使用陰影複製來快速復原案例，例如，LUN 重新同步處理或 LUN 交換。

將資料從陰影複製複製到磁帶或其他卸載式媒體時，儲存在媒體上的內容會構成備份。 複製資料之後，可以刪除陰影複製本身。

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>磁碟區陰影複製服務支援的最大磁片區大小為何？

磁碟區陰影複製服務支援高達 64 TB 的磁片區大小。

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>我在 Windows Server 2008 上進行了備份。 我可以在 Windows Server 2008 R2 上還原嗎？

這取決於您所使用的備份軟體。 大部分的備份程式都支援這種資料案例，但不適用於系統狀態備份。

在這兩個版本的 Windows 上建立的陰影複製，都可以用在另一個上。

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>我在 Windows Server 2003 上進行了備份。 我可以在 Windows Server 2008 上還原嗎？

這取決於您所使用的備份軟體。 如果您在 Windows Server 2003 上建立陰影複製，就無法在 Windows Server 2008 上使用它。 此外，如果您在 Windows Server 2008 上建立陰影複製，就無法在 Windows Server 2003 上進行還原。

### <a name="how-can-i-disable-vss"></a>如何停用 VSS？

您可以使用 Microsoft Management Console 來停用磁碟區陰影複製服務。 不過，您不應該這麼做。 停用 VSS 會對您使用的任何軟體（例如系統還原和 Windows Server Backup）造成不良的影響。

如需詳細資訊，請參閱下列 Microsoft TechNet 網站：

  - [系統還原](http://go.microsoft.com/fwlink/?linkid=157113)(http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891)(http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>我可以從陰影複製中排除檔案來節省空間嗎？

VSS 的設計目的是要建立整個磁片區的陰影複製。 系統會自動從陰影複製省略暫存檔案，例如分頁檔案，以節省空間。

若要從陰影複製中排除特定檔案，請使用下列登錄機碼：**FilesNotToSnapshot**。


> [!NOTE]
> <STRONG>FilesNotToSnapshot</STRONG>登錄機碼僅供應用程式使用。 嘗試使用它的使用者將會遇到下列限制：
> <br>
> <UL>
> <LI>它無法從使用 [舊版] 功能在 Windows Server 上建立的陰影複製中刪除檔案。<BR><BR>
> <LI>它無法從共用資料夾的陰影複製中刪除檔案。<BR><BR>
> <LI>它可以從使用<a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a>公用程式所建立的陰影複製中刪除檔案，但無法從使用<a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a>公用程式所建立的陰影複製中刪除檔案。<BR><BR>
> <LI>檔案會以最大的方式從陰影複製中刪除。 這表示不保證會刪除它們。<BR><BR></LI></UL>


如需詳細資訊，請參閱 MSDN 上的[從陰影複製排除](http://go.microsoft.com/fwlink/?linkid=180904)檔案（ http://go.microsoft.com/fwlink/?LinkId=180904) 英文）。

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>我的非 Microsoft 備份程式失敗，發生 VSS 錯誤。 我可以做什麼？

檢查建立備份程式之公司網站的 [產品支援] 區段。 可能會有您可以下載並安裝的產品更新來修正問題。 如果沒有，請聯絡公司的產品支援部門。

系統管理員可以使用下列 Microsoft TechNet Library 網站上的 VSS 疑難排解資訊，收集有關 VSS 相關問題的診斷資訊。

如需詳細資訊，請參閱[磁碟區陰影複製服務](http://go.microsoft.com/fwlink/?linkid=180905)（ http://go.microsoft.com/fwlink/?LinkId=180905) TechNet 上的。

### <a name="what-is-the-diff-area"></a>什麼是「差異區域」？

「陰影複製」存放區域（或「差異區域」）是儲存系統軟體提供者所建立之陰影複製資料的位置。

### <a name="where-is-the-diff-area-located"></a>差異區域位於何處？

Diff 區域可以位於任何本機磁片區上。 不過，它必須位於擁有足夠空間可以儲存的 NTFS 磁片區上。

### <a name="how-is-the-diff-area-location-determined"></a>如何判斷差異區域位置？

系統會依此順序評估下列準則，以判斷 diff 區域的位置：

  - 如果磁片區已有現有的陰影複製，則會使用該位置。  
      
  - 如果原始磁片區與陰影複製磁片區位置之間有預先設定的手動關聯，則會使用該位置。  
      
  - 如果前兩個準則並未提供位置，陰影複製服務就會根據可用空間來選擇位置。 如果有多個磁片區正在進行陰影複製，陰影複製服務會根據可用空間的大小（以遞減順序），建立可能的快照集位置清單。 提供的位置數目等於陰影複製的磁片區數目。  
      
  - 如果要陰影複製的磁片區是其中一個可能的位置，則會建立本機關聯。 否則會建立與具有最多可用空間的磁片區之間的關聯。  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS 是否可以建立非 NTFS 磁片區的陰影複製？

是的。 不過，持續性陰影複製只能針對 NTFS 磁片區進行。 此外，在系統上至少掛接一個磁片區必須是 NTFS 磁片區。

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>一次可以建立的最大陰影複製數目？

單一陰影複製組中陰影複製的磁片區數目上限為64。 請注意，這與陰影複製的數目不同。

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>可以為磁片區維護的系統提供者所建立的軟體陰影複製數目上限為何？

每個磁片區的軟體陰影複製數目上限為512。 不過，根據預設，您只能維護「共用資料夾的陰影複製」功能所使用的64陰影複製。 若要變更 [共用資料夾的陰影複製] 功能的限制，請使用下列登錄機碼：**MaxShadowCopies**。

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>如何控制用於陰影複製儲存空間的空間？

輸入**vssadmin resize shadowstorage**命令。

如需詳細資訊，請參閱[Vssadmin resize shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) （ http://go.microsoft.com/fwlink/?LinkId=180906) TechNet 上的。

### <a name="what-happens-when-i-run-out-of-space"></a>當我用盡空間時，會發生什麼事？

系統會從最舊的陰影複製開始刪除磁片區的陰影複製。

## <a name="volume-shadow-copy-service-tools"></a>磁碟區陰影複製服務工具

Windows 作業系統提供下列工具來使用 VSS：

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907)(http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008)(http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow 是 VSS 要求者，可讓您用來管理系統上可以擁有的所有硬體和軟體快照集。 DiskShadow 包含如下所示的命令：

  - **list**：列出 VSS 寫入器、VSS 提供者和陰影複製  
      
  - **建立**：建立新的陰影複製  
      
  - 匯**入**：匯入可轉移的陰影複製  
      
  - **公開**：公開持續陰影複製（例如磁碟機號）  
      
  - **還原**：將磁片區還原回指定的陰影複製  
      

這項工具是供 IT 專業人員使用，但是開發人員在測試 VSS 寫入器或 VSS 提供者時，可能也會發現它很有用。

DiskShadow 僅適用于 Windows Server 作業系統。 無法在 Windows 用戶端作業系統上使用。

### <a name="vssadmin"></a>VssAdmin

VssAdmin 是用來建立、刪除和列出陰影複製的相關資訊。 它也可以用來調整陰影複製儲存區域的大小（「差異區域」）。

VssAdmin 包含如下所示的命令：

  - **建立陰影**：建立新的陰影複製  
      
  - **刪除陰影**：刪除陰影複製  
      
  - **清單提供者**：列出所有已註冊的 VSS 提供者  
      
  - **清單寫入**器：列出所有已訂閱的 VSS 寫入器  
      
  - **調整 shadowstorage 的大小**：變更陰影複製存放區域的大小上限  
      

VssAdmin 只能用來管理系統軟體提供者所建立的陰影複製。

VssAdmin 適用于 Windows 用戶端和 Windows Server 作業系統版本。

## <a name="volume-shadow-copy-service-registry-keys"></a>磁碟區陰影複製服務登錄機碼

下列登錄機碼可與 VSS 搭配使用：

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

這個金鑰是用來指定哪些使用者有陰影複製的存取權。

如需詳細資訊，請參閱 MSDN 網站上的下列專案：

  - [寫入器的安全性考慮](http://go.microsoft.com/fwlink/?linkid=157739)(http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [要求者的安全性考慮](http://go.microsoft.com/fwlink/?linkid=180908)(http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

此機碼會指定可在電腦的每個磁片區上儲存的用戶端可存取的陰影複製數目上限。 共用資料夾陰影複製使用用戶端可存取的陰影複製。

如需詳細資訊，請參閱 MSDN 網站上的下列專案：

**MaxShadowCopies**在登錄機[碼下進行備份和還原](http://go.microsoft.com/fwlink/?linkid=180909)（ http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

此機碼會指定陰影複製存放區域的最小初始大小（以 MB 為單位）。

如需詳細資訊，請參閱 MSDN 網站上的下列專案：

**MinDiffAreaFileSize**在登錄機[碼下進行備份和還原](http://go.microsoft.com/fwlink/?linkid=180910)（ http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# ' 支援的作業系統版本

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
<td><p>LUN 重新同步處理</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong>登錄機碼</p></td>
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
<td><p>舊版的 Windows Server</p></td>
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
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />注意</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>這是匯入陰影複製一次以上的功能。 一次只能執行一個匯入作業。
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
<td><p>並行備份會話（最多64）</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>單一還原會話與備份並行</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 SP2</p></td>
</tr>
<tr class="even">
<td><p>最多8個還原會話同時備份</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另請參閱

[Windows 開發人員中心中的磁碟區陰影複製服務](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)