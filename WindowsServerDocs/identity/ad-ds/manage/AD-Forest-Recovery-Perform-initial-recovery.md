---
title: AD 樹系復原-執行初始復原
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: a369347fe889c7f6675d0091d05a6dee93cb4434
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369072"
---
# <a name="perform-initial-recovery"></a>執行初始復原  

>適用於：Windows Server 2016、Windows Server 2012 及 2012 R2、Windows Server 2008 和 2008 R2

本節包含下列步驟：  

- [還原每個網域中第一個可寫入的網域控制站](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [將每個已還原的可寫入網域控制站重新連線到網路](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [將通用類別目錄新增至樹系根域中的網域控制站](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>還原每個網域中第一個可寫入的網域控制站  

從樹系根域中可寫入的 DC 開始，完成本節中的步驟，以便還原第一個 DC。 樹系根域很重要，因為它會儲存 Schema Admins 和 Enterprise Admins 群組。 它也有助於維護樹系中的信任階層。 此外，樹系根域通常會保存樹系 DNS 命名空間的 DNS 根功能變數名稱伺服器。 因此，該網域的 Active Directory 整合 DNS 區域包含樹系中所有其他 Dc （複寫所需）和通用類別目錄 DNS 資源記錄的別名（CNAME）資源記錄。 
  
復原樹系根域之後，請重複相同的步驟來復原樹系中的其餘網域。 您可以同時復原一個以上的網域;不過，請一律先復原父系網域，再復原子系，以避免信任階層或 DNS 名稱解析中的任何中斷。 
  
針對您復原的每個網域，只從備份還原一個可寫入的 DC。 這是復原最重要的部分，因為 DC 必須有不會受到任何導致樹系失敗之影響的資料庫。 在將受信任的備份引進生產環境之前，請務必先進行徹底的測試。 
  
然後執行下列步驟。 執行特定步驟的程式位於[AD 樹系復原-程式](AD-Forest-Recovery-Procedures.md)中。 
  
1. 如果您打算還原實體伺服器，請確定目標 DC 的網路纜線未連接，因此未連接到生產網路。 若為虛擬機器，您可以移除網路介面卡，或使用連接到另一個網路的網路介面卡，您可以在其中測試復原程式，同時與生產網路隔離。 
  
2. 因為這是網域中第一個可寫入的 DC，所以您必須執行 AD DS 的非系統授權還原，以及 SYSVOL 的授權還原。 還原作業必須使用 Active Directory 感知備份和還原應用程式來完成，例如 Windows Server Backup （也就是，您不應該使用不支援的方法來還原 DC，例如還原 VM 快照集）。 
   - 需要有 SYSVOL 的授權還原，因為在您從嚴重損壞復原之後，必須啟動 SYSVOL 複寫資料夾的複寫。 在網域中新增的所有後續 Dc 都必須將其 SYSVOL 資料夾與已選取要授權的資料夾複本重新同步處理，才能通告資料夾。 

   > [!CAUTION]
   > 僅針對要在樹系根域中還原的第一個 DC，執行 SYSVOL 的授權（或主要）還原作業。 不正確地在其他 Dc 上執行 SYSVOL 的主要還原作業，會導致 SYSVOL 資料的複寫衝突。 

   - 有兩個選項可執行 AD DS 的非系統授權還原和 SYSVOL 的權威還原：  
   - 執行完整伺服器復原，然後強制執行 SYSVOL 的授權同步處理。 如需詳細程式，請參閱[執行完整伺服器](AD-Forest-Recovery-Perform-a-Full-Recovery.md)復原和[執行 DFSR 複寫 SYSVOL 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理。 
   - 執行完整伺服器復原，後面接著系統狀態還原。 此選項需要您事先建立兩種類型的備份：完整伺服器備份和系統狀態備份。 如需詳細程式，請參閱[執行完整伺服器](AD-Forest-Recovery-Perform-a-Full-Recovery.md)[復原和執行 Active Directory Domain Services 的非系統授權還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。 
  
3. 還原並重新啟動可寫入的 DC 之後，請確認失敗不會影響 DC 上的資料。 如果 DC 資料損毀，請使用不同的備份來重複步驟2。 
   - 如果還原的網域控制站裝載操作主機角色，您可能需要新增下列登錄專案，以避免在完成可寫入的目錄磁碟分割複寫之前，無法使用 AD DS：  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl 執行初始同步處理**  
  
      建立具有資料類型**REG_DWORD**且值為**0**的專案。 完整復原樹系之後，您可以將此專案的值重設為**1**，這需要重新開機並保留操作主機角色的網域控制站，才能成功地 AD DS 輸入和輸出複寫及其已知複本合作夥伴在將本身通告為網域控制站，並開始提供服務給用戶端之前， 如需初始同步處理需求的詳細資訊，請參閱知識庫文章[305476](https://support.microsoft.com/kb/305476)。 
  
      只有在您還原和驗證資料之後，以及將這部電腦加入生產網路之前，才繼續進行下一個步驟。 
  
4. 如果您懷疑整個樹系失敗與網路入侵或惡意攻擊有關，請重設所有系統管理帳戶的帳戶密碼，包括 Enterprise Admins、Domain Admins、Schema Admins、Server Operators、Account 的成員操作員群組等等。 在樹系復原的下一個階段安裝額外的網域控制站之前，應該先完成系統管理帳戶密碼的重設。 
5. 在樹系根域中第一個還原的 DC 上，捕獲所有全網域和全樹系操作主機角色。 需要 Enterprise Admins 和 Schema Admins 認證，才能抓取全樹系操作主機角色。 
  
     在每個子域中，會佔用全網域操作主機角色。 雖然您可能只會暫時在還原的 DC 上保留操作主機角色，但佔用這些角色可確保您在樹系復原程式中的哪個 DC 上裝載它們。 作為復原後程式的一部分，您可以視需要轉散發操作主機角色。 如需有關取取操作主機角色的詳細資訊，請參閱[佔用操作主機角色](AD-forest-recovery-seizing-operations-master-role.md)。 如需有關放置操作主機角色之位置的建議，請參閱[什麼是操作主機？](https://technet.microsoft.com/library/cc779716.aspx)。 
  
6. 清除樹系根域中所有其他可寫入 Dc 的中繼資料，而不是從備份進行還原（網域中所有可寫入的 Dc，但此第一個 DC 除外）。 如果您使用 Windows Server 2008 或更新版本隨附的 Active Directory 使用者和電腦或 Active Directory 網站和服務版本，或是 Windows Vista 或更新版本的 RSAT，則當您刪除 DC 物件時，將會自動執行中繼資料清除。 此外，已刪除之 DC 的伺服器物件和電腦物件也會自動刪除。 如需詳細資訊，請參閱[清除已移除可寫入 dc 的中繼資料](AD-Forest-Recovery-Cleaning-Metadata.md)。 
  
     清除中繼資料可避免在不同網站的 DC 上安裝 AD DS 時，可能會重複的 NTDS 設定物件。 這可能也會在 Dc 本身不存在時，儲存知識一致性檢查程式（KCC）建立複寫連結的程式。 此外，在中繼資料清除的過程中，網域中所有其他 Dc 的 DC 定位程式 DNS 資源記錄將會從 DNS 中刪除。 
  
     在移除網域中所有其他 Dc 的中繼資料之前，如果此 DC 在復原之前是 RID 主機，將不會採用 RID 主機角色，因此無法發行新的 Rid。 您可能會在指出此失敗的系統記錄檔事件檢視器中看到事件識別碼16650，但在您清除中繼資料之後，您應該會看到事件識別碼16648指出成功的一小段時間。 
  
7. 如果您的 DNS 區域儲存在 AD DS 中，請確定已在您還原的 DC 上安裝並執行本機 DNS 伺服器服務。 如果此 DC 不是樹系失敗前的 DNS 伺服器，您必須安裝和設定 DNS 伺服器。 
  
    > [!NOTE]
    > 如果還原的 DC 執行 Windows Server 2008，您必須在 KB 文章[975654](https://support.microsoft.com/kb/975654)中安裝此修補程式，或暫時將伺服器連接到隔離的網路，才能安裝 DNS 伺服器。 任何其他版本的 Windows Server 都不需要此修補程式。 
  
     在樹系根域中，使用自己的 IP 位址（或回送位址，例如127.0.0.1）來設定還原的 DC，作為其慣用的 DNS 伺服器。 您可以在區域網路（LAN）介面卡的 TCP/IP 屬性中進行這項設定。 這是樹系中的第一部 DNS 伺服器。 如需詳細資訊，請參閱[CONFIGURE tcp/ip to USE DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在每個子域中，將還原的 DC 設定為樹系根域中第一部 DNS 伺服器的 IP 位址，作為其慣用的 DNS 伺服器。 您可以在 LAN 介面卡的 TCP/IP 屬性中進行這項設定。 如需詳細資訊，請參閱[CONFIGURE tcp/ip to USE DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在 _msdcs 和網域 DNS 區域中，刪除在中繼資料清除後已不存在之 Dc 的 NS 記錄。 檢查已清除之 Dc 的 SRV 記錄是否已移除。 若要協助加速 DNS SRV 記錄移除，請執行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. 將可用的 RID 集區值提高100000。 如需詳細資訊，請參閱[提高可用 RID 集區的值](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果您有理由相信以100000引發 RID 集區不足以因應您的特定情況，您應該判斷仍然可以安全使用的最低增加。 Rid 是一種不應不必要地使用的有限資源。 
  
     如果在您用於還原的備份時間之後，已在網域中建立新的安全性主體，這些安全性主體可能會擁有特定物件的存取權限。 這些安全性主體已在復原後不再存在，因為復原已還原至備份;不過，他們的存取權限可能仍然存在。 如果在還原後未引發可用的 RID 集區，則在樹系復原之後建立的新使用者物件可能會取得相同的安全識別碼（Sid），而且可以存取原本不是預期的物件。 
  
     為了說明，請考慮簡介中所提及名為張瑾的新員工範例。 「張瑾雯」的使用者物件不再存在於還原作業之後，因為它是在用來還原網域的備份之後所建立。 不過，指派給該使用者物件的任何存取權限可能會在還原作業之後保存。 如果在還原作業之後，將該使用者物件的 SID 重新指派給新的物件，新的物件就會取得這些存取權限。 
  
9. 使目前的 RID 集區失效。 目前的 RID 集區在系統狀態還原後失效。 但是，如果未執行系統狀態還原，目前的 RID 集區就必須失效，以防止還原的 DC 從建立備份時指派的 RID 集區重新發行 Rid。 如需詳細資訊，請參閱[使目前的 RID 集區失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)。 
  
    > [!NOTE]
    > 當您第一次嘗試在讓 RID 集區失效之後建立具有 SID 的物件時，將會收到錯誤。 嘗試建立物件會觸發新 RID 集區的要求。 因為將配置新的 RID 集區，所以操作的重試成功。 
  
10. 請重設此 DC 的電腦帳戶密碼兩次。 如需詳細資訊，請參閱[重設網域控制站的電腦帳戶密碼](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。 
  
11. 重設 krbtgt 密碼兩次。 如需詳細資訊，請參閱[重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。 
  
     因為 krbtgt 密碼歷程記錄是兩個密碼，請重設密碼兩次，以移除密碼歷程記錄中的原始（prefailure）密碼。 
  
    > [!NOTE]
    > 如果樹系復原是為了回應安全性缺口，您也可以重設信任密碼。 如需詳細資訊，請參閱在[信任的一端重設信任密碼](AD-Forest-Recovery-Reset-Trust.md)。 
  
12. 如果樹系有多個網域，而還原的 DC 在失敗前是通用類別目錄伺服器，請清除 [NTDS 設定] 屬性中的 [**通用類別目錄**] 核取方塊，以移除 DC 中的通用類別目錄。 此規則的例外狀況是只有一個網域的樹系的常見案例。 在此情況下，不需要移除通用類別目錄。 如需詳細資訊，請參閱[移除通用類別目錄](AD-Forest-Recovery-Remove-GC.md)。 
  
     藉由從備份中還原的通用類別目錄，比其他在其他網域中用來還原 Dc 的備份還要新，您可能會引進延遲物件。 請考慮下列範例。 在網域 A 中，DC1 是從時間 T1 取得的備份進行還原。 在網域 B 中，DC2 是從時間 T2 所取得的通用類別目錄備份進行還原。 假設 T2 比 T1 新，而且有些物件是在 T1 和 T2 之間建立的。 在還原這些 Dc 之後，DC2 就是通用類別目錄，它會保存網域 A 的部分複本的較新資料，而不是網域 A 本身的保存。 在此情況下，DC2 會保留延遲物件，因為 DC1 上不會有這些物件。 
  
     延遲物件的存在可能會導致問題。 例如，電子郵件訊息可能無法傳遞給使用者，其使用者物件已在網域之間移動。 讓過期的 DC 或通用類別目錄伺服器恢復上線之後，使用者物件的兩個實例都會出現在通用類別目錄中。 這兩個物件都有相同的電子郵件地址;因此，無法傳遞電子郵件訊息。 
  
     第二個問題是，不再存在的使用者帳戶可能仍會出現在全域通訊清單中。 第三個問題是，不再存在的萬用群組可能仍會出現在使用者的存取權杖中。 
  
     如果您還原的是通用類別目錄的 DC （不小心或因為這是您信任的單獨備份），建議您在還原作業之後立即停用通用類別目錄，以避免出現延遲物件的情況。套. 停用通用類別目錄旗標會導致電腦遺失其所有部分複本（分割區），並 relegating 到正常的 DC 狀態。 
  
13. 設定 Windows Time 服務。 在樹系根域中，將 PDC 模擬器設定為從外部時間來源同步處理時間。 如需詳細資訊，請參閱在[樹系根域中的 PDC 模擬器上設定 Windows 時間服務](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>將每個已還原的可寫入網域控制站重新連接到通用網路

在這個階段中，您應該在樹系根域和每個其餘的網域中還原一個 DC （以及執行的復原步驟）。 將這些 Dc 加入與環境的其餘部分隔離的通用網路，並完成下列步驟，以驗證樹系健全狀況和複寫。

> [!NOTE]
> 當您將實體 Dc 加入隔離的網路時，您可能需要變更其 IP 位址。 因此，DNS 記錄的 IP 位址會錯誤。 因為無法使用通用類別目錄伺服器，所以 DNS 的安全動態更新將會失敗。 在此情況下，虛擬 Dc 較有利，因為它們可以聯結至新的虛擬網路，而不需要變更其 IP 位址。 這是為什麼將虛擬 Dc 建議為在樹系復原期間還原的第一個網域控制站的其中一個原因。 
  
驗證之後，請將 Dc 加入生產網路，並完成步驟以確認樹系複寫健全狀況。

- 若要修正名稱解析，請建立 DNS 委派記錄，並視需要設定 DNS 轉送和根提示。 執行**repadmin/replsum**以檢查 dc 之間的複寫。 
- 如果還原的 DC 不是直接複寫協力電腦，則在兩者之間建立暫存連線物件，複寫復原的速度會更快。 
- 若要驗證中繼資料清除，請執行**Repadmin/viewlist \\** *，以取得樹系中所有 dc 的清單。 執行**Nltest/DCList：** *< 網域\>*  ，以取得網域中所有 dc 的清單。 
- 若要檢查 DC 和 DNS 健全狀況，請執行 DCDiag/v 以報告樹系中所有 Dc 的錯誤。 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>將通用類別目錄新增至樹系根域中的網域控制站

基於下列原因，需要通用類別目錄：  
  
- 為使用者啟用登入。 
- 若要啟用在每個子網域的 Dc 上執行的 Net Logon 服務，以在根域的 DNS 伺服器上註冊和移除記錄。 
  
雖然樹系根 DC 偏好成為通用類別目錄，但您可以選擇任何已還原的 Dc 成為通用類別目錄。 
  
> [!NOTE]
> DC 不會通告為通用類別目錄伺服器，直到它完成樹系中所有目錄分割的完整同步處理為止。 因此，DC 應強制與樹系中每個已還原的 Dc 進行複寫。 
>
> 監視事件識別碼1119事件檢視器中的目錄服務事件記錄檔，這表示此 DC 是通用類別目錄伺服器，或確認下列登錄機碼的值為1：  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global 類別目錄升級完成**  
  
如需詳細資訊，請參閱[新增通用類別目錄](AD-Forest-Recovery-Add-GC.md)。 
  
在這個階段，您應該有一個穩定的樹系，其中每個網域都有一個 DC，而樹系中有一個通用類別目錄。 您應該為您剛還原的每個 Dc 建立新的備份。 您現在可以藉由安裝 AD DS，開始重新部署樹系中的其他 Dc。 

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
