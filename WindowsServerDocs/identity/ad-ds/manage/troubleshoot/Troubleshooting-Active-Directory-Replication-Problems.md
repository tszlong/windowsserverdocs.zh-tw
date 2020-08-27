---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: 進行 Active Directory 複寫問題疑難排解
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2ceb13e3729310e01063c0c5c3694806b1565363
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941488"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>進行 Active Directory 複寫問題疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 複寫問題可能有數個不同的來源。 例如，網域名稱系統 (DNS) 問題、網路問題或安全性問題，可能會導致 Active Directory 複寫失敗。

本主題的其餘部分將說明用來修正 Active Directory 複寫錯誤的工具和一般方法。 下列子主題涵蓋徵兆、原因，以及如何解決特定的複寫錯誤：

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>針對 Active Directory 複寫進行疑難排解的簡介和資源

輸入或輸出複寫失敗會導致代表複寫拓撲、複寫排程、網域控制站、使用者、電腦、密碼、安全性群組、群組成員資格和群組原則的 Active Directory 物件在網域控制站之間不一致。 目錄不一致和複寫失敗，會導致作業失敗或不一致的結果（視操作所連接的網域控制站而定），而且可能會導致群組原則和存取控制許可權的應用程式無法運作。 Active Directory Domain Services (AD DS) 取決於網路連線能力、名稱解析、驗證和授權、目錄資料庫、複寫拓撲，以及複寫引擎。 當複寫問題的根本原因無法立即察覺時，判斷許多可能原因的原因，需要系統地排除可能的原因。

如需以 UI 為基礎的工具來協助監視複寫和診斷錯誤，請參閱 [Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)

如需說明如何使用 Repadmin 工具進行疑難排解的完整檔，Active Directory 複寫可供使用;請參閱 [使用 Repadmin 進行 Active Directory 複寫的監視和疑難排解](https://go.microsoft.com/fwlink/?LinkId=122830)。

如需 Active Directory 複寫運作方式的詳細資訊，請參閱下列技術參考：

- [Active Directory 複寫模型技術參考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Director 複寫拓撲技術參考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具解決方案建議

在理想的情況下，紅色的 (錯誤) 和黃色的 (警告) 目錄服務事件記錄檔中的事件會建議在來源或目的地網域控制站上造成複寫失敗的特定條件約束。 如果事件訊息建議解決方案的步驟，請嘗試此事件中所述的步驟。 Repadmin 工具和其他診斷工具也會提供可協助您解決複寫失敗的資訊。

如需使用 Repadmin 針對複寫問題進行疑難排解的詳細資訊，請參閱 [使用 Repadmin 監視和疑難排解 Active Directory](https://go.microsoft.com/fwlink/?LinkId=122830)複寫。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>排除蓄意中斷或硬體故障

有時會因蓄意中斷而發生複寫錯誤。 例如，當您對 Active Directory 複寫問題進行疑難排解時，請先排除刻意中斷連線和硬體故障或升級的問題。

### <a name="intentional-disconnections"></a>蓄意中斷連線

如果網域控制站報告複寫錯誤，而該網域控制站嘗試複寫的網域控制站已內建于預備網站，且目前正在離線中等待其部署在最終生產網站中 (遠端網站（例如分公司) ），您可以考慮這些複寫錯誤。 為了避免將網域控制站與複寫拓撲分開，這會在網域控制站重新連線之前造成連續錯誤，請考慮將這類電腦一開始新增為成員伺服器，並使用從媒體安裝 (IFM) 方法來安裝 Active Directory Domain Services (AD DS) 。 您可以使用 Ntdsutil 命令列工具來建立安裝媒體，以便儲存在卸載式媒體上， (CD、DVD 或其他媒體) ，然後送至目的地網站。 然後，您可以使用安裝媒體，在網站的網域控制站上安裝 AD DS，而不需要使用複寫。

### <a name="hardware-failures-or-upgradestitle"></a>硬體故障或升級</title>

如果因為硬體失敗而發生複寫問題 (例如，主機板、磁片子系統或硬碟) 失敗，請通知伺服器擁有者，以便能夠解決硬體問題。

定期硬體升級也會導致網域控制站無法服務。 確定您的伺服器擁有者有很好的系統，可以事先進行這類的中斷通訊。

### <a name="firewall-configuration"></a>防火牆設定

根據預設，Active Directory replication 遠端程序呼叫 (的) Rpc 會透過埠135上的 RPC 端點對應程式 (RPCSS) ，以動態方式在可用的埠上進行。 請確定已正確設定具有 Advanced Security 和其他防火牆的 Windows 防火牆，以允許複寫。 如需有關指定 Active Directory 複寫和埠設定之埠的詳細資訊，請參閱 [Microsoft 知識庫中的文章 224196](https://go.microsoft.com/fwlink/?LinkId=22578)。

如需 Active Directory 複寫所使用之埠的詳細資訊，請參閱 [Active Directory 複製工具和設定](https://go.microsoft.com/fwlink/?LinkId=123774)。

如需有關透過防火牆管理 Active Directory 複寫的詳細資訊，請參閱透過 [防火牆 Active Directory](https://go.microsoft.com/fwlink/?LinkId=123775)複寫。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>回應執行 Windows 2000 伺服器的過期伺服器失敗

如果執行 Windows 2000 伺服器的網域控制站的時間超過標記存留期的天數，則解決方案永遠相同：

1. 將伺服器從公司網路移至私人網路。
2. 請強制移除 Active Directory 或重新安裝作業系統。
3. 從 Active Directory 移除伺服器中繼資料，讓伺服器物件無法恢復。

您可以使用腳本來清除大部分 Windows 作業系統上的伺服器中繼資料。 如需使用此腳本的詳細資訊，請參閱 [移除 Active Directory 網網域控制站中繼資料](https://go.microsoft.com/fwlink/?LinkID=123599)。

根據預設，已刪除的 NTDS 設定物件會自動進行一段14天的時間。 因此，如果您沒有移除伺服器中繼資料 (使用 Ntdsutil 或先前提及的腳本來執行中繼資料清除) ，伺服器中繼資料會在目錄中復原，以提示進行複寫嘗試。 在此情況下，因為無法使用遺失的網域控制站進行複寫，所以會持續記錄錯誤。

## <a name="root-causes"></a>根源

如果您排除刻意中斷連線、硬體故障和過期的 Windows 2000 網域控制站，則複寫問題的其餘部分幾乎一律會有下列其中一個根本原因：

- 網路連線能力：網路連線可能無法使用，或網路設定未正確設定。
- 名稱解析： DNS 錯誤配置是複寫失敗的常見原因。
- 驗證和授權：當網域控制站嘗試連接至其複寫夥伴時，驗證和授權問題會導致「拒絕存取」錯誤。
- 目錄資料庫 (存放區) ：目錄資料庫可能無法以足夠的速度處理交易，以趕上複寫超時時間。
- 複寫引擎：如果網站間複寫排程太短，則複寫佇列可能太大，而無法在輸出複寫排程所需的時間內處理。 在此情況下，某些變更的複寫可能會無限期停止，且長度足以超過標記存留期。
- 複寫拓撲：網域控制站必須有 AD DS 中的站上連結，以對應至 (WAN) 或虛擬私人網路 (VPN) 連線的網路。 如果您在網路的實際網站拓撲不支援的複寫拓撲 AD DS 中建立物件，則需要設定錯誤拓撲的複寫會失敗。

## <a name="general-approach-to-fixing-problems"></a>修正問題的一般方法

使用下列一般方法來修正複寫問題：

1. 每天監視複寫健康情況，或使用 Repadmin.exe 每日取出複寫狀態。
2. 使用事件訊息和本指南中所述的方法，嘗試及時解決任何回報的錯誤。 如果軟體可能造成問題，請先卸載軟體，再繼續進行其他解決方案。
3. 如果任何已知的方法都無法解決導致複寫失敗的問題，請從伺服器移除 AD DS，然後重新安裝 AD DS。 如需重新安裝 AD DS 的詳細資訊，請參閱 [解除委任網域控制站](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果在伺服器連線到網路時無法正常移除 AD DS，請使用下列其中一種方法來解決問題：

   - 在目錄服務還原模式中強制 AD DS 移除 (DSRM) 、清除伺服器中繼資料，然後重新安裝 AD DS。
   - 重新安裝作業系統，並重建網域控制站。

如需強制移除 AD DS 的詳細資訊，請參閱 [強制移除網域控制站](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 取出複寫狀態</title>

複寫狀態是評估目錄服務狀態的重要方法。 如果複寫正常運作且沒有錯誤，您就會知道線上的網域控制站。 您也知道下列系統和服務可正常運作：


- DNS 基礎結構
- Kerberos 驗證通訊協定
- Windows Time 服務 (W32time) 
- 遠端程序呼叫 (RPC)
- 網路連線

藉由執行命令來評估樹系中所有網域控制站的複寫狀態，以使用 Repadmin 每天監視複寫狀態。 此程式會產生一個 .csv 檔案，您可以在 Microsoft Excel 中開啟該檔案，並篩選複寫失敗。

您可以使用下列程式來取得樹系中所有網域控制站的複寫狀態。

規格需求

若要完成此程序，至少需要 **Enterprise Admins** 的成員資格或同等資格。

工具：

- Repadmin.exe
- Excel (Microsoft Office) 

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>為網域控制站產生 repadmin/showrepl 試算表

1. 以系統管理員身分開啟命令提示字元：在 [[開始] 功能表上，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請視需要提供 Enterprise Admins 認證，然後按一下 [繼續]。
2. 在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `repadmin /showrepl * /csv > showrepl.csv`
3. 開啟 Excel。
4. 按一下 [Office] 按鈕，按一下 [開啟]，流覽至 showrepl.csv，然後按一下 [開啟]。
5. 隱藏或刪除資料行 A 及傳輸類型資料行，如下所示：
6. 選取您要隱藏或刪除的資料行。

   - 若要隱藏資料行，請以滑鼠右鍵按一下資料行，然後按一下 [隱藏]。
   - 若要刪除資料行，請在選取的資料行上按一下滑鼠右鍵，然後按一下 [刪除]。

7. 選取資料行標題列下方的第1列。 在 [視圖] 索引標籤上，按一下 [凍結窗格]，然後按一下 [凍結頂端資料列]。
8. 選取整個試算表。 在 [資料] 索引標籤上，按一下 [篩選]。
9. 在 [上次成功時間] 資料行中，按一下向下箭號，然後按一下 [昇冪]。
10. 在 [來源 DC] 資料行中，按一下 [篩選向下] 箭號，指向 [文字篩選]，然後按一下 [自訂篩選]。
11. 在 [自訂自動篩選] 對話方塊的 [顯示資料列位置] 下，按一下 [不包含]。 在連續的文字方塊中，輸入 <userInput>del</userInput> 以避免查看已刪除網域控制站的結果。
12. 針對 [上次失敗時間] 資料行重複步驟11，但使用 [不等於] 值，然後輸入值0。
13. 解決複寫失敗。

針對樹系中的每個網域控制站，試算表會顯示來源複寫夥伴、上次發生複寫的時間，以及每個命名內容 (目錄分割) 的上次複寫失敗時間。 藉由使用 Excel 中的 [自動篩選]，您可以只查看工作網域控制站的複寫健全狀況、僅失敗的網域控制站，或是最少或最新的網域控制站，而且您可以看到複寫的夥伴複寫成功。

## <a name="replication-problems-and-resolutions"></a>複寫問題和解決方式

當應用程式或服務嘗試操作時，事件訊息和各種錯誤訊息中會報告複寫問題。 在理想情況下，這些訊息是由您的監視應用程式或在您取得複寫狀態時所收集。

在目錄服務事件記錄檔中記錄的事件訊息中，會識別大部分的複寫問題。 您也可以在 <system>repadmin/showrepl</system> 命令的輸出中，以錯誤訊息的形式來識別複寫問題。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl 指出複寫問題的錯誤訊息

若要識別 Active Directory 複寫問題，請使用 <system>repadmin/showrepl</system> 命令，如上一節中所述。 下表顯示此命令所產生的錯誤訊息，以及錯誤的根本原因，以及提供錯誤解決方案的主題連結。

|Repadmin 錯誤|根本原因|解決方案|
| --- | --- | --- |
|自上次複寫此伺服器以來的時間已超過標記存留期。|網域控制站的輸入複寫具有已命名來源網域控制站的失敗，夠長，足以讓您從 AD DS 中將刪除作業重寫、複寫和垃圾收集。|事件識別碼 2042：自此電腦複寫後已過太久|
|沒有輸入相鄰專案。|如果在 repadmin/showrepl 產生之輸出的 [輸入鄰居] 區段中沒有出現任何專案，則網域控制站無法與另一個網域控制站建立複寫連結。|修正複寫連線問題 (事件識別碼 1925)|
|存取遭到拒絕。|兩個網域控制站之間存在複寫連結，但因為驗證失敗，所以無法正確執行複寫。|修正複寫安全性問題|
|上次嘗試 <日期時間> 失敗，因為「目標帳戶名稱不正確」。|此問題可能與連線能力、DNS 或驗證問題有關。 如果這是 DNS 錯誤，本機網域控制站無法解析其複寫夥伴的 (GUID) DNS 名稱的全域唯一識別碼。|修正複寫 DNS 查閱問題 (事件識別碼1925、2087、2088) 修正複寫安全性問題，以修正複寫連線問題 (事件識別碼 1925) |
|LDAP 錯誤49。|網域控制站電腦帳戶可能未與金鑰發佈中心 (KDC) 同步。|修正複寫安全性問題|
|無法開啟本機主機的 LDAP 連接|管理工具無法聯絡 AD DS。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)|
|Active Directory 複寫已被搶先。|輸入複寫的進度中斷了較高優先順序的複寫要求，例如使用 repadmin/sync 命令手動產生的要求。|等候複寫完成。 此參考用訊息表示正常操作。|
|複寫已張貼，正在等候。| 網域控制站已公佈複寫要求，正在等候回應。 從這個來源進行複寫。|等候複寫完成。 此參考用訊息表示正常操作。|

下表列出可能指出 Active Directory 複寫問題的常見事件，以及問題的根本原因，以及提供問題解決方案的主題連結。

|事件識別碼和來源|根本原因|解決方案|
| --- | --- | --- |
|1311 NTDS KCC|AD DS 中的複寫設定資訊無法精確反映網路的實體拓撲。|修正複寫拓撲問題 (事件識別碼 1311)|
|1388 NTDS 複寫|Strict replication 一致性未生效，而且已將延遲物件複寫到網域控制站。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|1925 NTDS KCC|嘗試建立可寫入目錄分割的複寫連結失敗。 此事件可能會有不同的原因，視錯誤而定。| 修正複寫連接問題 (事件識別碼 1925) 修正複寫 DNS 查閱問題 (事件識別碼1925、2087、2088) |
|1988 NTDS 複寫|本機網域控制站嘗試從不存在於本機網域控制站上的來源網域控制站複寫物件，因為它可能已被刪除且已進行垃圾收集。 在問題解決之前，不會在此夥伴進行此目錄分割的複寫作業。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|2042 NTDS 複寫|此夥伴未在標記存留期內進行複寫，而且複寫無法繼續。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|2087 NTDS 複寫|AD DS 無法將來源網域控制站的 DNS 主機名稱解析為 IP 位址，且複寫失敗。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)|
|2088 NTDS 複寫 |AD DS 無法將來源網域控制站的 DNS 主機名稱解析為 IP 位址，但複寫成功。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)|
|5805 Net 登入|電腦帳戶無法進行驗證，這通常是因為相同電腦名稱稱的多個實例或電腦名稱稱未複寫到每個網域控制站。|修正複寫安全性問題|

如需有關複寫概念的詳細資訊，請參閱 [Active Directory 複寫技術](https://go.microsoft.com/fwlink/?LinkId=41950)。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，包括錯誤碼專屬的支援文章，請參閱支援文章： [如何針對常見的 Active Directory 複寫錯誤進行疑難排解](https://support.microsoft.com/help/3108513)
