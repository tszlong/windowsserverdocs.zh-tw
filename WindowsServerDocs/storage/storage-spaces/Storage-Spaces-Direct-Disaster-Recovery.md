---
title: Hyper-Converged 基礎結構的嚴重損壞修復案例
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/29/2018
description: '本文說明 Microsoft HCI (儲存空間直接存取的災難復原目前可用的案例) '
ms.localizationpriority: medium
ms.openlocfilehash: d1da4cf9e3c6b1d3b0043bc73462730d1e9457c4
ms.sourcegitcommit: 1e94c10ff51f43325fa9184b09bbdfeb8c8fed36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99081764"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>使用儲存空間直接存取的嚴重損壞修復

> 適用於：Windows Server 2019、Windows Server 2016

本主題提供有關如何為損毀修復設定超交集基礎結構 (HCI) 的案例。

有許多公司執行超融合式解決方案並規劃嚴重損壞，可讓您在發生嚴重損壞的情況下，快速維持或取回生產環境的能力。 有幾種方式可以設定 HCI 以進行嚴重損壞修復，這份檔將說明您目前可用的選項。

當發生嚴重損壞時，若要還原可用性的討論，請瞭解所謂的復原時間目標 (RTO) 。 這是以必須還原服務的目標時間持續時間，以避免對企業造成無法接受的後果。 在某些情況下，幾乎會立即還原生產環境。 在其他情況下，必須手動進行系統管理員介入才能還原服務。

現今超融合的嚴重損壞修復選項如下：

1. 利用儲存體複本的多個叢集
2. 叢集之間的 hyper-v 複本
3. 備份與還原

## <a name="multiple-clusters-utilizing-storage-replica"></a>利用儲存體複本的多個叢集

[儲存體複本](../storage-replica/storage-replica-overview.md) 可複寫磁片區，同時支援同步和非同步複寫。 選擇使用同步或非同步複寫時，您應該將復原點目標視為 (RPO) 。 復原點目標是您願意在被視為主要遺失之前，可能遺失的資料遺失量。 如果您使用同步複寫，則會依序同時寫入兩端。 如果您使用非同步，寫入將會非常快速，但仍會遺失。 您應該考慮應用程式或檔案的使用方式，以瞭解哪一種最適合您。

儲存體複本是區塊層級複製機制，而不是檔案層級;也就是說，複寫的資料類型並不重要。 這讓它成為超融合式基礎結構的熱門選項。 儲存體複本也可以在複寫協力電腦之間使用不同類型的磁片磁碟機，因此單一 HCI 上的所有類型儲存體和另一個類型儲存體都能完美地運作。

儲存體複本的其中一個重要功能是，它可以在 Azure 和內部部署中執行。 您可以設定內部部署至內部部署、Azure 至 Azure，甚至是內部部署至 Azure 的 (，反之亦然) 。

在此案例中，有兩個不同的獨立叢集。 若要設定 HCI 之間的儲存體複本，您可以依照叢集 [對叢集儲存體](../storage-replica/cluster-to-cluster-storage-replication.md)複寫中的步驟執行。

![儲存體複寫圖表](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

下列考慮適用于部署儲存體複本。

1.    設定複寫是在容錯移轉叢集之外進行。
2.    選擇複寫方法將取決於您的網路延遲和 RPO 需求。 同步會在具有損毀一致性的低延遲網路上複寫資料，以確保在發生失敗時不會遺失任何資料。 非同步會透過具有較高延遲的網路複寫資料，但每個網站在失敗時可能不會有相同的複本。
3.    在發生嚴重損壞的情況下，叢集之間的容錯移轉不會自動進行，且需要透過儲存體複本 PowerShell Cmdlet 以手動方式進行協調。 在上圖中，>servicefabric.clustera.contoso.com 是主要的，而 ClusterB 是次要。 如果 >servicefabric.clustera.contoso.com 停止運作，您必須手動將 ClusterB 設定為主要，才能讓資源上線。 >servicefabric.clustera.contoso.com 完成備份後，您必須將它設為次要。 當所有資料都已同步處理之後，請進行變更，並將角色交換回最初設定的方式。
4.    由於儲存體複本只會複寫資料，因此必須在複本夥伴的容錯移轉叢集管理員中建立新的虛擬機器或 Scale Out 檔案伺服器 (SOFS) 利用此資料。

如果您的叢集上有虛擬機器或 SOFS 正在執行，則可以使用儲存體複本。 您可以使用 PowerShell 腳本，透過手動或自動方式在複本 HCI 中讓資源上線。

## <a name="hyper-v-replica"></a>Hyper-V 複本

[Hyper-v 複本](../../virtualization/hyper-v/manage/set-up-hyper-v-replica.md) 針對超融合式基礎結構上的災難復原提供虛擬機器層級複寫。 Hyper-v 複本可以做的是建立虛擬機器，並將它複寫至次要網站或 Azure (複本) 。 然後，在次要網站上，Hyper-v 複本可以將虛擬機器複寫到第三個 (擴充複本) 。

![Hyper-v 複寫圖表](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

使用 Hyper-v 複本時，Hyper-v 會負責複寫。 當您第一次啟用虛擬機器的複寫時，有三種方式可以選擇將初始複本傳送至對應的複本叢集 (s) 。

1.    透過網路傳送初始複本
2.    將初始複本傳送到外部媒體，讓它可以手動複製到您的伺服器
3.    使用已在複本主機上建立的現有虛擬機器

另一個選項適用于您想要進行此初始複寫的時機。

1.    立即開始複寫
2.    排程初始複寫發生的時間。

您將需要的其他考慮如下：

- 您想要複寫的 VHD/VHDX。 您可以選擇全部複寫，或只複製其中一個。
- 您要儲存的復原點數目。 如果您想要有數個有關您想要還原的時間點的選項，請指定您想要的數目。 如果您只想要一個還原點，也可以選擇它。
- 您希望磁碟區陰影複製服務 (VSS) 複寫增量陰影複製的頻率。
-  (30 秒、5分鐘、15分鐘) 複寫變更的頻率。

當 HCI 參與 Hyper-v 複本時，您必須在每個叢集中建立 [Hyper-v 複本](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization) 代理人資源。 此資源會執行幾項作業：

1.    為您的每個叢集提供單一命名空間，以便連接到 Hyper-v 複本。
2.    判斷在該叢集中，複本 (或擴展複本) 將在第一次收到複本時所在的節點。
3.    當虛擬機器移至另一個節點時，會追蹤哪個節點擁有複本 (或擴充的複本) 。 它需要追蹤這一點，如此一來，當複寫進行時，它就可以將資訊傳送到適當的節點。

## <a name="backup-and-restore"></a>備份與還原

一種傳統的嚴重損壞修復選項，它並不是很重要，而是整個叢集或叢集中節點的故障。 此案例的其中一個選項會使用 Windows Server Backup。

建議您一律定期備份超交集基礎結構。 當叢集服務正在執行時，如果您進行系統狀態備份，叢集登錄資料庫將會是該備份的一部分。 還原叢集或資料庫有兩種不同的方法 (非授權和授權) 。

### <a name="non-authoritative"></a>非授權

您可以使用 Windows Server Backup 來完成非授權的還原，這等同于只有叢集節點本身的完整還原。 如果您只需要還原叢集節點 (而且叢集登錄資料庫) 和所有目前的叢集資訊都很好，您就可以使用非權威還原。 您可以透過 Windows Server Backup 介面或命令列 WBADMIN.EXE 來完成非授權的還原。

一旦您還原節點，讓它加入叢集。 將會發生的情況是，它會移至現有執行中的叢集，並使用目前的內容來更新其所有資訊。

### <a name="authoritative"></a>權威

相反地，叢集設定的授權還原會將叢集設定回來。 只有當叢集資訊本身已遺失且需要還原時，才應完成這類的還原。 例如，有人不慎刪除了內含超過1000個共用的檔案伺服器，而且您需要它們回來。 完成叢集的授權還原需要從命令列執行備份。

在叢集節點上起始系統授權還原時，叢集的所有其他節點上都會停止叢集服務，而且叢集設定會凍結。 這就是為什麼執行還原的節點上的叢集服務必須先啟動，才能使用叢集設定的新複本來形成叢集的原因。

若要執行授權還原，可以完成下列步驟。

1. 從系統管理命令提示字元執行 WBADMIN.EXE，以取得您想要安裝的最新備份版本，並確保系統狀態是您可以還原的其中一個元件。

    ```powershell
    wbadmin get versions
    ```

2. 判斷您擁有的版本備份是否有叢集登錄資訊做為元件。 您需要在此命令中有幾個專案，也就是要在步驟3中使用的版本和應用程式/元件。 例如，假設備份是在2018年1月3日的2：04am 完成，這就是您需要還原的備份。

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3. 啟動授權還原，只復原您需要的叢集登錄版本。

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

一旦還原完成，此節點就必須是先啟動叢集服務的節點，然後形成叢集。 接著，所有其他節點都必須啟動並加入叢集。

## <a name="summary"></a>摘要

為了加總這一點，超融合式嚴重損壞修復是應該謹慎規劃的。 有幾個案例可以滿足您的需求，而且應該經過徹底的測試。 要注意的一個專案是，如果您熟悉過去的容錯移轉叢集，stretch 叢集就是多年來的最受歡迎的選項。 超融合式解決方案有一些設計變更，而且是以復原為基礎。 如果您在超交集叢集中遺失兩個節點，整個叢集將會停止運作。 就這種情況而言，在超交集的環境中，不支援延展案例。
