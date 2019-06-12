---
title: 準備 CAPolicy.inf 檔案
description: CAPolicy.inf 包含安裝 「 Active Directory 憑證服務 (AD CS) 時，或更新 CA 憑證時所使用的各種設定。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19a87df7c4f165d3b0e6c5add4bc40ff97cc87cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446466"
---
# <a name="capolicyinf-syntax"></a>CAPolicy.inf Syntax
>   適用於：Windows Server （半年通道），Windows Server 2016

CAPolicy.inf 是組態檔中定義的擴充功能、 限制和其他組態設定會套用至根 CA 憑證和根 CA 所簽發的所有憑證。 CAPolicy.inf 檔案必須安裝在主機伺服器的根 CA 會開始安裝程式常式之前。 要修改的根 CA 上的安全性限制時，必須更新根憑證，更新程序開始之前，必須伺服器上安裝更新的 CAPolicy.inf 檔案。

CAPolicy.inf 是：

-   建立並由系統管理員以手動方式定義

-   根和次級 CA 憑證的建立期間使用

-   在簽署和發行憑證 (不是授與要求的 CA) 簽署的 CA 上定義

當您建立您的 CAPolicy.inf 檔案之後時，您必須將它複製到 **%systemroot%** 伺服器才能安裝 ADCS 或更新 CA 憑證的資料夾。

CAPolicy.inf 可讓您能夠指定並設定各種不同的 CA 的屬性和選項。 下一節會說明要建立您的特定需求來量身打造的.inf 檔案的所有選項。

## <a name="capolicyinf-file-structure"></a>CAPolicy.inf 檔案結構

下列詞彙用來描述.inf 檔案結構：

-   _區段_– 是區域檔案中涵蓋的索引鍵的邏輯群組。 .Inf 檔案中的區段名稱均不會出現在括號。 許多，但不是全部區段可用來設定憑證延伸。

-   _索引鍵_– 項目的名稱，並在等號左邊會出現。

-   _值_– 是參數，而在等號右邊。

在下列範例中， **[版本]** 區段中，**簽章**索引鍵，並 **」\$Windows NT\$"** 的值。

範例：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

識別為.inf 檔案的檔案。 唯一的必要區段和 CAPolicy.inf 檔案的開頭必須是版本。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出由組織所定義的原則，以及其屬於選擇性或強制性。 以逗號分隔多個原則。 名稱在特定的部署，或相對於檢查這些原則存在的自訂應用程式內容中具有意義。

每個原則定義，必須定義該特定的原則設定的區段。 針對每個原則中，您需要提供使用者定義的物件識別碼 (OID)，以及文字您想要顯示的原則陳述式或原則陳述式指向的 URL。 HTTP、 FTP 或 LDAP URL 的形式可以是 URL。

如果您要的原則陳述式中具有描述性文字，CAPolicy.inf 的接下來的三行應該會像這樣：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果您要使用 URL 來裝載 CA 的原則陳述式，然後接下來的三行改為如下：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

其他情況：

-   支援多個 URL，請注意索引鍵。

-   支援通知與 URL 相同的 [原則] 區段中的索引鍵。

-   必須以引號括住有空格的 Url 或空格的文字。 這適用於**URL**索引鍵，不論它出現的區段。

多個通知和原則 區段中的 Url 的範例如下：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

您可以指定在 CAPolicy.inf 中的根 CA 憑證的 CRL 發佈點 (Cdp)。  安裝 CA 之後, 您可以設定 CDP Url，其中包含在每個發行憑證的 CA。 根 CA 憑證顯示此區段的 CAPolicy.inf 檔案中指定的 Url。 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

本節中一些其他資訊：
-   支援：
    - HTTP 
    - 檔案的 Url
    - LDAP Url 
    - 多個 Url
   
    >[!IMPORTANT]
    >不支援 HTTPS Url。

-   引號必須住空格的 Url。

-   如果沒有任何 Url 指定 – 也就是，如果 **[CRLDistributionPoint]** – 區段存在檔案中，但卻是空白的根 CA 憑證授權單位資訊存取延伸模組會略過。 設定根 CA 時，這是通常偏好。 Windows 不會執行撤銷檢查根 CA 憑證，所以多餘的根 CA 憑證的 CDP 延伸。

-    CA 可以發行至檔案的 UNC，比方說，代表用戶端會透過 HTTP 擷取的網站資料夾的共用。

-   只有當您設定的根 CA 或更新根 CA 憑證時，才能使用這一節。 CA 會判斷次級 CA CDP 延伸模組。
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

在 CAPolicy.inf 中的根 CA 憑證，您可以指定授權單位資訊存取點。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

在 [授權單位資訊存取] 區段上一些其他注意事項：

-   支援多個 Url。

-   支援 HTTP、 FTP、 LDAP 和檔案的 Url。 不支援 HTTPS Url。

-   如果您要設定根 CA，或更新根 CA 憑證，才會使用這一節。 次級 CA AIA 延伸模組是由 CA 發行次級 CA 的憑證決定。

-   使用空間的 Url 必須以引號括住。

-   如果沒有任何 Url 指定 – 也就是，如果 **[AuthorityInformationAccess]** – 區段存在檔案中，但卻是空白的 CRL 發佈點延伸模組會略過的根 CA 憑證。 同樣地，這會是偏好的設定，在根 CA 憑證的情況下，因為沒有任何高於的根 CA 必須要由其憑證的連結參考的授權單位。

### <a name="certsrvserver"></a>certsrv_Server

CAPolicy.inf 的另一個選擇性的區段是 [certsrv_server]，用來指定更新的金鑰長度，更新的有效期間，憑證撤銷清單 (CRL) 的有效期間，ca 所更新或安裝。 在本節中的索引鍵都需要。 許多這些設定都有足以滿足大多數需求且只需 CAPolicy.inf 檔案從省略的預設值。 或者，許多這些設定可以變更在安裝 CA 之後。

範例如下：

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength**設定只更新索引鍵的大小。 此外，這才使用 CA 憑證更新期間產生新的金鑰組時。 CA 安裝時，會設定為初始的 CA 憑證的金鑰大小。

更新 CA 憑證時使用新的金鑰組，金鑰長度可以增加或減少。 比方說，如果您已將設定根 CA 金鑰大小為 4096 個位元組或更高版本，然後發現您的 Java 應用程式或網路裝置只能支援金鑰大小為 2048 個位元組。 無論您增加或減少大小，您必須重新發出該 CA 所發行的所有憑證。

**RenewalValidityPeriod**並**RenewalValidityPeriodUnits**更新舊的根 CA 憑證時，建立新的根 CA 憑證的存留期。 它只適用於根 CA。 次級 CA 的憑證存留時間取決於其上層。 RenewalValidityPeriod 可以有下列值：小時、 天、 週、 月和年。

**CRLPeriod**並**CRLPeriodUnits**建立基底 CRL 有效期間。 **CRLPeriod**可以有下列值：小時、 天、 週、 月和年。

**CRLDeltaPeriod**並**Crldeltaperiodunits=0**建立 delta CRL 的有效期間。 **CRLDeltaPeriod**可以有下列值：小時、 天、 週、 月和年。

在安裝 CA 之後，所有這些設定可以進行設定：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

請務必重新啟動 Active Directory 憑證服務所做的變更才會生效。

**LoadDefaultTemplates**僅適用於在企業 CA 安裝期間。 此設定，其中 True 或 False （或 1 或 0），指出是否 CA 已設定任何預設範本。

在 CA 的預設安裝中，預設憑證範本子集會新增至憑證授權單位 嵌入式管理單元的 憑證範本 資料夾。 這表示，只要 AD CS 服務啟動之後已安裝角色的使用者或電腦具有足夠權限可以立即註冊憑證。

此外，您可能不想發行任何憑證，只有在已安裝 CA 之後，立即讓您可以使用 LoadDefaultTemplates 設定來防止新增至企業 CA 的預設範本。 如果沒有任何 CA 上設定的範本，它可以發出任何憑證。

**AlternateSignatureAlgorithm**會設定 CA 以支援 PKCS\#1 V2.1 簽章格式的 CA 憑證和憑證要求。 當設定為 1 的根 CA 的 CA 憑證將包含 PKCS\#1 V2.1 簽章格式。 次級 CA 上設定時，次級 CA 將會建立憑證要求包含 PKCS\#1 V2.1 簽章格式。

**ForceUTF8**變更預設編碼方式的相對辨別名稱 (Rdn) 中主旨和簽發者辨別名稱為 utf-8。 僅支援 utf-8，例如那些定義為目錄字串型別，由 RFC、 受影響的 Rdn。 比方說，RDN 的網域元件 (DC) 支援而 Country RDN (C) 只支援做為可列印的字串編碼為 IA5 或 utf-8 編碼方式。 ForceUTF8 指示詞會影響 DC RDN，但不是會影響 C RDN。

**EnableKeyCounting**設定 CA 以每次使用 CA 簽署金鑰時，遞增計數器。 除非您有硬體安全性模組 (HSM) 和支援金鑰計算的相關聯的密碼編譯服務提供者 (CSP)，請勿啟用此設定。 不是 Microsoft 強式 CSP 或 Microsoft 軟體金鑰儲存提供者 (KSP) 支援索引鍵計數。


## <a name="create-the-capolicyinf-file"></a>建立 CAPolicy.inf 檔案

在安裝 AD CS 之前，您設定 CAPolicy.inf 檔案使用特定的設定為您的部署。

**必要條件：** 您必須是 Administrators 群組的成員。

1. 在您打算安裝 AD CS，開啟 Windows PowerShell，在電腦上輸入**記事本 c:\CAPolicy.inf**按 ENTER 鍵。

2. 當系統提示您建立新檔案時，按一下 [是]  。

3. 輸入下列檔案內容：
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
   [CRLDistributionPoint]  
   [AuthorityInformationAccess]
   ```
4. 按一下 **檔案**，然後按一下**另存新檔**。

5. 瀏覽至 %systemroot%資料夾。

6. 確認下列事項：

   -   [檔案名稱]  設為 **CAPolicy.inf**

   -   [存檔類型]  設定為 [所有檔案] 

   -   [編碼]  為 **[ANSI]**

7. 按一下 [儲存]  。

8. 當系統提示您覆寫檔案時，按一下 [是]  。

   ![將儲存為 CAPolicy.inf 檔案位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   務必以 inf 副檔名儲存 CAPolicy.inf。 如果您未在檔案名稱結尾明確輸入 **.inf** 並選取上述選項，檔案將儲存為文字檔案，而且將不會用於 CA 安裝。

9. 關閉 [記事本]。

> [!IMPORTANT]
>   在 CAPolicy.inf 中，您可以看到有一行，指定 URL https://pki.corp.contoso.com/pki/cps.txt。 CAPolicy.inf 的內部原則區段只是用來舉例說明如何指定憑證實施準則 (CPS) 的位置。 在本指南中，您不會指示來建立憑證實施 (準則 CPS)。
