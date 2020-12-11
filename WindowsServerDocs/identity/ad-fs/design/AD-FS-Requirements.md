---
description: 深入瞭解： Windows Server AD FS 需求
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Windows Server AD FS 需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 03b6d03910b3d9cef0c886801e2018c62b0f04b3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043286"
---
# <a name="ad-fs-requirements-for-windows-server"></a>Windows Server AD FS 需求

以下是部署 AD FS 時必須符合的各種需求：

- [憑證需求](AD-FS-Requirements.md#BKMK_1)

- [硬體需求](AD-FS-Requirements.md#BKMK_2)

- [軟體需求](AD-FS-Requirements.md#BKMK_3)

- [AD DS 需求](AD-FS-Requirements.md#BKMK_4)

- [設定資料庫需求](AD-FS-Requirements.md#BKMK_5)

- [瀏覽器需求](AD-FS-Requirements.md#BKMK_6)

- [外部網路需求](AD-FS-Requirements.md#BKMK_extranet)

- [網路需求](AD-FS-Requirements.md#BKMK_7)

- [屬性存放區需求](AD-FS-Requirements.md#BKMK_8)

- [應用程式需求](AD-FS-Requirements.md#BKMK_9)

- [驗證需求](AD-FS-Requirements.md#BKMK_10)

- [Workplace join 需求](AD-FS-Requirements.md#BKMK_11)

- [密碼編譯需求](AD-FS-Requirements.md#BKMK_12)

- [權限需求](AD-FS-Requirements.md#BKMK_13)

## <a name="certificate-requirements"></a><a name="BKMK_1"></a>憑證需求
憑證在保護同盟伺服器、Web 應用程式 proxy、宣告感知應用程式和 Web 用戶端之間的通訊時，扮演最重要的角色。 憑證的需求取決於您要設定同盟伺服器或 proxy 電腦，如本節所述。

**同盟伺服器憑證**

| **憑證類型** | **需求，支援 & 須知事項** |
|--|--|
| **安全通訊端層 (SSL) 憑證：** 這是標準 SSL 憑證，用於保護同盟伺服器和用戶端之間的通訊。 | -此憑證必須是公開信任的 * X509 v3 憑證。<br>-所有存取任何 AD FS 端點的用戶端都必須信任此憑證。 強烈建議使用公開 (協力廠商) 證書 (頒發機構單位) 所發行的憑證。 您可以在測試實驗室環境中的同盟伺服器上成功使用自我簽署 SSL 憑證。 不過，在生產環境中，我們建議您從公用 CA 取得憑證。<br>-支援 Windows Server 2012 R2 for SSL 憑證所支援的任何金鑰大小。<br>-不支援使用 CNG 金鑰的憑證。<br>-搭配 Workplace Join/裝置註冊服務使用時，AD FS 服務之 SSL 憑證的主體別名必須包含值 enterpriseregistration，後面接著使用者主體名稱 (UPN) 尾碼（例如 enterpriseregistration.contoso.com）。<br>-支援萬用字元憑證。 當您建立 AD FS 伺服器陣列時，系統會提示您提供 AD FS 服務的服務名稱 (例如 **adfs.contoso.com**。<br>-強烈建議您對 Web 應用程式 Proxy 使用相同的 SSL 憑證。 但是，透過 Web 應用程式 Proxy 支援 Windows 整合式驗證端點時，以及開啟 [擴充保護驗證] (預設設定) 時 **，必須是** 相同的。<br>-此憑證的主體名稱可用來代表您部署的每個 AD FS 實例的同盟服務名稱。 基於這個理由，您可能想要考慮在任何 CA 簽發的新憑證上，選擇對合作夥伴而言最能代表貴公司或組織名稱的主體名稱。<br> 憑證的身分識別必須符合 federation service 名稱 (例如，fs.contoso.com) 。身分識別可以是 dNSName 類型的主體別名延伸，如果沒有主體的別名專案，則會指定為一般名稱的主體名稱。 有多個主體別名專案可以存在於憑證中，前提是其中一個符合 federation service 名稱。<br>- **重要：** 強烈建議在您的 AD FS 伺服器陣列的所有節點以及 AD FS 伺服器陣列中的所有 Web 應用程式 proxy，使用相同的 SSL 憑證。 |
| **服務通訊憑證：** 此憑證可讓 WCF 訊息安全性安全，以保護同盟伺服器之間的通訊安全。 | -根據預設，SSL 憑證會用來做為服務通訊憑證。  但是您也可以選擇將另一個憑證設定為服務通訊憑證。<br>- **重要事項：** 如果您使用 ssl 憑證做為服務通訊憑證，當 SSL 憑證過期時，請務必將更新後的 SSL 憑證設定為服務通訊憑證。 這不會自動發生。<br>-此憑證必須由使用 WCF 訊息安全性 AD FS 的用戶端所信任。<br>-我們建議您使用由公用 (協力廠商) 憑證授權單位單位 (CA) 所發行的伺服器驗證憑證。<br>-服務通訊憑證不可以是使用 CNG 金鑰的憑證。<br>-您可以使用 AD FS 管理主控台來管理此憑證。 |
| **權杖簽署憑證：** 這是標準的 X509 憑證，用來安全地簽署同盟伺服器發出的所有權杖。 | -根據預設，AD FS 會建立具有2048位金鑰的自我簽署憑證。<br>-也支援 CA 發出的憑證，並可使用 AD FS 管理嵌入式管理單元來變更<br>-CA 發行的憑證必須儲存 & 透過 CSP 密碼編譯提供者存取。<br>-權杖簽署憑證不可以是使用 CNG 金鑰的憑證。<br>-AD FS 不需要對外註冊的憑證來簽署權杖。<br> AD FS 會在這些自我簽署憑證到期前自動予以更新，請先將新憑證設定為次要憑證，以允許夥伴取用它們，然後在稱為「自動憑證變換」的進程中翻轉至主要憑證。建議您使用預設的自動產生憑證來簽署權杖。<br> 如果您的組織有需要針對權杖簽署進行不同憑證設定的原則，您可以在安裝期間使用 Powershell 指定憑證 (使用 Install-AdfsFarm Cmdlet) 的– SigningCertificateThumbprint 參數。  安裝之後，您可以使用 AD FS 管理主控台或 Powershell Cmdlet Set-AdfsCertificate 和 >get-adfscertificate，來查看及管理權杖簽署憑證。<br> 當外部註冊的憑證用於權杖簽署時，AD FS 不會執行自動憑證更新或變換。  此程式必須由系統管理員執行。<br> 若要在其中一個憑證即將到期時允許憑證變換，可以在 AD FS 中設定次要權杖簽署憑證。 根據預設，所有權杖簽署憑證都會在同盟中繼資料中發佈，但 AD FS 只會使用主要權杖簽署憑證來實際簽署權杖。 |
| **權杖解密/加密憑證：** 這是標準的 X509 憑證，用來解密/加密任何連入權杖。 它也會在同盟中繼資料中發佈。 | -根據預設，AD FS 會建立具有2048位金鑰的自我簽署憑證。<br>-也支援 CA 發出的憑證，並可使用 AD FS 管理嵌入式管理單元來變更<br>-CA 發行的憑證必須儲存 & 透過 CSP 密碼編譯提供者存取。<br>-權杖解密/加密憑證不能是使用 CNG 金鑰的憑證。<br>-根據預設，AD FS 會產生並使用自己的內部產生和自我簽署憑證來解密權杖。  AD FS 不需要對外註冊的憑證來進行此用途。<br> 此外，AD FS 會在這些自我簽署憑證到期前自動加以更新。<br> **建議您使用預設的自動產生憑證來解密權杖。**<br> 如果您的組織有需要針對權杖解密設定不同憑證的原則，您可以在安裝期間使用 Powershell 指定憑證 (使用 Install-AdfsFarm Cmdlet) 的– DecryptionCertificateThumbprint 參數。  安裝之後，您可以使用 AD FS 管理主控台或 Powershell Cmdlet Set-AdfsCertificate 和 >get-adfscertificate，來查看及管理權杖解密憑證。<br> **當外部註冊的憑證用於權杖解密時，AD FS 不會執行自動憑證更新。 此程式必須由系統管理員執行**。<br>-在本機電腦的個人存放區中，AD FS 服務帳戶必須能夠存取權杖簽署憑證的私密金鑰。 這會由安裝程式負責。 如果您之後變更權杖簽署憑證，也可以使用 AD FS 管理嵌入式管理單元來確保此存取權。 |

> [!CAUTION]
> 權杖簽署和權杖解密/加密所使用的憑證對同盟服務的穩定性而言很重要。 管理自身權杖簽署 & 權杖解密/加密憑證的客戶，應該確保這些憑證會進行備份，並在復原事件期間獨立提供。

> [!NOTE]
> 在 AD FS 您可以將用於數位簽章的安全雜湊演算法 (SHA) 層級變更為 SHA-1 或 SHA-256 (更安全的) 。 AD FS 不支援將憑證與其他雜湊方法搭配使用，例如 MD5 (與 Makecert.exe 命令列工具) 搭配使用的預設雜湊演算法。 基於安全性最佳作法的考量，建議您針對所有簽章使用 SHA-256 (此為預設設定)。 只有在您必須與不支援使用 SHA-256 通訊的產品（例如，非 Microsoft 產品或舊版 AD FS）交互操作的情況下，才建議使用 SHA-1。

> [!NOTE]
> 在您收到來自 CA 的憑證之後，請確定會將所有憑證匯入本機電腦的個人憑證存放區。 您可以使用 [憑證] MMC 嵌入式管理單元，將憑證匯入個人存放區。

## <a name="hardware-requirements"></a><a name="BKMK_2"></a>硬體需求
下列最低和建議的硬體需求適用于 Windows Server 2012 R2 中的 AD FS 同盟伺服器：

|**硬體需求**|**最低需求**|**建議需求**|
|--|--|--|
|CPU 速度|1.4 Ghz 64 位元處理器|四核心，2 GHz|
|RAM|512 MB|4 GB|
|磁碟空間|32 GB|100 GB|

## <a name="software-requirements"></a><a name="BKMK_3"></a>軟體需求

下列 AD FS 需求適用于 Windows Server 2012 R2 作業系統內建的伺服器功能 &reg; ：

- 針對外部網路存取，您必須部署 Web 應用程式 Proxy 角色服務，這是 Windows Server &reg; 2012 R2 遠端存取服務器角色的一部分。 Windows Server 2012 R2 中的 AD FS 不支援舊版的同盟伺服器 proxy &reg; 。

- 同盟伺服器和 Web 應用程式 Proxy 角色服務無法安裝在同一部電腦上。

## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>AD DS 需求

**網域控制站需求**

所有使用者網域中的網域控制站，以及要加入 AD FS 伺服器的網域，都必須執行 Windows Server 2008 或更新版本。

> [!NOTE]
> 具有 Windows Server 2003 網域控制站之環境的所有支援將于 Windows Server 2003 的延伸支援結束日期之後結束。 強烈建議客戶儘快升級其網域控制站。 如需 Microsoft 支援服務生命週期的詳細資訊，請流覽 [此頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) 。 針對 Windows Server 2003 網域控制站環境特有的問題，系統只會針對安全性問題發出修正程式，並在 Windows Server 2003 的延伸支援到期前發出修正。

>[!NOTE]
> AD FS 需要完整的可寫入網域控制站，才能正常運作，而不是 Read-Only 網域控制站。 如果規劃的拓撲包含 Read-Only 網域控制站，則 Read-Only 網域控制站可以用來進行驗證，但 LDAP 宣告處理將需要連接到可寫入的網域控制站。

**網域功能層級需求**

所有使用者帳戶的網域以及 AD FS 伺服器所加入的網域，都必須在 Windows Server 2003 或更新版本的網域功能等級上運作。

大部分的 AD FS 功能不需要進行 AD DS 功能等級修改，就能成功運作。 但是，如果憑證明確對應到 AD DS 中的使用者帳戶，則用戶端憑證驗證需要 Windows Server 2008 網域功能等級或更高等級才能成功運作。

**結構描述需求**

- AD FS 不需要對 AD DS 進行架構變更或功能層級修改。

- 若要使用 Workplace Join 功能，AD FS 伺服器加入之樹系的架構必須設定為 Windows Server 2012 R2。

**服務帳戶需求**

- 任何標準服務帳戶都可用來做為 AD FS 的服務帳戶。 也支援群組受管理的服務帳戶。 這至少需要一個網域控制站 (建議您部署兩個或多個執行 Windows Server 2012 或更高版本的) 。

- 若要讓 Kerberos 驗證在加入網域的用戶端與 AD FS 之間運作，必須在服務帳戶上將 ' HOST/<adfs_service_name> ' 註冊為 SPN。 根據預設，如果有足夠的許可權可執行此作業，AD FS 將會在建立新的 AD FS 伺服器陣列時進行設定。

- 每個使用者網域中都必須信任 AD FS 服務帳戶，而這些使用者網域包含驗證 AD FS 服務的使用者。

**網域需求**

- 所有 AD FS 伺服器都必須加入 AD DS 網域。

- 伺服器陣列中的所有 AD FS 伺服器都必須部署在單一網域中。

- AD FS 伺服器加入的網域必須信任每個使用者帳戶網域，其中包含向 AD FS 服務驗證的使用者。

**多樹系需求**

- AD FS 伺服器所加入的網域，必須信任每個使用者帳戶網域或樹系，其中包含向 AD FS 服務驗證的使用者。

- 每個使用者網域中都必須信任 AD FS 服務帳戶，而這些使用者網域包含驗證 AD FS 服務的使用者。

## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>設定資料庫需求
以下是根據設定存放區類型所適用的需求和限制：

**WID**

- 如果您有100或更少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。

- WID 設定資料庫不支援 SAML 2.0 中的成品解析設定檔。  WID 設定資料庫不支援權杖重新執行偵測。 這項功能僅適用于 AD FS 作為同盟提供者，並使用來自外部宣告提供者之安全性權杖的案例。

- 只要伺服器數目不超過30，就可以在不同的資料中心內部署 AD FS 伺服器以進行容錯移轉或地理負載平衡。

下表提供使用 WID 伺服器陣列的摘要。  您可以使用它來規劃您的執行。

| 1-100 個 RP 信任 | 超過 100 個 RP 信任 |
|--|--|
| **1-30 個 AD FS 節點：** 支援 WID | **1-30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |
| **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |

**SQL Server**

若為 Windows Server 2012 R2 中的 AD FS，您可以使用 SQL Server 2008 和更新版本

## <a name="browser-requirements"></a><a name="BKMK_6"></a>瀏覽器需求
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時，您的瀏覽器必須符合下列需求：

- 必須啟用 JavaScript

- 必須開啟 cookie

- 必須支援伺服器名稱指示 (SNI) 

- 若為使用者憑證 & 裝置憑證驗證 (workplace join 功能) ，則瀏覽器必須支援 SSL 用戶端憑證驗證

有幾個主要的瀏覽器和平臺已通過驗證以進行轉譯和功能，如下所列的詳細資料。 如果瀏覽器和裝置符合上述需求，仍支援此表格所涵蓋的裝置：

| **瀏覽器** | **平台** |
|--|--|
| IE 10。0 | Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2 |
| IE 11。0 | 最為理想、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2 |
| Windows Web 驗證訊息代理程式 | Windows 8.1 |
| Firefox [v21] | Windows 7、Windows 8。1 |
| Safari [v7] | iOS 6，Mac OS-X 10。7 |
| Chrome [v27] | Windows 7、Windows 8.1、Windows Server 2012、Windows Server 2012 R2、Mac OS-X 10。7 |

> [!IMPORTANT]
> 已知問題-Firefox：使用裝置憑證識別裝置的 Workplace Join 功能在 Windows 平臺上無法運作。 Firefox 目前不支援使用布建到 Windows 用戶端上的使用者憑證存放區的憑證來執行 SSL 用戶端憑證驗證。

**餅乾**

AD FS 會建立以工作階段為基礎的永續性 Cookie，這類 Cookie 必須儲存於用戶端電腦上，以提供登入、登出、單一登入 (SSO) 及其他功能。 因此，用戶端瀏覽器必須設定為接受 Cookie。 用來驗證的 Cookie 一律為安全超文字傳輸通訊協定 (HTTPS) 工作階段 Cookie，它們是針對原始伺服器所撰寫的。 如果未將用戶端瀏覽器設定為允許這些 Cookie，AD FS 就無法正確運作。 永續性 Cookie 可用來保留宣告提供者的使用者選項。 您可以在適用於 AD FS 登入頁面的設定檔中使用組態設定來停用它們。 基於安全理由，需要支援 TLS/SSL。

## <a name="extranet-requirements"></a><a name="BKMK_extranet"></a>外部網路需求
若要提供 AD FS 服務的外部網路存取，您必須將 Web 應用程式 Proxy 角色服務部署為對外網路的角色，以安全的方式將驗證要求 Proxy 給 AD FS 服務。 這可隔離 AD FS 服務端點，以及隔離所有安全性金鑰 (例如來自網際網路的要求) 的權杖簽署憑證。 此外，軟外部網路帳戶鎖定等功能都需要使用 Web 應用程式 Proxy。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 [Web 應用程式 proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn584107(v=ws.11))。

如果您想要使用協力廠商 proxy 進行外部網路存取，則協力廠商 proxy 必須支援中定義的通訊協定 [http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf) 。

## <a name="network-requirements"></a><a name="BKMK_7"></a>網路需求
適當地設定下列網路服務，對於在組織中成功部署 AD FS 很重要：

**設定公司防火牆**

位於 Web 應用程式 Proxy 與同盟伺服器陣列之間的防火牆，以及位於用戶端與 Web 應用程式 Proxy 之間的防火牆，都必須啟用 TCP 連接埠 443 的輸入。

此外，如果用戶端使用者憑證驗證 (使用 X509 使用者憑證的 Clienttls 是 authentication) 是必要的，則 Windows Server 2012 R2 中的 AD FS 會要求在用戶端與 Web 應用程式 Proxy 之間的防火牆上，啟用 TCP 埠49443的輸入。 Web 應用程式 Proxy 與同盟伺服器之間的防火牆) 不需要這項功能。

> [!NOTE]
> 此外，請確定 AD FS 與 Web 應用程式 Proxy 伺服器上的任何其他服務都未使用埠49443。

**設定 DNS**

- 針對內部網路存取，存取內部公司網路內 AD FS 服務 (內部網路) 的所有用戶端都必須能夠將 SSL 憑證 (所提供的 AD FS 服務名稱) 名稱解析為 AD FS 伺服器或 AD FS 伺服器的負載平衡器。

- 針對外部網路存取，從公司網路外部存取 AD FS 服務的所有用戶端 (外部網路/網際網路) 必須能夠將 SSL 憑證 (所提供的 AD FS 服務名稱) 名稱解析為 Web 應用程式 Proxy 伺服器或 Web 應用程式 Proxy 伺服器的負載平衡器。

- 若要讓外部網路存取正常運作，DMZ 中的每個 Web 應用程式 Proxy 伺服器都必須能夠解析 SSL 憑證) 的 AD FS 服務名稱 (名稱，至 AD FS 伺服器或 AD FS 伺服器的負載平衡器。 您可以使用 DMZ 網路中的其他 DNS 伺服器，或使用 HOSTS 檔案變更本機伺服器解析來達成此目的。

- 若要讓 Windows 整合式驗證在網路內，以及透過 Web 應用程式 Proxy 公開之端點子集的網路之外運作，您必須使用 A 記錄 (非 CNAME) 來指向負載平衡器。

如需針對 federation service 和裝置註冊服務設定公司 DNS 的相關資訊，請參閱 [設定同盟服務和 DRS 的公司 dns](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486786(v=ws.11))。

如需有關為 Web 應用程式 Proxy 設定公司 DNS 的詳細資訊，請參閱 [步驟1：設定 Web 應用程式 Proxy 基礎結構](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))中的「設定 DNS」一節。

如需如何使用 NLB 設定叢集 IP 位址或叢集 FQDN 的詳細資訊，請參閱在中指定叢集參數 [http://go.microsoft.com/fwlink/?LinkId=75282](https://go.microsoft.com/fwlink/?LinkId=75282) 。

## <a name="attribute-store-requirements"></a><a name="BKMK_8"></a>屬性存放區需求
AD FS 至少需要一個屬性存放區來驗證使用者，並為這些使用者解壓縮安全性宣告。 如需 AD FS 支援的屬性存放區清單，請參閱 [屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。

> [!NOTE]
> AD FS 預設會自動建立 "Active Directory" 屬性存放區。 屬性存放區需求需視您的組織是帳戶夥伴 (主控同盟使用者) 或資源夥伴 (主控同盟應用程式) 而定。

**LDAP 屬性存放區**

當您使用其他以輕量型目錄存取通訊協定 (LDAP) 為基礎的屬性存放區時，必須連線到支援 Windows 整合式驗證的 LDAP 伺服器。 LDAP 連線字串也必須以 LDAP URL 的格式來書寫，如 RFC 2255 中所述。

AD FS 服務的服務帳戶也必須具有在 LDAP 屬性存放區中取得使用者資訊的許可權。

**SQL Server 屬性存放區**

若要讓 Windows Server 2012 R2 中的 AD FS 順利運作，裝載 SQL Server 屬性存放區的電腦必須執行 Microsoft SQL Server 2008 或更新版本。 當您使用以 SQL 為基礎的屬性存放區時，也必須設定連線字串。

**自訂屬性存放區**

您可以開發自訂屬性存放區，以啟用進階案例。

- AD FS 內建的原則語言可以參考自訂屬性存放區，因此能夠增強下列任一個案例：

    - 建立適用於本機驗證使用者的宣告

    - 補充適用於外部驗證使用者的宣告

    - 授權使用者取得權杖

    - 授權服務代表使用者取得權杖

    - 在 AD FS 簽發給信賴憑證者的安全性權杖中發出額外的資料。

- 所有自訂屬性存放區都必須以 .NET 4.0 或更新版本為基礎。

當您使用自訂屬性存放區時，您可能也必須設定連接字串。 在此情況下，您可以輸入您選擇的自訂程式碼，以啟用自訂屬性存放區的連接。 在此情況下，連接字串是一組成對的名稱/值組，其會由自訂屬性存放區的開發人員所執行。如需開發和使用自訂屬性存放區的詳細資訊，請參閱 [屬性存放區總覽](https://go.microsoft.com/fwlink/?LinkId=190782)。

## <a name="application-requirements"></a><a name="BKMK_9"></a>應用程式需求
AD FS 支援宣告感知應用程式，這些應用程式使用下列通訊協定：

- WS-同盟

- WS-Trust

- 使用 IDPLite、SPLite & eGov 1.5 設定檔的 SAML 2.0 通訊協定。

- OAuth 2.0 授權授與設定檔

AD FS 也支援 Web 應用程式 Proxy 所支援之任何非宣告感知應用程式的驗證和授權。

## <a name="authentication-requirements"></a><a name="BKMK_10"></a>驗證需求
**AD DS 驗證 (主要驗證)**

針對內部網路存取，支援下列 AD DS 的標準驗證機制：

- 使用 Negotiate & NTLM 的 Windows 整合式驗證

- 使用使用者名稱/密碼的表單驗證

- 在 AD DS 中使用對應至使用者帳戶的憑證進行憑證驗證

針對外部網路存取，支援下列驗證機制：

- 使用使用者名稱/密碼的表單驗證

- 在 AD DS 中使用對應至使用者帳戶的憑證驗證憑證

- 使用 Negotiate 的 Windows 整合式驗證 (僅限 NTLM) 適用于接受 Windows 整合式驗證的 WS-Trust 端點。

針對憑證驗證：

- 擴充到可以釘選保護的智慧卡。

- AD FS 不會提供使用者輸入其 pin 的 GUI，而且必須是使用用戶端 TLS 時所顯示之用戶端作業系統的一部分。

- 智慧卡的讀取器和密碼編譯服務提供者 (CSP) 必須在瀏覽器所在的電腦上運作。

- 智慧卡憑證必須在所有的 AD FS 伺服器和 Web 應用程式 Proxy 伺服器上，與受信任的根目錄鏈。

- 憑證必須使用下列任一個方法，對應到 AD DS 中的使用者帳戶：

    - 憑證主體名稱會對應到 AD DS 中使用者帳戶的 LDAP 辨別名稱。

    - 憑證主體 altname 延伸含有 AD DS 中使用者帳戶的使用者主體名稱 (UPN)。

若要在內部網路使用 Kerberos 進行無縫的 Windows 整合式驗證，

- 服務名稱必須是信任的網站或近端內部網路網站的一部分。

- 此外，您必須在 AD FS 伺服器陣列執行所在的服務帳戶上設定主機/<adfs_service_name> SPN。

**Multi-Factor Authentication**

AD FS 支援 AD DS) 所支援的主要驗證以外的其他驗證 (使用提供者模型，讓廠商/客戶可以建立自己的多重要素驗證介面卡，讓系統管理員可以在登入期間註冊和使用。

每個 MFA 介面卡都必須以 .NET 4.5 為基礎。

如需 MFA 的詳細資訊，請參閱 [使用機密應用程式的額外 Multi-Factor Authentication 管理風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

**裝置驗證**

AD FS 支援使用裝置註冊服務所布建的憑證來進行裝置驗證。

## <a name="workplace-join-requirements"></a><a name="BKMK_11"></a>Workplace join 需求
終端使用者可以使用 AD FS，將其裝置加入組織。 AD FS 中的裝置註冊服務可支援此功能。 如此一來，使用者就能在 AD FS 所支援的應用程式中取得 SSO 的額外優點。 此外，系統管理員也可以限制只有已加入工作場所的裝置才能存取應用程式，以管理風險。 以下是啟用此案例的需求。

- AD FS 支援 Windows 8.1 和 iOS 5 + 裝置的 workplace join

- 若要使用 Workplace Join 功能，AD FS 伺服器加入之樹系的架構必須是 Windows Server 2012 R2。

- AD FS 服務之 SSL 憑證的主體別名必須包含值 enterpriseregistration，後面接著您組織的使用者主體名稱 (UPN) 尾碼，例如 enterpriseregistration.corp.contoso.com。

## <a name="cryptography-requirements"></a><a name="BKMK_12"></a>密碼編譯需求
下表提供 AD FS 權杖簽署、權杖加密/解密功能的其他加密支援資訊：

|**演算法**|**金鑰長度**|**通訊協定/應用程式/批註**|
|--|--|--|
|TripleDES –預設 192 (支援的192– 256) - [http://www.w3.org/2001/04/xmlenc#tripledes-cbc](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>= 192|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|
|AES128 [http://www.w3.org/2001/04/xmlenc#aes128-cbc](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|
|AES192 [http://www.w3.org/2001/04/xmlenc#aes192-cbc](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|
|AES256 [http://www.w3.org/2001/04/xmlenc#aes256-cbc](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**Default**。 支援加密安全性權杖的演算法。|
|TripleDESKeyWrap [http://www.w3.org/2001/04/xmlenc#kw-tripledes](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|.NET 4.0 + 支援的所有金鑰大小|加密加密安全性權杖的對稱金鑰所支援的演算法。|
|AES128KeyWrap [http://www.w3.org/2001/04/xmlenc#kw-aes128](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|加密加密安全性權杖的對稱金鑰所支援的演算法。|
|AES192KeyWrap [http://www.w3.org/2001/04/xmlenc#kw-aes192](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|加密加密安全性權杖的對稱金鑰所支援的演算法。|
|AES256KeyWrap [http://www.w3.org/2001/04/xmlenc#kw-aes256](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|加密加密安全性權杖的對稱金鑰所支援的演算法。|
|RsaV15KeyWrap - [http://www.w3.org/2001/04/xmlenc#rsa-1_5](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|加密加密安全性權杖的對稱金鑰所支援的演算法。|
|RsaOaepKeyWrap [http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|預設值： 加密加密安全性權杖的對稱金鑰所支援的演算法。|
|SHA1[http://www.w3.org/PICS/DSig/SHA1_1_0.html](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N/A|由 AD FS Server 在構件 SourceId 產生中使用：在此案例中，STS 會根據 SAML 2.0 標準) 中的建議使用 SHA1 (，為成品 sourceiD 建立一個較短的160位值。<p>ADFS web 代理程式也會使用 WS2003 時間範圍內 (舊版元件) 來識別「上次更新」時間值的變更，讓它知道何時要從 STS 更新資訊。|
|SHA1withRSA<p>[http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N/A|當 AD FS 伺服器驗證 SAML AuthenticationRequest 的簽章、簽署構件解析要求或回應、建立權杖簽署憑證時使用。<p>在這些情況下，SHA256 是預設值，只有在夥伴 (信賴憑證者) 無法支援 SHA256 且必須使用 SHA1 時，才會使用 SHA1。|

## <a name="permissions-requirements"></a><a name="BKMK_13"></a>權限需求
執行安裝和初始設定 AD FS 的系統管理員必須在本機 (網域中具有網域系統管理員許可權，也就是同盟伺服器加入的網域。 ) 

## <a name="see-also"></a>另請參閱
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
