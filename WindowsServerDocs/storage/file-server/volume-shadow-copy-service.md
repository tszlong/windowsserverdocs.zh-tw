---
Title: 磁碟區陰影複製服務
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 0a4af25723c6d1e796cd3255875c15faf21fb8be
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284388"
---
# <a name="volume-shadow-copy-service"></a>磁碟區陰影複製服務

適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2、 Windows Server 2008、 Windows 10，Windows 8.1，Windows 8、 Windows 7

備份及還原重要商務資料可以是非常複雜，因為下列問題：

  - 資料通常需要產生資料的應用程式仍在執行時進行備份。 這表示某些資料檔案可能已開啟，或它們可能處於不一致的狀態。  
      
  - 如果資料集很大，它可能難以的所有備份它一次。  
      

關閉協調備份應用程式，要備份的特定業務應用程式和存放裝置管理硬體與軟體才能正確執行 備份和還原作業。 磁碟區陰影複製服務 (VSS)，Windows Server® 2003年中引進，可協助讓它們更融洽地合作這些元件之間的對話。 當所有元件都支援 VSS 時，您可以使用它們來備份您的應用程式資料，而不用讓應用程式離線。

VSS 會協調建立一致的陰影複製 （也稱為快照集或時間點副本） 是要備份的資料所需的動作。 陰影複製可用來當做-，或用於案例，如下所示：

  - 您想要備份應用程式資料和系統狀態資訊，包括封存資料到另一個硬碟機、 磁帶或其他卸除式媒體。  
      
  - 您是資料採礦。  
      
  - 您正在執行磁碟到磁碟備份。  
      
  - 您藉由將資料還原到原始的 LUN 或取代失敗的原始 LUN 史無前例的全新 LUN 必須從資料遺失的快速復原。  
      

Windows 功能和使用 VSS 的應用程式包括下列項目：

  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [共用資料夾的陰影複製](http://go.microsoft.com/fwlink/?linkid=142874)(http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [系統還原](http://go.microsoft.com/fwlink/?linkid=180893)(http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>磁碟區陰影複製服務的運作方式

完整的 VSS 解決方案需要的所有下列的基本部分：

**VSS 服務**   屬於 Windows 作業系統，可確保其他元件可以正確地彼此通訊，並一起運作。

**VSS requester，因此**   要求實際建立陰影複製 （或其他高層級的作業，例如匯入或刪除） 的軟體。 一般而言，這是備份的應用程式。 Windows Server Backup 公用程式和 System Center Data Protection Manager 應用程式是 VSS 要求者。 非 Microsoft® VSS 要求者包含幾乎所有的備份軟體在 Windows 上執行。

**VSS 寫入器**   保證我們已經一致的資料集，以將備份的元件。 這通常被提供做為特定業務應用程式，例如 sql 或 Exchange Server 的一部分。 VSS 寫入器的各種 Windows 元件，例如登錄中，是隨附於 Windows 作業系統。 非 Microsoft VSS 寫入器會包含許多需要保證資料一致性備份期間，註冊的 Windows 應用程式。

**VSS 提供者**   建立及維護的陰影複製的元件。 這可能會發生硬體或軟體中。 Windows 作業系統包含使用寫入時複製的 VSS 提供者。 如果您使用存放區域網路 (SAN)，請務必安裝 VSS 硬體提供者，為 san 中，如果提供的其中一個。 硬體提供者會卸載建立與維護的陰影複製，從主機作業系統的工作。

下圖說明如何 VSS 服務要求者、 與寫入器，提供者建立陰影複製磁碟區的座標。

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**圖 1**   的磁碟區陰影複製服務架構圖

### <a name="how-a-shadow-copy-is-created"></a>如何建立陰影複製

這一節將列出會帶您前往建立陰影複製所需的步驟放入內容的要求者、 作家和提供者的各種角色。 下圖顯示磁碟區陰影複製服務如何控制整體協調的要求者、 作家和提供者。

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**圖 2**陰影複製建立程序

若要建立的陰影複製，要求者、 作家和提供者執行下列動作：

1.  要求者會要求列舉的寫入器、 收集的寫入器中繼資料，並準備好建立陰影複製磁碟區陰影複製服務。  
      
2.  每個寫入器會建立需要備份的元件和資料存放區的 XML 描述，並提供它給磁碟區陰影複製服務。 寫入器也會定義一個還原方法，可用於所有的元件。 磁碟區陰影複製服務會提供給要求者，其會選取的元件，將備份寫入器的描述。  
      
3.  磁碟區陰影複製服務通知來準備資料進行陰影複製的所有寫入器。  
      
4.  每個寫入器準備適當的資料，例如完成所有開啟的交易，復原交易記錄檔，並清除快取。 備妥要陰影複製資料時，寫入器會通知磁碟區陰影複製服務。  
      
5.  磁碟區陰影複製服務會告訴暫時凍結應用程式寫入 I/O 要求 （I/O 要求仍有可能發生的讀取） 的寫入器建立的磁碟區或磁碟區的陰影複製所需的幾秒鐘。 若要花 60 秒以上不允許應用程式凍結。 磁碟區陰影複製服務排清檔案系統緩衝區，則會凍結檔案系統中，可確保檔案系統中繼資料會記錄正確，且要陰影複製的資料以一致的順序。  
      
6.  磁碟區陰影複製服務會指示提供者建立陰影複製。 陰影複製建立期間會持續不超過 10 秒，此時所有寫入至檔案系統的 I/O 要求都會凍結。  
      
7.  磁碟區陰影複製服務版本檔案系統寫入 I/O 要求。  
      
8.  VSS 會指示寫入器，來解除凍結的應用程式寫入 I/O 要求。 此時應用程式可以隨意繼續將資料寫入至磁碟正在陰影複製。  
      

> [!NOTE]
> 如果寫入器會保留在超過 60 秒，或如果需要超過 10 秒認可的陰影複製提供者的凍結狀態，可能會中止建立陰影複製。 
<br>

9. 要求者可以重試程序 （請返回步驟 1），或通知系統管理員稍後再試一次。  
      
10. 如果成功建立陰影複製，則磁碟區陰影複製服務會傳回給要求者的陰影複製的位置資訊。 在某些情況下，陰影複製暫時可為讀寫磁碟區讓該 VSS 和一或多個應用程式可以修改的內容的陰影複製之前完成的陰影複製。 VSS 和應用程式進行其轉變工作之後，陰影複製會變成唯讀狀態。 這個階段會呼叫自動復原，它用來復原檔案系統或應用程式的任何交易上陰影複製磁碟區陰影複製建立之前未完成。  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>提供者如何建立陰影複製

硬體或軟體的陰影複製提供者會使用其中一種下列方法建立的陰影複製：

**完成複製**   此方法會在原始的磁碟區的完整複本 （稱為 「 完整複製 」 或 「 複製 」） 在指定的時間點進行的時間。 此複本是唯讀的。

**寫入時複製**   這個方法不會複製原始磁碟區。 相反地，它會藉由複製之後所做的磁碟區的指定點在時間中的所有變更 （已完成的寫入 I/O 要求） 建立差異複本。

**寫入重新導向**   這個方法不會複製原始磁碟區，以及它不會進行任何變更原始磁碟區指定的時間點之後的時間。 相反地，它會建立差異複本重新導向至不同的磁碟區的所有變更。

## <a name="complete-copy"></a>完整複本

完整複本通常會建立藉由 「 分割鏡像 」，如下所示：

1.  原始的磁碟區和陰影複製磁碟區是鏡像磁碟區組。  
      
2.  陰影複製磁碟區被分開的原始磁碟區。 這會中斷鏡像連接。  
      

鏡像連接會中斷之後，原始的磁碟區和陰影複製磁碟區是獨立。 原始的磁碟區會繼續接受所有的變更 （寫入 I/O 要求） 時，陰影複製磁碟區仍在中斷時完全相同的唯讀複本的原始資料。

### <a name="copy-on-write-method"></a>寫入時複製方法

在寫入時複製方法中，當原始的磁碟區的變更，就會發生 （但在寫入之前完成 I/O 要求），修改每個區塊是讀取，並接著會寫入至磁碟區的陰影複製存放區域 （也稱為其 「 差異區域 」）。 陰影複製存放區域可以位於相同的磁碟區或不同的磁碟區上。 這會保留一份原始磁碟區上的資料區塊之前變更會覆寫它。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>來源資料 （「 狀態 」 和 「 資料 」）</th>
<th>陰影複製 （狀態和資料）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始的資料：1 2 3 4 5</p></td>
<td><p>任何複本:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>變更快取中的資料：3 至 3 '</p></td>
<td><p>陰影複製建立 （只有差異）：3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>覆寫原始的資料：1 2 3’ 4 5</p></td>
<td><p>差異與儲存的陰影複製上的索引：3</p></td>
</tr>
</tbody>
</table>

**表 1**   建立陰影複製的寫入時複製方法

寫入時複製方法是建立陰影複製的快速方法，因為它會複製變更的資料。 由於相異區域中複製的區塊可以結合在原始的磁碟區，以進行任何變更之前，將磁碟區還原其狀態變更的資料。 如果有許多變更，寫入時複製方法可能會變得昂貴。

### <a name="redirect-on-write-method"></a>寫入重新導向方法

在寫入時重新導向方法，每當原始磁碟區會接收到的變更 （寫入 I/O 要求），變更不會套用到原始的磁碟區。 相反地，變更會寫入至另一個磁碟區陰影複製存放區域。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>來源資料 （「 狀態 」 和 「 資料 」）</th>
<th>陰影複製 （狀態和資料）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>原始的資料：1 2 3 4 5</p></td>
<td><p>任何複本:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>變更快取中的資料：3 至 3 '</p></td>
<td><p>陰影複製建立 （只有差異）：3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>原始資料保持不變：1 2 3 4 5</p></td>
<td><p>差異與儲存的陰影複製上的索引：3’</p></td>
</tr>
</tbody>
</table>

**表 2**   建立陰影複製的寫入重新導向方法

寫入時複製方法，例如寫入重新導向方法相當快速的方法，來建立陰影複製，因為它會將所做的變更複製到資料。 由於相異區域中複製的區塊可以結合在原始的磁碟區，以建立完整的最新複本的資料未變更的資料。 如果有許多讀取的 I/O 要求，寫入時重新導向方法可能會變得昂貴。

## <a name="shadow-copy-providers"></a>陰影複製提供者

有兩種類型的陰影複製提供者： 硬體為基礎的提供者和以軟體為基礎的提供者。 此外，還有系統提供者，也就是內建在 Windows 作業系統的軟體提供者。

### <a name="hardware-based-providers"></a>硬體為基礎的提供者

硬體架構的陰影複製提供者做為磁碟區陰影複製服務與硬體層級所搭配使用硬體儲存體介面卡或控制站之間的介面。 建立與維護的陰影複製的工作會執行存放裝置陣列。

硬體提供者一律會採用的陰影複製的整個 LUN，但磁碟區陰影複製服務只會公開磁碟區或所要求的磁碟區的陰影複製。

硬體架構的陰影複製提供者會利用磁碟區陰影複製服務的時間，定義的點的功能可讓資料同步處理、 管理陰影複製，和備份應用程式提供通用介面。 不過，磁碟區陰影複製服務未指定的硬體為基礎的提供者會產生和維護陰影複製的基礎機制。

### <a name="software-based-providers"></a>以軟體為基礎的提供者

以軟體為基礎的陰影複製提供者通常攔截和處理程序讀取及寫入 I/O 要求中檔案系統與磁碟區管理員軟體之間的軟體層。

這些提供者會實作為使用者模式 DLL 元件，並且至少一個核心模式裝置驅動程式，通常是儲存體篩選器驅動程式。 不同於硬體為基礎的提供者，以軟體為基礎的提供者會建立陰影複製，在軟體層級，而不是硬體層級。

以軟體為基礎的陰影複製提供者必須維護磁碟區的 「 點中時間 」 檢視，可用來重新建立磁碟區的狀態，再陰影複製建立時的資料集的存取。 舉例來說，系統提供者寫入時複製項技術。 然而，磁碟區陰影複製服務會造成何種技術，以軟體為基礎的提供者建立和維護陰影複製沒有任何限制。

軟體提供者適用於廣泛的儲存體平台比硬體為基礎的提供者，而且它應該使用基本磁碟或邏輯磁碟區也一樣。 （邏輯磁碟區是由結合兩個或多個磁碟上的可用空間的磁碟區）。相較於硬體陰影複製，軟體提供者會使用作業系統資源，來維護陰影複製。

如需有關基本磁碟的詳細資訊，請參閱[基本磁碟和磁碟區是什麼？](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) TechNet 上。

### <a name="system-provider"></a>系統提供者

Windows 作業系統會提供一個陰影複製提供者，系統提供者。 雖然在 Windows 中提供的預設提供者，其他廠商沒有提供適合其儲存體的硬體和軟體應用程式的實作。

若要維護磁碟區陰影複製中所包含的 「 點中時間 」 檢視，系統提供者會使用寫入時複製技術。 磁碟區上的區塊建立陰影複製開始之後修改過的複本會儲存在陰影複製存放區域。

系統提供者可以公開生產磁碟區，可用來寫入和讀取正常。 當需要陰影複製時，它以邏輯方式的差異資料上套用生產磁碟區，以公開完整的陰影複製。

系統提供者，必須是 NTFS 磁碟區的陰影複製存放區域。 要進行陰影複製的磁碟區不需要是 NTFS 磁碟區，但至少一個系統上掛接的磁碟區必須是 NTFS 磁碟區。

系統提供者所組成的元件檔案還有 swprv.dll volsnap.sys。

### <a name="in-box-vss-writers"></a>內建的 VSS 寫入器

Windows 作業系統包含一組負責列舉資料所需的各種 Windows 功能的 VSS 寫入器。

如需有關這些寫入器的詳細資訊，請參閱下列 Microsoft 網站：

  - [內建的 VSS 寫入器](http://go.microsoft.com/fwlink/?linkid=180895)(http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [適用於 Windows Server 2008 和 Windows Vista SP1 的新內建的 VSS 寫入器](http://go.microsoft.com/fwlink/?linkid=180896)(http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Windows Server 2008 R2 和 Windows 7 的新內建的 VSS 寫入器](http://go.microsoft.com/fwlink/?linkid=180897)(http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>如何使用陰影複製

除了備份應用程式資料和系統狀態的資訊，陰影複製可用於數個用途，包括下列：

  - 還原 Lun （LUN 重新同步處理和 LUN 交換）  
      
  - 還原個別檔案 （共用資料夾的陰影複製）  
      
  - 使用傳送陰影複製的資料採礦  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>還原 Lun （LUN 重新同步處理和 LUN 交換）

在 Windows Server 2008 R2 和 Windows 7，VSS 要求者可以使用名為 LUN 重新同步處理 （或 「 LUN 重新同步處理 」） 的硬體陰影複製提供者功能。 這是一種快速復原配置，可讓應用程式系統管理員，將資料從陰影複製還原到原始的 LUN 或新的 LUN。

完整再製或差異的陰影複製，可以使用陰影複製。 在任一情況下，結尾的重新同步處理作業中，目的地 LUN 必須與陰影複製 LUN 相同的內容。 重新同步處理作業期間，陣列會執行區塊層級的複製到目的地 LUN 從陰影複製。


> [!NOTE]
> 陰影複製，必須容易進行傳輸的硬體陰影複製。 
<br>


大部分的陣列會允許繼續重新同步作業開始後不久的實際執行 I/O 作業。 在進行重新同步作業時，讀取要求重新導向至陰影複製 LUN，並將要求寫入目的地 LUN。 這可讓復原的大型資料集，並在數秒後繼續正常作業的陣列。

LUN 的重新同步處理點不同於 LUN 交換。 LUN 交換是自 Windows Server 2003 SP1 便開始支援 VSS 的快速復原案例。 LUN 交換中的陰影複製是匯入，並接著轉換成讀寫磁碟區。 轉換是無法復原的作業，和磁碟區和基礎 LUN 無法控制使用 VSS Api 之後。 下列清單描述 LUN 重新同步處理如何比較與交換 LUN:

  - 在 LUN 重新同步處理，陰影複製不是改變，因此它可以使用多次。 在 LUN 交換，陰影複製可用於一次復原。 對於最注重安全性的系統管理員，這很重要。 使用 LUN 重新同步處理時，要求者可以重試整個還原作業發生錯誤時第一次。  
      
  - 在 LUN 交換結束時，陰影複製 LUN 用於實際執行 I/O 要求。 基於這個理由，陰影複製 LUN 必須使用相同的儲存體的品質為原始的實際執行 LUN 來確保復原作業之後不會影響效能。 如果使用 LUN 重新同步處理時，請改為硬體提供者可以維護比生產環境品質的儲存體成本較低的儲存體上的陰影複製。  
      
  - 如果目的地 LUN 無法使用，而且需要重新建立，LUN 交換可能會更具經濟效益因為它不需要目的地 LUN。  
      


> [!WARNING]
> 所有列出的作業都是 LUN 層級的作業。 如果您嘗試使用 LUN 重新同步處理來復原特定的磁碟區，您不知情的狀況下要還原的所有其他磁碟區是共用的 LUN。 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>還原個別檔案 （共用資料夾的陰影複製）

共用資料夾的陰影複製會使用磁碟區陰影複製服務，來共用的網路資源，例如檔案伺服器提供檔案的時間點副本。 共用資料夾的陰影複製，使用者可以快速復原已刪除或變更的檔案儲存在網路上。 也可以不需要系統管理員協助來這麼做，因為共用資料夾的陰影複製可以提高生產力並降低管理成本。

共用資料夾的陰影複本的相關資訊，請參閱[共用資料夾的陰影複製](http://go.microsoft.com/fwlink/?linkid=180898)(http://go.microsoft.com/fwlink/?LinkId=180898) TechNet 上。

### <a name="data-mining-by-using-transportable-shadow-copies"></a>使用傳送陰影複製的資料採礦

是設計用於磁碟區陰影複製服務硬體提供者，您可以建立可匯入到相同的子系統 (例如 SAN) 內的伺服器上的傳送陰影複製。 這些陰影複製可用來植入生產或測試資料採礦的唯讀資料的安裝。

磁碟區陰影複製服務和硬體提供者，專為與磁碟區陰影複製服務搭配使用的儲存體陣列，就可以在一部伺服器，建立來源資料磁碟區的陰影複製，然後匯入到另一部伺服器上的陰影複製 （或移回相同的伺服器）。 幾分鐘的時間，無論資料的大小來完成此程序。 傳輸程序會透過一系列的使用支援傳送陰影複製，陰影複製要求者 （存放裝置管理應用程式） 的步驟來完成。

## <a name="to-transport-a-shadow-copy"></a>若要傳輸的陰影複製

1.  在伺服器上建立來源資料的傳送陰影複製。

2.  匯入伺服器，連接到 SAN 的陰影複製 （您可以匯入另一部伺服器或相同的伺服器）。

3.  現在已備妥可用的資料。

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**圖 3**   建立陰影複製和兩部伺服器之間的傳輸


> [!NOTE]
> 建立 Windows Server 2003 傳送陰影複製無法匯入到執行 Windows Server 2008 的伺服器或 Windows Server 2008 R2。 Windows Server 2008 或 Windows Server 2008 R2 建立可傳送的陰影複製無法匯入到執行 Windows Server 2003 的伺服器。 不過，在 Windows Server 2008 建立的陰影複製可以匯入到的伺服器上正在執行 Windows Server 2008 R2，反之亦然。 
<br>


陰影複製是唯讀的。 如果您想要將讀取/寫入 LUN 的陰影複製，您可以使用的虛擬磁碟服務為基礎的儲存體管理應用程式 （包括某些要求者） 除了磁碟區陰影複製服務。 藉由使用此應用程式，您可以從磁碟區陰影複製服務管理中移除的陰影複製，並將它轉換成讀取/寫入的 LUN。

磁碟區陰影複製服務傳輸是一個進階的解決方案，在執行 Windows Server 2003 Enterprise Edition、 Windows Server 2003 Datacenter Edition、 Windows Server 2008 或 Windows Server 2008 R2 電腦上。 它只有在沒有硬體提供者，存放裝置陣列上運作。 陰影複製傳輸可以用於許多用途，包括磁帶備份，資料採礦，並測試。

## <a name="frequently-asked-questions"></a>常見問題集

此常見問題集會回答問題的相關磁碟區陰影複製服務 (VSS) 的系統管理員。 VSS 應用程式開發介面的相關資訊，請參閱[磁碟區陰影複製服務](http://go.microsoft.com/fwlink/?linkid=180899)(http://go.microsoft.com/fwlink/?LinkId=180899) Windows 開發人員中心文件庫中。

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>何時磁碟區陰影複製服務引進了？ 在 Windows 作業系統版本上是否可用？

VSS 是在 Windows XP 引進。 在 Windows XP、 Windows Server 2003、 Windows Vista®、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2 上使用。

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>陰影複製和備份之間的差異為何？

在硬碟備份的情況下建立的陰影複製也是備份。 可以將資料複製關閉陰影複製還原或陰影複製可用來快速復原案例 — 比方說，LUN 重新同步處理或 LUN 交換。

資料複製時從陰影複製到磁帶或其他卸除式媒體，則儲存在媒體的內容會構成備份。 從中複製資料之後，就可以刪除陰影複製本身。

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>什麼是磁碟區陰影複製服務支援的最大大小磁碟區？

磁碟區陰影複製服務支援最高 64 TB 的磁碟區的大小。

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>我在 Windows Server 2008 上進行備份。 可以還原它在 Windows Server 2008 R2？

這取決於您所使用的備份軟體。 大部分的備份程式在系統狀態備份資料但不是支援這種情況。

在其他可用在任一版本的 Windows 建立的陰影複製。

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>我在 Windows Server 2003 上進行備份。 可以還原它在 Windows Server 2008？

這取決於您所使用的備份軟體。 如果您在 Windows Server 2003 上建立陰影複製，您無法在 Windows Server 2008 上使用它。 此外，如果您在 Windows Server 2008 上建立陰影複製，您無法在 Windows Server 2003 上它還原。

### <a name="how-can-i-disable-vss"></a>如何停用 VSS？

可以使用 Microsoft Management Console 來停用磁碟區陰影複製服務。 不過，您不應該這麼。 停用 VSS 造成不良影響而定，例如系統還原和 Windows Server Backup 任何軟體，用。

如需詳細資訊，請參閱下列 Microsoft TechNet 網站上：

  - [系統還原](http://go.microsoft.com/fwlink/?linkid=157113)(http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>我可以從陰影複製儲存空間中排除檔案嗎？

VSS 被設計來建立整個磁碟區的陰影複製。 暫存檔案，例如分頁檔，自動將略過陰影複製，以節省空間。

若要從陰影複製中排除特定檔案，請使用下列登錄機碼：**FilesNotToSnapshot**。


> [!NOTE]
> <STRONG>FilesNotToSnapshot</STRONG>登錄機碼專門用於只能由應用程式。 嘗試使用它的使用者將會遇到限制，如下所示：
> <br>
> <UL>
> <LI>它無法刪除使用 [舊版] 功能建立在 Windows Server 的陰影複製的檔案。<BR><BR>
> <LI>從共用資料夾的陰影複製，而無法刪除檔案。<BR><BR>
> <LI>它可以刪除檔案，從利用所建立的陰影複製<a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a>公用程式，但是它不能刪除的檔案使用所建立的陰影複製<a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a>公用程式。<BR><BR>
> <LI>從陰影複製以最佳方式為基礎，會刪除檔案。 這表示它們不一定要刪除。<BR><BR></LI></UL>


如需詳細資訊，請參閱 <<c0> [ 排除的檔案，從陰影複製](http://go.microsoft.com/fwlink/?linkid=180904)(http://go.microsoft.com/fwlink/?LinkId=180904) MSDN 上。

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>我的非 Microsoft 備份程式失敗且產生 VSS 錯誤。 我可以做什麼？

檢查建立備份程式的公司網站的產品支援區段。 可能有的產品更新，您可以下載並安裝來修正此問題。 如果沒有，請連絡該公司的產品支援部門。

系統管理員可以使用下列 Microsoft TechNet Library 網站上的 VSS 的疑難排解資訊，來收集診斷資訊與 VSS 相關的問題。

如需詳細資訊，請參閱 <<c0> [ 磁碟區陰影複製服務](http://go.microsoft.com/fwlink/?linkid=180905)(http://go.microsoft.com/fwlink/?LinkId=180905) TechNet 上。

### <a name="what-is-the-diff-area"></a>什麼是 「 差異區域 」？

陰影複製存放區域 （或 「 差異區域 」） 是儲存的資料由系統軟體提供者的陰影複製的位置。

### <a name="where-is-the-diff-area-located"></a>由於相異區域位於何處？

由於相異區域可以位於任何本機的磁碟區。 不過，它必須位於 NTFS 磁碟區具有足夠空間來儲存它。

### <a name="how-is-the-diff-area-location-determined"></a>有何差異區域位置決定？

下列準則的評估，依此順序，來判斷差異區域位置：

  - 如果磁碟區已經有現有的陰影複製，則會使用該位置。  
      
  - 如果沒有預先設定的手動關聯，會與原始的磁碟區陰影複製磁碟區位置，則會使用該位置。  
      
  - 如果先前的兩個準則不會提供一個位置，陰影複製服務會選擇根據可用空間的位置。 如果多個磁碟區陰影複製，陰影複製服務會建立一份可能的快照集位置取決於可用的空間，以遞減順序的大小。 提供的位置數目等於啟用了陰影複製的磁碟區數目。  
      
  - 如果啟用了陰影複製磁碟區是其中一個可能的位置，則會建立本機關聯。 否則，系統會建立與具有最多可用空間的磁碟區的關聯。  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS 可以建立非 NTFS 磁碟區陰影複製？

是的。 不過，可以只針對 NTFS 磁碟區進行持續的陰影複製。 此外，至少一個系統上掛接的磁碟區必須是 NTFS 磁碟區。

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>我可以一次建立的陰影複製的數目上限為何？

在單一的陰影複製組中的陰影複製磁碟區的最大數目為 64。 請注意，這並非陰影複製的數目相同。

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>什麼是軟體陰影複製的數目上限系統提供者建立，我可以維護磁碟區？

每個磁碟區是 512 的最大數目是軟體陰影複製。 不過，根據預設您可以只維護 64 個陰影複製所使用的共用資料夾的陰影複製功能。 若要變更的共用資料夾的陰影複製功能的限制，請使用下列登錄機碼：**MaxShadowCopies**。

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>我要如何控制陰影複製儲存空間使用的空間？

型別**vssadmin 調整大小的 shadowstorage**命令。

如需詳細資訊，請參閱 < [Vssadmin 調整大小的 shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) TechNet 上。

### <a name="what-happens-when-i-run-out-of-space"></a>當我用盡空間時，會發生什麼事？

會刪除磁碟區陰影複製，從舊的陰影複製。

## <a name="volume-shadow-copy-service-tools"></a>磁碟區陰影複製服務工具

Windows 作業系統會使用 VSS 提供下列工具：

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow 是可用來管理所有的硬體和軟體快照集，您可以在系統上的 VSS 要求者。 DiskShadow 包含命令，如下所示：

  - **清單**:列出 VSS 寫入器、 VSS 提供者，以及陰影複製  
      
  - **建立**:建立新的陰影複製  
      
  - **匯入**:匯入傳送陰影複製  
      
  - **公開 （expose)** :（如磁碟機代號，例如） 公開的持續性的陰影複製  
      
  - **還原**:會還原回指定的陰影複製磁碟區  
      

此工具適用於 IT 專業人員，但開發人員可能也很有用時測試 VSS 寫入器或 VSS 提供者。

只能在 Windows Server 作業系統上使用 DiskShadow。 它並不適用於 Windows 用戶端作業系統。

### <a name="vssadmin"></a>VssAdmin

VssAdmin 用來建立、 刪除和列出陰影複製的相關資訊。 它也可以用來調整陰影複製存放區域 （「 差異區域 」）。

VssAdmin 包含命令，如下所示：

  - **建立陰影**:建立新的陰影複製  
      
  - **刪除陰影**:刪除陰影複製  
      
  - **列出提供者**:列出所有已註冊的 VSS 提供者  
      
  - **list**:列出所有訂閱的 VSS 寫入器  
      
  - **調整大小 shadowstorage**:變更陰影複製存放區域的大小上限  
      

VssAdmin 只可用來管理系統軟體提供者所建立的陰影複製。

在 Windows 用戶端和 Windows Server 作業系統版本上使用 VssAdmin。

## <a name="volume-shadow-copy-service-registry-keys"></a>磁碟區陰影複製服務登錄機碼

下列登錄機碼可供與 VSS 搭配使用：

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

此金鑰用來指定哪些使用者可以存取陰影複製。

如需詳細資訊，請參閱 MSDN 網站上的下列項目：

  - [寫入器的安全性考量](http://go.microsoft.com/fwlink/?linkid=157739)(http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [要求者的安全性考量](http://go.microsoft.com/fwlink/?linkid=180908)(http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

此索引鍵會指定可以儲存在電腦的每個磁碟區的用戶端可存取的陰影複製的數目上限。 用戶端可存取陰影複製會使用共用資料夾的陰影複製。

如需詳細資訊，請參閱 MSDN 網站上的下列項目：

**MaxShadowCopies**底下[登錄機碼備份和還原](http://go.microsoft.com/fwlink/?linkid=180909)(http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

此機碼指定的最小的初始大小，以 mb 為單位的陰影複製存放區域。

如需詳細資訊，請參閱 MSDN 網站上的下列項目：

**MinDiffAreaFileSize**底下[登錄機碼備份和還原](http://go.microsoft.com/fwlink/?linkid=180910)(http://go.microsoft.com/fwlink/?LinkId=180910)

`##`#' 支援的作業系統版本

下表列出 VSS 功能的最小支援的作業系統的版本。


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
<td><p>LUN 的重新同步處理</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong>登錄機碼</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>傳送陰影複製</p></td>
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
<td><p>使用 LUN 交換的快速復原</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003 SP1</p></td>
</tr>
<tr class="odd">
<td><p>多個匯入硬體陰影複製</p>
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
<td>這是能夠一次匯入的陰影複製。 只有一個匯入作業可以執行一次。
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
<td><p>共用資料夾的陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>傳送的自動復原的陰影複製</p></td>
<td><p>都不支援</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>（最多 64) 的並行備份工作階段</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>並行備份的單一還原工作階段</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 sp2</p></td>
</tr>
<tr class="even">
<td><p>最多 8 個並行備份的還原工作階段</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另請參閱

[在 Windows 開發人員中心中的磁碟區陰影複製服務](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)