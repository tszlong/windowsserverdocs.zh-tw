---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: "規劃安全和部署 AD FS 的最佳做法"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>規劃安全和部署 AD FS 的最佳做法

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供最佳資訊來幫助您計劃以及當您設計 Active Directory 同盟 Services (AD FS) 部署評估安全性。 此主題是 「 檢視及評估會影響您的 AD FS 使用的整體安全性考量的起點。 此主題中的資訊是用來與和延伸您的現有安全性計劃與其他設計最佳做法的規範。  
  
## <a name="core-security-best-practices-for-ad-fs"></a>AD fs 核心安全性最佳做法  
下列幾個核心最佳的常見的所有 AD FS 安裝您想要改善或擴充設計或部署安全性：  
  
-   **AD FS 特定安全的最佳做法會套用到聯盟伺服器與聯盟 proxy 伺服器的電腦使用的安全性設定精靈**  
  
    安全性設定精靈 (SCW) 是在 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 的電腦進入預先安裝的工具。 您可以使用它來適用的最佳做法，可協助您減少伺服器，根據您所安裝的伺服器角色攻擊 surface 安全性。  
  
    當您安裝 AD FS 時，安裝程式會建立角色擴充功能來建立安全性原則，將會套用到特定 AD FS 伺服器角色 （聯盟伺服器或聯盟伺服器 proxy），您在設定期間選擇使用 SCW 的檔案。  
  
    安裝每個角色延伸模組檔案代表角色 subrole 每一台電腦的設定類型。 下列角色延伸檔案安裝 C:WindowsADFSScw directory 中：  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml （是您在聯盟 proxy 伺服器的角色設定電腦時，才有此檔案）。  
  
    若要套用中 SCW AD FS 角色擴充功能，請完成順序下列步驟：  
  
    1.  安裝 AD FS，然後選擇該電腦的適當的伺服器角色。 如需詳細資訊，請查看[安裝同盟服務 Proxy 角色服務](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md)中的 AD FS 部署。  
  
    2.  登記使用 Scwcmd 命令列工具適當的角色延伸模組檔案。 查看下表中的角色您的電腦設定中使用此工具的相關詳細資訊。  
  
    3.  請確認命令已成功完成，請檢查位於 WindowssecurityMsscwLogs directory SCWRegister_log.xml 檔案。  
  
    您必須在每個聯盟伺服器或想要套用 AD FS 為基礎 SCW 安全性原則聯盟伺服器 proxy 電腦上執行所有這些步驟。  
  
    下表如何登記適當 SCW 角色擴充功能，根據您選擇您已安裝 AD FS 使用的電腦的 AD FS 伺服器角色。  
  
    |AD FS 伺服器角色|AD FS 使用的設定資料庫|在命令提示字元中輸入下列命令：|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |聯盟獨立伺服器|Windows 內部資料庫|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |發電廠加入聯盟伺服器|Windows 內部資料庫|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |發電廠加入聯盟伺服器|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |聯盟伺服器 proxy|不適用|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    如需資料庫，您可以使用 AD FS 使用的詳細資訊，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **在中安全性時非常重要的問題，例如 kiosk 時使用權杖重播偵測功能。**  
    權杖重播偵測是 AD FS 可確保偵測到任何嘗試重新執行的專為同盟服務權杖要求並捨棄要求的功能。 預設的話權杖重播偵測功能。 這也適用於 WS 同盟被動式設定檔和安全性判斷提示標記的語言 (SAML) WebSSO 設定檔來確保相同權杖一律不會超過一次。  
  
    聯盟服務時，它開始建置實現任何權杖要求的快取。 長時間在後續權杖要求加入的快取，來偵測到重新執行權杖要求多次任何嘗試的功能增加同盟服務。 如果您重新執行權杖偵測停用，稍後再試一次讓它，請記住同盟服務會仍接受一段時間，檔案可能使用之前，直到重新顯示快取的已允許時間不足重建內容權杖選擇。 如需詳細資訊，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **尤其是當您正在使用支援 SAML 成品解析度，請使用權杖加密。**  
  
    加密權杖會建議您提高安全性與防護可能在中央男人 (MITM)，可能會嘗試針對 AD FS 部署。 使用使用加密可能會有輕微影響整個，但通常，它應該不會通常注意到而且在許多部署適用於更高安全性的優點超過成本則伺服器的效能。  
  
    若要以便權杖加密，第一組新增加密您依賴廠商信任的憑證。 您可以設定可能可以在建立時廠商信任的加密憑證或更新版本。 將的加密憑證新增至現有信賴廠商信任的之後，您可以在設定使用的憑證**加密**索引標籤中時使用 AD FS 信任屬性。 指定使用 AD FS cmdlet 現有信任的憑證，請使用任一個的 EncryptionCertificate 參數**設定為 ClaimsProviderTrust**或**設定為 RelyingPartyTrust** cmdlet。 設定同盟服務解密權杖時要使用的憑證，請使用**設定為 ADFSCertificate** cmdlet 並指定 」`Token-Encryption`」 的*CertificateType*參數。 讓和停用加密的特定信賴信任可透過使用*EncryptClaims*的參數**設定為 RelyingPartyTrust** cmdlet。  
  
-   **利用驗證延伸的保護**  
  
    為了協助保護您的部署，您可以設定並使用 AD FS 進行驗證功能的延伸的保護。 此設定中指定延伸支援聯盟伺服器的驗證的保護層的級。  
  
    驗證延伸的保護可協助防護在中央男人 (MITM) 攻擊，攻擊攔截 client 認證然後轉寄給伺服器。 這類防護是因為透過通道繫結權杖 (CBT) 可以可能需要、 允許，或建立與戶端通訊時，不需要伺服器。  
  
    要保護的延伸的功能，請使用**ExtendedProtectionTokenCheck**上的參數**設定為 ADFSProperties** cmdlet。 下表描述此設定和層級的安全性，值提供可能值。  
  
    |讓參數值|安全性層級|保護設定|  
    |-------------------|------------------|----------------------|  
    |需要|完全強化伺服器。|延伸的保護會執行並一定。|  
    |允許|部分強化伺服器。|延伸的保護會執行系統有修補支援。|  
    |無|雖然伺服器。|不被執行延伸的保護。|  
  
-   **如果您使用的登入和追蹤，確認所有敏感資訊的隱私權。**  
  
    AD FS 不會預設公開或直接曲目同盟服務或正常運作的一部分 (PII) 的個人資訊。 當事件登入及偵錯追蹤登入 AD FS 中的功能時，但是，根據您所設定的部分宣告宣告原則類型與自己相關的值可能包含 PII 可能會在登入 AD FS 事件或追蹤登。  
  
    因此，非常建議執行存取控制 AD FS 設定和檔案登入。 如果您不想資訊，可看見這類，您應該停用 loggin，或之前與其他人共用您登入篩選掉任何 PII 或敏感性資料。  
  
    以下秘訣，可協助您避免 content 的登入檔案不小心被公開：  
  
    -   請確定 AD FS 事件登入和追蹤登入檔案保護的存取控制清單 (ACL)，以限制只有這些受信任的系統管理員需要存取權限存取。  
  
    -   請勿複製或封存登入檔案，使用副檔名或可輕鬆地提供使用的 Web 要求的路徑。 例如，.xml 檔案副檔名並不安全的選擇。 您可以檢查網際網路服務 (IIS) 管理指南看到擴充功能，可提供一份。  
  
    -   如果修改登入檔案的路徑，請務必指定絕對應該以外 Web 主機 virtual 根 (vroot) 公用 directory 防止存取外部廠商使用網頁瀏覽器登入檔案位置的路徑。  

-   **AD FS 外部鎖定保護**  
    
    來自 Web 應用程式 Proxy invalid(bad) 密碼驗證要求的形式攻擊，在 AD FS 外部鎖定可讓您從 AD FS 鎖定保護您的使用者。 除了從 AD FS 保護您的使用者帳號鎖定，AD FS 外部鎖定也會防止猜測攻擊暴力密碼。  如需詳細資訊請查看[AD FS 外部鎖定保護](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md)。  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>AD fs 的 SQL Server – 特定安全性最佳做法  
下列幾個安全性最佳專屬於 Microsoft SQL Server® 或 Windows 內部資料庫 (WID) 使用時這些資料庫技術管理 AD FS 設計和部署的資料。  
  
> [!NOTE]  
> 這些建議是延長，但不會取代，SQL Server product 安全性指南。 如需有關計劃的安全 SQL Server 安裝，請查看[安全性考量安全 SQL 安裝的](https://go.microsoft.com/fwlink/?LinkID=139831)(https://go.microsoft.com/fwlink/?LinkID=139831)。  
  
-   **隨時防火牆實體安全的網路環境中部署 SQL Server。**  
  
    不應直接與網際網路公開 SQL Server 安裝。 您應該可以瑞曲之戰 SQL server 安裝支援 AD FS 資料中心中的電腦。 如需詳細資訊，請查看[安全性最佳做法檢查清單](https://go.microsoft.com/fwlink/?LinkID=189229)(https://go.microsoft.com/fwlink/?LinkID=189229)。  
  
-   **執行而不是使用建預設系統服務帳號服務 account SQL Server。**  
  
    根據預設，SQL Server 通常安裝和使用其中一個支援的建系統帳號，例如帳號 LocalSystem 或其他設定。 若要提升 AD fs SQL Server 安裝的安全性，不論可能來存取您 SQL Server 服務使用不同的服務帳號以及 Kerberos 驗證，在 Active Directory 部署登記這個 account 安全性主體名稱 (SPN)。 這可讓 client 和 server 之間的相互驗證。 SPN 登記的不同服務帳號，而 SQL Server 將使用 NTLM 適用於 windows 的驗證，驗證只 client 的位置。  
  
-   **最小化 surface SQL Server 區域。**  
  
    讓所需 SQL Server 端點。 根據預設，SQL Server 提供無法移除的單一建 TCP 端點。 AD fs，您應該讓 F:kerberos 驗證此 TCP 結束點。 若要檢視目前 TCP 端點以查看是否其他使用者定義 TCP 連接埠] 會新增至 SQL 安裝，您可以使用 「 選取 * sys.tcp_endpoints 從 「 查詢聲明 SQL (SQL At&t) 的活動中。 如需 SQL Server 端點設定的詳細資訊，請查看[如何：多 TCP 連接埠設定資料庫引擎接聽](https://go.microsoft.com/fwlink/?LinkID=189231)(https://go.microsoft.com/fwlink/?LinkID=189231)。  
  
-   **避免使用 SQL 架構的驗證。**  
  
    若要避免明文密碼傳輸到您的網路，或將密碼儲存在設定，Windows 驗證只適用於您安裝 SQL Server。 SQL Server 驗證是舊版驗證模式。 儲存結構化查詢的語言 (SQL) 登入認證 （SQL 使用者名稱和密碼） 使用時 SQL Server 驗證不建議。 如需詳細資訊，請查看[驗證模式](https://go.microsoft.com/fwlink/?LinkID=189232)(https://go.microsoft.com/fwlink/?LinkID=189232)。  
  
-   **仔細評估 SQL 安裝中的其他頻道安全性的需求。**  
  
    事實上，即使有 F:kerberos 驗證 SQL Server 安全性支援提供者介面 (SSPI) 並不會提供通道層級的安全性。 不過，安裝中伺服器確實位於防火牆受保護的網路，加密 SQL 通訊可能不需。  
  
    雖然加密可協助您確保安全性寶貴工具，就不能視為適用於所有的資料或連接。 當您決定是否實作加密時，請考慮使用者存取資料的方式。 如果使用者透過公用網路存取的資料，可能需要提高安全性資料加密。 不過，如果 AD FS SQL 資料的所有存取都包含內部安全的網路設定，加密可能不需。 使用任何加密應該也包含密碼、 按鍵，以及憑證維護策略。  
  
    如果有任何 SQL 資料，可能會看到或竄改透過您的網路，以協助保護您的 SQL 連接使用網際網路通訊協定的安全性 (IPsec) 或安全通訊端層 (SSL) 的問題。 不過，這可能會影響負 SQL Server 效能，這可能會影響或限制有時 AD FS 效能。 例如，AD FS 效能權杖發行也可能降低當 SQL 為基礎的屬性存放區的屬性對應權杖發行的重要。 您可以更排除 SQL 竄改威脅所遇到周邊安全性設定。 保護您的 SQL Server 安裝好方案，例如是確保該 app 會維持網際網路使用者無法存取和電腦，且該仍然可以存取只的使用者或電腦 datacenter 環境中。  
  
    如需詳細資訊，請查看[加密連接 SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234)或[SQL Server 加密](https://go.microsoft.com/fwlink/?LinkID=189233)。  
  
-   **安全地設計的存取設定儲存程序使用 AD FS 的 SQL 儲存的資料來執行所有 SQL 為基礎的對應。**  
  
    為了提供更好的服務，資料隔離，您可以建立所有屬性市集中搜尋命令儲存程的序。 您可以建立的您再授予權限來執行儲存程序資料庫角色。 將服務的身分 AD FS Windows 服務指派給此資料庫角色。 AD FS Windows 服務不應該可以執行任何其他 SQL 隱私權聲明，適用於屬性對應的適當儲存程序以外。 鎖定存取這種方式 SQL Server 資料庫降低攻擊權限提高權限的風險。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
