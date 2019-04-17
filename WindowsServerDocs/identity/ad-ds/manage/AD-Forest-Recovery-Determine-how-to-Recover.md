---
title: "廣告樹系修復-判斷如何復原森林"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>判斷如何復原森林  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 復原整個 Active Directory 樹系在森林中每個網域控制站 (DC) 需要從備份還原或重新安裝 Active Directory Domain Services (AD DS)。 復原樹系還原森林中的每個網域狀態一次的受信任的最後一個備份。 因此，還原將會導致遺失至少 Active Directory 下列資料：  
  
-   已新增受信任的最後一個備份後 （例如，使用者與電腦） 所有物件  
  
-   自從現有物件受信任的最後一個備份的所有更新  
  
-   所有上次信任備份設定磁碟分割或架構 （例如架構變更） AD ds 磁碟分割所做的變更  
  
 森林中的每個網域，必須知道的網域管理員 account 密碼。 最好是，這是建，且不會停用的密碼。 您還必須知道 DSRM 執行系統狀態還原 DC 的密碼。 一般而言，它是個好習慣，只要備份不正確，也就是或刪除物件期間期間如果尚未 Active Directory 資源回收筒中標記期間期間保存管理員和 DSRM 密碼歷史在安全的地方的。 您也可以使用核對使用者的 DSRM 密碼同步處理為了讓您更輕鬆地記住。 如需詳細資訊，查看知識庫文章[961320](https://support.microsoft.com/kb/961320)。 同步處理 DSRM account 必須完成之前樹系復原，準備的一部分。  
  
> [!NOTE]
>  管理員是根據預設，系統管理員 」 建群組成員網域系統管理員 」 及企業系統管理員 」 群組。 此群組網域中有完整的所有網域控制站的控制項。  
  
## <a name="determining-which-backups-to-use"></a>判斷要使用的備份  
 在至少兩個寫入網域控制站的每個網域定期備份，您有幾個可選擇備份。 請注意，您無法使用唯讀網域控制站 (RODC) 的備份還原寫入 DC。 我們建議您在使用拍攝發生失敗的前幾天的備份還原網域控制站。 一般而言，您必須判斷 recentness 和還原資料的 safeness 之間折衷。 選擇較新的備份復原更多有用的資料，但可能會增加 reintroducing 危險資料入還原樹系的風險。  
  
 原始作業系統和伺服器的備份還原備份系統狀態而有所不同。 例如，您不應該系統狀態的備份還原到不同的伺服器。 在這種情形下，您可能會看見下列警告：  
  
 「非目前不同是伺服器的指定的備份。 我們不建議執行系統狀態復原備份至另一部因為伺服器可能會進入不穩定。 確定您想要使用此備份復原目前伺服器？」  
  
 如果您需要 Active Directory 還原到不同的硬體，請建立 server 的完整備份和來執行完整伺服器修復計劃。  
  
> [!IMPORTANT]
>  開始使用 Windows Server 2008，它不受支援系統狀態備份還原到新的 Windows Server 安裝新的硬體或相同的硬體。 如果建議在本文稍後的方式相同的硬體，會重新安裝 Windows Server，您可以還原此訂單的網域控制站：  
>   
>  1.  若要還原作業系統所有檔案和應用程式執行完整伺服器都還原。  
> 2.  執行系統狀態還原為了 SYSVOL 標示為授權使用 wbadmin.exe。  
>   
>  如需詳細資訊，查看 Microsoft 知識庫文章[249694](https://support.microsoft.com/kb/249694)。  
  
 如果不明相符項目的失敗的時間，進一步調查找出保留最後一個安全狀態的樹系的備份。 這種方式較不建議。 因此，我們非常建議這樣的樹系失敗時，可以找大約失敗時 AD DS 的健康狀態的相關詳細的登讓每日。 您也應該保留本機的備份，您可以更快速地復原複本。  
  
 如果尚未 Active Directory 資源回收桶，備份期間相當於**deletedObjectLifetime**值或**tombstoneLifetime**的值，少於。 如需詳細資訊，請查看[Active Directory 資源回收桶 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=178657)(https://go.microsoft.com/fwlink/?LinkId=178657)。  
  
 或者，您也可以使用 Active Directory 資料庫裝載工具 (Dsamain.exe) 和輕量型 Directory 存取通訊協定 (LDAP) 工具，例如 Ldp.exe 或 Active Directory 使用者和電腦找出要備份具有樹系的最後安全狀態。 Active Directory 資料庫裝載工具，包括 Windows Server 2008 和 Windows Server 作業系統更新中，將會公開 Active Directory 資料儲存在備份或快照為 LDAP 伺服器。 然後，您可以使用 LDAP 工具瀏覽資料。 此方法不需要重新任何俠中 Directory 服務還原模式 (DSRM) 來檢查到 AD ds 備份的優點。  
  
 如需有關如何使用 Active Directory 資料庫裝載工具的詳細資訊，請[Active Directory 資料庫裝載工具 Step-by-Step 指南](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx)。  
  
 您也可以使用**ntdsutil 快照**命令建立的 Active Directory 資料庫快照。 排程定期建立快照的工作，您可以取得其他複本 Active Directory 資料庫段時間。 您可以使用這些複本變得更好找出發生樹系失敗時，然後選擇 [還原最佳備份。 若要建立快照，使用的版本**ntdsutil**的船與 Windows Server 2008 或遠端伺服器管理工具 (RSAT) 適用於 Windows Vista 或更新版本。 目標俠執行任何的 Windows Server 版本。 如需有關使用**ntdsutil 快照**命令，查看[快照](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx)。  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>判斷要還原的網域控制站  
 還原程序輕鬆時決定還原的網域控制站要素。 建議您已針對每個網域 DC 還原慣用的專用的 DC。 專用俠更容易可靠地計劃和因為您使用的已用來執行相同來源的設定，請執行樹系復原還原測試。 您可以指令碼復原，並不應付不同的設定，例如是否 DC 保有作業主要的角色，或是否或不是 GC 或 DNS 伺服器。  
  
> [!NOTE]
>  雖然不建議還原求簡便作業主角擁有者，某些組織可能選擇還原另一個用於其他優點。 例如還原 RID 主機有助於避免管理 Rid 復原期間的問題。  
  
 選擇 DC 最符合下列條件：  
  
-   寫入俠。 這是必要的。  
  
-   執行 Windows Server 2012 為一樣支援 VM-GenerationID hypervisor DC。 這個網域控制站可以複製使用做為來源。  
  
-   可以存取實體或 virtual 網路上，最好是位於 datacenter DC。 如此一來，您可以輕鬆地找出它從網路期間樹系復原。  
  
-   DC 有完整的伺服器良好備份。 良好備份是可以成功還原、拍攝失敗時前, 幾天，其中包含更有用的資料，盡可能為備份。  
  
-   已失敗之前的網域名稱系統」(DNS) 伺服器俠。 這樣可以省重新安裝 DNS 所需的時間。  
  
-   如果您也可以使用 Windows 部署服務，選擇 DC 的不設定為使用 BitLocker 網路解除鎖定。 此時，請 BitLocker 網路解除鎖定不支援用來從備份還原的樹系復原期間的第一個 DC。  
  
     BitLocker 網路解除鎖定為*只*鍵保護裝置*無法*上使用 Dc 因為如此一來，會導致案例中，有部署 Windows 部署服務 (WDS) 的第一個 DC 需要 Active Directory 和 WDS 工作以解除鎖定的位置。 尚未還原的第一個 DC 之前，Active Directory 適用於 WDS，讓它無法解除鎖定。  
  
     若要判斷是否 DC 已設定為使用 BitLocker 網路解除鎖定，請檢查網路解除鎖定的憑證可在下列機碼：  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 維護處理，或還原備份的檔案包含 Active Directory 時的安全性程序。 只要隨附樹系修復不小心導致安全的最佳做法而錯過了。 如需詳細資訊，請查看中的「建立網域控制站備份及還原策略」一節[最佳做法指南保護 Active Directory 安裝和 Day-to-Day 作業：第二部](https://technet.microsoft.com/library/bb727066.aspx)。  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>找出目前的樹系結構和 DC 函式  
 檢測軍人森林中的所有網域判斷目前的樹系結構。 在每個網域中，尤其是 Dc 已備份和模擬的 Dc 可以複製的來源，讓所有的網域控制站的清單。 由於您會先復原這個網域將最重要的樹系根網域網域控制站的清單。 還原森林根網域之後，您可以使用 Active Directory 嵌入式管理單元取得其他網域、Dc，並在森林中的網站清單。  
  
 下列範例所示，準備表格網域中顯示的每個 DC 功能。 這將有助於還原回修復之後的樹系前失敗設定。  
  
|DC 名稱|作業系統|FSMO|GC|RODC|備份|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|網域命名主機的架構主機|[是]|否]|[是]|否]|否]|[是]|[是]|  
|DC_2|Windows Server 2012|無|[是]|否]|[是]|[是]|否]|[是]|[是]|  
|DC_3|Windows Server 2012|基礎結構主機|否]|否]|否]|[是]|[是]|[是]|[是]|  
|DC_4|Windows Server 2012|肯定移除主機|[是]|否]|否]|否]|否]|[是]|否]|  
|DC_5|Windows Server 2012|無|否]|否]|[是]|[是]|否]|[是]|[是]|  
|RODC_1|Windows Server 2008 R2|無|[是]|[是]|[是]|[是]|[是]|[是]|否]|  
|RODC_2|Windows Server 2008|無|[是]|[是]|否]|[是]|[是]|[是]|否]|  
  
 森林中的每個網域中找出單一寫入 DC 的 Active Directory 資料庫該網域信任的備份。 當您選擇還原 DC 備份，請務必小心。 如果大約已知的日期和失敗的原因，一般建議使用所建立的備份日期的前幾天。  
  
 在此範例中，有四種備份的候選項目：DC_1、DC_2、DC_4，以及 DC_5。 這些備份的候選項目的還原只有一個。 建議的俠是 DC_5 原因如下：  
  
-   使用它做為來源模擬俠複製的需求，滿足、為複製允許 virtual DC 支援 VM-GenerationID，執行的軟體，hypervisor 上執行 Windows Server 2012 (或，可以將無法是否移除複製)。 復原之後，PDC 角色模擬器將會以取回伺服器，它可以加入的網域複製網域控制站群組。  
  
-   它會執行完整的 Windows Server 2012 安裝。 執行 Server Core 安裝 DC 可以是不方便為目標進行復原。  
  
-   它是 DNS 伺服器。 因此，DNS 不必則會重新安裝。  
  
> [!NOTE]
>  DC_5 不是通用伺服器，因為它也有利用通用不需要還原之後被移除。 但 DC 也是通用伺服器不是決定性係數因為開始使用 Windows Server 2012，所有網域控制站預設值，並移除並在任何案例還原建議的樹系的修復程序的一部分，加入通用通用伺服器。  
  
## <a name="recover-the-forest-in-isolation"></a>復原隔離森林  
 若要關閉所有寫入網域控制站之前第一次還原網域控制站帶回 production 是慣用的案例。 這樣可確保回到復原森林不會複寫危險的任何資料。 請務必尤其是關機所有作業主角位置。  
  
> [!NOTE]
>  可能有您想要的每個網域隔離網路復原允許其他保留 online 為了降到最低系統當機的網域控制站同時的第一個 DC 將移動的案例。 例如，如果您要修復的架構失敗的升級，您選擇保留 production 在網路上執行時修復的步驟執行隔離的網域控制站。  
  
 如果您正在執行模擬的 Dc，您可以移動 virtual 網路隔 production 網路位置，您將會執行復原。 移動到不同的網路的模擬的 Dc 提供兩個優點：  
  
-   無法復原的 Dc 再次造成樹系復原，因為它們隔離的問題。  
  
-   模擬俠複製可以在不同網路上執行，使 Dc 重大一些可以執行和之前 production 網路回到匯測試。  
  
 如果您正在執行網域控制站在實體硬體，中斷連接想要還原森林根網域中的第一個 dc 網路線。 如果可能的話，也拔除所有其他網域控制站的網路纜長度。 如此可防止 Dc 複寫，如果您不小心開始使用的樹系的修復程序期間。  
  
 大的樹系分散在多個位置，很難保證所有寫入網域控制站的關機。 基於這個原因，修復的步驟，例如重設電腦 account 和 krbtgt 帳號，此外中繼資料清理 — 設計用來確保復原寫入網域控制站執行不複寫危險寫入 dc（以防一些仍然 online 森林中）。  
  
 不過，只要 offline 拍攝寫入 Dc 可以您保證不會複寫嗎？ 因此，可能的話，您應該部署，可協助您關機並實體樹系復原期間隔離寫入網域控制站的遠端管理技術。  
  
 Rodc 可以繼續運作寫入網域控制站在離線時。 任何其他俠會直接從任何 RODC 複寫的任何變更，尤其不是架構或設定容器變更，讓它們復原時不會為寫入 Dc 相同的風險。 所有寫入網域控制站復原並 online 之後，您應該重建所有 Rodc。  
  
 Rodc 將繼續允許存取本機資源時復原作業的平行進行的快取中他們各自的網站。 本機不會在 RODC 快取的資源將會有轉寄給寫入 DC 驗證要求。 因為寫入 Dc 離線，將會失敗這些要求。 除非您復原寫入網域控制站某些作業，例如變更密碼也將無法運作。  
  
 如果您正在使用的網路中樞支點架構，您可以復原寫入網域控制站中心站台上第一次專注。 之後，您可以重建 Rodc 遠端網站。  
  
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
