---
title: AD 樹系修復-決定如何復原樹系
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fbb1f0f0f1b21c626f344bb01b793211586c7cf3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953970"
---
# <a name="determine-how-to-recover-the-forest"></a>決定如何復原樹系

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

復原整個 Active Directory 樹系牽涉到在樹系中的每個網域控制站（DC）上，從備份或重新安裝 Active Directory Domain Services （AD DS）還原它。 復原樹系會將樹系中的每個網域還原到上次信任備份時的狀態。 因此，還原作業會導致至少遺失下列 Active Directory 資料：

- 上次受信任備份之後新增的所有物件（例如使用者和電腦）
- 自上次受信任備份之後，對現有物件所做的所有更新
- 自上次信任備份後，對 AD DS 中的設定分割區或架構分割區進行的所有變更（例如架構變更）

對於樹系中的每個網域，必須知道網域系統管理員帳戶的密碼。 最好的是內建系統管理員帳戶的密碼。 您也必須知道 DSRM 密碼，才能執行 DC 的系統狀態還原。 一般來說，將系統管理員帳戶和 DSRM 密碼歷程記錄封存在安全的位置，只要備份有效，也就是在標記存留期期間內，或在已刪除的物件存留期期間內（如果已啟用 Active Directory 回收站），這是很好的作法。 您也可以同步處理 DSRM 密碼與網域使用者帳戶，以便更容易記住。 如需詳細資訊，請參閱知識庫文章[961320](https://support.microsoft.com/kb/961320)。 同步處理 DSRM 帳戶必須在樹系復原之前完成，做為準備工作的一部分。

> [!NOTE]
> 系統管理員帳戶預設是內建系統管理員群組的成員，如同 Domain Admins 和 Enterprise Admins 群組。 此群組具有網域中所有 Dc 的完全控制權。

## <a name="determining-which-backups-to-use"></a>判斷要使用的備份

請定期備份每個網域的至少兩個可寫入 Dc，讓您有數個備份可供選擇。 請注意，您無法使用唯讀網域控制站（RODC）的備份來還原可寫入的 DC。 我們建議您在失敗發生之前的幾天內，使用備份來還原 Dc。 一般來說，您必須決定已還原資料的 recentness 與 safeness 之間的取捨。 選擇最新備份可還原更有用的資料，但可能會增加重新將危險資料引進還原之樹系的風險。

還原系統狀態備份取決於原始的作業系統和備份伺服器。 例如，您不應該將系統狀態備份還原到不同的伺服器。 在此情況下，您可能會看到下列警告：

「指定的備份與目前的伺服器不同。 我們不建議使用備份來執行系統狀態復原，因為伺服器可能會變得無法使用。 您確定要使用此備份來復原目前的伺服器嗎？」

如果您需要將 Active Directory 還原到不同的硬體，請建立完整伺服器備份，並規劃執行完整伺服器復原。

> [!IMPORTANT]
> 從 Windows Server 2008 開始，不支援將系統狀態備份還原至新硬體或相同硬體上的新 Windows Server 安裝。 如果在同一個硬體上重新安裝 Windows Server，如本指南稍後所建議，您可以依照下列順序還原網域控制站：
>
> 1. 執行完整伺服器還原，以便還原作業系統和所有檔案和應用程式。
> 2. 使用 wbadmin.exe 執行系統狀態還原，以便將 SYSVOL 標示為授權。
>
> 如需詳細資訊，請參閱 Microsoft 知識庫文章[249694](https://support.microsoft.com/kb/249694)。

如果發生失敗的時間不明，請進一步調查，以找出保留樹系最後安全狀態的備份。 這種方法比較不理想。 因此，我們強烈建議您每日保留有關 AD DS 健全狀況狀態的詳細記錄，如此一來，如果發生全樹系失敗，則可以識別大約失敗的時間。 您也應該保留備份的本機複本，以加快復原的速度。

如果已啟用 Active Directory 回收站，則備份存留期會等於**msds-deletedobjectlifetime**值或**tombstoneLifetime**值（取兩者中較小者）。 如需詳細資訊，請參閱[Active Directory 回收站逐步指南](https://go.microsoft.com/fwlink/?LinkId=178657)（） https://go.microsoft.com/fwlink/?LinkId=178657) 。

或者，您也可以使用 Active Directory 資料庫掛接工具（Dsamain.exe）和輕量型目錄存取協定（LDAP）工具（例如 Ldp.exe 或 Active Directory 使用者和電腦）來識別哪個備份具有樹系的最後安全狀態。 Windows Server 2008 和更新版本的 Windows Server 作業系統中包含的 Active Directory 資料庫掛接工具，會將儲存在備份或快照中的 Active Directory 資料公開為 LDAP 伺服器。 然後，您可以使用 LDAP 工具來流覽資料。 這種方法的優點是不需要您重新開機任何目錄服務還原模式（DSRM）中的 DC 來檢查 AD DS 備份的內容。

如需有關使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱[Active Directory 資料庫掛接工具逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10))。

您也可以使用**ntdsutil snapshot**命令來建立 Active Directory 資料庫的快照集。 藉由排程工作定期建立快照集，您可以在一段時間內取得 Active Directory 資料庫的其他複本。 您可以使用這些複本來更清楚地識別整個樹系失敗發生的時間，然後選擇要還原的最佳備份。 若要建立快照集，請使用 Windows Server 2008 隨附的**ntdsutil**版本，或適用于 windows Vista 或更新版本的遠端伺服器管理工具（RSAT）。 目標 DC 可以執行任何版本的 Windows Server。 如需使用**ntdsutil snapshot**命令的詳細資訊，請參閱[快照](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10))集。

## <a name="determining-which-domain-controllers-to-restore"></a>判斷要還原的網域控制站

在決定要還原的網域控制站時，簡化還原程式是一個很重要的因素。 建議將每個網域的專用 DC 用於還原的慣用 DC。 專用的還原 DC 可讓您更容易可靠地規劃和執行樹系復原，因為您使用的是用來執行還原測試的相同來源設定。 您可以編寫復原的腳本，而不會爭用不同的設定，例如 DC 是否保留操作主機角色，或是否為 GC 或 DNS 伺服器。

> [!NOTE]
> 雖然不建議您在簡單的情況下還原操作主機角色持有者，但某些組織可能會選擇還原一個，以提供其他優點。 例如，還原 RID 主機可能有助於防止在復原期間管理 Rid 的問題。  

選擇最符合下列準則的 DC：

- 可寫入的 DC。 這是必要項目。

- 執行 Windows Server 2012 作為虛擬機器的 DC，在支援 VM GenerationID 的管理程式上。 此 DC 可用來做為複製的來源。
- 可以在實體或虛擬網路上存取，且最好位於資料中心的 DC。 如此一來，您就可以在樹系復原期間，輕鬆地將它與網路隔離。
- 具有良好完整伺服器備份的 DC。 良好的備份是可以成功還原的備份，在失敗前的幾天內會執行，並盡可能包含最多有用的資料。
- 在失敗前做為網域名稱系統（DNS）伺服器的 DC。 這可節省重新安裝 DNS 所需的時間。
- 如果您也使用 Windows 部署服務，請選擇未設定為使用 BitLocker 網路解除鎖定的 DC。 在此情況下，不支援將 BitLocker 網路解除鎖定用於您在樹系復原期間從備份還原的第一個 DC。

   BitLocker 網路解除鎖定作為*唯一*的金鑰保護裝置，*無法*在您已部署 Windows 部署服務（WDS）的 dc 上使用，因為這樣做會導致第一個 DC 需要 Active Directory 和 WDS 才能解除鎖定的情況。 但在還原第一個 DC 之前，Active Directory 尚未提供 WDS 使用，因此無法解除鎖定。

   若要判斷 DC 是否設定為使用 BitLocker 網路解除鎖定，請檢查下列登錄機碼中是否已識別出網路解除鎖定憑證：

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

維護安全性程式，以處理或還原包含 Active Directory 的備份檔案。 隨附樹系復原的緊急程度可能會不小心導致忽略的安全性最佳作法。 如需詳細資訊，請參閱[保護 Active Directory 安裝和日常作業的最佳作法指南](/previous-versions/windows/it-pro/windows-2000-server/bb727066(v=technet.10))中的「建立網域控制站備份和還原策略」一節：第二部分。

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>識別目前的樹系結構和 DC 函數

藉由識別樹系中的所有網域來判斷目前的樹系結構。 建立每個網域中所有 Dc 的清單，特別是具有備份的 Dc，以及可以做為複製來源的虛擬 Dc。 樹系根域的 Dc 清單會是最重要的，因為您會先復原此網域。 還原樹系根域之後，您可以使用 Active Directory 嵌入式管理單元，取得樹系中其他網域、Dc 和網站的清單。

準備一個資料表，以顯示網域中每個 DC 的功能，如下列範例所示。 這可協助您在復原之後還原為樹系的預先失敗設定。

|DC 名稱|作業系統|FSMO|GC|RODC|備份|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|架構主機、網域命名主機|是|否|是|否|否|是|是|  
|DC_2|Windows Server 2012|無|是|否|是|是|否|是|是|  
|DC_3|Windows Server 2012|基礎結構主機|否|否|否|是|是|是|是|  
|DC_4|Windows Server 2012|PDC 模擬器，RID 主機|是|否|否|否|否|是|否|  
|DC_5|Windows Server 2012|無|否|否|是|是|否|是|是|  
|RODC_1|Windows Server 2008 R2|無|是|是|是|是|是|是|否|  
|RODC_2|Windows Server 2008|無|是|是|否|是|是|是|否|  

針對樹系中的每個網域，識別具有該網域之 Active Directory 資料庫之受信任備份的單一可寫入 DC。 當您選擇要還原 DC 的備份時，請務必小心。 如果發生失敗的日期和原因大約是已知的，一般建議是使用在該日期之前的幾天進行的備份。
  
在此範例中，有四個備份候選項目： DC_1、DC_2、DC_4 和 DC_5。 在這些備份候選項目中，您只會還原一個。 建議的 DC 會因為下列原因而 DC_5：  

- 它滿足使用它做為虛擬化 DC 複製來源的需求，也就是，它會在支援 VM GenerationID 的管理程式上執行 Windows Server 2012 做為虛擬 DC，執行允許複製的軟體（如果無法複製，則可以移除）。 還原之後，PDC 模擬器角色將會被取回至該伺服器，並可新增至該網域的 Cloneable 網域控制站群組。  
- 它會執行 Windows Server 2012 的完整安裝。 執行 Server Core 安裝的 DC 可能比較不方便做為復原目標。  
- 這是一台 DNS 伺服器。 因此，不需要重新安裝 DNS。  

> [!NOTE]
> 因為 DC_5 不是通用類別目錄伺服器，所以也有一項優點，那就是不需要在還原之後移除通用類別目錄。 但是，不論 DC 是否也是通用類別目錄伺服器，都不是決定性因素，因為從 Windows Server 2012 開始，所有 Dc 預設都是通用類別目錄伺服器，在任何情況下，建議您在進行還原之後移除通用類別目錄並將其新增為樹系復原程式的一部分。  

## <a name="recover-the-forest-in-isolation"></a>獨立復原樹系

慣用的案例是在第一個還原的 DC 重新進入生產環境之前，關閉所有可寫入的 dc。 這可確保任何危險的資料都不會複寫回已復原的樹系。 關閉所有操作主機角色持有者是特別重要的。  

> [!NOTE]
> 在某些情況下，您可能會將您打算針對每個網域復原的第一個 DC 移至隔離的網路，同時允許其他 Dc 保持連線，以將系統停機時間降到最低。 例如，如果您要從失敗的架構升級復原，您可以選擇讓網域控制站在生產網路上執行，而在隔離的情況下執行修復步驟。

如果您執行的是虛擬網域控制站，您可以將它們移至虛擬網路，而該虛擬網路會與您要執行復原的生產網路隔離。 將虛擬化的 Dc 移至不同的網路提供兩個優點：  

- 復原的 Dc 無法重複發生導致樹系修復的問題，因為它們已隔離。  
- 虛擬化的 DC 複製可以在不同的網路上執行，如此一來，很重要的 Dc 就可以在恢復到生產網路之前，執行和測試。

如果您在實體硬體上執行 Dc，請中斷您打算在樹系根域中還原之第一個 DC 的網路纜線。 可能的話，也請中斷所有其他 Dc 的網路纜線。 這可防止 Dc 複寫（如果它們在樹系復原程式中意外啟動）。  

在分散到多個位置的大型樹系中，很難以保證所有可寫入的 Dc 都已關閉。 基於這個理由，復原步驟（例如，重設電腦帳戶和 krbtgt 帳戶，以及中繼資料清除除外）的設計是為了確保復原的可寫入 Dc 不會使用危險的可寫入 Dc 來複寫（萬一樹系中的某些仍在線上）。  
  
不過，只有透過讓可寫入的 Dc 離線，您才能保證不會進行複寫。 因此，您應該盡可能部署遠端系統管理技術，協助您在樹系復原期間關閉並實際隔離可寫入的 Dc。  
  
當可寫入的 Dc 離線時，Rodc 可以繼續運作。 其他 DC 則不會直接從任何 RODC 複寫任何變更（尤其是不會變更任何架構或設定容器），因此它們在復原期間不會造成與可寫入 Dc 相同的風險。 在所有可寫入的 Dc 都復原並上線之後，您應該重建所有 Rodc。  
  
Rodc 會繼續允許存取在其個別網站中快取的本機資源，同時復原作業會以平行方式進行。 未在 RODC 上快取的本機資源，會將驗證要求轉送至可寫入的 DC。 這些要求將會失敗，因為可寫入的 Dc 已離線。 在您復原可寫入的 Dc 之前，某些作業（例如密碼變更）也無法運作。  
  
如果您使用中樞和輪輻網路架構，您可以先專注于復原中樞網站中的可寫入 Dc。 稍後，您可以在遠端網站中重建 Rodc。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
