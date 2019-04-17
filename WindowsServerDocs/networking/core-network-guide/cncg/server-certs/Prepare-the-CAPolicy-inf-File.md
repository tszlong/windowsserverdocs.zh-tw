---
title: 準備 CAPolicy.inf 檔案
description: CAPolicy.inf 包含安裝 Active Directory 認證服務 (AD CS) 或是約 CA 時所使用的各種設定憑證。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>CAPolicy.inf 語法
>   適用於：Windows Server（以每年次管道）、Windows Server 2016

CAPolicy.inf 是定義擴充功能、限制和其他設定可套用到根憑證及發行根 CA 所有憑證設定檔。 必須先根 CA 開始安裝程序的主機伺服器上安裝 CAPolicy.inf 檔案。 修改 ca 安全性限制時，必須更新根憑證，並更新的 CAPolicy.inf 檔案必須先安裝在伺服器上開始更新程序。

CAPolicy.inf 是：

-   建立和系統管理員所定義的以手動方式

-   利用期間建立根和附屬 CA 憑證

-   在您登入並發行憑證 (不能要求 CA) 簽章 CA 定義

當您建立您的 CAPolicy.inf 檔案之後時，您必須複製到**隱藏資料夾**安裝 ADC 或 CA 憑證之前 server 的資料夾。

CAPolicy.inf 可讓您可以指定並設定各種不同的 CA 屬性和選項。 下一節描述所有選項，讓您建立.inf 檔案量身打造，以您的需求。

## <a name="capolicyinf-file-structure"></a>CAPolicy.inf 檔案結構

下列條款用來描述.inf 檔案結構：

-   _區段_–是涵蓋邏輯按鍵群組的檔案使用的區域。 一節中.inf 檔案的名稱都會出現在 [括號。 區段許多，但並非全部，用來設定憑證擴充功能。

-   _按鍵_–名稱的項目，會顯示左邊等號。

-   _值_–參數，號右邊會出現。

範例所示，在**[版本]**區段，**簽章**是將金鑰，和**」\ $ Windows NT \ $]**是值。

範例：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>版本

將檔案辨識為.inf 檔案。 版本只需要一節和必須 CAPolicy.inf 檔案的開頭。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出定義組織的原則與是否有選用或管轄。 多個原則是以逗號分隔。 名稱或就自訂應用程式，檢查有這些原則的特定部署，環境中有的意義。

針對每個定義原則，必須要有一節，以定義特定原則設定。 針對每個原則，您必須向使用者定義物件識別碼 (OID) 和文字您想要顯示為政策或 URL 指標原則聲明。 URL 可以形式 HTTP、FTP 或 LDAP URL。

如果您要原則聲明中已描述文字，然後 CAPolicy.inf 的下一步三行想看起來像：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果您要使用的 URL 放 CA 原則聲明，然後接下來三行想改為看起來像：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

此外︰

-   多個 URL 與通知按鍵的支援。

-   通知和 URL 相同的原則一節中按鍵的支援。

-   必須以報價住空間 Url 或文字空間。 這適用於**URL**鍵，無論它出現的區段。

多個通知和 Url 原則一節中的範例想看起來像：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

您可以指定 CRL Distribution 點 (Cdp) 中 CAPolicy.inf 根憑證。 CA 安裝完成後，您可以設定 CA 包含每個發行憑證 CDP Url。 根憑證本身」包含 CAPolicy.inf 檔案的此一節中所指定的 Url。

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

本章節一些其他資訊：

-   多個 Url 的支援。

-   支援 HTTP、FTP 和 LDAP Url。 不支援 HTTPS Url。

-   如果您的設定 ca 或更新根憑證，才會使用此一節。 CA 問題附屬 CA 憑證，來判定附屬 CA CDP 擴充功能。

-   必須以報價住空間 Url。

-   如果不 Url 指定–也就是，如果**[CRLDistributionPoint]**區段在於檔案，但是空的–從 ca 憑證省略 CRL Distribution 點擴充功能。 設定 ca 時，這是通常較佳。 Windows 不會執行撤銷 CDP 擴充功能非必要中根憑證檢查根憑證、上。

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

您可以指定 CAPolicy.inf 的根憑證授權單位資訊的存取點。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

一些額外的授權單位資訊存取一節注意事項︰

-   多個 Url 的支援。

-   支援 HTTP、FTP、LDAP 和檔案的 Url。 不支援 HTTPS Url。

-   如果您的設定根或更新根憑證，才會使用此一節。 CA 發出附屬 CA 憑證，來判定附屬 CA AIA 擴充功能。

-   必須以報價住空間 Url。

-   如果不 Url 指定–也就是，如果**[AuthorityInformationAccess]**區段在於檔案，但是空的–從 ca 憑證省略 CRL Distribution 點擴充功能。 再試一次，這是在根憑證的偏好的設定為根憑證授權單位參考連結到其憑證會需要比未授權。

### <a name="certsrvserver"></a>certsrv_Server

另一個選擇性 CAPolicy.inf 區段是 [certsrv_server]，用來指定憑證授權單位是續約金鑰長度、續約，有效期和憑證撤銷清單 (CRL) 有效期正在更新或安裝。 都需要本節中的按鍵。 預設值是滿足大部分需求，且可以只是要省略 CAPolicy.inf 檔案從有許多這些設定。 或者，許多這些設定可以變更之後已安裝 CA。

範例看起來像：

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

**RenewalKeyLength**設定金鑰大小只續約。 這只用當新的金鑰期間 CA 憑證續約。 安裝 CA 設定金鑰初始 CA 憑證的大小。

當更新憑證，以新的金鑰，金鑰長度可以是提高或降低。 例如，如果您已設定根金鑰 CA 大小 4096 位元組或更高版本，並，然後找出您有 JAVA 應用程式或網路的裝置，可以僅限支援的 2048 位元組金鑰大小。 無論您提高或降低大小，您必須重新發出所有這個 CA 憑證的憑證。

**RenewalValidityPeriod**與**RenewalValidityPeriodUnits**建立的新根 CA 憑證期間時更新舊 CA 根憑證。 它只適用於 ca。 附屬 CA 憑證期間是由其上層判斷。 RenewalValidityPeriod 可以有下列值：小時、日期、星期、月份和年。

**CRLPeriod**與**CRLPeriodUnits**的基本 CRL 建立有效期。 **CRLPeriod**可以有下列值：小時、日期、星期、月份和年。

**CRLDeltaPeriod**與**CRLDeltaPeriodUnits** delta CRL 有效期進行通訊。 **CRLDeltaPeriod**可以有下列值：小時、日期、星期、月份和年。

這些設定可以設定 CA 安裝之後：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

請記得重新開機 Active Directory 憑證服務所做的變更才會生效。

**LoadDefaultTemplates**僅適用於企業 CA 安裝期間。 此設定，請為 True，或 \ [false\]（1 或是 0），是否已使用的預設範本 CA 規定。

CA 預設安裝時，預設憑證範本子集會新增到憑證授權單位嵌入式管理單元 [憑證範本 \] 資料夾。 這表示，只要 AD CS 服務開始安裝角色之後使用者或電腦的權限不足可以立即註冊憑證。

您可能不希望發行任何憑證已安裝 CA 之後，您可以使用 LoadDefaultTemplates 設定以防止預設範本新增至企業版 CA。 如果不有任何範本 CA 上設定就可以發行不到憑證。

**AlternateSignatureAlgorithm**設定 CA 憑證和憑證要求支援 PKCS\ #1 V2.1 簽章格式。 設定為上根 1 時 CA 憑證將會包含 PKCS\ #1 V2.1 簽章格式。 設定從時 CA、附屬 CA 會建立憑證要求，包含格式 PKCS\ #1 V2.1 簽章。

**ForceUTF8**變更預設的主題和發行者中相關分辨名稱 (Rdn) 編碼分辨 utf-8 的名稱。 只支援 utf-8，例如這些定義影響 Directory 字串類型 RFC，來為這些 Rdn。 例如，RDN 的網域元件 (DC) 支援編碼為 IA5 或 utf-8，Country RDN (C) 僅支援做為可列印字串編碼時。 ForceUTF8 指示詞因此會影響俠 RDN，但不是會影響 C RDN。

**EnableKeyCounting**設定每次使用 CA 簽署金鑰，請增加計數器 CA。 請不要此設定除非您有支援享有金鑰的相關聯的密碼編譯服務提供者 (CSP) 和硬體安全性模組」(HSM)。 非 Microsoft 強 CSP 也 Microsoft 軟體的金鑰儲存提供者 (KSP) 支援按鍵計算。


## <a name="create-the-capolicyinf-file"></a>建立 CAPolicy.inf 檔案

您安裝 AD CS 之前，您設定 CAPolicy.inf 檔案的特定設定為您的部署。

**必要條件：**您必須是系統管理員群組成員。

1.  在您的計劃安裝 AD CS，開放的 Windows PowerShell，電腦上輸入**「記事本」c:.inf**按下 ENTER。

2.  出現提示時，以建立新的檔案，請按一下**[是]**。

3.  輸入與檔案：
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
    [CRLDistributionPoint]  
    [AuthorityInformationAccess]
   ```
1.  按一下**檔案**，然後按**另存新檔**。

2.  瀏覽至 %systemroot%資料夾。

3.  請確定下列動作：

    -   **檔案名稱**設定為 [ **CAPolicy.inf**

    -   **另存新檔輸入**設定為 [**的所有檔案**

    -   **編碼]**是**ANSI**

4.  按一下**儲存**。

5.  當您接到覆寫的檔案時，請按一下**[是]**。

    ![另存新檔 CAPolicy.inf 檔案的位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   務必儲存 CAPolicy.inf inf 副檔名。 如果您不專門輸入**.inf**結尾的檔案名稱及選取的選項所述，檔案將會被儲存成文字檔案和 CA 安裝時將無法使用。

6.  關閉「記事本」。

>   [!IMPORTANT]  
>   在 CAPolicy.inf，您可以看到指定 URL 行http://pki.corp.contoso.com/pki/cps.txt。 只顯示的方式，您可以指定的位置 (CPS) 憑證做法聲明，例如 CAPolicy.inf 內部原則區段。 本指南，您不指示建立憑證做法聲明 (CPS)。
