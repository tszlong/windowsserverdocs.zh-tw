---
title: 伺服器憑證部署規劃
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0d14ed33c9bf389433f59774c04ff15e37256cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839529"
---
# <a name="server-certificate-deployment-planning"></a>伺服器憑證部署規劃

>適用於：Windows Server （半年通道），Windows Server 2016

在部署伺服器憑證之前，您必須規劃下列項目：  
  
-   [規劃基本伺服器設定](#bkmk_basic)  
  
-   [規劃網域存取](#bkmk_domain)  
  
-   [規劃您的 Web 伺服器上的虛擬目錄的名稱與位置](#bkmk_virtual)  
  
-   [規劃您的 Web 伺服器的 DNS 別名 (CNAME) 記錄](#bkmk_cname)  
  
-   [計劃的 CAPolicy.inf 的組態](#bkmk_capolicy)  
  
-   [CA1 之 CDP 和 AIA 延伸模組的計劃設定](#bkmk_cdp)  
  
-   [規劃 CA 和網頁伺服器之間的複製作業](#bkmk_copy)  
  
-   [規劃在 CA 上的伺服器憑證範本的設定](#bkmk_template)  
  
## <a name="bkmk_basic"></a>規劃基本伺服器設定  
在您打算使用與您的憑證授權單位和 Web 伺服器的電腦上安裝 Windows Server 2016 之後，您必須將電腦重新命名和指派與設定本機電腦的靜態 IP 位址。  
  
如需詳細資訊，請參閱 Windows Server 2016[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>規劃網域存取  
若要登入網域，電腦必須是網域成員電腦，必須先登入嘗試的 AD DS 中建立使用者帳戶。 此外，本指南中的大部分程序需要使用者帳戶是在 Active Directory 使用者和電腦的 Enterprise Admins 或 Domain Admins 群組的成員，因此您必須登入帳戶具有適當的群組成員資格的 CA。  
  
如需詳細資訊，請參閱 Windows Server 2016[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>規劃您的 Web 伺服器上的虛擬目錄的名稱與位置  
若要提供存取 CRL 和 CA 憑證至其他電腦，您必須在虛擬目錄儲存這些項目 Web 伺服器上。 本指南中，虛擬目錄位於 網頁伺服器 WEB1。 此資料夾位於"C:"磁碟機上，和名為"pki。 」 您可以在您的 Web 伺服器，在適用於您部署的任何資料夾位置中找到虛擬目錄。  
  
## <a name="bkmk_cname"></a>規劃您的 Web 伺服器的 DNS 別名 (CNAME) 記錄  
別名 (CNAME) 資源記錄有時也稱為正式名稱資源記錄。 透過這些記錄，您可以使用多個名稱指向單一主機，方便您的檔案傳輸通訊協定 (FTP) 伺服器和 Web 伺服器在同一部電腦上的執行主控件等項目。 例如，已知的伺服器名稱 （ftp，www） 會註冊使用別名 (CNAME) 資源記錄會對應至網域名稱系統 (DNS) 主機名稱，例如 WEB1，伺服器電腦裝載這些服務。  
  
本指南提供指示來設定您的 Web 伺服器來裝載您的憑證授權單位 (CA) 憑證撤銷清單 (CRL)。 您也可以用於其他用途，使用您的 Web 伺服器這類裝載 FTP 或 Web 站台，因為它會是個不錯的主意，在 DNS 中建立別名資源記錄，您的 Web 伺服器。 在本指南中，CNAME 記錄名為"pki，"但您可以選擇適合您的部署名稱。  
  
## <a name="bkmk_capolicy"></a>計劃的 CAPolicy.inf 的組態  
安裝 AD CS 之前，您必須設定對您的部署是正確的資訊在 CA 上的 CAPolicy.inf。 CAPolicy.inf 檔案包含下列資訊：  
  
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
您必須規劃下列項目，這個檔案：  
  
-   **URL**。 範例 CAPolicy.inf 檔案的 URL 值為**https://pki.corp.contoso.com/pki/cps.txt**。 這是 pki 的因為名為 WEB1 本指南中的 Web 伺服器，而且有 DNS CNAME 資源記錄。 Web 伺服器也會加入 corp.contoso.com 網域中。 此外，在名為"pki 」 儲存的憑證撤銷清單的 Web 伺服器上沒有虛擬目錄。 請確認您提供 URL 以 CAPolicy.inf 檔案點為單位，到虛擬目錄在您的網域中的 Web 伺服器的值。  
  
-   **RenewalKeyLength**。 Windows Server 2012 中的 AD CS 的預設更新索引鍵長度為 2048年。 您選取的金鑰長度應盡可能，同時又能提供與您想要使用的應用程式相容性。  
  
-   **RenewalValidityPeriodUnits**。 範例 CAPolicy.inf 檔案具有 RenewalValidityPeriodUnits 值為 5 年。 這是因為在 CA 的預期存留時間約十年。 RenewalValidityPeriodUnits 的值應該會反映 CA 或您要提供註冊的年數最高的整體有效期間。  
  
-   **CRLPeriodUnits**。 範例 CAPolicy.inf 檔案有 CRLPeriodUnits 值為 1。 這是因為在本指南中的憑證撤銷清單的範例中重新整理間隔是 1 週。 在您使用此設定指定的間隔值，您必須將 CA 上的 CRL 發佈到 Web 伺服器虛擬目錄儲存 CRL 的位置，並提供驗證程序中的電腦存取權。  
  
-   **AlternateSignatureAlgorithm**。 此 CAPolicy.inf 實作改進的安全性機制，藉由實作替代簽章格式。 如果您仍有 Windows XP 用戶端需要此 CA 的憑證，您不應該實作這項設定。  
  
如果您不打算加入的公開金鑰基礎結構中的任何次級 Ca，在稍後的時間，而且您想要防止任何次級 Ca，您可以 PathLength 將金鑰新增至您的 CAPolicy.inf 檔案，使用值為 0。 若要新增此機碼，複製並貼到檔案的下列程式碼：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 不建議您變更 CAPolicy.inf 檔案中的任何其他設定，除非您有特定的理由，這項操作。  
  
## <a name="bkmk_cdp"></a>CA1 之 CDP 和 AIA 延伸模組的計劃設定  
當您設定憑證撤銷清單 (CRL) 發佈點 (CDP) 和授權資訊存取 (AIA) 設定 CA1 上時，您會需要您的 Web 伺服器和您的網域名稱的名稱。 您也需要您在您儲存的憑證撤銷清單 (CRL) 和憑證授權單位憑證的 Web 伺服器建立虛擬目錄的名稱。  
  
您必須輸入在此步驟中部署的 CDP 位置的格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
比方說，如果您的 Web 伺服器名為 WEB1，而且您的 DNS 別名的 CNAME 記錄的 Web 伺服器 」 pki 」，您的網域是 corp.contoso.com，和名為 pki 虛擬目錄，CDP 位置是：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
您必須輸入的 AIA 位置的格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
比方說，您的 Web 伺服器名為 WEB1，而且您的 DNS 別名的 CNAME 記錄的 Web 伺服器是 「 pki 」，您的網域是 corp.contoso.com，和名為 pki 虛擬目錄、 AIA 位置是：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>規劃 CA 和網頁伺服器之間的複製作業  
若要發行至 Web 伺服器虛擬目錄的 CA 的 CRL 和 CA 憑證，您可以執行 certutil-crl 命令之後在 CA 上設定 CDP 和 AIA 位置。 請確定您在 [CA 內容上設定正確的路徑**延伸模組**] 索引標籤，然後再執行此命令使用本指南中的指示。 此外，企業 CA 憑證複製到 Web 伺服器，您必須已建立的虛擬目錄網頁伺服器上並設定為共用資料夾的資料夾。  
  
## <a name="bkmk_template"></a>規劃在 CA 上的伺服器憑證範本的設定  
若要部署的自動註冊伺服器憑證，您必須將名為的憑證範本複製**RAS 及 IAS 伺服器**。 依預設，此複本稱為**複製的 RAS 及 IAS 伺服器**。 如果您想要重新命名此範本的複本，計劃您想要在此步驟中部署的名稱。  
  
> [!NOTE]  
> 本指南的最後三個部署章節-這可讓您設定伺服器憑證自動註冊、 在伺服器上，重新整理群組原則，並確認伺服器已從 CA 收到有效的伺服器憑證-不需要額外的規劃步驟。  
  


