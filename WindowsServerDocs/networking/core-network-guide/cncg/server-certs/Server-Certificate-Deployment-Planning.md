---
title: 伺服器憑證部署規劃
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ec5bc315381f85434753f9becc94409a74271b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356113"
---
# <a name="server-certificate-deployment-planning"></a>伺服器憑證部署規劃

>適用於：Windows Server (半年度管道)、Windows Server 2016

在您部署伺服器憑證之前，您必須先規劃下列專案：  
  
-   [規劃基本伺服器設定](#bkmk_basic)  
  
-   [規劃網域存取](#bkmk_domain)  
  
-   [規劃 Web 服務器上虛擬目錄的位置和名稱](#bkmk_virtual)  
  
-   [規劃 Web 服務器的 DNS 別名（CNAME）記錄](#bkmk_cname)  
  
-   [規劃 Capolicy.inf 的設定](#bkmk_capolicy)  
  
-   [在 CA1 上規劃 CDP 和 AIA 延伸模組的設定](#bkmk_cdp)  
  
-   [規劃 CA 與 Web 服務器之間的複製操作](#bkmk_copy)  
  
-   [在 CA 上規劃伺服器憑證範本的設定](#bkmk_template)  
  
## <a name="bkmk_basic"></a>規劃基本伺服器設定  
在您打算用來作為憑證授權單位單位和網頁伺服器的電腦上安裝 Windows Server 2016 之後，您必須重新命名電腦，並為本機電腦指派和設定靜態 IP 位址。  
  
如需詳細資訊，請參閱《 Windows Server 2016[核心網路指南》](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>規劃網域存取  
若要登入網域，電腦必須是網域成員電腦，而且必須在登入嘗試之前 AD DS 中建立使用者帳戶。 此外，本指南中的大部分程式都需要使用者帳戶是 Active Directory 使用者和電腦中 Enterprise Admins 或 Domain Admins 群組的成員，因此您必須使用具有適當群組成員資格的帳戶登入 CA。  
  
如需詳細資訊，請參閱《 Windows Server 2016[核心網路指南》](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>規劃 Web 服務器上虛擬目錄的位置和名稱  
若要將 CRL 和 CA 憑證的存取權提供給其他電腦，您必須將這些專案儲存在 Web 服務器上的虛擬目錄中。 在本指南中，虛擬目錄位於 Web 服務器 WEB1 上。 此資料夾位於 "C：" 磁片磁碟機上，且名稱為 "pki"。 您可以在您的部署適用的任何資料夾位置上，找出 Web 服務器上的虛擬目錄。  
  
## <a name="bkmk_cname"></a>規劃 Web 服務器的 DNS 別名（CNAME）記錄  
別名（CNAME）資源記錄有時也稱為正式名稱資源記錄。 透過這些記錄，您可以使用多個名稱指向單一主機，讓您輕鬆地將檔案傳輸通訊協定（FTP）伺服器和網頁伺服器裝載在同一部電腦上。 例如，已知的伺服器名稱（ftp、www）是使用別名（CNAME）資源記錄（對應至裝載這些服務之伺服器電腦的網域名稱系統（DNS）主機名稱，例如 WEB1）來註冊。  
  
本指南提供設定 Web 服務器以裝載憑證授權單位單位（CA）之憑證撤銷清單（CRL）的指示。 因為您可能也會想要使用網頁伺服器來進行其他用途，例如裝載 FTP 或網站，所以最好是在 DNS 中為您的 Web 服務器建立別名資源記錄。 在本指南中，CNAME 記錄名為「pki」，但您可以選擇適合您部署的名稱。  
  
## <a name="bkmk_capolicy"></a>規劃 Capolicy.inf 的設定  
安裝 AD CS 之前，您必須在 CA 上設定 Capolicy.inf，並提供您部署的正確資訊。 Capolicy.inf .inf 檔案包含下列資訊：  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=https://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
您必須為此檔案規劃下列專案：  
  
-   **URL**。 範例 Capolicy.inf 檔案的 URL 值為 **https://pki.corp.contoso.com/pki/cps.txt** 。 這是因為本指南中的網頁伺服器名為 WEB1，並具有 pki 的 DNS CNAME 資源記錄。 網頁伺服器也會加入 corp.contoso.com 網域。 此外，在名為「pki」的 Web 服務器上有一個虛擬目錄，其中儲存了憑證撤銷清單。 請確定您在 Capolicy.inf .inf 檔案中為 URL 提供的值指向網域中 Web 服務器上的虛擬目錄。  
  
-   **RenewalKeyLength**。 Windows Server 2012 中 AD CS 的預設更新金鑰長度為2048。 您選取的金鑰長度應該盡可能長，同時仍然提供與您要使用之應用程式的相容性。  
  
-   **RenewalValidityPeriodUnits**。 範例 Capolicy.inf .inf 檔案的 RenewalValidityPeriodUnits 值為5年。 這是因為 CA 的預期生命週期大約十年。 RenewalValidityPeriodUnits 的值應該反映 CA 的整體有效期間，或您想要為其提供註冊的最高年數。  
  
-   **CRLPeriodUnits**。 範例 Capolicy.inf .inf 檔案的 CRLPeriodUnits 值為1。 這是因為本指南中憑證撤銷清單的範例重新整理間隔為1周。 在您使用此設定指定的間隔值上，您必須將 CA 上的 CRL 發佈到您儲存 CRL 的 Web 服務器虛擬目錄，並針對驗證程式中的電腦提供其存取權。  
  
-   **AlternateSignatureAlgorithm**。 這個 Capolicy.inf 會藉由執行替代簽章格式來實行改良的安全性機制。 如果您仍然有需要此 CA 憑證的 Windows XP 用戶端，則不應該執行此設定。  
  
如果您不打算稍後再將任何次級 Ca 新增至您的公開金鑰基礎結構，而且想要防止新增任何次級 Ca，您可以將 PathLength 機碼新增至您的 Capolicy.inf .inf 檔案，其值為0。 若要新增此金鑰，請將下列程式碼複製並貼到您的檔案中：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 不建議您變更 Capolicy.inf .inf 檔案中的任何其他設定，除非您有特定原因要這麼做。  
  
## <a name="bkmk_cdp"></a>在 CA1 上規劃 CDP 和 AIA 延伸模組的設定  
當您在 CA1 上設定憑證撤銷清單（CRL）發佈點（CDP）和授權單位資訊存取（AIA）設定時，您需要 Web 服務器的名稱和功能變數名稱。 您也需要在 Web 服務器上建立的虛擬目錄名稱，其中會儲存憑證撤銷清單（CRL）和憑證授權單位單位憑證。  
  
您在此部署步驟中必須輸入的 CDP 位置格式如下：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
例如，如果您的網頁伺服器名為 WEB1，而 Web 服務器的 DNS 別名 CNAME 記錄是「pki」，則您的網域是 corp.contoso.com，而您的虛擬目錄命名為 pki，CDP 位置為：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
您必須輸入的 AIA 位置格式如下：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
例如，如果您的網頁伺服器名為 WEB1，而 Web 服務器的 DNS 別名 CNAME 記錄是「pki」，則您的網域是 corp.contoso.com，而您的虛擬目錄命名為 pki，AIA 位置會是：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>規劃 CA 與 Web 服務器之間的複製操作  
若要將 CRL 和 CA 憑證從 CA 發佈至 Web 服務器虛擬目錄，您可以在設定 CA 上的 CDP 和 AIA 位置之後，執行 certutil-CRL 命令。 請確定您在 [CA 內容**延伸**] 索引標籤上設定正確的路徑，然後再使用本指南中的指示執行此命令。 此外，若要將企業 CA 憑證複製到 Web 服務器，您必須已經在 Web 服務器上建立虛擬目錄，並將資料夾設定為共用資料夾。  
  
## <a name="bkmk_template"></a>在 CA 上規劃伺服器憑證範本的設定  
若要部署自動註冊的伺服器憑證，您必須複製名為**RAS 和 資訊存取伺服器**的憑證範本。 根據預設，此複本的名稱為 [ **RAS 和 資訊存取伺服器的複本**]。 如果您想要重新命名此範本複本，請在此部署步驟中規劃您要使用的名稱。  
  
> [!NOTE]  
> 本指南的最後三個部署章節，可讓您設定伺服器憑證自動註冊、重新整理伺服器上的群組原則，以及確認伺服器已從 CA 收到有效的伺服器憑證-不需要額外的規劃步驟.  
  


