---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Windows Server 2016 功能等級
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: 5f7a8f08ff10102fbc04b6f8272320bd3b77785d
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80825491"
---
# <a name="forest-and-domain-functional-levels"></a>樹系和網域功能等級

>適用於：Windows Server

功能等級決定了 Active Directory Domain Services (AD DS) 網域或樹系中能使用的功能。 功能等級也決定您能夠在網域或樹系中的網域控制站上執行哪些 Windows Server 作業系統。 不過，功能等級不會影響您能夠在已加入網域或樹系的工作站或成員伺服器上執行哪些作業系統。

當您建立新網域或新樹系部署 AD DS 時，請將網域和樹系的功能等級設為您環境能支援的最高值。 這樣您就可以儘可能使用到最多 AD DS 功能。 當您部署新樹系時，會提示您設定樹系功能等級，然後再設定網域功能等級。 您可以將網域功能等級設定為高於樹系功能等級的值，但無法將網域功能等級設定為低於樹系功能等級的值。

在 Windows 2003 的生命週期結束後，Windows 2003 網域控制站 (DC) 必須更新為 Windows Server 2008、2008R2、2012、2012R2、2016 或 2019。 因此，執行 Windows Server 2003 的任何網域控制站都應該從網域中移除。

在 Windows Server 2008 及更高版本的網域功能等級，分散式檔案服務 (DFS) 複寫是用來複寫網域控制站之間的 SYSVOL 資料夾內容。 如果您在 Windows Server 2008 網域功能等級或更高版本建立新的網域，DFS 複寫會自動複寫 SYSVOL。 如果您已在較低的功能等級建立網域，則需要從使用 FRS 移轉至 SYSVOL 的 DFS 複寫。 至於移轉的步驟，您可以遵循 [TechNet 上的程序](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)，也可以參閱[Storage Team File Cabinet 上的簡化步驟](https://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

## <a name="windows-server-2019"></a>Windows Server 2019

此版本中並未新增樹系或網域功能等級。

新增 Windows Server 2019 網域控制站的最低需求是 Windows Server 2008 功能等級。 網域也必須使用「DFS-R」作為引擎來複寫 SYSVOL。

## <a name="windows-server-2016"></a>Windows Server 2016

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 樹系功能等級的功能

* 可以在 Windows Server 2012R2 樹系功能等級使用的所有功能，以及下列功能：
   * [使用 Microsoft Identity Manager (MIM) 的特殊權限存取管理 (PAM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 網域功能等級的功能

* 所有預設的 Active Directory 功能、來自 Windows Server 2012R2 網域功能等級的所有功能，以及下列功能：
   * 在設定為需要 PKI 驗證的使用者帳戶上，DC 可以支援自動回復 NTLM 和其他密碼型秘密。 此設定也稱為「互動式登入需要智慧卡」
   * 當限制使用者為已加入特定網域的裝置時，DC 可以支援允許網路 NTLM。
   * Kerberos 用戶端成功使用 PKInit Freshness Extension 進行驗證時，將會取得全新的公開金鑰身分識別 SID。

    如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)以及[認證保護的新功能](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)。

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 樹系功能等級的功能

* 可以在 Windows Server 2012 樹系功能等級使用的所有功能，沒有其他功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 網域功能等級的功能

* 所有預設的 Active Directory 功能、來自 Windows Server 2012 網域功能等級的所有功能，以及下列功能：
   * Protected Users 的 DC 端保護。 向 Windows Server 2012 R2 網域驗證的 Protected Users 無法再執行：
      * 使用 NTLM 驗證進行驗證
      * 在 Kerberos 預先驗證中使用 DES 或 RC4 加密套件
      * 以非限制或限制委派方式受到委派
      * 在初始 4 小時存留期之後更新使用者票證 (TGT)
   * 驗證原則
      * 新的以樹系為基礎的 Active Directory 原則，可套用至 Windows Server 2012 R2 網域中的帳戶，以控制帳戶可以從哪些主機登入，並將驗證的存取控制條件套用至以帳戶身分執行的服務。
   * 驗證原則定址接收器
      * 新的以樹系為基礎的 Active Directory 物件，可以建立使用者、受控服務與電腦之間的關係，可以建立用來分類帳戶以進行驗證原則或驗證隔離的帳戶。

## <a name="windows-server-2012"></a>Windows Server 2012

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 樹系功能等級的功能

* 可以在 Windows Server 2008 R2 樹系功能等級使用的所有功能，沒有其他功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 網域功能等級的功能

* 所有預設的 Active Directory 功能、來自 Windows Server 2008R2 網域功能等級的所有功能，以及下列功能：
   * 「宣告、複合驗證與 Kerberos 防護的 KDC 支援」KDC 系統管理範本原則有兩個設定 ([永遠提供宣告] 與 [不通過受保護的驗證要求] )，而這兩個設定都需要 Windows Server 2012 網域功能等級。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)。

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 樹系功能等級的功能

* 可以在 Windows Server 2003 樹系功能等級上使用的所有功能，以及下列功能：
   * Active Directory 資源回收筒，在 AD DS 執行時可完整還原其中已刪除的物件。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 網域功能等級的功能

* 所有預設的 Active Directory 功能、來自 Windows Server 2008 網域功能等級的所有功能，以及下列功能：
   * 驗證機制保證，將登入方法 (智慧卡或使用者名稱/密碼) 的類型資訊封裝，用來驗證每一個在 Kerberos 權杖中的網域使用者。 若在已經部署同盟識別身分管理基礎結構 (例如 Active Directory Federation Services，AD FS) 的網路環境中啟用此功能，每當使用者嘗試存取已開發的任何宣告感知 (Claims-Aware) 應用程式時，權杖中的資訊便可被擷取以根據使用者的登入方法來判定授權。
   * 自動 SPN 管理，當電腦帳戶的名稱或 DNS 主機名稱變更時，用於在受控服務帳戶的情境下特定電腦上執行的服務。 如需受控服務帳戶的詳細資訊，請參閱[服務帳戶的逐步指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 樹系功能等級的功能

* 可以在 Windows Server 2003 樹系功能等級使用的所有功能，沒有其他功能。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 網域功能等級的功能

* 所有預設的 AD DS 功能、來自 Windows Server 2003 網域功能等級的所有功能，以及下列功能：
  * Windows Server 2003 系統磁碟區 (SYSVOL) 的分散式檔案系統 (DFS) 複寫支援
    * DFS 複寫支援，可提供更健全與細緻的 SYSVOL 內容複寫。

      > [!NOTE]
      > 從 Windows Server 2012 R2 開始，檔案複寫服務 (FRS) 已被取代。 在至少執行 Windows Server 2012 R2 的網域控制站中建立的新網域，必須設為 Windows Server 2008 網域功能等級或更高版本。

  * 網域型 DFS 命名空間，在 Windows Server 2008 模式中執行，此模式支援存取型列舉和更高的延展性。 Windows Server 2008 模式中的網域型命名空間也需要樹系使用 Windows Server 2003 樹系功能等級。 如需詳細資訊，請參閱[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。
  * 支援 Kerberos 通訊協定的進階加密標準 (AES 128 與 AES 256)。 為了要使用 AES 來釋放 TGT，網域功能等級必須是 Windows Server 2008 或更高版本，而且需要變更網域密碼。 
    * 如需詳細資訊，請參閱[Kerberos 增強功能](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。

      > [!NOTE]
      >如果網域控制站已複寫 DFL 變更，但尚未重新整理 krbtgt 密碼，網域控制站上的驗證可能會在網域功能等級提升至 Windows Server 2008 或更高版本之後發生錯誤。 在此情況下，重新啟動網域控制站上的 KDC 服務會觸發新 krbtgt 密碼的記憶體中更新，並解決相關的驗證錯誤。

  * [上次互動登入](https://go.microsoft.com/fwlink/?LinkId=180387)資訊會顯示以下資訊：
     * 在已加入網域的 Windows Server 2008 伺服器或 Windows Vista 工作站上的登入嘗試失敗總數
     * 成功登入 Windows Server 2008 伺服器或 Windows Vista 工作站之後登入嘗試失敗的總數
     * 在 Windows Server 2008 或 Windows Vista 工作站上，最近一次登入嘗試失敗的時間
     * 在 Windows Server 2008 伺服器或 Windows Vista 工作站上，最近一次登入嘗試成功的時間
  * 更細緻的密碼原則，可讓您指定網域中使用者與通用安全性群組的密碼及帳戶鎖定原則。 如需詳細資訊，請參閱[更細緻的密碼與帳戶鎖定原則設定逐步指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
  * 個人虛擬桌面
     * 若要使用 Active Directory [使用者和電腦] 的 [使用者帳戶內容] 對話方塊中 [個人虛擬桌面] 索引標籤提供的附加功能，您的 AD DS架構必須為了適用於 Windows Server 2008 R2 而擴充 (架構物件版本 = 47)。 如需詳細資訊，請參閱[使用 RemoteApp 和桌面連線逐步指南來部署個人虛擬桌面](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

支援的網域控制站作業系統：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 樹系功能等級的功能

* 所有預設的 AD DS 功能以及下列功能：
   * 樹系信任
   * 網域重新命名
   * 連結值複寫
      - 連結數值複寫可讓您對群組成員資格進行變更，可儲存及複寫個別成員的值，而不是以整個成員資格為一個單位來進行複寫。 儲存和複寫個別成員的值，會在複寫期間使用較少的網路頻寬和較少的處理器週期，並防止您在不同網域控制站同時新增或移除多個成員時遺失更新。
   * 部署唯讀網域控制站 (RODC) 的能力
   * 改進的知識一致性檢查程式 (KCC) 演算法與延展性
      - 站台間拓撲產生器 (ISTG) 使用改進的演算法擴大對樹系的支援，它可支援的站台數量遠超過 Windows 2000 樹系功能等級中 AD DS 所能夠支援的數量。 改良的 ISTG 選擇演算法是在 Windows 2000 樹系功能等級中選擇 ISTG 時較無干擾的機制。
   * 在網域目錄磁碟分割中建立名為 **dynamicObject** 的動態輔助類別執行個體的能力
   * 將 **inetOrgPerson** 物件執行個體轉換為 **User** 物件執行個體的能力，以及反向轉換的能力
   * 建立新群組類型的執行個體以支援角色型授權的能力。 
      - 這些類型稱為應用程式基本群組和 LDAP 查詢群組。
   * 停用和重新定義架構中的屬性及類別。 下列屬性可以重複使用：ldapDisplayName、schemaIdGuid、OID、mapiID。
   * 網域型 DFS 命名空間，在 Windows Server 2008 模式中執行，此模式支援存取型列舉和更高的延展性。 如需詳細資訊，請參閱[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 網域功能等級的功能

* 所有預設的 AD DS 功能、Windows Server 2000 原生網域功能等級的所有功能，以及下列功能：
   * 網域管理工具 Netdom.exe，可讓您為網域控制站重新命名
   * 登入時間戳記更新
      * 利用使用者或電腦最後登入時間來更新 **lastLogonTimestamp** 屬性。 而且會將此屬性複寫至整個網域。
   * 將 **userPassword** 屬性設定為 **inetOrgPerson** 與使用者物件的有效密碼的能力
   * 重新導向使用者和電腦容器的能力
      * 根據預設值，系統會提供兩個已知容器，裝載電腦和使用者帳戶，也就是 cn=Computers,<domain root> 和 cn=Users,<domain root>。 您可以使用此功能為這些帳戶定義新的已知位置。
   * 授權管理員可將授權原則儲存在 AD DS 中的能力
   * 限制委派
      * 限制委派，讓應用程式透過 Kerberos 型驗證，善加利用使用者認證的安全委派優點。
      * 您可以限制只能委派至特定的目的地服務。
   * 選擇性驗證
      * 選擇性驗證讓您可以指定受信任樹系中的使用者和群組，允許他們對信任樹系中的資源伺服器進行驗證。

## <a name="windows-2000"></a>Windows 2000

支援的網域控制站作業系統：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 原生樹系功能等級的功能

* 所有預設的 AD DS 功能。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 原生網域功能等級的功能

* 所有預設的 AD DS 功能以及下列目錄功能：
   * 可供發佈群組和安全性使用的萬用群組。
   * 群組巢狀化
   * 群組轉換，可以進行安全性群組及發佈群組之間的轉換。
   * 安全性識別碼 (SID) 歷程記錄

## <a name="next-steps"></a>後續步驟

* [提高網域功能等級](https://technet.microsoft.com/library/cc753104.aspx)  
* [提高樹系功能等級](https://technet.microsoft.com/library/cc730985.aspx)
