---
description: 深入瞭解： SPN 和 UPN 的唯一性
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: SPN 和 UPN 的唯一性
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2cc9fa031ea41bddf76285cdeabb3ed33a10688a
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711753"
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 的唯一性

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**： Justin Turner，與 Windows 群組的資深支援擴大工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀
執行 Windows Server 2012 R2 的網域控制站會封鎖 (SPN) 和使用者主體名稱 (UPN) 建立重複的服務主體名稱。 這包括還原或重新引發已刪除的物件，或重新命名物件是否會導致重複。

### <a name="background"></a>背景
重複的服務主體名稱 (SPN) 通常會導致驗證失敗，而且可能會導致過多的 LSASS CPU 使用率。 沒有任何現成的方法可以封鎖重複的 SPN 或 UPN 的加入。 *

重複的 UPN 值會中斷內部部署 AD 與 Office 365 之間的同步處理。

* Setspn.exe 通常用來建立新的 Spn，而且功能已內建于 Windows Server 2008 發行的版本中，可新增重複的檢查。

**表 SEQ 資料表 \\ \* 阿拉伯文1： UPN 和 SPN 唯一性**

|功能|註解|
|-----------|-----------|
|UPN 唯一性|使用 Windows Azure AD 為基礎的服務（例如 Office 365）複製內部部署 AD 帳戶的 Upn 中斷同步處理。|
|SPN 唯一性|Kerberos 需要 Spn 以進行相互驗證。  重複的 Spn 會導致驗證失敗。|

如需 Upn 和 Spn 唯一性需求的詳細資訊，請參閱 [唯一性條件約束](/openspecs/windows_protocols/ms-adts/3c154285-454c-4353-9a99-fb586e806944)。

## <a name="symptoms"></a>徵兆
錯誤碼8467或8468或其十六進位、符號或字串對等專案會記錄在各種螢幕上的對話中，以及目錄服務事件記錄檔中的事件識別碼2974。 只有在下列情況下，才會封鎖嘗試建立重複的 UPN 或 SPN：

-   寫入是由 Windows Server 2012 R2 DC 所處理

**表 SEQ 資料表 \\ \* 阿拉伯文2： UPN 和 SPN 唯一性錯誤代碼**

|Decimal|Hex|符號|String|
|-----------|-------|------------|----------|
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供用於新增/修改的 SPN 值不是整個樹系的唯一值。|
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供來新增/修改的 UPN 值不是全樹系唯一的。|

## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果 UPN 不是唯一的，則會建立新的使用者

### <a name="dsamsc"></a>DSA.MSC
您所選擇的使用者登入名稱已在此企業中使用。 請選擇另一個登入名稱，然後再試一次。

![顯示訊息的螢幕擷取畫面，其中指出您所選擇的登入名稱已在使用中。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)

修改現有的帳戶：

指定的使用者登入名稱已存在於企業中。 您可以藉由變更首碼，或從清單中選取不同的尾碼，以指定新的尾碼。

![顯示訊息的螢幕擷取畫面，其中指出您使用的登入名稱已存在於企業中。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)

### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心 ( # A0) 
嘗試以已存在的 UPN 在 Active Directory 管理中心中建立新的使用者，將會產生下列錯誤。

![顯示訊息的螢幕擷取畫面，指出未建立新的使用者。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文1當新使用者建立因為 UPN 重複而失敗時，AD 管理中心顯示的錯誤**

### <a name="event-2974-source-activedirectory_domainservice"></a>事件2974來源： ActiveDirectory_DomainService
![顯示的螢幕擷取畫面 ](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文2事件識別碼2974，錯誤8648**

事件2974會列出已封鎖的值，以及一個或多個物件的清單， (最多10個) 已經包含該值。  在下圖中，您可以看到 UPN 屬性值 **<em>dhunt@blue.contoso.com</em>** 已存在於其他四個物件上。  由於這是 Windows Server 2012 R2 中的新功能，因此當下層 Dc 處理寫入嘗試時，仍會發生在混合式環境中意外建立重複的 UPN 和 Spn。

![顯示阿拉伯文2事件識別碼2974的螢幕擷取畫面，錯誤8648。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文3事件2974顯示包含重複 UPN 的所有物件**

> [!TIP]
> 定期審核事件識別碼2974s 以：
>
> -   識別嘗試建立重複的 UPN 或 Spn
> -   識別已包含重複專案的物件

8648 = "作業失敗，因為提供用於加法/修改的 UPN 值不是唯一的全樹系。」

### <a name="setspn"></a>SetSPN
Setspn.exe 在使用 **"-S"** 選項時，自 Windows Server 2008 版起，內建了重複的 SPN 偵測。  不過，您可以使用 **"-A"** 選項來略過重複的 SPN 偵測。  使用 SetSPN 搭配-A 選項，在以 Windows Server 2012 R2 DC 為目標時，會封鎖建立重複的 SPN。  顯示的錯誤訊息與使用-S 選項時所顯示的錯誤訊息相同：「找到重複的 SPN，正在中止作業！」

### <a name="adsiedit"></a>ADSI

```
Operation failed. Error code: 0x21c8
The operation failed because UPN value provided for addition/modification is not unique forest-wide.
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)
```

![顯示作業失敗且錯誤碼為0x21c8 的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文4已封鎖新增 UPN 時，ADSIEdit 中顯示的錯誤訊息**

### <a name="windows-powershell"></a>Windows PowerShell
Windows Server 2012 R2：

![顯示訊息的螢幕擷取畫面，指出作業失敗。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)

從伺服器2012（以 Windows Server 2012 R2 DC 為目標）執行的 PS：

![顯示不明錯誤的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)

以 Windows server 2012 R2 DC 為目標的 Windows Server 2012 上執行的 DSAC.exe：

![在以 Windows Server 2012 R2 DC 為目標時，顯示非 Windows Server 2012 R2 上的使用者建立錯誤的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文 5 dsac.exe 在以 windows Server 2012 r2 DC 為目標時，非 Windows Server 2012 R2 上的使用者建立錯誤**

![在以 Windows Server 2012 R2 DC 為目標時，顯示非 Windows Server 2012 R2 上使用者修改錯誤的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文 6 Dsac.exe 以 windows Server 2012 r2 DC 為目標時，非 Windows Server 2012 R2 上的使用者修改錯誤**

### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>還原可能導致重複 UPN 的物件失敗：
![顯示如何還原物件的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)

![螢幕擷取畫面，顯示作業失敗，因為提供用於新增/修改的 UPN 值不是整個樹系的唯一值。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)

當物件因為 UPN/SPN 重複而無法還原時，不會記錄任何事件。

物件的 UPN 必須是唯一的，才能進行還原。

1.  識別存在於物件中的 UPN 資源回收筒

2.  識別具有相同值的所有物件

3.  移除重複的 UPN (s) 

### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>識別已刪除 objectUsing 上的衝突 UPN repadmin.exe

```
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname
```

```
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname

C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object
s,DC=blue,DC=contoso,DC=com
    1> userPrincipalName: dhunt@blue.contoso.com
```

### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>使用相同的 UPN 識別所有物件：使用 Repadmin.exe

```
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN

C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com
```

> [!TIP]
> repadmin.exe 中先前未記載的 **/deleted** 參數是用來將已刪除的物件包含在結果集中。

### <a name="using-global-search"></a>使用全域搜尋

-   開啟 Active Directory 管理中心並流覽至 **全域搜尋**

-   選取 [ **轉換為 LDAP** ] 選項按鈕

-   輸入 **(userPrincipalName =*ConflictingUPN*)**

    -   以衝突的實際 UPN 取代 **_ConflictingUPN_* _

-   Select _ *Apply**

![顯示全域搜尋頁面的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)

### <a name="using-windows-powershell"></a>使用 Windows PowerShell

```
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com
```

![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)

如果需要還原物件，您將需要從其他物件中移除重複的 Upn。  對於只有一個物件，使用 ADSIEdit 來移除重複項就夠簡單了。  如果有多個物件有重複專案，則 Windows PowerShell 可能是更好的工具。

若要使用 Windows PowerShell 以 null 輸出 UserPrincipalName 屬性：

![顯示操作失敗的螢幕擷取畫面，錯誤碼為0x21c7。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)

> [!NOTE]
> UserPrincipalName 屬性是單一值屬性，因此此程式只會移除重複的 UPN。

### <a name="duplicate-spn"></a>重複的 SPN
![當封鎖新增重複的 SPN 時，顯示 ADSIEdit 中顯示之錯誤訊息的螢幕擷取畫面。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文8當新增重複的 SPN 遭到封鎖時，ADSIEdit 中顯示的錯誤訊息**

記錄在目錄服務事件記錄檔中是 **ActiveDirectory_DomainService** 事件識別碼 **2974**。

```
Operation failed. Error code: 0x21c7
The operation failed
The attribute value provided is not unique in the forest or partition. Attribute:
servicePrincipalName Value=<SPN>
<Object DN> Winerror: 8467
```

![顯示建立重複的 SPN 時所記錄之錯誤的螢幕擷取畫面已被封鎖。](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文9建立重複的 SPN 時所記錄的錯誤已遭封鎖**

### <a name="workflow"></a>工作流程

-   **如果 DC = = GC**

    -   不需要 offbox 呼叫，您可以在本機滿足查詢

    -   **_UPN 案例_* _

        -   查詢提供的 UPN (_userPrincipalName 的本機樹系通用 UPN 索引;全域索引 * ) 

            -   如果傳回的專案 = = 0-> 寫入繼續

            -   如果傳回的專案！ = 0-> 寫入失敗

                -   已記錄事件

                -   也會傳回擴充錯誤：

                    -   **8648：**

                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*

    -   ***SPN 案例** _

        -   查詢提供的 SPN (_servicePrincipalName 的本機整個樹系 SPN 索引;全域索引 * ) 

            -   如果傳回的專案 = = 0-> 寫入繼續

            -   如果傳回的專案！ = 0-> 寫入失敗

                -   已記錄事件

                -   也會傳回擴充錯誤：

                    -   **8647：**

                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**

-   **如果 DC！ = GC**

    -   Offbox 呼叫 **需要** ，但不是關鍵，也就是這是最佳的唯一性檢查

        -   只有在找不到 GC 時，檢查才會繼續進行本機 DIT

        -   記錄的事件指出這類

    -   **_UPN 案例_* _

        -   對最接近的 GC 提交 LDAP 查詢？ 針對提供的 UPN (_userPrincipalName 查詢 GC 的全樹系 UPN 索引;全域索引 * ) 

            -   如果傳回的專案 = = 0-> 寫入繼續

            -   如果傳回的專案！ = 0-> 寫入失敗

                -   已記錄事件

                -   也會傳回擴充錯誤：

                    -   **8648：**

                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*

    -   ***SPN 案例** _

        -   對最接近的 GC 提交 LDAP 查詢？ 查詢 GC 的全樹系 SPN 索引，提供給提供的 SPN (_servicePrincipalName;全域索引 * ) 

            -   如果傳回的專案 = = 0-> 寫入繼續

            -   如果傳回的專案！ = 0-> 寫入失敗

                -   已記錄事件

                -   也會傳回擴充錯誤：

                    -   **8647：**

                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*

當刪除的物件重新繪製動畫時，會檢查 SPN 或存在的 UPN 值是否為唯一性。 如果找到重複的，則要求會失敗。

-   針對某些屬性變更（例如 DNS 主機名稱、SAM 帳戶名稱等）進行修改時，會據以更新 Spn。 在此程式中會刪除過時的 Spn，並會建立新的 Spn，並將其新增至資料庫。 針對此路徑觸發的必要屬性修改為：

    -   ATT_DNS_HOST_NAME

    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME

    -   ATT_SAM_ACCOUNT_NAME

    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME

    -   ATT_SERVER_REFERENCE_BL

    -   ATT_USER_ACCOUNT_CONTROL

如果有任何新的 SPN 值重複，我們就無法進行修改。 在上述清單中，重要的屬性是 ATT_DNS_HOST_NAME (機名稱) 和 ATT_SAM_ACCOUNT_NAME (SAM 帳戶名稱) 。

### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>試試看：探索 SPN 和 UPN 的唯一性
這是此課程模組中的幾個「**試試這個**」活動的第一個。  此課程模組沒有個別的實驗室指南。  **試用這** 項活動基本上是自由形式的活動，可讓您在實驗室環境中探索課程內容。  您可以選擇遵循提示或關閉腳本，並提出您自己的活動。

> [!NOTE]
> -   這是「**試試這個**」活動的第一項。
> -   此課程模組沒有個別的實驗室指南。
> -   **試用這** 項活動基本上是自由形式的活動，可讓您在實驗室環境中探索課程內容。
> -   您可以選擇遵循提示或關閉腳本，並提出您自己的活動。
> -   雖然並非所有章節都有 **試用這個** 提示，但仍建議您在適當的情況下流覽實驗室中的課程內容。

使用 SPN 和 UPN 的唯一性進行實驗。  遵循這些提示或完成您自己的提示。

1.  使用 UPN 建立新的使用者

2.  建立具有 Spn 的帳戶

3.  使用先前已定義或變更現有帳戶 UPN 的 UPN 來建立新的使用者。  對另一個帳戶的 SPN 進行相同的動作

    1.  使用已在使用中的 UPN 填入現有的使用者帳戶

        1.  使用 PowerShell、ADSIEDIT 或 Active Directory 管理中心 ( # A0) 

    2.  使用已在使用中的 SPN 填入現有的帳戶

        1.  使用 Windows PowerShell、ADSIEDIT 或 SetSPN

4.  觀察錯誤

**選擇**

1.  驗證教室講師確定可以在 Active Directory 管理中心中啟用 *[AD 資源回收筒](../../get-started/adac/advanced-ad-ds-management-using-active-directory-administrative-center--level-200-.md#BKMK_EnableRecycleBin)* 。  如果是，請移至下一個步驟。

2.  在使用者帳戶上填入 UPN

3.  刪除帳戶

4.  使用與已刪除帳戶相同的 UPN 來填入不同的帳戶

5.  嘗試使用資源回收筒 GUI 來還原帳戶

6.  假設您剛剛看到在上一個步驟中看到的錯誤。   (，且沒有您剛剛執行的步驟歷程記錄) 您的目標是要完成帳戶的還原。  如需範例步驟，請參閱活頁簿。

