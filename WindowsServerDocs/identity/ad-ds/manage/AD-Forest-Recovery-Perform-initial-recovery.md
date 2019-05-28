---
title: AD 樹系復原-執行初始的復原
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 8e05043d029636ddeb3a24349897ac61a713b2a7
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034117"
---
# <a name="perform-initial-recovery"></a>執行初始的復原  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本章節包含下列步驟：  

- [還原每個網域中的第一個可寫入的網域控制站](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [重新連線到網路的每個還原的可寫入網域控制站](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [將通用類別目錄新增至樹系根網域的網域控制站](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>還原每個網域中的第一個可寫入的網域控制站  

若要還原的第一個 DC，從可寫入 DC 的樹系根網域中，完成這一節的步驟。 樹系根網域是很重要，因為它會儲存 Schema Admins 和 Enterprise Admins 群組。 它也有助於維護樹系中的信任階層。 此外，樹系根網域通常會保存 DNS 根伺服器的樹系 DNS 命名空間。 因此，該網域的 Active Directory 整合 DNS 區域會包含在樹系 （也就是為了複寫） 和通用類別目錄的 DNS 資源記錄中所有其他網域控制站的別名 (CNAME) 資源記錄。 
  
復原樹系根網域後，重複相同步驟來復原的剩餘網域樹系中。 您可以同時; 復原多個網域不過，永遠復原父系網域之前復原子系，以避免任何中斷的信任層級或 DNS 名稱解析。 
  
如您所復原的每個網域，請從備份還原只能有一個可寫入 DC。 這是復原的最重要的一部分，因為 DC 都必須具有不已受到任何造成樹系失敗的資料庫。 請務必徹底測試過它導入生產環境之前的受信任備份。 
  
然後執行下列步驟。 執行特定步驟的程序已[AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)。 
  
1. 如果您計畫還原的實體伺服器，請確認目標 DC 未附加，而且因此未連線到生產環境網路的網路纜線。 虛擬機器，您可以移除網路介面卡，或使用連接到其他網路的網路介面卡，您可以在此測試復原程序時於生產網路隔離。 
  
2. 因為這是在網域中的第一個可寫入 DC 時，您必須執行 AD DS 非系統授權還原和 SYSVOL 的授權還原。 還原作業必須完成使用 Active Directory 感知的備份和還原應用程式，例如 Windows Server Backup （也就是您應該還原 DC 使用不支援的方法，例如還原 VM 快照集）。 
   - SYSVOL 的授權還原需要，因為複寫 SYSVOL 的複寫從災害復原後，必須先啟動資料夾。 加入網域中的所有後續 Dc 必須重新同步處理他們的 SYSVOL 資料夾的資料夾，選取之前可以公告資料夾視為已授權的複本。 

   > [!CAUTION]
   > 執行只會針對要還原的樹系根網域中的第一個 DC 的 SYSVOL 的授權 （或主要） 的還原作業。 不正確地在其他網域控制站上執行 SYSVOL 的主要的還原作業會導致複寫的 SYSVOL 資料的衝突。 

   - 有兩個選項執行非系統授權還原 AD DS 和 SYSVOL 的授權還原：  
   - 執行完整伺服器復原，然後強制 SYSVOL 的權威同步。 如需詳細的程序，請參閱[執行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)並[執行權威的同步處理的 DFSR 複寫的 SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。 
   - 執行完整伺服器復原，後面接著系統狀態還原。 這個選項需要您事先建立這兩種備份類型： 完整伺服器備份和系統狀態備份。 如需詳細的程序，請參閱[執行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)並[執行的 Active Directory 網域服務的非系統授權還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。 
  
3. 還原後，當您重新啟動的可寫入 DC 時，請確認該失敗不會影響 DC 上的資料。 如果 DC 資料已損毀，然後重複步驟 2，使用不同的備份。 
   - 如果還原的網域控制站裝載了操作主機角色，您可能需要新增下列登錄項目，以避免變成無法使用，直到它完成複寫可寫入的目錄磁碟分割的 AD DS:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations**  
  
      建立具有資料類型的項目**REG_DWORD**值，並針對**0**。 樹系完整復原之後，您可以重設到這個項目的值**1**、 需要重新啟動網域控制站，以及保留有成功的 AD DS 操作主機角色的輸入和輸出具有複寫其已知複本合作夥伴之前它會自行通告為網域控制站，並開始為用戶端提供服務。 如需初始同步處理需求，請參閱知識庫文章[305476](https://support.microsoft.com/kb/305476)。 
  
      您只還原，並確認資料之後，您將此電腦加入實際執行網路之前，請繼續至下一個步驟。 
  
4. 如果您懷疑全樹系失敗與有關網路入侵或惡意的攻擊，重設所有的系統管理帳戶，包括 Enterprise Admins、 Domain Admins、 Schema Admins、 Server Operators、 帳戶成員的帳戶密碼運算子群組等等。 其他網域控制站樹系復原中的下一步的工作階段安裝之前，就應該完成的系統管理帳戶密碼重設。 
5. 在第一個樹系根網域中的還原網域控制站拿取所有全網域和全樹系操作主機角色。 需要 Enterprise Admins 和 Schema Admins 認證才能拿取全樹系操作主機角色。 
  
     在每一個子網域中，拿取全網域操作主機角色。 雖然您可能會保留操作主機角色只能暫時，已還原的 DC 上拿取這些角色可確保您哪些 DC 是裝載它們目前在樹系修復程序。 您後續復原程序的一部分，您可以重新發佈操作主機角色所需。 如需拿取操作主機角色的詳細資訊，請參閱[拿取操作主機角色](AD-forest-recovery-seizing-operations-master-role.md)。 如需有關在哪裡放置操作主機角色的建議，請參閱[什麼是操作主機？](https://technet.microsoft.com/library/cc779716.aspx)。 
  
6. 清除您不還原從備份 (在這個第一個網域控制站以外的網域中的所有可寫入 Dc) 的樹系根網域中所有其他可寫入 Dc 的中繼資料。 如果您使用的 Active Directory 使用者和電腦或 Active Directory 站台和隨附於 Windows Server 2008 或更新版本的服務或 Windows Vista 的 RSAT 版本或更新版本中，中繼資料清除作業會自動執行時刪除的 DC 物件。 此外，針對已刪除的網域控制站電腦物件的伺服器物件會一併刪除自動。 如需詳細資訊，請參閱 <<c0> [ 清除中繼資料移除可寫入 Dc](AD-Forest-Recovery-Cleaning-Metadata.md)。 
  
     清除中繼資料，如果在不同站台 DC 上安裝 AD DS 時，即會阻止可能重複的 NTDS 設定物件。 原則上，這可能也儲存知識一致性檢查程式 (KCC) 網域控制站本身可能不會出現時，建立複寫連結的程序。 此外，組件的中繼資料清除作業，在網域中所有其他網域控制站的 DC 定位程式 DNS 資源記錄，將會刪除從 DNS。 
  
     移除網域中所有其他網域控制站的中繼資料，直到此 DC，就好像在復原之前的 RID 主機不會假設 RID 主機角色，因此不可能會無法發出新的 Rid。 您可能會看到事件識別碼 16650 系統記錄檔中指出這項失敗的事件檢視器中，但您應該會看到事件識別碼 16648 表示成功一段時間之後，您已清除中繼資料。 
  
7. 如果您有儲存在 AD DS 的 DNS 區域，請確定本機 DNS 伺服器服務會安裝並已還原的 DC 上執行。 如果此 DC 不是 DNS 伺服器的樹系失敗前，您必須安裝並設定 DNS 伺服器。 
  
    > [!NOTE]
    > 如果還原的網域控制站執行 Windows Server 2008，您需要安裝知識庫文件中的 hotfix [975654](https://support.microsoft.com/kb/975654)或將伺服器連線到隔離網路暫時才能安裝 DNS 伺服器。 不需要任何其他版本的 Windows Server hotfix。 
  
     在樹系根網域中，設定它自己的 IP 位址 （或迴路位址，例如 127.0.0.1） 還原網域控制站作為其偏好的 DNS 伺服器。 您可以在區域網路 (LAN) 介面卡的 TCP/IP 內容中設定此設定。 這是第一部 DNS 伺服器的樹系中。 如需詳細資訊，請參閱 <<c0> [ 設定為使用 DNS 的 TCP/IP](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在每一個子網域中，設定還原的網域控制站的樹系根網域中的第一個 DNS 伺服器的 IP 位址作為其偏好的 DNS 伺服器。 您可以在區域網路介面卡的 TCP/IP 內容中設定此設定。 如需詳細資訊，請參閱 <<c0> [ 設定為使用 DNS 的 TCP/IP](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。 
  
     在 _msdcs 和網域 DNS 區域中，刪除不再存在的中繼資料清除作業之後的 Dc 的 NS 記錄。 檢查是否已清除的網域控制站的 SRV 記錄。 若要加快 DNS SRV 記錄移除，請執行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. 引發 100,000 可用的 RID 集區的值。 如需詳細資訊，請參閱 <<c0> [ 引發的可用 RID 集區值](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果您有理由相信 100,000 引發 RID 集區，並不足以執行您的特定情況時，您應該判斷仍然安全地使用最低增加。 Rid 是不應增加不必要的有限資源。 
  
     如果網域中建立新的安全性主體在您還原所用之備份的時間之後，這些安全性主體可能會對某些物件的存取權限。 這些安全性主體不存在復原之後因為復原已還原備份;不過，可能仍然會存在其存取權限。 如果可用的 RID 集區就不會引發還原之後，新的使用者物件建立之後的樹系復原可能會取得相同的安全性識別碼 (Sid)，並可能會有存取這些物件，其最初不是。 
  
     為了說明，假設名為簡介中所提及的 Amy 新員工的範例。 因為它用來還原網域中的備份之後建立 Amy 的使用者物件不存在之後還原作業。 不過，任何已指派給該使用者物件的存取權可能會保存在還原作業之後。 如果該使用者物件 SID 會指派給新的物件，在還原作業之後，新的物件會取得這些存取權限。 
  
9. 目前的 RID 集區使其失效。 系統狀態還原之後，將目前的 RID 集區會失效。 但如果未執行系統狀態還原，需要目前的 RID 集區失效，以防止重新發行 RID 集區中建立備份時已指派的 Rid 還原的網域控制站。 如需詳細資訊，請參閱 <<c0> [ 使目前的 RID 集區失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)。 
  
    > [!NOTE]
    > 第一次您嘗試建立物件的 sid，使 RID 集區之後，您會收到錯誤。 嘗試建立的物件就會觸發新的 RID 集區的要求。 重試此作業成功，因為將會配置新的 RID 集區。 
  
10. 兩次重設此 DC 的電腦帳戶密碼。 如需詳細資訊，請參閱 <<c0> [ 重設電腦帳戶密碼的網域控制站](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。 
  
11. 重設 krbtgt 密碼兩次。 如需詳細資訊，請參閱 <<c0> [ 重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。 
  
     由於 krbtgt 密碼歷程記錄是兩個密碼，重設密碼兩次，密碼歷程記錄中移除原始 (prefailure) 密碼。 
  
    > [!NOTE]
    > 如果樹系復原時出現安全性缺口回應中，您可能會重設信任密碼。 如需詳細資訊，請參閱 <<c0> [ 重設一方的信任的信任密碼](AD-Forest-Recovery-Reset-Trust.md)。 
  
12. 如果樹系有多個網域，而還原的網域控制站是否為通用類別目錄伺服器在失敗前的，清除**通用類別目錄**NTDS 設定屬性，若要移除之 dc 的通用類別目錄中的核取方塊。 此規則的例外狀況是只在一個網域的樹系的常見案例。 在此情況下，不需要移除通用類別目錄。 如需詳細資訊，請參閱 <<c0> [ 移除通用類別目錄](AD-Forest-Recovery-Remove-GC.md)。 
  
     從多個的備份還原為通用類別目錄比其他備份用來還原網域控制站在其他網域中，您可能會造成延遲物件。 請考慮下列的範例。 在網域 A 中，DC1 會從在 T1 時間點拍攝的備份還原。 在網域 B 中，DC2 從 T2 時間建立的通用類別目錄備份還原。 假設 T1 和 T2 之間建立某些物件和 T2 會比 T1。 還原這些網域控制站之後，DC2，也就是通用類別目錄，包含網域 A 的部分複本比網域 A 保存較新的資料本身。 DC2，因為這些物件未出現在 DC1 上，在此情況下，保留延遲物件。 
  
     問題可能會導致延遲物件的目前狀態。 比方說，可能無法傳遞的電子郵件訊息，其使用者物件已移動網域之間的使用者。 您將過期的 DC 或通用類別目錄伺服器恢復上線之後，這兩個執行個體的使用者物件出現在通用類別目錄。 這兩個物件具有相同的電子郵件地址;因此，無法傳送電子郵件訊息。 
  
     第二個問題已不存在的使用者帳戶可能仍會出現在全域通訊清單。 第三個問題是萬用群組不存在，可能仍會出現在使用者的存取權杖。 
  
     如果您未還原的 DC 是通用類別目錄，— 可能是不小心或因為這是您信任的單獨備份 — 我們建議您避免在還原作業後，請停用通用類別目錄延遲物件的相符項目完成。 停用的通用類別目錄的旗標，會導致遺失所有其部分複本 （資料分割），以及將本身一般 DC 狀態的電腦。 
  
13. 設定 Windows 時間服務。 在樹系根網域中，將 PDC 模擬器設定為與外部時間來源同步處理時間。 如需詳細資訊，請參閱 <<c0> [ 樹系根網域中的 PDC 模擬器上設定 Windows 時間服務](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>重新連線到通用網路的每個還原的可寫入網域控制站

在這個階段您應該有一個還原的網域控制站 （執行的復原步驟） 樹系根網域中，並在每個剩餘的網域。 加入一般網路隔離環境的其餘部分中的這些網域控制站，並完成下列步驟以驗證樹系健康情況和複寫。

> [!NOTE]
> 當您將實體的網域控制站加入隔離網路時，您可能需要變更其 IP 位址。 如此一來，DNS 記錄的 IP 位址就會出錯。 因為無法使用通用類別目錄伺服器，將會失敗的 DNS 的安全動態更新。 因為加入新的虛擬網路而不需要變更其 IP 位址，會在此情況下更有利虛擬網域控制站。 這是為什麼虛擬網域控制站建議使用做為第一個網域控制站在樹系復原期間要還原的原因之一。 
  
通過驗證之後，加入實際執行網路中的網域控制站，並完成步驟以驗證樹系複寫健康情況。

- 若要修正名稱解析，建立 DNS 委派記錄，並設定 DNS 轉寄和根提示，視需要。 執行**repadmin /replsum**檢查網域控制站之間的複寫。 
- 如果還原網域控制站不直接複寫協力電腦，複寫復原將會更快藉由建立它們之間的暫時連線物件。 
- 若要驗證中繼資料清除作業，請執行**Repadmin /viewlist \*** 樹系中的所有網域控制站的清單。 執行**Nltest /DCList:** *< 網域\>* 取得一份網域中的所有網域控制站。 
- 若要檢查 DC 和 DNS 的健全狀況，請執行 DCDiag /v 來報告錯誤，在樹系中的所有網域控制站上。 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>將通用類別目錄新增至樹系根網域的網域控制站

通用類別目錄做是為了這些和其他的原因：  
  
- 若要啟用使用者的登入。 
- 若要啟用 Net Logon 服務在網域控制站的每一個子網域來註冊和根網域中移除 DNS 伺服器上的記錄上執行。 
  
雖然它是慣用的樹系根 DC 會成為通用類別目錄，就可以選擇任何還原網域控制站成為通用類別目錄。 
  
> [!NOTE]
> 不會將 DC 通告為通用類別目錄伺服器，直到它完成完整同步處理的樹系中的所有目錄分割。 因此，應該強制 DC 以每個樹系中已還原的網域控制站進行複寫。 
>
> 監視目錄服務事件記錄檔事件檢視器中的事件識別碼 1119，這表示此 DC 是通用類別目錄伺服器，或確認下列登錄機碼具有的值為 1:  
>
> **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global Catalog Promotion Complete**  
  
如需詳細資訊，請參閱 <<c0> [ 新增通用類別目錄](AD-Forest-Recovery-Add-GC.md)。 
  
在這個階段您應該具有穩定樹系中，有一個網域控制站，為每個網域和一個通用類別目錄的樹系中。 您應該讓新的備份，每個網域控制站，您剛還原。 您現在可以開始安裝 AD DS 來重新部署樹系中的其他網域控制站。 

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
