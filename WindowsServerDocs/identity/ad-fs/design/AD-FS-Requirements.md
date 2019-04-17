---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: "使用 SQL Server 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>AD FS 需求

>適用於： Windows Server 2012 R2

以下是您必須符合部署 AD FS 時的各種需求：  
  
-   [憑證需求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬體需求](AD-FS-Requirements.md#BKMK_2)  
  
-   [軟體需求](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS 需求](AD-FS-Requirements.md#BKMK_4)  
  
-   [設定資料庫需求](AD-FS-Requirements.md#BKMK_5)  
  
-   [瀏覽器需求](AD-FS-Requirements.md#BKMK_6)  
  
-   [外部需求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [網路需求](AD-FS-Requirements.md#BKMK_7)  
  
-   [屬性市集需求](AD-FS-Requirements.md#BKMK_8)  
  
-   [應用程式需求](AD-FS-Requirements.md#BKMK_9)  
  
-   [驗證的需求](AD-FS-Requirements.md#BKMK_10)  
  
-   [地點加入需求](AD-FS-Requirements.md#BKMK_11)  
  
-   [密碼編譯需求](AD-FS-Requirements.md#BKMK_12)  
  
-   [使用權限要求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>憑證需求  
憑證播放最重要的角色保護聯盟伺服器、 Web 應用程式 Proxy、 claims\ 感知應用程式，以及 Web 戶端間通訊。 根據是否您的設定聯盟伺服器或 proxy 電腦，此一節中所述，而有所不同憑證的需求。  
  
**聯盟伺服器的憑證**  
  
|||  
|-|-|  
|**憑證類型**|**需求、 支援和必須知道的事項**|  
|**安全通訊端層 \(SSL\) 憑證：**這是確保聯盟伺服器戶端間通訊使用標準 SSL 憑證。|-此憑證必須公開 trusted\ * X509 v3 憑證。<br />-所有戶端存取任何 AD FS 端點必須都信任此憑證。 建議使用公用 \(third\-party\) 憑證授權單位發行憑證 \(CA\)。 您可以在實驗室測試環境聯盟伺服器成功使用 self\ 簽章 SSL 憑證。 不過，production 環境中，我們建議您從公開 CA 取得該憑證。<br />-支援 Windows Server 2012 R2 的支援 SSL 憑證任何按鍵大小。<br />-並不支援使用 CNG 按鍵的憑證。<br />-使用工作地點裝置 Join\ 日登記服務一起時, 主題替代名稱 SSL 憑證 AD FS 服務必須包含後面組織，例如 enterpriseregistration.contoso.com 的使用者主體名稱 \(UPN\) 尾碼值 enterpriseregistration。<br />-萬用字元憑證的支援。 當您建立您的 AD FS 陣列時，將會系統會提示您以提供服務的名稱，AD FS 服務 \ (例如， **adfs.contoso.com**。<br />-建議使用 Web 應用程式 Proxy 相同 SSL 憑證。 但這是**需要**以支援 Windows 整合式驗證端點透過 Web 應用程式 Proxy 和延伸保護驗證會亮起來 \(default setting\) 時，會相同。<br />此憑證-主體名稱用來表示同盟服務的名稱為每個您要部署的 AD FS 執行個體。 基於這個原因，您可能要考慮選擇主體名稱在任何新 CA\ 發行憑證的最佳代表您的公司或組織的合作夥伴的名稱。<br />    認證身分必須符合同盟服務名稱 \ (例如，fs.contoso.com\)。身分任一主旨替代副檔名的類型 dNSName 或者，如果不有任何主題替代名稱的項目，請主體名稱指定為保持一般的名稱。 其中一個符合同盟服務名稱提供多個主題替代名稱的項目可以是憑證中。<br />-   **重要事項：**已經建議您 AD FS 發電廠的所有節點，以及陣列 AD FS 中的所有 Web 應用程式 proxy 上使用相同的 SSL 憑證。|  
|**服務通訊的憑證：**這個憑證允許 WCF 訊息安全性保護之間聯盟伺服器通訊。|-預設為服務通訊憑證使用 SSL 憑證。  但您也可以選擇另一個憑證設定為服務通訊的憑證。<br />-   **重要事項：**如果您使用 SSL 憑證服務通訊的憑證，以在 SSL 憑證過期時，請確定您服務通訊的憑證以進行更新的 SSL 憑證。 這不會自動執行。<br />-此憑證的必須信任的 AD FS 使用 WCF 訊息安全性用。<br />-我們建議您使用伺服器驗證憑證的公用 \(third\-party\) 憑證授權單位發行 \(CA\)。<br />-服務通訊憑證無法使用 CNG 按鍵的憑證。<br />-請使用 AD FS 管理主控台可以管理此憑證。|  
|**Token\ 簽署的憑證：**此為標準 X509 用於安全地登入所有權杖問題聯盟伺服器的憑證。|依預設，AD FS 建立 2048 元按鍵 self\ 簽署的憑證。<br />-也支援發出 CA 憑證，並可以使用 AD FS 管理 snap\ 中變更<br />必須會儲存 CA 發行憑證與 CSP 密碼編譯提供者所提供的存取。<br />憑證登入-預付碼無法使用 CNG 按鍵的憑證。<br />-AD FS 不需要權杖簽署的憑證外部參加授權。<br />    AD FS 自動續約這些 self\ 簽署的憑證，過期之前第一次設定新的憑證，以次要的憑證以允許的合作夥伴使用，然後到在 [處理程序稱為 「 自動憑證變換主要翻轉。我們建議使用預設值，自動權杖簽署的憑證。<br />    如果您的組織會有不同的憑證來設定需要權杖登的原則，您可以指定憑證使用 Powershell 的安裝時間 \ （使用 Install\-AdfsFarm cmdlet\ 的 – SigningCertificateThumbprint 參數）。  安裝完成後，您可以檢視和管理使用 AD FS Management console 或 Powershell cmdlet Set\-AdfsCertificate 和 Get\-AdfsCertificate 權杖專屬的簽署憑證。<br />    當憑證外部參加授權用於權杖登入時，AD FS 不會執行憑證自動續約或變換。  必須由系統管理員的身分執行此程序。<br />    若要允許憑證變換一個憑證接近過期時，可在 AD FS 設定次要權杖簽署的憑證。 根據預設，所有權杖專屬的簽署憑證發行聯盟中繼資料中，但僅主要 token\ 簽署的憑證使用 AD FS 確實登入權杖。|  
|**Token\ decryption\/加密憑證：**這是一種標準 X509 憑證也就是用來 decrypt\ 日加密任何連入權杖。 這也被發行聯盟中繼資料中。|依預設，AD FS 建立 2048 元按鍵 self\ 簽署的憑證。<br />-也支援發出 CA 憑證，並可以使用 AD FS 管理 snap\ 中變更<br />必須會儲存 CA 發行憑證與 CSP 密碼編譯提供者所提供的存取。<br />-Token\ decryption\/加密憑證無法使用 CNG 按鍵的憑證。<br />依預設，AD FS 產生，並使用它自己，內部並 self\ 簽署的憑證權杖解密。  AD FS 不需要為這個項目的外部參加授權的憑證。<br />    此外，AD FS 自動續約這些 self\ 簽署的憑證到期。<br />    **我們建議使用預設值，自動權杖解密的憑證。**<br />    如果您的組織會有不同的憑證來設定需要權杖解密原則，您可以指定憑證使用 Powershell 的安裝時間 \ （使用 Install\-AdfsFarm cmdlet\ 的 – DecryptionCertificateThumbprint 參數）。  安裝完成後，您可以檢視和管理使用 AD FS Management console 或 Powershell cmdlet Set\-AdfsCertificate 和 Get\-AdfsCertificate 權杖解密憑證。<br />    **使用外部參加授權的憑證權杖解密時, AD FS 不會執行憑證自動續約。必須由系統管理員的身分執行此程序**。<br />-AD FS 服務 account 必須 token\ 簽署的憑證私密金鑰存取在本機電腦的個人的市集。 這是處理的安裝程式。 您也可以使用 AD FS 管理 snap\ 中，以確保如果後續變更 token\ 簽署的憑證的存取權。|  
  
> [!CAUTION]  
> 用來登入 token\ 和 token\ decrypting\ 日加密憑證的重大同盟服務的穩定性。 針對管理自己 token\ 簽署與 token\ decrypting\ 日加密憑證應該確定這些憑證的備份，且可獨立修復事件期間。  
  
> [!NOTE]  
> AD FS 中，您可以變更適用於數位簽章 SHA\-1 或 SHA\-256 \(more secure\) 安全 Hash 演算法 \(SHA\) 層級。 AD FS 不支援使用憑證的其他 hash 方法，例如 MD5 \ （預設 hash 的演算法所使用的 Makecert.exe command\ 列 tool\）。 最好的安全性，以我們建議您使用 SHA\-256 \（這由 default\ 設定）的所有特徵標記。 建議 SHA\-1 只在您必須交互的通訊，例如 non\ Microsoft product 或傳統版本 AD fs 使用 SHA\ 256 不支援的案例。  
  
> [!NOTE]  
> 您收到 CA 憑證之後，確認所有憑證的匯都入至本機電腦的個人憑證存放區。 您可以個人憑證 MMC snap\ 在使用網上商店匯入的憑證。  
  
## <a name="BKMK_2"></a>硬體需求  
適用於在 Windows Server 2012 R2 的 AD FS 聯盟伺服器下列最低與建議的硬體需求：  
  
||||  
|-|-|-|  
|**硬體需求**|**最低需求**|**建議的需求**|  
|CPU 速度|1.4 64\ 位元處理器|Quad\ 核心，2 GHz|  
|RAM|512 MB|4 GB|  
|磁碟空間|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>軟體需求  
AD FS 下列需求適用於建置 Windows Server® 2012 R2 作業系統伺服器功能：  
  
-   您必須外部網路存取權限的部署 Web 應用程式 Proxy 角色服務 \-部分的 Windows Server® 2012 R2 遠端存取伺服器角色。 在 Windows Server® 2012 R2 AD FS 不支援舊版聯盟 proxy 伺服器。  
  
-   聯盟 server 和 Web 應用程式 Proxy 角色服務無法安裝所在的電腦上。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站需求**  
  
所有使用者網域 AD FS 伺服器的加入的網域中的網域控制站必須執行 Windows Server 2008 或更新版本。  
  
> [!NOTE]  
> 與 Windows Server 2003 網域控制站的環境中的所有支援都會之後延伸支援都結束日期的 Windows Server 2003 都終止。 針對建議儘快升級他們網域控制站。 請造訪[這個頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)如需 Microsoft 技術支援週期詳細資訊。 針對發現的問題，而特定 Windows Server 2003 網域控制站環境修正發出僅限安全性問題修正如果可發行之前的延伸支援的 Windows Server 2003 到期。  
  
**網域 functional\ 層級需求**  
  
所有使用者 account 網域與 AD FS 伺服器的加入的網域必須網域層級功能或更高版本的 Windows Server 2003 進行操作。  
  
大部分的 AD FS 功能不需要 AD DS functional\ 層級修改順利運作。 不過，Windows Server 2008 網域功能層級或更高版本，才能 client 憑證驗證憑證明確對應到 AD DS 中的使用者 account 如果順利運作。  
  
**架構需求**  
  
-   AD FS 不需要的架構變更或修改 AD DS functional\ 層級。  
  
-   若要使用的工作地點加入的功能，AD FS 伺服器加入的樹系的結構描述必須為 Windows Server 2012 R2。  
  
**服務 account 需求**  
  
-   任何標準服務 account 可以當做服務 account AD fs。 也支援群組管理服務帳號。 這需要一個至少網域控制站 \ (建議部署兩個或 more\) 執行 Windows Server 2012 或更高版本。  
  
-   適用於 Kerberos 驗證，AD FS domain\ 加入戶端之間的功能 ' HOST\ 日 < adfs\_service\_name >' 必須為服務帳號 SPN 會登記。 根據預設，AD FS 會設定此建立新的 AD FS 發電廠有權限來執行此作業不足時。  
  
-   AD FS 服務 account 必須信任使用者網域中，包含使用者向 AD FS 服務。  
  
**網域需求**  
  
-   所有 AD FS 伺服器都必須連接到 AD DS 網域。  
  
-   必須在單一網域部署發電廠中的所有 AD FS 伺服器。  
  
-   AD FS 伺服器加入網域必須信任包含使用者向 AD FS 服務每個使用者 account 網域。  
  
**使用多監視器樹系需求**  
  
-   每個使用者 account 網域或森林包含使用者服務 AD FS 進行驗證，AD FS 伺服器加入網域必須標示為信任。  
  
-   AD FS 服務 account 必須信任使用者網域中，包含使用者向 AD FS 服務。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
以下是需求和適用的限制會依據的設定存放區類型：  
  
**WID**  
  
-   如果您依賴 100 或較少廠商信任，WID 發電廠的 30 聯盟伺服器的上限。  
  
-   在 WID 設定資料庫不支援 SAML 2.0 成品解析度設定檔。  在 WID 設定資料庫權杖重播偵測不受支援。 只在案例中，做為聯盟提供者，使用外部宣告提供者的安全性權杖 AD FS 只使用這項功能。  
  
-   部署 AD FS 伺服器中出現的資料中心容錯移轉或，只要伺服器數量不會超過 30 地理負載平衡支援。  
  
下表使用 WID 發電廠提供摘要。  使用它來規劃實作。  
  
||||  
|-|-|-|  
||1 \-100 資源點數信任|超過 100 資源點數信任|  
|1 \-30 AD FS 節點|WID 支援|不支援使用 WID \-SQL 需要|  
|超過 30 AD FS 節點|不支援使用 WID \-SQL 需要|不支援使用 WID \-SQL 需要|  
  
**SQL Server**  
  
在 Windows Server 2012 R2 AD FS，您可以使用 SQL Server 2008 與更高版本  
  
## <a name="BKMK_6"></a>瀏覽器需求  
AD FS 驗證的瀏覽器或瀏覽器控制項透過執行時，您的瀏覽器必須符合下列需求：  
  
-   JavaScript 必須將支援  
  
-   Cookie 必須亮  
  
-   伺服器名稱指示 \(SNI\) 必須支援  
  
-   使用者憑證與裝置憑證驗證 \ (的工作地點加入 functionality\)，在瀏覽器必須支援 SSL client 憑證驗證  
  
幾個重要的瀏覽器與平台已經過驗證的轉譯與功能的詳細資訊，如下所示。 瀏覽器並不此表格所涵蓋的裝置仍支援符合上述的需求：  
  
|||  
|-|-|  
|**瀏覽器**|**平台**|  
|IE 10.0|Windows 7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|IE 11.0|Windows7、 Windows 8.1、 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012 R2|  
|Windows Web 驗證代理人|Windows 8.1|  
|Firefox \[v21\]|Windows 7、 Windows 8.1|  
|Safari \[v7\]|iOS 6 Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2、 Mac OS\ X 10.7|  
  
> [!IMPORTANT]  
> 已知問題 \-Firefox： 將裝置使用裝置的憑證辨識的工作地點加入的功能不正常 Windows 平台上運作。 Firefox 目前不支援使用在 Windows 戶端使用者憑證存放區提供憑證執行的 SSL client 憑證驗證。  
  
**Cookie**  
  
AD FS 建立 session\ 與持續性必須 client 電腦 sign\ 中提供、 sign\ 出、 單一 sign\ 上 \(SSO\)，以及其他功能儲存 cookie。 因此，必須接受 cookie 設定 client 瀏覽器。 Cookie 可用來驗證都是安全超傳輸通訊協定 \(HTTPS\) 工作階段 cookie 所撰寫的原生伺服器。 如果 client 瀏覽器不允許 cookie 這些設定，AD FS 無法正常運作。 持續 cookie 可用來保留宣告提供者的使用者選取項目。 您可以停用來設定檔 AD FS sign\ 在網頁中使用的設定。 基於安全性考量需要 SSL TLS\ 日的支援。  
  
## <a name="BKMK_extranet"></a>外部需求  
為了提供給 AD FS 服務外部網路的存取，您必須部署 Web 應用程式 Proxy 角色服務外部面對角色該 proxy 驗證要求 AD FS 服務安全的方式。 這會提供 AD FS 服務端點隔離以及隔離的所有安全性金鑰 \ （例如權杖簽署 certificates\) 從來自網際網路的要求。 此外，例如柔軟的外部鎖定的功能需要 Proxy Web 應用程式的使用。 如需 Web 應用程式 Proxy 的詳細資訊，請查看[Web 應用程式 Proxy](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果您想要使用 third\ 廠商 proxy 外部網路的存取，這個 third\ 廠商 proxy 必須支援定義的通訊協定[http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)。  
  
## <a name="BKMK_7"></a>網路需求  
設定適當的網路下列服務已成功 AD FS 您在組織中部署的重要：  
  
**設定公司防火牆**  
  
這兩個防火牆位於之間 Web 應用程式 Proxy 聯盟伺服器發電廠及戶端和 Web 應用程式 Proxy 之間的防火牆必須 443 支援的 TCP 連接埠輸入。  
  
此外，如果 client 使用者憑證驗證 \ (使用 X509 clientTLS 驗證使用者 certificates\) 時，在 Windows Server 2012 R2 AD FS 需要用的 TCP 連接埠 49443 戶端和 Web 應用程式 Proxy 之間防火牆上輸入。 這不是需要應用程式網路 Proxy 之間聯盟 servers\ 防火牆上）。  
  
**設定 DNS**  
  
-   為內部網路存取權限存取 AD FS 服務的公司連絡 \(intranet\) 中的所有戶端都必須能 AD FS 服務名稱解析 \ （SSL certificate\ 所提供的名稱） AD FS 伺服器或 AD FS 伺服器的負載平衡器。  
  
-   外部網路存取權限存取 AD FS 服務的公司網路 \(extranet\/internet\) 以外的所有戶端都必須能 AD FS 服務名稱解析 \ （SSL certificate\ 所提供的名稱） 的 Web 應用程式的 Proxy 伺服器或網路應用程式的 Proxy 伺服器的負載平衡器。  
  
-   存取外部正常運作，在 DMZ 每個 Web 應用程式的 Proxy 伺服器必須能 AD FS 服務的名稱解析 \ （SSL certificate\ 所提供的名稱） AD FS 伺服器或 AD FS 伺服器的負載平衡器。 這可以使用其他 DNS 伺服器 DMZ 網路，或變更本機伺服器解析度使用主機檔案達成。  
  
-   對於整合式 Windows 工作中網路，以及網路上的端點透過 Web 應用程式 Proxy 公開子集外驗證，您必須使用 A 記錄 \(not CNAME\) 指向負載平衡器。  
  
適用於企業 DNS 同盟服務和裝置登記服務設定的詳細資訊，請查看[設定 DRS 和同盟服務的公司 DNS](https://technet.microsoft.com/library/dn486786.aspx)。  
  
設定公司 DNS proxy Web 應用程式的詳細資訊，會看到的 「 設定 DNS] 區段中[步驟 1： 設定 Web 應用程式 Proxy 架構](https://technet.microsoft.com/library/dn383644.aspx)。  
  
了解如何設定叢集 IP 位址或叢集 FQDN 使用 NLB 資訊，會看到指定叢集參數， [http:///\/go.microsoft.com\/fwlink\/ 嗎？LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>屬性市集需求  
AD FS 需要至少屬性市集驗證使用者與解壓縮安全性宣告那些使用者使用。 針對一份屬性儲存 AD FS 支援，請查看[的角色的屬性儲存](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> AD FS 預設會自動建立 「 Active Directory 「 屬性市集。 您的組織是否做為 account 合作夥伴屬性市集需求而定 \ （主持聯盟的 users\） 或資源合作夥伴 \ （主持聯盟的 application\）。  
  
**LDAP 屬性存放區**  
  
當您使用其他輕量型 Directory 存取通訊協定 \ \-based 屬性存放區 (LDAP\)，您必須連接到 LDAP 伺服器支援 Windows 整合式驗證。 RFC 2255 中所述 LDAP 連接字串必須也撰寫 LDAP URL 的格式。  
  
它也會需要 AD FS 服務服務負責有擷取 LDAP 屬性存放區中的使用者資訊的權限。  
  
**SQL Server 屬性存放區**  
  
AD fs 在 Windows Server 2012 R2 順利運作，裝載 SQL Server 屬性網上商店的電腦必須執行 Microsoft SQL Server 2008，或更高版本。 當您使用 SQL\ 為基礎的屬性存放區時，您還必須設定連接字串。  
  
**自訂屬性存放區**  
  
您可以開發自訂屬性存放區，可讓進階的案例。  
  
-   建置到 AD FS 原則語言可以參考自訂屬性存放區，以便增強案例下列任一項：  
  
    -   建立 [本機驗證使用者宣告  
  
    -   補充外部驗證使用者宣告  
  
    -   若要取得權杖使用者的授權  
  
    -   若要取得行為的使用者權杖服務的授權  
  
    -   發行信賴派對發出 AD FS 的安全性權杖中的其他資料。  
  
-   .NET 4.0 上方或更高版本，就必須先建置所有自訂屬性存放區。  
  
當您使用自訂的屬性網上商店時，您也可能設定連接字串。 若是如此，您可以輸入自訂可讓您自訂屬性存放區的連接您選擇的程式碼。 在這種情形連接字串是一組 name\ 日值組解譯為實作自訂屬性網上商店的開發人員。如需有關開發和使用自訂屬性存放區的詳細資訊，請[屬性市集概觀](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="BKMK_9"></a>應用程式需求  
AD FS 支援，請使用下列通訊協定 claims\ 感知應用程式：  
  
-   -聯盟 WS\  
  
-   WS\ 信任  
  
-   使用 IDPLite、 SPLite 與 eGov1.5 設定檔 SAML 2.0 通訊協定。  
  
-   OAuth 2.0 授權授與的設定檔  
  
AD FS 也支援驗證以及授權的任何支援的應用程式網路 Proxy non\ claims\ 感知應用程式。  
  
## <a name="BKMK_10"></a>驗證的需求  
**AD DS 驗證 \(Primary Authentication\)**  
  
內部網路存取權限的支援下列標準驗證機制 AD ds:  
  
-   Windows 使用交涉 Kerberos 與 NTLM 的整合式驗證  
  
-   使用密碼 username\ 日表單驗證  
  
-   使用對應至帳號 AD ds 憑證憑證驗證  
  
外部網路的存取，支援下列驗證機制：  
  
-   使用密碼 username\ 日表單驗證  
  
-   使用對應至帳號 AD ds 憑證憑證驗證  
  
-   Windows 使用交涉 \(NTLM only\) WS\ 信任端點接受 Windows 整合式驗證，驗證整合。  
  
憑證驗證：  
  
-   延伸到智慧卡，可保護 pin 碼。  
  
-   輸入 pin 碼使用者 GUI AD FS 不提供，也不需要將 client 作業系統使用 client TLS 時所顯示的一部分。  
  
-   讀取和密碼編譯服務提供者智慧卡 \(CSP\) 必須瀏覽器所在的電腦上運作。  
  
-   智慧卡憑證必須鏈到所有伺服器 AD FS 和 Web 應用程式的 Proxy 伺服器上受信任的網站。  
  
-   憑證必須對應到 AD ds 帳號下列方法：  
  
    -   憑證主體名稱相當於在 AD DS 帳號 LDAP 分辨名稱。  
  
    -   憑證主旨 altname 擴充功能有使用者主體名稱使用者中帳號 AD DS \(UPN\)。  
  
進行完美的 Windows 整合驗證使用 Kerberos 內部網路  
  
-   需要服務的名稱為 「 信任的網站或近端網站的一部分。  
  
-   此外，HOST\ / < adfs\_service\_name > SPN 必須設定服務帳號，AD FS 農場上執行。  
  
**Multi\ 雙因素驗證**  
  
AD FS 支援額外的驗證 \ （超過主要驗證，AD DS\ 支援） 使用提供者模型是針對 vendors\ 日可以建立自己的系統管理員可以登記並登入時使用 multi\ 雙因素驗證介面卡。  
  
每個 MFA 配接器必須建立.NET 4.5 上方。  
  
適用於 MFA 詳細資訊，請查看[管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**裝置驗證**  
  
AD FS 支援的裝置驗證使用憑證的裝置登記服務提供期間終端使用者的工作地點加入他們的裝置的動作。  
  
## <a name="BKMK_11"></a>地點加入需求  
使用者可以的工作地點加入他們的裝置，請使用 AD FS 的組織。 AD FS 中裝置登記服務支援此功能。 因此，使用者會取得 SSO 跨越 AD FS 支援的應用程式的其他優點。 此外，系統管理員可以管理藉由應用程式只有已加入組織的工作地點裝置上限制存取的風險。 以下是此案例，以便下列需求。  
  
-   AD FS 適用於 Windows 8.1 和 iOS 裝置 5\ + 支援的工作地點加入  
  
-   若要使用的工作地點加入的功能，AD FS 伺服器加入的樹系的結構描述必須 Windows Server 2012 R2。  
  
-   主旨替代名稱 SSL 憑證 AD FS 服務必須包含後面組織，例如 enterpriseregistration.corp.contoso.com 的使用者主體名稱 \(UPN\) 尾碼值 enterpriseregistration。  
  
## <a name="BKMK_12"></a>密碼編譯需求  
下表 AD FS 權杖登入，權杖 encryption\/解密功能提供額外的密碼編譯支援資訊：  
  
||||  
|-|-|-|  
|**演算法**|**長度鍵**|**回應/Applications\ Protocols\ 日**|  
|TripleDES – 預設 192 \ (支援 192 – 256\) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|支援的安全性權杖的加密演算法。|  
|AES128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|支援的安全性權杖的加密演算法。|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|支援的安全性權杖的加密演算法。|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**預設的**。 支援的安全性權杖的加密演算法。|  
|TripleDESKeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|.NET 4.0\+ 所支援的所有鍵大小|支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|預設值。 支援演算法加密對稱式的安全性權杖已加密金鑰。|  
|SHA1\ http: / / \ 日 www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|A N\ 日|AD FS 伺服器用於成品 SourceId 代： 在本案例中，STS 使用 SHA1 \ （每個中 SAML 2.0 standard\ 推薦） 來建立成品 sourceiD 簡短 160 元值。<br /><br />ADFS web 代理程式也用 \ （傳統元件 WS2003 timeframe\） 找出的變更，在 [上次更新日期 」 的時間數值，讓它知道更新 STS 資訊的時機。|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|A N\ 日|當 AD FS 伺服器的驗證的 SAML AuthenticationRequest 簽章，請使用案例中，登入成品解析度要求或回應、 建立 token\ 簽署的憑證。<br /><br />在這些案例中，SHA256 預設值，且如果合作夥伴 \(relying party\) 不支援 SHA256，必須使用 SHA1 只會使用 SHA1。|  
  
## <a name="BKMK_13"></a>使用權限要求  
執行安裝及 AD FS 的初始設定的系統管理員必須網域系統管理員權限在本機網域 \ (亦即的聯盟伺服器所加入的網域。 \)  
  
## <a name="see-also"></a>也了  
[在 Windows Server 2012 R2 的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

