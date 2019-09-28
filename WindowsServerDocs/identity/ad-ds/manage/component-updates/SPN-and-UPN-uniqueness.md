---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: SPN 和 UPN 的唯一性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ded707276471fccd28f0ec17afef0a24015ff32f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390035"
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 的唯一性

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**：Justin Turner，Microsoft 團隊的資深支援擴大工程師  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
執行 Windows Server 2012 R2 的網域控制站會阻止建立重複的服務主體名稱（SPN）和使用者主體名稱（UPN）。 這包括若還原或重新引發已刪除的物件，或物件的重新命名會導致重複。  
  
### <a name="background"></a>背景  
經常會發生重複的服務主體名稱（SPN），因而導致驗證失敗，而且可能會導致過多的 LSASS CPU 使用率。 沒有內建方法可封鎖重複的 SPN 或 UPN 的加入。 *  
  
重複的 UPN 值會中斷內部部署 AD 與 Office 365 之間的同步處理。  
  
\* Setspn 通常用來建立新的 Spn，而功能內建在 Windows Server 2008 發行的版本中，這會新增重複檢查。  
  
@no__t 0Table SEQ 資料表 \\ @ no__t-2 阿拉伯文1：UPN 和 SPN 唯一性 @ no__t-0  
  
|功能|註解|  
|-----------|-----------|  
|UPN 唯一性|重複的 Upn 會中斷內部部署 AD 帳戶與 Windows Azure AD 型服務（例如 Office 365）的同步處理。|  
|SPN 唯一性|Kerberos 需要 Spn 以進行相互驗證。  重複的 Spn 會導致驗證失敗。|  
  
如需 Upn 和 Spn 之唯一性需求的詳細資訊，請參閱[唯一性條件約束](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>問題  
錯誤碼8467或8468或其十六進位、符號或字串對等專案會記錄在各種螢幕上的對話中，以及目錄服務事件記錄檔中的事件識別碼2974。 只有在下列情況下，才會封鎖建立重複 UPN 或 SPN 的嘗試：  
  
-   寫入是由 Windows Server 2012 R2 DC 處理  
  
@no__t 0Table SEQ 資料表 \\ @ no__t-2 阿拉伯文2：UPN 和 SPN 唯一性錯誤代碼 @ no__t-0  
  
|Decimal|Hex|符號|字串|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供給新增/修改的 SPN 值不是唯一全樹系。|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供給新增/修改的 UPN 值不是唯一全樹系。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果 UPN 不是唯一的，新的使用者建立將會失敗  
  
### <a name="dsamsc"></a>DSA.MSC  
您選擇的使用者登入名稱已在此企業中使用。 請選擇另一個登入名稱，然後再試一次。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改現有的帳戶：  
  
指定的使用者登入名稱已經存在於企業中。 藉由變更前置詞，或從清單中選取不同的尾碼，來指定一個新的。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心（DSAC.EXE .exe）  
嘗試在 Active Directory 管理中心中使用已經存在的 UPN 來建立新使用者，將會產生下列錯誤。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文1當新使用者建立因 UPN 重複而失敗時，在 AD 管理中心顯示的錯誤**  
  
### <a name="event-2974-source-activedirectory_domainservice"></a>事件2974來源：ActiveDirectory_DomainService  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文2事件識別碼2974，錯誤8648**  
  
事件2974會列出已封鎖的值，以及一或多個已包含該值的物件清單（最多10個）。  在下圖中，您可以看到 UPN 屬性值 **<em>dhunt@blue.contoso.com</em>** 已存在於其他四個物件上。  由於這是 Windows Server 2012 R2 的新功能，因此當下層 Dc 處理寫入嘗試時，仍然會在混合環境中意外建立重複的 UPN 和 Spn。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文3事件2974，顯示包含重複 UPN 的所有物件**  
  
> [!TIP]  
> 定期查看事件識別碼2974s：  
>   
> -   識別建立重複 UPN 或 Spn 的嘗試  
> -   識別已包含重複專案的物件  
  
8648 = "作業失敗，因為提供給新增/修改的 UPN 值不是全樹系的唯一。"  
  
### <a name="setspn"></a>SetSPN  
在使用 **"-S"** 選項時，從 Windows Server 2008 版本開始，Setspn 已內建重複的 SPN 偵測。  不過，您可以使用 **"-A"** 選項略過重複的 SPN 偵測。  使用 SetSPN 搭配-A 選項，以目標為 Windows Server 2012 R2 DC 時，會封鎖建立重複的 SPN。  所顯示的錯誤訊息與使用-S 選項時所顯示的相同：「發現重複的 SPN，正在中止操作！」  
  
### <a name="adsiedit"></a>ADSIEDIT  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文4當已封鎖重複的 UPN 時，會顯示在 ADSIEdit 中的錯誤訊息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
以 Windows Server 2012 R2 DC 為目標的伺服器2012執行的 PS：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
以 Windows server 2012 R2 DC 為目標的 Windows Server 2012 上執行的 DSAC.EXE：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文 5 DSAC.EXE 在以 Windows server 2012 R2 DC 為目標時，非 Windows Server 2012 R2 上的使用者建立錯誤**  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文 6 DSAC.EXE 以 Windows server 2012 R2 DC 為目標時，非 Windows Server 2012 R2 的使用者修改錯誤**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>還原會導致重複 UPN 的物件失敗：  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
當物件因為 UPN/SPN 重複而無法還原時，不會記錄任何事件。  
  
物件的 UPN 必須是唯一的，才能還原。  
  
1.  識別回收站中的物件上存在的 UPN  
  
2.  識別所有具有相同值的物件  
  
3.  移除重複的 UPN  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>識別已刪除之 objectUsing repadmin 上的衝突 UPN  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>識別具有相同 UPN 的所有物件：使用 Repadmin  
  
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
> 在 repadmin .exe 中，先前未記載的 **/deleted**參數是用來在結果集中包含已刪除的物件。  
  
### <a name="using-global-search"></a>使用全域搜尋  
  
-   開啟 Active Directory 管理中心並流覽至 [**全域搜尋**]  
  
-   選取 [**轉換為 LDAP** ] 選項按鈕  
  
-   類型 **（userPrincipalName =*ConflictingUPN*）**  
  
    -   將***ConflictingUPN***取代為發生衝突的實際 UPN  
  
-   選取**套用**  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果需要還原物件，您將需要從其他物件中移除重複的 Upn。  就只有一個物件而言，使用 ADSIEdit 來移除重複的就夠簡單。  如果有多個物件有重複的專案，則 Windows PowerShell 可能是較好的工具來使用。  
  
使用 Windows PowerShell 將 UserPrincipalName 屬性設為 null：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> UserPrincipalName 屬性是單一值屬性，因此此程式只會移除重複的 UPN。  
  
### <a name="duplicate-spn"></a>重複的 SPN  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文8當加入重複的 SPN 遭到封鎖時，ADSIEdit 中顯示的錯誤訊息**  
  
記錄在目錄服務事件記錄檔中是**ActiveDirectory_DomainService**事件識別碼**2974**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**圖 SEQ 圖 \\ @ no__t-2 阿拉伯文9在建立重複的 SPN 遭到封鎖時記錄的錯誤**  
  
### <a name="workflow"></a>工作流程  
  
-   **If DC = = GC**  
  
    -   不需要 offbox 呼叫，可以在本機滿足查詢  
  
    -   ***UPN 案例***  
  
        -   查詢所提供 UPN 的本機全樹系 UPN 索引（*userPrincipalName; 全域索引*）  
  
            -   如果傳回的專案 = = 0-> 寫入繼續  
  
            -   如果傳回的專案！ = 0-> 寫入失敗  
  
                -   記錄的事件  
  
                -   也會傳回擴充錯誤：  
  
                    -   **8648：**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   針對提供的 SPN （*servicePrincipalName; 一個全域索引*）查詢本機全樹系 SPN 索引  
  
            -   如果傳回的專案 = = 0-> 寫入繼續  
  
            -   如果傳回的專案！ = 0-> 寫入失敗  
  
                -   記錄的事件  
  
                -   也會傳回擴充錯誤：  
  
                    -   **8647：**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC！ = GC**  
  
    -   **需要**Offbox 呼叫但不重要，亦即，這是最佳的唯一性檢查  
  
        -   只有在無法找到 GC 時，檢查才會繼續進行本機 DIT  
  
        -   記錄的事件，以指出這種情況  
  
    -   ***UPN 案例***  
  
        -   要針對最接近的 GC 提交 LDAP 查詢嗎？ 查詢 GC 的全樹系 UPN 索引，適用于提供的 UPN （*userPrincipalName; 全域索引*）  
  
            -   如果傳回的專案 = = 0-> 寫入繼續  
  
            -   如果傳回的專案！ = 0-> 寫入失敗  
  
                -   記錄的事件  
  
                -   也會傳回擴充錯誤：  
  
                    -   **8648：**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   要針對最接近的 GC 提交 LDAP 查詢嗎？ 查詢 GC 的全樹系 SPN 索引，適用于所提供的 SPN （*servicePrincipalName; 全域索引*）  
  
            -   如果傳回的專案 = = 0-> 寫入繼續  
  
            -   如果傳回的專案！ = 0-> 寫入失敗  
  
                -   記錄的事件  
  
                -   也會傳回擴充錯誤：  
  
                    -   **8647：**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
當刪除的物件重新製作動畫時，系統會檢查存在的 SPN 或 UPN 值是否為唯一性。 如果找到重複的，要求就會失敗。  
  
-   針對某些屬性變更（例如 DNS 主機名稱、SAM 帳戶名稱等）進行修改時，會據以更新 Spn。 在此過程中，會刪除過時的 Spn，並建立新的 Spn，並將其新增至資料庫。 觸發此路徑的必要屬性修改為：  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果有任何新的 SPN 值重複，我們就無法進行修改。 在上述清單中，重要的屬性為 ATT_DNS_HOST_NAME （電腦名稱稱）和 ATT_SAM_ACCOUNT_NAME （SAM 帳戶名稱）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>試試看：探索 SPN 和 UPN 的唯一性  
這是課程模組中的幾個「**嘗試此**」活動的第一個。  此課程模組沒有個別的實驗室指南。  **試用這**項活動基本上是自由形式的活動，可讓您在實驗室環境中探索課程材料。  您可以選擇遵循 [提示] 或 [關閉腳本]，並啟動您自己的活動。  
  
> [!NOTE]  
> -   這是幾個「**試用此**」活動的第一個。  
> -   此課程模組沒有個別的實驗室指南。  
> -   **試用這**項活動基本上是自由形式的活動，可讓您在實驗室環境中探索課程材料。  
> -   您可以選擇遵循 [提示] 或 [關閉腳本]，並啟動您自己的活動。  
> -   雖然並非所有區段都有**嘗試使用此**提示，但仍建議您在適當的情況下探索實驗室中的課程內容。  
  
使用 SPN 和 UPN 的唯一性進行實驗。  請遵循這些提示，或完成您自己的指示。  
  
1.  使用 UPN 建立新的使用者  
  
2.  建立具有 Spn 的帳戶  
  
3.  請建立新的使用者，並在先前已定義過 UPN 或變更現有帳戶的 UPN。  對另一個帳戶上的 SPN 執行相同動作  
  
    1.  以已在使用中的 UPN 填入現有的使用者帳戶  
  
        1.  使用 PowerShell、ADSIEDIT 或 Active Directory 管理中心（DSAC.EXE .exe）  
  
    2.  以已在使用中的 SPN 填入現有帳戶  
  
        1.  使用 Windows PowerShell、ADSIEDIT 或 SetSPN  
  
4.  觀察錯誤  
  
**也**  
  
1.  向教室講師確認是否可以在 Active Directory 管理中心中啟用 *[AD 回收站](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* 。  若是如此，請繼續下一個步驟。  
  
2.  在使用者帳戶上填入 UPN  
  
3.  刪除帳戶  
  
4.  使用與已刪除帳戶相同的 UPN 填入不同的帳戶  
  
5.  嘗試使用回收站 GUI 來還原帳戶  
  
6.  假設您剛看到您在上一個步驟中看到的錯誤。  （而且沒有您剛執行之步驟的歷程記錄）您的目標是要完成帳戶的還原。  如需範例步驟，請參閱活頁簿。  
  


