---
title: 伺服器的憑證部署計劃
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>伺服器的憑證部署計劃

>適用於：Windows Server（以每年次管道）、Windows Server 2016

部署伺服器的憑證之前，您必須計劃下列項目：  
  
-   [計劃基本的伺服器設定](#bkmk_basic)  
  
-   [規劃網域存取](#bkmk_domain)  
  
-   [在您的網頁伺服器計劃名稱 virtual directory 與位置](#bkmk_virtual)  
  
-   [您的網頁伺服器的 DNS 別名 (CNAME) 記錄計劃](#bkmk_cname)  
  
-   [計劃設定的 CAPolicy.inf](#bkmk_capolicy)  
  
-   [CA1 計劃設定的 CDP 和 AIA 擴充功能](#bkmk_cdp)  
  
-   [規劃 CA 與 Web 伺服器之間複製作業](#bkmk_copy)  
  
-   [CA 計劃的設定伺服器的憑證範本](#bkmk_template)  
  
## <a name="bkmk_basic"></a>計劃基本的伺服器設定  
您計劃要做為您憑證授權單位和網頁伺服器的電腦上安裝 Windows Server 2016 之後，您必須重新命名電腦指派並設定為 [本機電腦靜態 IP 位址。  
  
如需詳細資訊，查看 Windows Server 2016[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>規劃網域存取  
若要登入至網域中，電腦必須成員網域的電腦和使用者 account 必須建立在嘗試登入之前 AD DS。 此外，在本指南大部分程序都需要帳號，因此您必須登入以具有適當群組成員資格帳號 CA 是的企業系統管理員或網域管理員 Active Directory 使用者與電腦群組成員。  
  
如需詳細資訊，查看 Windows Server 2016[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>在您的網頁伺服器計劃名稱 virtual directory 與位置  
若要提供存取 CRL 並 CA 憑證到其他電腦，您必須在您的網頁伺服器的這些項目儲存在 virtual directory。 本指南，virtual directory 位於 WEB1 網頁伺服器上。 此資料夾 「 c: 「 磁碟機上，為 「 pki 」。 您可以在任何資料夾位置，適用於您的部署網頁伺服器上找到您 virtual directory。  
  
## <a name="bkmk_cname"></a>您的網頁伺服器的 DNS 別名 (CNAME) 記錄計劃  
(CNAME) 別名資源記錄有時候也稱為的正式名稱資源記錄。 這些記錄時，您可以使用多個名稱指向單一主機，讓您輕鬆地執行等主機相同的電腦上的網頁伺服器和檔案傳輸通訊協定 （） 失敗。 例如，已知伺服器名稱 （ftp、 www） 被登記使用別名 (CNAME) 對應的網域名稱系統 」 (DNS) 主機名稱，例如 WEB1，伺服器電腦至該主機的資源記錄這些服務。  
  
本指南指示來設定您的網頁伺服器管理您憑證授權單位憑證撤銷清單 (CRL)。 您也可以在主機 FTP 或網站適用於其他用途，使用您的網頁伺服器像是，因為它最好別名資源記錄 DNS 中建立您的網頁伺服器。 本指南，CNAME 記錄稱為 「 pki 」，但您可以選擇適合您的部署的名稱。  
  
## <a name="bkmk_capolicy"></a>計劃設定的 CAPolicy.inf  
您安裝 AD CS 之前，您必須設定 CAPolicy.inf CA 是正確的部署的資訊。 CAPolicy.inf 檔案包含下列資訊：  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
您必須計劃此檔案下列項目：  
  
-   **URL**。 範例 CAPolicy.inf 檔案的 URL 值為**http://pki.corp.contoso.com/pki/cps.txt**。 這是因為本文中的網頁伺服器為 WEB1 且為 pki 記錄 DNS CNAME 資源。 Web 伺服器也加入 corp.contoso.com 網域。 除此之外，有位於 virtual directory 網頁伺服器名為 「 pki 」 憑證撤銷清單儲存的位置。 請確定您所提供的 URL virtual directory 您 CAPolicy.inf 檔案指向在您的網域中的網頁伺服器的值。  
  
-   **RenewalKeyLength**。 Windows Server 2012 中 AD CS 預設續約按鍵長度為 2048年。 您的主要長度應該儘同時提供與您想要使用的應用程式的相容性。  
  
-   **RenewalValidityPeriodUnits**。 範例 CAPolicy.inf 檔案具有 5 年的 RenewalValidityPeriodUnits 值。 這是因為預期的 CA 壽命約 10 年。 RenewalValidityPeriodUnits 值應能反映 CA 或您想要提供註冊年最多的整體有效期。  
  
-   **CRLPeriodUnits**。 範例 CAPolicy.inf 檔案有 1 CRLPeriodUnits 值。 這是因為為 1 星期的憑證本指南撤銷清單的範例重新整理間隔。 間隔指定的值，您使用此設定時，您必須到您將儲存 CRL Web 的伺服器 virtual directory 發行 CRL CA 上的以及驗證程序的電腦提供存取權限。  
  
-   **AlternateSignatureAlgorithm**。 這個 CAPolicy.inf 實作替代簽章格式實作改善的安全機制。 如果您仍然擁有的是 Windows XP 戶端需要從這個 CA 憑證，您應該不執行這項設定。  
  
如果您不打算任何附屬 Ca 加入稍後公用基礎結構，如果您想要避免加入的任何附屬 Ca 您可以新增 PathLength 金鑰 CAPolicy.inf 檔案與設定為 0。 若要加入此機碼，複製並貼下列程式碼到您的檔案：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 不建議您變更 CAPolicy.inf 檔案中的任何其他設定，除非您有特定的原因。  
  
## <a name="bkmk_cdp"></a>CA1 計劃設定的 CDP 和 AIA 擴充功能  
當您在 CA1 設定憑證撤銷清單 (CRL) Distribution 點 (CDP) 和的授權單位資訊存取權 (AIA) 的設定時，您需要您的網頁伺服器和您的網域名稱的名稱。 您也需要在您的網頁伺服器的憑證撤銷清單 (CRL) 和憑證授權單位憑證的儲存位置建立 virtual directory 的名稱。  
  
您必須在此步驟中部署輸入 CDP 位置具有格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
例如如果 WEB1 名為您的網頁伺服器 DNS 別名網頁伺服器 CNAME 記錄是 「 pki 」，您的網域 corp.contoso.com，以及您 virtual directory 稱為 pki，CDP 的位置處於：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
您必須輸入 AIA 位置具有格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
例如如果 WEB1 名為您的網頁伺服器 DNS 別名網頁伺服器 CNAME 記錄是 「 pki 」，您的網域 corp.contoso.com，與您 virtual directory 稱為 pki，AIA 位置：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>規劃 CA 與 Web 伺服器之間複製作業  
若要發行 CRL 和 CA 憑證 ca 網頁伺服器 virtual directory，您可以 CA 上設定 CDP 和 AIA 位置之後，執行 certutil-crl 命令。 確認您的 CA 屬性設定正確的路徑**的擴充功能**索引標籤上，才能執行此命令本指南使用的指示。 此外，複製企業 CA 憑證 Web 伺服器，您必須已經建立 virtual directory 網頁伺服器上並設定資料夾為的共用資料夾。  
  
## <a name="bkmk_template"></a>CA 計劃的設定伺服器的憑證範本  
若要部署自動註冊伺服器的憑證，您必須將複製憑證範本名為**RAS 及 IAS 伺服器**。 根據預設，這複製名為**的 RAS 複製及 IAS 伺服器**。 如果您想要重新命名此範本複製、 計畫您想要在此步驟中部署使用的名稱。  
  
> [!NOTE]  
> 本指南三張部署區段的程式，可讓您設定伺服器認證自動授權、 重新整理群組原則伺服器，並確認伺服器，已收到 CA 有效的伺服器的憑證-不需要額外計劃的步驟。  
  


