---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: "Windows Server 2016 功能層級"
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>樹系和網域正常運作的層級

>適用於：Windows Server

使用 Windows 2003 生活結束後，更新到 Windows Server 2008、2012 年或 2016 年需要 Windows 2003 網域控制站（網域控制站）。 如此一來，應該會執行 Windows Server 2003 的任何網域控制站從網域。 網域和樹系提高功能等級應該要至少為防止新增的環境中執行較舊版本的 Windows Server 的網域控制站的 Windows Server 2008。

我們建議針對的更新他們的網域功能等級 (DFL) 和功能等級 (FFL) 樹系的一部分，因為 FFL 與 2003 DFL 取代在 Windows Server 2016 和他們將不再支援在未來的版本。

針對需要額外的時間評估他們 DFL 與 FFL 從 2003 年移轉的 2003 DFL 及 FFL 將會繼續支援 Windows 10 與 Windows Server 2016 提供網域森林中的所有網域控制站都的 Windows Server 2008、2008R2，在 2012，2012R2，或是 2016 年。

Windows Server 2008 及較高的網域功能層級，散發檔案服務 (DFS) 複寫用來網域控制站之間複製 SYSVOL 資料夾內容。 如果您建立新的網域網域層級 Windows Server 2008 功能或更高版本，DFS 複寫自動用於複寫 SYSVOL。 如果您建立網域層級會正常運作，您必須從使用 SYSVOL DFS 複寫 FRS 移轉。 移轉的步驟，您可以依照任一個[參考 TechNet 上的程序](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)或您可以參考[簡化步驟儲存小組檔案櫃部落格上的](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

Windows Server 2003 網域及森林功能等級繼續支援，但組織應該提高以 Windows Server 2008（或更高版本，如果可能的話）功能等級確保 SYSVOL 複寫相容性，並在未來的支援。 除此之外，有許多其他優點和功能提供較高的功能等級更高版本。 查看下列的詳細資訊的資源：

## <a name="windows-server-2016"></a>Windows Server 2016

支援的網域控制站作業系統中：

* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 的樹系層級功能

* 在 Windows Server 2012R2 森林功能層級，有可用的功能與項功能，都可使用：
    * [權限存取管理 (PAM) 使用 Microsoft 的身分管理員 (MIM)](https://docs.microsoft.com/windows-server/identity/what's-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 網域層級功能

* 預設 Active Directory 的所有功能，從 Windows Server 2012R2 網域功能層級，所有功能，以及下列功能：
    * 網域控制站可支援循環公用按鍵只使用者的 NTLM 的機密資訊。 
    * 網域控制站使用者限於特定加入網域的裝置時，可支援允許網路 NTLM。 
    * 成功驗證 PKInit 有效期限副檔名 Kerberos 戶端將會收到新公開金鑰身分 SID。

    如需詳細資訊請查看[F:kerberos 驗證中的新功能](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[認證保護中的新功能](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

支援的網域控制站作業系統中：

* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 森林層級功能

* 所有功能，可在 Windows Server 2012 的樹系層級，但不是額外功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 網域層級功能

* 預設 Active Directory 的所有功能，從 Windows Server 2012 網域功能層級，所有功能，以及下列功能：
    * 保護使用者俠端保護。 保護使用者網域不能再 Windows Server 2012 R2 驗證：
        * 驗證 NTLM 驗證
        * 使用 F:kerberos 預先驗證 DES 或 RC4 密碼套件
        * 使用未限制或限制委派委派
        * 續約初始 4 小時期間以外的使用者門票 (Tgt)
    * 驗證原則
        * 新的樹系的 Active Directory 原則可套用到 Windows Server 2012 R2 網域控制的主機中帳號，account 可以登入從且適用於驗證存取控制項條件為 account 執行的服務。
    * 驗證原則筒倉
        * 為基礎新的樹系的 Active Directory 物件，可以建立的使用者，受管理的服務和電腦上，用來可帳號驗證原則或驗證隔離帳號之間的關係。

## <a name="windows-server-2012"></a>Windows Server 2012

支援的網域控制站作業系統中：

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 森林層級功能

* 所有功能，可在 Windows Server 2008 R2 的樹系層級，但不是額外功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 網域層級功能

* 預設 Active Directory 的所有功能，從 Windows Server 2008R2 網域功能層級，所有功能，以及下列功能：
    * \ [KDC 支援宣告、複合驗證以及 Kerberos 保護 \ [KDC 系統管理範本原則有兩種設定（永遠提供宣告和失敗護身的驗證要求）需要 Windows Server 2012 網域功能層級。 如需詳細資訊，請查看[在 F:kerberos 驗證中的新功能](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

支援的網域控制站作業系統中：

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 森林層級功能

* 所有功能，可在 Windows Server 2003 的樹系正常運作的層級，再加上下列功能：
    * Active Directory 資源回收桶，會提供 AD DS 執行時還原刪除的物件完整的能力。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 網域層級功能

* 預設 Active Directory 的所有功能，從 Windows Server 2008 網域功能層級，所有功能，以及下列功能：
    * 驗證機制保證，封裝類型的登入方法（智慧卡或使用者名稱/密碼）來驗證網域使用者每個使用者的 Kerberos 權杖中相關資訊。 這項功能在已部署聯盟的身分管理基礎結構，例如 Active Directory 同盟 Services (AD FS) 網路環境時可以再任何時候使用者嘗試存取已開發判斷根據使用者登入方法授權的任何宣告感知應用程式中擷取權杖中的資訊。
    * 自動 SPN 管理時 DNS 名稱或主機的 account 變更電腦的名稱下方的服務管理 Account 特定電腦上執行的服務。 如需受管理的服務帳號，請查看[服務帳號 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

支援的網域控制站作業系統中：

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 森林層級功能

* 所有功能，都可在 Windows Server 2003 森林功能層級，但不是額外的功能都都可使用。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 網域層級功能

* 所有的預設 AD DS 功能、所有的 Windows Server 2003 網域功能層級的功能和下列功能可供使用：
    * 分散式的檔案系統 (DFS) 複寫支援針對 Windows Server 2003 系統磁碟區 (SYSVOL)
        * DFS 複寫支援提供更加穩定與詳細複寫 SYSVOL 內容。
        [!NOTE]>
        >開始使用 Windows Server 2012 R2，會取代檔案複寫服務 (FRS)。 新的網域建立網域控制站最少執行的 Windows Server 2008 網域功能等級或更高版本必須設定 Windows Server 2012 R2。

    * 網域型 DFS 命名空間執行 Windows Server 2008 模式，包括存取型為基礎的值與增加擴充性的支援。 Windows Server 2008 模式中的網域型命名空間也需要樹系使用的 Windows Server 2003 森林功能層級。 如需詳細資訊，請查看[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。
    * 進階加密標準（好一段 128 和好一段 256）支援 Kerberos 通訊協定。 為了讓 Tgt 好一段發行，網域功能等級必須 Windows Server 2008，或更高版本，並網域密碼需要變更。 
        * 如需詳細資訊，請查看[Kerberos 調節](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。
        [!NOTE]>
        >驗證可能會發生錯誤網域控制站之後提高網域功能等級是以 Windows Server 2008，或更高的網域控制站如果已經有複寫 DFL 變更，但未尚未重新整理 krbtgt 密碼。 在這種情形下，將觸發新 krbtgt 密碼記憶體中重新整理網域控制站的 \ [KDC 服務重新開機，並解析相關的驗證錯誤。

    * [最後一次互動式登入](https://go.microsoft.com/fwlink/?LinkId=180387)的資訊會顯示下列資訊：
        * 總加入網域的 Windows Server 2008 server 或 Windows Vista 工作站嘗試登入失敗的次數
        * Windows Server 2008 server 或 Windows Vista 工作站成功登入後的嘗試登入失敗總數目
        * 中的上一次登入失敗的嘗試在 Windows Server 2008 或 Windows Vista 工作站時間
        * Windows Server 2008 server 或 Windows Vista 工作站嘗試的最後成功登入的時間
    * 細緻密碼原則，讓您的網域中指定的使用者和安全性的全域群組的密碼，以及 account 鎖定原則。 如需詳細資訊，請查看[適用於 Fine-Grained 密碼和 Account 鎖定原則設定中的指示](https://go.microsoft.com/fwlink/?LinkID=91477)。
    * 個人 Virtual 桌面
        * 若要使用新增個人 Virtual 桌面版中的索引標籤使用者 Account 屬性] 對話方塊中 Active Directory 使用者電腦所提供的功能，您必須 AD DS 結構描述擴充 Windows Server 2008 R2 的 (架構物件版本 = 47)。 如需詳細資訊，請查看[部署個人 Virtual 桌面使用 RemoteApp 並桌面連接 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

支援的網域控制站作業系統中：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 森林層級功能

* 所有 AD DS 預設的功能，以及項功能，可：
    * 信任的樹系
    * 重新命名網域
    * 連結值複寫
        - 連結值複寫可讓您變更來儲存和值複寫個人的成員，而不是複寫單位整個成員資格群組成員資格。 儲存複寫個人成員值使用較少的頻寬，較少的處理器循環期間複寫，並會防止您遺失的更新，當您新增或移除多個不同的網域控制站同時的成員。
    * 部署唯讀網域控制站 (RODC) 的能力
    * 已改善知識一致性檢查程式 (KCC) 演算法和擴充性
        - 間拓撲發電機 (ISTG) 使用改進縮放超過 AD DS 可支援層級 Windows 2000 的樹系功能支援更多的網站的樹系的演算法。 改善的 ISTG 選舉演算法是小於干擾機制來選擇 ISTG 層級 Windows 2000 的樹系正常運作。
    * 建立名為動態輔助執行個體的能力**dynamicObject**在網域 directory 磁碟分割
    * 要轉換的能力**需要**到物件執行個體**使用者**物件執行個體，並完成以相反的方向轉換
    * 建立的新群組類型角色為基礎的授權，才能執行個體的能力。 
        - 這些類型稱為「基本的應用程式群組和 LDAP 查詢群組。
    * 停用的屬性和類別架構中的重新定義。 重複使用下列屬性：ldapDisplayName，schemaIdGuid，OID，以及 mapiID。
    * 網域型 DFS 命名空間執行 Windows Server 2008 模式，包括存取型為基礎的值與增加擴充性的支援。 如需詳細資訊，請查看[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 網域層級功能

* 是可用的所有 AD DS 預設功能、所有網域層級 Windows 2000 原生正常運作，有可用的功能和下列功能：
    * 網域管理工具，Netdom.exe，可讓您的網域控制站重新命名
    * 登入頻率更新
        * **LastLogonTimestamp**屬性登入上次的使用者或電腦的更新。 此屬性複製網域中。
    * 若要設定的功能**userPassword**上屬性為有效的密碼**需要**和使用者物件
    * 重新導向使用者及電腦的能力容器
        * 有兩個已知的容器提供適用於電腦和使用者帳號，與容納根據預設，也就是 data-cn = 電腦，<domain root>和 data-cn = 的使用者，<domain root>。 這項功能可讓您的新的已知位置這些帳號定義。
    * 功能的授權管理員將其授權原則中 AD DS
    * 限制的委派
        * 限制的委派可讓應用程式可以利用 Kerberos 驗證透過安全的使用者的認證委派。
        * 您可以限制委派特定目的服務。
    * 選擇式驗證
        * 也可讓您指定的使用者和群組來自信任的樹系獲准信任的樹系的資源伺服器的驗證選擇性驗證讓。

## <a name="windows-2000"></a>Windows 2000

支援的網域控制站作業系統中：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 原生森林層級功能

* 預設 AD DS 功能都可使用。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 原生網域層級功能

* 預設 AD DS 的功能與下列 directory 功能都可包括：
    * 萬用 distribution 和安全性群組。
    * 巢群組
    * 群組轉換，可讓安全性與 distribution 群組之間轉換
    * 安全性識別碼 (SID) 歷史

## <a name="next-steps"></a>後續步驟

* [提升網域功能](https://technet.microsoft.com/library/cc753104.aspx)  
* [提升森林功能](https://technet.microsoft.com/library/cc730985.aspx)
