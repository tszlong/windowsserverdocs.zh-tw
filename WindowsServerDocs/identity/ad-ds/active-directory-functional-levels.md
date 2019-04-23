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
ms.openlocfilehash: ea56c718394d145a36145d32e5769661a62efd56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840999"
---
# <a name="forest-and-domain-functional-levels"></a>樹系和網域功能等級

>適用於：Windows Server

功能等級可以決定可用的 Active Directory 網域服務 (AD DS) 網域或樹系功能。 它們也會決定您可以在網域或樹系的網域控制站執行的 Windows Server 作業系統。 不過，功能等級不會影響哪些作業系統，您可以在工作站執行和已加入網域或樹系的成員伺服器。

當您部署 AD DS 時，設定網域和樹系功能等級為您的環境可以支援的最大值。 如此一來，您可以使用多個 AD DS 功能越好。 當您部署新樹系時，系統會提示您將樹系功能等級設定，然後設定 網域功能等級。 您可以設定網域功能等級的值，高於樹系功能等級，但您無法將網域功能等級設定為低於樹系功能等級的值。

Windows 2003 的生命週期結束時，Windows 2003 網域控制站 (Dc) 將需要更新為 Windows Server 2008、 2008 r2，2012、 2012 r2，2016年或 2019年。 如此一來，應該從網域移除任何執行 Windows Server 2003 的網域控制站。

在 Windows Server 2008 和更新版本的網域功能等級，分散式檔案服務 」 (DFS) 複寫來複寫網域控制站之間的 SYSVOL 資料夾的內容。 如果在 Windows Server 2008 網域功能層級或更高版本，您會建立新的網域，則 DFS 複寫會自動使用來複寫 SYSVOL 中。 如果您在建立定義域較低功能層級，您必須從使用 FRS 到 DFS 複寫的 SYSVOL 移轉。 移轉步驟中，您可以其中一個後續[TechNet 上的程序](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)或您可以參考[簡化一組儲存體小組的檔案櫃部落格上的步驟](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

## <a name="windows-server-2019"></a>Windows Server 2019

沒有任何新的樹系或此版本中新增的網域功能等級。

若要新增 Windows Server 2019 網域控制站的最低需求是 Windows Server 2008 r2 功能層級。

## <a name="windows-server-2016"></a>Windows Server 2016

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 樹系功能層級功能

* 所有功能，可在 Windows Server 2012 r2 的樹系功能等級，以及下列功能，都可以使用：
   * [使用 Microsoft Identity Manager (MIM) 特殊權限的存取管理 (PAM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 網域功能層級功能

* 所有預設的 Active Directory 功能、 Windows Server 2012 r2 網域功能等級的所有功能以及下列功能：
   * 網域控制站可以支援自動輪流與 ntlm，並設定為需要 PKI 驗證的使用者帳戶上的其他密碼祕密。 此設定，也就是 「 互動式登入需要智慧卡 」
   * 當使用者限制為特定網域的裝置時，網域控制站可支援允許網路 NTLM。
   * PKInit 有效期限延伸模組已成功驗證的 Kerberos 用戶端會收到最新公開的索引鍵識別的 SID。

    如需詳細資訊，請參閱[What's New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[認證保護中最新消息](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012 r2 的樹系功能層級功能

* 所有功能，可在 Windows Server 2012 樹系功能層級，但沒有其他功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012 r2 網域功能層級功能

* 所有預設的 Active Directory 功能、 Windows Server 2012 網域功能等級，所有功能以及下列功能：
   * Protected Users 的 DC 端保護。 受保護的使用者向 Windows Server 2012 R2 的網域不能再：
      * 使用 NTLM 驗證進行驗證
      * Kerberos 預先驗證中使用 DES 或 RC4 加密套件
      * 委派與非限制或限制委派
      * 超過初始 4 小時存留期之後更新使用者票證 (Tgt)
   * 驗證原則
      * 新樹系為基礎的 Active Directory 原則可以套用至 Windows Server 2012 R2 來控制哪些主機的網域帳戶的帳戶可以登入從和套用存取控制條件來驗證執行身分帳戶的服務。
   * 驗證原則定址接收器
      * 新的樹系架構 Active Directory 物件，可以建立使用者、 受管理的服務和電腦，可用來分類驗證原則或驗證隔離帳戶的帳戶之間的關聯性。

## <a name="windows-server-2012"></a>Windows Server 2012

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 樹系功能層級功能

* 所有功能，可在 Windows Server 2008 R2 樹系功能層級，但沒有其他功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 網域功能層級功能

* 所有預設的 Active Directory 功能、 Windows Server 2008 r2 網域功能等級的所有功能以及下列功能：
   * KDC 支援宣告、 複合驗證以及 Kerberos 防護的 KDC 系統管理範本原則具有需要 Windows Server 2012 網域功能等級的兩個設定 （永遠提供宣告與拒絕未受防護的驗證要求）。 如需詳細資訊，請參閱[What's New in Kerberos 驗證](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008 r2 樹系功能層級功能

* 可以在 Windows Server 2003 樹系功能等級上使用的所有功能，以及下列功能：
   * Active Directory 資源回收筒，在 AD DS 執行時可完整還原其中已刪除的物件。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008 r2 網域功能層級功能

* 所有預設的 Active Directory 功能、 Windows Server 2008 網域功能等級，所有功能以及下列功能：
   * 驗證機制保證以登入方法類型的套件資訊 (智慧卡或使用者名稱/密碼) 來驗證每一個在 Kerberos 權杖中的網域使用者。 在網路環境中部署同盟身分識別管理基礎結構，例如 Active Directory Federation Services (AD FS) 啟用這項功能是語彙基元中的資訊便可被擷取每次使用者嘗試存取任何時若要判斷使用者的登入方法為基礎的授權已開發的宣告感知應用程式。
   * 自動執行的受管理的服務帳戶內容下的特定電腦上時的名稱或 DNS 主機名稱的電腦帳戶變更服務的 SPN 管理。 如需有關受管理的服務帳戶的詳細資訊，請參閱 <<c0> [ 服務帳戶的逐步指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

支援的網域控制站作業系統：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 樹系功能層級功能

* 所有功能，都可在 Windows Server 2003 樹系功能等級，但無額外功能都可用。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 網域功能層級功能

* 所有預設的 AD DS 功能，所有的功能，從 Windows Server 2003 網域功能等級，並有下列功能：
   * 分散式的檔案系統 (DFS) 複寫支援的 Windows Server 2003 系統磁碟區 (SYSVOL)
      * DFS 複寫的支援會提供更健全與細緻的 SYSVOL 內容複寫。
        [!NOTE]>
        >從 Windows Server 2012 R2 開始，檔案複寫服務 (FRS) 已被取代。 會在至少執行的網域控制站建立新的網域必須設定 Windows Server 2012 R2，Windows Server 2008 網域功能等級為或更高。

   * 網域型 DFS 命名空間執行在 Windows Server 2008 模式中，其中包含對存取式列舉和增強的延展性的支援。 在 Windows Server 2008 模式中的網域型命名空間也需要使用 Windows Server 2003 樹系功能等級的樹系。 如需詳細資訊，請參閱 <<c0> [ 選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。
   * Kerberos 通訊協定的進階的加密標準 （AES 128 與 256） 支援。 為了讓使用 AES 發出的 Tgt，網域功能等級必須是 Windows Server 2008 或更新版本，並需要變更網域密碼。 
      * 如需詳細資訊，請參閱 < [Kerberos 增強功能](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。
        [!NOTE]>
        >驗證錯誤，可能發生網域控制站上，網域功能等級提高到 Windows Server 2008 或更高版本之後如果網域控制站已經被複寫的網域功能等級變更，但未尚未重新整理 krbtgt 密碼。 在此情況下，重新啟動網域控制站上的 KDC 服務將會觸發新的 krbtgt 密碼的記憶體中重新整理，並解決相關的驗證錯誤。

   * [上次互動式登入](https://go.microsoft.com/fwlink/?LinkId=180387)資訊會顯示下列資訊：
      * 在已加入網域的 Windows Server 2008 server 或 Windows Vista 工作站的失敗登入嘗試總次數
      * 在 Windows Server 2008 server 或 Windows Vista 工作站的成功登入後的失敗登入嘗試總次數
      * 嘗試在 Windows Server 2008 或 Windows Vista 工作站的最後一個失敗的登入的時間
      * 嘗試在 Windows Server 2008 server 或 Windows Vista 工作站的最後一個成功的登入的時間
   * 更細緻的密碼原則，讓您指定網域中的使用者和全域安全性群組的密碼與帳戶鎖定原則。 如需詳細資訊，請參閱 <<c0> [ 更細緻的密碼和帳戶鎖定原則設定的逐步指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
   * 個人虛擬桌面
      * 若要使用 Active Directory 使用者和電腦中的 [使用者帳戶內容] 對話方塊中的 [個人虛擬桌面] 索引標籤所提供的新增的功能，您的 AD DS 結構描述必須延伸 Windows Server 2008 r2 (結構描述物件版本 = 47)。 如需詳細資訊，請參閱 <<c0> [ 部署個人虛擬桌面藉由使用 RemoteApp 與桌面連線的逐步指南 》](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

支援的網域控制站作業系統：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 樹系功能層級功能

* 預設 AD DS 的功能，以及下列功能，均可取得：
   * 樹系信任
   * 網域重新命名
   * 連結數值複寫
      - 連結數值複寫可讓您變更群組成員資格，以便儲存和複寫值，而不是複寫整個視為單一單位的成員資格的個別成員。 儲存和複寫的個別成員的值會使用較少的網路頻寬和較少的處理器週期，在複寫期間，並可避免您遺失的更新，當您新增或移除多個成員，同時在不同的網域控制站。
   * 部署唯讀網域控制站 (RODC) 的能力
   * 改進的知識一致性檢查程式 (KCC) 演算法與延展性
      - 站台間拓撲產生器 (ISTG) 使用改進擴充到比 AD DS 可支援在 Windows 2000 樹系功能等級的樹系的更多站台支援的演算法。 改良的 ISTG 選擇演算法是較不干擾的機制中選擇 ISTG 時在 Windows 2000 樹系功能等級。
   * 建立名為的動態輔助類別的執行個體的能力**dynamicObject**在網域目錄磁碟分割
   * 要轉換的能力**inetOrgPerson**物件執行個體**使用者**物件執行個體，並完成在相反方向的轉換
   * 建立新的群組類型，以支援角色型授權的執行個體的能力。 
      - 這些類型稱為應用程式基本群組和 LDAP 查詢群組。
   * 停用和重新定義架構中的屬性及類別。 可重複使用的下列屬性： ldapDisplayName、 schemaIdGuid、 OID，和 mapiID。
   * 網域型 DFS 命名空間執行在 Windows Server 2008 模式中，其中包含對存取式列舉和增強的延展性的支援。 如需詳細資訊，請參閱 <<c0> [ 選擇命名空間類型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 網域功能層級功能

* 所有預設 AD DS 的功能、 Windows 2000 原生網域功能等級，則適用的所有功能以及下列功能有：
   * 網域管理工具 Netdom.exe，可讓您重新命名網域控制站
   * 登入時間戳記更新
      * 利用使用者或電腦最後登入時間來更新 **lastLogonTimestamp** 屬性。 而且會將此屬性複寫至整個網域。
   * 若要設定的能力**userPassword**屬性的有效密碼**inetOrgPerson**和使用者物件
   * 它能夠重新導向使用者和電腦容器
      * 根據預設，兩個已知容器系統會提供裝載電腦和使用者帳戶，也就是，cn = Computers，<domain root>和 cn = Users，<domain root>。 這項功能可讓新，這是已知的位置給這些帳戶的定義。
   * 授權管理員可以將其授權原則儲存在 AD DS 的能力
   * 限制委派
      * 限制的委派，可讓應用程式利用透過 Kerberos 驗證的使用者認證的安全委派。
      * 您可以限制只有特定的目的地服務的委派。
   * 選擇性驗證
      * 選擇性驗證，可以讓您指定的使用者和群組從受信任的樹系可信任的樹系中的資源伺服器進行驗證。

## <a name="windows-2000"></a>Windows 2000

支援的網域控制站作業系統：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 原生的樹系功能層級功能

* 預設的 AD DS 功能均可取得。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 原生網域功能層級功能

* 所有的預設 AD DS 功能以及下列的目錄功能都可包括：
   * 通訊和安全性群組的萬用群組。
   * 群組巢狀結構
   * 群組轉換，可讓安全性與通訊群組之間的轉換
   * 安全性識別碼 (SID) 歷程記錄

## <a name="next-steps"></a>後續步驟

* [提高網域功能等級](https://technet.microsoft.com/library/cc753104.aspx)  
* [提高樹系功能等級](https://technet.microsoft.com/library/cc730985.aspx)
