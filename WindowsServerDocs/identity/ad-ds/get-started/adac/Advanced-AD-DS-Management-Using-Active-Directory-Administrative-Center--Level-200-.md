---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: "使用系統管理員中心 (層級 200) Active Directory 進階的 AD DS 管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>使用系統管理員中心 (層級 200) Active Directory 進階的 AD DS 管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題中更多詳細資料，包括架構範例一般工作，以及疑難排解資訊涵蓋更新的 Active Directory 管理中心與其新 Active Directory 資源回收桶，Fine-grained 密碼原則，Windows PowerShell 歷史檢視器。 如簡介、 [Active Directory 系統管理員中心調節和 #40; 簡介層級 100 和 #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Active Directory 系統管理員中心架構](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [讓和管理 Active Directory 資源回收筒使用 Active Directory 系統管理員中心](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [設定及管理細緻密碼原則使用 Active Directory 系統管理員中心](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [使用 Active Directory 系統管理員中心 Windows PowerShell 歷史檢視器](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [疑難排解 AD DS 管理](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Active Directory 系統管理員中心架構  
  
### <a name="adprep-executables-dlls"></a>ADPrep 可執行檔 Dll  
Active Directory 管理中心基礎架構與模組未變更新資源回收筒、 與 FGPP、 歷史檢視器功能。  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
基礎 Windows PowerShell 和層級的作業資源回收筒的新功能的如下所示：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>讓和管理 Active Directory 資源回收筒使用 Active Directory 系統管理員中心  
  
### <a name="capabilities"></a>功能  
  
-   Windows Server 2012 Active Directory 系統管理員中心可讓您設定及管理 Active Directory 資源回收筒的森林中的任何網域磁碟分割。 不是使用 Windows PowerShell 或 Ldp.exe 讓 Active Directory 資源回收桶，或還原網域磁碟分割中的物件的需求。  
  
-   Active Directory 管理中心已進階篩選準則，在大許多刻意刪除物件的環境中進行目標的還原變得更容易。  
  
### <a name="limitations"></a>限制  
  
-   Active Directory 管理中心只能管理網域磁碟分割，因為它無法還原刪除的物件的設定、 DNS 網域或樹系 DNS 磁碟分割 (您無法從架構磁碟分割 delete 物件)。 若要還原物件的非網域磁碟分割，使用[還原-ADObject](https://technet.microsoft.com/library/ee617262.aspx)。  
  
-   Active Directory 管理中心將無法還原子樹單一動作中的物件。 例如，如果您 delete 巢 Ou、 使用者、 群組和電腦的組織單位，還原基底組織單位不會還原子女物件。  
  
    > [!NOTE]  
    > Active Directory 管理中心批次還原未 」 最佳成就] 有點刪除的物件*選擇只在*讓排序家長之前子女的 [還原] 清單。 簡單的測試案例，可能會在單一動作還原子樹物件。 但角落案例，例如包含部分樹-的家長刪除的節點遺失部分樹-選取範圍或錯誤案例，例如略過子女物件家長還原失敗時，可能無法如預期般運作。 基於這個原因，您應該會還原子樹的物件不同的動作之後的家長物件還原。  
  
Active Directory 資源回收桶需要 Windows Server 2008 R2 森林功能層級，您必須將企業系統管理員群組成員。 一旦支援，您無法停用 Active Directory 資源回收筒]。 Active Directory 資源回收桶放大 Active Directory 資料庫 (NTDS。DIT) 森林中的每個網域控制站上。 [資源回收筒] 所使用的磁碟空間持續增加一段時間為也會保留物件和屬性所有的資料。  
  
如需詳細資訊，請查看[Active Directory 資源回收筒中的指示](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx)和[（適用於 Active Directory 資源回收桶） 評估硬體需求](https://technet.microsoft.com/library/cc753439(WS.10).aspx)。  
  
您可以*低*樹系和網域功能層級的 Windows Server 2012 Windows Server 2008 R2 即使讓 Active Directory 資源回收筒。 這需要使用**設定為 ADForestMode**和**ADDomainMode 設定為**Active Directory cmdlet。  
  
例如：  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
您無法使用 Active Directory 管理中心進行此變更-只會引發功能層級。  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>讓 Active Directory 資源回收桶使用 Active Directory 管理中心  
若要讓 Active Directory 資源回收桶，請打開**Active Directory 管理中心**，按一下 [瀏覽窗格中樹系的名稱。 從**工作**窗格中，按**可讓資源回收筒]**。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Active Directory 管理中心會顯示**讓資源回收筒確認**對話方塊。 此對話方塊會警告您，讓資源回收筒是無法還原。 按一下**[確定]**以便 Active Directory 資源回收筒]。 Active Directory 管理中心會顯示提醒您所有的網域控制站將設定變更，直到不完整功能 Active Directory 資源回收筒的另一個對話方塊。  
  
> [!IMPORTANT]  
> 選項可讓 Active Directory 資源回收桶，即表示如果：  
>   
> -   樹系功能等級小於 Windows Server 2008 R2  
> -   它已  
  
相當於 Active Directory Windows PowerShell cmdlet 仍是：  
  
```  
Enable-ADOptionalFeature  
```  
  
如需有關如何使用 Windows PowerShell 來讓 Active Directory 資源回收筒的詳細資訊，請查看[Active Directory 資源回收筒中的指示](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx)。  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>管理 Active Directory 資源回收桶使用 Active Directory 管理中心  
本章節使用現有的網域名稱的範例**corp.contoso.com**。這個網域組織使用者到家長組織單位名為**使用者帳戶**。 **使用者帳戶**組織單位包含三個子女 Ou 漂流到由部門，讓每個進一步包含 Ou、 使用者和群組。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>儲存空間及篩選  
Active Directory 資源回收筒會保留所有在樹系的物件。 它會儲存於這些物件**msDS-deletedObjectLifetime**屬性，預設設定成符合**tombstoneLifetime**的樹系的屬性。 任何森林中建立使用 Windows Server 2003 SP1 或更新版本的值**tombstoneLifetime**預設為 180 天。 從 Windows 2000 升級或安裝 Windows Server 2003 (不 service pack) 的任何森林中未設定預設 tombstoneLifetime 屬性，並 Windows 因此使用內部 60 天為預設值。 這就是可設定。若要還原之子-森林網域磁碟分割從刪除任何物件，您可以使用 Active Directory 管理中心。 您必須使用 cmdlet 繼續**還原-ADObject**還原刪除物件的其他磁碟分割，例如 Configuration.Enabling 可 Active Directory 資源回收筒]**刪除物件**容器才在 Active Directory 管理中心在每個網域磁碟分割。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
**刪除物件**容器顯示您所有的可還原物件網域分割中。 刪除物件超過**msDS-deletedObjectLifetime**稱為回收物件。 Active Directory 管理中心不會顯示回收的物件，您將無法還原使用 Active Directory 管理中心這些物件。  
  
[資源回收筒] 架構與處理規則的深入解釋，請查看[AD 資源回收筒： 了解、 實作、 最佳方式，以及疑難排解](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx)。  
  
Active Directory 管理中心手動限制物件 20000 物件的容器返回預設的數目。 您可以將這項限制高達 100000 物件提高按一下**管理**功能表上，然後**管理清單選項**。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>還原  
  
##### <a name="filtering"></a>篩選  
Active Directory 管理中心提供強大條件和篩選的選項，您應該熟悉之前，您需要使用這些真實還原。 網域刻意 delete 透過他們期間的許多物件。與 180 天可能刪除的物件期間，您只是無法還原所有物件發生意外時。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
除了書寫複雜的 LDAP 篩選 UTC-10 值轉換日期和時間，使用 [基本和進階**篩選**功能表清單只有相關的物件。 如果您知道刪除、 物件的名稱或任何其他重要的資料，請使用優點時篩選。 按一下 > 形箭號 [搜尋] 方塊右邊切換進階的篩選選項。  
  
還原作業支援所有標準篩選準則選項，與任何其他搜尋相同。 建篩選，還原物件最重要的通常是：  
  
-   *ANR (明確的名稱解析-未列出的功能表上，但您輸入時 * * * 篩選 * * * 中)*  
  
-   上次修改之間指定日期  
  
-   物件是使用者/需要日電腦日群組日組織單位  
  
-   名稱  
  
-   時  
  
-   最後一個已知的父系  
  
-   輸入  
  
-   描述  
  
-   城市  
  
-   國家/地區的國家  
  
-   部門  
  
-   員工編號  
  
-   名字  
  
-   職稱  
  
-   姓氏  
  
-   SAMaccountname  
  
-   省/市  
  
-   電話號碼  
  
-   UPN  
  
-   郵遞區號  
  
您可以新增多個條件。 例如，您可以找到所有使用者物件刪除在 2016 年 9 月 24 日<sup>第</sup>對於工作管理員] 中的標題從芝加哥，伊利諾 2012年。  
  
您也可以新增、 修改或重新排列欄標頭評估的物件復原時提供更多詳細資料。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
如需模糊的名稱解析的詳細資訊，請查看[ANR 屬性](https://msdn.microsoft.com/library/ms675092(VS.85).aspx)。  
  
##### <a name="single-object"></a>單一物件  
還原刪除的物件一直一次。  Active Directory 管理中心可更輕鬆地操作。 若要還原刪除的物件，例如單一使用者：  
  
1.  按一下瀏覽窗格中的 Active Directory 管理中心網域名稱。  
  
2.  按兩下**刪除物件**在 [管理] 清單。  
  
3.  物件上按一下滑鼠右鍵，然後按一下**還原**，或按**還原**從**工作**窗格。  
  
此物件會還原到其原始位置。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
按一下**還原為...**變更還原位置。 如果刪除的物件的家長容器也已，但不是想要還原家長，這非常有用。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>多個等物件  
您可以還原多個等層級物件，例如 [中的所有使用者。 按住 CTRL 鍵，然後按一下您想要還原的一或多個刪除的物件。 按一下**還原**[工作] 窗格中。 您也可以選取 [顯示所有物件按住 CTRL 和金鑰，或使用 shift 鍵，然後按一下物件的範圍。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>多個家長和子女物件  
請務必因為 Active Directory 管理中心將無法還原的一項動作刪除物件巢的樹了解多 parent 子女還原程序。  
  
1.  還原樹上的最高的刪除的物件。  
  
2.  還原立即子女的家長物件。  
  
3.  還原立即這些家長物件的子女。  
  
4.  重複視還原所有物件。  
  
您將無法還原父之前先還原子物件。 嘗試這個還原傳回下列錯誤：  
  
**因為物件的父系是取消或刪除無法執行此作業。**  
  
**最後一個已知的父系**屬性顯示每個物件的父系關係。 **最後一個已知的父系**屬性變更刪除位置還原位置 Active Directory 管理中心重新整理之後還原家長時。 因此，您可以還原子女物件時父項目的位置不再顯示分辨刪除的物件的容器的名稱。  
  
請考慮系統管理員的身分不小心刪除銷售組織單位，其中包含子女 Ou 與使用者的案例。  
  
首先，觀察到的值**最後一個已知的父系**刪除所有使用者的屬性，它會讀取**組織單位 = Sales\0ADEL:*< guid + 刪除的物件的容器分辨名稱 >** *:  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
篩選返回刪除的組織單位，然後還原銷售明確的名稱：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
重新整理 Active Directory 管理中心查看刪除的使用者物件的最後一個已知的父系屬性還原銷售組織單位分辨名稱來變更：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
篩選銷售的所有使用者。 按住 CTRL，然後選取所有刪除的銷售使用者的按鍵。 按一下**還原**將物件的**刪除物件**群組成員資格與保留屬性銷售組織單位容器。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
如果**銷售**組織單位包含自己的子女 Ou，然後您可以還原之前先還原子女，子女 Ou 等。  
  
若要還原所有巢刪除的物件指定刪除的家長容器，查看[附錄 b 還原多個刪除 Active Directory 物件 （範例指令碼）](https://technet.microsoft.com/library/dd379504(WS.10).aspx)。  
  
Active Directory Windows PowerShell cmdlet 還原刪除的物件的是：  
  
```  
Restore-adobject  
  
```  
  
**還原-ADObject** Windows Server 2008 R2 和 Windows Server 2012 之間未變更 cmdlet 功能。  
  
##### <a name="server-side-filtering"></a>伺服器端篩選  
它可能會，長時間刪除物件的容器將累積超過 20000 （或甚至 100000） 中型與大型企業中的物件並聲音方面有困難顯示所有物件。 在 Active Directory 管理中心篩選機制依賴 client 端篩選，因為它無法顯示這些其他物件。 若要替代這項限制，請使用下列步驟來執行伺服器端搜尋：  
  
1.  以滑鼠右鍵按一下**刪除物件**容器和**在這個節點搜尋**。  
  
2.  按一下 > 形箭號公開**+ 新增條件**功能表上，選取並新增**之間上次修改日期指定**。 上次修改時間 ( **whenChanged**屬性) 時的刪除時間; 關閉大約在大部分的環境，都一樣。 這項查詢執行伺服器端搜尋。  
  
3.  找到要使用進一步顯示篩選排序，在結果中，依此類推還原刪除的物件，通常會還原它們。  
  
## <a name="BKMK_FGPP"></a>設定及管理細緻密碼原則使用 Active Directory 系統管理員中心  
  
### <a name="configuring-fine-grained-password-policies"></a>設定細緻密碼原則  
Active Directory 管理中心可讓您建立和管理 Fine-Grained 密碼原則 (FGPP) 物件。 Windows Server 2008 導入了 FGPP 功能，但 Windows Server 2012 的第一個管理圖形介面它。 您用於網域層級 Fine-Grained 密碼原則，它可以讓覆寫所需的 Windows Server 2003 的單一網域密碼。 使用不同的設定建立不同 FGPP，個人的使用者或群組取得不同的密碼原則網域中。  
  
有關 Fine-Grained 密碼原則，請查看[AD DS Fine-Grained 密碼，以及 Account 鎖定原則 Step-by-Step 指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
在瀏覽窗格中，按一下 [樹檢視您的網域，按一下**系統**，按一下 [**密碼設定容器**，然後在 [工作] 窗格中，按一下 [**新**和**密碼設定**。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>管理細緻密碼原則  
建立新的 FGPP 或編輯現有就會出現**設定密碼**編輯器。 從，您設定所有所需的密碼原則，您就必須在 Windows Server 2008 或 Windows Server 2008 R2、 只現在的特殊用途編輯器。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
填寫所需所有 （紅色星號） 和任何選擇性欄位，，然後按一下**新增**若要設定的使用者或群組接收這項原則。 FGPP 會覆寫預設網域原則設定，針對那些指定的安全性原則。 在上圖中非常限制原則僅適用於建，以避免危害。 原則太複雜標準使用者遵守，但適合高風險 account 僅供 IT 專業人員。  
  
您也可以設定優先順序，並原則套用指定的網域中的使用者和群組。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Active Directory Windows PowerShell cmdlet Fine-Grained 密碼原則︰  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
Windows Server 2012 和 Windows Server 2008 R2 之間不會變更精細密碼原則 cmdlet 功能。 為了方便下, 圖顯示 cmdlet 的相關引數：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Active Directory 管理中心也可讓您尋找結果套用 FGPP 特定使用者的設定。 以滑鼠右鍵按一下任何使用者，按一下 [ **[檢視設定密碼結果...**打開*設定密碼*頁面，並透過隱含或明確指派使用者適用於：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
檢查**屬性**任何使用者或群組顯示的**直接相關密碼設定**，這是明確指派的 FGPPs:  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
隱含 FGPP 指派不會顯示在此處。對於，您必須使用**[檢視設定密碼結果...**選項。  
  
## <a name="BKMK_HistoryViewer"></a>使用 Active Directory 系統管理員中心 Windows PowerShell 歷史檢視器  
在未來的 Windows 管理是 Windows PowerShell。 層次圖形自動化工作架構上方的工具，來管理最複雜分散式系統變得一致且有效率。 您需要了解 Windows PowerShell 以瑞曲之戰完整您可能會與您電腦的投資放到最大的運作方式。  
  
Active Directory 管理中心現在提供完整的它執行所有 Windows PowerShell cmdlet 和引數和值歷史。 您可以複製 cmdlet 歷史其他地方的研究或修改或重複使用。 您可以建立工作資訊來協助找出所您 Active Directory 管理中心造成 Windows PowerShell 中的命令。 您也可以篩選歷史尋找感興趣的點。  
  
Active Directory 系統管理員中心 Windows PowerShell 歷史檢視器的目的是讓您了解透過實用的體驗。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
按一下 > 形箭號 （箭號） 以顯示 Windows PowerShell 歷史檢視器。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
然後，建立的使用者或修改群組成員資格。 摺疊檢視的每個 cmdlet Active Directory 管理中心執行引數指定的持續更新歷史檢視器。  
  
展開可查看所有值提供 cmdlet 的引數感任何的興趣明細項目：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
按一下**開始工作]**功能表，以建立手動標記，才能使用 Active Directory 管理中心來建立、 修改或 delete 物件。 輸入您所執行的動作。  完成您的變更，選取 [**結束工作]**。 插入摺疊便箋更好了解您可以使用所有的這些動作執行工作注意群組。  
  
例如，若要查看 Windows PowerShell 命令可用來變更使用者的密碼，並移除他的群組：  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
選取 [顯示所有核取方塊也會顯示取得-* 動詞 Windows PowerShell cmdlet 只擷取的資料。  
  
![進階的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
歷史檢視器顯示文字的命令執行 Active Directory 管理中心和您可能會請注意，某些 cmdlet 出現必要執行。 例如，您可以建立新的使用者：  
  
```  
new-aduser   
```  
  
並不需要使用：  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
Active Directory 管理中心的設計需要使用量降到最低的程式碼和模組化程度。 因此，而不是一組功能，建立新的使用者與另一組修改現有的使用者，它至少會每項功能，然後將它們 cmdlet 一起。 當您將會學習 Active Directory Windows PowerShell 時，請牢記這點。 您也可以使用的學習技巧，您看到如何只是您可以使用 Windows PowerShell 來完成單一工作。  
  
## <a name="BKMK_Tshoot"></a>疑難排解 AD DS 管理  
  
### <a name="introduction-to-troubleshooting"></a>疑難排解簡介  
因為其相對我們，在現有客戶環境中使用的 Active Directory 管理中心有限疑難排解選項。  
  
### <a name="troubleshooting-options"></a>疑難排解選項  
  
#### <a name="logging-options"></a>登入選項  
Active Directory 管理中心現在包含建登入 Windows Server 2012，追蹤設定檔的一部分。 建立日修改下列檔案 dsac.exe 相同的資料夾中︰  
  
**dsac.exe.config**  
  
建立下列動作：  
  
```  
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>   
 <trace autoflush="false" indentsize="4">   
  <listeners>   
   <add name="myListener"   
    type="System.Diagnostics.TextWriterTraceListener"   
    initializeData="dsac.trace.log" />   
   <remove name="Default" />   
  </listeners>   
 </trace>   
</system.diagnostics>  
  
```  
  
詳細資訊層級的**DsacLogLevel**的**無**、**錯誤**、**警告**，**資訊**，和**詳細資訊**。 輸出檔案名稱設定，並將寫入 dsac.exe 相同的資料夾。 輸出可以告訴您更多約 ADAC 運作方式的網域控制站它連絡時，所執行的 Windows PowerShell 命令，項目回應，並進一步詳細資料。  
  
例如，而使用資訊的層級，它會傳回追蹤層級詳細資訊以外的所有結果：  
  
-   DSAC.exe 開始  
  
-   登入開始  
  
-   網域控制站要求退貨初始的網域資訊  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   網域控制站 DC1 傳回網域 Corp  
  
-   廣告 virtual 磁碟機 PS 載入  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   取得網域根 DSE 資訊  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   取得 AD 網域資源回收筒] 資訊  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   取得 AD 森林  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   取得支援的加密類型，FGPP，某些使用者的資訊架構資訊  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
    -Properties:lDAPDisplayName  
    -ResultPageSize:"100"  
    -ResultSetSize:$null  
    -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
    -SearchScope:"OneLevel"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
    [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   取得有關網域物件，以系統管理員，並按下網域標頭顯示的所有資訊。  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -IncludeDeletedObjects:$false  
    -LDAPFilter:"(objectClass=*)"  
    -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
    -ResultPageSize:"100"  
    -ResultSetSize:"20201"  
    -SearchBase:"DC=corp,DC=contoso,DC=com"  
    -SearchScope:"Base"  
    -Server:"dc1.corp.contoso.com"  
  
    ```  
  
設定的詳細資訊層級也會顯示.NET 堆疊的每項功能，但這些不包括不足，無法資料以外時疑難排解 Dsac.exe 遭受存取違規或當機非常有用。  
  
> [!IMPORTANT]  
> 還有稱為服務超出的版本[Active Directory 管理閘道](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852)，在 Windows Server 2008 SP2 與 Windows Server 2003 SP2 上執行。  
  
有兩個這個問題的可能原因有：  
  
-   不執行任何無障礙網域控制站 ADWS 服務。  
  
-   網路通訊封鎖 ADWS 服務執行 Active Directory 管理中心的電腦  
  
不 Active Directory 網頁的服務執行個體可供使用時所顯示的錯誤︰  
  
|||  
|-|-|  
|錯誤|操作|  
|「 無法連接到任何網域。 重新整理或再試一次連接可 」|顯示在 [開始] 畫面的 Active Directory 管理中心應用程式|  
|[可用的伺服器中找不到* <NetBIOS domain name> *執行 Active Directory Web 服務 (ADWS) 網域 」|顯示 Active Directory 管理中心應用程式中選取網域節點嘗試時|  
  
這個問題的疑難排解，使用下列步驟：  
  
1.  驗證 Active Directory Web 服務服務會開始至少網域中的網域控制站 （以及最好是所有網域控制站森林中的）。 請確定它已設定自動開始上所有網域控制站同時。  
  
2.  執行 Active Directory 管理中心的電腦，驗證，您可以找出執行這些 NLTest.exe 命令執行 ADWS 伺服器：  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    這些測試失敗即使 ADWS 服務順利執行時，問題的名稱解析 LDAP 並不 ADWS 或 Active Directory 管理中心。 這項測試失敗，錯誤 」 1355年 0x54B ERROR_NO_SUCH_DOMAIN 「 達到任何結論前請再次檢查 ADWS 不執行任何網域控制站不過，如果。  
  
3.  網域控制站傳回 NLTest，傾印連接埠聆聽命令清單：  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    檢查 ports.txt 檔案，並驗證 ADWS 服務正在聽取指令 9389 連接埠。 範例：  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    是否正在聽取指令，驗證 Windows 免，並確認它們允許 9389 TCP 輸入。 根據預設，網域控制站讓防火牆規則 「 Active Directory Web 服務 （TCP 單元） 」。 如果您無法聆聽，再試一次驗證服務執行此伺服器上並重新開機。 驗證其他處理序已經聆聽 9389 連接埠。  
  
4.  執行 Active Directory 管理中心的電腦上並傳回 NLTEST 網域控制站，安裝 NetMon 或其他網路擷取公用程式。 收集同時網路擷取的兩部電腦，您開始 Active Directory 管理中心並停止擷取之前，請先查看錯誤。 驗證 client 是無法用來傳送和接收的網域控制站 TCP 9389 連接埠。 如果封包傳送，但不會時接到提醒，或時接到提醒，網域控制站回覆，但它們不會瑞曲之戰 client 很有可能是防火牆空行連接埠卸除封包網路上的電腦。 這個防火牆可能的軟體或硬體，而且可能第三方 endpoint protection （防毒） 軟體的一部分。  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>已知日可能的問題與支援案例  
Active Directory 管理中心問題可能只是無法連接到 Active Directory Web 服務 (ADWS) 執行 Windows Server 2012 或 Windows Server 2008 R2 網域控制站。  
  
## <a name="see-also"></a>也了  
[Active Directory 系統管理員中心調節和 #40; 簡介層級 100 和 #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

