---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: 目錄服務元件更新
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f3e0553b1919a7f9129d47616d0ffb66b6ff48f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874439"
---
# <a name="directory-services-component-updates"></a>目錄服務元件更新

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

**作者**:Justin Turner 資深支援高階工程師，與 Windows 群組  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
本課程將說明 Windows Server 2012 R2 中的目錄服務元件更新。  
  
## <a name="what-you-will-learn"></a>您將學到  
說明下列新的目錄服務元件更新：  
  
-   說明下列新的目錄服務元件更新：  
  
    -   [網域與樹系功能等級](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [NTFRS 過時](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [LDAP 查詢最佳化工具變更](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [1644 事件改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>網域和樹系功能等級  
  
### <a name="overview"></a>總覽  
本節提供網域和樹系功能層級變更的簡介。  
  
### <a name="new-dfl-and-ffl"></a>新的網域功能等級和 FFL  
版本中，有新的網域和樹系功能等級：  
  
-   樹系功能等級：Windows Server 2012 R2  
  
-   網域功能等級：Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 網域功能等級啟用了下列支援：  
  
1.  針對 DC 端保護*Protected Users*  
  
    *受保護的使用者*向 Windows Server 2012 R2 網域可以**不再**:  
  
    -   使用 NTLM 驗證進行驗證  
  
    -   Kerberos 預先驗證中使用 DES 或 RC4 加密套件  
  
    -   委派與非限制或限制委派  
  
    -   超過初始 4 小時存留期之後更新使用者票證 (Tgt)  
  
2.  驗證原則  
  
    新樹系為基礎的 Active Directory 原則可以套用至 Windows Server 2012 R2 來控制哪些主機的網域帳戶的帳戶可以登入從和適用於執行身分帳戶的服務進行驗證的存取控制條件  
  
3.  驗證原則定址接收器  
  
    新的樹系 Active Directory 物件可以建立使用者、 受管理的服務與電腦帳戶，用來分類驗證原則或驗證隔離帳戶之間的關聯性。  
  
請參閱 < How to Configure Protected Accounts 如需詳細資訊。  
  
除了上述的功能，Windows Server 2012 R2 網域功能等級可確保網域中的任何網域控制站執行 Windows Server 2012 R2。  
Windows Server 2012 R2 的樹系功能等級不提供任何新的功能，但是它確保樹系中建立的任何新網域會自動運作在 Windows Server 2012 R2 網域功能等級。  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>在 建立新網域上強制執行的最低網域功能等級  
Windows Server 2008 網域功能等級是在新的網域建立支援的最小功能層級。  
  
> [!NOTE]  
> 取代 FRS 的作業被透過移除具有網域功能等級低於 Windows Server 2008 使用伺服器管理員或透過 Windows PowerShell 安裝新網域的能力。  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低樹系和網域功能等級  
樹系和網域功能等級預設設定為 Windows Server 2012 R2 上建立新網域和樹系，但可以使用 Windows PowerShell 會降低。  
  
若要提高或降低樹系功能等級使用 Windows PowerShell，使用**Set-adforestmode** cmdlet。  
  
**將 contoso.com FFL 設定為 Windows Server 2008 模式：**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
若要提高或降低網域功能等級使用 Windows PowerShell，使用 Set-addomainmode cmdlet。  
  
**若要設定 contoso.com 網域功能等級提高至 Windows Server 2008 模式：**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
DC，以新增額外的複本中遇到執行 2003年網域功能等級的現有網域的 Windows Server 2012 R2 的升級運作方式。  
  
在現有樹系中建立的新網域  
  
![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
沒有任何新的樹系或網域作業，在此版本中。  
  
這些.ldf 檔案包含結構描述變更**Device Registration Service**。  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**工作資料夾：**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**驗證原則和定址接收器**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>Ntfrs 過時  
  
### <a name="overview"></a>總覽  
Windows Server 2012 R2 中已被取代 FRS。  取代 FRS 的作業被透過強制執行 Windows Server 2008 的最低網域功能等級 (DFL)。  使用伺服器管理員] 或 [Windows PowerShell 來建立新的網域時，才有此強制執行。  
  
您使用與 Install-addsforest 或 Install-addsdomain cmdlet 的-DomainMode 參數來指定網域功能等級上。  支援的值，這個參數可以是有效的整數或對應的列舉的字串值。 比方說，若要設定 Windows Server 2008 R2 網域模式層級，您可以指定的值為 4，或是 「 Win2008R2"。  從有效的 Server 2012 R2 中執行這些 cmdlet 時的值包括 Windows Server 2008 (3，Win2008) 的 Windows Server 2008 R2 (4，Win2008R2) Windows Server 2012 (5，Win2012) 和 Windows Server 2012 R2 (6，Win2012R2)。 網域功能等級不能低於、但可以高於樹系功能等級。  因為在這一版 Windows Server 2003 (2，Win2003) 已被取代 FRS 不是可辨識的參數，使用這些 cmdlet，從 Windows Server 2012 R2 中執行。  
  
![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>LDAP 查詢最佳化工具變更  
  
### <a name="overview"></a>總覽  
LDAP 查詢最佳化工具演算法已重新評估，並進一步最佳化。  結果是 LDAP 搜尋效率與 LDAP 搜尋時間的複雜查詢的效能改善。  
  
> [!NOTE]  
> **從開發人員：** ESE 查詢的查詢從 LDAP 對應中的改進搜尋的效能改進。  除了複雜程度的 LDAP 篩選器會防止最佳化的索引選項，因而導致大幅降低效能 （1000 x 或以上）。 這項變更會改變我們可以在其中選取索引的 LDAP 查詢，以避免這個問題的方式。  
  
> [!NOTE]  
> LDAP 查詢最佳化工具演算法，產生的完整更新了：  
>   
> -   更快速的搜尋時間  
> -   效率提升可讓網域控制站以執行更多作業  
> -   關於 AD 效能問題較電話支援服務  
> -   上一步移植到 Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>背景  
搜尋 Active Directory 的功能是由網域控制站提供的核心服務。  其他服務和企業營運應用程式依賴 Active Directory 的搜尋。  如果無法使用這項功能，商務作業可以停止暫止。  為核心和常用的服務，因此務必網域控制站會有效率地處理 LDAP 搜尋流量。  LDAP 查詢最佳化工具演算法會嘗試將 LDAP 搜尋篩選器對應至結果集，可以透過在資料庫中已編製索引的記錄符合，讓 LDAP 搜尋盡可能有效率。  此演算法已重新評估，並進一步最佳化。  結果是 LDAP 搜尋效率與 LDAP 搜尋時間的複雜查詢的效能改善。  
  
### <a name="details-of-change"></a>變更的詳細資料  
LDAP 搜尋包含：  
  
-   若要開始搜尋階層中的位置 （NC 標頭，OU 物件）  
  
-   搜尋篩選器  
  
-   要傳回的屬性清單  
  
搜尋程序摘要如下：  
  
1.  盡可能簡化搜尋篩選器。  
  
2.  選取會傳回最小涵蓋的索引鍵的一組。  
  
3.  執行一或多個索引鍵，以減少涵蓋的組的交集。  
  
4.  涵蓋的集合中每一筆記錄，評估篩選運算式，以及安全性。 如果篩選條件評估為 TRUE，並在授與存取權，則傳回此記錄到用戶端。  
  
LDAP 查詢最佳化工作修改步驟 2 和 3，以減少涵蓋集的大小。 更具體來說，目前的實作選取重複的索引鍵，並會執行備援的交集。  
  
### <a name="comparison-between-old-and-new-algorithm"></a>舊的和新的演算法之間的比較  
在此範例中的效率不佳的 LDAP 搜尋的目標是 Windows Server 2012 網域控制站。  搜尋會在大約有 44 秒由於找不到更有效率的索引來完成。  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt  
  
Using server: WINSRV-DC1.blue.contoso.com:389  
  
<removed search results>  
  
Statistics  
=====  
Elapsed Time: 44640 (ms)  
Returned 324 entries of 553896 visited - (0.06%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 DNT_index:516615:N  
  
Pages Referenced          : 4619650  
Pages Read From Disk      : 973  
Pages Pre-read From Disk  : 180898  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
### <a name="sample-results-using-the-new-algorithm"></a>使用新的演算法的範例結果  
此範例會重複上述完全相同的搜尋，但為目標的 Windows Server 2012 R2 網域控制站。  相同的搜尋會在因為 LDAP 查詢最佳化工具演算法中的改進少於一秒來完成。  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt  
  
Using server: winblueDC1.blue.contoso.com:389  
  
.<removed search results>  
  
Statistics  
=====  
Elapsed Time: 672 (ms)  
Returned 324 entries of 648 visited - (50.00%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 idx_userPrincipalName:648:N  
 idx_postalCode:323:N  
 idx_cn:1:N  
  
Pages Referenced          : 15350  
Pages Read From Disk      : 176  
Pages Pre-read From Disk  : 2  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
-   如果無法最佳化樹狀結構：  
  
    -   例如： 在樹狀目錄中的運算式是經由未編製索引的資料行  
  
    -   記錄防止最佳化的索引的清單  
  
    -   透過 ETW 追蹤和事件識別碼 1644年公開  
  
        ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>若要啟用在 LDP 的統計資料控制項  
  
1.  開啟 LDP.exe，和連線並繫結至網域控制站。  
  
2.  在 **選項**功能表上，按一下**控制項**。  
  
3.  在 控制項 對話方塊中，展開**載入預先定義**下拉功能表中，按一下**搜尋統計資料**，然後按一下 **確定**。  
  
    ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  在 **瀏覽**功能表上，按一下 **搜尋**  
  
5.  在 搜尋 對話方塊中，選取**選項** 按鈕。  
  
6.  請確定**Extended**  核取方塊已選取搜尋選項 對話方塊中，然後選取**確定**。  
  
    ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>試試看吧：使用 LDP 來傳回查詢統計資料  
在網域控制站，或從已加入網域的用戶端或已安裝的 AD DS 工具的伺服器，請執行下列。  重複下列以您的 Windows Server 2012 DC 和 Windows Server 2012 R2 DC 為目標。  
  
1.  檢閱[「 建立更有效率 Microsoft AD 啟用應用程式 」](https://msdn.microsoft.com/library/ms808539.aspx)一文，並視回頭參考它。  
  
2.  使用 LDP，請啟用搜尋統計資料 (請參閱[啟用統計資料中的控制 LDP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  執行數個 LDAP 搜尋，並觀察結果頂端的統計資訊。  您將會重複相同的搜尋，在其他活動因此記錄這些 「 記事本 」 文字檔案中。  
  
4.  執行查詢最佳化工具能夠因為屬性索引最佳化的 LDAP 搜尋  
  
5.  建構會很長的時間才能完成搜尋 (您可能想要增加**時限**選項讓搜尋服務會不會逾時)。  
  
### <a name="additional-resources"></a>其他資源  
[Active Directory 搜尋有哪些？](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[如何在 Active Directory 搜尋工作](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[建立更有效率 Microsoft Active Directory 功能的應用程式](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) LDAP 查詢執行速度比預期在 AD 中，或可能會記錄 LDS/ADAM 目錄服務和事件識別碼 1644年  
  
## <a name="BKMK_1644"></a>1644 事件改進  
  
### <a name="overview"></a>總覽  
此更新會新增其他 LDAP 搜尋結果統計資料，事件識別碼 1644年以協助進行疑難排解。  此外，還有可用來啟用記錄以時間為基礎的臨界值的新登錄值。  這些已改善適用於 Windows Server 2012 和 Windows Server 2008 R2 SP1 透過 KB [2800945](https://support.microsoft.com/kb/2800945)和將可供 Windows Server 2008 SP2。  
  
> [!NOTE]  
> -   其他 LDAP 搜尋統計資料會加入至事件識別碼 1644 以協助疑難排解效率不佳或昂貴的 LDAP 搜尋  
> -   您現在可以指定搜尋時間閾值 （例如。 超過 100 毫秒的搜尋記錄檔事件 1644年） 而不是指定昂貴且 Inefficient 搜尋結果臨界值  
  
### <a name="background"></a>背景  
Active Directory 的效能問題進行疑難排解時便可明顯看出 LDAP 搜尋活動可能會造成問題。  您決定要啟用記錄功能，以便您可以看到處理的網域控制站的昂貴或效率不佳的 LDAP 查詢。  若要啟用記錄，您必須將欄位工程診斷值，並可以選擇性地指定昂貴 / 效率不佳的搜尋結果的臨界值。  在啟用工程記錄層級設為 5 的值，符合這些準則的搜尋會記錄在事件識別碼 1644年以目錄服務事件記錄檔。  
  
事件包含：  
  
-   用戶端 IP 和連接埠  
  
-   開始節點  
  
-   篩選  
  
-   搜尋範圍  
  
-   屬性選取項目  
  
-   伺服器控制項  
  
-   瀏覽過的項目  
  
-   傳回的項目  
  
不過，索引鍵的資料中遺漏事件等上的搜尋作業以及所花費的時間量 （如果有的話） 使用索引。  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>其他搜尋統計資料加入至事件 1644  
  
-   使用的索引  
  
-   參考頁面  
  
-   從磁碟讀取分頁  
  
-   從磁碟 preread 的頁面  
  
-   修改的中途頁面  
  
-   修改的中途分頁  
  
-   搜尋時間  
  
-   防止最佳化的屬性  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>新的時間型閾值登錄值，用於記錄的事件 1644  
而不是指定昂貴且 Inefficient 搜尋結果臨界值，您可以指定搜尋時間閾值。  如果您想要記錄所有的搜尋結果所花費 50 毫秒或更高，您可以指定十進位 50 / 32 hex （除了設定工程欄位的值）。  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>舊的和新的事件識別碼 1644年的比較  
舊  
  
![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
新增  
  
![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>試試看吧：使用事件記錄檔傳回查詢統計資料  
  
1.  重複下列以您的 Windows Server 2012 DC 和 Windows Server 2012 R2 DC 為目標。 請注意這兩個網域控制站上的事件識別碼 1644s年之後每個搜尋。  
  
2.  使用 regedit，啟用 Windows Server 2012 R2 DC 和 Windows Server 2012 DC 上的舊方法上使用以時間為基礎的臨界值的事件識別碼 1644年記錄。  
  
3.  執行數個 LDAP 搜尋，會超過臨界值，並觀察結果頂端的統計資訊。  使用您稍早所述的 LDAP 查詢，然後重複相同的搜尋。  
  
4.  執行查詢最佳化工具不能最佳化，因為一個或多個屬性都不會編製索引的 LDAP 搜尋。  
  
## <a name="BKMK_ADRepl"></a>Active Directory 複寫輸送量改進  
  
### <a name="overview"></a>總覽  
AD 複寫會使用 RPC 的複寫傳輸。 預設情況下，RPC 會使用 8k 傳輸緩衝區和 5k 封包大小。 這已傳送的執行個體將傳輸三個封包 (大約 15 K 值得的資料)，則有之前要等待的網路來回傳送多個結果。 假設 3 毫秒來回時間 40mbps 調整，即使在 1Gbps 或 10 Gbps 的網路上大約是最高的輸送量。  
  
> [!NOTE]  
> -   此更新會調整的最大 AD 複寫輸送量由 40mbps 調整為約 600 Mbps。  
>   
>     -   它會增加 RPC 傳送緩衝區大小可減少網路來回行程  
> -   影響會最明顯的高速度、 高延遲網路。  
  
此更新會增加為約 600 Mbps 的最大輸送量，藉由變更從 8k 的 RPC 傳送緩衝區大小為 256 KB。  這項變更可讓 TCP 視窗大小成長至超出 8 K 減少網路往返。  
  
> [!NOTE]  
> 沒有任何可設定的設定，來修改此行為。  
  
### <a name="additional-resources"></a>其他資源  
[Active Directory 複寫模型的運作方式](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


