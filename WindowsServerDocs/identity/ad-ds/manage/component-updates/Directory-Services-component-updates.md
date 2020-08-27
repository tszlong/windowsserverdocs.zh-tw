---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: 目錄服務元件更新
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 39f13d18210a6527c5da2ccb6655150be9608a46
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939588"
---
# <a name="directory-services-component-updates"></a>目錄服務元件更新

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**： Justin Turner，與 Windows 群組的資深支援擴大工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

本課程說明 Windows Server 2012 R2 中的目錄服務元件更新。

## <a name="what-you-will-learn"></a>學習內容
說明下列新的目錄服務元件更新：

-   說明下列新的目錄服務元件更新：

    -   [網域與樹系功能等級](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)

    -   [NTFRS 過時](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)

    -   [LDAP 查詢最佳化工具變更](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)

    -   [1644 事件改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)

    -   [Active Directory 複寫輸送量改進](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)

## <a name="domain-and-forest-functional-levels"></a><a name="BKMK_FL"></a>網域和樹系功能等級

### <a name="overview"></a>概觀
本節提供網域和樹系功能等級變更的簡介。

### <a name="new-dfl-and-ffl"></a>新的 DFL 和 FFL
在發行時，有新的網域和樹系功能等級：

-   樹系功能等級： Windows Server 2012 R2

-   網域功能等級： Windows Server 2012 R2

### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 網域功能等級可支援下列各項：

1.  *受保護使用者*的 DC 端保護

    對 Windows Server 2012 R2 網域進行驗證的*受保護使用者*無法**再**執行：

    -   使用 NTLM 驗證進行驗證

    -   在 Kerberos 預先驗證中使用 DES 或 RC4 加密套件

    -   以非限制或限制委派方式受到委派

    -   在初始 4 小時存留期之後更新使用者票證 (TGT)

2.  驗證原則

    新的樹系架構 Active Directory 原則可套用至 Windows Server 2012 R2 網域中的帳戶，以控制帳戶可從哪些主機登入，並將驗證的存取控制條件套用至以帳戶執行的服務

3.  驗證原則定址接收器

    新的樹系架構 Active Directory 物件，可建立使用者、受控服務和電腦帳戶之間的關聯性，以用來分類帳戶以進行驗證原則或進行驗證隔離。

如需詳細資訊，請參閱如何設定受保護的帳戶。

除了上述功能之外，Windows Server 2012 R2 網域功能等級也可確保網域中的任何網域控制站都執行 Windows Server 2012 R2。
Windows Server 2012 R2 樹系功能等級不提供任何新功能，但可確保樹系中建立的任何新網域都會自動在 Windows Server 2012 R2 網域功能等級運作。

### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>新網域建立時強制執行的最小 DFL
Windows Server 2008 DFL 是新網域建立時支援的最小功能等級。

> [!NOTE]
> 藉由移除以 Windows Server 2008 的網域功能等級低於 Windows Server （含伺服器管理員或透過 Windows PowerShell）來安裝新網域的功能，可淘汰 FRS。

### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低樹系和網域功能等級
樹系和網域功能等級預設會在新網域和新樹系建立時設定為 Windows Server 2012 R2，但可使用 Windows PowerShell 降低。

若要使用 Windows PowerShell 提升或降低樹系功能等級，請使用 **set-adforestmode** Cmdlet。

**若要將 contoso.com FFL 設定為 Windows Server 2008 模式：**

```sql
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com
```

若要使用 Windows PowerShell 提高或降低網域功能等級，請使用 Windows2008r2forestset-addomainmode Cmdlet。

**若要將 contoso.com DFL 設定為 Windows Server 2008 模式：**

```powershell
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com
```

將執行 Windows Server 2012 R2 的 DC 升階成為執行 2003 DFL 之現有網域的額外複本。

在現有樹系中建立新網域

![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)

### <a name="adprep"></a>ADPREP.LOG
此版本中沒有新的樹系或網域作業。

這些 .ldf 檔案包含 **裝置註冊服務**的架構變更。

1.  Sch59

2.  Sch61

3.  Sch62

4.  Sch63

5.  Sch64

6.  Sch65

7.  Sch67

**工作資料夾：**

1.  Sch66

**MSODS**

1.  Sch60

**驗證原則與定址接收器**

1.  Sch68

2.  Sch69

## <a name="deprecation-of-ntfrs"></a><a name="BKMK_NTFRS"></a>NTFRS 淘汰

### <a name="overview"></a>概觀
FRS 已在 Windows Server 2012 R2 中淘汰。  藉由強制執行 Windows Server 2008 (DFL) 的最低網域功能等級，即可完成 FRS 的淘汰。  只有在使用伺服器管理員或 Windows PowerShell 建立新網域時，才會出現這項強制執行。

您可以使用-DomainMode 參數搭配 Install-addsforest 或 Install-addsdomain Cmdlet 來指定網域功能等級。  此參數支援的值可以是有效的整數或對應的列舉字串值。 例如，若要將網域模式層級設定為 Windows Server 2008 R2，您可以指定值4或 "Win2008R2"。  從 Server 2012 R2 中執行這些 Cmdlet 時，有效的值包括 Windows Server 2008 (3、Win2008) Windows Server 2008 R2 (4、Win2008R2) Windows Server 2012 (5、Win2012) 和 Windows Server 2012 R2 (6、Win2012R2) 。 網域功能等級不能低於、但可以高於樹系功能等級。  由於 FRS 在此版本中已被取代，因此從 Windows Server 2012 R2 執行時，Windows Server 2003 (2，Win2003) 不是可辨識的參數與這些 Cmdlet。

![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)

![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)

## <a name="ldap-query-optimizer-changes"></a><a name="BKMK_LDAPQuery"></a>LDAP 查詢最佳化工具變更

### <a name="overview"></a>概觀
LDAP 查詢最佳化工具演算法已重新評估並進一步優化。  結果是在 LDAP 搜尋效率和複雜查詢的 LDAP 搜尋時間方面的效能改進。

> [!NOTE]
> <strong>從開發人員：</strong>透過從 LDAP 查詢到 ESE 查詢的對應改善搜尋效能改進。  超過特定複雜性層級的 LDAP 篩選準則會防止優化索引的選取，進而大幅降低效能 (1000x 或更) 。 這項變更改變了針對 LDAP 查詢選取索引以避免這個問題的方式。
>
> [!NOTE]
> 全面徹底的 LDAP 查詢最佳化工具演算法，產生：
>
> -   更快速的搜尋時間
> -   提高效率可讓 Dc 完成更多工具
> -   不支援 AD 效能問題的相關呼叫
> -   回到 Windows Server 2008 R2 (KB 2862304) 

### <a name="background"></a>背景
搜尋 Active Directory 的能力是網域控制站所提供的核心服務。  其他服務和企業營運應用程式依賴 Active Directory 搜尋。  如果無法使用這項功能，商務作業可能會停止運作。  在使用核心和大量使用的服務時，網域控制站必須有效率地處理 LDAP 搜尋流量。  LDAP 查詢最佳化工具演算法會嘗試將 LDAP 搜尋篩選器對應至結果集，讓 LDAP 搜尋更有效率，方法是將 LDAP 搜尋篩選器對應到可透過已在資料庫中建立索引的記錄來滿足。  此演算法已重新評估並進一步優化。  結果是在 LDAP 搜尋效率和複雜查詢的 LDAP 搜尋時間方面的效能改進。

### <a name="details-of-change"></a>變更的詳細資料
LDAP 搜尋包含：

-   階層中 (NC 標頭、OU、物件) 的位置，以開始搜尋

-   搜尋篩選準則

-   要傳回的屬性清單

搜尋程式可摘要如下：

1.  盡可能簡化搜尋篩選準則。

2.  選取一組會傳回最小涵蓋集合的索引鍵。

3.  執行一或多個索引鍵的交集，以減少涵蓋的設定。

4.  針對涵蓋的集合中的每一筆記錄，評估篩選條件運算式和安全性。 如果篩選準則評估為 TRUE，並授與存取權，則會將此記錄傳回用戶端。

LDAP 查詢優化工作會修改步驟2和3，以縮減涵蓋集的大小。 更具體來說，目前的執行會選取重複的索引鍵並執行多餘的交集。

### <a name="comparison-between-old-and-new-algorithm"></a>新舊演算法的比較
在此範例中，無效率 LDAP 搜尋的目標是 Windows Server 2012 網域控制站。  因為找不到更有效率的索引，所以搜尋會在大約44秒內完成。

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

### <a name="sample-results-using-the-new-algorithm"></a>使用新演算法的範例結果
此範例會重複上述完全相同的搜尋，但以 Windows Server 2012 R2 網域控制站為目標。  由於 LDAP 查詢最佳化工具演算法的改進，相同的搜尋會在不到一秒內完成。

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

-   如果無法優化樹狀結構：

    -   例如：樹狀結構中的運算式是在未建立索引的資料行上

    -   記錄防止優化的索引清單

    -   透過 ETW 追蹤和事件識別碼1644公開

        ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)

### <a name="to-enable-the-stats-control-in-ldp"></a><a name="BKMK_EnableStats"></a>啟用 LDP 中的 Stats 控制項

1.  開啟 LDP.exe，並連接並系結至網域控制站。

2.  在 [ **選項** ] 功能表上，按一下 [ **控制項**]。

3.  在 [控制項] 對話方塊中，展開 [ **載入預先定義** ] 下拉式功能表，然後按一下 [ **搜尋統計** 資料]，再按一下 **[確定]**。

    ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)

4.  在 [**流覽]** 功能表上，按一下 [**搜尋**]

5.  在 [搜尋] 對話方塊中，選取 [ **選項** ] 按鈕。

6.  確定已在 [搜尋選項] 對話方塊中選取 [ **擴充** ] 核取方塊，然後選取 **[確定]**。

    ![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)

### <a name="try-this-use-ldp-to-return-query-statistics"></a>試試看：使用 LDP 傳回查詢統計資料
請在網域控制站上，或從已安裝 AD DS 工具的已加入網域的用戶端或伺服器執行下列動作。  針對您的 Windows Server 2012 DC 和您的 Windows Server 2012 R2 DC，重複下列目標。

1.  請參閱「 [建立更有效率的 MICROSOFT AD 應用程式](/previous-versions/ms808539(v=msdn.10)) 」一文，並視需要回頭查看。

2.  使用 LDP、啟用搜尋統計資料 (查看 [如何啟用 LDP 中的 Stats 控制項](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats)) 

3.  進行數個 LDAP 搜尋，並觀察結果頂端的統計資訊。  您將在其他活動中重複相同的搜尋，以將其記錄在記事本文字檔中。

4.  執行 LDAP 搜尋，讓查詢最佳化工具能夠優化，因為屬性索引

5.  嘗試建立需要很長時間才能完成的搜尋 (您可能會想要增加 **時間限制** 選項，讓搜尋不會將) 超時。

### <a name="additional-resources"></a>其他資源
[什麼是 Active Directory 搜尋？](/previous-versions/windows/it-pro/windows-server-2003/cc783845(v=ws.10))

[Active Directory 搜尋的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc755809(v=ws.10))

[建立更有效率的 Microsoft Active Directory 啟用應用程式](/previous-versions/ms808539(v=msdn.10))

[951581](https://support.microsoft.com/kb/951581) LDAP 查詢執行速度比 AD 或 LDS/ADAM 目錄服務中的預期更慢，而且可能會記錄事件識別碼1644

## <a name="1644-event-improvements"></a><a name="BKMK_1644"></a>1644 事件改進

### <a name="overview"></a>概觀
此更新會將額外的 LDAP 搜尋結果統計資料新增至事件識別碼1644，以協助進行疑難排解。  此外，還有新的登錄值，可用來啟用以時間為基礎的閾值記錄。  這些改良功能已在 Windows Server 2012 和 Windows Server 2008 R2 SP1 中透過 KB [2800945](https://support.microsoft.com/kb/2800945) 提供，且將可供 windows SERVER 2008 SP2 使用。

> [!NOTE]
> -   額外的 LDAP 搜尋統計資料會新增至事件識別碼1644，以協助針對無效率或昂貴的 LDAP 搜尋進行疑難排解
> -   您現在可以指定搜尋時間閾值 (例如 記錄事件1644，讓搜尋花費的時間超出100毫秒) 而不是指定昂貴且效率不佳的搜尋結果臨界值

### <a name="background"></a>背景
針對 Active Directory 效能問題進行疑難排解時，可能會發現 LDAP 搜尋活動可能會對此問題造成影響。  您決定啟用記錄功能，讓您可以看到網域控制站處理的昂貴或無效率的 LDAP 查詢。  若要啟用記錄功能，您必須設定「欄位工程診斷」值，並可選擇性地指定昂貴/沒有效率的搜尋結果閾值。  當啟用欄位工程記錄層級的值為5時，符合這些準則的任何搜尋都會記錄在目錄服務事件記錄檔中，且事件識別碼為1644。

此事件包含：

-   用戶端 IP 和埠

-   開始節點

-   Filter

-   搜尋範圍

-   屬性選取

-   伺服器控制項

-   造訪的專案

-   傳回的專案

不過，事件中遺失重要資料，例如搜尋作業所花費的時間量，以及使用任何) 索引的 (。

#### <a name="additional-search-statistics-added-to-event-1644"></a>新增至事件1644的其他搜尋統計資料

-   使用的索引

-   參考的頁面

-   從磁片讀取的頁面

-   從磁片 preread 的頁面

-   修改的清除頁面

-   修改過的中途頁面

-   搜尋時間

-   防止優化的屬性

#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>事件1644記錄的新以時間為基礎的閾值登錄值
您可以指定搜尋時間閾值，而不是指定昂貴且沒有效率的搜尋結果臨界值。  如果您想要記錄 50 ms 或以上的所有搜尋結果，除了將 [欄位工程] 值設定為 [) ] 之外，您還可以指定 50 decimal/32 hex (。

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]
"Search Time Threshold (msecs)"=dword:00000032
```

#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>舊的和新事件識別碼1644的比較
OLD

![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)

NEW

![目錄服務更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)

#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>試試看：使用事件記錄檔傳回查詢統計資料

1.  針對您的 Windows Server 2012 DC 和您的 Windows Server 2012 R2 DC，重複下列目標。 在每次搜尋之後觀察兩個 Dc 上的事件識別碼1644s。

2.  使用 regedit，在 Windows Server 2012 R2 DC 上使用以時間為基礎的閾值，以及 Windows Server 2012 DC 上的舊方法來啟用事件識別碼1644記錄。

3.  進行數個超過閾值的 LDAP 搜尋，並觀察結果頂端的統計資訊。  使用您稍早記載的 LDAP 查詢，並重複相同的搜尋。

4.  執行 LDAP 搜尋，查詢最佳化工具無法優化，因為一或多個屬性未建立索引。

## <a name="active-directory-replication-throughput-improvement"></a><a name="BKMK_ADRepl"></a>Active Directory 複寫輸送量改進

### <a name="overview"></a>概觀
AD 複寫會針對其複寫傳輸使用 RPC。 根據預設，RPC 會使用8K 傳輸緩衝區和容量更大的封包大小。 這會產生最大的影響，其中傳送的實例會傳輸三個封包 (大約有15，000個數據) ，然後必須等待網路往返再傳送更多。 假設3ms 的往返時間，即使是在1Gbps 或 10 Gbps 的網路上，最高的輸送量仍會是大約40Mbps。

> [!NOTE]
> -   此更新會將 AD 複寫輸送量的最大值從40Mbps 調整至大約 600 Mbps。
>
>     -   它會增加 RPC 傳送緩衝區大小，以減少網路往返次數
> -   在高速、高延遲的網路上，效果會最明顯。

這項更新會將 RPC 傳送緩衝區大小從8K 變更為 256 KB，以增加大約 600 Mbps 的最大輸送量。  這項變更可讓 TCP 視窗大小成長超過8K，減少網路往返次數。

> [!NOTE]
> 沒有可設定的設定可修改此行為。

### <a name="additional-resources"></a>其他資源
[Active Directory 複寫模型的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc772726(v=ws.10))

