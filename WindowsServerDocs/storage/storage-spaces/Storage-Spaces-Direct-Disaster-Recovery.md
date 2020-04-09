---
title: 超融合式基礎結構的嚴重損壞修復案例
ms.prod: windows-server
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/29/2018
description: 本文說明 Microsoft HCI （儲存空間直接存取）嚴重損壞修復的今日可用案例
ms.localizationpriority: medium
ms.openlocfilehash: 5f3159e0c215d898848df71c6488cd491b7ded38
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859161"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>使用儲存空間直接存取的嚴重損壞修復

> 適用于： Windows Server 2019、Windows Server 2016

本主題提供如何設定超融合基礎結構（HCI）以進行嚴重損壞修復的案例。

許多公司都在執行超融合式解決方案，而針對嚴重損壞狀況的規劃，讓您能夠在發生嚴重損壞時，快速地繼續進行並恢復生產環境。 有數種方式可以設定 HCI 以進行嚴重損壞修復，而本檔將說明您目前可以使用的選項。

當發生嚴重損壞時，還原可用性的討論會圍繞所謂的復原時間目標（RTO）。 這是針對服務必須還原的目標時間長度，以避免對企業造成無法接受的結果。 在某些情況下，此程式可能會在幾乎立即還原生產環境時自動發生。 在其他情況下，還原服務必須手動進行系統管理員介入。

現今超融合的嚴重損壞修復選項如下：

1. 利用儲存體複本的多個叢集
2. 叢集之間的 hyper-v 複本
3. 備份與還原

## <a name="multiple-clusters-utilizing-storage-replica"></a>利用儲存體複本的多個叢集

[儲存體複本](../storage-replica/storage-replica-overview.md)可讓您複寫磁片區，並同時支援同步和非同步複寫。 選擇使用同步或非同步複寫時，您應該考慮您的復原點目標（RPO）。 復原點目標是您願意在被視為主要損失之前產生的可能資料遺失量。 如果您使用同步複寫，則會依序寫入兩個結束。 如果您使用非同步方式，寫入會非常快速地複寫，但仍可能遺失。 您應該考慮應用程式或檔案使用方式，以查看哪一個最適合您。

儲存體複本是區塊層級複製機制與檔案層級的比較;也就是說，要複寫的資料類型並不重要。 這讓它成為超融合式基礎結構的熱門選項。 「儲存體複本」也可以利用複寫協力電腦之間不同類型的磁片磁碟機，因此在一個 HCI 上儲存一種類型的儲存體，而在另一種類型的儲存體上，則完全沒問題。 

儲存體複本的一項重要功能，就是它可以在 Azure 和內部部署環境中執行。 您可以設定內部部署至內部部署、Azure 或甚至是內部部署至 Azure （反之亦然）。

在此案例中，有兩個獨立的獨立叢集。 如需在 HCI 之間設定儲存體複本，您可以依照叢集[對叢集儲存體](../storage-replica/cluster-to-cluster-storage-replication.md)複寫中的步驟進行。

![儲存體複寫圖表](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

部署儲存體複本時適用下列考慮事項。 

1.    設定複寫是在容錯移轉叢集外部完成。 
2.    選擇複寫方法將取決於您的網路延遲和 RPO 需求。 同步會在具有損毀一致性的低延遲網路上複寫資料，以確保失敗時不會遺失資料。 非同步會透過較高延遲的網路複寫資料，但每個網站在失敗時可能不會有相同的複本。 
3.    萬一發生嚴重損壞，叢集之間的容錯移轉並不會自動進行，而且必須透過儲存體複本 PowerShell Cmdlet 手動協調。 在上圖中，Clustera.contoso.com 是主要的，而 ClusterB 是次要。 如果 Clustera.contoso.com 停止運作，您必須手動將 ClusterB 設定為 [主要]，然後才能讓資源上線。 Clustera.contoso.com 備份之後，您必須將它設為 [次要]。 當所有資料都已同步處理之後，請進行變更，並將角色交換回到原先設定的方式。
4.    由於儲存體複本只會複寫資料，因此使用此資料的新虛擬機器或 Scale Out 檔案伺服器（SOFS）必須在複本夥伴的容錯移轉叢集管理員內建立。

如果您的叢集上有虛擬機器或 SOFS 正在執行，就可以使用儲存體複本。 透過使用 PowerShell 腳本，可以手動或自動化將複本 HCI 中的資源上線。

## <a name="hyper-v-replica"></a>Hyper-V 複本

[Hyper-v 複本](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)提供虛擬機器層級複寫，以在超融合式基礎結構上進行嚴重損壞修復。 Hyper-v 複本可執行檔動作是將虛擬機器複寫至次要網站或 Azure （複本）。 然後，從次要網站，Hyper-v 複本可以將虛擬機器複寫到第三個（延伸複本）。

![Hyper-v 複寫圖表](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

使用 Hyper-v 複本時，Hyper-v 會負責複寫。 當您第一次啟用虛擬機器進行複寫時，會有三個選項可供您想要如何將初始複本傳送到對應的複本叢集。

1.    透過網路傳送初始複本
2.    將初始複本傳送到外部媒體，讓它可以手動複製到您的伺服器上
3.    使用已在複本主機上建立的現有虛擬機器

另一個選項適用于您想要進行此初始複寫的時機。

1.    立即開始複寫
2.    排程初始複寫發生的時間。 

您需要的其他考慮如下：

- 您想要複寫的 VHD/VHDX。 您可以選擇全部複寫，或只複寫其中一個。
- 您想要儲存的復原點數目。 如果您想要有數個選項可供您想要還原的時間點，則您會想要指定您想要的數目。 如果您只想要一個還原點，您也可以選擇它。
- 您想要讓磁碟區陰影複製服務（VSS）複寫增量陰影複製的頻率。
- 變更複寫的頻率（30秒、5分鐘、15分鐘）。

當 HCI 參與 Hyper-v 複本時，您必須在每個叢集中建立[Hyper-v 複本](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/)代理人資源。 此資源會執行幾項工作：

1.    提供每個叢集的單一命名空間，讓 Hyper-v 複本連接到該叢集。
2.    決定當複本（或擴充複本）第一次收到複本時，該叢集內的哪個節點會位於該叢集中。
3.    當虛擬機器移至另一個節點時，會持續追蹤哪個節點擁有複本（或延伸複本）。 它需要追蹤這一點，以便在複寫發生時，可以將資訊傳送至適當的節點。

## <a name="backup-and-restore"></a>備份與還原

一個傳統的嚴重損壞修復選項，其中未提到太多，但也就是整個叢集或叢集中節點的失敗。 這種情況下，其中一個選項會使用 Windows NT Backup。 

定期備份超融合式基礎結構時，一定是建議的做法。 當叢集服務正在執行時，如果您進行系統狀態備份，叢集登錄資料庫會是該備份的一部分。 還原叢集或資料庫有兩種不同的方法（非授權和授權）。

### <a name="non-authoritative"></a>非權威

您可以使用 Windows NT Backup 來完成非權威還原，這等同于只有叢集節點本身的完整還原。 如果您只需要還原叢集節點（和叢集登錄資料庫）和所有目前的叢集資訊，您會使用非權威還原。 您可以透過 Windows NT 備份介面或命令列 WBADMIN 來完成非權威還原。CONVERT.EXE.

還原節點後，讓它加入叢集。 會發生什麼事，那就是它會移至現有的執行中叢集，並使用目前的內容來更新其所有資訊。

### <a name="authoritative"></a>還原

相反地，叢集設定的授權還原會使叢集設定回到當時。 只有當叢集資訊本身遺失且需要還原時，才應該完成這種類型的還原。 例如，有人不小心刪除了包含超過1000個共用的檔案伺服器，而您需要它們。 若要完成叢集的授權還原，您必須從命令列執行備份。

在叢集節點上起始授權還原時，叢集的所有其他節點上的叢集服務都會停止，而且叢集設定會凍結。 這就是為什麼執行還原之節點上的叢集服務必須先啟動，才能使用新的叢集設定複本形成叢集的原因。

若要透過授權還原執行，可以完成下列步驟。

1.    執行 WBADMIN。從系統管理命令提示字元，取得您想要安裝的最新備份版本，並確定系統狀態是您可以還原的其中一個元件。

    ```powershell
    Wbadmin get versions
    ```

2.    判斷您擁有的版本備份是否在其中做為元件的叢集登錄資訊。 在步驟3中，您將需要此命令的幾個專案，也就是版本和應用程式/元件。 例如，如果備份是在2018年1月3日完成，則為2：04am，而這是您需要還原的版本。

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  啟動 [授權還原]，只復原您需要的叢集登錄版本。 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

進行還原之後，此節點必須是要先啟動叢集服務的節點，並形成叢集。 接著，所有其他節點都必須啟動並加入叢集。

## <a name="summary"></a>摘要 

為了加總，超交集的嚴重損壞修復是應該仔細規劃的專案。 有幾個案例最適合您的需求，而且應該進行徹底測試。 其中一個要注意的事項是，如果您在過去已熟悉容錯移轉叢集，延展叢集在多年中已經是非常受歡迎的選項。 超融合解決方案有一些設計變更，而它是以復原為基礎。 如果您遺失超交集叢集中的兩個節點，則整個叢集將會關閉。 就這種情況而言，在超融合式環境中，不支援延展案例。


