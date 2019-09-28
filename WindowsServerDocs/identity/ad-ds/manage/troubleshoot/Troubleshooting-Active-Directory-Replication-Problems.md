---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: 進行 Active Directory 複寫問題疑難排解
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409066"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>進行 Active Directory 複寫問題疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 複寫問題可以有數個不同的來源。 例如，網域名稱系統（DNS）問題、網路問題或安全性問題都可能導致 Active Directory 複寫失敗。 

本主題的其餘部分將說明用來修正 Active Directory 複寫錯誤的工具和一般方法。 下列子主題涵蓋徵兆、原因，以及如何解決特定複寫錯誤：

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>疑難排解 Active Directory 複寫的簡介和資源

輸入或輸出複寫失敗會導致代表複寫拓撲、複寫排程、網域控制站、使用者、電腦、密碼、安全性群組、群組成員資格和群組原則的 Active Directory 物件不一致在網域控制站之間。 目錄不一致和複寫失敗，會造成作業失敗或不一致的結果，視與作業聯繫的網域控制站而定，並可防止應用程式群組原則和存取控制許可權。 Active Directory Domain Services （AD DS）取決於網路連接、名稱解析、驗證和授權、目錄資料庫、複寫拓撲和複寫引擎。 當複寫問題的根本原因不是立即明顯時，判斷許多可能原因之間的原因需要有系統地排除可能的原因。

如需以 UI 為基礎的工具，以協助監視複寫和診斷錯誤，請參閱[Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)

如需完整的檔，說明如何使用 Repadmin 工具進行疑難排解 Active Directory 複寫功能;請參閱[使用 Repadmin 監視和疑難排解 Active Directory](https://go.microsoft.com/fwlink/?LinkId=122830)複寫。

如需有關 Active Directory 複寫運作方式的詳細資訊，請參閱下列技術參考：

- [Active Directory 複寫模型技術參考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Directory 複寫拓撲技術參考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具解決方案建議

在理想的情況下，目錄服務事件記錄檔中的紅色（錯誤）和黃色（警告）事件，會建議在來源或目的地網域控制站上導致複寫失敗的特定條件約束。 如果事件訊息建議解決方案的步驟，請嘗試執行事件中所述的步驟。 Repadmin 工具和其他診斷工具也提供可協助您解決複寫失敗的資訊。 

如需使用 Repadmin 針對複寫問題進行疑難排解的詳細資訊，請參閱[使用 Repadmin 監視和疑難排解 Active Directory](https://go.microsoft.com/fwlink/?LinkId=122830)複寫。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>排除蓄意中斷或硬體故障

有時候複寫錯誤是因為刻意中斷所造成。 例如，當您針對 Active Directory 複寫問題進行疑難排解時，請先排除刻意中斷連線和硬體失敗，或先進行升級。

### <a name="intentional-disconnections"></a>蓄意中斷連線

如果複寫錯誤是由嘗試複寫的網域控制站所報告，而該控制器已內建在預備網站中，而且目前正在離線等候其部署在最後一個生產網站（例如分公司），您可以考慮這些複寫錯誤。 若要避免長時間將網域控制站與複寫拓撲隔開，這會造成連續錯誤，直到網域控制站重新連線為止，請考慮一開始將這類電腦新增為成員伺服器，並使用 [從媒體安裝] （IFM）方法來安裝 Active Directory Domain Services （AD DS）。 您可以使用 Ntdsutil 命令列工具來建立可儲存在卸載式媒體（CD、DVD 或其他媒體）上的安裝媒體，並寄送到目的地網站。 然後，您可以使用安裝媒體在網站的網域控制站上安裝 AD DS，而不需要使用複寫。 

### <a name="hardware-failures-or-upgradestitle"></a>硬體故障或升級 @ no__t-0

如果由於硬體故障而導致複寫問題（例如，主機板、磁片子系統或硬碟失敗），請通知伺服器擁有者，讓硬體問題得以解決。

定期硬體升級也會導致網域控制站無法服務。 請確定您的伺服器擁有者有一個很好的系統，可以事先進行這類中斷的溝通。

### <a name="firewall-configuration"></a>防火牆設定

根據預設，Active Directory 複寫遠端程序呼叫（Rpc）會透過埠135上的 RPC 端點對應程式（RPCSS），以動態方式在可用的埠上執行。 請確定已正確設定 [具有 Advanced Security 的 Windows 防火牆] 和其他防火牆，以允許進行複寫。 如需指定 Active Directory 複寫和埠設定之通訊埠的詳細資訊，請參閱[Microsoft 知識庫中的文章 224196](https://go.microsoft.com/fwlink/?LinkId=22578)。 

如需 Active Directory 複寫所使用之通訊埠的詳細資訊，請參閱[Active Directory 複寫工具和設定](https://go.microsoft.com/fwlink/?LinkId=123774)。

如需有關透過防火牆管理 Active Directory 複寫的詳細資訊，請參閱透過[防火牆 Active Directory](https://go.microsoft.com/fwlink/?LinkId=123775)複寫。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>回應執行 Windows 2000 伺服器的過時伺服器失敗

如果執行 Windows 2000 Server 的網域控制站的執行時間超過標記存留期的天數，解決方案一律相同： 

1. 將伺服器從公司網路移至私人網路。
2. 請強制移除 Active Directory 或重新安裝作業系統。
3. 從 Active Directory 移除伺服器中繼資料，以便無法恢復伺服器物件。 

您可以使用腳本來清除大部分 Windows 作業系統上的伺服器中繼資料。 如需使用此腳本的相關資訊，請參閱[移除 Active Directory 網網域控制站中繼資料](https://go.microsoft.com/fwlink/?LinkID=123599)。 

根據預設，刪除的 NTDS 設定物件會自動復原14天的時間。 因此，如果您未移除伺服器中繼資料（使用 Ntdsutil 或先前所述的腳本來執行中繼資料清除），則會在目錄中恢復伺服器中繼資料，這會提示覆寫嘗試發生。 在此情況下，將會持續記錄錯誤，因為無法以遺失的網域控制站進行複寫。

## <a name="root-causes"></a>根本原因

如果您排除刻意中斷連線、硬體失敗和過時的 Windows 2000 網域控制站，則複寫問題的其餘部分幾乎一律會有下列其中一個根本原因：

- 網路連線能力：網路連線可能無法使用，或網路設定未正確設定。
- 名稱解析：DNS 錯誤配置是複寫失敗的常見原因。
- 驗證和授權：當網域控制站嘗試連線到其複寫協力電腦時，驗證和授權問題會導致「拒絕存取」錯誤。
- 目錄資料庫（存放區）：目錄資料庫的處理速度可能不夠快，而無法跟上複寫時間。
- 複寫引擎：如果網站間複寫排程太短，則複寫佇列可能太大，而無法在輸出複寫排程所需的時間內處理。 在此情況下，某些變更的複寫可能會無限期停止，長時間足以超過標記存留期。
- 複寫拓撲：網域控制站在 AD DS 中必須有可對應到真實廣域網路（WAN）或虛擬私人網路（VPN）連線的站上連結。 如果您在 AD DS 中為網路的實際網站拓朴不支援的複寫拓撲建立物件，則需要設定錯誤拓撲的複寫會失敗。

## <a name="general-approach-to-fixing-problems"></a>修正問題的一般方法

若要修正複寫問題，請使用下列一般方法： 

1. 每天監視複寫健全狀況，或使用 Repadmin 來每天取出複寫狀態。
2. 使用事件訊息和本指南中所述的方法，嘗試及時解決任何回報的失敗。 如果軟體可能造成此問題，請先卸載軟體，再繼續進行其他解決方案。
3. 如果任何已知的方法無法解析導致複寫失敗的問題，請從伺服器移除 AD DS，然後重新安裝 AD DS。 如需有關重新安裝 AD DS 的詳細資訊，請參閱[解除委任網域控制站](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果在伺服器連線到網路時，無法正常移除 AD DS，請使用下列其中一種方法來解決問題：

   - 強制移除目錄服務還原模式（DSRM）中的 AD DS，清除伺服器中繼資料，然後重新安裝 AD DS。
   - 重新安裝作業系統，並重建網域控制站。

如需強制移除 AD DS 的詳細資訊，請參閱[強制移除網域控制站](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 來取出複寫狀態 @ no__t-0

複寫狀態是評估目錄服務狀態的重要方式。 如果複寫正常運作而沒有錯誤，您就知道已上線的網域控制站。 您也知道下列系統和服務正在運作：


- DNS 基礎結構
- Kerberos 驗證通訊協定
- Windows 時間服務（W32time）
- 遠端程序呼叫（RPC）
- 網路連線

藉由執行命令來評估樹系中所有網域控制站的複寫狀態，使用 Repadmin 監視每天的複寫狀態。 程式會產生一個 .csv 檔案，您可以在 Microsoft Excel 中開啟該檔案，並篩選複寫失敗。

您可以使用下列程式來取得樹系中所有網域控制站的複寫狀態。 

需求

若要完成此程序，至少需要 **Enterprise Admins** 的成員資格或同等資格。 

工具：

- Repadmin.exe
- Excel （Microsoft Office）

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>為網域控制站產生 repadmin/showrepl 試算表

1. 以系統管理員身分開啟命令提示字元：在 [開始] 功能表上，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請提供企業系統管理員認證（如有需要），然後按一下 [繼續]。
2. 在命令提示字元中，輸入下列命令，然後按 ENTER： `repadmin /showrepl * /csv > showrepl.csv`
3. 開啟 Excel。
4. 按一下 [Office] 按鈕，按一下 [開啟]，流覽至 showrepl，然後按一下 [開啟]。
5. 隱藏或刪除資料行 A 和 [傳輸類型] 資料行，如下所示：
6. 選取您想要隱藏或刪除的資料行。

   - 若要隱藏資料行，請以滑鼠右鍵按一下資料行，然後按一下 [隱藏]。
   - 若要刪除資料行，請以滑鼠右鍵按一下選取的資料行，然後按一下 [刪除]。

7. 在資料行標題列下方選取 [資料列 1]。 在 [View] 索引標籤上，按一下 [凍結窗格]，然後按一下 [凍結頂端資料列]。
8. 選取整份試算表。 在 [資料] 索引標籤上，按一下 [篩選]。
9. 在 [上次成功時間] 資料行中，按一下向下箭號，然後按一下 [昇冪]。
10. 在 [來源 DC] 資料行中，按一下 [篩選向下] 箭號，指向 [文字篩選]，然後按一下 [自訂篩選]。
11. 在 [自訂自動篩選] 對話方塊的 [顯示資料列位置] 底下，按一下 [不包含]。 在連續的文字方塊中，輸入<userInput>del</userInput>以消除已刪除網域控制站的結果。
12. 針對 [上次失敗時間] 資料行重複步驟11，但使用 [不等於] 值，然後輸入0值。
13. 解決複寫失敗。

對於樹系中的每個網域控制站，試算表會顯示來源複寫協力電腦、複寫上次發生的時間，以及每個命名內容（目錄分割）發生上次複寫失敗的時間。 藉由在 Excel 中使用 [自動篩選]，您可以只查看工作網域控制站的複寫健全狀況、僅針對網域控制站執行失敗，或是最少或最新的網域控制站，以及您可以查看正在複寫的複寫協力電腦。能夠.

## <a name="replication-problems-and-resolutions"></a>複寫問題和解決方式

複寫問題會在事件訊息中報告，以及在應用程式或服務嘗試操作時所發生的各種錯誤訊息中回報。 在理想情況下，這些訊息是由您的監視應用程式收集，或是在您抓取複寫狀態時發生。

在目錄服務事件記錄檔中記錄的事件訊息中，會識別大部分的複寫問題。 在<system>repadmin/showrepl</system>命令的輸出中，可能也會以錯誤訊息的形式來識別複寫問題。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl 指出複寫問題的錯誤訊息

若要識別 Active Directory 複寫問題，請使用<system>repadmin/showrepl</system>命令，如上一節所述。 下表顯示此命令所產生的錯誤訊息，以及錯誤的根本原因，以及提供錯誤解決方案的主題連結。

|Repadmin 錯誤|根本原因|方案|
| --- | --- | --- |
|上次與此伺服器複寫後的時間已超過標記存留期。|網域控制站具有名為來源網域控制站的失敗輸入複寫，已有足夠的時間可從 AD DS 進行刪除、複寫和垃圾收集。|事件識別碼2042：這台機器已複寫，因此太長|
|沒有輸入鄰近專案。|如果在 repadmin/showrepl 產生的輸出之「輸入鄰近專案」區段中沒有任何專案出現，網域控制站就無法與另一個網域控制站建立複寫連結。|修正複寫連線問題 (事件識別碼 1925)| 
|存取遭到拒絕。|兩個網域控制站之間存在複寫連結，但由於驗證失敗而無法正確執行複寫。|修正複寫安全性問題| 
|上次嘗試于 < 日期時間 > 失敗，並出現「目標帳戶名稱不正確」。|此問題可能與連線能力、DNS 或驗證問題有關。 如果這是 DNS 錯誤，本機網域控制站無法解析其複寫協力電腦的全域唯一識別碼（GUID） DNS 名稱。|修正複寫 DNS 查閱問題（事件識別碼1925、2087、2088）修正複寫安全性問題修正複寫連接問題（事件識別碼1925）| 
|LDAP 錯誤49。|網域控制站電腦帳戶可能未與金鑰發佈中心（KDC）同步處理。|修正複寫安全性問題| 
|無法開啟連至本機主機的 LDAP 連線|管理工具無法與 AD DS 連線。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)| 
|Active Directory 複寫已被搶先。|輸入複寫的進度已由較高優先順序的複寫要求中斷，例如使用 repadmin/sync 命令手動產生的要求。|等待複寫完成。 此參考用訊息表示正常操作。| 
|複寫已張貼，等待中。| 網域控制站已張貼複寫要求，而且正在等候解答。 此來源正在進行複寫。|等待複寫完成。 此參考用訊息表示正常操作。| 

下表列出可能表示 Active Directory 複寫問題的常見事件，以及問題的根本原因，以及為問題提供解決方案的主題連結。 

|事件識別碼和來源|根本原因|方案|
| --- | --- | --- | 
|1311 NTDS KCC|AD DS 中的複寫設定資訊不會正確反映網路的實體拓撲。|修正複寫拓撲問題 (事件識別碼 1311)| 
|1388 NTDS 複寫|Strict 複寫一致性不會生效，且延遲物件已複寫至網域控制站。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|1925 NTDS KCC|嘗試為可寫入的目錄磁碟分割建立複寫連結失敗。 視錯誤而定，此事件可能會有不同的原因。| 修正複寫連線問題（事件識別碼1925），以修正複寫 DNS 查閱問題（事件識別碼1925、2087、2088）| 
|1988 NTDS 複寫|本機網域控制站已嘗試從不存在於本機網域控制站上的來源網域控制站複寫物件，因為它可能已被刪除並已進行垃圾收集。 在此情況解決之前，不會為此合作夥伴的此目錄磁碟分割進行複寫。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|2042 NTDS 複寫|此合作夥伴未發生重複標記存留期的複寫，因此無法繼續複寫。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)| 
|2087 NTDS 複寫|AD DS 無法將來源網域控制站的 DNS 主機名稱解析為 IP 位址，且複寫失敗。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)| 
|2088 NTDS 複寫 |AD DS 無法將來源網域控制站的 DNS 主機名稱解析為 IP 位址，但複寫成功。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)|
|5805 Net 登入|電腦帳戶無法進行驗證，這通常是因為相同電腦名稱稱的多個實例或未複寫到每個網域控制站的電腦名稱稱所造成。|修正複寫安全性問題| 

如需複寫概念的詳細資訊，請參閱[Active Directory 複寫技術](https://go.microsoft.com/fwlink/?LinkId=41950)。
  
## <a name="next-steps"></a>後續步驟

如需詳細資訊，包括錯誤碼特定的支援文章，請參閱支援文章：[如何針對常見的 Active Directory 複寫錯誤進行疑難排解](https://support.microsoft.com/help/3108513)
