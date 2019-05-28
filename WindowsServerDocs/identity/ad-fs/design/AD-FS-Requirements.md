---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: 使用 SQL Server 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6aa91956f4a90b32b82e6c970e68b3164c732f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191719"
---
# <a name="ad-fs-requirements"></a>AD FS 需求

您必須符合部署 AD FS 時的各種需求如下：  
  
-   [憑證需求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬體需求](AD-FS-Requirements.md#BKMK_2)  
  
-   [軟體需求](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS 需求](AD-FS-Requirements.md#BKMK_4)  
  
-   [設定資料庫需求](AD-FS-Requirements.md#BKMK_5)  
  
-   [瀏覽器需求](AD-FS-Requirements.md#BKMK_6)  
  
-   [外部網路的需求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [網路需求](AD-FS-Requirements.md#BKMK_7)  
  
-   [屬性存放區需求](AD-FS-Requirements.md#BKMK_8)  
  
-   [應用程式的需求](AD-FS-Requirements.md#BKMK_9)  
  
-   [驗證需求](AD-FS-Requirements.md#BKMK_10)  
  
-   [工作場所聯結需求](AD-FS-Requirements.md#BKMK_11)  
  
-   [密碼編譯需求](AD-FS-Requirements.md#BKMK_12)  
  
-   [權限需求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>憑證需求  
憑證扮演最重要的角色，在保護同盟之間的通訊伺服器、 Web 應用程式 Proxy、 宣告\-感知的應用程式和 Web 用戶端。 憑證的需求而有所不同，您會設定為同盟伺服器或 proxy 電腦，這一節所述。  
  
**同盟伺服器憑證**  
  
|||  
|-|-|  
|**憑證類型**|**需求、 支援和應該知道的事項**|  
|**安全通訊端層\(SSL\)憑證：** 這是標準的 SSL 憑證，用來保護同盟伺服器和用戶端之間的通訊。|-此憑證必須是公開信任\*X509 v3 憑證。<br />-所有存取任何 AD FS 端點的用戶端必須信任此憑證。 強烈建議使用公用所發出的憑證\(第三個\-合作對象\)憑證授權單位\(CA\)。 您可以使用自我\-簽署 SSL 憑證已成功在同盟伺服器在測試實驗室環境中的。 不過，針對生產環境中，我們建議您從公用 CA 取得憑證。<br />-支援 Windows Server 2012 R2 支援的 SSL 憑證的任何金鑰大小。<br />-不支援使用 CNG 金鑰的憑證。<br />-當搭配 Workplace Join\/Device Registration Service，主體替代名稱的 SSL 憑證變更為 AD FS 服務必須包含後面接著使用者主體名稱值enterpriseregistration\(UPN\)組織，例如，enterpriseregistration.contoso.com 的尾碼。<br />的支援萬用字元憑證。 當您建立您的 AD FS 伺服器陣列時，您將會提示您提供 AD FS 服務的服務名稱\(，例如**adfs.contoso.com**。<br />-它是強烈建議針對 Web 應用程式 Proxy 中使用相同的 SSL 憑證。 這不過是**必要**必須相同，支援 Windows 整合式驗證端點透過 Web Application Proxy 及延伸保護驗證開啟時\(的預設設定\).<br />的此憑證主體名稱用來代表每個執行個體，您將部署的 AD fs 的 Federation Service 名稱。 基於這個理由，您可能要考慮選擇在任何新的 CA 上的主體名稱\-已發行的憑證最能代表貴公司或組織到夥伴的名稱。<br />    憑證的身分識別必須符合 federation service 名稱\(例如，fs.contoso.com\)。身分識別不是主體別名延伸模組的 dNSName 類型，或作為一般的名稱沒有主旨替代名稱項目，如果指定的主體名稱。 多個主體別名項目中可以存在的憑證，前提是其中一個符合 federation service 名稱。<br />-   **重要事項：** 強烈建議在您的 AD FS 伺服器陣列的所有節點以及您的 AD FS 伺服器陣列中的所有 Web 應用程式 proxy 中使用相同的 SSL 憑證。|  
|**服務通訊憑證：** 這個憑證會啟用 WCF 訊息安全性，來保護同盟伺服器之間的通訊。|-根據預設，SSL 憑證可做為服務通訊憑證。  但是，您也可以選擇另一個憑證設定為服務通訊憑證。<br />-   **重要事項：** 如果您使用的 SSL 憑證做為服務通訊憑證，在 SSL 憑證過期時，請務必將更新的 SSL 憑證設定為您的服務通訊憑證。 這不會不會自動發生。<br />-使用 WCF 訊息安全性的 AD FS 用戶端必須信任此憑證。<br />-我們建議您先使用伺服器驗證憑證所發出的公用\(第三個\-合作對象\)憑證授權單位\(CA\)。<br />-服務通訊憑證不能使用 CNG 金鑰的憑證。<br />-此憑證可以使用 AD FS 管理主控台來管理。|  
|**語彙基元\-簽署憑證：** 這是標準的 X509 憑證，可以用來安全地簽署同盟伺服器簽發的所有權杖。|-根據預設，AD FS 會建立自我\-以 2048 位元金鑰簽署的憑證。<br />-發行的 CA 憑證也支援，而且可以使用 AD FS 管理嵌入式管理單元變更\-中<br />-CA 發行的憑證必須儲存及透過 CSP 密碼編譯提供者存取。<br />-權杖簽署憑證不能使用 CNG 金鑰的憑證。<br />AD FS 不需要外部註冊的憑證來簽署權杖。<br />    AD FS 會自動更新這些自我\-簽署憑證在到期前，先設定新的憑證做為次要憑證，以便讓合作夥伴可以使用它們，然後翻轉為主要程序稱為自動憑證變換。我們建議您使用預設值，自動產生的憑證來簽署權杖。<br />    如果您的組織有需要設定不同的憑證來簽署權杖的原則，您可以在安裝期間使用 Powershell 來指定憑證\(使用安裝的 – SigningCertificateThumbprint 參數\-AdfsFarm cmdlet\)。  安裝完成後，您可以檢視和管理使用 AD FS 管理主控台或 Powershell cmdlet 設定的權杖簽署憑證\-AdfsCertificate 並取得\-AdfsCertificate。<br />    當外部註冊的憑證用於權杖簽署時，AD FS 不會執行自動憑證更新或進行變換。  此程序必須由系統管理員身分執行。<br />    若要一個憑證即將到期時，允許進行憑證變換，可以在 AD FS 中設定的次要權杖簽署憑證。 根據預設，所有的權杖簽署憑證都會發佈同盟中繼資料，但只有主要權杖\-可到由 AD FS 來實際簽署權杖簽署憑證。|  
|**語彙基元\-解密\/加密憑證：** 這是標準 X509 憑證，用來解密\/加密任何連入權杖。 它也會在同盟中繼資料中發佈。|-根據預設，AD FS 會建立自我\-以 2048 位元金鑰簽署的憑證。<br />-發行的 CA 憑證也支援，而且可以使用 AD FS 管理嵌入式管理單元變更\-中<br />-CA 發行的憑證必須儲存及透過 CSP 密碼編譯提供者存取。<br />-T\-解密\/加密憑證不能使用 CNG 金鑰的憑證。<br />-根據預設，AD FS 會產生，並使用它自己，在內部產生和自身\-簽署的權杖解密憑證。  針對此目的，AD FS 不需要外部註冊的憑證。<br />    此外，AD FS 會自動更新這些自我\-簽署憑證在到期前。<br />    **我們建議您使用預設值，自動產生權杖解密憑證。**<br />    如果您的組織有需要設定的不同憑證來進行權杖解密的原則，您可以在安裝期間使用 Powershell 來指定憑證\(使用 – DecryptionCertificateThumbprint 參數安裝\-AdfsFarm cmdlet\)。  安裝完成後，您可以檢視和管理使用 AD FS 管理主控台或 Powershell cmdlet 設定的權杖解密憑證\-AdfsCertificate 並取得\-AdfsCertificate。<br />    **AD FS 權杖解密使用外部註冊的憑證，不會執行自動更新憑證。此程序必須由系統管理員執行**。<br />-AD FS 服務帳戶必須具有存取此語彙基元\-簽署憑證的私密金鑰，本機電腦的個人存放區中。 這是由負責安裝程式。 您也可以使用 AD FS 管理嵌入式管理單元\-才可確保此存取權，如果您接著變更權杖\-簽署憑證。|  
  
> [!CAUTION]  
> 憑證用於語彙基元\-簽署和權杖\-解密\/加密至關重要的 Federation Service 的穩定性。 客戶管理自己的語彙基元\-簽署和權杖\-解密\/加密憑證，應該確定這些憑證備份，並可獨立復原事件期間。  
  
> [!NOTE]  
> 在 AD FS 中，您可以變更安全雜湊演算法\(SHA\)使用於數位簽章來任一 SHA 的層級\-1 或 SHA\-256\(更安全\)。 AD FS 不支援使用憑證與其他雜湊方法，例如 MD5 \(Makecert.exe 命令搭配使用的預設雜湊演算法\-線條 工具\)。 最佳安全性做法，我們建議您改用 SHA\-256\(根據預設，這會設定\)針對所有簽章。 SHA\-1 建議只有在案例中您必須與交互操作的產品，不支援使用 SHA 通訊\-256，例如非\-的 AD FS 的 Microsoft 產品或舊版版本。  
  
> [!NOTE]  
> 在您收到來自 CA 的憑證之後，請確定會將所有憑證匯入本機電腦的個人憑證存放區。 您可以將憑證匯入到個人存放區，使用憑證 MMC 嵌入式管理單元\-中。  
  
## <a name="BKMK_2"></a>硬體需求  
下列的最低和建議硬體需求都適用於 Windows Server 2012 R2 中 AD FS 同盟伺服器：  
  
||||  
|-|-|-|  
|**硬體需求**|**最低需求**|**建議的需求**|  
|CPU 速度|1.4 GHz 64\-位元處理器|四\-核心，2 GHz|  
|RAM|512 MB|4 GB|  
|磁碟空間|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>軟體需求  
下列 AD FS 需求適用於 Windows Server® 2012 R2 作業系統內建的伺服器功能：  
  
-   外部網路存取，您必須部署 Web 應用程式 Proxy 角色服務\-的 Windows Server® 2012 R2 的遠端存取伺服器角色的一部分。 Windows Server® 2012 R2 中 AD FS 不支援舊版的同盟伺服器 proxy。  
  
-   無法在相同電腦上安裝同盟伺服器和 Web 應用程式 Proxy 角色服務。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站的需求**  
  
在所有的使用者網域和 AD FS 伺服器所加入之網域的網域控制站必須執行 Windows Server 2008 或更新版本。  
  
> [!NOTE]  
> 所有與 Windows Server 2003 網域控制站的環境支援將結束之後延伸支援結束日期的 Windows Server 2003。 若要儘速升級網域控制站，強烈建議客戶。 請瀏覽[本頁](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)如需 Microsoft 支援週期的其他資訊。 探索到的問題，是 Windows Server 2003 的網域控制站環境所特有，修正將會只針對發出安全性問題，並且如果可以擴充支援適用於 Windows Server 2003 在到期前發出修正。  
  
**網域功能\-層級需求**  
  
所有的使用者帳戶網域和 AD FS 伺服器所加入的網域必須在 Windows Server 2003 或更高版本的網域功能等級進行操作。  
  
大部分的 AD FS 功能不需要 AD DS 功能\-層級修改，才能成功運作。 但是，如果憑證明確對應到 AD DS 中的使用者帳戶，則用戶端憑證驗證需要 Windows Server 2008 網域功能等級或更高等級才能成功運作。  
  
**結構描述的需求**  
  
-   AD FS 不需要結構描述變更或功能\-層級修改至 AD DS。  
  
-   若要使用工作地點加入的功能，已加入 AD FS 伺服器的樹系的架構必須設定到 Windows Server 2012 R2。  
  
**服務帳戶需求**  
  
-   適用於 AD FS，可以做為服務帳戶使用任何標準的服務帳戶。 也支援群組受管理的服務帳戶。 這需要至少一個網域控制站\(建議您部署兩個或多個\)執行 Windows Server 2012 或更新版本。  
  
-   為了讓 Kerberos 驗證網域之間的函式\-加入用戶端與 AD FS 中，' 主機\/< adfs\_服務\_名稱 >' 必須註冊為服務帳戶的 SPN。 根據預設，AD FS 會設定此建立新的 AD FS 伺服器陣列，如果有足夠的權限，才能執行此作業時。  
  
-   每個包含使用者向 AD FS 服務的使用者網域必須信任的 AD FS 服務帳戶。  
  
**網域需求**  
  
-   所有 AD FS 伺服器必須都是 AD DS 網域加入。  
  
-   在伺服陣列中的所有 AD FS 伺服器必須都部署在單一網域。  
  
-   已加入 AD FS 伺服器的網域必須信任每個使用者帳戶網域包含 AD FS 服務進行驗證的使用者。  
  
**多樹系需求**  
  
-   每個使用者帳戶網域或樹系包含 AD FS 服務進行驗證的使用者都必須信任的 AD FS 伺服器已加入網域。  
  
-   每個包含使用者向 AD FS 服務的使用者網域必須信任的 AD FS 服務帳戶。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
需求如下，並套用的限制為基礎的組態存放區類型：  
  
**WID**  
  
-   WID 伺服器陣列已限制為 30 的同盟伺服器，如果您有 100 或更少信賴憑證者信任。  
  
-   WID 組態資料庫中不支援 SAML 2.0 中的成品解析設定檔。  WID 組態資料庫中不支援權杖重新執行偵測。 這項功能僅只用於 AD FS 是做為同盟提供者並取用來自外部的宣告提供者的安全性權杖。  
  
-   地理負載平衡支援，只要伺服器數目不超過 30 或部署 AD FS 伺服器，在不同的資料中心容錯移轉。  
  
下表提供摘要使用 WID 伺服器陣列。  您可以使用它來協助您規劃您的實作。  
  
||||  
|-|-|-|  
||1 \- 100 的 RP 信任|100 個以上的 RP 信任|  
|1 \- 30 AD FS 節點|WID 支援|不支援使用 WID\-所需的 SQL|  
|30 多個 AD FS 節點|不支援使用 WID\-所需的 SQL|不支援使用 WID\-所需的 SQL|  
  
**SQL Server**  
  
適用於 Windows Server 2012 R2 中 AD FS，您可以使用 SQL Server 2008 及更新版本  
  
## <a name="BKMK_6"></a>瀏覽器需求  
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時，您的瀏覽器必須符合下列需求：  
  
-   必須啟用 JavaScript  
  
-   必須開啟 cookie  
  
-   伺服器名稱指示\(SNI\)必須支援  
  
-   使用者憑證和裝置憑證驗證\(workplace join 功能\)，瀏覽器必須支援 SSL 用戶端憑證驗證  
  
數個索引鍵的瀏覽器與平台經過轉譯和功能的詳細資料如下的驗證。 如果它們符合上述需求，仍會支援瀏覽器與本表中未涵蓋的裝置：  
  
|||  
|-|-|  
|**瀏覽器**|**平台**|  
|IE 10.0|Windows 7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、windows Server 2012 R2|  
|IE 11.0|Windows7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|Windows Web 驗證代理人|Windows 8.1|  
|Firefox \[v21\]|Windows 7，Windows 8.1|  
|Safari \[v7\]|iOS 6、 Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7、 Windows 8.1、 Windows Server 2012，Windows Server 2012 R2 中，Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> 已知問題\-Firefox:找不到在 Windows 平台上運作的工作場所聯結功能可識別使用裝置憑證的裝置。 Firefox 目前不支援使用憑證佈建到使用者憑證存放區，在 Windows 用戶端上執行的 SSL 用戶端憑證驗證。  
  
**Cookie**  
  
AD FS 會建立工作階段\-根據和持續性 cookie 必須儲存在用戶端電腦，以提供登\-中，登入\-時，單一登\-上\(SSO\)，和其他功能。 因此，用戶端瀏覽器必須設定為接受 Cookie。 用於驗證的 cookie 一律是安全超文字傳輸通訊協定\(HTTPS\)針對原始伺服器所撰寫的工作階段 cookie。 如果未將用戶端瀏覽器設定為允許這些 Cookie，AD FS 就無法正確運作。 永續性 Cookie 可用來保留宣告提供者的使用者選項。 您可以使用 AD FS 正負號的組態檔中的組態設定停用它們\-頁面中。 支援 TLS\/都基於安全性理由需要 SSL。  
  
## <a name="BKMK_extranet"></a>外部網路的需求  
若要提供外部網路存取的 AD FS 服務，您必須部署 Web 應用程式 Proxy 角色服務做為外部網路對向角色，proxy 將驗證要求以安全的方式，AD FS 服務。 這會提供隔離的 AD FS 服務端點，以及所有的安全性金鑰的隔離\(例如 權杖簽署憑證\)與源自網際網路的要求。 此外，例如軟性的外部網路帳戶鎖定的功能都需要使用的 Web 應用程式 Proxy。 如需有關 Web 應用程式 Proxy 的詳細資訊，請參閱 < [Web 應用程式 Proxy](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果您想要使用第三\-此第三方的外部網路存取的 proxy\-協力廠商 proxy 必須支援中定義的通訊協定[http:\/\/download.microsoft.com\/下載\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bms\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>網路需求  
適當地設定下列網路服務最重要的是成功部署組織中的 AD FS:  
  
**設定公司防火牆**  
  
這兩個位於 Web 應用程式 Proxy 與同盟伺服器陣列和用戶端和 Web 應用程式 Proxy 之間的防火牆之間的防火牆必須有 TCP 連接埠 443 已啟用輸入。  
  
此外，如果用戶端使用者憑證驗證\(clientTLS 驗證使用 X509 使用者憑證\)是有需要，Windows Server 2012 R2 中的 AD FS 需要啟用 TCP 連接埠 49443 輸入之間的防火牆用戶端和 Web 應用程式 Proxy。 這不需要在 Web 應用程式 Proxy 與同盟伺服器之間的防火牆上\)。  
  
**設定 DNS**  
  
-   內部網路存取，所有的用戶端存取 AD FS 服務內部的公司網路內\(內部網路\)必須要能夠解析的 AD FS 服務名稱\(提供的 SSL 憑證名稱\)負載AD FS 伺服器或 AD FS 伺服器的平衡器。  
  
-   外部網路存取，所有的用戶端存取 AD FS 服務從公司網路外部\(外部網路\/網際網路\)必須能夠解析 AD FS 服務名稱\(SSL 憑證所提供的名稱\) Web 應用程式 Proxy 伺服器或 Web Application Proxy 伺服器的負載平衡器。  
  
-   外部網路存取，才能正確運作，在 DMZ 中的每個 Web Application Proxy 伺服器必須能夠解析 AD FS 服務名稱\(提供的 SSL 憑證名稱\)AD FS 伺服器或 AD FS 伺服器的負載平衡器。 這可以使用替代的 DNS 伺服器，在 DMZ 網路或變更使用主機檔案的本機伺服器解析來達成。  
  
-   Windows 整合式驗證，能夠運作的網路內部和外部網路上的 Web Application Proxy 透過公開的端點的子集，您必須使用 A 記錄\(不是 CNAME\)指向負載平衡器。  
  
如需設定公司 DNS federation service 及裝置註冊服務的資訊，請參閱[設定的 Federation Service 和 DRS 的公司 DNS](https://technet.microsoft.com/library/dn486786.aspx)。  
  
設定公司 DNS，Web 應用程式 proxy 的資訊，請參閱中的 「 設定 DNS 」 一節[步驟 1:設定 Web Application Proxy 基礎結構](https://technet.microsoft.com/library/dn383644.aspx)。  
  
如如何設定叢集 IP 位址或叢集 FQDN 使用 NLB 的詳細資訊，請參閱 < 指定叢集參數，在[http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>屬性存放區需求  
AD FS 需要至少一個要用於驗證使用者，並擷取這些使用者的安全性宣告的屬性存放區。 如需屬性清單會儲存在 AD FS 支援，請參閱 <<c0> [ 角色的屬性儲存](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> AD FS 會自動建立預設的 [Active Directory] 屬性存放區。 屬性存放區需求取決於您的組織是否做為帳戶夥伴\(主控同盟的使用者\)或資源夥伴\(裝載同盟應用程式\)。  
  
**LDAP 屬性存放區**  
  
當您使用其他輕量型目錄存取通訊協定\(LDAP\)\-基礎屬性存放區中，您必須連線到 LDAP 伺服器，支援 Windows 整合式驗證。 LDAP 連線字串也必須以 LDAP URL 的格式來書寫，如 RFC 2255 中所述。  
  
它也是所需的 AD FS 服務的服務帳戶具有擷取 LDAP 屬性存放區中的使用者資訊的權限。  
  
**SQL Server 屬性存放區**  
  
適用於 AD FS 順利運作的 Windows Server 2012 R2 中，裝載 SQL Server 屬性存放區的電腦必須執行 Microsoft SQL Server 2008 或更新版本。 當您使用 SQL\-基礎屬性存放區中，您也必須設定連接字串。  
  
**自訂屬性存放區**  
  
您可以開發自訂屬性存放區，以啟用進階案例。  
  
-   AD FS 內建的原則語言可以參考自訂屬性存放區，因此能夠增強下列任一個案例：  
  
    -   建立適用於本機驗證使用者的宣告  
  
    -   補充適用於外部驗證使用者的宣告  
  
    -   授權使用者取得權杖  
  
    -   授權服務代表使用者取得權杖  
  
    -   發出給信賴憑證者的合作對象的 AD FS 所發出的安全性權杖中的其他資料。  
  
-   在.NET 4.0 之上或更高版本，必須先建置所有自訂屬性存放區。  
  
當您使用的自訂屬性存放區時，您可能也必須設定連接字串。 在此情況下，您可以輸入自訂的程式碼，可讓您的自訂屬性存放區的連接您選擇。 在此情況下的連接字串是一份名稱\/值解譯為自訂屬性存放區的開發人員所實作的組。如需有關開發和使用自訂屬性存放區的詳細資訊，請參閱 <<c2> [ 屬性存放區概觀](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="BKMK_9"></a>應用程式的需求  
AD FS 支援宣告\-感知的應用程式，使用下列通訊協定：  
  
-   WS\-同盟  
  
-   WS\-信任  
  
-   SAML 2.0 通訊協定使用 IDPLite，SPLite & eGov1.5 的設定檔。  
  
-   OAuth 2.0 授權授與設定檔  
  
AD FS 也支援驗證和授權的任何非\-宣告\-感知的應用程式所支援的 Web 應用程式 Proxy。  
  
## <a name="BKMK_10"></a>驗證需求  
**AD DS 驗證\(主要驗證\)**  
  
內部網路存取，支援下列標準驗證機制適用於 AD DS:  
  
-   使用交涉的 Kerberos 與 NTLM 的 Windows 整合式驗證  
  
-   表單驗證使用的使用者名稱\/密碼  
  
-   使用對應至 AD DS 中的使用者帳戶的憑證的憑證驗證  
  
外部網路存取，支援下列的驗證機制：  
  
-   表單驗證使用的使用者名稱\/密碼  
  
-   使用會對應到 AD DS 中的使用者帳戶的憑證的憑證驗證  
  
-   Windows 整合式驗證使用 Negotiate \(NTLM 僅\)ws\-信任接受 Windows 整合式驗證的端點。  
  
憑證驗證：  
  
-   會擴充為可為受保護的 pin 的智慧卡。  
  
-   使用者輸入 pin 的 GUI 未提供 AD fs，而且必須使用用戶端 TLS 時，會顯示用戶端作業系統的一部分。  
  
-   讀取器和密碼編譯服務提供者\(CSP\)智慧卡必須在瀏覽器位置的電腦上運作。  
  
-   智慧卡憑證必須鏈結到信任的根上所有的 AD FS 伺服器和 Web 應用程式 Proxy 伺服器。  
  
-   憑證必須對應到使用者帳戶在 AD DS 中的其中一種下列方法：  
  
    -   憑證主體名稱對應至 AD DS 中的使用者帳戶的 LDAP 辨別名稱。  
  
    -   憑證主體 altname 延伸含有使用者主體名稱\(UPN\)的 AD DS 中的使用者帳戶。  
  
無縫式的 Windows 整合式驗證，請在內部網路中使用 Kerberos  
  
-   需要服務名稱是受信任的網站] 或 [近端內部網路網站的一部分。  
  
-   此外，主機\/< adfs\_服務\_名稱 > 必須在 AD FS 伺服器陣列執行的服務帳戶設定 SPN。  
  
**多重\-因素驗證**  
  
AD FS 支援額外的驗證\(支援 AD ds 的主要驗證超出\)藉此讓廠商使用提供者模型\/客戶可以建置自己的多個\-因素驗證配接器以系統管理員可以註冊及登入期間使用。  
  
每個 MFA 配接器必須建置在.NET 4.5 之上。  
  
如需有關 MFA 的詳細資訊，請參閱 < [Manage Risk with Additional Multi-factor Authentication 為機密的應用程式](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**裝置驗證**  
  
AD FS 支援使用終端使用者工作地點加入其裝置的動作期間佈建的裝置註冊服務的憑證進行裝置驗證。  
  
## <a name="BKMK_11"></a>工作場所聯結需求  
使用者可以加入工作地點使用 AD FS 的組織其裝置。 這被支援裝置註冊服務在 AD FS。 如此一來，使用者會取得 SSO 的 AD FS 所支援的應用程式間的額外好處。 此外，系統管理員可以藉由限制應用程式只到已加入至組織的工作場所的裝置存取管理風險。 以下是啟用此案例中的下列需求。  
  
-   AD FS 支援加入工作地點，適用於 Windows 8.1 和 iOS 5\+裝置  
  
-   若要使用工作地點加入的功能，已加入 AD FS 伺服器的樹系的架構必須是 Windows Server 2012 R2。  
  
-   主體替代名稱的 SSL 憑證的 AD FS 服務必須包含後面接著使用者主體名稱值 enterpriseregistration \(UPN\)組織的尾碼，比方說，enterpriseregistration.corp.contoso.com。  
  
## <a name="BKMK_12"></a>密碼編譯需求  
下表提供 AD FS 權杖簽署、 權杖加密其他密碼編譯支援資訊\/解密功能：  
  
||||  
|-|-|-|  
|**演算法**|**金鑰長度**|**通訊協定\/應用程式\/註解**|  
|TripleDES – 預設 192\(支援 192 – 256\) \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|支援的演算法解密安全性權杖。 不支援加密的安全性權杖，使用此演算法。|  
|AES128 \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#aes128\-cbc|128|支援的演算法解密安全性權杖。 不支援加密的安全性權杖，使用此演算法。|  
|AES192 \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#aes192\-cbc|192|支援的演算法解密安全性權杖。 不支援加密的安全性權杖，使用此演算法。|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**預設值**。 支援加密的安全性權杖的演算法。|  
|TripleDESKeyWrap \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#千瓦\-tripledes|支援的.NET 4.0 的所有索引鍵大小\+|支援演算法來加密對稱金鑰來加密安全性權杖。|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#千瓦\-aes128](http://www.w3.org/2001/04/xmlenc)|128|支援演算法來加密對稱金鑰來加密安全性權杖。|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#千瓦\-aes192](http://www.w3.org/2001/04/xmlenc)|192|支援演算法來加密對稱金鑰來加密安全性權杖。|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#千瓦\-aes256](http://www.w3.org/2001/04/xmlenc)|256|支援演算法來加密對稱金鑰來加密安全性權杖。|  
|RsaV15KeyWrap \- http:\/\/www.w3.org\/2001年\/04\/xmlenc\#rsa\-1\_5|1024|支援演算法來加密對稱金鑰來加密安全性權杖。|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001年\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|預設。 支援演算法來加密對稱金鑰來加密安全性權杖。|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0.log html|N\/A|成品 SourceId 層代中使用 AD FS 伺服器：在此案例中，STS 會使用 SHA1\(每個 SAML 2.0 標準中的建議\)建立成品 sourceiD 簡短的 160 位元值。<br /><br />ADFS web 代理程式也使用\(從 WS2003 時間範圍內的舊版元件\)來識別變更的 「 上次更新的時間值，讓它知道何時更新來自 STS 的資訊。|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|當 AD FS 伺服器驗證的 SAML AuthenticationRequest 簽章，請使用案例中，登入的成品解析要求或回應中，建立語彙基元\-簽署憑證。<br /><br />在這些情況下，還會使用 SHA256 是預設值，而且如果，才會使用 SHA1 夥伴\(信賴憑證者的合作對象\)無法支援 SHA256，而且必須使用 SHA1。|  
  
## <a name="BKMK_13"></a>權限需求  
執行安裝和初始設定的 AD FS 系統管理員必須具有網域系統管理員權限，本機網域中\(換句話說，網域加入同盟伺服器。\)  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

