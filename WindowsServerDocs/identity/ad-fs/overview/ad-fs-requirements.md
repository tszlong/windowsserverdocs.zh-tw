---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: "AD FS 2016 需求"
description: "安裝 Active Directory 同盟服務需求。"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>AD FS 需求

>適用於：Windows Server 2016

以下是部署 AD FS 需求：  
  
-   [憑證需求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬體需求](ad-fs-requirements.md#BKMK_2)  
  
-   [Proxy 需求](ad-fs-requirements.md#BKMK_3)  
  
-   [AD DS 需求](ad-fs-requirements.md#BKMK_4)  
  
-   [設定資料庫需求](ad-fs-requirements.md#BKMK_5)  
  
-   [瀏覽器需求](ad-fs-requirements.md#BKMK_6)  

-   [網路需求](ad-fs-requirements.md#BKMK_7)  
  
-   [使用權限要求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>憑證需求  
  
### <a name="ssl-certificates"></a>SSL 憑證

每個 AD FS 和 Web 應用程式 Proxy 伺服器有服務 HTTPS 要求同盟服務 SSL 憑證。  應用程式網路 Proxy，可以讓要求發行的應用程式提供服務的其他 SSL 憑證。

**建議：**適用於所有 AD FS 聯盟伺服器和 Web 應用程式 proxy 使用相同的 SSL 憑證。 

**需求：**

聯盟伺服器上的 SSL 憑證必須符合下列需求
- 憑證是公開受信任 （針對 production 部署）
- 憑證包含伺服器驗證增強金鑰使用量 (EKU) 值
- 憑證包含同盟服務的名稱，例如 「 fs.contoso.com 「 主旨或主旨另一種方式名稱 （舊） 中
- 連接埠 443 使用者憑證驗證，憑證包含 「 certauth。 \ < 同盟服務 name\ > 」，例如 「 certauth.fs.contoso.com 」 中舊
- 裝置登記或現代化驗證使用前的 Windows 10 戶端場所資源，舊必須包含 「 enterpriseregistration。 \ < upn suffix\ > [在使用每個 UPN 尾碼您在組織中。

在應用程式網路 Proxy SSL 憑證必須符合下列需求
- 如果您使用的 proxy 使用 Windows 整合驗證 proxy SSL 憑證 proxy AD FS 要求必須是相同（使用相同的按鍵）為聯盟伺服器 SSL 憑證
- 如果」ExtendedProtectionTokenCheck」是 AD FS 屬性支援（預設值 AD FS 中），proxy SSL 憑證必須是相同（使用相同的按鍵）聯盟伺服器 SSL 憑證
- 否則，proxy SSL 憑證的需求的一樣聯盟伺服器 SSL 憑證

### <a name="service-communication-certificate"></a>服務通訊的憑證
不需要大部分 AD FS 案例，其中包括 Azure AD 憑證此與 Office 365。 根據預設，AD FS 設定為服務通訊憑證的初始設定所提供的 SSL 憑證。

**建議：**
- 您在使用 SSL 使用相同憑證。  

### <a name="token-signing-certificate"></a>權杖簽署的憑證
這是憑證使用的登入發出權杖給信賴的對象，信賴廠商應用程式必須辨識憑證及相關已知及受信任的按鍵。 當權杖專屬的簽署憑證的變更，例如過期時，您設定一個新的憑證時，必須更新所有信賴派對。

**建議：**使用 AD FS 預設內部產生自我的權杖簽署的憑證。  

**需求：**
- 如果您的組織需要從企業 PKI 憑證會用於權杖登入，這可以使用 Install-AdfsFarm cmdlet SigningCertificateThumbprint 參數。
- 您是否使用的預設內部產生憑證或外部退出的憑證，當您變更權杖專屬的簽署憑證必須確保所有信賴派對更新的新的憑證的資訊。  否則，登入給任何依賴不更新將會失敗。

### <a name="token-encryptingdecrypting-certificate"></a>權杖加密日解密憑證
宣告提供者加密權杖發行給 AD FS 使用這個憑證。

**建議：**使用 AD FS 預設內部產生解密憑證自我預付碼。  

**需求：**
- 如果您的組織需要從企業 PKI 憑證會用於權杖登入，這可以使用 Install-AdfsFarm cmdlet DecryptingCertificateThumbprint 參數。
- 您是否使用的預設內部產生憑證或外部退出的憑證，當您變更解密憑證權杖必須確保所有宣告提供者都更新的新的憑證的資訊。  否則，登入使用任何宣告不在更新的提供者將會失敗。
  
> [!CAUTION]  
> 用來登入 token\ 和 token\ decrypting\ 日加密憑證的重大同盟服務的穩定性。 針對管理自己 token\ 簽署與 token\ decrypting\ 日加密憑證應該確定這些憑證的備份，且可獨立修復事件期間。  

### <a name="user-certificates"></a>使用者憑證
- 當使用的 x509 使用者憑證驗證，AD FS，所有使用者憑證的必須鏈到受到 AD FS 和 Web 應用程式 Proxy 伺服器信任的根憑證授權單位。

## <a name="BKMK_2"></a>硬體需求  
AD FS 和 Web 應用程式 Proxy 硬體需求 （實體或 virtual） 的 CPU、 上閘，因此您應該大小您發電廠處理容量。  
- 使用[AD FS 2016 容量規劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)若要判斷 AD FS 和 Web 應用程式 Proxy 伺服器，您將需要數目。

AD FS 的記憶體和磁碟需求是相當靜態，請查看如下表所示：


|**硬體需求**|**最低需求**|**建議的需求**|
|----- | ----- |-----|
|RAM|使用 2 GB|4 GB |
|磁碟空間|32 GB|100 GB |

**SQL Server 硬體需求**

如果您使用 AD FS 設定資料庫 SQL Server、 大小 SQL Server 根據最基本 SQL Server 建議。  AD FS 資料庫大小太小，而且 AD FS 不會將重要的處理負載放在資料庫執行個體。  AD FS，不過，連接至資料庫多次在驗證期間，因此應該穩定網路連接。  很抱歉，AD FS 設定資料庫無法支援 SQL Azure。
  
## <a name="BKMK_3"></a>Proxy 需求  
  
-   您必須外部網路存取權限的部署 Web 應用程式 Proxy 角色服務 \-遠端存取伺服器角色的一部分。 

-   第三方 proxy 必須支援[MS-ADFSPIP 通訊協定](https://msdn.microsoft.com/en-us/library/dn392811.aspx)以 AD FS proxy 支援。  第 3 個廠商清單廠商看到[常見問題集](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip)。

-   AD FS 2016 需要在 Windows Server 2016 上 Web 應用程式的 Proxy 伺服器。  舊版 proxy 無法執行 2016年發電廠行為層級 AD FS 2016 發電廠的設定。
  
-   聯盟 server 和 Web 應用程式 Proxy 角色服務無法安裝所在的電腦上。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站需求**  
  
- AD FS 需要網域控制站執行 Windows Server 2008，或更新版本。

- Microsoft Passport 工作的需要至少一個 Windows Server 2016 網域控制站。
  
> [!NOTE]  
> 與 Windows Server 2003 網域控制站的環境中的所有支援已都終止。 請造訪[這個頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)如需 Microsoft 技術支援週期詳細資訊。  
  
**網域 functional\ 層級需求**  
  
 - 所有使用者 account 網域與 AD FS 伺服器的加入的網域必須網域層級功能或更高版本的 Windows Server 2003 進行操作。  
  
 - Windows Server 2008 網域功能層級或更高版本，才能 client 憑證驗證如果憑證明確對應帳號到 AD DS 中。  
  
**架構需求**  
  
-   AD FS 2016 的全新安裝需要 Active Directory 2016 架構 （最小版本 85）。

-   提高 2016年層級 AD FS 發電廠行為層級 (FBL) 需要 Active Directory 2016 架構 （最小版本 85）。  

  
**服務 account 需求**  
  
-   任何標準核對可以當做服務 account AD fs。 也支援群組管理服務帳號。 當您設定 AD FS，會自動新增所需執行階段的權限。

-   群組管理服務帳號需要一個至少網域控制站執行 Windows Server 2012 或更高版本。  GMSA 必須在 [預設 live ' DATA-CN = 管理服務帳號的容器。  

-   Kerberos 驗證服務主體名稱為 '`HOST/<adfs\_service\_name>`' AD FS 服務 account 上必須將登記完畢。 根據預設，AD FS 會設定此建立新的 AD FS 發電廠時。  如果失敗，例如在碰撞或權限不足，您會看到一則警告，您必須手動新增。  
   
**網域需求**  
  
-   所有 AD FS 伺服器都必須連接到 AD DS 網域。  
  
-   必須在相同的網域部署發電廠中的所有 AD FS 伺服器。  
   
**使用多監視器樹系需求**  
  
-   每個網域或森林包含使用者向 AD FS 服務，則必須信任 AD FS 伺服器的加入的網域。  

-   樹系的 AD FS 服務 account 成員，必須所有使用者登入的樹系標示為都信任。 
  
-   AD FS 服務 account 必須為每個包含服務 AD FS 進行驗證使用者網域中的使用者屬性權限。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
本節需求和 AD FS 農場作為分別 Windows 內部資料庫 (WID) 或 SQL Server 資料庫限制：  
  
**WID**  
  
-   在 WID 發電廠不支援 SAML 2.0 成品解析度設定檔。    

-   權杖重播偵測不支援 WID 發電廠。 （此功能僅使用只能在何處做為聯盟提供者，使用外部宣告提供者的安全性權杖 AD FS 案例中）。  
  
下表提供多少 AD FS 伺服器的摘要 WID 與 SQL Server 發電廠支援。    
  
  
|| 1-100 可以廠商 （資源點數） 信任 AD FS 中設定 | 超過 100 資源點數信任設定  |
| --- |--- | --- |
|1-30 AD FS 伺服器|WID 支援|不支援使用 WID-所需的 SQL Server |
|超過 30 AD FS 伺服器|不支援使用 WID-所需的 SQL Server|不支援使用 WID-所需的 SQL Server  
  
**SQL Server**  
  
- 在 Windows Server 2016 AD fs 的支援 SQL Server 2008 和更高版本。

- 同時 SAML 成品解析度權杖重播偵測支援和陣列 SQL Server 中。  
  
## <a name="BKMK_6"></a>瀏覽器需求  
AD FS 驗證的瀏覽器或瀏覽器控制項透過執行時，您的瀏覽器必須符合下列需求：  
  
-   JavaScript 必須將支援  
  
-   對於單一登入，必須 client 瀏覽器設定允許 cookie  
  
-   伺服器名稱指示 \(SNI\) 必須支援  
  
-   使用者憑證與裝置憑證驗證的瀏覽器必須支援 SSL client 憑證驗證  

-   使用 Windows 整合式驗證順暢的登入，必須在本機該處或受信任的網站設定同盟服務名稱 （例如 https:\/\/fs.contoso.com)。
## <a name="BKMK_7"></a>網路需求  
 
**防火牆需求**  
  
這兩個防火牆位於之間 Web 應用程式 Proxy 聯盟伺服器發電廠及戶端和 Web 應用程式 Proxy 之間的防火牆必須 443 支援的 TCP 連接埠輸入。  
  
此外，如果 client 使用者憑證驗證 \ (使用 X509 clientTLS 驗證使用者 certificates\) 需要並不支援 certauth 端點 443 連接埠，AD FS 2016 需要用的 TCP 連接埠 49443 戶端和 Web 應用程式 Proxy 之間防火牆上輸入。 這不是需要應用程式網路 Proxy 之間聯盟 servers\ 防火牆上）。 

如需有關混合連接埠需求查看[混合身分連接埠和通訊協定](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

如需詳細資訊請查看[的最佳做法保護 Active Directory 同盟服務](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**DNS 需求**  
  
-   內部網路存取權限存取 AD FS 服務的公司連絡 \(intranet\) 中的所有戶端都必須無法解析負載平衡器 AD FS 伺服器或 AD FS 伺服器 AD FS 服務名稱。  
  
-   外部網路存取權限存取 AD FS 服務的公司網路 \(extranet\/internet\) 以外的所有戶端都必須 AD FS 服務名稱解析負載平衡器 Web 應用程式的 Proxy 伺服器或網路應用程式的 Proxy 伺服器。  
  
-   在 DMZ 每個 Web 應用程式的 Proxy 伺服器必須 AD FS 服務的名稱解析為負載平衡器 AD FS 伺服器或 AD FS 伺服器。 這可以使用其他 DNS 伺服器 DMZ 網路，或變更本機伺服器解析度使用主機檔案達成。  
  
-   適用於 Windows 整合式驗證，您必須使用 DNS A 記錄 \(not CNAME\) 同盟服務的名稱。  

-   連接埠 443 使用者憑證驗證，必須設定 「 certauth。 \ < 同盟服務 name\ > [DNS 伺服器聯盟或 web 應用程式 proxy 解析中。

-   裝置登記或現代化驗證使用前的 Windows 10 戶端場所資源，「 enterpriseregistration。 \ < upn suffix\ > 」，您在組織中使用每個 UPN 尾碼必須解析聯盟伺服器或網路應用程式的 proxy 設定。

**負載平衡器需求**
- 不必須負載平衡器終止 SSL。 AD FS 支援，將會使時終止 SSL 憑證驗證多個使用的案例。 使用區分大小寫不支援終止 SSL 負載平衡器在。 
- 建議使用支援 SNI 負載平衡器。 事件未在您 AD FS 使用 0.0.0.0 回溯繫結 / Web 應用程式的 Proxy 伺服器應該提供因應措施。
- 建議使用 HTTP (不 HTTPS) 健康探查端點進行負載平衡器健康檢查路由傳輸。 這可避免任何相關的許多 SNI 的問題。 這些探查端點回應 HTTP 200 [確定]，本機提供的任何依賴後端服務。 可以存取 HTTP 探查透過使用路徑 '日 adfs 日探查' HTTP
    - http://&lt;Web 應用程式 Proxy 名稱&gt;日 adfs 日探查
    - http://&lt;ADFS 伺服器名稱&gt;日 adfs 日探查
    - http://&lt;Web 應用程式 Proxy IP 位址&gt;日 adfs 日探查
    - http://&lt;ADFS IP 位址&gt;日 adfs 日探查
- 不建議使用 DNS 負載平衡用來循環配置資源。 使用這類負載平衡不提供移除使用健康探查負載平衡器節點自動化的方式。 
- 不建議使用 IP 為基礎的工作階段相關性或工作階段自黏驗證流量 AD FS 負載平衡器中。 連接至 (換貨 Online) 的 Office 365 郵件服務使用的電子郵件戶端舊版驗證通訊協定時，這可能會造成某些節點載。 

## <a name="BKMK_13"></a>使用權限要求  
AD FS 伺服器上執行，安裝，以及 AD FS 的初始設定的系統管理員必須本機系統管理員權限。  如果本機系統管理員會建立 Active Directory 物件的權限，就必須先網域系統管理員建立所需的廣告物件，然後使用 AdminConfiguration 參數 AD FS 發電廠的設定。  
  
  

