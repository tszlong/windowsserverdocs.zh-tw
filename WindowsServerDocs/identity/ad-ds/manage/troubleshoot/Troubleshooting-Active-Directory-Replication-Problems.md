---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: 進行 Active Directory 複寫問題疑難排解
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869419"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>進行 Active Directory 複寫問題疑難排解

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 複寫問題可以有數個不同的來源。 例如，網域名稱系統 (DNS) 問題、 網路問題或安全性問題可能全部會導致 Active Directory 複寫失敗。 

本主題的其餘部分將說明工具和一般的方法，可修正 Active Directory 複寫錯誤。 下列子主題涵蓋徵狀、 原因和解決特定的複寫錯誤：

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>簡介和疑難排解 Active Directory 複寫的資源

輸入或輸出複寫失敗會造成代表複寫拓撲、 複寫排程、 網域控制站、 使用者、 電腦、 密碼、 安全性群組、 群組成員資格，以及群組原則 設為不一致的 Active Directory 物件之間的網域控制站。 目錄不一致性和複寫失敗會導致作業失敗或不一致的結果，取決於作業中，會連絡網域控制站可以防止應用程式的群組原則和存取控制 」 權限。 Active Directory 網域服務 (AD DS) 取決於網路連線、 名稱解析、 驗證和授權、 目錄資料庫、 複寫拓撲中，以及複寫引擎。 不明顯的複寫問題的根本原因時，判斷之間許多可能的原因造成，需要有系統地消除可能的原因。

若要協助您監視複寫及診斷錯誤 UI 型工具，請參閱[Active Directory 複寫狀態的工具](https://www.microsoft.com/download/details.aspx?id=30005)

完整的文件描述如何使用 Repadmin 工具進行疑難排解 Active Directory 複寫功能適用於;請參閱[監視和疑難排解 Active Directory 複寫使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

如需 Active Directory 複寫的運作方式的資訊，請參閱下列技術參照：

- [Active Directory 複寫模型技術參考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Directory 複寫拓撲技術參照](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具解決方案的建議

在理想情況下，紅色 （錯誤） 和目錄服務事件記錄檔中的黃色 （警告） 事件建議的來源或目的地網域控制站造成複寫失敗的特定條件約束。 如果事件訊息所建議的解決方案的步驟，請嘗試在事件中所述的步驟。 Repadmin 工具和其他診斷工具也提供可協助您解決複寫失敗的資訊。 

如需使用 Repadmin 進行複寫問題疑難排解的詳細資訊，請參閱[監視和疑難排解 Active Directory 複寫使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>把刻意中斷或硬體故障

有時會發生複寫錯誤，因為刻意中斷。 例如，當您疑難排解 Active Directory 複寫問題時，排除刻意中斷連線和硬體失敗或升級第一次。

### <a name="intentional-disconnections"></a>刻意中斷連線

如果報告正在嘗試使用已在預備網站中建立，並等候其最終的生產網站 （遠端站台，例如分公司辦公室中的部署目前離線網域控制站複寫網域控制站的複寫錯誤)，您便可以解釋這些複寫錯誤。 若要避免將網域控制站與複寫拓撲很長，這會導致連續錯誤，直到重新連線的網域控制站，請考慮一開始將這類電腦新增為成員伺服器，並使用 「 從媒體 （安裝IFM) 方法來安裝 Active Directory 網域服務 (AD DS)。 您可以使用 Ntdsutil 命令列工具來建立安裝媒體，您可以將儲存在卸除式媒體 （CD、 DVD 或其他媒體），然後運送到目的地站台。 然後，您可以使用安裝媒體安裝 AD DS 站台上，而不需要使用複寫的網域控制站上。 

### <a name="hardware-failures-or-upgradestitle"></a>硬體故障或進行升級</title>

如果因為硬體失敗 （例如，主機板、 磁碟子系統或硬碟失敗） 而發生的複寫問題，通知伺服器擁有者，如此即可解決硬體問題。

定期的硬體升級也可能會導致中斷服務的網域控制站。 請確定您的伺服器擁有者已事先通訊，例如中斷好的系統。

### <a name="firewall-configuration"></a>防火牆組態

根據預設，Active Directory 複寫遠端程序呼叫 (Rpc) 可能會發生動態可用的連接埠透過 RPC 端點對應程式 (RPCSS) 連接埠 135。 請確定 具有進階安全性的 Windows 防火牆與其他防火牆已正確設定以供複寫。 指定 Active Directory 複寫和連接埠設定的連接埠的相關資訊，請參閱[224196 Microsoft 知識庫文件編號](https://go.microsoft.com/fwlink/?LinkId=22578)。 

如需 Active Directory 複寫使用的通訊埠資訊，請參閱[Active Directory 複寫的工具和設定](https://go.microsoft.com/fwlink/?LinkId=123774)。

如需管理 Active Directory 複寫在防火牆上的資訊，請參閱[在防火牆上的 Active Directory 複寫](https://go.microsoft.com/fwlink/?LinkId=123775)。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>回應的過期的伺服器執行 Windows 2000 Server 的失敗

如果執行 Windows 2000 Server 的網域控制站失敗的時間超過標記存留時間中的天數，解決方案就是一律相同： 

1. 從公司網路的伺服器移到私人網路中。
2. 強制移除 Active Directory 或重新安裝作業系統。
3. 使無法恢復伺服器物件，則您可以移除 Active Directory 伺服器中繼資料。 

您可以使用指令碼來清理伺服器中繼資料，在大部分的 Windows 作業系統上。 如需使用此指令碼的資訊，請參閱[移除 Active Directory 網域控制站中繼資料](https://go.microsoft.com/fwlink/?LinkID=123599)。 

根據預設，會刪除的 NTDS 設定物件會自動 14 天的期間內恢復。 因此，如果您沒有移除伺服器中繼資料 （使用 Ntdsutil 或執行中繼資料清除先前所述的指令碼），在目錄中，系統會提示進行的複寫嘗試恢復伺服器中繼資料。 在此情況下，錯誤會持續記錄的結果無法以遺漏的網域控制站進行複寫。

## <a name="root-causes"></a>根本原因

如果您排除刻意中斷連線、 硬體故障及過期的 Windows 2000 網域控制站時，複寫問題的其餘部分幾乎都有下列的根本原因：

- 網路連線：網路連線可能會無法使用，或未正確設定網路設定。
- 名稱解析：DNS 設定錯誤會造成複寫失敗的常見原因。
- 驗證和授權：驗證和授權的問題會導致 「 拒絕存取 」 錯誤，當網域控制站嘗試連線至其複寫夥伴。
- 目錄資料庫 （市集）：目錄資料庫可能無法跟上複寫逾時的速度不夠快處理交易。
- 複寫引擎：如果站台間複寫排程太短，複寫佇列可能太大而無法處理所需的輸出複寫排程的時間。 在此情況下，某些變更的複寫可以是已無限期可能停止，夠長，超過標記存留期。
- 複寫拓撲：網域控制站必須對應到實際廣域網路 (WAN) 或虛擬私人網路 (VPN) 連線的 AD DS 中具有站台間的連結。 如果您不會受到您網路的實際站台拓撲的 AD DS 複寫拓撲中建立物件，需要設定不正確的拓撲的複寫就會失敗。

## <a name="general-approach-to-fixing-problems"></a>若要修正問題的一般方法

使用下列的一般方法，來修正複寫問題： 

1. 每日、 監視複寫健全狀況，或使用 Repadmin.exe，每日擷取複寫狀態。
2. 嘗試及時解決任何已回報的失敗，方法是使用事件訊息和本指南所述的方法。 如果軟體可能會造成問題，解除安裝軟體，然後再繼續使用其他解決方案。
3. 如果無法解析的問題，導致複寫失敗的任何已知的方法，從伺服器移除 AD DS，然後重新安裝 AD DS。 如需有關重新安裝 AD DS 的詳細資訊，請參閱 <<c0> [ 解除委任網域控制站](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果伺服器連線到網路時，無法像正常移除 AD DS，請使用下列方法之一來解決問題：

   - 強制清理伺服器中繼資料，AD DS 移除在目錄服務還原模式 (DSRM)，然後再重新安裝 AD DS。
   - 重新安裝作業系統，並重建的網域控制站。

如需有關如何強制移除 AD DS 的詳細資訊，請參閱 <<c0> [ 強制移除網域控制站](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 擷取複寫狀態</title>

複寫狀態會是重要的差異，供您評估目錄服務的狀態。 如果複寫運作正常未發生錯誤，就會知道網域控制站，都在線上。 您也了解下列的系統和服務能夠正常運作：


- DNS 基礎結構
- Kerberos 驗證通訊協定
- Windows Time 服務 (W32time)
- 遠端程序呼叫 (RPC)
- 網路連線

使用 Repadmin 來監控複寫狀態每日執行的命令，會評估您的樹系中的所有網域控制站的複寫狀態。 程序會產生您可以在 Microsoft Excel 和複寫失敗的篩選器中開啟.csv 檔案。

您可以使用下列程序來擷取樹系中的所有網域控制站的複寫狀態。 

需求

若要完成此程序，至少需要 **Enterprise Admins** 的成員資格或同等資格。 

工具：

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>若要產生的網域控制站的 repadmin /showrepl 試算表

1. 系統管理員身分開啟命令提示字元：在 開始 功能表中，以滑鼠右鍵按一下 命令提示字元中，然後按一下 以系統管理員身分執行 如果出現 [使用者帳戶控制] 對話方塊中，提供企業系統管理員認證，如有必要，，然後按一下 [繼續]。
2. 在命令提示字元中，輸入下列命令，並按 ENTER: `repadmin /showrepl * /csv > showrepl.csv`
3. 開啟 Excel。
4. 按一下 Office 按鈕，按一下開啟、 瀏覽至 showrepl.csv，，然後按一下 開啟。
5. 隱藏或刪除資料行 A 與傳輸類型資料行，如下所示：
6. 選取您想要隱藏或刪除的資料行。

   - 若要隱藏資料行，以滑鼠右鍵按一下資料行，然後按一下 隱藏。
   - 若要刪除的資料行，以滑鼠右鍵按一下選取的資料行，然後按一下 [刪除]

7. 選取資料列 1 資料行標題列下方。 在 [檢視] 索引標籤上按一下凍結窗格，然後按一下凍結頂端資料列。
8. 選取整個試算表。 在 資料 索引標籤中，按一下 篩選器。
9. 在 [上次成功時間] 欄中，按一下向下箭號，，然後按一下遞增排序。
10. 在來源 DC 欄中，按一下向下箭號篩選器，指向 文字篩選，然後按一下 自訂篩選。
11. 在 自訂自動篩選 對話方塊中下按一下 不包含的位置，顯示資料列。 在相鄰的文字方塊中，輸入<userInput>del</userInput>從檢視排除的結果已刪除的網域控制站。
12. 上次失敗時間 欄中，但使用的值不相等，然後輸入值 0，重複步驟 11。
13. 解決複寫失敗。

樹系中每個網域控制站，試算表會顯示來源複寫協力電腦，時間上一次發生複寫，以及每項命名內容 （目錄磁碟分割） 最後一個複寫失敗，發生的時間。 藉由在 Excel 中使用自動篩選，您可以檢視複寫健康狀態的工作，網域控制站失敗，網域控制站或網域控制站的最低或大部分目前，，您可以看到複寫的複寫協力電腦已成功。

## <a name="replication-problems-and-resolutions"></a>複寫問題和解決方式

複寫問題會回報事件訊息中，並在各種應用程式或服務嘗試執行作業時，會發生的錯誤訊息。 在理想情況下，監視應用程式，或當您擷取複寫狀態時，會收集這些訊息。

大部分的複寫問題都會列在目錄服務事件記錄檔中記錄的事件訊息。 複寫問題可能也會識別出的輸出中的錯誤訊息的形式<system>repadmin /showrepl</system>命令。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl 錯誤訊息指出複寫問題

若要識別 Active Directory 複寫問題，請使用<system>repadmin /showrepl</system>命令上, 一節中所述。 下表顯示此命令會產生，以及主題連結，提供解決方案的錯誤與錯誤的根本原因的錯誤訊息。

|Repadmin 錯誤|根本原因|方案|
| --- | --- | --- |
|與這個上次複寫之後的時間伺服器已超過標記存留期。|網域控制站無法與具名的來源網域控制站很長足夠已標記刪除，刪除複寫，且記憶體回收從 AD DS 的輸入的複寫。|事件識別碼 2042年:它已經過太久複寫此機器|
|沒有輸入鄰近項目。|如果沒有任何項目出現在 repadmin /showrepl 所產生的輸出的 「 輸入芳鄰 」 一節中，網域控制站無法建立與另一個網域控制站的複寫連結。|修正複寫連線問題 (事件識別碼 1925)| 
|存取遭到拒絕。|有兩個網域控制站之間的複寫連結，但無法正確地執行複寫，因為發生驗證錯誤。|修正複寫安全性問題| 
|< 日期-時間 > 在上次嘗試失敗，發生 「 目標帳戶名稱不正確。 」|這個問題可以連線、 DNS 或驗證問題相關。 如果這是 DNS 錯誤，本機網域控制站無法解析的全域唯一識別碼 (GUID) 為基礎的複寫協力電腦的 DNS 名稱。|修正複寫 DNS 查閱問題 （事件識別碼 1925、 2087、 2088年） 修正複寫安全性問題修正複寫連線問題 （事件識別碼 1925年）| 
|LDAP 錯誤 49。|網域控制站電腦帳戶可能不會同步處理與金鑰發佈中心 (KDC)。|修正複寫安全性問題| 
|無法開啟 LDAP 連線到本機主機|管理工具無法連絡 AD DS。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)| 
|Active Directory 複寫已遭先占。|較高優先順序複寫要求，例如要求使用 repadmin /sync 命令手動產生已中斷作業的輸入複寫進度。|等候複寫完成。 此參考用訊息表示正常的作業。| 
|複寫回傳時，它會等候。| 網域控制站張貼複寫要求，而且正在等候回應。 複寫是從這個來源的進行中。|等候複寫完成。 此參考用訊息表示正常的作業。| 

下表列出常見的事件可能表示 Active Directory 複寫，以及根原因的問題和問題提供解決方案的主題連結發生問題。 

|事件識別碼和來源|根本原因|方案|
| --- | --- | --- | 
|1311 NTDS KCC|在 AD DS 中的複寫組態資訊不會精確地反映的實體網路拓撲。|修正複寫拓撲問題 (事件識別碼 1311)| 
|1388 NTDS 複寫|嚴格的複寫一致性不會生效，並延遲物件已複寫到網域控制站。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|1925 NTDS KCC|嘗試建立可寫入的目錄磁碟分割的複寫連結失敗。 這個事件可以有不同的原因，視錯誤而定。| 修正複寫連線問題 （事件識別碼 1925年） 修正複寫 DNS 查閱問題 （事件識別碼 1925、 2087、 2088年）| 
|1988 NTDS 複寫|本機網域控制站嘗試複寫物件不存在於本機網域控制站上，因為已刪除與已回收的來源網域控制站。 複寫不會繼續此合作夥伴具有此目錄磁碟分割，直到問題得到解決為止。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)|
|2042 NTDS 複寫|複寫不發生與此合作夥伴的標記存留期，無法繼續複寫。|修正複寫延遲物件問題 (事件識別碼 1388、1988、2042)| 
|2087 NTDS 複寫|AD DS 無法解析 IP 位址，以及失敗的複寫來源網域控制站的 DNS 主機名稱。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)| 
|2088 NTDS 複寫 |AD DS 無法解析的 IP 位址，但是成功的複寫來源網域控制站的 DNS 主機名稱。|修正複寫 DNS 查閱問題 (事件識別碼 1925、2087、2088)|
|5805 網路登入|機器帳戶驗證失敗，這種情形通常因相同的電腦名稱的多個執行個體或並未複寫到每個網域控制站的電腦名稱。|修正複寫安全性問題| 

如需有關複寫概念的詳細資訊，請參閱 < [Active Directory 複寫技術](https://go.microsoft.com/fwlink/?LinkId=41950)。
  
## <a name="next-steps"></a>後續步驟

如需詳細資訊，包括支援特有的錯誤碼文件請參閱支援文章：[如何疑難排解常見的 Active Directory 複寫錯誤](https://support.microsoft.com/help/3108513)
