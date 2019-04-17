---
title: "廣告樹系修復-執行初始復原"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>執行初始復原  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 這一節包含下列步驟：  
  
-   [還原在每個網域中的第一個寫入的網域控制站](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [重新連接網路每個還原寫入網域控制站](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [通用加入網域控制站森林根網域中](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>還原在每個網域中的第一個寫入的網域控制站  
 開頭寫入 DC 森林根網域中，若要還原的第一個 DC 完成本節中的步驟操作。 森林根網域很重要，因為它存放區結構描述管理員和企業系統管理員 」 群組。 這也有助於維護信任的樹系階層。 此外，森林根網域通常會保留根的 DNS 伺服器的樹系 DNS 命名空間。 因此，該網域 Active Directory – 整合 DNS 區域包含別名 (CNAME) 資源森林 （所需的複寫），與通用 DNS 資源記錄所有其他網域控制站。  
  
 您可以復原森林根網域之後，請重複執行相同的步驟來復原森林中的其餘的網域。 您也可以同時; 復原一個以上的網域不過，一律復原家長網域之前復原的一位子女來防止任何信任階層或 DNS 名稱解析中斷。  
  
 針對每個您復原網域，從備份還原只有一個寫入俠。 這是最重要復原的一部分，因為 DC 必須具有已不受到導致樹系失敗資料庫。 請務必讓您有備無患信任完全引入 production 環境之前測試。  
  
 然後執行下列步驟。 來執行特定步驟程序會在[AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)。  
  
1.  如果您想要還原實體伺服器，確定俠未連接，因此未連接到 production 網路目標的網路線。 一樣，您可以移除網路介面卡，或使用的網路介面卡連接到其他網路位置，您可以測試隔離的實際執行網路時的修復程序。  
  
2.  因為這是第一個寫入 DC 網域中的，您必須執行 AD ds 未授權的還原和 SYSVOL 授權。 還原操作必須完成使用 Active Directory 感知備份及還原應用程式，例如 「 Windows 備份伺服器 （也就是，您應該不 DC 使用還原不支援的方法，例如還原 VM 快照）。  
  
     授權 SYSVOL 是因為 SYSVOL 複寫複寫之後從損壞您復原必須開始進行] 資料夾。 加入網域中的所有後續 Dc 必須先授權可將通知資料夾已選取的資料夾一份與他們 SYSVOL 資料夾重新同步處理。  
  
    > [!CAUTION]
    >  操作僅會針對還原森林根網域中的第一個 DC SYSVOL 授權 （或主要） 還原。 會導致複寫衝突 SYSVOL 資料的正確執行其他網域控制站 SYSVOL 的主要還原操作。  
  
     有兩個選項執行 AD ds 未授權的還原和授權 SYSVOL:  
  
    -   執行完整伺服器修復，然後強制 SYSVOL 授權同步處理。 詳細的程序，請查看[進行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[執行的 DFSR 複寫 SYSVOL 授權同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  
  
    -   執行系統還原狀態，以及完整伺服器修復。 這個選項必須事先建立的備份這兩種類型： server 的完整備份與系統狀態備份。 詳細的程序，請查看[進行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)和[執行未授權的 Active Directory Domain Services 還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)。  
  
3.  您還原並寫入俠重新開機之後，請確認該失敗不會影響 DC 上的資料。 如果已損壞俠資料，再使用不同的備份重複步驟 2。  
  
     如果還原的網域控制站裝載作業主角，您可能需要新增避免 AD DS 無法使用，直到完成寫入 directory 磁碟分割的複寫登錄下列項目：  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl 執行初始同步處理**  
  
     建立的資料類型的項目**呼叫完成**並為**0**。 樹系復原完全之後，您可以重設值的此項目，以**1**、 輸入這需要重新開機，並保留作業有主機角色成功的網域控制站 AD DS 和戶端輸出複寫已知的複本合作之前為網域控制站通知本身並開始提供服務。 如需有關初始同步需求，查看知識庫文章[305476](https://support.microsoft.com/kb/305476)。  
  
     只有還原和驗證資料之後，這台電腦加入 production 網路之前，請繼續接下來的步驟。  
  
4.  如果您懷疑的樹系的失敗相關網路入侵或惡意攻擊，重設密碼管理帳號，包括成員企業系統管理員，網域系統管理員，架構系統管理員，伺服器電信業者、 Account 電信業者群組，等等。 管理 account 密碼重設應該額外的網域控制站安裝下一個階段的樹系復原之前完成。  
  
5.  森林根網域中的第一次還原 DC、 上抓取所有網域全和樹系作業主機角色。 抓取樹系操作主要的角色所需企業系統管理員 」 及架構管理員認證。  
  
     在每個子女網域中抓取全網域操作主機角色。 雖然您可能會保留操作主機角色還原 DC 只能暫時、 上的抓取這些角色可確保您哪些俠主控它們此時中的樹系的修復程序。 您後修復程序的一部分，您可以將套件轉操作主機角色視。 如需有關抓取操作主機角色資訊，請查看[抓取作業主角](AD-forest-recovery-seizing-operations-master-role.md)。 放置操作主機角色建議，請查看[操作主機有哪些？](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  清除所有其他寫入網域控制站的樹系根網域中，您不從 (除了此第一次 DC 網域中的所有寫入 Dc) 的備份還原中繼資料。 如果您使用的 Active Directory 使用者和電腦或 Active Directory 網站和服務會包含 Windows Server 2008，或更新版本或 RSAT 適用於 Windows Vista 版本或更新版本，清除中繼資料會自動執行當您 delete DC 物件。 此外，伺服器物件與電腦物件的刪除俠也會刪除自動。 如需詳細資訊，請查看[清潔中繼資料中移除寫入 Dc](AD-Forest-Recovery-Cleaning-Metadata.md)。  
  
     如果已安裝在不同的網站 DC AD DS，清除中繼資料會防止可能重複的物件 NTDS 設定。 潛在，這可能也儲存知識一致性檢查程式 (KCC) 網域控制站本身可能不會出現時，建立複製連結的程序。 此外，中繼資料清理] 中，部分俠定位 DNS 網域中的所有其他 Dc 資源記錄繼續嗎 DNS 從。  
  
     已移除網域中的所有其他 Dc 中繼資料，除非此 DC，它就像之前復原 RID 主機將假設 RID 主角和因此將無法發出新 Rid。 您可能會看到 263 16650 系統木頭中的指示此錯誤，事件檢視器中，但您應該會看到 263 16648 表示成功有點時之後您已清除中繼資料。  
  
7.  如果您擁有的儲存在 AD DS DNS 區域，請確定本機 DNS 伺服器服務安裝並還原您的 DC 上執行。 如果此俠不是樹系失敗前的 DNS 伺服器，您必須安裝和設定的 DNS 伺服器。  
  
    > [!NOTE]
    >  還原網域控制站執行 Windows Server 2008，如果您需要知識庫文章 hotfix [975654](https://support.microsoft.com/kb/975654)或連接伺服器隔離網路暫時才能安裝 DNS 伺服器。 不需要任何其他版本的 Windows Server hotfix。  
  
     在 [樹系根網域中，設定還原網域控制站具有其本身的 IP 位址 （或回送地址，例如 127.0.0.1） 做為其慣用 DNS 伺服器。 您可以設定此設定中的區域網路 （區域網路） 介面卡 TCP/IP 屬性。 這是森林中的第一個 DNS 伺服器。 如需詳細資訊，請查看[設定 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。  
  
     在每個子女網域中的樹系根網域中的第一個 DNS 伺服器的 IP 位址設定還原網域控制站做為其慣用 DNS 伺服器。 您可以設定此設定中的區域網路介面卡 TCP/IP 屬性。 如需詳細資訊，請查看[設定 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx)。  
  
     _Msdcs 和網域 DNS 區域中 delete 奈秒記錄的網域控制站的中繼資料清除之後已不存在。 檢查是否已移除 SRV 記錄向上已清除網域控制站。 為了加快 DNS-SRV 使用碼表進行移除，請執行：  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  透過 100000 提高提供 RID 集區的值。 如需詳細資訊，請查看[引發的值提供 RID 集區的](AD-Forest-Recovery-Raise-RID-Pool.md)。 如果您認為調高移除集區 100000 是不足特定遭遇問題的原因，您應該會判斷使用仍然安全最低增加。 Rid 提供有限應該不會用完不必要的資源。  
  
     如果您用來還原備份的時間後網域中建立新的安全性原則，這些安全性原則可能會有特定物件的存取權限。 這些安全性原則找不到修復之後因為復原已還原備份。不過，可能仍然存在其存取權。 如果不使用 RID 集區引發還原之後，新的使用者會物件所建立的樹系復原可能會取得的相同的安全 Id (Sid) 之後，而且可能會原本不這些物件，存取。  
  
     若要闡述，請考慮員工名張瑾所述導入新的範例。 因為它用來還原網域備份後建立的使用者張瑾物件不存在之後還原操作。 不過，可能會還原後仍然存在任何存取權限已指派給使用者物件。 如果使用者物件的 SID 會還原操作之後將指派給新的物件，新物件會取得的存取權限。  
  
9. 使目前 RID 集區。 目前 RID 集區系統狀態還原後失效。 但無法執行系統還原狀態，如果需要失效防止還原網域控制站重新 RID 集區中已建立備份之同時指派發行 Rid 目前 RID 集區。 如需詳細資訊，請查看[失效目前 RID 集區](AD-Forest-Recovery-Invaildate-RID-Pool.md)。  
  
    > [!NOTE]
    >  第一次嘗試使用 SID 建立物件之後您使 RID 集區,，您會收到一則錯誤。 嘗試將建立物件觸發 RID 新集區的要求。 重試作業的成功因為將配置 RID 新集區。  
  
10. 重設電腦 account 密碼此 dc 兩次。 如需詳細資訊，請查看[重設電腦密碼的網域控制站的](AD-Forest-Recovery-Reset-Computer-Account-DC.md)。  
  
11. 重設 krbtgt 密碼兩次。 如需詳細資訊，請查看[krbtgt 密碼重設](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)。  
  
     因為 krbtgt 密碼歷史兩個的密碼，請重設密碼兩次，以移除密碼歷史原始 (prefailure) 密碼。  
  
    > [!NOTE]
    >  如果樹系復原是的回應安全性漏洞，您也可能會重設信任的密碼。 如需詳細資訊，請查看[重設密碼信任一方信任的](AD-Forest-Recovery-Reset-Trust.md)。  
  
12. 如果樹系有多個網域還原網域控制站是一個通用伺服器失敗之前，清除 [**通用**核取方塊，在通用移除 DC NTDS 設定屬性。 此規則例外是一個網域的樹系的常見案例。 若是如此，不需要移除通用。 如需詳細資訊，請查看[移除通用](AD-Forest-Recovery-Remove-GC.md)。  
  
     藉由從更多的備份還原通用最近比其他備份用來還原網域控制站在其他網域中，您可能會介紹延遲物件。 請參考下列範例。 在 A 網域中，從次 T1 拍攝的備份還原 DC1。 網域 B，DC2 會從拍攝次 T2 通用備份還原。 假設 T2 T1，比新，而且某些物件建立 T1 之間 T2。 還原這些 Dc 之後，DC2，首先，保留較新的資料網域 A 對比網域 A 的部分複本本身。 因為這些物件未出現在 DC1 DC2，這種情形下，會保留延遲物件。  
  
     出現的 [延遲物件可能會造成問題。 例如，電子郵件可能不會傳遞給使用者的使用者物件網域之間移動。 您將過時的俠或通用伺服器回 online 後，使用者物件兩個出現在通用。 這兩個物件有相同的電子郵件地址。因此，您無法傳送電子郵件訊息。  
  
     第二個問題時，不存在帳號可能仍會出現在全球地址清單。 第三個問題，不存在通用群組中的使用者存取權杖可能仍會出現。  
  
     如果您未還原是通用俠-「 不小心或，因為您信任的孤獨備份，我們建議您避免即將還原作業完成之後，停用通用延遲物件的項目。 停用通用旗標，會導致遺失所有其部分複本 （分割），並將本身一般俠狀態的電腦。  
  
13. 設定 Windows 時間服務。 在森林根網域中，設定肯定同步處理時間時間外部來源。 如需詳細資訊，請查看[上的樹系根網域中肯定設定 Windows 時間服務](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>重新連接常見網路每個還原寫入網域控制站  
 在這個階段您應該會有一個俠還原 （和修復的步驟執行） 森林根網域中，並在每個剩餘網域。 加入這些網域控制站常見的環境中的其餘部分隔離的網路，以驗證樹系健康和複寫完成下列步驟。  
  
> [!NOTE]
>  當您加入網域控制站實體隔離的網路時，您可能需要變更其 IP 位址。 如此一來，您將會錯誤 DNS 記錄的 IP 位址。 因為通用伺服器無法使用，DNS 安全的動態更新將會失敗。 因為加入新的網路 virtual 而不會變更其 IP 位址，會在這種情形下更多的優點 virtual Dc。 這就是 virtual 網域控制站原因做的第一個網域控制站建議樹系修復時需要還原。  
  
 驗證之後加入網域控制站 production 網路，並完成步驟以確認 [樹系複寫健康。  
  
-   若要修正名稱解析，建立 DNS 委派記錄並設定 DNS 轉寄和根提示視。 執行**repadmin /replsum**來檢查複寫之間網域控制站。  
  
-   如果還原網域控制站不複寫直接合作夥伴，請複寫復原將會更快地建立之間暫時連接物件。  
  
-   如果要驗證清除中繼資料，執行**Repadmin /viewlist \ ***森林中的所有網域控制站的清單。 執行**Nltest /DCList:** *< domain\ >*的網域中的所有網域控制站的清單。  
  
-   若要檢查 DC 和 DNS 的健康狀態，來報告錯誤森林中的所有網域控制站上執行 DCDiag /v。  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>通用加入網域控制站森林根網域中  
 首先，才能這些和其他原因：  
  
-   若要讓使用者登入。  
  
-   若要讓每個子女網域中的 dc 登記並移除根網域中的 DNS 伺服器上的記錄執行網路登入服務。  
  
 雖然它慣用的樹系根俠變得通用，就可以選擇任何還原網域控制站成為通用。  
  
> [!NOTE]
>  直到完成完整的同步處理森林中的所有 directory 磁碟分割，DC 不會通知為通用伺服器。 因此，DC 應該被迫複寫還原網域控制站森林中的每個。  
>   
>  監視 Directory 服務事件登入事件檢視器的事件編號 1119，這表示在這個網域控制站是通用伺服器，或驗證下列機碼會有 1 的值：  
>   
>  **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global Catalog 升級完成**  
  
 如需詳細資訊，請查看[新增通用](AD-Forest-Recovery-Add-GC.md)。  
  
 在此階段您應該會有一個網域控制站的每個網域與穩定森林和一個通用森林中。 您應該讓每個您只要還原 Dc 新備份。 您現在可以重新安裝 AD DS 部署森林中的其他 Dc 開始。  

## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
  
