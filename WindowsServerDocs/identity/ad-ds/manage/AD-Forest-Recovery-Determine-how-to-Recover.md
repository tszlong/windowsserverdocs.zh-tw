---
title: AD 樹系復原-決定如何復原樹系
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: dda621c8b567822a882e8230aba604ce0a115835
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939788"
---
# <a name="determine-how-to-recover-the-forest"></a>判斷如何復原樹系

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

復原整個 Active Directory 樹系牽涉到從備份還原，或在樹系中的每個網域控制站) DC (上重新安裝 Active Directory Domain Services (AD DS) 。 復原樹系時，會將樹系中的每個網域還原為上次受信任備份時的狀態。 因此，還原作業會導致至少遺失下列 Active Directory 資料：

- 所有物件 (例如最後一個受信任備份之後新增的使用者和電腦) 
- 自上次信任備份之後對現有物件所做的所有更新
- 在 AD DS (中設定分割區或架構磁碟分割的所有變更，例如上次信任備份之後) 的架構變更

針對樹系中的每個網域，必須知道網域系統管理員帳戶的密碼。 這是內建的系統管理員帳戶的密碼。 您也必須知道 DSRM 密碼，才能執行 DC 的系統狀態還原。 一般情況下，最好是將系統管理員帳戶和 DSRM 密碼歷程記錄保存在安全的位置，只要備份有效，也就是在標記存留期內或在已刪除的物件存留期內，如果 Active Directory 資源回收筒已啟用。 您也可以將 DSRM 密碼與網域使用者帳戶同步處理，以便更容易記住。 如需詳細資訊，請參閱知識庫文章 [961320](https://support.microsoft.com/kb/961320)。 您必須在樹系復原之前完成 DSRM 帳戶同步處理，做為準備工作的一部分。

> [!NOTE]
> 系統管理員帳戶預設為內建系統管理員群組的成員，如同 Domain Admins 和 Enterprise Admins 群組。 此群組具有網域中所有 Dc 的完整控制權。

## <a name="determining-which-backups-to-use"></a>判斷要使用的備份

請定期備份每個網域至少有兩個可寫入的 Dc，讓您有數個備份可供選擇。 請注意，您不能使用唯讀網域控制站 (RODC) 的備份來還原可寫入的 DC。 建議您在失敗發生之前的幾天內，使用備份來還原 Dc。 一般而言，您必須判斷 recentness 和還原的資料 safeness 之間的取捨。 選擇最新備份可還原更有用的資料，但可能會增加重新將危險資料引進還原之樹系的風險。

還原系統狀態備份取決於原始的作業系統和備份的伺服器。 例如，您不應該將系統狀態備份還原到不同的伺服器。 在此情況下，您可能會看到下列警告：

「指定的備份與目前的備份的伺服器不同。 由於伺服器可能無法使用，因此不建議您對替代伺服器執行系統狀態復原，因為伺服器可能會變成無法使用。 您確定要使用此備份來復原目前的伺服器嗎？」

如果您需要將 Active Directory 還原至不同的硬體，請建立完整的伺服器備份，並規劃執行完整伺服器復原。

> [!IMPORTANT]
> 從 Windows Server 2008 開始，就不支援在新硬體或相同硬體上，將系統狀態備份還原至新的 Windows Server 安裝。 如果在相同硬體上重新安裝 Windows Server，則您可以依照本指南稍後的建議來還原網域控制站，如下所示：
>
> 1. 執行完整伺服器還原，以便還原作業系統和所有檔案和應用程式。
> 2. 使用 wbadmin.exe 執行系統狀態還原，以便將 SYSVOL 標示為授權。
>
> 如需詳細資訊，請參閱 Microsoft 知識庫文章 [249694](https://support.microsoft.com/kb/249694)。

如果發生失敗的時間不明，請進一步調查以找出持有樹系最後安全狀態的備份。 這種方式比較不理想。 因此，我們強烈建議您每天保留有關 AD DS 健全狀況狀態的詳細記錄，如此一來，如果有全樹系的失敗，則可以識別出大約的失敗時間。 您也應該保留備份的本機複本，以加快復原的速度。

如果 Active Directory 資源回收筒已啟用，則備份存留期等於 **msds-deletedobjectlifetime** 值或 **tombstoneLifetime** 值（以較小者為准）。 如需詳細資訊，請參閱 [Active Directory 資源回收筒逐步指南](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657) 。

或者，您也可以使用 Active Directory 資料庫掛接工具 ( # A0) 和輕量目錄存取通訊協定 (LDAP) 工具，例如 Ldp.exe 或 Active Directory 消費者和電腦，以識別哪個備份具有樹系的最後安全狀態。 包含在 Windows Server 2008 和更新版本之 Windows Server 作業系統中的 Active Directory 資料庫掛接工具，會將儲存在備份或快照集中的 Active Directory 資料公開為 LDAP 伺服器。 然後，您可以使用 LDAP 工具來流覽資料。 這種方法的優點是，您不需要以目錄服務還原模式重新開機任何 DC (DSRM) 檢查 AD DS 的備份內容。

如需有關使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱 [Active Directory 資料庫掛接工具的逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10))。

您也可以使用 **ntdsutil snapshot** 命令來建立 Active Directory 資料庫的快照集。 藉由排程工作以定期建立快照集，您可以在一段時間內取得 Active Directory 資料庫的額外複本。 您可以使用這些複本來更清楚地識別整個樹系發生失敗的時間，然後選擇要還原的最佳備份。 若要建立快照集，請使用 windows Server 2008 隨附的 **ntdsutil** 版本，或是 windows Vista 或更新版本的遠端伺服器管理工具 (RSAT) 。 目標 DC 可以執行任何版本的 Windows Server。 如需使用 **ntdsutil snapshot** 命令的詳細資訊，請參閱 [snapshot](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10))。

## <a name="determining-which-domain-controllers-to-restore"></a>判斷要還原的網域控制站

在決定要還原的網域控制站時，簡化還原程式是很重要的因素。 建議您為每個網域提供專用的 DC，作為還原的慣用 DC。 專用的還原 DC 可讓您更容易可靠地規劃和執行樹系復原，因為您使用的是用來執行還原測試的相同來源設定。 您可以編寫復原的腳本，而不會爭用不同的設定，例如，DC 是否保有操作主機角色，或它是否為 GC 或 DNS 伺服器。

> [!NOTE]
> 雖然不建議您在簡單的情況下還原操作主機角色持有者，但某些組織可能會選擇還原，以提供其他優點。 例如，還原 RID 主機可能有助於防止在復原期間管理 Rid 時發生問題。

選擇最符合下列準則的 DC：

- 可寫入的 DC。 這是必要項目。

- 在支援 VM >generationid 的虛擬程式上，執行 Windows Server 2012 作為虛擬機器的 DC。 此 DC 可以用來做為複製的來源。
- 可以在實體或虛擬網路上存取的 DC，最好是位於資料中心。 如此一來，您就可以在樹系復原期間輕鬆地將它與網路隔離。
- 具有良好完整伺服器備份的 DC。 良好的備份是可以成功還原的備份，在失敗之前的幾天內都有很多有用的資料。
- 在失敗之前，網功能變數名稱稱系統 (DNS) server 的 DC。 這可節省重新安裝 DNS 所需的時間。
- 如果您也使用 Windows 部署服務，請選擇未設定為使用 BitLocker 網路解除鎖定的 DC。 在此情況下，不支援 BitLocker 網路解除鎖定，以用於在樹系復原期間從備份還原的第一個 DC。

   BitLocker 網路解除鎖定，因為 *唯一* 的金鑰保護裝置 *無法* 在已部署 Windows 部署服務 (WDS) 的 dc 上使用，因為這樣做會導致第一個 dc 需要 Active Directory 的情況，且 WDS 必須正常運作才能解除鎖定。 但是，在您還原第一個 DC 之前，Active Directory 尚無法供 WDS 使用，因此無法解除鎖定。

   若要判斷是否已設定 DC 使用 BitLocker 網路解除鎖定，請檢查下列登錄機碼中是否已識別出網路解除鎖定憑證：

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

處理或還原包含 Active Directory 的備份檔案時，請維護安全性程式。 隨附樹系復原的緊急情況可能會不慎導致忽略安全性最佳作法。 如需詳細資訊，請參閱在 [保護 Active Directory 安裝和日常作業的最佳作法指南](/previous-versions/windows/it-pro/windows-2000-server/bb727066(v=technet.10))中標題為「建立網域控制站備份和還原策略」一節：第二部分。

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>識別目前的樹系結構和 DC 函數

藉由識別樹系中的所有網域，判斷目前的樹系結構。 建立每個網域中的所有 Dc 清單，特別是具有備份的 Dc，以及可作為複製來源的虛擬化 Dc。 樹系根域的 Dc 清單最重要，因為您將會先復原此網域。 還原樹系根域之後，您可以使用 Active Directory 嵌入式管理單元，取得樹系中其他網域、Dc 和網站的清單。

準備資料表以顯示網域中每個 DC 的函式，如下列範例所示。 這可協助您在復原後還原至樹系的前置設定失敗。

|DC 名稱|作業系統|FSMO|GC|RODC|備份|DNS|Server Core|VM|VM-GenID|
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|
|DC_1|Windows Server 2012|架構主機，網網域命名主機|是|否|是|否|否|是|是|
|DC_2|Windows Server 2012|無|是|否|是|是|否|是|是|
|DC_3|Windows Server 2012|基礎結構主機|否|否|否|是|是|是|是|
|DC_4|Windows Server 2012|PDC 模擬器，RID 主機|是|否|否|否|否|是|否|
|DC_5|Windows Server 2012|無|否|否|是|是|否|是|是|
|RODC_1|Windows Server 2008 R2|無|是|是|是|是|是|是|否|
|RODC_2|Windows Server 2008|無|是|是|否|是|是|是|否|

針對樹系中的每個網域，識別具有該網域 Active Directory 資料庫之受信任備份的單一可寫入 DC。 當您選擇要還原 DC 的備份時，請特別小心。 如果發生失敗的日期和原因大約是已知的，一般建議是使用在該日期之前的幾天進行的備份。

在此範例中，有四個備份候選項目： DC_1、DC_2、DC_4 和 DC_5。 在這些備份候選項目中，您只會還原一個。 建議的 DC 因為下列原因而 DC_5：

- 它滿足將其作為虛擬化 DC 複製來源的需求，也就是，它會在支援 VM >generationid 的虛擬程式上執行 Windows Server 2012 作為虛擬 DC、執行可 (複製的軟體，或者，如果無法複製) ，則可以移除。 還原之後，PDC 模擬器角色將會被收回至該伺服器，並可新增至網域的 Cloneable 網域控制站群組。
- 它會執行 Windows Server 2012 的完整安裝。 執行 Server Core 安裝的 DC 可能較方便作為復原目標。
- 它是 DNS 伺服器。 因此，不需要重新安裝 DNS。

> [!NOTE]
> 因為 DC_5 不是通用類別目錄伺服器，所以它也有一個優點，那就是不需要在還原後移除通用類別目錄。 但是，即使 DC 也是通用類別目錄伺服器，也不是決定性的因素，因為從 Windows Server 2012 開始，所有的 Dc 預設都是通用類別目錄伺服器，而且在還原之後，建議在任何情況下，將通用類別目錄視為樹系復原程式的一部分來移除和新增通用類別目錄。

## <a name="recover-the-forest-in-isolation"></a>以隔離方式復原樹系

慣用的案例是在第一次還原的 DC 恢復生產之前，關閉所有可寫入的 Dc。 這樣可確保任何危險的資料都不會複寫回復原的樹系中。 請特別注意，關閉所有操作主機角色持有者。

> [!NOTE]
> 在某些情況下，您可能會將打算復原的第一個 DC 移至隔離的網路，同時讓其他 Dc 保持上線，以便將系統停機時間降到最低。 例如，如果您要從失敗的架構升級進行復原，您可以選擇在執行獨立的復原步驟時，讓網域控制站在生產網路上保持執行狀態。

如果您正在執行虛擬化的 Dc，則可以將它們移至與您將執行復原的生產網路隔離的虛擬網路。 將虛擬化的 Dc 移至不同的網路提供兩個優點：

- 復原的 Dc 無法重複發生造成樹系復原的問題，因為它們是隔離的。
- 虛擬化 DC 複製可以在個別的網路上執行，如此一來，您就可以執行和測試重要的 Dc 數目，然後再將它們重新進入生產網路。

如果您是在實體硬體上執行 Dc，請將您計畫在樹系根域中還原之第一個 DC 的網路纜線中斷連線。 可能的話，也請中斷所有其他 Dc 的網路纜線。 這可防止 Dc 複寫，如果在樹系復原過程中不小心啟動 Dc。

在分散于多個位置的大型樹系中，很難保證所有可寫入的 Dc 都已關閉。 基於這個理由，除了重新整理中繼資料之外，復原步驟（例如重設電腦帳戶和 krbtgt 帳戶）的設計目的，是為了確保復原的可寫入 Dc 不會使用危險的可寫入 Dc 來進行複寫 (以防部分在) 樹系中仍處於線上狀態。

不過，只有讓可寫入的 Dc 離線，您才能保證不會進行複寫。 因此，您應該盡可能部署遠端系統管理技術，以協助您在樹系復原期間關閉可寫入的 Dc 並實際加以隔離。

Rodc 可以在可寫入的 Dc 離線時繼續運作。 任何其他 DC 都不會直接從任何 RODC 複寫任何變更（特別是沒有架構或設定容器變更），因此它們在復原期間不會造成與可寫入 Dc 相同的風險。 在所有可寫入的 Dc 都復原並上線之後，您應該重建所有 Rodc。

Rodc 將繼續允許存取在個別網站中快取的本機資源，同時復原作業會以平行方式進行。 未在 RODC 上快取的本機資源會將驗證要求轉送至可寫入的 DC。 這些要求將會失敗，因為可寫入的 Dc 已離線。 在您復原可寫入的 Dc 之前，某些作業（例如密碼變更）也無法運作。

如果您使用中樞和輪輻網路架構，您可以先專注于復原中樞網站中的可寫入 Dc。 稍後，您可以在遠端網站重建 Rodc。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
