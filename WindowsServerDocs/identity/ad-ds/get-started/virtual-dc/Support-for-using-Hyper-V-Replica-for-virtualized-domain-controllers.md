---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: 支援針對虛擬網域控制站使用 Hyper-V 複本
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b4c04583452182479064b48e01c6ee495620e040
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962970"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>支援針對虛擬網域控制站使用 Hyper-V 複本

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這個主題說明使用 Hyper-V 複本來複寫做為網域控制站 (DC) 執行的虛擬機器 (VM) 的支援能力。 Hyper-V 複本是從 Windows Server 2012 開始提供的新 Hyper-V 功能，可在 VM 層級提供內建的複寫機制。  
  
Hyper-V 複本會以非同步方式，跨 LAN 或 WAN 連結，將從主要 Hyper-V 主機選取的 VM 複寫到複本 Hyper-V 主機。 初始複寫完成之後，會以系統管理員所定義的間隔來複寫後續變更。  
  
不一定需要規劃容錯移轉。 規劃的容錯移轉是由系統管理員在主要 VM 上所起始，而任何未複寫的變更都會複製到複本 VM，以防止遺失任何資料。 未規劃的容錯移轉則是在複本 VM 上所起始，以回應主要 VM 上未預期發生的失敗。 因為沒有機會可以傳輸主要 VM 上尚未複寫的變更，因此可能遺失資料。  
  
如需 Hyper-v 複本的詳細資訊，請參閱[Hyper-v 複本總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134172(v=ws.11))和[部署 hyper-v 複本](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134207(v=ws.11))。  
  
> [!NOTE]  
> Hyper-V 複本只能在 Windows Server Hyper-V 上執行，不能在 Windows 8 上執行的 Hyper-V 版本上執行。  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>需要 Windows Server 2012 或更新版本的網域控制站

Windows Server 2012 Hyper-v 引進了 VM-GenerationID （VMGenID）。 VMGenID 提供一種方式，讓 Hypervisor 可以在發生顯著變更時，與客體作業系統進行通訊。 例如，Hypervisor 可以在發生從快照還原的動作時 (Hyper-V 快照還原技術，非備份還原)，與虛擬 DC 進行通訊。 Windows Server 2012 和更新版本中的 AD DS 都知道 VMGenID VM 技術，並使用它來偵測是否執行虛擬程式作業（例如快照還原），讓它能夠更適當地保護本身。  
  
> [!NOTE]
> 只有 Windows Server 2012 Dc 或更新版本上的 AD DS 提供 VMGenID 所產生的這些安全措施;執行所有舊版 Windows Server 的 Dc 會受限於使用不支援的機制（例如快照還原）還原虛擬 DC 時可能發生的 USN 復原問題。 如需這些保護措施和觸發時機的詳細資訊，請參閱[虛擬網域控制站架構](./virtualized-domain-controller-architecture.md)。  
  
當發生 Hyper-v 複本容錯移轉（已規劃或未規劃）時，虛擬化的 DC 會偵測到 VMGenID 重設，並觸發上述的安全性功能。 Active Directory 作業之後會照常繼續執行。 複本 VM 會取代主要 VM 執行。  
  
> [!NOTE]  
> 假設現在有兩個執行個體具備相同的 DC 識別，則有可能主要執行個體和複寫執行個體同時執行。 儘管 Hyper-V 複本有適當的控制機制，可確保主要和複本 VM 不會同時執行，但是如果它們之間的連結於 VM 複寫之後失敗，便有可能同時執行。 發生這個罕見的情況時，執行 Windows Server 2012 的虛擬 DC 會有保護措施來協助保護 AD DS，而執行舊版 Windows Server 的虛擬 DC 則無法提供保護。  
  
使用 Hyper-v 複本時，請確定您遵循在[hyper-v 上執行虛擬網域控制站](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))的最佳作法。 例如，這個討論建議在虛擬 SCSI 磁碟上儲存 Active Directory 檔案，以便為資料持久性提供更強大的保證。  
  
## <a name="supported-and-unsupported-scenarios"></a>支援和不支援的案例

只有執行 Windows Server 2012 或更新版本的 Vm 支援未計畫的容錯移轉和測試容錯移轉。 即使是規劃的容錯移轉，建議將 Windows Server 2012 或更新版本用於虛擬化 DC，以便在系統管理員不小心同時啟動主要 VM 和複寫 VM 時，降低風險。  
  
執行舊版 Windows Server 的 VM 支援規劃的容錯移轉，但不支援未規劃的容錯移轉，因為可能會發生 USN 復原。 如需 USN 復原的詳細資訊，請參閱[usn 和 Usn 回復](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))。  
  
> [!NOTE]  
> 沒有任何適用於網域或樹系的功能層級要求；只有執行來做為 VM (使用 Hyper-V 複本所複寫) 的 DC 需符合的作業系統需求。 VM 可以部署於包含其他舊版 Windows Server 的實體或虛擬 DC 的樹系中，而且這些 DC 不一定也會使用 Hyper-V 複本進行複寫。  
  
這個支援聲明是以在單一網域樹系中執行的測試為依據，儘管如此，還是支援多網域的樹系設定。 針對這些測試，虛擬網域控制站 DC1 和 DC2 都是相同站台上的 Active Directory 複寫協力電腦，它們都是裝載於在 Windows Server 2012 上執行 Hyper-V 的伺服器上。 執行 DC2 的 VM 客體已啟用 Hyper-V 複本。 複本伺服器裝載於另一個地理位置遙遠的資料中心。 為了協助說明以下概述的測試案例程序，會將在複本伺服器上執行的 VM 稱為 DC2-Rec (雖然實際上它仍保留與原始 VM 相同的名稱)。  
  
### <a name="windows-server-2012"></a>Windows Server 2012

下表說明對於執行 Windows Server 2012 的虛擬網域控制站的支援以及測試案例。  
  
|||  
|-|-|  
|計劃性容錯移轉|未規劃的容錯移轉|  
|支援|支援|  
|測試案例：<p>-DC1 和 DC2 正在執行 Windows Server 2012。<p>-DC2 已關閉，並在 DC2-Rec 上執行容錯移轉。容錯移轉可以是已規劃或未計畫的。<p>-DC2 啟動之後，它會檢查它在其資料庫中的 VMGenID 值是否與 Hyper-v 複本伺服器所儲存之虛擬機器驅動程式中的值相同。<p>-因此，DC2-Rec 觸發了虛擬化保護措施;換句話說，它會重設其 InvocationID、捨棄其 RID 集區，並設定初始同步處理需求，然後才會採用操作主機角色。 如需初始同步處理需求的詳細資訊，請參閱：<p>-DC2 會在其資料庫中儲存 VMGenID 的新值，並認可新 InvocationID 內容中的任何後續更新。<p>-由於 InvocationID 重設的結果，DC1 會針對 DC2-Rec 引進的所有 AD 變更進行匯總，即使它已準時回復也是一樣，這表示在容錯移轉之後，在 DC2 上執行的任何 AD 更新都會安全地收斂|這個測試案例與規劃的容錯移轉的案例相同，但有下列例外：<p>-在進行容錯移轉事件之前，在 DC2 上收到但尚未由 AD 複寫到複寫協力電腦的任何 AD 更新都將遺失。<p>-在將 AD 複寫到 DC1 之後，在 DC2 上收到的 AD 更新會從 DC1 複寫回到 DC2-Rec。|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 及較舊版本

下表說明對於執行 Windows Server 2008 R2 及較舊版本的虛擬 DC 的支援。  
  
|||  
|-|-|  
|計劃性容錯移轉|未規劃的容錯移轉|  
|支援但不建議使用，因為執行這些 Windows Server 版本的 DC 不支援 VMGenID 或使用相關的虛擬化防護措施。 這會讓它們處於 USN 復原的風險之中。 如需詳細資訊，請參閱[usn 和 Usn 回復](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))。|不支援的**注意事項：** 如果 USN 復原不是風險，例如樹系中的單一 DC （不建議設定），則會支援未計畫的容錯移轉。|  
|測試案例：<p>-DC1 和 DC2 正在執行 Windows Server 2008 R2。<p>-DC2 已關閉，而且已規劃的容錯移轉會在 DC2-Rec 上執行。在關機完成之前，DC2 上的所有資料會複寫至 DC2-Rec。<p>-DC2 啟動之後，它會使用與 DC2 相同的 invocationID，繼續與 DC1 進行複寫。|不適用|  
