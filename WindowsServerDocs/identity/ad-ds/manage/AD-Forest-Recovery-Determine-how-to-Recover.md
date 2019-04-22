---
title: AD 樹系復原-判斷如何復原樹系
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817729"
---
# <a name="determine-how-to-recover-the-forest"></a>判斷如何復原樹系

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

復原整個 Active Directory 樹系包含從備份中還原，或重新安裝 Active Directory 網域服務 (AD DS) 樹系中每個網域控制站 (DC) 上。 復原樹系會還原樹系中的每個網域到其狀態在信任的最後備份的時間。 因此，在還原作業會導致遺失至少下列的 Active Directory 資料：

- （例如使用者和電腦） 已加入信任的最後備份之後的所有物件
- 已對現有的物件由於受信任的最後一個備份的所有更新
- 在受信任的上次備份之後對設定磁碟分割或結構描述中的資料分割 （例如結構描述變更） 的 AD DS 所做的所有變更

樹系中的每個網域必須知道網域系統管理員帳戶的密碼。 最好的是，這是內建的系統管理員帳戶的密碼。 您也必須知道 DSRM 密碼，才能執行系統狀態還原的 DC。 一般情況下，最好的只要是有效的備份，也就是或已刪除的物件存留期內標記存留期期間，如果 Active Directory 資源回收封存在安全的地方的 DSRM 密碼歷程記錄與系統管理員帳戶已啟用分類收納。 您也可以使用網域使用者帳戶同步 DSRM 密碼，以使其更容易記住。 如需詳細資訊，請參閱知識庫文章[961320](https://support.microsoft.com/kb/961320)。 同步處理 DSRM 帳戶必須完成前的準備過程在樹系復原。

> [!NOTE]
> 系統管理員帳戶是 Domain Admins 與 Enterprise Admins 群組為依預設，內建的 Administrators 群組的成員。 此群組擁有網域中的所有網域控制站的完整控制權。

## <a name="determining-which-backups-to-use"></a>決定要使用哪一個備份

針對每個網域的至少兩個可寫入 Dc 定期備份讓您有數個可從中選擇的備份。 請注意，您無法使用唯讀網域控制站 (RODC) 的備份來還原可寫入 DC。 我們建議您在使用已取得失敗的項目之前的幾天的備份來還原網域控制站。 一般情況下，您必須決定 recentness 並還原資料的 safeness 之間進行取捨。 選擇較新的備份復原更有用的資料，但可能會增加重新將危險資料到已還原的樹系的風險。

還原系統狀態備份，取決於原始的作業系統和伺服器的備份。 例如，您應該還原系統狀態備份到不同的伺服器。 在此情況下，您可能會看到下列警告：

「 指定的備份是伺服器的在不同與目前。 我們不建議執行系統狀態復原的備份來替代伺服器，因為伺服器可能變成無法使用。 確定您想要用於此備份復原目前的伺服器？ 」

如果您需要將 Active Directory 還原至不同的硬體，請建立完整伺服器備份，並執行完整伺服器復原的計劃。

> [!IMPORTANT]
> 從 Windows Server 2008 開始，它不支援系統狀態備份還原到新的硬體或相同的硬體上新安裝的 Windows Server。 如果稍後在本指南建議的方式，在相同硬體上，會重新安裝 Windows Server，您就可以還原網域控制站，依此順序：
>
> 1. 若要還原的作業系統和所有檔案和應用程式執行完整伺服器都還原。
> 2. 執行系統狀態還原，才能將 SYSVOL 標示為權威使用 wbadmin.exe。
>
> 如需詳細資訊，請參閱 Microsoft 知識庫文章[249694](https://support.microsoft.com/kb/249694)。

如果失敗的發生時間不明，進一步調查，以識別備份保存在樹系的最後一個安全的狀態。 這種方法是較不建議。 因此，我們強烈建議您每天都保持健全狀況狀態的 AD DS 相關的詳細的記錄，以便全樹系失敗時，可以識別失敗的大約時間。 您也應該保留備份，以啟用更快獲得復原的本機副本。

如果已啟用 Active Directory 資源回收筒，備份的存留期會等於**deletedObjectLifetime**值，或**tombstoneLifetime**值，兩者中較少。 如需詳細資訊，請參閱 < [Active Directory 資源回收筒逐步指南](https://go.microsoft.com/fwlink/?LinkId=178657)(https://go.microsoft.com/fwlink/?LinkId=178657)。

或者，您也可以使用 Active Directory 資料庫掛接工具 (Dsamain.exe)，而且是輕量型目錄存取通訊協定 (LDAP) 工具，例如 Ldp.exe 或 Active Directory 使用者和電腦，以識別哪一個備份的最後一個安全的狀態樹系。 Active Directory 資料庫掛接工具，隨附於 Windows Server 2008 和更新版本的 Windows Server 作業系統，會公開為 LDAP 伺服器儲存在備份或快照集的 Active Directory 資料。 然後，您可以使用的 LDAP 工具來瀏覽資料。 這種方法的優點是能夠不需要您重新啟動任何 DC 在目錄服務還原模式 (DSRM) 來檢查 AD ds 備份的內容。

如需使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱[Active Directory 資料庫掛接工具逐步指南](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx)。

您也可以使用**ntdsutil 快照集**命令來建立 Active Directory 資料庫的快照集。 排程工作以定期建立快照集，您可以取得額外的 Active Directory 資料庫的複本一段時間。 若要更精確識別發生全樹系失敗時]，然後選擇 [要還原的最佳備份，您可以使用這些複本。 若要建立快照集，使用新版**ntdsutil**隨附的 Windows Server 2008 或遠端伺服器管理工具 (RSAT) 適用於 Windows Vista 或更新版本。 目標 DC 可以執行任何版本的 Windows Server。 如需使用詳細資訊**ntdsutil 快照**命令，請參閱[快照集](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx)。

## <a name="determining-which-domain-controllers-to-restore"></a>判斷要還原的網域控制站

當您決定要還原哪一個網域控制站時，簡化還原程序會是重要的因素。 建議您擁有專用的 DC，每個網域都是慣用的 DC 進行還原。 專用的還原 DC 輕鬆可靠地計劃和執行樹系復原，因為您是使用相同的來源組態用來執行還原測試。 您可以編寫指令碼復原，並不都努力應付著不同的組態，例如是否 DC 持有作業主機角色，或是否不在 GC 或 DNS 伺服器。

> [!NOTE]
> 雖然不建議以還原操作主機角色持有者為了簡單起見，某些組織可能選擇要還原另一個用於其他優點。 例如還原 RID 主機，有助於避免管理 Rid 在復原期間發生問題。  

選擇最符合下列準則的 DC:

- 是可寫入 DC。 這是必要的。

- 做為虛擬機器執行 Windows Server 2012，在支援 Vm-generationid 之 hypervisor 上的 DC。 此 DC 可以用做為來源，進行複製。
- DC 實體或虛擬網路可供存取，且最好位於資料中心。 如此一來，您可以輕鬆地隔離它從網路樹系復原。
- 有很好的完整伺服器備份 DC。 良好的備份是備份可以成功還原、 建立失敗前, 幾天，其中包含為盡可能更有用的資料。
- 已在失敗前的網域名稱系統 (DNS) 伺服器的 DC。 這樣可以節省重新安裝 DNS 所需的時間。
- 如果您也可以使用 Windows 部署服務，請選擇 未設定為使用 BitLocker 網路解除鎖定的 DC。 在此情況下，BitLocker 網路解除鎖定不支援用於從備份還原在樹系復原期間的第一個 DC。

   BitLocker 網路解除鎖定做*僅*金鑰保護裝置*無法*用於網域控制站因為這樣做會導致案例中，已部署 Windows 部署服務 (WDS) 需要的第一個 DC 的位置Active Directory 和 WDS 努力才能解除鎖定。 但是，第一個 DC 在還原之前，Active Directory 尚無法使用 wds，因此無法解除鎖定。

   若要判斷是否 DC 已設定為使用 BitLocker 網路解除鎖定，請檢查下列登錄機碼中，已識別的網路解除鎖定憑證：

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

維護安全性程序處理，或還原備份的檔案，包括 Active Directory 時。 急迫性所附的樹系復原不小心可能怎麼也發掘安全性最佳作法。 如需詳細資訊，請參閱中的 「 建立網域控制站備份和還原策略 」 一節[保護 Active Directory 安裝和 Day-to-Day 作業的最佳做法指南：第二部](https://technet.microsoft.com/library/bb727066.aspx)。

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>識別目前的樹系結構和 DC 函式

判斷目前的樹系結構識別樹系中的所有網域。 請在每個網域中，尤其是有備份的網域控制站和可複製的來源虛擬網域控制站的所有網域控制站的清單。 樹系根網域的網域控制站清單將會最重要的因為您會先復原這個網域。 還原樹系根網域之後，您可以使用 Active Directory 的嵌入式管理單元來取得其他網域、 Dc 及樹系中的站台的清單。

準備在網域中，顯示每個 DC 的函式的資料表，如下列範例所示。 這有助於還原回為失敗前設定樹系復原之後。

|DC 名稱|作業系統|FSMO|GC|RODC|備份|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|架構主機網域命名主機|是|否|是|否|否|是|是|  
|DC_2|Windows Server 2012|None|是|否|是|是|否|是|是|  
|DC_3|Windows Server 2012|基礎結構主機|否|否|否|是|是|是|是|  
|DC_4|Windows Server 2012|PDC 模擬器，RID 主機|是|否|否|否|否|是|否|  
|DC_5|Windows Server 2012|None|否|否|是|是|否|是|是|  
|RODC_1|Windows Server 2008 R2|None|是|是|是|是|是|是|否|  
|RODC_2|Windows Server 2008|None|是|是|否|是|是|是|否|  

每個網域樹系中，找出單一的可寫入 DC 具有受信任的網域的 Active Directory 資料庫的備份。 當您選擇要還原 DC 的備份時，請務必小心。 如果大約已知的日期和失敗的原因，一般建議是使用已在該日期之前的幾天的備份。
  
在此範例中，有四個備份的候選項目：DC_1、 DC_2、 DC_4 和 DC_5。 這些備份的候選項目，您要還原其中之一。 建議的 DC 是 DC_5，原因如下：  

- 它符合使用它做為虛擬化 DC 複製，來源是的需求、 支援 VM 世代識別碼 」 的 hypervisor 上的虛擬 DC 執行軟體，可為執行 Windows Server 2012 複製 （或如果不可以是複製可以移除的d) 中。 在還原之後，PDC 模擬器角色會以取回伺服器，而且可以加入網域的 Cloneable Domain Controllers 群組。  
- 它會執行 Windows Server 2012 的完整安裝。 執行 Server Core 安裝的 DC 可以是較不方便做為目標進行復原。  
- 它是 DNS 伺服器。 因此，DNS 並沒有重新安裝。  

> [!NOTE]
> 因為 DC_5 不是通用類別目錄伺服器，它也有一項優點在於不需要在還原之後，會移除通用類別目錄。 但無論 DC 也是通用類別目錄伺服器並不是決定性因素因為從 Windows Server 2012 開始，所有網域控制站為通用類別目錄伺服器的預設值，並移除它之後還原為建議在樹系新增通用類別目錄復原在任何情況下處理。  

## <a name="recover-the-forest-in-isolation"></a>復原中隔離的樹系

建議的案例是關閉所有可寫入的網域控制站之前先還原的網域控制站回復到生產環境。 這可確保任何危險的資料不會複寫回復原樹系。 它會關閉所有操作主機角色持有者特別重要。  

> [!NOTE]
> 可能會發生您用來移動您打算復原到隔離網路的每個網域，同時允許其他網域控制站，以維持線上為了盡量減少系統停機時間的第一個 DC 的情況。 例如，如果您要從失敗的結構描述升級復原，您可以選擇保留在隔離狀態執行修復步驟時，生產網路上執行的網域控制站。

如果您正在執行虛擬網域控制站，您可以前往它們的虛擬網路與生產網路隔離，您將在其中執行修復。 將虛擬網域控制站移至不同的網路提供兩個優點：  

- 造成樹系復原，因為它們是隔離的問題再次發生無法復原的網域控制站。  
- 虛擬化 DC 複製可以對個別的網路，以便可以執行重要的數目的 Dc 並測試之前他們就可以享受回復到生產環境網路。

如果您在實體硬體上執行的 Dc，中斷連接您計畫還原的樹系根網域中的第一個 DC 的網路纜線。 可能的話，也中斷連線所有其他網域控制站的網路纜的線。 這可防止網域控制站複寫，如果它們不小心在樹系修復程序啟動。  

大型的樹系，則會分散到多個位置，很難保證所有的可寫入網域控制站已關閉。 基於這個理由，復原步驟 — 例如重設 krbtgt 帳戶與電腦帳戶除了中繼資料清理，旨在確保復原的可寫入網域控制站，不會複寫有危險的可寫入 Dc (以防有些仍保持線上狀態中樹系）。  
  
不過，只能藉由離線可寫入網域控制站可以確保不會發生複寫。 因此，可能的話，您應該部署遠端管理技術，可協助您關閉並實際隔離的樹系復原期間的可寫入網域控制站。  
  
Rodc 可以繼續運作而可寫入網域控制站已離線。 其他 DC 完全沒有會直接從任何的 RODC 複寫任何變更，特別是，沒有結構描述或設定容器進行變更，讓它們在復原期間不會為可寫入網域控制站相同的風險。 所有可寫入網域控制站已復原和線上之後，您應該重建所有 Rodc。  
  
Rodc 會繼續以允許存取時的復原作業以平行方式時，會其各自的網站中快取的本機資源。 不會快取在 RODC 的本機資源會有驗證要求轉送至可寫入 DC。 這些要求會失敗，因為可寫入網域控制站已離線。 直到您復原的可寫入網域控制站，也將無法運作如密碼變更的某些作業。  
  
如果您使用的中樞輪輻網路架構，您可以先專心在復原中中樞站台的可寫入 Dc。 稍後，您可以重建中遠端站台的 Rodc。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原-必要條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
