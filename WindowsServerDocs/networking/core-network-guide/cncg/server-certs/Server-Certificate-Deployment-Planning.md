---
title: 伺服器憑證部署規劃
description: 瞭解如何為您的伺服器憑證部署進行規劃。
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c46ede801c590614b9144bead0539f56d31db384
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038678"
---
# <a name="server-certificate-deployment-planning"></a>伺服器憑證部署規劃

>適用於：Windows Server (半年度管道)、Windows Server 2016

部署伺服器憑證之前，您必須先規劃下列專案：

- [規劃基本伺服器設定](#bkmk_basic)

- [規劃網域存取](#bkmk_domain)

- [規劃網頁伺服器上虛擬目錄的位置和名稱](#bkmk_virtual)

- [為您的 Web 服務器規劃 DNS 別名 (CNAME) 記錄](#bkmk_cname)

- [Capolicy.inf .inf 的規劃設定](#bkmk_capolicy)

- [在 CA1 上規劃 CDP 和 AIA 擴充功能的設定](#bkmk_cdp)

- [規劃 CA 與網頁伺服器之間的複製操作](#bkmk_copy)

- [規劃 CA 上伺服器憑證範本的設定](#bkmk_template)

## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>規劃基本伺服器設定
當您在打算用來作為憑證授權單位單位和網頁伺服器的電腦上安裝 Windows Server 2016 之後，您必須重新命名電腦，並為本機電腦指派並設定靜態 IP 位址。

如需詳細資訊，請參閱《 Windows Server 2016 [核心網路指南》](../../../core-network-guide/Core-Network-Guide.md)。

## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>規劃網域存取
若要登入網域，電腦必須是網域成員電腦，而且必須在嘗試登入之前在 AD DS 中建立使用者帳戶。 此外，本指南中的大部分程式都需要使用者帳戶是 Active Directory 消費者和電腦中 Enterprise Admins 或 Domain Admins 群組的成員，因此您必須以具有適當群組成員資格的帳戶登入 CA。

如需詳細資訊，請參閱《 Windows Server 2016 [核心網路指南》](../../../core-network-guide/Core-Network-Guide.md)。

## <a name="plan-the-location-and-name-of-the-virtual-directory-on-your-web-server"></a><a name="bkmk_virtual"></a>規劃網頁伺服器上虛擬目錄的位置和名稱
若要將 CRL 和 CA 憑證的存取權提供給其他電腦，您必須將這些專案儲存在 Web 服務器的虛擬目錄中。 在本指南中，虛擬目錄位於 Web 服務器 WEB1 上。 此資料夾位於 "C：" 磁片磁碟機上，名為「pki」。 您可以在 Web 服務器上，找到適合您部署的任何資料夾位置的虛擬目錄。

## <a name="plan-a-dns-alias-cname-record-for-your-web-server"></a><a name="bkmk_cname"></a>為您的 Web 服務器規劃 DNS 別名 (CNAME) 記錄
別名 (CNAME) 資源記錄有時也稱為正式名稱資源記錄。 您可以使用這些記錄，將一個以上的名稱指向單一主機，讓在相同電腦上裝載檔案傳輸通訊協定 (FTP) 伺服器和網頁伺服器變得容易。 例如，已知的伺服器名稱 (ftp、www) 是使用別名 (CNAME) 資源記錄來註冊，這些資源會對應至裝載這些服務之伺服器電腦的網域名稱系統 (DNS) 主機名稱（例如 WEB1）。

本指南提供的指示說明如何設定您的網頁伺服器，以裝載憑證授權單位單位的憑證撤銷清單 (CRL)  (CA) 。 因為您可能也想要使用 Web 服務器來進行其他用途，例如裝載 FTP 或網站，所以最好是在 DNS 中為您的 Web 服務器建立別名資源記錄。 在本指南中，CNAME 記錄的名稱為 "pki"，但您可以選擇適合您部署的名稱。

## <a name="plan-configuration-of-capolicyinf"></a><a name="bkmk_capolicy"></a>Capolicy.inf .inf 的規劃設定
在安裝 AD CS 之前，您必須在 CA 上使用針對您的部署正確的資訊來設定 Capolicy.inf。 Capolicy.inf .inf 檔案包含下列資訊：

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

您必須為這個檔案規劃下列專案：

- **URL**。 範例 Capolicy.inf .inf 檔案的 URL 值為 **https://pki.corp.contoso.com/pki/cps.txt** 。 這是因為本指南中的網頁伺服器命名為 WEB1，且具有 pki 的 DNS CNAME 資源記錄。 Web 服務器也會加入 corp.contoso.com 網域。 此外，在名為「pki」的 Web 服務器上有一個虛擬目錄，憑證撤銷清單會儲存在其中。 確定您在 Capolicy.inf .inf 檔案中提供的 URL 值指向網域中 Web 服務器上的虛擬目錄。

- **RenewalKeyLength**。 Windows Server 2012 中 AD CS 的預設更新金鑰長度是2048。 您所選取的金鑰長度應該盡可能長，但仍提供與您想要使用之應用程式的相容性。

- **RenewalValidityPeriodUnits**。 範例 Capolicy.inf .inf 檔案的 RenewalValidityPeriodUnits 值為5年。 這是因為 CA 預期的存留期大約是十年。 RenewalValidityPeriodUnits 的值應該會反映 CA 的整體有效期間，或您想要提供註冊的最高年數。

- **CRLPeriodUnits**。 範例 Capolicy.inf .inf 檔案的 CRLPeriodUnits 值為1。 這是因為本指南中憑證撤銷清單的範例重新整理間隔是1周。 在使用此設定指定的間隔值中，您必須將 CA 上的 CRL 發佈到您儲存 CRL 的 Web 服務器虛擬目錄，並為驗證程式中的電腦提供其存取權。

- **AlternateSignatureAlgorithm**。 這個 Capolicy.inf 藉由執行替代的簽章格式來實行改良的安全性機制。 如果您的 Windows XP 用戶端需要來自此 CA 的憑證，則不應執行這種設定。

如果您不打算稍後將任何次級 Ca 新增至您的公開金鑰基礎結構，而且您想要防止新增任何次級 Ca，則可以將 PathLength 金鑰新增至 Capolicy.inf .inf 檔案，其值為0。 若要加入此金鑰，請將下列程式碼複製並貼到您的檔案中：

```
[BasicConstraintsExtension]
PathLength=0
Critical=Yes
```

> [!IMPORTANT]
> 除非您有特定原因，否則不建議您變更 Capolicy.inf .inf 檔案中的任何其他設定。

## <a name="plan-configuration-of-the-cdp-and-aia-extensions-on-ca1"></a><a name="bkmk_cdp"></a>在 CA1 上規劃 CDP 和 AIA 擴充功能的設定
當您將憑證撤銷清單設定 (CRL) 發佈點 (CDP) 以及 CA1 上的授權資訊存取 (AIA) 設定時，您需要您的 Web 服務器名稱和功能變數名稱。 您也需要在 Web 服務器上建立的虛擬目錄名稱，憑證撤銷清單 (CRL) 和儲存憑證授權單位單位憑證。

在此部署步驟中，您必須輸入的 CDP 位置格式如下：

`http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`

例如，如果您的 Web 服務器命名為 WEB1，而 Web 服務器的 DNS 別名 CNAME 記錄為「pki」，則您的網域會是 corp.contoso.com，而您的虛擬目錄會命名為「pki」，則 CDP 位置為：

`http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`

您必須輸入的 AIA 位置格式如下：

`http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`

例如，如果您的 Web 服務器命名為 WEB1，而 Web 服務器的 DNS 別名 CNAME 記錄為「pki」，則您的網域會是 corp.contoso.com，而您的虛擬目錄會命名為「pki」，則 AIA 位置為：

`http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`

## <a name="plan-the-copy-operation-between-the-ca-and-the-web-server"></a><a name="bkmk_copy"></a>規劃 CA 與網頁伺服器之間的複製操作
若要將 CRL 和 CA 憑證從 CA 發佈至 Web 服務器虛擬目錄，您可以在 CA 上設定 CDP 和 AIA 位置之後，執行 certutil-CRL 命令。 使用本指南中的指示，在執行此命令之前，請確定您已在 [CA 屬性 **擴充** 功能] 索引標籤上設定正確的路徑。 此外，若要將企業 CA 憑證複製到 Web 服務器，您必須已經在 Web 服務器上建立虛擬目錄，並將資料夾設定為共用資料夾。

## <a name="plan-the-configuration-of-the-server-certificate-template-on-the-ca"></a><a name="bkmk_template"></a>規劃 CA 上伺服器憑證範本的設定
若要部署自動註冊的伺服器憑證，您必須複製名為 **RAS 和 資訊存取伺服器** 的憑證範本。 根據預設，此複本的名稱為 [ **RAS 和 資訊存取伺服器的複本**]。 如果您想要重新命名此範本複本，請規劃您要在此部署步驟中使用的名稱。

> [!NOTE]
> 本指南的最後三個部署章節，可讓您設定伺服器憑證自動註冊、重新整理伺服器上的群組原則，以及確認伺服器是否已從 CA 收到有效的伺服器憑證-不需要額外的規劃步驟。
