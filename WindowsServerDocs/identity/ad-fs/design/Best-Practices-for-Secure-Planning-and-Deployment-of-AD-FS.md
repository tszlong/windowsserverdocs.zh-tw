---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: 安全規劃和部署 AD FS 的最佳做法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 95f9fd468df39525a2fe7d18647f399214486bbb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280602"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>安全規劃和部署 AD FS 的最佳做法


本主題提供可協助您規劃和評估安全性，當您設計您的 Active Directory Federation Services (AD FS) 部署的最佳做法資訊。 本主題會檢閱和評估會影響您的 AD FS 使用的整體安全性的考量事項的起始點。 這個主題中的資訊是用來補充及延伸現有安全性規劃及其他設計最佳做法。  
  
## <a name="core-security-best-practices-for-ad-fs"></a>AD FS 的核心安全性最佳做法  
下列的核心最佳做法通用於所有的 AD FS 安裝您想要用來改善或擴充設計或部署的安全性：  

-   **保護 AD FS 做為 「 第 0 層 」 系統** 

    AD FS 基本上是驗證系統。  因此，它應該被視為 「 第 0 層 」 系統，例如 網路上其他身分識別系統。  [Microsoft Docs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)有 Active Directory 系統管理層模型的詳細資訊。 


-   **若要將 AD FS 特定的安全性最佳作法套用到同盟伺服器和同盟伺服器 proxy 電腦使用安全性設定精靈**  
  
    安全性設定精靈 (SCW) 是所有的 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 電腦上預先安裝的工具。 您可以根據所安裝的伺服器角色，使用它來套用可協助減少伺服器攻擊面的安全性最佳做法。  
  
    當您安裝 AD FS 時，安裝程式會建立角色延伸檔案，您可以將該檔案與 SCW 搭配使用來建立安全性原則，此原則將套用到您在安裝期間選擇的特定 AD FS 伺服器角色 (可能是同盟伺服器或同盟伺服器 Proxy)。  
  
    每個安裝的角色延伸檔案都代表每台電腦所設定的角色和子角色類型。 C:WindowsADFSScw 目錄中，會安裝下列角色延伸檔案：  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (只有當您在同盟伺服器 Proxy 角色中設定電腦時，這個檔案才會存在)。  
  
    若要在 SCW 中套用 AD FS 角色延伸，請依序完成下列步驟：  
  
    1.  安裝 AD FS，然後為該電腦選擇適當的伺服器角色。 如需詳細資訊，請參閱 <<c0> [ 安裝 Federation Service Proxy 角色服務](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md)AD FS 部署指南中。  
  
    2.  使用 Scwcmd 命令列工具，登錄適當的角色延伸檔案。 請參閱下表，以取得如何在為電腦設定的角色中使用這個工具的詳細資料。  
  
    3.  確認命令已順利完成，藉由檢查的 SCWRegister_log.xml 檔案，位於 WindowssecurityMsscwLogs 目錄中。  
  
    您必須在想要套用以 AD FS 為基礎的 SCW 安全性原則的每台同盟伺服器或同盟伺服器 Proxy 電腦上，執行所有的這些步驟。  
  
    下表說明如何根據您在安裝 AD FS 的電腦上選擇的 AD FS 伺服器角色，來登錄適當的 SCW 角色延伸。  
  
    |AD FS 伺服器角色|使用的 AD FS 設定資料庫|在命令提示字元中輸入下列命令：|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |獨立同盟伺服器|Windows 內部資料庫|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |已加入伺服陣列的同盟伺服器|Windows 內部資料庫|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |已加入伺服陣列的同盟伺服器|[SQL Server]|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |同盟伺服器 Proxy|N/A|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    如需您可以與 AD FS 搭配使用之資料庫的相關詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **安全性是非常重要的考量，例如，使用 kiosk 時的情況下使用權杖重新執行偵測。**  
    權杖重新執行偵測是一項功能可確保偵測到任何嘗試重新執行權杖要求，對同盟服務，並會捨棄該要求的 AD fs。 預設會啟用權杖重新執行偵測。 這個功能可藉由確保同一個權杖絕對不會使用超過一次，針對 WS-Federation 被動設定檔和安全性聲明標記語言 (SAML) WebSSO 設定檔來運作。  
  
    啟動 Federation Service 時，即會開始建置它所履行之任何權杖要求的快取。 經過一段時間之後，因為已將後續的權杖要求新增到快取，因此能夠提高 Federation Service 偵測任何嘗試多次重新執行權杖要求的能力。 如果您停用權杖重新執行偵測且之後選擇再次啟用它，請記住，Federation Service 仍然會在一段期間內接受先前已使用的權杖，直到重新執行快取有足夠的時間重新建置其內容。 如需詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **特別是當您使用支援 SAML 成品解析，請使用權杖加密。**  
  
    若要提高安全性和保護來抵禦潛在可能會嘗試攻擊您的 AD FS 部署的攔截層 (MITM) 攻擊強烈建議您使用權杖加密。 使用加密可能會對整體產生些微影響，但一般來說，通常這類影響應該不會被注意到，而且在許多部署中，更高安全性的好處遠超過伺服器效能方面所產生的任何成本。  
  
    若要啟用權杖加密，首先請針對您的信賴憑證者信任設定新增加密憑證。 您可以在建立信賴憑證者信任時或在之後設定加密憑證。 若要將加密憑證新增至現有信賴憑證者信任的更新版本，您可以設定使用的憑證**加密**信任內容，同時使用 AD FS 嵌入式管理單元中的索引標籤。 若要指定使用 AD FS cmdlet 現有信任的憑證，請使用 EncryptionCertificate 參數**Set-claimsprovidertrust**或是**Set-relyingpartytrust** cmdlet。 若要設定 Federation Service 解密權杖時所要使用的憑證，請使用**Set-adfscertificate** cmdlet 並指定"`Token-Encryption`"的*CertificateType*參數。 可以使用 *Set-RelyingPartyTrust* Cmdlet 的 **EncryptClaims** 參數來啟用和停用特定信賴憑證者信任的加密。  
  
-   **使用驗證擴充的保護**  
  
    為了保護您的部署，您可以設定，以及使用延伸的保護與 AD FS 的驗證功能。 此設定指定同盟伺服器所支援的驗證擴充保護層的級。  
  
    驗證的延伸保護可協助提供保護，以抵禦攔截式 (MITM) 攻擊，攻擊者會在攻擊時攔截用戶端認證並將它們轉送到伺服器。 您能夠透過通道繫結權杖 (CBT) 提供保護來抵禦這類攻擊，對伺服器而言，當它建立與用戶端的通訊時，這類保護可能是必要、允許，或非必要的。  
  
    若要啟用延伸保護功能，請使用 **ExtendedProtectionTokenCheck** 參數 (位於 **Set-ADFSProperties** Cmdlet 上)。 下表說明這個設定的可能值以及該值所提供的安全性等級。  
  
    |參數值|安全性層級|保護設定|  
    |-------------------|------------------|----------------------|  
    |必要|會完全強化伺服器。|會強制執行且一律必須強制執行延伸的保護。|  
    |允許|會部分強化伺服器。|若已修補所涉及的系統來支援此功能，即會強制執行延伸的保護。|  
    |None|伺服器容易受到攻擊。|不會強制執行延伸的保護。|  
  
-   **如果您使用記錄和追蹤，請確定任何敏感資訊的隱私權。**  
  
    AD FS 不會根據預設，公開或追蹤個人識別資訊 (PII) 直接在 Federation Service 或正常運作。 當 AD FS 中啟用事件記錄和偵錯追蹤記錄時，不過，根據您設定某些宣告的宣告原則類型及其相關聯的值可能會包含可能會記錄在 AD FS 事件或追蹤記錄的 PII。  
  
    因此，強烈建議強制執行的 AD FS 設定和其記錄檔的存取控制。 如果您不想顯示此類資訊，您應該先停用登入，或將您記錄中的任何 PII 或敏感資料篩除掉，然後再與他人共用資訊。  
  
    下列秘訣可協助您防止意外公開記錄檔的內容：  
  
    -   請確定 AD FS 事件記錄檔和追蹤記錄檔會受到存取控制清單 (ACL)，限制只有這些受信任的系統管理員需要存取它們。  
  
    -   不要使用透過 Web 要求即可輕易取得的副檔名或路徑來複製或封存記錄檔。 例如，.xml 副檔名就不是安全的選擇。 您可以檢查網際網路資訊服務 (IIS) 管理指南，以查看可提供的副檔名清單。  
  
    -   如果您修改記錄檔的路徑，請確定有指定記錄檔位置的絕對路徑，這個位置應該是在 Web 主機虛擬根 (vroot) 公開目錄之外，以防止外部人員使用 Web 瀏覽器來存取它。  

-   **AD FS 外部網路軟式鎖定和外部網路的 AD FS 的智慧鎖定保護**  
    
    如果攻擊者都透過 Web Application Proxy 的 invalid(bad) 密碼驗證要求的形式，AD FS 外部網路鎖定可讓您從 AD FS 帳戶鎖定保護您的使用者。 除了從 AD FS 保護您的使用者帳戶鎖定，AD FS 外部網路鎖定也會防止暴力密碼破解攻擊。  
    
    適用於 Windows Server 2012 R2 上的 AD FS 外部網路的軟式鎖定，請參閱[AD FS 外部網路軟式鎖定保護](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。  

     如為 Windows Server 2016 上的 AD FS 外部網路的智慧鎖定[AD FS 外部網路智慧鎖定保護](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)。  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>適用於 AD FS 的 SQL Server 特定的安全性最佳做法  
當這些資料庫技術來管理 AD FS 設計和部署中的資料時，下列安全性最佳作法特有 Microsoft SQL Server® 或 Windows 內部資料庫 (WID) 的使用。  
  
> [!NOTE]  
> 這些建議的用意是要延伸 (而非取代) SQL Server 產品安全性指導方針。 如需規劃安全 SQL Server 安裝的詳細資訊，請參閱[安全 SQL 安裝的安全性考量](https://go.microsoft.com/fwlink/?LinkID=139831)(https://go.microsoft.com/fwlink/?LinkID=139831) 。  
  
-   **一律將 SQL Server 部署在實體上安全的網路環境使用防火牆。**  
  
    絕對不應將 SQL Server 安裝直接向網際網路公開。 只有在您的資料中心內的電腦應該能夠連線到您支援 AD FS 的 SQL server 安裝。 如需詳細資訊，請參閱 <<c0> [ 安全性最佳做法檢查清單](https://go.microsoft.com/fwlink/?LinkID=189229)(https://go.microsoft.com/fwlink/?LinkID=189229) 。  
  
-   **執行 SQL Server，而不是使用內建的預設系統服務帳戶的服務帳戶。**  
  
    根據預設，通常會安裝並設定 SQL Server 來使用其中一個支援的內建系統帳戶，例如 LocalSystem 或 NetworkService 帳戶。 若要增強的 SQL Server 安裝 AD FS 的安全性，盡可能使用個別的任一處服務帳戶來存取您的 SQL Server 服務和登錄中的這個帳戶的安全性主體名稱 (SPN)，藉以啟用 Kerberos 驗證程式Active Directory 部署。 這會在用戶端和伺服器之間啟用相互驗證。 在沒有為個別服務帳戶登錄 SPN 的情況下，SQL Server 將針對 Windows 型驗證使用 NTLM，這只會驗證用戶端。  
  
-   **最小化 SQL Server 的介面區。**  
  
    只啟用必要的 SQL Server 端點。 根據預設，SQL Server 會提供單一內建 TCP 端點，且無法移除。 適用於 AD FS，您應該啟用這個 TCP 端點，Kerberos 驗證。 若要檢閱目前的 TCP 端點以查看是否已將其他使用者定義的 TCP 連接埠新增到 SQL 安裝，您可以在 Transact-SQL (T-SQL) 工作階段中使用 "SELECT * FROM sys.tcp_endpoints" 查詢陳述式。 如需有關 SQL Server 端點設定的詳細資訊，請參閱[How To:設定 Database Engine 接聽多個 TCP 通訊埠](https://go.microsoft.com/fwlink/?LinkID=189231)(https://go.microsoft.com/fwlink/?LinkID=189231) 。  
  
-   **請避免使用以 SQL 為基礎的驗證。**  
  
    為避免必須透過網路以純文字形式傳輸密碼或在組態設定中儲存密碼，請只將 Windows 驗證與 SQL Server 安裝搭配使用。 SQL Server 驗證是舊有的驗證模式。 不建議您在使用 SQL Server 驗證時儲存結構化查詢語言 (SQL) 登入認證 (SQL 使用者名稱和密碼)。 如需詳細資訊，請參閱 <<c0> [ 驗證模式](https://go.microsoft.com/fwlink/?LinkID=189232)(https://go.microsoft.com/fwlink/?LinkID=189232) 。  
  
-   **請仔細評估 SQL 安裝中的其他通道安全性的需求。**  
  
    即使是使用 Kerberos 驗證，SQL Server 安全性支援提供者介面 (SSPI) 還是不提供通道等級的安全性。 但是，對於伺服器安全地位於受防火牆保護之網路上的安裝而言，可能就不需要將 SQL 通訊加密。  
  
    雖然加密是一項可協助確保安全性的好用工具，但還是不應該將它視為適用於所有資料或連線。 當您在決定是否要實作加密時，請考量使用者將存取資料的方式。 如果使用者是透過公用網路存取資料，可能就需要資料加密來增加安全性。 不過，如果 AD fs 的 SQL 資料的所有存取牽涉都到與安全內部網路設定，加密不可能需要。 任何加密的用法也都應該包含對密碼、金鑰及憑證的維護策略。  
  
    如果對於任何 SQL 資料可能透過網路被看見或遭到竄改有所疑慮，請使用網際網路通訊協定安全性 (IPsec) 或安全通訊端層 (SSL)，來協助保護您的 SQL 連線。 不過，這可能需要在 SQL Server 效能，這可能會影響或限制在某些情況下的 AD FS 效能產生負面影響。 比方說，從 SQL 屬性存放區查閱而言非常重要，對權杖發行時，可能會降低在權杖發行的 AD FS 效能。 您可以設定強大的周邊安全性設定，來更完善地排除 SQL 竄改威脅。 例如，對於保護 SQL Server 安裝而言，更好解決方案是確保網際網路使用者和電腦無法存取它，而且只有位於您資料中心環境內的使用者和電腦可以存取它。  
  
    如需詳細資訊，請參閱 <<c0> [ 加密 SQL Server 的連接](https://go.microsoft.com/fwlink/?LinkID=189234)或是[SQL Server 加密](https://go.microsoft.com/fwlink/?LinkID=189233)。  
  
-   **使用預存程序來執行所有以 SQL 為基礎的查閱 AD FS 的 SQL 預存的資料，以設定安全設計的存取。**  
  
    若要提供更安全的服務和資料隔離，您可以針對所有屬性存放區查閱命令建立預存程序。 您可以建立資料庫角色，然後授與權限以執行預存程序。 將 AD FS Windows 服務的服務識別指派給此資料庫角色中。 AD FS Windows 服務應該不能執行任何其他 SQL 陳述式，不用於查閱屬性的適當預存程序。 以這種方式鎖定對 SQL Server 資料庫的存取，可降低受到「權限提高」攻擊的風險。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
