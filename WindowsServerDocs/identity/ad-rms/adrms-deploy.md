---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: 升級至 Windows Server 2016 的 AD RMS
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: ce058a2885315c84d2c1c6701ad2801790d3c590
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814071"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>升級至 Windows Server 2016 的 AD RMS

## <a name="introduction"></a>簡介

Active Directory Rights Management Services (AD RMS) 是保護機密文件和電子郵件的 Microsoft 服務。 不同於傳統的保護方法，例如防火牆和 Acl，AD RMS 加密和保護是持續性檔案所前往的地方無論在傳輸的方式。 

本文件提供從 Windows Server 2012 R2 與 SQL Server 2012 移轉到 Windows Server 2016 和 SQL Server 2016 的指引。 相同的程序可用來從較舊但支援版本的 AD RMS 移轉。
請注意，Active Directory Rights Management Services 不再進行開發，而最新功能的客戶應考慮移轉至[Azure Information Protection](https://azure.microsoft.com/services/information-protection/)，提供更多將組完整的更完整的裝置與應用程式支援的功能。 

如需有關從 AD RMS 移轉至 Azure Information Protection 而不必重新保護您的內容資訊，請參閱[Azure 資訊保護移轉文件](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms)。

## <a name="about-the-environment-used-in-this-guide"></a>關於本指南中使用的環境

AD FS 是 AD RMS 安裝的選用元件。 在本指南中，會假設使用 ADFS。 如果 ADFS 尚未在您的環境中使用以支援 AD RMS 的使用者，您可以跳至 ADFS 所參考的所有步驟。

在本指南中，SQL Server 會升級到 SQL Server 2016 藉由執行平行的安裝，並透過移動資料庫，透過備份。 或者，如果您可以在 SQL Server 2016 就地升級您的 AD RMS 和 ADFS 資料庫伺服器，您可以移至本文件中的下一節之後需要完成，而不需要遵循本節中的步驟。  

## <a name="installation"></a>安裝

### <a name="configuring-sql-server-2016"></a>設定 SQL Server 2016

下列章節詳細資料實作的工作直接相關的 SQL Server 2016 組態。 本指南著重於使用 伺服器管理員和 SQL Server Management Studio 來完成這些工作。

必須完成下列步驟，在 SQL Server 2016 安裝。 根據您組織的標準作法和原則的適當硬體上安裝 SQL Server 2016。 

#### <a name="preparing-the-sql-server"></a>準備 SQL Server

下一節詳細說明如何準備 SQL Server，讓它可以升級 AD RMS 平台，若要使用 Windows Server 2016 中的其他服務前請先升級至 SQL Server 2016。

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>適用於 SQL Server 2016 加入 DNS CNAME

CNAME 用來協助確保，Windows Server 2016 安裝程式將會出現適當的資料因為它會指向新的 SQL Server 2016。 **注意：如果已經使用 ADFS 與 AD RMS 服務的 CNAME，您可以移至下一個步驟。**


**若要將 SQL Server 2016 的 CNAME 新增至 DNS**

1.  Windows Server 2012 R2 網域控制站與網域系統管理員認證登入。

2.  開啟伺服器管理員。

3.  按一下 **工具**，然後選取**DNS**以開啟 DNS 管理員。

4.  從左側的導覽窗格中，展開 DC，然後開啟**正向對應區域**。

5.  開啟適當的網域資源，然後在右方檢視 窗格中以滑鼠右鍵按一下並選取**新別名 (CNAME)** 開始建立 CNAME。

6.  區分它與其他的邏輯名稱的別名名稱輸入中可能存在 （ex. SQLADRMS 或 SQLADFS）

7.  輸入名稱之後，提供將會是新的 SQL Server 2016 伺服器的目標主機的 FQDN。 （例如。 SQL2016.contoso.com)

8.  一旦輸入所有資訊後，按一下**確定**。

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>AD RMS 和 ADFS 資料庫備份

AD RMS 和 ADFS 的資料庫會保存重要的資訊需要 AD RMS，例如伺服器授權人憑證、 權限原則範本、 ADFS 組態資料和記錄資訊的公開金鑰。 不含這些資料庫中，用戶端無法發出授權，才能使用受保護的內容，在其他問題。

一個資料庫，AD RMS 設定資料庫會被視為最重要的因為它會儲存 SLC，權限原則範本、 使用者的金鑰和組態資訊。 因此，雖然您應該注意的所有 AD RMS 和 ADFS 的資料庫備份，您應該規劃定期備份組態資料庫。

記錄資料庫會儲存到 AD RMS 叢集的憑證與使用授權的使用者要求的相關資訊。 此資料庫的備份策略應該根據公司原則，來保留這種類型的資訊。

目錄服務資料庫不是 AD RMS 功能的關鍵，資料庫的最新的資料遺失時，將資訊重新填入，AD RMS 伺服器收到憑證要求，並使用授權。 您不需要定期備份此資料庫，但您需要有至少一份資料庫，因為它原本設定之後部署 AD RMS。

**若要將 AD RMS 和/或 ADFS 的資料庫與 Microsoft SQL Server 備份**

1.  SQL 2012 Windows Server 2012 R2 AD RMS 資料庫伺服器登入。

2.  按一下 **開始**，按一下**所有程式**，按一下  **Microsoft SQL Server**，然後按一下**SQL Server Management Studio**。

3.  在 [**連接到伺服器**] 視窗中，確認裝載 AD RMS 資料庫的伺服器是在**伺服器名稱**方塊，然後按一下**Connect**。

4.  依序展開**資料庫**。 以滑鼠右鍵按一下適當的資料庫 (**DRM**並**Adfs**)，指向**工作**，然後選取**備份**。

5.  針對剩餘的資料庫重複步驟 4。

6.  請確定網路或使用存放裝置，因為它們會在移轉期間所需的後續步驟的其他電腦可以存取資料庫的備份。

現在您可以在安全的位置來儲存資料庫複本。 請務必經常備份資料庫。

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>新增網域系統管理員、 SQL、 AD RMS，及/或 ADFS 服務 SQL Server 2016 的帳戶

下列步驟將會展示如何將各種不同的服務帳戶新增至 SQL Server 2016，以便協助您從 Windows Server 2012 R2 環境移轉資料。 嘗試存取的內容及處理資料時，這會提供適當的權限。

**若要將網域系統管理員、 SQL、 AD RMS，及/或 ADFS 服務帳戶新增至 SQL Server**

1.  本機系統管理員帳戶登入 SQL Server 2016 的伺服器。

2.  按一下 **開始**，按一下**所有程式**，按一下  **Microsoft SQL Server**，然後按一下**SQL Server Management Studio**。

3.  在 [**連接到伺服器**] 視窗中，確認裝載 AD RMS 資料庫的伺服器處於**伺服器名稱**方塊則驗證中，按一下下拉式選單，然後選取**SQL Server驗證**。

4.  在 **登入**欄位中，輸入本機系統管理員帳戶 （例如名稱 localadmin) 再提供適當的密碼，然後按一下  **Connect**。

5.  依序展開**安全性**然後以滑鼠右鍵按一下**登入**，然後選取**新登入**從出現的內容功能表。

6.  視窗出現時輸入網域系統管理員帳戶中一旦**登入名稱**欄位 （例如 Contoso\\ContosoAdmin)

7.  從左側的導覽窗格中，選擇**伺服器角色**。

8.  然後標示核取方塊**sysadmin**下方伺服器角色，然後按一下**確定**。

9.  重新啟動**SQL Server 管理**。

10. 在 **連接到伺服器** 視窗中，確認裝載 AD RMS 資料庫的伺服器處於**伺服器名稱**方塊則驗證中，按一下下拉式選單，然後選取**Windows驗證**，按一下  **Connect**。

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>將 AD RMS 和 ADFS 資料庫還原到 SQL Server 2016

下列步驟將會展示如何從先前的 SQL Server 執行個體的資料還原到新的 2016年執行個體。 這可讓新的 SQL，使用從先前的 AD RMS 和 ADFS 資料庫的相關組態資料。

**若要從先前的 SQL Server 的將資料還原到新的 SQL Server**

1.  使用 SQL Server 2016 的伺服器，使用適當的帳戶登入。

2.  從左側的導覽窗格中，以滑鼠右鍵按一下**資料庫**，然後選取**Restore Database**開始還原程序。

3.  底下**來源**選擇**裝置**然後瀏覽資料庫檔案已在先前步驟中儲存的位置。

4.  一旦檔案已選取，按一下**確定**。

5.  請確定所有資料庫檔案已加入，並完成程序，依序按一下**確定**。

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>設定 Windows Server 2016 Active Directory Federation Services (AD FS)

AD FS 部署 AD rms，做為應用程式提供單一登入 (SSO) 存取。 它也已經設定使用 AD RMS 行動裝置延伸模組 (MDE)，可讓 Mac 和行動裝置支援的使用者。

下列各節將提供您可能需要在 AD FS 部署上執行的操作工作的指引。

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>2016 AD FS 伺服器加入伺服器陣列

您可以部署額外的 AD FS 伺服器，以支援 AD RMS 部署。 您可以選擇執行此動作發生時增加的流量，AD RMS 伺服器或其他應用程式，或如果您要淘汰的其中一個目前正用於 AD FS 伺服器。

**若要將 2016 AD FS 伺服器新增至伺服器陣列**

1.  在 Azure AD Connect 伺服器中，按兩下**Azure AD Connect**圖示以啟動 Azure AD Connect 精靈。

2.  在 [歡迎使用] 頁面上，按一下**設定**。

3.  在 其他工作 頁面中，按一下**部署其他同盟伺服器**，然後按一下**下一步**。

4.  在 [連接到 Azure AD 頁面中，輸入使用者名稱和密碼具有全域系統管理權限的帳戶，然後按一下**下一步]** 。

5.  中的網域系統管理員認證 頁面中，輸入使用者名稱和網域系統管理員權限的帳戶的密碼，然後按一下**下一步**。

6.  按一下 **瀏覽**和憑證檔案時所使用的選取設定使用 Azure AD Connect 之 AD FS 伺服器陣列。

7.  按一下 **輸入密碼**以開啟 憑證密碼 對話方塊。

8.  在 [密碼] 欄位中輸入憑證的密碼，然後按一下**確定**。

9.  按一下 [下一步]  。

10. 在 [AD FS 伺服器] 頁面中，輸入名稱或新的 AD FS 伺服器的 IP 位址，然後按一下**新增**。

11. 在 [準備設定] 頁面上，按一下**安裝**。

12. 在安裝完成 頁面上，按一下**結束**。

#### <a name="raising-the-adfs-farm-behavior-level"></a>引發的 ADFS 伺服器陣列行為層級

當部署超過目前的環境層級，例如具有 Windows Server 2012 R2 上的 ADFS，然後再新增 Windows Server 2016 執行 ADFS、 伺服器陣列行為層級，將會需要增加的 ADFs 伺服器。 這樣才能確保環境都使用最新的資訊和函式。

**若要引發 ADFS 伺服器陣列行為層級**

1.  瀏覽至 microsoft Windows Server 2016 ADFS。

2.  開啟系統管理員 PowerShell 工作階段。

3.  輸入下列命令： **\$認證 = Get-credential**

4.  要求認證，會出現一個視窗，輸入網域系統管理員認證。

5.  然後輸入下列命令：**Invoke-AdfsFarmBehaviorLevelRaise -Credential \$cred**

6.  會出現提示，詢問**您要繼續進行這項作業嗎？** 然後輸入  先接受提示。

7.  命令完成之後，將會設定伺服陣列行為層級，並準備。

#### <a name="enabling-mobile-device-extension-logging"></a>啟用行動裝置擴充功能記錄

行動裝置延伸模組可以記錄它接收來自終端使用者裝置的要求。 預設會停用記錄，我們建議您僅啟用 登入疑難排解案例。 來自行動裝置和桌面的電腦，以啟動程序，或取得使用授權的所有要求都會都記錄在 Azure 儲存體帳戶的 AD RMS 記錄資料庫。 MDE 記錄將會建立額外的兩個資料表，以使用 AD rms SQL Server： 用戶端偵錯記錄檔和用戶端效能記錄檔資料表。

**若要啟用行動裝置延伸模組記錄**

1.  從 AD RMS 伺服器，請以系統管理員身分開啟 Windows PowerShell。

2.  輸入下列命令，然後按**Enter**:**匯入模組 AdRmsAdmin**

3.  輸入下列命令，然後按**Enter**:**New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  輸入下列命令，然後按**Enter**:**Set-itemproperty-路徑 AdrmsCluster:\\ -IsLoggingEnabled 名稱-值\$，則為 true**

如果您使用 MDE 記錄進行疑難排解，我們建議解決問題之後停用它。

**若要停用行動裝置延伸模組記錄**

1.  從 AD RMS 伺服器，請以系統管理員身分開啟 Windows PowerShell。

2.  輸入下列命令，然後按**Enter**:**匯入模組 AdRmsAdmin**

3.  輸入下列命令，然後按**Enter**:**New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  輸入下列命令，然後按**Enter**:**Set-itemproperty-路徑 AdrmsCluster:\\ -IsLoggingEnabled 名稱-值\$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>升級至 Windows Server 2016 的 AD RMS

下列各節提供如何將 Windows Server 2016 為基礎的 AD RMS 伺服器加入至目前的 Windows Server 2012 R2 叢集的指引。 伺服器會新增到叢集中，資訊將會複寫到它，使先前的 AD RMS 伺服器可以釋出資源已被取代。

您加入一個 Windows Server 2016 為基礎的 AD RMS 伺服器已新增至您的 AD RMS 叢集之後，根據舊版 Windows 的所有節點將會都變成非使用中。 此動作完成之後可以取消佈建這些伺服器 （例如關閉、 重新設定或重新安裝與加入 AD RMS 叢集的 Windows Server 2016）。 

您可以部署額外的 AD RMS 伺服器叢集以支援 AD RMS 部署上的負載。 您也可以選擇執行此動作發生時增加的流量，AD RMS 伺服器。

本指南未涵蓋改變的負載平衡的機制，您也可以使用您的環境中來排除已淘汰的伺服器，並包含您要新增到叢集中的項目所需的步驟。 

#### <a name="adding-a-2016-ad-rms-server"></a>新增 2016 AD RMS 伺服器

如果您的 AD RMS 叢集使用硬體安全性模組而不集中管理的金鑰其伺服器授權人憑證，您必須在伺服器上安裝的軟體及其他 HSM 成品 （例如索引鍵和若在組態檔案），然後再安裝 AD RMS。 您也必須將 HSM 連線到伺服器，可以是實體或相關的網路組態。 請遵循這些步驟您 HSM 的指引。 

**若要新增 2016 AD RMS 伺服器**

1.  安裝 AD RMS 角色上所需的 Windows Server 2016 部署。

2.  安裝完成後，選取連結至**執行其他設定**。

3.  選取 [**加入現有的 AD RMS 叢集**然後按一下**下一步]** 。

4.  在 **選取組態資料庫**頁面上，輸入 2016 SQL server (FQDN) 的 DNS 中指定的 CNAME。

5.  按一下 **清單**第二個的線條，然後選取**DefaultInstance**從下拉式清單。

6.  底下**設定資料庫的名稱**，選取下拉式選單，然後選擇出現的 DRM 組態。 然後按一下 [下一步]  。

7.  在 **資料庫資訊**頁面上，提供的欄位中輸入叢集金鑰密碼。 接下來，按一下**下一步**。

8.  在精靈的下一個頁面中，指定 AD RMS 服務帳戶，並提供密碼，然後按一下**下一步**之後已經過驗證。

9.  一次 **[叢集網站**頁面出現時，只需確定已選取適當的網站，然後按一下**下一步]** 。

10. 在 **選擇伺服器驗證憑證**頁面上，選取 匯入的 SSL 憑證，然後按一下**下一步**。

11. 按一下 [安裝]  可開始進行安裝。

12. 完成設定之後，您必須登入的關閉 AD RMS 的管理。

13. 一旦登入的上一步，開啟**伺服器管理員**選取**工具**，然後**Active Directory Rights Management**。 [管理] 視窗應該會出現，並指出叢集有額外的伺服器，在叢集中。

14. 14. 如果 AD RMS 行動裝置延伸模組已安裝在原始的 AD RMS 叢集，您必須安裝 MDE 更新後的叢集節點中。 遵循 MDE 文件中的指示，將 MDE 新增至您的 AD RMS 叢集。 到目前為止，您可以重新規劃所有預先存在的節點，或將它們升級到 Windows Server 2016 並重新將其加入上面所述的相同程序的 AD RMS 叢集。 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>設定 Windows Server 2016 Web 應用程式 Proxy (WAP)

下列各節將提供您可能需要在您的 Web 應用程式 Proxy 部署上執行的操作工作的指引。 這是選擇性的步驟，不需要 如果您要透過其他機制網際網路發佈 AD RMS。 

#### <a name="adding-a-windows-server-2016-wap-server"></a>新增 Windows Server 2016 WAP 伺服器

您可以部署額外的 Web 應用程式 Proxy 伺服器，以支援 AD RMS 部署。 您可以選擇執行此動作發生時增加的流量，AD RMS 伺服器，或如果您要淘汰的其中一個目前正用於 Web 應用程式 Proxy 伺服器。

**若要新增的 2016 Web 應用程式 Proxy 伺服器**

1.  從您想要安裝的 Web 應用程式 proxy 伺服器，請在瀏覽至 伺服器管理員 主控台，然後按一下 **新增角色及功能**。

2.  在 [**新增角色及功能精靈**，按一下**下一步]** 直到到達 [伺服器角色選取] 畫面。

3.  在 選取伺服器角色 畫面中，選取**遠端存取**，然後按一下**下一步**直到您在 選取伺服器角色 畫面。

4.  在 選取伺服器角色 畫面中，選取**Web 應用程式 Proxy**，按一下**新增功能**，然後按一下**下一步**。

5.  在 [確認安裝選項] 畫面中，按一下**安裝**。

6.  一旦安裝完成後，按一下**關閉**。

7.  現在就可以開始設定伺服器。 若要這樣做，請開啟 Web 應用程式 Proxy 伺服器上的遠端存取管理主控台。 開啟**開始**功能表中，輸入**RAMgmtUI.exe**，然後選取 應用程式。

8.  在 [導覽] 窗格中，按一下**Web 應用程式 Proxy**。

9.  在遠端存取管理主控台中按一下**執行 Web 應用程式 Proxy 組態精靈**。 一次在精靈中，按一下**下一步**。

10. 在 [同盟伺服器] 畫面上輸入 （例如 AD FS 伺服器的完整的網域名稱 adfs.contoso.com)，然後在 AD FS 伺服器上的系統管理員輸入認證。

11. 在 AD FS Proxy 憑證] 畫面中的 [目前安裝在 Web 應用程式 Proxy 伺服器上的憑證清單中選取 [AD FS proxy，Web 應用程式 Proxy 所使用的憑證，然後按一下**下一步]** 。

12. 在確認畫面上，檢閱設定，然後按一下 **設定**。

13. 設定完成後，按一下**關閉**。

#### <a name="dns-configuration-for-2016-wap-server"></a>2016 的 DNS 設定 WAP 伺服器

一旦在就地已暫停的 Windows Server 2016 Web 應用程式 Proxy 伺服器，某些 DNS 變更需要進行。 這將需要在 2016 WAP 伺服器使用的服務，例如 GoDaddy 指向 ADFS，AD RMS 服務。

**使其指向在 WAP 伺服器的 DNS**

1.  瀏覽至您的提供者網站 （例如。 GoDaddy)。

2.  移至 定義域管理 和 DNS 管理。

3.  尋找 ADFS 與 AD RMS 服務，並取代**指向**2016 WAP 伺服器的公用 IP 位址的部分並**儲存**。

4.  所做的變更可能需要時間才能傳播，但之後就會完成這項設定。

#### <a name="enabling-debugging-logs"></a>啟用偵錯記錄檔

使用 Web 應用程式 Proxy 伺服器上記錄的詳細的資訊。 您可以設定使用事件檢視器的進階偵錯記錄。 其他設定以協助確保分析有用的檢視器，也選取記錄檔的大小。

**針對 Web 應用程式 Proxy 啟用偵錯記錄檔**

1.  開啟**事件檢視器**Web Application Proxy 上的主控台。

2.  依序展開**Microsoft**節點。

3.  依序展開**Windows**節點。

4.  開啟**Web 應用程式 Proxy**記錄檔。

5.  您接著可以開啟**Admin**記錄檔。

6.  開啟**動作**功能表上，位於左上方，然後選取**屬性**。

7.  底下**一般**索引標籤上，選擇**Enable Logging**。

8.  最後，您就能夠自訂記錄檔大小上限和最大事件記錄檔大小到達時，會發生什麼事。

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Windows Server 2016 服務設定高可用性

下列各節將提供您可能需要安裝程式的 Windows Server 2016 環境中的高可用性的操作工作的指引。

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>新增 2016 AD RMS 伺服器以獲得高可用性

您可以部署額外的 AD RMS 伺服器，以設定高可用性。 您可以選擇執行此動作發生時增加的流量，AD RMS 伺服器。

**若要新增 2016 AD RMS 伺服器以獲得高可用性**

1.  安裝 AD RMS 角色上所需的 Windows Server 2016 部署。

2.  安裝完成後，選取連結至**執行其他設定**。

3.  選取 [**加入現有的 AD RMS 叢集**然後按一下**下一步]** 。

4.  在 **選取組態資料庫**頁面上，輸入 2016 SQL server (FQDN) 的 DNS 中指定的 CNAME。

5.  按一下 **清單**第二個的線條，然後選取**DefaultInstance**從下拉式清單。

6.  底下**設定資料庫的名稱**，選取下拉式選單，然後選擇出現的 DRM 組態。 然後按一下 [下一步]  。

7.  在 **資料庫資訊**頁面上，提供的欄位中輸入叢集金鑰密碼。 接下來，按一下**下一步**。

8.  在精靈的下一個頁面中，指定 AD RMS 服務帳戶，並提供密碼，然後按一下**下一步**之後已經過驗證。

9.  一次 **[叢集網站**頁面出現時，只需確定已選取適當的網站，然後按一下**下一步]** 。

10. 在 **選擇伺服器驗證憑證**頁面上，選取 匯入的 SSL 憑證，然後按一下**下一步**。

11. 按一下 [安裝]  可開始進行安裝。

12. 完成設定之後，您必須登入的關閉 AD RMS 的管理。

13. 一旦登入的上一步，開啟**伺服器管理員**選取**工具**，然後**Active Directory Rights Management**。 [管理] 視窗應該會出現，並指出叢集有額外的伺服器，在叢集中。

14. 確認伺服器安裝程式之後, 將負載平衡服務設定為平衡叢集中不同的 AD RMS 伺服器之間的負載。

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>新增 Windows Server 2016 AD FS 伺服器的高可用性

您可以部署額外的 AD FS 伺服器，以設定高可用性。 您可以選擇執行此動作發生時增加 AD FS 伺服器的流量。 **注意： 引發後的伺服器陣列行為層級，新的資料庫項目將會輸入 SQL Server 2016 (Adfs Configv3)，必須刪除舊的組態資料庫，才能繼續這些步驟。**

**若要新增的 Windows Server 2016 AD FS 伺服器的高可用性**

1.  安裝 AD RMS 角色上所需的 Windows Server 2016 部署。

2.  安裝完成後，選取連結至**這部伺服器上設定 federation service**。

3.  在精靈的 歡迎使用 區段中，選擇**同盟伺服器新增至同盟伺服器陣列**，然後按一下**下一步**。

4.  指定適當的系統管理員帳戶，然後按一下**下一步**。

5.  上**指定的伺服器陣列**頁面上，選擇**指定現有的伺服器陣列使用 SQL Server 的資料庫位置**然後輸入 CNAME 資料庫主機名稱的 SQL 服務，然後按一下 **下一步**.

6.  底下**指定服務帳戶**區域精靈 中，輸入 AD FS 服務帳戶的認證，然後按一下 **下一步**。

7.  在 [**檢閱選項**，按一下**下一步]** 。

8.  按一下 **設定**時按鈕會變成可用。

9.  完成設定後，重新啟動電腦。

10. 後確認 AD FS 伺服器所需的伺服器安裝程式，而負載平衡。

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>新增 Windows Server 2016 WAP 伺服器的高可用性

您可以部署額外的 WAP 伺服器，以設定高可用性。 您可以選擇執行此動作發生時增加的流量，AD RMS 伺服器。

**將 Windows Server 2016 Web 應用程式 Proxy 伺服器的高可用性**

1.  從您想要安裝的 Web 應用程式 proxy 伺服器，請在瀏覽至 伺服器管理員 主控台，然後按一下 **新增角色及功能**。

2.  在 [**新增角色及功能精靈**，按一下**下一步]** 直到到達 [伺服器角色選取] 畫面。

3.  在 選取伺服器角色 畫面中，選取**遠端存取**，然後按一下**下一步**直到您在 選取伺服器角色 畫面。

4.  在 選取伺服器角色 畫面中，選取**Web 應用程式 Proxy**，按一下**新增功能**，然後按一下**下一步**。

5.  在 [確認安裝選項] 畫面中，按一下**安裝**。

6.  一旦安裝完成後，按一下**關閉**。

7.  現在就可以開始設定伺服器。 若要這樣做，請開啟 Web 應用程式 Proxy 伺服器上的遠端存取管理主控台。 開啟**開始**功能表中，輸入**RAMgmtUI.exe**，然後選取 應用程式。

8.  在 [導覽] 窗格中，按一下**Web 應用程式 Proxy**。

9.  在遠端存取管理主控台中按一下**執行 Web 應用程式 Proxy 組態精靈**。 一次在精靈中，按一下**下一步**。

10. 在 [同盟伺服器] 畫面上輸入 （例如 AD FS 伺服器的完整的網域名稱 adfs.contoso.com)，然後在 AD FS 伺服器上的系統管理員輸入認證。

11. 在 AD FS Proxy 憑證] 畫面中的 [目前安裝在 Web 應用程式 Proxy 伺服器上的憑證清單中選取 [AD FS proxy，Web 應用程式 Proxy 所使用的憑證，然後按一下**下一步]** 。

12. 在確認畫面上，檢閱設定，然後按一下 **設定**。

13. 設定完成後，按一下**關閉**。

14. 確認伺服器安裝程式之後, 提供負載平衡在 DMZ 中的 WAP 伺服器。

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>將 SQL Server 2016 節點新增為 Always On 高可用性

您可以部署額外的 SQL 伺服器，以設定 Always On 高可用性。 您可以選擇執行此動作發生時增加的流量，AD RMS 伺服器。 **注意： 請確定兩部 SQL Server 已輸入的連接埠 5022 開啟。**

**加入 SQL server 2016 伺服器 for Always On 高可用性**

1.  從您想要安裝其他 SQL Server 2016 伺服器的伺服器，請在瀏覽至 伺服器管理員 主控台，然後按一下 **新增角色及功能**。

2.  按一下 [**下一步**直到**選取功能**] 對話方塊。

3.  選取 **容錯移轉叢集**核取方塊。 **注意： 依照此步驟中的原始 SQL server 2016 伺服器以及，讓兩部 SQL Server 已容錯移轉叢集功能。**

4.  按一下 **安裝**安裝容錯移轉叢集功能。

5.  現在，開啟**伺服器管理員**，然後選取**工具**然後**容錯移轉叢集管理員**。

6.  從左側的功能表窗格中，以滑鼠右鍵按一下**容錯移轉叢集管理員**，然後選取**建立叢集**

7.  這會開啟**建立叢集精靈 」** 。

8.  SQL server 2016 伺服器將會用於 Always On 高可用性，並進入然後按一下 [瀏覽**下一步]** 。

9.  您會收到驗證警告。 選取 [ **[是]** 以驗證叢集節點，然後按一下**下一步]** 。

10. 底下**測試選項**頁面上，選取選項**執行所有測試**按一下**下一步。**

11. **注意：叢集驗證精靈應該會傳回幾個警告訊息，特別是當您將不使用共用存放裝置。除此之外，如果您發現任何錯誤訊息您需要在建立 Windows Server 容錯移轉叢集之前加以修正**。

12. 在 **用於管理叢集的存取點** 對話方塊中，輸入叢集名稱和虛擬 IP 位址，Windows Server 容錯移轉叢集，然後按一下**下一步**。

13. 確認設定是否在成功**摘要**然後按一下**完成**。

14. 回到**容錯移轉叢集管理員] 中，** 以滑鼠右鍵按一下叢集，然後選取**其他動作**然後選擇 [**設定叢集仲裁設定**

15. 按一下 [**下一步]** ，然後挑選的選項 **[選取仲裁見證**並按下**下一步]** 一次。

16. 在 **選取仲裁見證**頁面上，選取**設定檔案共用見證**選項。 然後按一下 [下一步]  。

17. 選取 [**瀏覽**並找出您想要在檔案共用路徑] 對話方塊中的檔案共用的路徑。 按一下 [下一步]  。

18. 在 [確認] 頁面中，按一下 [**下一步]** 。

19. 在 摘要 頁面中，按一下 **完成**。

20. 現在，開啟**開始**功能表，然後搜尋**SQL Server 組態管理員**。

21. 以滑鼠右鍵按一下 SQL Server 名稱，並挑選**屬性**。

22. 在 屬性 對話方塊中，選取**AlwaysOn 高可用性** 索引標籤。請檢查**啟用 AlwaysOn 可用性群組**核取方塊。 按一下 [確定]  。 **附註： 這樣在這兩個 SQL server 2016 伺服器。**

23. 然後重新啟動 SQL Server 服務。

24. 現在，開啟**開始**功能表，然後搜尋**SQL Server Management Studio**從左側的導覽窗格中，以滑鼠右鍵按一下**可用性群組**按一下**新增可用性群組精靈**然後按一下**下一步**。

25. 在 **指定可用性群組名稱**頁面選擇的群組名稱 (Ex.SQLAvailabilityGroup2016)。 然後按一下 [下一步]  。

26. 底下**選取資料庫**區段中，指定資料庫。 接著按 [下一步]。 **注意： 有些資料庫可能需要一次備份或處於完整復原模式**。

27. 上一次**指定複本**頁面上，按一下**Add Replica**按鈕，然後選取您其他的 2016 SQL Server。

28. 新增其他伺服器之後，按一下核取方塊，設定要讀取的次要複本的次要伺服器。

29. 瀏覽至**端點**索引標籤，然後按一下**重新整理**選項。 同時也在這裡中, 捲動，並確保相同的服務帳戶是在主要和次要節點上。

30. 現在，請選擇**備份喜好設定**索引標籤，然後選取**次要偏好**選項。

31. 移至**接聽程式** 索引標籤。

32. 指定的名稱 （例如 SQLListener)，並確定連接埠**1433年**，然後按一下**下一步**。

33. 在 [**選取初始資料同步處理**頁面的精靈中，選擇**完整**選項並指定所有 SQL server 可存取的網路位置，然後按一下**下一步]** .

34. 最後，按一下**完成**和程序將會完成。

### <a name="decommission-windows-server-2012-r2-nodes"></a>將 Windows Server 2012 R2 節點解除委任

下列各節將提供您可能需要移除您的 Windows Server 2012 R2 伺服器之後順利升級到 Windows Server 2016 的 AD RMS 叢集的操作工作的指引。

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>移除 Windows Server 2012 R2 AD RMS 伺服器

在升級之後，您可以移除不必要的 AD RMS 伺服器。 您可以選擇執行此動作，它會成為需要解除委任 AD RMS 伺服器時。

**若要移除** **Windows Server 2012 R2 AD RMS 伺服器**

1.  在 Windows Server 2012 R2 AD RMS 伺服器在 [伺服器管理員] 中，選取**管理**從頂端功能表以滑鼠右鍵，然後選擇**移除角色及功能**。

2.  **移除角色及功能精靈**將會開啟並在**在您開始前**畫面上，按一下**下一步**。

3.  在 [**伺服器選取項目**畫面上，按一下**下一步]** 。

4.  在 **伺服器角色**畫面上，取消核取旁**Active Directory Rights Management Services** ，按一下 **下一步**。

5.  在 [**功能**畫面上，按一下**下一步]** 。

6.  在 **確認**畫面上，按一下**移除**。

7.  完成之後，重新啟動伺服器。

8.  您現在可以關閉這部伺服器，並視需要重新配置資源。
