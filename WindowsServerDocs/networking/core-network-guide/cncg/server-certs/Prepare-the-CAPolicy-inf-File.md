---
title: 準備 CAPolicy.inf 檔案
description: Capolicy.inf 包含各種設定，在安裝 Active Directory 憑證服務（AD CS）或更新 CA 憑證時使用。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb2e25dcd27ed3046eeeb444a9f167ccff6e1dd3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868955"
---
# <a name="capolicyinf-syntax"></a>Capolicy.inf .inf 語法
>   適用於：Windows Server (半年度管道)、Windows Server 2016

Capolicy.inf 是定義延伸模組、條件約束和其他設定的設定檔，這些設定會套用至根 CA 憑證和根 CA 發行的所有憑證。 在根 CA 的安裝程式常式開始之前，必須先在主機伺服器上安裝 Capolicy.inf .inf 檔案。 當根 CA 的安全性限制要修改時，必須更新根憑證，而且必須先在伺服器上安裝更新的 Capolicy.inf .inf 檔案，然後才能開始續訂程式。

Capolicy.inf 是：

-   由系統管理員手動建立和定義

-   在建立根和次級 CA 憑證時使用

-   定義于簽署 CA 上，您簽署併發行憑證（而不是授與要求的 CA）

建立 Capolicy.inf .inf 檔案之後，您必須先將它複製到伺服器的 **% systemroot%** 資料夾，再安裝 ADCS 或更新 CA 憑證。

Capolicy.inf 可以指定及設定各種不同的 CA 屬性和選項。 下一節描述的所有選項，可讓您建立專為您的特定需求而量身打造的 .inf 檔案。

## <a name="capolicyinf-file-structure"></a>Capolicy.inf .inf 檔案結構

下列詞彙是用來描述 .inf 檔案結構：

-   _Section_ –是檔案的一個區域，其中涵蓋索引鍵的邏輯群組。 .Inf 檔案中的區段名稱會以方括弧顯示。 許多（但非全部）區段都是用來設定憑證延伸。

-   _金鑰_–是專案的名稱，並出現在等號的左邊。

-   _Value_ –是參數，並出現在等號的右邊。

在下列範例中， **[Version]** 是區段， **Signature**是 key，而 **"\$Windows\$NT"** 是值。

範例：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

將檔案識別為 .inf 檔案。 版本是唯一必要的區段，而且必須在 Capolicy.inf .inf 檔案的開頭。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出組織所定義的原則，以及它們是選擇性或強制的。 多個原則會以逗號分隔。 這些名稱在特定部署的內容中具有意義，或與檢查這些原則是否存在的自訂應用程式相關。

針對每個定義的原則，必須有一個區段定義該特定原則的設定。 針對每個原則，您必須提供使用者定義物件識別碼（OID），以及您想要顯示為原則語句的文字或原則語句的 URL 指標。 此 URL 可以是 HTTP、FTP 或 LDAP URL 的格式。

如果您要在原則語句中有描述性文字，則 Capolicy.inf 的接下來三行看起來會像這樣：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果您要使用 URL 來裝載 CA 原則語句，接下來的三行會如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

其他情況：

-   支援多個 URL 和通知金鑰。

-   同時也支援相同原則區段中的 [通知] 和 [URL] 索引鍵。

-   含有空格的 Url 或包含空格的文字必須以引號括住。 這適用于**URL**索引鍵，不論其出現的區段為何。

原則區段中的多個通知和 Url 範例如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

您可以為 Capolicy.inf 中的根 CA 憑證指定 CRL 發佈點（Cdp）。  安裝 CA 之後，您可以設定 CA 在每個發出的憑證中所包含的 CDP Url。 根 CA 憑證會顯示在 Capolicy.inf 的這個區段中所指定的 Url。 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

本節的一些額外資訊：
-   支援
    - HTTP 
    - 檔案 Url
    - LDAP Url 
    - 多個 Url
   
    >[!IMPORTANT]
    >不支援 HTTPS Url。

-   引號必須以空格括住 Url。

-   如果未指定任何 Url （也就是，如果檔案中有 **[CRLDistributionPoint]** 區段，但卻是空的），則根 CA 憑證會省略授權單位資訊存取延伸模組。 這通常是設定根 CA 時的理想選項。 Windows 不會對根 CA 憑證執行撤銷檢查，因此，CDP 延伸模組在根 CA 憑證中是多餘的。

-    例如，CA 可以發佈至檔案 UNC，以代表用戶端透過 HTTP 抓取之網站資料夾的共用。

-   只有在您要設定根 CA 或更新根 CA 憑證時，才使用此區段。 CA 會決定次級 CA CDP 延伸模組。
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

您可以在 Capolicy.inf 中指定根 CA 憑證的授權資訊存取點。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

授權資訊存取區段上的一些其他注意事項：

-   支援多個 Url。

-   支援 HTTP、FTP、LDAP 和檔案 Url。 不支援 HTTPS Url。

-   只有在您要設定根 CA，或更新根 CA 憑證時，才會使用此區段。 次級 CA AIA 延伸是由發行次級 CA 憑證的 CA 所決定。

-   具有空格的 Url 必須以引號括住。

-   如果未指定任何 Url （也就是，如果檔案中有 **[AuthorityInformationAccess]** 區段，但卻是空的），則會省略根 CA 憑證中的 CRL 發佈點延伸模組。 同樣地，在根 CA 憑證的情況下，這會是慣用的設定，因為沒有任何授權高於根 CA，而其憑證的連結必須參考它。

### <a name="certsrv_server"></a>certsrv_Server

Capolicy.inf 的另一個選擇性區段是 [certsrv_server]，用來指定要更新或安裝之 CA 的更新金鑰長度、更新有效期間和憑證撤銷清單（CRL）有效期間。 本節中沒有任何索引鍵是必要的。 這些設定中有許多都有足夠的預設值，足以滿足大部分的需求，而且可以直接從 Capolicy.inf 中省略。 或者，您也可以在安裝 CA 之後變更其中許多設定。

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

**RenewalKeyLength**只會設定更新的金鑰大小。 這只會在 CA 憑證更新期間產生新的金鑰組時使用。 在安裝 CA 時，會設定初始 CA 憑證的金鑰大小。

以新的金鑰組來更新 CA 憑證時，金鑰長度可以是增加或減少。 例如，如果您已將根 CA 金鑰大小設定為4096個位元組或更高，然後發現您的 JAVA 應用程式或網路裝置只能支援2048個位元組的金鑰大小。 無論您增加或減少大小，都必須重新發出該 CA 發行的所有憑證。

**RenewalValidityPeriod**和**RenewalValidityPeriodUnits**會在更新舊的根 ca 憑證時，建立新根 ca 憑證的存留期。 它只適用于根 CA。 次級 CA 的憑證存留期取決於其上層。 RenewalValidityPeriod 可以有下列值：小時、天、周、月和年。

**CRLPeriod**和**CRLPeriodUnits**會建立基底 CRL 的有效期間。 **CRLPeriod**可以有下列值：小時、天、周、月和年。

**CRLDeltaPeriod**和**CRLDeltaPeriodUnits**會建立 delta CRL 的有效期間。 **CRLDeltaPeriod**可以有下列值：小時、天、周、月和年。

在安裝 CA 之後，您可以設定每個設定：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

請記得重新開機 Active Directory 憑證服務，變更才會生效。

**LoadDefaultTemplates**只適用于安裝企業 CA 的期間。 此設定為 True 或 False （或1或0），會指示 CA 是否設定為使用任何預設範本。

在 CA 的預設安裝中，預設憑證範本的子集會新增至 [憑證授權單位單位] 嵌入式管理單元的 [憑證範本] 資料夾中。 這表示當 AD CS 服務在安裝角色之後，一旦使用者或電腦具有足夠的許可權，就可以立即註冊憑證。

您可能不想在安裝 CA 之後立即發行任何憑證，因此您可以使用 LoadDefaultTemplates 設定來防止將預設範本新增到企業 CA。 如果 CA 上未設定任何範本，則它不會發出任何憑證。

**AlternateSignatureAlgorithm**會設定 ca，以支援 ca\#憑證和憑證要求的 PKCS 1 v 2.1 簽章格式。 在根 CA 上設定為1時，CA 憑證將包含 PKCS\#1 2.1 版簽章格式。 在次級 CA 上設定時，次級 CA 會建立包含 PKCS\#1 v 2.1 簽章格式的憑證要求。

**ForceUTF8**會將主體和簽發者辨別名稱中的相對辨別名稱（RDNs）的預設編碼變更為 utf-8。 只有支援 UTF-8 的 RDNs （例如，由 RFC 定義為目錄字串類型的）會受到影響。 例如，網域元件（DC）的 RDN 支援將編碼成 IA5 或 UTF-8，而國家/地區 RDN （C）只支援編碼為可列印的字串。 因此，ForceUTF8 指示詞會影響 DC RDN，但不會影響 C RDN。

**EnableKeyCounting**會設定 ca 在每次使用 ca 的簽署金鑰時遞增計數器。 除非您有支援金鑰計數的硬體安全性模組（HSM）和相關聯的密碼編譯服務提供者（CSP），否則請勿啟用此設定。 Microsoft 強式 CSP 或 Microsoft 軟體金鑰儲存提供者（KSP）都不支援金鑰計數。


## <a name="create-the-capolicyinf-file"></a>建立 Capolicy.inf .inf 檔案

在安裝 AD CS 之前，您可以使用部署的特定設定來設定 Capolicy.inf .inf 檔案。

**必要**您必須是 Administrators 群組的成員。

1. 在您打算安裝 AD CS 的電腦上，開啟 Windows PowerShell，輸入**notepad c:\CAPolicy.inf** ，然後按 enter。

2. 當系統提示您建立新檔案時，按一下 [是]。

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
4. 按一下 [檔案 **]，然後按一下 [** **另存**新檔]。

5. 流覽至% systemroot% 資料夾。

6. 確認下列事項：

   -   [檔案名稱] 設為 **CAPolicy.inf**

   -   [存檔類型] 設定為 [所有檔案]

   -   [編碼] 為 **[ANSI]**

7. 按一下 [儲存]。

8. 當系統提示您覆寫檔案時，按一下 [是]。

   ![Capolicy.inf .inf 檔案的另存新檔位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   務必以 inf 副檔名儲存 CAPolicy.inf。 如果您未在檔案名稱結尾明確輸入 **.inf** 並選取上述選項，檔案將儲存為文字檔案，而且將不會用於 CA 安裝。

9. 關閉 [記事本]。

> [!IMPORTANT]
>   在 Capolicy.inf 中，您可以看到有一行指定 URL https://pki.corp.contoso.com/pki/cps.txt 。 CAPolicy.inf 的內部原則區段只是用來舉例說明如何指定憑證實施準則 (CPS) 的位置。 在本指南中，不會指示您建立憑證實務聲明（CPS）。
