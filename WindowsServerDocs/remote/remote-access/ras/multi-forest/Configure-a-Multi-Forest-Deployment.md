---
title: Configure a Multi-Forest Deployment
description: 本主題是在 Windows Server 2016 的多樹系環境中部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c8feff2-cae1-4376-9dfa-21ad3e4d5d99
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f4675e8f465cc44597e16b0312911cae28bd7a1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860491"
---
# <a name="configure-a-multi-forest-deployment"></a>Configure a Multi-Forest Deployment

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明如何在數個潛在案例中設定遠端存取多樹系部署。 所有案例均假設 DirectAccess 目前已部署於名為 Forest1 的單一樹系上，而您正在設定 DirectAccess 以便與名為 Forest2 的新樹系一起運作。  
  
## <a name="access-resources-from-forest2"></a><a name="AccessForest2"></a>從 Forest2 存取資源  
在這個案例中，DirectAccess 已經部署於 Forest1，而且已設定為允許 Forest1 的用戶端存取公司網路。 根據預設，透過 DirectAccess 連線的用戶端只能存取 Forest1 中的資源，無法存取 Forest2 中的任何伺服器。  
  
#### <a name="to-enable-directaccess-clients-to-access-resources-from-forest2"></a>讓 DirectAccess 用戶端可以存取 Forest2 的資源  
  
1.  如果 Forest2 的 DNS 尾碼不是 Forest1 的 DNS 尾碼一部分，請使用 Forest2 中網域的尾碼來新增 NRPT 規則，並選擇性地將 Forest2 中網域的尾碼新增到 DNS 尾碼搜尋清單。  
  
2.  如果 IPv6 已部署於內部網路中，請在 Forest2 中新增相關的內部 IPv6 首碼。  
  
## <a name="enable-clients-from-forest2-to-connect-via-directaccess"></a><a name="EnableForest2DA"></a>允許來自 Forest2 的用戶端透過 DirectAccess 連線  
在這個案例中，您可以設定遠端存取部署，以允許 Forest2 的用戶端存取公司網路。 假設您已針對 Forest2 中的用戶端電腦建立必要的安全性群組。   
  
#### <a name="to-allow-clients-from-forest2-to-access-the-corporate-network"></a>允許 Forest2 的用戶端存取公司網路  
  
1.  新增 Forest2 的用戶端的安全性群組。  
  
2.  如果 Forest2 的 DNS 尾碼不是 Forest1 DNS 尾碼的一部分，請在 Forest2 中新增具有用戶端網域尾碼的 NRPT 規則，以啟用網域控制站的存取權以進行驗證，並選擇性地將 Forest2 中網域的尾碼新增至 DNS尾碼搜尋清單。 
  
3.  在 Forest2 中新增內部 IPv6 首碼，啟用 DirectAccess 來建立網域控制站的 IPsec 通道以進行驗證。  
  
4.  重新整理管理伺服器清單。  
  
## <a name="add-entry-points-from-forest2"></a><a name="AddEPForest2"></a>新增 Forest2 的進入點  
在這個案例中，DirectAccess 已部署於 Forest1 上的多站台設定中，而您想要從 Forest2 新增名為 DA2 的遠端存取伺服器，以做為現有 DirectAccess 多站台部署的進入點。  
  
#### <a name="to-add-a-remote-access-server-from-forest2-as-an-entry-point"></a>從 Forest2 新增遠端存取伺服器以做為進入點  
  
1.  請確定遠端存取管理員具備足夠的權限可以在 DA2 的網域上寫入 GPO，而且該遠端存取管理員是 DA2 的本機系統管理員。  
  
2.  新增 DA2 做為進入點。   
  
3.  使用 Forest2 中網域的尾碼來新增 NRPT 規則，以啟用網域控制站的存取來進行驗證，並選擇性地將 Forest2 中網域的尾碼新增到 DNS 尾碼搜尋清單。  
  
4.  視需要在 Forest2 中新增相關的內部 IPv6 首碼，以啟用遠端存取來建立公司資源的 IPsec 通道，並確定 NCSI 探查可以正確運作。  
  
5.  重新整理管理伺服器清單。  
  
## <a name="configure-otp-in-a-multi-forest-deployment"></a><a name="OTPMultiForest"></a>在多樹系部署中設定 OTP  
在多樹系部署中設定 OTP 時，請注意下列術語：  
  
-   根 CA-樹系主要的 PKI 樹狀結構 CA。  
  
-   企業 CA-所有其他 Ca。  
  
-   資源樹系-包含根 CA 的樹系，並被視為「管理 forest\domain」。  
  
-   帳戶樹系-拓撲中的所有其他樹系。  
  
PowerShell 指令碼 PKISync.ps1 為此程序的必要指令碼。 請參閱 [AD CS：適用於跨樹系憑證註冊的 PKISync.ps1 指令碼](https://technet.microsoft.com/library/ff961506.aspx)。  
  
> [!NOTE]  
> 本主題包含可讓您用以自動化文中所述部分程序的範例 Windows PowerShell 指令程式。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
### <a name="configure-cas-as-certificate-publishers"></a><a name="BKMK_CertPub"></a>將 Ca 設定為憑證發行者  
  
1.  從提升權限的命令提示字元執行下列命令，在所有樹系的所有企業 CA 中啟用 LDAP 轉介支援：  
  
    ```  
    certutil -setreg Policy\EditFlags +EDITF_ENABLELDAPREFERRALS  
    ```  
  
2.  將所有的企業 CA 電腦帳戶新增到每個帳戶樹系中的 Active Directory Cert Publishers 安全性群組。  
  
3.  從提升權限的命令提示字元執行下列命令，在所有樹系的所有 CA 電腦上重新啟動所有的 certsvc 服務：  
  
    ```  
    net stop certsvc && net start certsvc  
    ```  
  
4.  從提升權限的命令提示字元執行下列命令，以擷取根 CA 憑證：  
  
    ```  
    certutil -config <Computer-Name>\<Root-CA-Name> -ca.cert <root-ca-cert-filename.cer>  
    ```  
  
    （如果您在根 CA 上執行命令，您可以省略連線資訊-config < 電腦名稱稱 >\\< 根 CA 名稱 >）  
  
    1.  從提升權限的命令提示字元執行下列命令，在帳戶樹系 CA 上匯入上一個步驟的根 CA 憑證：  
  
        ```  
        certutil -dspublish -f <root-ca-cert-filename.cer> RootCA  
        ```  
  
    2.  將資源樹系憑證範本的讀取/寫入權限授與 \<帳戶樹系\>\\< 系統管理員帳戶\>。  
  
    3.  從提升權限的命令提示字元執行下列命令，以擷取所有資源樹系企業 CA 憑證：  
  
        ```  
        certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
        ```  
  
        （如果您在根 CA 上執行命令，您可以省略連線資訊-config < 電腦名稱稱 >\\< 根 CA 名稱 >）  
  
    4.  從提升權限的命令提示字元執行下列命令，在帳戶樹系 CA 上匯入上一個步驟的企業 CA 憑證：  
  
        ```  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> NTAuthCA  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> SubCA  
        ```  
  
    5.  從發行的憑證範本清單移除帳戶樹系 OTP 憑證範本。  
  
### <a name="delete-and-import-otp-certificate-templates"></a><a name="BKMK_DelImp"></a>刪除和匯入 OTP 憑證範本  
  
1.  從帳戶樹系 (也就是 Forest2) 刪除 OTP 憑證範本。  
  
2.  使用下列 PowerShell 命令，將憑證範本和 Oid 物件從資源樹系複製到每個帳戶樹系：  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <DA OTP registration authority template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <Secure DA OTP logon certificate template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Oid -f  
    ```  
  
### <a name="publish-otp-certificate-templates"></a><a name="BKMK_Publish"></a>發佈 OTP 憑證範本  
  
-   在所有帳戶樹系 CA 上發行最新匯入的憑證範本。  
  
### <a name="extract-and-synchronize-the-ca"></a><a name="BKMK_Extract"></a>將 CA 解壓縮並同步處理  
  
1.  從提升權限的命令提示字元執行下列命令，從帳戶樹系擷取所有企業 CA 憑證：  
  
    ```  
    certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
    ```  
  
2.  使用下列 PowerShell 命令，跨樹系將 CA 從帳戶樹系同步至資源樹系：  
  
    ```  
    .\PKISync.ps1 -sourceforest <account forest DNS> -targetforest <resource forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
3.  使用下列 PowerShell 命令，跨樹系將 CA 從資源樹系同步至帳戶樹系：  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
## <a name="configuration-procedures"></a>設定程序  
下列各節包含上述案例部署的設定程序。 完成程序之後，請返回案例繼續。  
  
### <a name="add-nrpt-rules-and-dns-suffixes"></a><a name="NRPT_DNSSearchSuffix"></a>新增 NRPT 規則與 DNS 尾碼  
透過 DirectAccess 連線至公司網路的用戶端會使用名稱解析原則表格 (NRPT)，來判斷應使用哪一部 DNS 伺服器來解析不同資源的位址。 這允許用戶端解析公司資源位址，並協助用戶端維護適當的公司內部/公司外部分類，需要有此分類才能使 DirectAccess 維持運作。 DirectAccess 設定工具會自動偵測 Forest1 的根 DNS 尾碼，並將它新增到 NRPT 表格。 但是，Forest2 的 FQDN 尾碼並不會自動新增到 NRPT 表格，而遠端存取管理員必須手動新增它們。  
  
DNS 尾碼搜尋清單允許用戶端使用簡短標籤名稱，而不需使用 FQDN。 遠端存取設定工具會自動將 Forest1 中的所有網域新增到 DNS 尾碼搜尋清單。 如果您想要讓用戶端針對 Forest2 中的資源使用簡短標籤名稱，則需要手動新增它們。  
  
##### <a name="to-add-a-dns-suffix-to-the-nrpt-table-and-domain-suffixes-to-the-dns-suffix-search-list"></a>將 DNS 尾碼新增到 NRPT 表格並將網域尾碼新增到 DNS 尾碼搜尋清單  
  
1.  在 [遠端存取管理] 主控台中間窗格的 [步驟 3 基礎結構伺服器] 區域中，按一下 [編輯]。  
  
2.  在 [網路位置伺服器] 頁面上，按 [下一步]。  
  
3.  在 [DNS] 頁面的表格中，輸入屬於 Forest 2 中公司網路一部分的任何其他名稱尾碼。 在 [DNS 伺服器位址] 中，手動輸入 DNS 伺服器位址，或者按一下 [偵測]。 如果您未輸入位址，新的專案會套用為 NRPT 豁免。 然後按 **[下一步]** 。  
  
4.  選擇性：在 [DNS 尾碼搜尋清單] 頁面的 [新尾碼] 方塊中輸入尾碼，然後按一下 [新增] 來新增任意的 DNS 尾碼。 然後按 **[下一步]** 。  
  
5.  在 [管理] 頁面上，按一下 [完成]。  
  
6.  在 [遠端存取管理] 主控台的中間窗格中，按一下 [完成]。  
  
7.  在 [遠端存取檢閱] 對話方塊中，按一下 [套用]。  
  
8.  在 [套用遠端存取安裝精靈設定] 對話方塊上，按一下 [關閉]。  
  
### <a name="add-internal-ipv6-prefix"></a><a name="IPv6Prefix"></a>新增內部 IPv6 首碼  
  
> [!NOTE]  
> 只有在 IPv6 已部署於內部網路時，新增內部 IPv6 首碼才會有相關性。  
  
遠端存取會管理適用於公司資源的 IPv6 首碼清單。 透過 DirectAccess 連線的用戶端只能存取具備這些 IPv6 首碼的資源。 因為 [遠端存取管理] 主控台和 Windows PowerShell 命令會自動新增 Forest1 的 IPv6 首碼，而且可能不會新增其他樹系的首碼，所以您必須手動新增任何遺漏的 Forest2 首碼。  
  
##### <a name="to-add-an-ipv6-prefix"></a>新增 IPv6 首碼  
  
1.  在 [遠端存取管理] 主控台中間窗格的 [步驟 2 遠端存取伺服器] 區域中，按一下 [編輯]。  
  
2.  在 [遠端存取伺服器安裝精靈] 中，按一下 [首碼設定]。  
  
3.  在 [首碼設定] 頁面的 [內部網路 IPv6 首碼] 中，新增任何其他的 IPv6 首碼並以分號分隔，例如，2001:db8:1::/64;2001:db8:2::/64。 然後按 **[下一步]** 。  
  
4.  在 [驗證] 頁面上，按一下 [完成]。  
  
5.  在 [遠端存取管理] 主控台的中間窗格中，按一下 [完成]。  
  
6.  在 [遠端存取檢閱] 對話方塊中，按一下 [套用]。  
  
7.  在 [套用遠端存取安裝精靈設定] 對話方塊上，按一下 [關閉]。  
  
### <a name="add-client-security-groups"></a><a name="SGs"></a>新增用戶端安全性群組  
若要從 Forest2 啟用 Windows 8 用戶端電腦以透過 DirectAccess 存取資源，您必須將安全性群組從 Forest2 新增到遠端存取部署。  
  
##### <a name="to-add-windows-8-client-security-groups"></a>新增 Windows 8 用戶端安全性群組  
  
1.  在 [遠端存取管理] 主控台中間窗格的 [步驟 1 遠端用戶端] 區域中，按一下 [編輯]。  
  
2.  在 [DirectAccess 用戶端安裝精靈] 中，按一下 [選取群組]，然後在 [選取群組] 頁面上按一下 [新增]。  
  
3.  在 [選取群組] 對話方塊中，選取包含 DirectAccess 用戶端電腦的安全性群組。 然後按 **[下一步]** 。  
  
4.  在 [網路連線助理] 頁面上，按一下 [完成]。  
  
5.  在 [遠端存取管理] 主控台的中間窗格中，按一下 [完成]。  
  
6.  在 [遠端存取檢閱] 對話方塊中，按一下 [套用]。  
  
7.  在 [套用遠端存取安裝精靈設定] 對話方塊上，按一下 [關閉]。  
  
若要從 Forest2 啟用 Windows 7 用戶端電腦，以便在啟用多網站時透過 DirectAccess 存取資源，您必須將安全性群組從 Forest2 新增至每個進入點的遠端存取部署。 如需新增 Windows 7 安全性群組的相關資訊，請參閱3.6 中**用戶端支援**頁面的說明。 啟用多網站部署。  
  
### <a name="refresh-the-management-servers-list"></a><a name="RefreshMgmtServers"></a>重新整理管理伺服器清單  
遠端存取會自動探索所有包含 DirectAccess 設定 GPO 之樹系中的基礎結構伺服器。 如果已將 DirectAccess 部署於 Forest1 的伺服器上，則會將伺服器 GPO 寫入它在 Forest1 中的網域。 如果您針對 Forest2 的用戶端啟用 DirectAccess 存取，則會將用戶端 GPO 寫入 Forest2 中的網域。  
  
需要基礎結構伺服器的自動探索程式，才能允許透過 DirectAccess 存取網域控制站和 Microsoft 端點 Configuration Manager。 您必須手動啟動探索程序。  
  
##### <a name="to-refresh-the-management-servers-list"></a>重新整理管理伺服器清單  
  
1.  在 [遠端存取管理] 主控台中，按一下 [設定]，然後在 [工作] 窗格中，按一下 [重新整理管理伺服器]。  
  
2.  在 [重新整理管理伺服器] 對話方塊上，按一下 [關閉]。  
  


