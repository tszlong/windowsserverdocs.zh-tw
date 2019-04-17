---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: "SPN 和 UPN 唯一性"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 唯一性

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
## <a name="overview"></a>概觀  
重複執行 Windows Server 2012 R2 封鎖建立網域控制站的主體名稱 (SPN) 服務及使用者主體名稱 (UPN)。 這包括如果還原或重新刪除物件的引發的物件重新命名，會導致複本。  
  
### <a name="background"></a>背景  
重複服務主體名稱 (SPN) 通常會發生導致驗證失敗並過 LSASS CPU 使用率可能會導致。 有不是方塊中封鎖重複 SPN 或 UPN 加入。 *  
  
重複 UPN 值中斷先之間同步處理 AD 與 Office 365。  
  
*Setspn.exe 通常用來建立新的 Spn 和功能建置到與 Windows Server 2008 新增檢查有重複，一起發行的版本。  
  
**表格 7 表格 \\\ * 阿拉伯文 1: UPN 和 SPN 唯一性**  
  
|功能|意見|  
|-----------|-----------|  
|UPN 唯一性|重複 Upn 中斷同步處理的先 AD 帳號，例如 Office 365 Windows Azure AD 型服務。|  
|SPN 唯一性|Kerberos 需要 Spn 互加好友的驗證。  重複 Spn 導致驗證失敗。|  
  
如 Upn 和 Spn 唯一性需求的相關詳細資訊，請查看[唯一性限制](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>症狀  
錯誤碼 8467 或 8468 或符號十六進位或字串對等登入各種螢幕 dialogues 在事件 ID 2974 Directory 服務事件登入。 只有在下列環境封鎖建立重複 UPN 或 SPN 嘗試：  
  
-   Windows Server 2012 R2 俠處理寫入  
  
**表格 7 表格 \\\ * 阿拉伯文 2: UPN 和 SPN 唯一性錯誤碼**  
  
|小數點|16 進位|符號|字串|  
|-----------|-------|------------|----------|  
|8467|21C 7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|因為 SPN 值提供除了日修改不獨特的樹系作業失敗。|  
|8648|21C 8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|因為 UPN 值提供除了日修改不獨特的樹系作業失敗。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果不是唯一 UPN 建立新的使用者會失敗  
  
### <a name="dsamsc"></a>DSA.msc  
選擇您的使用者登入名稱已在企業中使用。 選擇其他登入的名稱，並再試一次。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改現有 account:  
  
登入指定的使用者名稱已存在企業版。 指定新的藉由變更前置詞或從清單中選取不同的結尾。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 系統管理員中心 (DSAC.exe)  
嘗試在 Active Directory 管理中心與已經 UPN 建立新的使用者產生下列錯誤。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**圖 7 圖 \\\ * 顯示廣告管理中心新的使用者建立失敗時因重複 UPN 阿拉伯文 1 錯誤**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>事件 2974年來源： ActiveDirectory_DomainService  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文 2 事件 ID 2974 8648 錯誤**  
  
事件 2974年列出封鎖的值與一份已經包含值一或多個物件 （最多 10)。  下圖，您可以看到該 UPN 屬性的值*** dhunt@blue.contoso.com ***已經在四個其他的物件。  這是 Windows Server 2012 R2 的新功能，因為重複 UPN 和 Spn 意外建立混合的環境中仍然會時舊版 Dc 處理寫入嘗試。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文 3 事件 2974 顯示所有包含重複 UPN 的物件**  
  
> [!TIP]  
> 定期到檢視事件 ID 2974s:  
>   
> -   若要建立重複 UPN 或 Spn 嘗試找出  
> -   找出已包含重複的物件  
  
8648 = 」 操作失敗，因為 UPN 值提供除了日修改不獨特的樹系。 」  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe 有重複 SPN 偵測建以將它自 Windows Server 2008 發行之後，使用**「-S]**選項。  您可以使用略過重複 SPN 偵測到**「-A]**不過選項。  針對 Windows Server 2012 R2 DC SetSPN 使用-選項時，會被封鎖重複 SPN 建立。  是一個使用的-S 選項時，顯示一樣的顯示的錯誤訊息: 「 重複 SPN 找到，中止作業 」 ！  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**圖 7 圖 \\\ * 封鎖重複 UPN 加入時顯示 ADSIEdit 阿拉伯文 4 錯誤訊息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
針對 Windows Server 2012 R2 俠 Server 2012 PS 執行：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
針對 Windows Server 2012 R2 俠 Windows Server 2012 上執行 DSAC.exe:  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文 5 DSAC 使用者建立錯誤上非-Windows Server 2012 R2 時針對 Windows Server 2012 R2 俠**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文 6 DSAC 使用者修改錯誤上非-Windows Server 2012 R2 時針對 Windows Server 2012 R2 俠**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>還原的物件，會導致重複 UPN 失敗：  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
事件不登入物件失敗而重複 UPN 還原時日 SPN。  
  
必須順序還原的唯一 UPN 的物件。  
  
1.  找出 UPN 在於物件的資源回收筒  
  
2.  找出有相同的值所有物件  
  
3.  移除重複 UPN(s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>找出發生衝突 UPN 刪除的 objectUsing repadmin.exe 上  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>使用的相同 UPN 找出所有物件： 使用 Repadmin.exe  
  
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
> 未記載先前**/ 刪除**中 repadmin.exe 參數用來包含刪除的物件結果集中  
  
### <a name="using-global-search"></a>使用通用搜尋  
  
-   Active Directory 管理中心開放並瀏覽至**全域搜尋**  
  
-   選取 [**轉換成 LDAP**按鈕  
  
-   輸入* *(userPrincipalName =*ConflictingUPN*) * *  
  
    -   取代***ConflictingUPN***的實際 UPN 衝突中  
  
-   選取 [**適用於**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果需要還原物件，您將需要移除重複 Upn 其他物件。  只有一個物件，很簡單 ADSIEdit 移除重複使用。  如果有多個物件的重複項目，Windows PowerShell 可能會使用更好的工具。  
  
若要使用 Windows PowerShell UserPrincipalName 屬性出空值：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> 此程序僅會移除重複 UPN 單一值屬性，是 userPrincipalName 屬性。  
  
### <a name="duplicate-spn"></a>重複 SPN  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**圖 7 圖 \\\ * 封鎖重複 SPN 加入時顯示 ADSIEdit 阿拉伯文 8 錯誤訊息**  
  
事件登入是 Directory 服務登入**ActiveDirectory_DomainService** 263 **2974年**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文封鎖建立重複 SPN 時登入的 9 錯誤**  
  
### <a name="workflow"></a>工作流程  
  
-   **如果俠 = GC**  
  
    -   需要不 offbox 通話，可以在本機滿意查詢  
  
    -   ***UPN 案例***  
  
        -   查詢本機樹系 UPN 索引提供 UPN (*userPrincipalName; 全球索引*)  
  
            -   如果退貨項目 = 0-> 寫入進行  
  
            -   如果退貨項目 ！ = 0-> 寫入失敗  
  
                -   事件登入  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   查詢本機樹系 SPN 索引提供 SPN (*servicePrincipalName; 全球索引*)  
  
            -   如果退貨項目 = 0-> 寫入進行  
  
            -   如果退貨項目 ！ = 0-> 寫入失敗  
  
                -   事件登入  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **如果俠 ！ = GC**  
  
    -   Offbox 通話**理想**，而不是重要了，也就是此最佳唯一性核取  
  
        -   如果找不到 GC，進行針對本機 DIT 核取  
  
        -   事件指出，例如登入  
  
    -   ***UPN 案例***  
  
        -   提交接近 GC LDAP 查詢嗎？ 查詢 GC 的樹系 UPN 索引提供 UPN (*userPrincipalName; 全球索引*)  
  
            -   如果退貨項目 = 0-> 寫入進行  
  
            -   如果退貨項目 ！ = 0-> 寫入失敗  
  
                -   事件登入  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   提交接近 GC LDAP 查詢嗎？ 查詢 GC 的樹系 SPN 索引提供 SPN (*servicePrincipalName; 全球索引*)  
  
            -   如果退貨項目 = 0-> 寫入進行  
  
            -   如果退貨項目 ！ = 0-> 寫入失敗  
  
                -   事件登入  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
重新動畫刪除的物件時，已唯一 SPN 或 UPN 的值。 如果找到有重複，此要求將會失敗。  
  
-   DNS 主機名稱坡 Account 名稱等等，例如的某些屬性對時進行修改，Spn 已隨之更新。 在 [處理程序，過時 Spn 刪除及 Spn 新的建構新增至資料庫。 針對此路徑觸發必要屬性修改︰  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果任何新 SPN 值的重複，我們將會失敗修改。 上述清單中，重要屬性很 ATT_DNS_HOST_NAME （電腦名稱） 和 ATT_SAM_ACCOUNT_NAME （坡 Account 名稱）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>試試看： 探索 SPN 和 UPN 唯一性  
這是數個的第一個 」**請嘗試**「 單元活動。  不是另一個實驗室指南這個模組。  **請嘗試**活動的基本上任意活動，可讓您瀏覽課程資料實驗室環境中的。  您有下列命令提示字元中，或前往關閉指令碼的選項，並會使用您自己的活動。  
  
> [!NOTE]  
> -   這是數個的第一個 」**請嘗試**「 活動。  
> -   不是另一個實驗室指南這個模組。  
> -   **請嘗試**活動的基本上任意活動，可讓您瀏覽課程資料實驗室環境中的。  
> -   您有下列命令提示字元中，或前往關閉指令碼的選項，並會使用您自己的活動。  
> -   並非所有的區段有時**請嘗試**命令提示字元中，您都仍然來探索課程 content 實驗室在適當的位置。  
  
實驗 SPN 和 UPN 唯一性。  這些提示，請依照下列或自己完成。  
  
1.  建立新的使用者使用 UPN  
  
2.  建立帳號 Spn 使用  
  
3.  請使用已預先定義 UPN 建立新的使用者，或變更現有 account UPN。  執行相同的另一個 account 上 SPN  
  
    1.  填入 UPN 使用中的現有的使用者帳號  
  
        1.  使用 PowerShell、 ADSIEDIT 或 Active Directory 系統管理員中心 (DSAC.exe)  
  
    2.  填入 SPN 使用中的現有 account  
  
        1.  使用 Windows PowerShell、 ADSIEDIT 或 SetSPN  
  
4.  觀察錯誤  
  
**選擇**  
  
1.  教室講師驗證是要確定] * [AD 資源回收筒]](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin) *中 Active Directory 管理中心。  若是如此，請移至下一個步驟。  
  
2.  填入使用者帳號 UPN  
  
3.  Delete account  
  
4.  填入刪除 account 做的相同 UPN 使用不同的帳號  
  
5.  嘗試使用 [資源回收筒 GUI 還原 account  
  
6.  請想像您只要呈現給您在上一個步驟中看到此錯誤。  （和不需要您執行的步驟歷史）您的目標是完成的 account 還原。  查看例如步驟活頁簿。  
  


