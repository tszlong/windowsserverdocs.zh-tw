---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: 支援針對虛擬網域控制站使用 Hyper-V 複本
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0203c6de55a4e691d7c484351a3280c49891f317
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866279"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>支援針對虛擬網域控制站使用 Hyper-V 複本

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

這個主題說明使用 Hyper-V 複本來複寫做為網域控制站 (DC) 執行的虛擬機器 (VM) 的支援能力。 Hyper-V 複本是從 Windows Server 2012 開始提供的新 Hyper-V 功能，可在 VM 層級提供內建的複寫機制。  
  
Hyper-V 複本會以非同步方式，跨 LAN 或 WAN 連結，將從主要 Hyper-V 主機選取的 VM 複寫到複本 Hyper-V 主機。 初始複寫完成之後，會以系統管理員所定義的間隔來複寫後續變更。  
  
不一定需要規劃容錯移轉。 規劃的容錯移轉是由系統管理員在主要 VM 上所起始，而任何未複寫的變更都會複製到複本 VM，以防止遺失任何資料。 未規劃的容錯移轉則是在複本 VM 上所起始，以回應主要 VM 上未預期發生的失敗。 因為沒有機會可以傳輸主要 VM 上尚未複寫的變更，因此可能遺失資料。  
  
如需 HYPER-V 複本的詳細資訊，請參閱[HYPER-V 複本概觀](https://technet.microsoft.com/library/jj134172.aspx)並[部署 HYPER-V 複本](https://technet.microsoft.com/library/jj134207.aspx)。  
  
> [!NOTE]  
> Hyper-V 複本只能在 Windows Server Hyper-V 上執行，不能在 Windows 8 上執行的 Hyper-V 版本上執行。  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Windows Server 2012 或更新版本所需的網域控制站

Windows Server 2012 HYPER-V 引進了 Vm-generationid (VMGenID)。 VMGenID 提供一種方式，讓 Hypervisor 可以在發生顯著變更時，與客體作業系統進行通訊。 例如，Hypervisor 可以在發生從快照還原的動作時 (Hyper-V 快照還原技術，非備份還原)，與虛擬 DC 進行通訊。 Windows Server 2012 及更新版本的 AD DS 所知的了解 VMGenID VM 技術，並使用它來偵測執行 hypervisor 作業，例如快照集還原，以便提供更多保護本身時。  
  
> [!NOTE]
> 只有 AD DS 在 Windows Server 2012 Dc 上或更新版本提供這些從 VMGenID; 所產生的安全措施執行所有舊版的 Windows Server 的網域控制站受限於可能使用不支援的機制，例如快照集還原還原虛擬的 DC 的 USN 復原的問題。 如需有關這些保護措施和及其觸發時機的詳細資訊，請參閱 <<c0> [ 虛擬化網域控制站架構](https://technet.microsoft.com/library/jj574118.aspx)。  
  
（計劃性或非計劃性），就會發生 HYPER-V 複本容錯移轉，虛擬的 DC 會偵測到 VMGenID 重設觸發上述的安全功能。 Active Directory 作業之後會照常繼續執行。 複本 VM 會取代主要 VM 執行。  
  
> [!NOTE]  
> 假設現在有兩個執行個體具備相同的 DC 識別，則有可能主要執行個體和複寫執行個體同時執行。 儘管 Hyper-V 複本有適當的控制機制，可確保主要和複本 VM 不會同時執行，但是如果它們之間的連結於 VM 複寫之後失敗，便有可能同時執行。 發生這個罕見的情況時，執行 Windows Server 2012 的虛擬 DC 會有保護措施來協助保護 AD DS，而執行舊版 Windows Server 的虛擬 DC 則無法提供保護。  
  
使用 HYPER-V 複本時，請確定您遵循最佳作法[在 HYPER-V 上執行虛擬網域控制站](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx)。 例如，這個討論建議在虛擬 SCSI 磁碟上儲存 Active Directory 檔案，以便為資料持久性提供更強大的保證。  
  
## <a name="supported-and-unsupported-scenarios"></a>支援與不支援的案例

執行的 Windows Server 2012 或更新版本支援非計劃性容錯移轉和容錯移轉測試的唯一 Vm。 即使是規劃的容錯移轉，Windows Server 2012 或更新版本建議，針對虛擬化網域控制站以降低風險，系統管理員不慎啟動主要 VM 和複寫的 VM 都在相同的時間。  
  
執行舊版 Windows Server 的 VM 支援規劃的容錯移轉，但不支援未規劃的容錯移轉，因為可能會發生 USN 復原。 如需有關 USN 復原的詳細資訊，請參閱 < [USN 和 USN 回復](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。  
  
> [!NOTE]  
> 沒有任何適用於網域或樹系的功能層級要求；只有執行來做為 VM (使用 Hyper-V 複本所複寫) 的 DC 需符合的作業系統需求。 VM 可以部署於包含其他舊版 Windows Server 的實體或虛擬 DC 的樹系中，而且這些 DC 不一定也會使用 Hyper-V 複本進行複寫。  
  
這個支援聲明是以在單一網域樹系中執行的測試為依據，儘管如此，還是支援多網域的樹系設定。 針對這些測試，虛擬網域控制站 DC1 和 DC2 都是相同站台上的 Active Directory 複寫協力電腦，它們都是裝載於在 Windows Server 2012 上執行 Hyper-V 的伺服器上。 執行 DC2 的 VM 客體已啟用 Hyper-V 複本。 複本伺服器裝載於另一個地理位置遙遠的資料中心。 為了協助說明以下概述的測試案例程序，會將在複本伺服器上執行的 VM 稱為 DC2-Rec (雖然實際上它仍保留與原始 VM 相同的名稱)。  
  
### <a name="windows-server-2012"></a>Windows Server 2012

下表說明對於執行 Windows Server 2012 的虛擬網域控制站的支援以及測試案例。  
  
|||  
|-|-|  
|規劃的容錯移轉|未規劃的容錯移轉|  
|支援|支援|  
|測試案例：<br /><br />-DC1 和 DC2 執行 Windows Server 2012。<br /><br />-DC2 已關機，DC2-Rec 上執行容錯移轉。不一定需要規劃容錯移轉。<br /><br />-在 DC2-Rec 啟動之後，它會檢查其資料庫中的 VMGenID 值是否相同的值從 HYPER-V 複本伺服器所儲存的虛擬機器驅動程式。<br /><br />-如此一來，DC2-Rec 會觸發虛擬化保護措施;換句話說，它會重設 InvocationID、 捨棄 RID 集區，並設定初始同步處理需求之前它會假設操作主機角色。 如需初始同步處理需求的詳細資訊，請參閱：<br /><br />-DC2-Rec 然後在其資料庫中儲存 VMGenID 的新值，並認可新 InvocationID 內容中任何後續的更新。<br /><br />-的 InvocationID 已重設，DC1 將會交集 DC2-Rec 所導入，即使它已復原的時間，所有 AD 變更，這表示之後容錯移轉將會安全地都交集 DC2-Rec 上執行所有 AD 更新|這個測試案例與規劃的容錯移轉的案例相同，但有下列例外：<br /><br />-任何 AD 更新在 DC2 上收到但尚未複寫 ad 複寫協力電腦之前容錯移轉事件將會遺失。<br /><br />AD 複寫 ad 到 DC1 的復原點的時間將會從 DC1 複寫回 DC2-Rec 之後於 DC2 上收到的更新。|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 及較舊版本

下表說明對於執行 Windows Server 2008 R2 及較舊版本的虛擬 DC 的支援。  
  
|||  
|-|-|  
|規劃的容錯移轉|未規劃的容錯移轉|  
|支援但不建議使用，因為執行這些 Windows Server 版本的 DC 不支援 VMGenID 或使用相關的虛擬化防護措施。 這會讓它們處於 USN 復原的風險之中。 如需詳細資訊，請參閱 < [USN 和 USN 回復](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。|不支援**附註：** 若沒有 USN 復原的風險，則支援未規劃的容錯移轉，例如，樹系中的單一 DC (不建議使用這個設定)。|  
|測試案例：<br /><br />-DC1 和 DC2 執行 Windows Server 2008 R2。<br /><br />-DC2 已關機，DC2-Rec 上執行規劃的容錯移轉。已在完成關機之前，將 DC2 上的所有資料複寫到 DC2-Rec。<br /><br />-在 DC2-Rec 啟動之後，它會繼續使用相同的 invocationID 與 DC2 與 DC1 複寫。|N/A|  
