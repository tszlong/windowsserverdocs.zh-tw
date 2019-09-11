---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Windows Server 2016 功能等級
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: c1e2108084b03fabbf7c6a18c2ecbcaf3cbd1dd9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868260"
---
# <a name="forest-and-domain-functional-levels"></a>樹系和網域功能等級

>適用於：Windows Server

功能等級會決定可用的 Active Directory Domain Services （AD DS）網域或樹系功能。 它們也會決定您可以在網域或樹系中的網域控制站上執行的 Windows Server 作業系統。 不過，功能等級並不會影響您可以在已加入網域或樹系的工作站和成員伺服器上執行的作業系統。

當您部署 AD DS 時，請將網域和樹系功能等級設定為您的環境可以支援的最高值。 如此一來，您可以盡可能使用多個 AD DS 功能。 當您部署新的樹系時，系統會提示您設定樹系功能等級，然後設定網域功能等級。 您可以將網域功能等級設定為高於樹系功能等級的值，但無法將網域功能等級設定為低於樹系功能等級的值。

在 Windows 2003 的生命週期結束後，Windows 2003 網域控制站（Dc）必須更新為 Windows Server 2008、2008R2、2012、2012R2、2016或2019。 因此，執行 Windows Server 2003 的任何網域控制站都應該從網域中移除。

在 Windows Server 2008 及更高版本的網域功能等級上，分散式檔案服務（DFS）複寫是用來複寫網域控制站之間的 SYSVOL 資料夾內容。 如果您在 Windows Server 2008 網域功能等級或更新版本建立新的網域，DFS 複寫會自動用來複寫 SYSVOL。 如果您已在較低的功能層級建立網域，則需要從使用 FRS 遷移至 SYSVOL 的 DFS 複寫。 針對遷移步驟，您可以遵循[TechNet 上的程式](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)，或參考[存放裝置小組檔案封包 blog 上的一組簡化的步驟](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

## <a name="windows-server-2019"></a>Windows Server Standard 2012 R2

此版本中未新增樹系或網域功能等級。

新增 Windows Server 2019 網域控制站的最低需求是 Windows Server 2008 功能等級。 網域也必須使用「DFS-R」作為引擎來複寫 SYSVOL。

## <a name="windows-server-2016"></a>Windows Server 2016

支援的網域控制站作業系統：

* Windows Server Standard 2012 R2
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 樹系功能等級功能

* 可在 Windows Server 2012R2 樹系功能等級使用的所有功能，以及下列功能：
   * [使用 Microsoft Identity Manager （MIM）的特殊許可權存取管理（PAM）](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 網域功能等級功能

* 所有預設的 Active Directory 功能、Windows Server 2012R2 網域功能等級的所有功能，以及下列功能：
   * Dc 可以在設定為需要 PKI 驗證的使用者帳戶上，支援自動回復 NTLM 和其他密碼型秘密。 此設定也稱為「互動式登入需要智慧卡」
   * 當使用者限制為已加入網域的特定裝置時，Dc 可以支援允許網路 NTLM。
   * Kerberos 用戶端成功使用 PKInit Freshness Extension 進行驗證時，將會取得全新的公開金鑰識別 SID。

    如需詳細資訊，請參閱[Kerberos 驗證的新功能](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[認證保護的新功能](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

支援的網域控制站作業系統：

* Windows Server Standard 2012 R2
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 樹系功能等級功能

* Windows Server 2012 樹系功能等級提供的所有功能，但沒有其他功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 網域功能等級功能

* 所有預設的 Active Directory 功能、Windows Server 2012 網域功能等級的所有功能，以及下列功能：
   * 保護使用者的 DC 端保護。 向 Windows Server 2012 R2 網域驗證的受保護使用者已無法再執行：
      * 使用 NTLM 驗證進行驗證
      * 在 Kerberos 預先驗證中使用 DES 或 RC4 加密套件
      * 使用非限制或限制委派進行委派
      * 更新超過初始4小時存留期的使用者票證（Tgt）
   * 驗證原則
      * 以樹系為基礎的新 Active Directory 原則，可套用至 Windows Server 2012 R2 網域中的帳戶，以控制帳戶可以登入的主機，並將驗證的存取控制條件套用至以帳戶身分執行的服務。
   * 驗證原則定址接收器
      * 以樹系為基礎的新 Active Directory 物件，可建立使用者、受控服務與電腦之間的關聯性、用來分類帳戶以進行驗證原則或驗證隔離的帳戶。

## <a name="windows-server-2012"></a>Windows Server 2012

支援的網域控制站作業系統：

* Windows Server Standard 2012 R2
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 樹系功能等級功能

* Windows Server 2008 R2 樹系功能等級提供的所有功能，但沒有其他功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 網域功能等級功能

* 所有預設的 Active Directory 功能、Windows Server 2008R2 網域功能等級的所有功能，以及下列功能：
   * 宣告、複合驗證和 Kerberos 防護的 KDC 支援 KDC 系統管理範本原則有兩個設定（永遠提供宣告和失敗未受防護驗證要求），這些都需要 Windows Server 2012 網域功能等級。 如需詳細資訊，請參閱[Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

支援的網域控制站作業系統：

* Windows Server Standard 2012 R2
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 樹系功能等級功能

* 可以在 Windows Server 2003 樹系功能等級上使用的所有功能，以及下列功能：
   * Active Directory 資源回收筒，在 AD DS 執行時可完整還原其中已刪除的物件。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 網域功能等級功能

* 所有預設的 Active Directory 功能、Windows Server 2008 網域功能等級的所有功能，以及下列功能：
   * 驗證機制保證，這會封裝登入方法（智慧卡或使用者名稱/密碼）類型的相關資訊，用來驗證每個使用者的 Kerberos 權杖內的網域使用者。 在已部署同盟身分識別管理基礎結構的網路環境（例如 Active Directory 同盟服務（AD FS）中啟用這項功能時，每當使用者嘗試存取任何時，權杖中的資訊就可以解壓縮已開發的宣告感知應用程式，可根據使用者的登入方法來判斷授權。
   * 當電腦帳戶的名稱或 DNS 主機名稱變更時，在受管理的服務帳戶的內容下，于特定電腦上執行之服務的自動 SPN 管理。 如需受管理的服務帳戶的詳細資訊，請參閱[服務帳戶的逐步指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

支援的網域控制站作業系統：

* Windows Server Standard 2012 R2
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 樹系功能等級功能

* Windows Server 2003 樹系功能等級提供的所有功能，但沒有其他功能可供使用。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 網域功能等級功能

* 所有預設的 AD DS 功能、Windows Server 2003 網域功能等級的所有功能，以及下列功能可供使用：
  * Windows Server 2003 系統磁碟區（SYSVOL）的分散式檔案系統（DFS）複寫支援
    * DFS 複寫支援提供更強大且更詳細的 SYSVOL 內容複寫。

      > [!NOTE]
      > 從 Windows Server 2012 R2 開始，檔案複寫服務（FRS）已被取代。 在至少執行 Windows Server 2012 R2 的網域控制站上建立的新網域，必須設定為 Windows Server 2008 網域功能等級或更高版本。

  * 以 Windows Server 2008 模式執行的網域型 DFS 命名空間，其中包含對存取型列舉和增加擴充性的支援。 Windows Server 2008 模式中的網域型命名空間也需要樹系使用 Windows Server 2003 樹系功能等級。 如需詳細資訊，請參閱[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。
  * 進階加密標準（AES 128 和 AES 256） Kerberos 通訊協定的支援。 若要使用 AES 來發行 Tgt，網域功能等級必須是 Windows Server 2008 或更高版本，而且需要變更網域密碼。 
    * 如需詳細資訊，請參閱[Kerberos 增強功能](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。

      > [!NOTE]
      >如果網域控制站已複寫 DFL 變更，但尚未重新整理 krbtgt 密碼，網域控制站上的驗證錯誤可能會在網域功能等級提升至 Windows Server 2008 或更高版本之後發生。 在此情況下，網域控制站上的 KDC 服務重新開機將會觸發新 krbtgt 密碼的記憶體中更新，並解決相關的驗證錯誤。

  * [上次互動式登入](https://go.microsoft.com/fwlink/?LinkId=180387)資訊會顯示下列資訊：
     * 已加入網域的 Windows Server 2008 伺服器或 Windows Vista 工作站的失敗登入嘗試總數
     * 成功登入 Windows Server 2008 伺服器或 Windows Vista 工作站後，登入嘗試失敗的總次數
     * Windows Server 2008 或 Windows Vista 工作站上上次嘗試登入失敗的時間
     * Windows Server 2008 伺服器或 Windows Vista 工作站上最後一次成功登入嘗試的時間
  * 更細緻的密碼原則可讓您指定網域中使用者和全域安全性群組的密碼和帳戶鎖定原則。 如需詳細資訊，請參閱更細緻[的密碼和帳戶鎖定原則設定的逐步指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
  * 個人虛擬桌面
     * 若要在 Active Directory 使用者和電腦 的 使用者帳戶內容 對話方塊中，使用 個人虛擬桌面 索引標籤所提供的新增功能，您的 AD DS 架構必須針對 Windows Server 2008 R2 （架構物件版本 = 47）延伸。 如需詳細資訊，請參閱[使用 RemoteApp 和桌面連線逐步指南部署個人虛擬桌面](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

支援的網域控制站作業系統：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 樹系功能等級功能

* 所有預設的 AD DS 功能和下列功能都可供使用：
   * 樹系信任
   * 網域重新命名
   * 連結值複寫
      - 連結值複寫可讓您變更群組成員資格，以儲存和複寫個別成員的值，而不是將整個成員資格複寫為單一單位。 儲存和複寫個別成員的值，會在複寫期間使用較少的網路頻寬和較少的處理器週期，並防止您在不同網域控制站同時新增或移除多個成員時遺失更新。
   * 部署唯讀網域控制站（RODC）的功能
   * 改良的知識一致性檢查程式（KCC）演算法和擴充性
      - 網站間拓撲產生器（ISTG）使用改進的演算法，其可調整為支援的樹係數目比 Windows 2000 樹系功能等級的 AD DS 更多。 改良的 ISTG 選舉演算法是在 Windows 2000 樹系功能等級選擇 ISTG 的低干擾性機制。
   * 在網域目錄分割中建立名為**dynamicObject**之動態輔助類別實例的能力
   * 將**inetOrgPerson**物件實例轉換成**使用者**物件實例的能力，並以相反的方向完成轉換
   * 能夠建立新群組類型的實例，以支援以角色為基礎的授權。 
      - 這些類型稱為應用程式基本群組和 LDAP 查詢群組。
   * 停用和重新定義架構中的屬性及類別。 可以重複使用下列屬性： ldapDisplayName、schemaIdGuid、OID 和 mapiID。
   * 以 Windows Server 2008 模式執行的網域型 DFS 命名空間，其中包含對存取型列舉和增加擴充性的支援。 如需詳細資訊，請參閱[選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 網域功能等級功能

* 所有預設的 AD DS 功能、Windows 2000 原生網域功能等級提供的所有功能，以及下列功能皆可使用：
   * 網域管理工具，Netdom .exe，讓您可以重新命名網域控制站
   * 登入時間戳記更新
      * 利用使用者或電腦最後登入時間來更新 **lastLogonTimestamp** 屬性。 而且會將此屬性複寫至整個網域。
   * 將**userPassword**屬性設定為**inetOrgPerson**和使用者物件的有效密碼的能力
   * 重新導向使用者和電腦容器的能力
      * 根據預設，會提供兩個已知的容器來存放電腦和使用者帳戶，也就是 cn =<domain root> computer 和 cn =<domain root>Users。 這項功能可讓您定義這些帳戶的新知名位置。
   * 授權管理員將其授權原則儲存在 AD DS 中的能力
   * 限制委派
      * 限制委派可讓應用程式透過 Kerberos 型驗證，利用使用者認證的安全委派。
      * 您只能將委派限制在特定目的地服務。
   * 選擇性驗證
      * 選擇性驗證可讓您將受信任樹系中的使用者和群組指定為允許向信任樹系中的資源伺服器進行驗證。

## <a name="windows-2000"></a>Windows 2000

支援的網域控制站作業系統：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 原生樹系功能等級功能

* 所有預設的 AD DS 功能都可供使用。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 原生網域功能等級功能

* 所有預設的 AD DS 功能和下列目錄功能都可供使用，包括：
   * 適用于通訊群組和安全性群組的萬用群組。
   * 群組嵌套
   * 群組轉換，允許在安全性與通訊群組之間進行轉換
   * 安全識別碼（SID）歷程記錄

## <a name="next-steps"></a>後續步驟

* [提高網域功能等級](https://technet.microsoft.com/library/cc753104.aspx)  
* [提高樹系功能等級](https://technet.microsoft.com/library/cc730985.aspx)
