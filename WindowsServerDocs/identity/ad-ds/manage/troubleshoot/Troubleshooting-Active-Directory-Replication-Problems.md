---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: "Active Directory 複寫問題進行疑難排解"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Active Directory 複寫問題進行疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


Active Directory 複寫問題可以有數個不同的來源。 例如，網域名稱系統」(DNS) 的問題、網路問題或安全性問題所有造成 Active Directory︰ 複寫失敗。 

本主題中的其餘部分解釋工具和方法一般修正 Active Directory 複寫錯誤。 實際實驗室示範如何疑難排解複寫 Active Directory 的問題，請查看[TechNet Virtual Lab: Active Directory 複寫錯誤疑難排解](https://go.microsoft.com/?linkid=9844718)

下列主題實體鍵盤保護蓋包括症狀、原因，以及如何解析特定複寫錯誤：
   
[修正複寫延遲物件問題 (事件 Id 1388，1988，2042 年)](https://technet.microsoft.com/library/cc949124.aspx)
  
[修正複寫安全性問題](https://technet.microsoft.com/library/cc949132.aspx)

[修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年)](https://technet.microsoft.com/library/cc949133.aspx)
  
[修正複寫連接問題 (263 1925 年)](https://technet.microsoft.com/library/cc949131.aspx)
  
[修正複寫拓撲問題 (263 1311 年)](https://technet.microsoft.com/library/cc949129.aspx)
  
[確認支援 Directory 複寫 DNS 功能](https://technet.microsoft.com/library/dd728017.aspx)
  
[複製錯誤 8614 Active Directory 無法複寫洽詢，因為自上次複寫伺服器的時間有超過標記期間](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[複寫錯誤 8524 DSA 操作程式無法繼續因為 DNS 搜尋](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[複製錯誤 8456 或 8457 來源 |目前目的伺服器拒絕複寫要求](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[複寫錯誤 8453 複寫被存取](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[複製錯誤 8452 命名操作正被移除或不會從指定的伺服器複寫](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[複製錯誤 5 存取](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[複製錯誤-2146893022 目標主體名稱不正確](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[複製錯誤 1753 年有的端點對應程式提供更多端點](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[不是可用複寫錯誤 1722 RPC 伺服器](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[複製錯誤 1396 年登入失敗目標帳號不正確](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[複寫錯誤 1256 年遠端系統不是可用](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[複寫時發生錯誤 1127 年存取硬碟磁碟操作失敗重試更後](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[複製錯誤 8451 複寫操作發生資料庫錯誤](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[已給予複寫屬性時發生錯誤 8606 不足，無法建立物件](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>程式設計和疑難排解複寫 Active Directory 資源

輸入或輸出︰ 複寫失敗造成 Active Directory 物件代表複寫拓撲、複寫排程、網域控制站、使用者、電腦、密碼、安全性群組、群組成員資格和為網域控制站之間一致的群組原則。 Directory 不一致，︰ 複寫失敗造成操作失敗或不一致的結果，根據網域控制站的連絡作業，可以使應用程式的群組原則及存取控制權限。 Active Directory Domain Services (AD DS) 網路連接、名稱解析、驗證和授權、directory 資料庫、複寫拓撲，以及定複寫引擎。 當不顯而易見複寫問題的根本原因時，判斷的原因很多可能的原因之間需要一抵銷的可能原因。 

UI 架構監視複寫及診斷錯誤協助工具，請查看[Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)。 另外還有[手動實驗室](https://go.microsoft.com/?linkid=9844718)來示範如何使用 Active Directory 複寫狀態及其他工具，來疑難排解錯誤。 

適用於完整的文件，告訴您如何使用疑難排解 Active Directory Repadmin 工具複寫可;查看[監視和疑難排解 Active Directory 複寫使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

複寫 Active Directory 的運作方式的相關資訊，會看到以下技術參考：


- [Active Directory 複寫模型技術參考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [作用中導演複寫拓撲技術參考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具方案建議

最好的紅色（錯誤）和黃色（警告）事件 Directory 服務事件登入建議造成︰ 複寫失敗來源或目的網域控制站的特定限制。 如果事件訊息建議方案的步驟，請嘗試的步驟操作事件中所述。 Repadmin 工具及其他診斷工具也會提供資訊，可協助您解析︰ 複寫失敗。 

適用於使用 Repadmin 複寫問題進行疑難排解的詳細資訊，請查看[監視和疑難排解 Active Directory 複寫使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>查看刻意受到干擾或硬體故障 ruling

有時會發生因為刻意受到干擾複寫錯誤。 例如，當您執行疑難排解複寫 Active Directory 的問題，排除刻意中斷及硬體故障或升級第一次。

### <a name="intentional-disconnections"></a>刻意中斷

如果複寫錯誤報告網域控制站已嘗試複製的網域控制站在臨時的網站已建置，且目前離線最終 production 網站（遠端網站，例如分公司）中的 deployment 使用者熱切地等待，您可以負責那些複寫錯誤。 若要避免分隔網域控制站從複寫拓撲長的時間，使得連續錯誤之前的網域控制站已重新連接，請考慮將新增這類電腦最初成員伺服器為使用安裝媒體 (IFM) 方法從安裝 Active Directory Domain Services (AD DS)。 您可以使用 Ntdsutil 命令列工具來建立安裝媒體，您可以在抽取式媒體（CD、DVD 或其他媒體）和目的地網站出貨。 然後，您可以使用安裝媒體的網站，而不使用複寫網域控制站安裝 AD DS。 

### <a name="hardware-failures-or-upgradestitle"></a>硬體故障或升級</title>

如果複寫問題發生硬體故障（例如，失敗主機板、子系統磁碟或磁碟機），以便硬體問題可以被解析通知伺服器擁有者。

定期硬體升級也會造成網域控制站都退出服務。 確定您伺服器擁有者有很好事先通訊這類問題的系統。

### <a name="firewall-configuration"></a>防火牆設定

根據預設，Active Directory 複寫遠端程序呼叫 (Rpc) 動態發生透過 RPC Endpoint 對應 (RPCSS) 135 連接埠使用連接埠。 確定 Windows 防火牆使用進階安全性和其他防火牆複寫允許設定正確。 用於指定連接埠複寫 Active Directory 及連接埠設定的相關資訊，請查看[文章 224196 Microsoft 知識庫在](https://go.microsoft.com/fwlink/?LinkId=22578)。 

有關使用複寫 Active Directory 連接埠，請查看[Active Directory 複寫工具和設定](https://go.microsoft.com/fwlink/?LinkId=123774)。 

用於管理複寫 Active Directory 防火牆透過相關資訊，請查看[防火牆的 Active Directory 複寫](https://go.microsoft.com/fwlink/?LinkId=123775)。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>回應執行 Windows 2000 Server 過時伺服器的商品

如果超過天數標記期間失敗網域控制站執行 Windows 2000 Server、方案都相同： 

1. 將伺服器的企業網路移到私人網路。
2. 指定 Active Directory 中移除或重新安裝作業系統。
3. Active directory 移除伺服器中繼資料不會恢復伺服器物件。 

若要清除伺服器中繼資料在大部分的 Windows 作業系統，您可以使用指令碼。 有關使用此指令碼，請查看[移除 Active Directory 網域控制站中繼資料](https://go.microsoft.com/fwlink/?LinkID=123599)。 

根據預設，NTDS 設定物件刪除會自動一段 14 天恢復。 因此，如果您不會移除伺服器中繼資料（使用 Ntdsutil 或指令碼執行的清除中繼資料所述），在 directory，這將會提示發生嘗試複寫恢復伺服器中繼資料。 在這種情形下，將會持續登錯誤無法複寫遺失網域控制站的結果。

## <a name="root-causes"></a>根本原因

如果您要排除刻意中斷、硬體故障，並過期的 Windows 2000 網域控制站，複寫問題的其餘部分幾乎都已根本原因下列其中一項： 


- 網路連接：網路連接可能無法使用或網路設定的設定不正確。
- 名稱解析：DNS 錯誤設定的常見︰ 複寫失敗的原因。
- 驗證和授權：驗證和授權問題會導致「拒絕存取「錯誤，當連接到其複寫合作夥伴嘗試網域控制站。
- Directory 資料庫（儲存）：directory 資料庫可能無法處理交易快速地與複寫逾時。
- 複寫引擎︰ 複寫佇列間複寫排程太簡短時，可能會太大處理中所需的輸出複寫排程的時間。 在本案例中的一些變更複寫可以停滯的 indefinitelypotentially，超過標記期間長度。
- 複製拓撲：網域控制站 AD DS 地圖 (WAN) 的寬形真正的區域網路或連接私人網路 virtual (VPN)，必須有間的連結。 如果您的複寫拓撲 AD DS，您的網路的實際網站拓撲不支援中建立物件，需要設定錯誤的拓撲︰ 複寫失敗。

## <a name="general-approach-to-fixing-problems"></a>一般修正問題的方法

使用下列以修正問題複寫一般的方法： 


1. 監視複寫健康每日，或使用 Repadmin.exe 每天取得複寫狀態。
2. 嘗試解析及時任何回報的失敗事件郵件本文中所述方式。 軟體可能會造成問題，如果解除安裝的軟體，才能繼續其他方案。
3. 如果任何已知方法無法解析造成︰ 複寫失敗的問題，請移除伺服器 AD DS，然後再重新安裝 AD DS。 如需有關重新安裝 AD DS，請查看[解除委任網域控制站](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果無法正常移除 AD DS 伺服器連接到網路時，，使用下列方法修正問題的相關：
 

    - 強制 AD DS 移除清除伺服器中繼資料中 Directory 服務還原模式 (DSRM)，然後再重新安裝 AD DS。
    - 重新安裝作業系統，並重新建立網域控制站。

如需有關強迫移除 AD DS，請查看[強迫移除網域控制站的](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 擷取複寫狀態</title>

複製狀態是重要的方式為您評估 directory 服務的狀態。 如果複寫無誤運作，您知道 online 網域控制站。 您也可以知道使用下列系統與服務：


- DNS 基礎結構
- Kerberos 驗證通訊協定
- Windows 時間服務 (W32time)
- 遠端程序呼叫 (RPC)
- 網路連接

使用 Repadmin 監視複寫狀態每日執行評估複寫狀態的所有網域控制站在您的樹系的命令。 此程序產生.csv 檔案，您可以在 Excel 及篩選︰ 複寫失敗開放。

您可以使用下列程序取得複寫森林中的所有網域控制站的狀態。 
      
需求
      
資格在**企業系統管理員**，或相當於，才能完成此程序最小值。 

工具： 


- Repadmin.exe 
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>若要產生 repadmin /showrepl 網域控制站試算表



1. 打開以系統管理員身分命令提示字元︰ 在 [開始] 功能表、命令提示字元中，以滑鼠右鍵按一下，然後按一下以系統管理員身分執行。 如果使用者 Account 控制項對話方塊，提供的認證企業系統管理員，如有需要，，然後按一下 [繼續]。 
2. 在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：`repadmin /showrepl * /csv &gt;showrepl.csv`
3. 打開 Excel。
4. 按一下 [Office] 按鈕，按一下 [開放，showrepl.csv，瀏覽，然後按一下開放。
5. 隱藏或 delete 欄以及傳輸類型的欄中，如下：
6. 選取您想要隱藏或 delete 欄。
      

    - 若要隱藏欄欄中，按一下滑鼠右鍵，然後按一下 [隱藏。
    - 若要 delete 欄、選取的欄中，按一下滑鼠右鍵，然後按一下 Delete。
7. 選取 [在標題行下方的列 1。 在 [檢視] 索引標籤，凍結窗格中，按一下 [，然後按一下凍結頂端列。
8. 選取整個試算表。 在 [資料] 索引標籤中，按一下篩選。
9. 在成功上次的欄中，按一下向下箭號，再遞增排序。
10. 在來源俠欄中，按一下向下箭號篩選，指向 [文字篩選器]，然後按一下自訂篩選。
11. 自訂篩選對話方塊中下，按一下 [未包含顯示列。 旁邊的文字方塊中輸入<userInput>del</userInput>檢視中排除的結果刪除網域控制站。
12. 重複執行「步驟 11 失敗上次的欄中，但使用值不相同，然後輸入 0。
13. 解析︰ 複寫失敗。

森林中每個網域控制站試算表會顯示原始檔複寫合作夥伴，時間複寫上次發生，與每個命名操作（directory 磁碟分割）最後一個︰ 複寫失敗的時間。 使用篩選在 Excel 中，您可以檢視複寫健康僅網域控制站的網域控制站僅或網域控制站的最低或大部分最新狀態，無法，您可以看到成功複寫複寫合作夥伴。

## <a name="replication-problems-and-resolutions"></a>複寫問題，以及解析度
    
事件訊息，並在各種不同的應用程式或服務會嘗試操作時發生錯誤訊息複寫問題報告。 最好監視您的應用程式或擷取複寫狀態時，會收集這些訊息。

登入 Directory 服務事件登入事件訊息中都會大部分複寫的問題。 複寫問題也可能會辨識錯誤訊息的輸出中的<system>repadmin /showrepl</system>命令。 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>Repadmin /showrepl 錯誤訊息，指出複寫問題

找出複寫 Active Directory 的問題，請使用<system>repadmin /showrepl</system>一節中所述命令。 下表顯示錯誤訊息，此命令產生，以及的錯誤和提供方案錯誤的主題的連結的根本原因。


|Repadmin 錯誤|根本原因|方案|
| --- | --- | --- |
|這一個複寫之後的時間伺服器超過標記期間。|網域控制站長滿足保存到標記，已複寫，而且回收 AD DS 從指定的來源網域控制站輸入︰ 複寫失敗。|事件 ID 2042：已經太長的時間後複寫這部電腦|
|不輸入的鄰居。|如果不出現「輸入鄰居」由 repadmin 輸出一節中的任何項目 /showrepl，網域控制站找不到建立與另一部網域控制站複製連結。|修正複寫連接問題 (263 1925 年)| 
|存取。|有兩個的網域控制站之間複寫連結，但無法正確執行複寫，根據驗證失敗。|修正複寫安全性問題| 
|上一次嘗試 < 日期-時間 > 在無法使用」目標帳號不正確。」|這個問題可以相關連接、DNS 或驗證的問題。 如果這是 DNS 錯誤，本機網域控制站無法解析全球唯一 (GUID)-根據其複寫合作夥伴的 DNS 名稱。|修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年) 修正複寫安全性問題修正複寫連接問題 (263 1925 年)| 
|LDAP 錯誤 49。|網域控制站電腦 account 可能不會同步的金鑰 Distribution 中心 (KDC)。|修正複寫安全性問題| 
|無法開放 LDAP 連接到本機主機|系統管理工具可以連絡 AD DS。|修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年)| 
|Active Directory 複寫優先處理了。|較高優先順序複寫要求，例如的要求，以手動方式與 repadmin /sync 命令產生中斷輸入複寫的進度。|等待複寫才能完成。 此資訊訊息指出正常運作。| 
|張貼複寫、等待。| 網域控制站張貼複寫要求和正在等待解答。 複寫正在進行此來源。|等待複寫才能完成。 此資訊訊息指出正常運作。| 

下表列出一般事件可能會使用 Active Directory 複寫，以及根造成問題的問題提供方案的主題的連結的問題。 

|事件 ID 和來源|根本原因|方案|
| --- | --- | --- | 
|1311 NTDS KCC|複寫設定資訊，以 AD DS 無法正確反映實體網路的拓撲。|修正複寫拓撲問題 (263 1311 年)| 
|1388 NTDS 複寫|嚴格複寫一致性不會生效，以及已複寫網域控制站的延遲物件。|修正複寫延遲物件問題 (事件 Id 1388，1988，2042 年)|
|1925 NTDS KCC|建立寫入 directory 磁碟分割的連結︰ 複寫失敗。 事件這可以讓不同的原因，根據錯誤。| 修正複寫連接的問題 (263 1925 年) 修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年)| 
|1988 NTDS 複寫|本機網域控制站已嘗試複製物件來源網域控制站未顯示在本機網域控制站，因為它可能已刪除和已經回收。 辨識情形之前複寫不會繼續使用此合作夥伴此 directory 磁碟分割。|修正複寫延遲物件問題 (事件 Id 1388，1988，2042 年)|
|2042 NTDS 複寫|複寫尚未標記期間，發生此合作夥伴使用，複寫無法繼續。|修正複寫延遲物件問題 (事件 Id 1388，1988，2042 年)| 
|2087 NTDS 複寫|AD DS 無法解析主機的 DNS 名稱來源網域控制站的 IP 位址和︰ 複寫失敗。|修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年)| 
|2088 NTDS 複寫 |AD DS 無法解析 IP 位址，但複寫成功來源網域控制站的 DNS 名稱主機。|修正複寫 DNS 查詢問題 (事件 Id 1925，2087，2088 年)|
|5805 網路登入|電腦 account 無法驗證，這通常是因為數個相同的電腦名稱實例或不複寫每個網域控制站的電腦名稱。|修正複寫安全性問題| 

有關更多複寫概念，請查看[Active Directory 複寫技術](https://go.microsoft.com/fwlink/?LinkId=41950)。
  
