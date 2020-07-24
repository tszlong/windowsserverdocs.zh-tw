---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: 將 AD RMS 升級至 Windows Server 2016
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.prod: windows-server
ms.topic: article
ms.openlocfilehash: cb27477f71dbded1f1171fde613f55f6267fc2cb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965470"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>將 AD RMS 升級至 Windows Server 2016

## <a name="introduction"></a>簡介

Active Directory Rights Management Services （AD RMS）是保護機密檔和電子郵件的 Microsoft 服務。 不同于傳統的保護方法（例如防火牆和 Acl），不論檔案的位置或傳輸方式為何，AD RMS 加密和保護都是持續性的。 

本檔提供從 Windows Server 2012 R2 （含 SQL Server 2012）遷移至 Windows Server 2016 和 SQL Server 2016 的指引。 您可以使用相同的程式，從舊版但支援的 AD RMS 版本進行遷移。
請注意，Active Directory Rights Management Services 已不再進行開發，而且客戶應該考慮遷移至[Azure 資訊保護](https://azure.microsoft.com/services/information-protection/)的最新功能，以提供更完整的裝置和應用程式支援的一組更全面的功能。 

如需從 AD RMS 遷移至 Azure 資訊保護，而不需要重新保護內容的詳細資訊，請參閱[Azure 資訊保護遷移檔](/azure/information-protection/migrate-from-ad-rms-to-azure-rms)。

## <a name="about-the-environment-used-in-this-guide"></a>關於本指南中使用的環境

AD FS 是 AD RMS 安裝的選擇性元件。 在本指南中，會假設使用 ADFS。 如果您的環境中未使用 ADFS 來支援 AD RMS 使用者，您可以略過參照 ADFS 的所有步驟。

在本指南中，SQL Server 會藉由執行平行安裝並透過備份移動資料庫，升級至 SQL Server 2016。 或者，如果您可以就地將 AD RMS 和 ADFS 資料庫伺服器升級為 SQL Server 2016，您可以在完成之後移至本檔的下一節，而不必依照本節中的步驟進行。  

## <a name="installation"></a>安裝

### <a name="configuring-sql-server-2016"></a>設定 SQL Server 2016

下一節將詳細說明直接與 SQL Server 2016 設定相關的執行工作。 本指南著重于使用伺服器管理員和 SQL Server Management Studio 來完成這些工作。

這些步驟必須在 SQL Server 2016 安裝完成。 根據貴組織的標準做法和原則，在適當的硬體上安裝 SQL Server 2016。 

#### <a name="preparing-the-sql-server"></a>準備 SQL Server

下一節將詳細說明如何準備 SQL Server，讓它可以升級至 SQL Server 2016，然後再升級 AD RMS 平臺中的其他服務，以使用 Windows Server 2016。

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>將 SQL Server 2016 的 CNAME 新增至 DNS

CNAME 是用來協助確保 Windows Server 2016 安裝程式將會取得適當的資料，因為它會指向新的 SQL Server 2016。 **注意：如果已針對 ADFS 和 AD RMS 服務使用 CNAME，您可以繼續進行後續步驟。**


**將 SQL Server 2016 的 CNAME 新增至 DNS**

1.  使用網域系統管理員認證登入 Windows Server 2012 R2 網域控制站。

2.  開啟 [伺服器管理員]。

3.  按一下 [**工具**]，然後選取 [ **dns** ] 以開啟 dns 管理員。

4.  從左側流覽窗格中，展開 [DC]，然後開啟 [**向前對應區域**]。

5.  開啟適當的網域資源，然後以滑鼠右鍵按一下右視圖窗格，再選取 [**新增別名（CNAME）** ] 開始建立 CNAME。

6.  在 [別名名稱] 中輸入邏輯名稱，以區別它與可能存在的其他類型（例如 SQLADRMS 或 SQLADFS）

7.  輸入名稱之後，請提供目標主機的 FQDN，這會是新的 SQL Server 2016 伺服器。 (例如 SQL2016.contoso.com）

8.  輸入所有資訊後，按一下 **[確定]**。

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>備份 AD RMS 和 ADFS 資料庫

AD RMS 和 ADFS 資料庫保留 AD RMS 所需的重要資訊，例如伺服器授權人憑證的公開金鑰、許可權原則範本、ADFS 設定資料，以及記錄資訊。 如果沒有這些資料庫，用戶端就無法發出授權來取用受保護的內容，還有其他問題。

在資料庫中，AD RMS 設定資料庫會被視為最重要的，因為它會儲存 SLC、許可權原則範本、使用者的金鑰和設定資訊。 因此，雖然您應該小心備份所有 AD RMS 和 ADFS 資料庫，但您應該規劃定期備份設定資料庫。

記錄資料庫會將使用者要求的相關資訊儲存至憑證的 AD RMS 叢集，並使用授權。 此資料庫的備份策略應以保留這類資訊的公司原則為基礎。

目錄服務資料庫並不是 AD RMS 功能的關鍵，如果遺失最新的資料，資料庫將會隨著 AD RMS 伺服器接收到憑證和使用授權的要求而重新填入資訊。 您不需要定期備份此資料庫，但您必須至少有一個資料庫複本，因為它原本是在部署 AD RMS 之後設定的。

**使用 Microsoft SQL Server 備份 AD RMS 和/或 ADFS 資料庫**

1.  使用 SQL 2012 登入 Windows Server 2012 R2 AD RMS 資料庫伺服器。

2.  依序按一下 [**開始**]、[**所有程式**]、[ **Microsoft SQL Server**]，然後按一下 [ **SQL Server Management Studio**]。

3.  在 [**連接到伺服器**] 視窗中，確認主控 AD RMS 資料庫的伺服器位於 [**伺服器名稱**] 方塊中，然後按一下 **[連接]**。

4.  展開 **[資料庫]** 。 以滑鼠右鍵按一下適當的資料庫（**drm**和**Adfs**），**指向 [工作]，然後**選取 [**備份**]。

5.  針對其餘的資料庫重複步驟4。

6.  請確定網路上的其他電腦可以存取資料庫的備份，或使用存放裝置，因為在進行遷移期間，將會需要它們來進行後續步驟。

現在您可以將資料庫複本儲存在安全的位置。 請記得經常備份您的資料庫。

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>將網域系統管理員、SQL、AD RMS 和/或 ADFS 服務帳戶新增至 SQL Server 2016

下列步驟將展示如何將各種服務帳戶新增至 SQL Server 2016，以協助從 Windows Server 2012 R2 環境遷移資料。 這會在嘗試存取內容並處理資料時，提供適當的許可權。

**若要將網域系統管理員、SQL、AD RMS 和/或 ADFS 服務帳戶新增至 SQL Server**

1.  以本機系統管理員帳戶的身分，登入具有 SQL Server 2016 的伺服器。

2.  依序按一下 [**開始**]、[**所有程式**]、[ **Microsoft SQL Server**]，然後按一下 [ **SQL Server Management Studio**]。

3.  在 [連線**到伺服器**] 視窗中，確認主控 AD RMS 資料庫的伺服器位於 [**伺服器名稱**] 方塊中，然後在 [驗證] 中按一下下拉式功能表，然後選取 [ **SQL Server 驗證**]。

4.  在 [**登**入] 欄位中，輸入本機系統管理員帳戶的名稱（例如 localadmin），然後提供適當的密碼，再按一下 **[連線]**。

5.  展開 [**安全性**]，然後以滑鼠**按右鍵 [** 登入]，然後從出現的內容功能表中選取 [**新增登**入]

6.  視窗出現之後，請在 [**登入名稱**] 欄位的 [網域系統管理員帳戶] 中輸入（例如， Contoso \\ ContosoAdmin）

7.  從左側導覽窗格中，選擇 [**伺服器角色**]。

8.  然後將伺服器角色下的 [**系統管理員**] 核取方塊標示為，然後按一下 **[確定]**。

9.  重新開機**SQL Server 管理**。

10. 在 [連線**到伺服器**] 視窗中，確認主控 AD RMS 資料庫的伺服器位於 [**伺服器名稱**] 方塊中，若要進行驗證，請按一下下拉式功能表並選取 [ **Windows 驗證**]，然後按一下 **[連接]**。

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>將 AD RMS 和 ADFS 資料庫還原至 SQL Server 2016

下列步驟將示範如何將前一個 SQL Server 實例的資料還原至新的2016實例。 這可讓新的 SQL 利用先前 AD RMS 和 ADFS 資料庫中的相關設定資料。

**將前一個 SQL Server 的資料還原至新的 SQL Server**

1.  使用適當的帳戶登入具有 SQL Server 2016 的伺服器。

2.  在左側流覽窗格中，以滑鼠右鍵按一下 [**資料庫**]，然後選取 [**還原資料庫**] 以開始還原程式。

3.  在 [**來源**] 底下，選擇 [**裝置**]，然後流覽在先前步驟中儲存資料庫檔案的位置。

4.  選取檔案之後，按一下 **[確定]**。

5.  請確認所有資料庫檔案都已新增，並按一下 **[確定]** 完成程式。

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>設定 Windows Server 2016 Active Directory 同盟服務（AD FS）

已部署 AD FS，以提供應用程式 AD RMS 單一登入（SSO）存取權。 它也已設定 AD RMS 的行動裝置延伸模組（MDE），可為使用者提供 Mac 和行動裝置支援。

下列各節提供有關您可能需要在 AD FS 部署上執行之作業工作的指引。

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>將 2016 AD FS 伺服器新增至伺服器陣列

您可以部署額外的 AD FS 伺服器，以支援 AD RMS 部署。 如果您要在 AD RMS 伺服器或其他應用程式的流量增加，或者您需要淘汰目前用於 AD FS 的其中一個伺服器時，可以選擇執行此動作。

**若要將 2016 AD FS 伺服器新增至伺服器陣列**

1.  在 Azure AD Connect 伺服器上，按兩下**Azure AD Connect**圖示以啟動 [Azure AD Connect wizard]。

2.  在 [歡迎使用] 頁面中，按一下 [**設定**]。

3.  在 [其他工作] 頁面中，按一下 [**部署其他同盟伺服器**]，然後按 **[下一步]**。

4.  在 [連線至 Azure AD] 頁面中，輸入具有全域系統管理許可權之帳戶的使用者名稱和密碼，然後按 **[下一步]**。

5.  在 [網域管理員認證] 頁面中，輸入具有網域系統管理員許可權之帳戶的使用者名稱和密碼，然後按 **[下一步]**。

6.  按一下 **[流覽]** ，然後選取使用 Azure AD Connect 設定 AD FS 伺服器陣列時所使用的憑證檔案。

7.  按一下 [**輸入密碼**] 以開啟 [憑證密碼] 對話方塊。

8.  在 [密碼] 欄位中輸入憑證的密碼，然後按一下 **[確定]**。

9.  按 [下一步]  。

10. 在 [AD FS 伺服器] 頁面中，輸入新 AD FS 伺服器的名稱或 IP 位址，**然後按一下 [新增]**。

11. 在 [準備設定] 頁面中，按一下 [**安裝**]。

12. 在 [安裝完成] 頁面中 **，按一下 [** 結束]。

#### <a name="raising-the-adfs-farm-behavior-level"></a>引發 ADFS 伺服器陣列行為層級

當部署的 ADFs 伺服器超過目前的環境層級（例如，在 Windows Server 2012 R2 上擁有 ADFS，然後新增 ADFS Windows Server 2016）時，將需要增加伺服器陣列行為層級。 這是為了確保環境使用的是最新的資訊和功能。

**若要提高 ADFS 伺服器陣列行為層級**

1.  流覽至 Windows Server 2016 ADFS。

2.  開啟系統管理 PowerShell 會話。

3.  輸入下列命令： ** \$ Credential = Get-Credential**

4.  隨即出現一個視窗，要求提供認證，請輸入網域系統管理員認證。

5.  然後，輸入此命令： **AdfsFarmBehaviorLevelRaise-認證 Credential \$ **

6.  系統會出現提示，詢問**您是否要繼續進行此作業？** 然後輸入**以**接受提示。

7.  在命令完成之後，伺服器陣列行為層級將會設定並備妥。

#### <a name="enabling-mobile-device-extension-logging"></a>啟用行動裝置延伸模組記錄

行動裝置延伸模組可以記錄它從終端使用者裝置接收的要求。 預設會停用記錄，建議您只在疑難排解案例中啟用記錄。 從行動裝置和桌上型電腦到啟動程式或取得使用授權的所有要求，都會記錄在 AD RMS 記錄資料庫或 Azure 儲存體帳戶中。 [MDE 記錄] 會建立兩個額外的資料表給 AD RMS 所使用的 SQL Server：用戶端 debug 記錄資料表和用戶端效能記錄資料表。

**啟用行動裝置延伸模組記錄**

1.  從 AD RMS 伺服器，以系統管理員身分開啟 Windows PowerShell。

2.  輸入下列命令，然後按**enter**： **import-module AdRmsAdmin**

3.  輸入下列命令，然後按**enter**： **PSDrive-Name AdrmsCluster-PsProvider AdRmsAdmin-Root https://localhost **

4.  輸入下列命令，然後按**enter**： **ItemProperty-Path AdrmsCluster： \\ -Name IsLoggingEnabled-Value \$ true**

如果您使用 MDE 記錄進行疑難排解，建議您在解決問題之後將它停用。

**停用行動裝置延伸模組記錄**

1.  從 AD RMS 伺服器，以系統管理員身分開啟 Windows PowerShell。

2.  輸入下列命令，然後按**enter**： **import-module AdRmsAdmin**

3.  輸入下列命令，然後按**enter**： **PSDrive-Name AdrmsCluster-PsProvider AdRmsAdmin-Root https://localhost **

4.  輸入下列命令，然後按**enter**： **ItemProperty-Path AdrmsCluster： \\ -Name IsLoggingEnabled-Value \$ false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>將 AD RMS 升級至 Windows Server 2016

下列各節提供有關如何將 Windows Server 2016 型 AD RMS 伺服器新增至目前 Windows Server 2012 R2 叢集中的指導方針。 伺服器將會新增到叢集中，而且資訊會複寫到其中，讓先前的 AD RMS 伺服器可以被取代，以釋放資源。

新增一部 Windows Server 2016 型 AD RMS 伺服器已加入至 AD RMS 叢集之後，以舊版 Windows 為基礎的所有節點將會變成非作用中。 完成此作業之後，您可以取消布建這些伺服器（例如，關機、重新規劃，或使用 Windows Server 2016 重新安裝以加入 AD RMS 叢集）。 

您可以將其他 AD RMS 伺服器部署到叢集，以支援 AD RMS 部署上的負載。 您也可以選擇在增加 AD RMS 伺服器流量的情況下執行此動作。

本指南並未涵蓋改變您在環境中使用的負載平衡機制所需的步驟，以排除您正在淘汰的伺服器，並包含您要新增至叢集的伺服器。 

#### <a name="adding-a-2016-ad-rms-server"></a>新增 2016 AD RMS 伺服器

如果您的 AD RMS 叢集使用硬體安全性模組，而非集中管理的金鑰做為其伺服器授權人憑證，則在安裝 AD RMS 之前，您必須先在伺服器上安裝軟體和其他 HSM 成品（例如金鑰和 configuragtion 檔）。 您也需要將 HSM 連線到伺服器，不論是實體或透過相關的網路設定。 請遵循您的 HSM 指引來進行這些步驟。 

**新增 2016 AD RMS 伺服器**

1.  在所需的 Windows Server 2016 部署上安裝 AD RMS 角色。

2.  安裝完成之後，請選取連結以**執行其他**設定。

3.  選取 [**加入現有的 AD RMS**叢集]，然後按 **[下一步]**。

4.  在 [**選取設定資料庫**] 頁面上，輸入 2016 SQL SERVER （FQDN）的 DNS 中指定的 CNAME。

5.  按一下第二行上的 [清單]，然後從下拉式**清單**中選取 **[defaultinstance** 。

6.  在 [設定**資料庫名稱**] 底下，選取下拉式功能表，然後選擇顯示的 drm 設定。 然後按一下 [下一步]。

7.  在 [**資料庫資訊**] 頁面上，于提供的欄位中輸入叢集金鑰密碼。 之後，按 **[下一步]**。

8.  在 wizard 的下一個頁面中，指定 AD RMS 服務帳戶並提供密碼，然後在驗證完成後按 **[下一步]** 。

9.  [叢集**網站**] 頁面出現之後，只需確定已選取適當的網站，然後按 **[下一步]**。

10. 在 [**選擇伺服器驗證憑證**] 頁面上，選取匯入的 SSL 憑證，然後按 **[下一步]**。

11. 按一下 [安裝]**** 可開始進行安裝。

12. 設定完成後，您必須登出再重新登入，才能管理 AD RMS。

13. 重新登入後，開啟**伺服器管理員**選取 [**工具**]，然後**Active Directory Rights Management**]。 [管理] 視窗應該會出現，並指出叢集具有叢集中的其他伺服器。

14. 如果 AD RMS 的行動裝置延伸模組已安裝在原始 AD RMS 叢集中，您也必須在更新的叢集節點中安裝 MDE。 依照 MDE 檔案中的指示，將 MDE 新增至您的 AD RMS 叢集。 此時，您可以重新規劃所有預先存在的節點，或將它們升級至 Windows Server 2016，然後使用上述相同的程式，將它們重新加入至 AD RMS 叢集。 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>設定 Windows Server 2016 Web 應用程式 Proxy （WAP）

下列各節提供您可能需要在 Web 應用程式 Proxy 部署上執行之作業工作的指引。 這是選擇性步驟，如果您透過其他機制將 AD RMS 發佈到網際網路，則不需要。 

#### <a name="adding-a-windows-server-2016-wap-server"></a>新增 Windows Server 2016 WAP 伺服器

您可以部署額外的 Web 應用程式 Proxy 伺服器，以支援 AD RMS 部署。 如果您想要將流量增加到 AD RMS 伺服器，或者您需要淘汰目前用於 Web 應用程式 Proxy 的其中一部伺服器，您可以選擇執行此動作。

**新增 2016 Web 應用程式 Proxy 伺服器**

1.  從您想要設定為 Web 應用程式 Proxy 的伺服器，流覽至伺服器管理員主控台，然後按一下 [**新增角色及功能**]。

2.  在 [**新增角色及功能] 嚮導**中，按 [**下一步]** ，直到到達 [伺服器角色選取] 畫面為止。

3.  在 [選取伺服器角色] 畫面上，選取 [**遠端存取**]，然後按 **[下一步]** ，直到您回到 [選取伺服器角色] 畫面為止。

4.  在 [選取伺服器角色] 畫面上，選取 [ **Web 應用程式 Proxy**]，按一下 [**新增功能**]，然後按 **[下一步]**。

5.  在 [確認安裝選項] 畫面上，按一下 [**安裝**]。

6.  安裝完成後，請按一下 [**關閉**]。

7.  現在是設定伺服器的時候了。 若要這麼做，請在 Web 應用程式 Proxy 伺服器上開啟 [遠端存取管理] 主控台。 開啟 [**開始**] 功能表，輸入**RAMgmtUI.exe**，然後選取應用程式。

8.  在導覽窗格中，按一下 [Web 應用程式 Proxy]****。

9.  在 [遠端存取管理] 主控台中，按一下 [**執行 Web 應用程式 Proxy 設定向導]**。 在嚮導中，按 **[下一步]**。

10. 在 [同盟伺服器] 畫面上，輸入 AD FS 伺服器的完整功能變數名稱（例如 adfs.contoso.com），然後輸入 AD FS 伺服器上系統管理員的認證。

11. 在 [AD FS Proxy 憑證] 畫面上，在 Web 應用程式 Proxy 伺服器上目前安裝的憑證清單中，選取要供 Web 應用程式 Proxy 用於 AD FS Proxy 的憑證，然後按 **[下一步]**。

12. 在 [確認] 畫面上，檢查設定，然後按一下 [**設定**]。

13. 設定完成後，按一下 [**關閉**]。

#### <a name="dns-configuration-for-2016-wap-server"></a>2016 WAP 伺服器的 DNS 設定

一旦將 Windows Server 2016 Web 應用程式 Proxy 伺服器放在原處，就必須進行一些 DNS 變更。 這將需要使用 GoDaddy 之類的服務，將 ADFS 和 AD RMS 服務指向 2016 WAP 伺服器。

**將 DNS 指向 WAP 伺服器**

1.  流覽至您提供者的網站（例如 GoDaddy）。

2.  進入 [網域管理]，然後按 [DNS 管理]。

3.  找出 ADFS 並 AD RMS 服務，並以 2016 WAP 伺服器的公用 IP 位址取代 [部分**點**]，然後 [**儲存**]。

4.  這些變更可能需要一些時間才能傳播，但在完成這段設定後，就會完成。

#### <a name="enabling-debugging-logs"></a>啟用調試記錄

詳細的記錄資訊可在 Web 應用程式 Proxy 伺服器上取得。 您可以使用事件檢視器來設定「高級」的調試記錄。 也可以選取記錄大小的其他設定，以協助確保分析對檢視器很有用。

**啟用 Web 應用程式 Proxy 的偵錯工具記錄檔**

1.  在 Web 應用程式 Proxy 上開啟**事件檢視器**主控台。

2.  展開 [ **Microsoft** ] 節點。

3.  展開 [ **Windows** ] 節點。

4.  開啟**Web 應用程式 Proxy**記錄檔。

5.  然後您就可以開啟**管理**記錄檔。

6.  開啟位於左上方的 [**動作**] 功能表，然後選取 [**屬性**]。

7.  在 [**一般**] 索引標籤下，選擇 [**啟用記錄**] 選項。

8.  最後，您可以自訂記錄大小上限，以及到達事件記錄檔大小上限時，會發生什麼情況。

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>設定 Windows Server 2016 服務的高可用性

下列各節提供在高可用性中設定 Windows Server 2016 環境所需操作工作的指引。

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>新增 2016 AD RMS 伺服器以取得高可用性

您可以部署額外的 AD RMS 伺服器，以設定高可用性。 當流量增加到 AD RMS 伺服器時，您可以選擇執行此動作。

**新增 2016 AD RMS 伺服器以取得高可用性**

1.  在所需的 Windows Server 2016 部署上安裝 AD RMS 角色。

2.  安裝完成之後，請選取連結以**執行其他**設定。

3.  選取 [**加入現有的 AD RMS**叢集]，然後按 **[下一步]**。

4.  在 [**選取設定資料庫**] 頁面上，輸入 2016 SQL SERVER （FQDN）的 DNS 中指定的 CNAME。

5.  按一下第二行上的 [清單]，然後從下拉式**清單**中選取 **[defaultinstance** 。

6.  在 [設定**資料庫名稱**] 底下，選取下拉式功能表，然後選擇顯示的 drm 設定。 然後按一下 [下一步]。

7.  在 [**資料庫資訊**] 頁面上，于提供的欄位中輸入叢集金鑰密碼。 之後，按 **[下一步]**。

8.  在 wizard 的下一個頁面中，指定 AD RMS 服務帳戶並提供密碼，然後在驗證完成後按 **[下一步]** 。

9.  [叢集**網站**] 頁面出現之後，只需確定已選取適當的網站，然後按 **[下一步]**。

10. 在 [**選擇伺服器驗證憑證**] 頁面上，選取匯入的 SSL 憑證，然後按 **[下一步]**。

11. 按一下 [安裝]**** 可開始進行安裝。

12. 設定完成後，您必須登出再重新登入，才能管理 AD RMS。

13. 重新登入後，開啟**伺服器管理員**選取 [**工具**]，然後**Active Directory Rights Management**]。 [管理] 視窗應該會出現，並指出叢集具有叢集中的其他伺服器。

14. 確認伺服器設定之後，請設定您的負載平衡服務，以平衡叢集中不同 AD RMS 伺服器之間的負載。

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>新增 Windows Server 2016 AD FS 伺服器以取得高可用性

您可以部署額外的 AD FS 伺服器，以設定高可用性。 當流量增加到 AD FS 伺服器時，您可以選擇執行此動作。 **注意：引發伺服器陣列行為層級之後，將會在 SQL Server 2016 （Adfs Configv3）中輸入新的資料庫專案，而且必須先刪除舊的設定資料庫，然後再繼續進行這些步驟。**

**新增 Windows Server 2016 AD FS 伺服器以取得高可用性**

1.  在所需的 Windows Server 2016 部署上安裝 AD RMS 角色。

2.  安裝完成之後，請選取連結以**設定這部伺服器上的 federation service**。

3.  在嚮導的 [歡迎使用] 區段中，選擇 [**將同盟伺服器新增至同盟伺服器**陣列] 選項，然後按 **[下一步]**。

4.  指定適當的系統管理員帳戶，然後按 **[下一步]**。

5.  在 [**指定伺服器**陣列] 頁面上，選擇 [**使用 SQL Server 指定現有伺服器陣列的資料庫位置**]，然後輸入資料庫主機名稱的 SQL 服務 CNAME，然後按 **[下一步]**。

6.  在 wizard 的 [**指定服務帳戶**] 區域中，輸入 AD FS 服務帳戶的認證，然後按 **[下一步]**。

7.  在 [**審查選項**] 中，按 **[下一步]**。

8.  當按鈕變成 [可用] 時，按一下 [**設定**]。

9.  設定之後，請重新開機電腦。

10. 確認伺服器設定之後，請視需要將 AD FS 伺服器負載平衡。

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>新增 Windows Server 2016 WAP 伺服器以取得高可用性

您可以部署額外的 WAP 伺服器來設定高可用性。 當流量增加到 AD RMS 伺服器時，您可以選擇執行此動作。

**新增 Windows Server 2016 Web 應用程式 Proxy 伺服器以取得高可用性**

1.  從您想要設定為 Web 應用程式 Proxy 的伺服器，流覽至伺服器管理員主控台，然後按一下 [**新增角色及功能**]。

2.  在 [**新增角色及功能] 嚮導**中，按 [**下一步]** ，直到到達 [伺服器角色選取] 畫面為止。

3.  在 [選取伺服器角色] 畫面上，選取 [**遠端存取**]，然後按 **[下一步]** ，直到您回到 [選取伺服器角色] 畫面為止。

4.  在 [選取伺服器角色] 畫面上，選取 [ **Web 應用程式 Proxy**]，按一下 [**新增功能**]，然後按 **[下一步]**。

5.  在 [確認安裝選項] 畫面上，按一下 [**安裝**]。

6.  安裝完成後，請按一下 [**關閉**]。

7.  現在是設定伺服器的時候了。 若要這麼做，請在 Web 應用程式 Proxy 伺服器上開啟 [遠端存取管理] 主控台。 開啟 [**開始**] 功能表，輸入**RAMgmtUI.exe**，然後選取應用程式。

8.  在導覽窗格中，按一下 [Web 應用程式 Proxy]****。

9.  在 [遠端存取管理] 主控台中，按一下 [**執行 Web 應用程式 Proxy 設定向導]**。 在嚮導中，按 **[下一步]**。

10. 在 [同盟伺服器] 畫面上，輸入 AD FS 伺服器的完整功能變數名稱（例如 adfs.contoso.com），然後輸入 AD FS 伺服器上系統管理員的認證。

11. 在 [AD FS Proxy 憑證] 畫面上，在 Web 應用程式 Proxy 伺服器上目前安裝的憑證清單中，選取要供 Web 應用程式 Proxy 用於 AD FS Proxy 的憑證，然後按 **[下一步]**。

12. 在 [確認] 畫面上，檢查設定，然後按一下 [**設定**]。

13. 設定完成後，按一下 [**關閉**]。

14. 確認伺服器設定之後，請對 DMZ 中的 WAP 伺服器進行負載平衡。

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>為 Always On 高可用性新增 SQL Server 2016 節點

您可以將額外的 SQL server 部署到安裝程式 Always On 高可用性。 當流量增加到 AD RMS 伺服器時，您可以選擇執行此動作。 **注意：請確定這兩部 SQL Server 都已開啟輸入埠5022。**

**為 Always On 高可用性新增 SQL server 2016 伺服器**

1.  從您想要設定為額外 SQL Server 2016 伺服器的伺服器，流覽至伺服器管理員主控台，然後按一下 [**新增角色及功能**]。

2.  按 [**下一步**]，直到 [**選取功能**] 對話方塊。

3.  選取 [**容錯移轉**叢集] 核取方塊。 **注意：請同時針對原始 SQL server 2016 伺服器執行此步驟，如此一來，這兩個 SQL 伺服器都具有容錯移轉叢集功能。**

4.  按一下 [**安裝**] 以安裝容錯移轉叢集功能。

5.  現在，開啟**伺服器管理員**並選取 [**工具**]，然後**容錯移轉叢集管理員**]。

6.  在左側功能表窗格中，以滑鼠右鍵按一下**容錯移轉叢集管理員**，然後選取 [**建立**叢集]

7.  這會開啟 [**建立叢集嚮導]**。

8.  流覽將用於 Always On 高可用性的 SQL server 2016 伺服器，並在中輸入它們，然後按 **[下一步]**。

9.  您會收到驗證警告。 選取 **[是]** 以驗證叢集節點，然後按 **[下一步]**。

10. 在 [**測試選項**] 頁面上，選取 [**執行所有測試**] 選項，然後按 **[下一步]。**

11. **注意：叢集驗證嚮導應該會傳回數個警告訊息，特別是當您不會使用共用存放裝置時。另外，如果您發現任何錯誤訊息，在建立 Windows Server 容錯移轉叢集之前，您必須先加以修正**。

12. 在 [**用於管理叢集的存取點**] 對話方塊中，輸入 Windows Server 容錯移轉叢集的 [叢集名稱] 和 [虛擬 IP 位址]，然後按 **[下一步]**。

13. 確認 [**摘要**] 中的設定為 [成功]，然後按一下 **[完成]**。

14. 回到**容錯移轉叢集管理員，** 以滑鼠右鍵按一下您的叢集，然後選取 [**其他動作**]，再選擇 [**設定叢集仲裁設定**]。

15. 按 **[下一步]** ，然後選取 [**選取仲裁見證**] 的選項，然後再按 **[下一步]** 。

16. 在 [**選取仲裁見證**] 頁面上，選取 [**設定檔案共用見證**] 選項。 然後按一下 [下一步]。

17. 選取 [**流覽]** 並找出您想要在 [檔案共用路徑] 對話方塊中使用的檔案共用路徑。 按 [下一步]  。

18. 在 [確認] 頁面中按一下 [ **下一步**]。

19. 在 [摘要] 頁面中按一下 [ **完成**]。

20. 現在，開啟 [**開始**] 功能表並搜尋**SQL Server 組態管理員**。

21. 以滑鼠右鍵按一下 SQL Server 名稱，然後選取 [**屬性**]。

22. 在 [屬性] 對話方塊中，選取 [ **AlwaysOn 高可用性**] 索引標籤。勾選 [**啟用 AlwaysOn 可用性群組**] 核取方塊。 按一下 [確定]  。 **注意：在這兩部 SQL server 2016 伺服器上執行這項操作。**

23. 然後重新啟動 SQL Server 服務。

24. 現在，開啟 [**開始**] 功能表並搜尋**SQL Server Management Studio** ，然後從左側流覽窗格中，以滑鼠右鍵按一下 [**可用性群組**]，再按一下 [**新增可用性群組嚮導]** ，然後按 **[下一步]**。

25. 在 [**指定可用性組名**] 頁面中，挑選組名（例如 SQLAvailabilityGroup2016）。 然後按一下 [下一步]。

26. 在 [**選取資料庫**] 區段下，指定資料庫。 接著按 [下一步]。 **注意：某些資料庫可能需要重新備份或進入完整復原模式**。

27. 在 [**指定複本**] 頁面上，按一下 [**新增複本**] 按鈕，然後挑選您的其他 2016 SQL Server。

28. 新增其他伺服器之後，請按一下核取方塊，並將次要伺服器設定為可讀取的次要複本。

29. 流覽至 [**端點**] 索引標籤，然後**按一下 [重新**整理] 選項。 此外，在此同時，請在主要和次要節點上進行滾動並確保相同的服務帳戶。

30. 現在，選擇 [**備份喜好**設定] 索引標籤，然後選取 [**慣用次要**] 選項。

31. 移**至 [接聽**程式] 索引標籤。

32. 指定名稱（例如 SQLListener），並確定埠為**1433** ，然後按 **[下一步]**。

33. 在嚮導的 [**選取初始資料同步**處理] 頁面中，選擇 [**完整**] 選項，並指定所有 SQL server 可存取的網路位置，然後按 **[下一步]**。

34. 最後，按一下 **[完成]** ，程式就會完成。

### <a name="decommission-windows-server-2012-r2-nodes"></a>解除委任 Windows Server 2012 R2 節點

下列各節提供在將 AD RMS 叢集成功升級至 Windows Server 2016 之後，您可能需要移除 Windows Server 2012 R2 伺服器之操作工作的指引。

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>移除 Windows Server 2012 R2 AD RMS 伺服器

升級之後，您可以移除不必要的 AD RMS 伺服器。 當您需要解除委任 AD RMS 伺服器時，可以選擇執行此動作。

**若要移除** **Windows Server 2012 R2 AD RMS 伺服器**

1.  在伺服器管理員的 Windows Server 2012 R2 AD RMS 伺服器上，從右上方的功能表中選取 [**管理**]，然後選擇 [**移除角色及功能**]。

2.  [**移除角色及功能] Wizard**隨即開啟，並在 [**開始之前**] 畫面上，按 **[下一步]**。

3.  在 [**伺服器選取**] 畫面上，按 **[下一步]**。

4.  在 [**伺服器角色**] 畫面上，移除**Active Directory Rights Management Services**旁的核取記號，然後按 **[下一步]**。

5.  在 [**功能**] 畫面上，按 **[下一步]**。

6.  在 [**確認**] 畫面上，按一下 [**移除**]。

7.  完成後，請重新開機伺服器。

8.  您現在可以關閉此伺服器，並視需要重新配置資源。
