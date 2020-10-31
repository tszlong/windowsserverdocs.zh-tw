---
title: AD 樹系復原-執行初始復原
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 8b0498b30966c22ec8dca267988e109d976f6b69
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067740"
---
# <a name="perform-initial-recovery"></a>執行初始復原

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本節包含下列步驟：

- [還原每個網域中第一個可寫入的網域控制站](#restore-the-first-writeable-domain-controller-in-each-domain)
- [將每個已還原的可寫入網域控制站重新連接到網路](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)
- [將通用類別目錄新增至樹系根域中的網域控制站](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>還原每個網域中第一個可寫入的網域控制站

從樹系根域中的可寫入 DC 開始，請完成本節中的步驟，以便還原第一個 DC。 樹系根域很重要，因為它會儲存 Schema Admins 和 Enterprise Admins 群組。 它也有助於維護樹系中的信任階層。 此外，樹系根域通常會保留樹系 DNS 命名空間的 DNS 根功能變數名稱伺服器。 因此，該網域的 Active Directory 整合式 DNS 區域包含樹系中所有其他網域控制站的別名 (CNAME) 資源記錄 (必須有複寫) 和通用類別目錄 DNS 資源記錄。

復原樹系根域之後，請重複相同的步驟來復原樹系中的其餘網域。 您可以同時復原一個以上的網域;不過，請一律先復原父系網域，再復原子系，以防止信任階層或 DNS 名稱解析中出現任何中斷。

針對您復原的每個網域，只從備份還原一個可寫入的 DC。 這是復原中最重要的部分，因為 DC 必須有資料庫不受影響，而導致樹系失敗。 在將受信任的備份引進生產環境之前，請務必先進行徹底測試。

然後執行下列步驟。 執行特定步驟的程式位於 [AD 樹系修復](AD-Forest-Recovery-Procedures.md)程式中。

1. 如果您打算還原實體伺服器，請確定目標 DC 的網路纜線並未連接，因此未連線至生產網路。 針對虛擬機器，您可以移除網路介面卡，或使用連接到另一個網路的網路介面卡，以在與生產網路隔離時測試復原程式。

2. 因為這是網域中第一個可寫入的 DC，所以您必須執行 AD DS 的非系統授權還原以及 SYSVOL 的授權還原。 還原作業必須使用 Active Directory 感知的備份和還原應用程式來完成，例如 Windows Server Backup (也就是，您不應該使用不支援的方法（例如還原 VM 快照) ）來還原 DC。
   - 需要 SYSVOL 的授權還原，因為從損壞復原之後，必須啟動 SYSVOL 複寫資料夾的複寫。 在網域中新增的所有後續 Dc 都必須重新同步處理其 SYSVOL 資料夾，以及在可以通告資料夾之前已選取要授權的資料夾複本。

   > [!CAUTION]
   > 只針對樹系根域中要還原的第一個 DC，執行權威 (或主要) 還原作業。 錯誤地在其他 Dc 上執行 SYSVOL 的主要還原作業會導致 SYSVOL 資料的複寫衝突。

   - 有兩個選項可執行 AD DS 的非系統授權還原和 SYSVOL 的授權還原：
   - 執行完整伺服器復原，然後強制執行 SYSVOL 的授權同步處理。 如需詳細程式，請參閱 [執行完整伺服器](AD-Forest-Recovery-Perform-a-Full-Recovery.md) 復原，以及 [執行 DFSR 複寫 SYSVOL 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理。
   - 執行完整伺服器復原，然後執行系統狀態還原。 此選項會要求您事先建立這兩種類型的備份：完整的伺服器備份和系統狀態備份。 如需詳細程式，請參閱 [執行完整伺服器](AD-Forest-Recovery-Perform-a-Full-Recovery.md) 復原，以及 [執行 Active Directory Domain Services 的非系統授權還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。

3. 在您還原並重新啟動可寫入的 DC 之後，請確認失敗不會影響 DC 上的資料。 如果 DC 資料已損毀，請使用不同的備份重複步驟2。
   - 如果還原的網域控制站裝載操作主機角色，您可能需要新增下列登錄專案，以避免 AD DS 無法使用，直到它完成可寫入目錄分割的複寫為止：

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl 執行初始同步處理**

      使用資料類型 **REG_DWORD** 建立專案，並以 **0** 為值建立專案。 完整復原樹系之後，您可以將此專案的值重設為 **1** ，這需要一個網域控制站來重新開機並保存操作主機角色，才能在將本身宣告為網域控制站，並開始為用戶端提供服務時，成功 AD DS 輸入和輸出複寫與其已知的複本夥伴。 如需有關初始同步處理需求的詳細資訊，請參閱知識庫文章 [305476](https://support.microsoft.com/kb/305476)。

      請在您還原和驗證資料之後，以及將這部電腦加入生產網路之前，繼續進行下一個步驟。

4. 如果您懷疑整個樹系的失敗與網路入侵或惡意攻擊有關，請重設所有系統管理帳戶的帳戶密碼，包括 Enterprise Admins、Domain Admins、Schema Admins、Server Operators、Account Operators 群組等的成員。 在樹系復原的下一個階段中安裝其他網域控制站之前，應先完成系統管理帳戶密碼的重設。
5. 在樹系根域中第一次還原的 DC 上，會佔用所有全網域和全樹系的操作主機角色。 需要 Enterprise Admins 和 Schema Admins 認證，才能奪取整個樹系的操作主機角色。

     在每個子域中，都會佔用整個網域的操作主機角色。 雖然您可能只會暫時在還原的 DC 上保留操作主機角色，但請在樹系復原程式中，讓這些角色可確保您在這個階段會有哪些 DC 裝載這些角色。 在復原後的流程中，您可以視需要轉散發操作主機角色。 如需有關取得操作主機角色的詳細資訊，請參閱 [佔用操作主機角色](AD-forest-recovery-seizing-operations-master-role.md)。 如需有關如何放置操作主機角色的建議，請參閱 [何謂操作主機？](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10))。

6. 清除樹系根域中所有其他可寫入 Dc 的中繼資料，而不是從備份還原 (網域中的所有可寫入 dc （除了第一個 DC) 以外）。 如果您使用 Windows Server 2008 或更新版本中所含的 Active Directory 消費者和電腦或 Active Directory 網站與服務，或是 Windows Vista 或更新版本的 RSAT，則在刪除 DC 物件時，會自動執行中繼資料清除。 此外，也會自動刪除已刪除 DC 的 [伺服器物件] 和 [電腦] 物件。 如需詳細資訊，請參閱 [清理已移除可寫入 dc 的中繼資料](AD-Forest-Recovery-Cleaning-Metadata.md)。

     如果 AD DS 安裝在不同網站的 DC 上，清除中繼資料可避免可能重複的 NTDS 設定物件。 這可能也會將知識一致性檢查程式儲存 (KCC) 當 Dc 本身可能不存在時建立複寫連結的程式。 此外，作為中繼資料清除的一部分，網域中所有其他 Dc 的 DC 定位器 DNS 資源記錄都會從 DNS 刪除。

     除非移除網域中所有其他網域控制站的中繼資料，否則此 DC （如果它是復原之前的 RID 主機）將不會採用 RID 主機角色，因此將無法發行新的 Rid。 您可能會在 [系統記錄檔] 中看到事件檢視器指出此失敗的事件識別碼16650，但在清除中繼資料之後，您應該會看到事件識別碼16648表示成功。

7. 如果您有儲存在 AD DS 中的 DNS 區域，請確定已在您還原的 DC 上安裝並執行本機 DNS 伺服器服務。 如果此 DC 不是樹系失敗之前的 DNS 伺服器，您必須安裝並設定 DNS 伺服器。

    > [!NOTE]
    > 如果還原的 DC 執行 Windows Server 2008，您需要在 KB 文章 [975654](https://support.microsoft.com/kb/975654) 中安裝該修正程式，或暫時將伺服器連接到隔離的網路，以便安裝 DNS 伺服器。 任何其他版本的 Windows Server 都不需要此修正程式。

     在樹系根域中，使用自己的 IP 位址 (或回送位址（例如 127.0.0.1) ）來設定還原的 DC，作為其慣用的 DNS 伺服器。 您可以在區域網路 (LAN) 介面卡的 TCP/IP 內容中設定此設定。 這是樹系中的第一部 DNS 伺服器。 如需詳細資訊，請參閱 [設定 tcp/ip 以使用 DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10))。

     在每個子域中，以樹系根域中第一部 DNS 伺服器的 IP 位址做為慣用的 DNS 伺服器，設定還原的 DC。 您可以在 LAN 介面卡的 TCP/IP 內容中設定此設定。 如需詳細資訊，請參閱 [設定 tcp/ip 以使用 DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10))。

     在 _msdcs 和網域 DNS 區域中，刪除中繼資料清除之後不再存在之 Dc 的 NS 記錄。 檢查已清除之 Dc 的 SRV 記錄是否已移除。 若要協助加速移除 DNS SRV 記錄，請執行：

    ```
    nltest.exe /dsderegdns:server.domain.tld
    ```

8. 依100000提高可用 RID 集區的值。 如需詳細資訊，請參閱 [提高可用 RID 集區的值](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果您有理由認為，100000所引發的 RID 集區對您的特定情況而言不足，您應該決定仍可安全使用的最低增加。 Rid 是一種有限的資源，不會不必要地使用。

     如果在您用於還原的備份之後，在網域中建立新的安全性主體，則這些安全性主體可能會有特定物件的存取權限。 復原後，這些安全性主體已不再存在，因為復原已還原至備份;不過，其存取權限可能仍然存在。 如果在還原後未產生可用的 RID 集區，則在樹系復原之後建立的新使用者物件，可能會 (Sid) 取得相同的安全識別碼，而且可能會存取這些物件，但原本並不適用。

     為了說明，請考慮簡介中所提及名為 Amy 的新員工範例。 在還原作業之後，您的 Amy 的使用者物件已不再存在，因為它是在用來還原網域的備份之後所建立。 不過，在還原作業之後，指派給該使用者物件的任何存取權限可能會保存下來。 如果在還原作業之後，將該使用者物件的 SID 重新指派給新的物件，新的物件就會取得這些存取權限。

9. 使目前的 RID 集區無效。 目前的 RID 集區在系統狀態還原之後會失效。 但是，如果未執行系統狀態還原，則目前的 RID 集區必須失效，以防止還原的 DC 從建立備份時所指派的 RID 集區重新發出 Rid。 如需詳細資訊，請參閱 [使目前的 RID 集區失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)。

    > [!NOTE]
    > 當您在將 RID 集區失效之後，第一次嘗試使用 SID 建立物件時，您將會收到錯誤訊息。 嘗試建立物件會觸發新 RID 集區的要求。 因為將配置新的 RID 集區，所以作業的重試成功。

10. 重設此 DC 的電腦帳戶密碼兩次。 如需詳細資訊，請參閱 [重設網域控制站的電腦帳戶密碼](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。

11. 重設 krbtgt 密碼兩次。 如需詳細資訊，請參閱 [重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。

     因為 krbtgt 密碼歷程記錄是兩個密碼，所以重設密碼兩次，以從密碼歷程記錄中移除原始 (prefailure) 密碼。

    > [!NOTE]
    > 如果樹系復原是為了回應安全性缺口，您也可以重設信任密碼。 如需詳細資訊，請參閱在 [信任的一端重設信任密碼](AD-Forest-Recovery-Reset-Trust.md)。

12. 如果樹系有多個網域，而還原的 DC 在失敗之前是通用類別目錄伺服器，請清除 [NTDS 設定] 屬性中的 [ **通用類別目錄** ] 核取方塊，以從 DC 移除通用類別目錄。 這項規則的例外狀況是僅具有一個網域的樹系的常見案例。 在此情況下，不需要移除通用類別目錄。 如需詳細資訊，請參閱 [移除通用類別目錄](AD-Forest-Recovery-Remove-GC.md)。

     藉由從備份中還原的通用類別目錄，比其他用來還原其他網域中 Dc 的備份還要新，您可能會引進延遲物件。 請思考一下下列範例。 在網域 A 中，DC1 是從在 T1 時所取得的備份還原。 在 B 網域中，DC2 是從在時間 T2 取得的通用類別目錄備份還原。 假設 T2 的最新數目為 T1，而某些物件是在 T1 和 T2 之間建立。 在還原這些 Dc 之後，DC2 （即通用類別目錄）會保留網域 A 的部分複本的較新資料，而非網域 A 本身的複本。 在此情況下，DC2 會保存延遲物件，因為這些物件不會出現在 DC1 上。

     延遲物件的存在可能會導致問題。 比方說，電子郵件訊息可能無法傳遞給使用者物件在網域之間移動的使用者。 當您讓過期的 DC 或通用類別目錄伺服器重新上線之後，使用者物件的兩個實例都會出現在通用類別目錄中。 這兩個物件都有相同的電子郵件地址;因此，無法傳遞電子郵件訊息。

     第二個問題是，不再存在的使用者帳戶可能仍會出現在全域通訊清單中。 第三個問題是，不再存在的萬用群組可能仍會出現在使用者的存取權杖中。

     如果您還原的是通用類別目錄的 DC （不論是不小心或因為這是您信任的單獨備份），建議您在還原作業完成之後，立即停用通用類別目錄，以防止發生延遲物件。 停用通用類別目錄旗標會導致電腦失去其所有部分複本 (分割區) 並 relegating 本身為一般 DC 狀態。

13. 設定 Windows 時間服務。 在樹系根域中，將 PDC 模擬器設定為從外部時間來源同步處理時間。 如需詳細資訊，請參閱在 [樹系根域的 PDC 模擬器上設定 Windows 時間服務](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731191%28v=ws.10%29)。

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>將每個已還原的可寫入網域控制站重新連接到通用網路

在這個階段中，您應該會在樹系根域和每個剩餘的網域中， (和執行的復原步驟) 來還原一個 DC。 將這些 Dc 加入與環境其餘部分隔離的一般網路，然後完成下列步驟，以驗證樹系健康情況和複寫。

> [!NOTE]
> 當您將實體 Dc 加入隔離的網路時，您可能需要變更其 IP 位址。 因此，DNS 記錄的 IP 位址會是錯誤的。 因為無法使用通用類別目錄伺服器，所以 DNS 的安全動態更新將會失敗。 在此情況下，虛擬 Dc 更有利，因為它們可以聯結至新的虛擬網路，而不需要變更其 IP 位址。 這是為什麼建議將虛擬 Dc 作為樹系復原期間要還原的第一個網域控制站的原因之一。

驗證之後，請將 Dc 加入生產網路，並完成步驟來確認樹系複寫健康情況。

- 若要修正名稱解析，請建立 DNS 委派記錄，並視需要設定 DNS 轉送和根提示。 執行 **repadmin/replsum** 來檢查 dc 之間的複寫。
- 如果還原的 DC 不是直接複寫的夥伴，則透過在兩者之間建立暫存連線物件，複寫復原會更快。
- 若要驗證中繼資料清除，請執行 **Repadmin/viewlist \\** _ 以取得樹系中所有網域控制站的清單。 執行 _ *Nltest/DCList：* *  *<網域 \>* ，取得網域中所有網域控制站的清單。
- 若要檢查 DC 和 DNS 的健康情況，請執行 DCDiag/v 來報告樹系中所有 Dc 的錯誤。

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>將通用類別目錄新增至樹系根域中的網域控制站

基於這些原因和其他原因，需要通用類別目錄：

- 為使用者啟用登入。
- 若要啟用每個子網域的 Dc 上執行的 Net Logon 服務，可在根域的 DNS 伺服器上註冊和移除記錄。

雖然樹系根網域控制站最好變成通用類別目錄，但您可以選擇任何已還原的 Dc 成為通用類別目錄。

> [!NOTE]
> DC 將不會公告為通用類別目錄伺服器，直到它完成樹系中所有目錄分割的完整同步處理。 因此，應該強制 DC 與樹系中每個已還原的 Dc 進行複寫。
>
> 監視事件識別碼1119事件檢視器中的目錄服務事件記錄檔，這表示此 DC 是通用類別目錄伺服器，或確認下列登錄機碼的值為1：
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global 類別目錄升級完成**

如需詳細資訊，請參閱 [新增通用類別目錄](AD-Forest-Recovery-Add-GC.md)。

在這個階段中，您應該會有一個穩定的樹系，其中每個網域都有一個 DC，而樹系中有一個通用類別目錄。 您應該為每個您剛剛還原的 Dc 進行新的備份。 您現在可以藉由安裝 AD DS，開始重新部署樹系中的其他 Dc。

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
