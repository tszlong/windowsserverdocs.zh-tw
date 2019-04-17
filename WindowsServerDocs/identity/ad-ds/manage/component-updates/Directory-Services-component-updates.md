---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: "Directory 服務的元件更新"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>Directory 服務的元件更新

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
這個課程解釋 Directory 服務的元件更新，在 Windows Server 2012 R2。  
  
## <a name="what-you-will-learn"></a>您會了解  
解釋下列新 Directory 服務的元件更新：  
  
-   解釋下列新 Directory 服務的元件更新：  
  
    -   [網域和森林功能層級](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [取代 NTFRS 了](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [變更 LDAP 最佳化](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [1644 事件改良功能](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>網域和森林功能層級  
  
### <a name="overview"></a>概觀  
區段會提供簡介網域和森林功能層級變更。  
  
### <a name="new-dfl-and-ffl"></a>新 DFL 和 FFL  
發行，有新的網域及森林功能等級：  
  
-   森林功能層級：Windows Server 2012 R2  
  
-   網域正常運作的層級：Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 網域功能等級可讓支援下列動作：  
  
1.  適用於俠端保護*受保護的使用者*  
  
    *保護使用者*Windows Server 2012 R2 網域驗證可以**再**:  
  
    -   驗證 NTLM 驗證  
  
    -   使用 F:kerberos 預先驗證 DES 或 RC4 密碼套件  
  
    -   使用未限制或限制委派委派  
  
    -   續約初始 4 小時期間以外的使用者門票 (Tgt)  
  
2.  驗證原則  
  
    新的樹系的 Active Directory 原則可套用到 Windows Server 2012 R2 網域控制的主機中帳號，account 可以登入的及適用於執行 account 與服務存取控制項條件驗證  
  
3.  驗證原則筒倉  
  
    為基礎新的樹系的 Active Directory 物件，可以建立用來可帳號驗證原則或驗證隔離的使用者，受管理的服務和電腦帳號之間的關係。  
  
查看如何設定保護帳號如需詳細資訊。  
  
除了上面的功能，Windows Server 2012 R2 網域功能等級可確保網域中的任何網域控制站執行 Windows Server 2012 R2。  
Windows Server 2012 R2 網域功能等級不提供任何新功能，但確保任何新的網域建立森林中將會自動運作層級 Windows Server 2012 R2 網域正常運作。  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>最小 DFL 執行上建立新的網域  
Windows Server 2008 DFL 是支援網域建立新的小功能層級。  
  
> [!NOTE]  
> 做出的 FRS 被透過移除安裝新的網域和 Windows Server 2008 的伺服器管理員中，或透過 Windows PowerShell 低於網域功能層級的能力。  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低的樹系和網域正常運作的層級  
樹系和網域功能層級設定為 Windows Server 2012 R2 網域和新的樹系建立新的預設，但可以使用 Windows PowerShell 調低。  
  
提高或降低使用 Windows PowerShell 的樹系功能層級，請使用**設定為 ADForestMode** cmdlet。  
  
**若要設定 contoso.com FFL Windows Server 2008 模式：**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
提高或降低使用 Windows PowerShell 網域功能等級，請使用 Set-ADDomainMode cmdlet。  
  
**若要設定 contoso.com DFL Windows Server 2008 模式：**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
Windows Server 2012 R2 做為額外的複本遇到現有的網域執行 2003 DFL DC 升級的運作方式。  
  
建立網域新在現有的樹系  
  
![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
不有任何新的樹系或在此版本的網域作業。  
  
這些.ldf 的檔案包含架構變更適用於**裝置登記服務**。  
  
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
  
**驗證原則和筒倉**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>取代 NTFRS 了  
  
### <a name="overview"></a>概觀  
在 Windows Server 2012 R2 會取代 FRS。  做出的 FRS 被透過執行 Windows Server 2008 的最低網域功能等級 (DFL)。  這個執法才會顯示出來使用伺服器管理員及 Windows PowerShell 來建立新的網域。  
  
若要指定網域功能等級-DomainMode 參數使用 Install-ADDSForest 或 Install-ADDSDomain cmdlet 中。  支援此參數值可以正確整數或對應列舉的字串值。 例如，若要設定 Windows Server 2008 R2 網域模式層級，，您可以指定值 4 或是」Win2008R2」。  從 Server 2012 R2 有效執行這些 cmdlet 時值包括與 Windows Server 2008 (3 Win2008) 的 Windows Server 2008 R2 (4 Win2008R2) (5 Win2012) 的 Windows Server 2012 和 Windows Server 2012 R2 (6 Win2012R2)。 層級不得低於的樹系功能的層級，但很高正常運作的網域。  由於 FRS 在此版本中，Windows Server 2003 (2，Win2003) 取代不是辨識的參數，使用下列 cmdlet 執行的 Windows Server 2012 R2。  
  
![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>變更 LDAP 最佳化  
  
### <a name="overview"></a>概觀  
LDAP 查詢最佳化演算法是評估，並進一步最佳化。  結果是 LDAP 搜尋效率和 LDAP 搜尋查詢複雜時間效能改進。  
  
> [!NOTE]  
> **開發：**改善效能的改進從 LDAP 對應的搜尋查詢 ESE 查詢。  LDAP 複雜的特定的層級以外的篩選器避免最佳化的索引選取項目，會導致效能（1000 x 以上）大幅降低。 這項變更更改我們中，選取 [索引 LDAP 查詢，以避免此問題的方式。  
  
> [!NOTE]  
> 完整的 LDAP 查詢最佳化演算法，會導致 overhaul:  
>   
> -   更快速地搜尋時間  
> -   提高可及範圍效率允許 Dc 執行更多  
> -   相關廣告效能問題較不支援電話  
> -   返回移植到 Windows Server 2008 R2 (2862304 KB)  
  
### <a name="background"></a>背景  
Active Directory 搜尋功能提供網域控制站的核心服務。  其他服務及營運應用程式需依賴 Active Directory 搜尋。  如果無法使用這項功能可以停止停滯企業營運。  Core 和常用的服務，請務必網域控制站處理 LDAP 搜尋傳輸有效率。  讓 LDAP 搜尋效率，可以透過記錄編製索引資料庫中滿足結果組對應 LDAP 搜尋篩選嘗試 LDAP 查詢最佳化演算法。  這個演算法是評估，並進一步最佳化。  結果是 LDAP 搜尋效率和 LDAP 搜尋查詢複雜時間效能改進。  
  
### <a name="details-of-change"></a>變更詳細資訊  
包含的 LDAP 搜尋：  
  
-   在 [開始搜尋階層某個位置（NC 標頭，組織單位，物件）  
  
-   搜尋條件  
  
-   若要返回屬性的清單  
  
搜尋程序摘要如下：  
  
1.  如果可能的話簡化搜尋篩選。  
  
2.  選取 [索引鍵，將會退還小涵蓋的設定的設定。  
  
3.  執行一個或多個索引鍵，以減少涵蓋的設定交叉。  
  
4.  每個記錄涵蓋設定中，評估篩選以及安全性。 如果篩選評估為 TRUE 且存取，然後回到此記錄 client。  
  
LDAP 查詢最佳化工作修改步驟 2 和 3，以減少涵蓋集的大小。 更多尤其是目前的實作選取索引重複的按鍵，並執行備援交叉。  
  
### <a name="comparison-between-old-and-new-algorithm"></a>舊和新演算法之間的比較  
在此範例中效率 LDAP 搜尋的目標是 Windows Server 2012 網域控制站。  搜尋完成根據無法找到更有效率索引大約 44 秒。  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>使用新的演算法範例結果  
重複上述完全相同搜尋此範例中，但針對 Windows Server 2012 R2 網域控制站。  相同搜尋完成小於秒因為 LDAP 查詢最佳化演算法中的改良功能。  
  
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
  
-   如果無法最佳化樹：  
  
    -   例如：樹運算式是透過不編製索引一欄  
  
    -   錄製指數防止最佳化的清單  
  
    -   透過 ETW 描圖和事件 1644 來電顯示公開  
  
        ![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>若要讓 LDP 統計資料控制項  
  
1.  打開 LDP.exe 連接並連結到網域控制站。  
  
2.  在**選項**功能表上，按**控制項**。  
  
3.  在控制項] 對話方塊中，展開**載入預先定義的**下拉式功能表，按**搜尋統計資料**，然後按一下 [ **[確定]**。  
  
    ![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  在**瀏覽]**功能表上，按**搜尋**  
  
5.  在 [搜尋] 對話方塊中，選取 [**選項**按鈕。  
  
6.  確認**延伸**核取方塊已選取上搜尋選項] 對話方塊中選取**[確定]**。  
  
    ![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>請嘗試︰ 使用 LDP 返回查詢統計資料  
網域控制站，或從加入網域的 client 或已安裝的 AD DS 工具的伺服器，請執行下列。  重複下列您的 Windows Server 2012 俠與 Windows Server 2012 R2 俠目標。  
  
1.  檢視[「建立更有效率 Microsoft AD 支援的應用程式]](https://msdn.microsoft.com/library/ms808539.aspx)文章，並視需要回到參考它。  
  
2.  使用 LDP，讓搜尋統計資料 (查看[以讓 LDP 統計資料控制項](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  為了數個 LDAP 搜尋，並觀察統計資訊頂端的結果。  您將會重複其他相同的搜尋活動中的文件它們記事本文字檔案。  
  
4.  執行最佳化無法因為屬性指數最佳化的 LDAP 搜尋  
  
5.  嘗試建構搜尋這需要很長的時間來完成 (您可能想要增加**的時間限制**選項，讓搜尋是否無法逾時)。  
  
### <a name="additional-resources"></a>其他資源  
[Active Directory 搜尋為何？](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Active Directory 搜尋的工作方式](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[建立更有效率 Microsoft Active Directory 功能的應用程式](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) LDAP 查詢的執行速度變慢比預期廣告或 LDS 日 ADAM directory 服務與事件 ID 1644 可能登入  
  
## <a name="BKMK_1644"></a>1644 事件改良功能  
  
### <a name="overview"></a>概觀  
這項更新會協助進行疑難排解的事件編號 1644 年其他 LDAP 搜尋結果統計資料。  此外，也可以用來登入以時間為基礎的閾值可讓新登錄值。  這些改良已可在 Windows Server 2012 和 Windows Server 2008 R2 SP1 KB 透過[2800945](https://support.microsoft.com/kb/2800945)，將可讓 Windows Server 2008 SP2。  
  
> [!NOTE]  
> -   其他 LDAP 搜尋統計資料] 會新增至事件 ID 1644，協助您疑難排解效率或便宜 LDAP 搜尋  
> -   您現在可以指定搜尋時間臨界值（例如。 登入事件 1644 年搜尋拍攝超過 100ms 年）而不是指定昂貴且 Inefficient 搜尋結果臨界值  
  
### <a name="background"></a>背景  
同時 Active Directory 效能問題的疑難排解，就能發揮 LDAP 搜尋活動可能造成問題的原因。  您可以選擇要登入，以便您可以看到寶貴或效率 LDAP 查詢網域控制站處理。  為了讓登入，您必須設定欄位工程診斷，也可以指定便宜 / 效率搜尋結果臨界值。  時讓欄位工程登入層級 5 的值，以符合下列條件任何搜尋已登入的事件編號 1644 年 Directory 服務事件登入。  
  
事件包含：  
  
-   Client IP 和連接埠  
  
-   開始節點  
  
-   篩選  
  
-   搜尋範圍  
  
-   屬性選取項目  
  
-   伺服器控制項  
  
-   瀏覽項目  
  
-   傳回項目  
  
不過，事件遺失重要的資料是的話指數等（如果有的話）上的搜尋作業和項目所花費的時間。  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>事件 1644 年新增額外的搜尋統計資料  
  
-   使用的索引  
  
-   參考的網頁  
  
-   朗讀從磁碟頁面  
  
-   從磁碟 preread 頁面  
  
-   修改全新的頁面  
  
-   修改髒頁面  
  
-   搜尋時間  
  
-   防止最佳化屬性  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>事件 1644 年登入新的時間型閾值來改善登錄值  
指定昂貴且 Inefficient 搜尋結果臨界值，而您可以指定搜尋時間臨界值。  如果您需要登入拍攝 50 ms 的所有搜尋結果中或大，您可以指定 50 小數點 / 32 十六進位（除了欄位工程值設定）。  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>舊和新的事件編號 1644 年的比較  
舊  
  
![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
新  
  
![Directory 服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>請嘗試︰ 使用事件登入以返回查詢統計資料  
  
1.  重複下列您的 Windows Server 2012 俠與 Windows Server 2012 R2 俠目標。 每個搜尋之後您會看到這兩個網域控制站的事件編號 1644s 年。  
  
2.  使用 regedit，讓 Windows Server 2012 R2 俠與 Windows Server 2012 DC 上舊的方法使用時間為基礎的閾值事件 ID 1644 登入。  
  
3.  進行幾個 LDAP 搜尋超過閾值來改善，並觀察統計資訊頂端的結果。  使用您稍早記載 LDAP 查詢和重複相同搜尋。  
  
4.  執行 LDAP 搜尋最佳化不能最佳化因為屬性一或多個不編製索引。  
  
## <a name="BKMK_ADRepl"></a>Active Directory 複寫輸送量改進  
  
### <a name="overview"></a>概觀  
使用它複寫傳輸 RPC AD 複寫。 根據預設，RPC 使用 8 K 傳輸緩衝和 5 K 封包大小。 這效果網路傳送執行個體將會傳送三個封包 (大約 15 K 值得的資料) 和需要往返將傳送更多之前，請先等候網路位置。 假設 3ms 往返的時間，最大的輸送量會在 40Mbps，即使是在 1Gbps 或 10 Gbps 網路。  
  
> [!NOTE]  
> -   此更新調整到約 600 Mbps 40Mbps 的最大 AD 複寫輸送量。  
>   
>     -   它會增加 RPC 傳送緩衝大小減少的網路，往返  
> -   將最高的速度，明顯效果高延遲網路。  
  
此更新變更 8 K RPC 傳送緩衝大小為 256 KB 增加約 600 Mbps 到最大的輸送量。  這項變更可成長 8 K 以外的 TCP 視窗大小往返減少的網路。  
  
> [!NOTE]  
> 有任何可設定的設定來變更此行為。  
  
### <a name="additional-resources"></a>其他資源  
[複寫 Active Directory 型號的運作方式](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


