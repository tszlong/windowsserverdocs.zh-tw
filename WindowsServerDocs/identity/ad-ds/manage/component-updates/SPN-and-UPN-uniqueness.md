---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: SPN 和 UPN 的唯一性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c9a769fdd9fb7d13c47da465b25bc59e7f55237f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856739"
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 的唯一性

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

**作者**:Justin Turner 資深支援高階工程師，與 Windows 群組  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
執行 Windows Server 2012 R2 區塊建立的網域控制站重複的服務主體名稱 (SPN) 和使用者主要名稱 (UPN)。 如果這個名稱包含還原或重新引發標記的已刪除的物件或重新命名物件，會導致重複。  
  
### <a name="background"></a>背景  
重複服務主要名稱 (SPN) 通常會發生並導致驗證失敗且可能會導致 LSASS CPU 使用率過高。 沒有任何內建方法，來封鎖，加上重複的 SPN 或 UPN。 *  
  
重複的 UPN 值中斷內部部署之間的同步處理 AD 和 Office 365。  
  
*Setspn.exe 通常用來建立新的 Spn，而且功能已內建新增的重複項目檢查的 Windows Server 2008 發行的版本。  
  
**資料表 SEQ 表格\\\*阿拉伯文 1:UPN 和 SPN 的唯一性**  
  
|功能|註解|  
|-----------|-----------|  
|UPN 的唯一性|重複的 upn，請中斷同步處理的內部部署 AD 帳戶與 Windows Azure AD 為基礎的服務，例如 Office 365。|  
|SPN 唯一性|Kerberos 進行相互驗證需要 Spn。  重複的 Spn 會導致驗證失敗。|  
  
如需有關部署之 Upn 和 Spn 的唯一性需求的詳細資訊，請參閱[唯一性條件約束](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>問題  
錯誤碼 8467 或 8468 或十六進位，符號或對等字串會記錄在各種螢幕之間的對話和目錄服務事件記錄檔中的事件識別碼 2974年中。 只有在下列情況下，會封鎖嘗試建立重複的 UPN 或 SPN:  
  
-   寫入處理由 Windows Server 2012 R2 網域控制站  
  
**資料表 SEQ 表格\\\*阿拉伯文 2:UPN 和 SPN 的唯一性錯誤碼**  
  
|十進位|Hex|符號|字串|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供給加入/修改的 SPN 值不是唯一的全樹系。|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|作業失敗，因為提供給加入/修改的 UPN 值不是唯一的全樹系。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>新的使用者建立失敗時，如果不是唯一的 UPN  
  
### <a name="dsamsc"></a>DSA.msc  
您選擇的使用者登入名稱已在企業中的使用中。 請選擇另一個登入名稱，並再試一次。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改現有的帳戶：  
  
在企業中已經存在指定的使用者登入名稱。 指定新的連線，藉由變更前置詞，或從清單中選取不同的後置詞。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心 (DSAC.exe)  
嘗試建立新的使用者在 Active Directory 管理中心內，使用已經存在的 UPN 會產生下列錯誤。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**圖 SEQ Figure \\ \*阿拉伯文 1 錯誤顯示在 AD 管理中心內，當新的使用者建立失敗，因為重複的 UPN**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>事件 2974年來源：ActiveDirectory_DomainService  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**圖 SEQ Figure \\ \* ARABIC 2 事件識別碼錯誤 8648 2974年**  
  
事件 2974年會列出已封鎖的值和一份已包含該值的一或多個物件 （最多 10 個）。  在下圖中，您可以看到該 UPN 屬性值***dhunt@blue.contoso.com***已在其他四個物件。  由於這是 Windows Server 2012 R2 中的新功能，則當下層網域控制站處理的 「 寫入 」 嘗試還是會發生重複的 UPN 和 Spn 意外建立混合式環境中。  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**圖 SEQ Figure \\ \* ARABIC 3 事件 2974 顯示所有物件，包含重複的 UPN**  
  
> [!TIP]  
> 事件識別碼 2974s年檢閱定期：  
>   
> -   識別嘗試建立重複的 UPN 或 Spn  
> -   找出已包含重複的物件  
  
8648 = 「 作業失敗，因為提供給加入/修改的 UPN 值不是唯一的全樹系。 」  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe 已內建 Windows Server 2008 發行以來，重複的 SPN 偵測，使用時 **"-S"** 選項。  您可以使用連線，略過重複的 SPN 偵測 **"-A"** 不過選項。  目標為 Windows Server 2012 R2 DC 使用 SetSPN-A 選項時，會封鎖建立重複的 SPN。  顯示的錯誤訊息是使用-S 選項時，顯示一個相同：「 重複的 SPN 發現，正在中止作業 ！ 」  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**圖 SEQ Figure \\ \*新增重複的 UPN 被封鎖時，顯示在 ADSIEdit 的阿拉伯文 4 錯誤訊息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
從目標為 Windows Server 2012 R2 DC Server 2012 PS 執行：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe 目標為 Windows Server 2012 R2 DC 的 Windows Server 2012 上執行：  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**圖 SEQ Figure \\ \* ARABIC 5 DSAC 使用者建立錯誤上非-Windows Server 2012 R2 時目標為 Windows Server 2012 R2 DC**  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**圖 SEQ Figure \\ \* ARABIC 6 DSAC 使用者修改錯誤上非-Windows Server 2012 R2 時目標為 Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>物件，重複的 UPN 會導致還原失敗：  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
會記錄任何事件，當物件失敗時還原因為重複的 UPN / SPN。  
  
物件的 UPN 必須是唯一，才能讓它要還原的。  
  
1.  找出資源回收筒中的物件存在的 UPN  
  
2.  識別具有相同值的所有物件  
  
3.  移除重複的 UPN(s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>識別在已刪除的 objectUsing repadmin.exe 衝突的 UPN  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>若要使用相同的 UPN 識別所有物件： 使用 Repadmin.exe  
  
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
> 先前未記載 **/ 刪除**在 repadmin.exe 中的參數用來包含結果集中的已刪除的物件  
  
### <a name="using-global-search"></a>使用全域搜尋  
  
-   開啟 Active Directory 管理中心瀏覽至**全域搜尋**  
  
-   選取 **轉換為 LDAP**選項按鈕  
  
-   Type **(userPrincipalName=*ConflictingUPN*)**  
  
    -   取代***ConflictingUPN***使用實際的 UPN 衝突  
  
-   選取**套用**  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果物件需要還原，您將需要移除重複的 Upn 其他物件。  對於只有一個物件，它十分簡單，要使用 ADSIEdit 來移除重複項目。  如果有多個重複項目物件，Windows PowerShell 可能是更好的工具，來使用。  
  
設定為 null 的 UserPrincipalName 屬性，使用 Windows PowerShell:  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> UserPrincipalName 屬性是單一值屬性，讓此程序只會移除重複的 UPN。  
  
### <a name="duplicate-spn"></a>重複的 SPN  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**圖 SEQ Figure \\ \*新增重複的 SPN 被封鎖時，顯示在 ADSIEdit 的阿拉伯文 8 錯誤訊息**  
  
記錄在事件記錄檔的目錄服務中**ActiveDirectory_DomainService**事件識別碼**2974年**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 和 UPN 的唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**圖 SEQ Figure \\ \*阿拉伯文 9 錯誤時無法建立重複的 SPN 的記錄**  
  
### <a name="workflow"></a>工作流程  
  
-   **If DC == GC**  
  
    -   沒有 networkconfig.netcfg 呼叫的必要項，可以在本機滿足查詢  
  
    -   ***UPN 案例***  
  
        -   查詢本機全樹系的 UPN 索引提供的 UPN (*userPrincipalName; 全域索引*)  
  
            -   如果傳回的項目 = = 0]-> [寫入會繼續進行  
  
            -   如果傳回的項目 ！ = 0]-> [寫入失敗  
  
                -   記錄事件  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   查詢本機樹系 SPN 索引提供的 spn (*servicePrincipalName; 全域索引*)  
  
            -   如果傳回的項目 = = 0]-> [寫入會繼續進行  
  
            -   如果傳回的項目 ！ = 0]-> [寫入失敗  
  
                -   記錄事件  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC != GC**  
  
    -   呼叫 Networkconfig.netcfg**理想**但不是嚴重，也就是這是最佳的唯一性檢查  
  
        -   找不到 GC 時，才繼續針對本機 DIT 核取  
  
        -   記錄檔指出這類的事件  
  
    -   ***UPN 案例***  
  
        -   提交最接近的 GC 的 LDAP 查詢？ 查詢 GC 的全樹系的 UPN 索引，對提供的 UPN (*userPrincipalName; 全域索引*)  
  
            -   如果傳回的項目 = = 0]-> [寫入會繼續進行  
  
            -   如果傳回的項目 ！ = 0]-> [寫入失敗  
  
                -   記錄事件  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 案例***  
  
        -   提交最接近的 GC 的 LDAP 查詢？ 查詢 GC 的全樹系 SPN 索引提供的 spn (*servicePrincipalName; 全域索引*)  
  
            -   如果傳回的項目 = = 0]-> [寫入會繼續進行  
  
            -   如果傳回的項目 ！ = 0]-> [寫入失敗  
  
                -   記錄事件  
  
                -   也會傳回延伸的錯誤：  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
已刪除的物件重新動畫時，存在的 SPN 或 UPN 值會檢查唯一性。 如果找到重複的要求將會失敗。  
  
-   針對特定的屬性變更，例如 DNS 主機名稱，SAM 帳戶名稱等，進行修改時，Spn 會隨之更新。 在過程中，刪除過時的 Spn，並會建構新的 Spn，並將其加入資料庫。 針對觸發此路徑的必要屬性修改為：  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果任何新的 SPN 值重複，無法修改。 上述清單中的重要屬性是 ATT_DNS_HOST_NAME （電腦名稱） 和 ATT_SAM_ACCOUNT_NAME （SAM 帳戶名稱）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>試試看吧：瀏覽 SPN 和 UPN 的唯一性  
這是一種第一 」**試試**」 模組中的活動。  沒有這個模組的另一個實驗室指南。  **試試**活動是基本上是自由格式的活動可讓您探索在實驗室環境中的課程資料。  您可以選擇下列提示字元，或將指令碼，並提供您自己的活動。  
  
> [!NOTE]  
> -   這是一種第一 」**試試**」 活動。  
> -   沒有這個模組的另一個實驗室指南。  
> -   **試試**活動是基本上是自由格式的活動可讓您探索在實驗室環境中的課程資料。  
> -   您可以選擇下列提示字元，或將指令碼，並提供您自己的活動。  
> -   雖然並非所有的各節含有**試試**提示字元中，您仍建議的探索實驗室的課程內容，在適當的地方。  
  
試驗 SPN 和 UPN 的唯一性。  請遵循這些提示，或您自己完成。  
  
1.  建立新的使用者的 upn  
  
2.  建立 Spn 的帳戶  
  
3.  建立新的使用者先前已定義的 upn 或變更現有帳戶的 UPN。  執行相同動作的另一個帳戶的 SPN  
  
    1.  填入現有的使用者帳戶已在使用中的 upn  
  
        1.  使用 PowerShell、 ADSIEDIT 或 Active Directory 管理中心 (DSAC.exe)  
  
    2.  填入現有的帳戶已在使用中的 spn  
  
        1.  使用 Windows PowerShell、 ADSIEDIT 或 SetSPN  
  
4.  觀察到的錯誤  
  
**（選擇性)**  
  
1.  與課堂講師，負責確認它是 [確定] 以啟用 *[AD 資源回收筒](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* 在 Active Directory 管理中心內。  如果是的話，移至下一個步驟。  
  
2.  填入使用者帳戶的 UPN  
  
3.  刪除帳戶  
  
4.  填入不同的帳戶，具有相同的 UPN，因為已刪除的帳戶  
  
5.  嘗試使用資源回收筒 GUI 來還原的帳戶  
  
6.  假設您只是提出與您在上一個步驟中看到的錯誤。  （而且沒有您剛執行的步驟執行的歷程記錄）您的目標是要完成還原的帳戶。  活頁簿，例如步驟，請參閱。  
  


