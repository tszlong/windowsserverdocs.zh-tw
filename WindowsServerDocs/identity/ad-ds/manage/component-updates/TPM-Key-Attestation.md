---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: TPM 金鑰證明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d7104daaa10cf7093370cb309e0366e1ab2b9b51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389856"
---
# <a name="tpm-key-attestation"></a>TPM 金鑰證明

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**：Justin Turner，Microsoft 團隊的資深支援擴大工程師  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
雖然從 Windows 8 開始支援 TPM 保護的金鑰已經存在，但 Ca 無法以密碼編譯方式證明憑證要求者私密金鑰實際上是由信賴平臺模組（TPM）所保護。 此更新可讓 CA 執行該證明，並在已發行的憑證中反映該證明。  
  
> [!NOTE]  
> 本文假設讀者熟悉憑證範本概念（如需參考，請參閱[憑證範本](https://technet.microsoft.com/library/cc730705.aspx)）。 它也假設讀者熟悉如何設定企業 Ca 來根據憑證範本頒發證書（如需參考，請參閱 @no__t 0Checklist：設定 Ca 以發行和管理憑證 @ no__t-0）。  
  
### <a name="terminology"></a>詞彙  
  
|詞彙|定義|  
|--------|--------------|  
|EK|簽署金鑰。 這是包含在 TPM 中的非對稱金鑰（在製造時間插入）。 EK 對每個 TPM 而言都是唯一的，而且可以識別它。 無法變更或移除 EK。|  
|EKpub|參考 EK 的公開金鑰。|  
|EKPriv|參考 EK 的私密金鑰。|  
|EKCert|EK 憑證。 EKPub TPM 製造商發行的憑證。 並非所有的 Tpm 都有 EKCert。|  
|TPM|信賴平臺模組。 TPM 的設計目的是要提供硬體型安全性相關的功能。 TPM 晶片是安全的密碼編譯處理器，其設計目的是執行密碼編譯操作。 此晶片包含多個實體安全性機制，使它具備防竄改功能，在 TPM 安全性功能的加持下，惡意程式碼軟體便無法進行竄改。|  
  
### <a name="background"></a>背景  
從 Windows 8 開始，可使用信賴平臺模組（TPM）來保護憑證的私密金鑰。 Microsoft 平臺密碼編譯提供者金鑰儲存提供者（KSP）可啟用這項功能。 此執行有兩個考慮：  

-   不保證金鑰實際上受到 TPM 保護（有人可以使用本機系統管理員認證，輕鬆地將軟體 KSP 假冒為 TPM KSP）。

-   無法限制允許保護企業發行之憑證的 Tpm 清單（如果 PKI 系統管理員想要控制可用來在環境中取得憑證的裝置類型），這是不可能的。  

### <a name="tpm-key-attestation"></a>TPM 金鑰證明  
TPM 金鑰證明可讓要求憑證的實體以密碼編譯方式向 CA 證明，憑證要求中的 RSA 金鑰是由 CA 所信任的「a」或「TPM」所保護。 在本主題稍後的[部署總覽](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)一節中，會詳細討論 TPM 信任模型。  
  
### <a name="why-is-tpm-key-attestation-important"></a>為什麼 TPM 金鑰證明很重要？  
具有 TPM 證明金鑰的使用者憑證可提供更高的安全性保證, 由 TPM 所提供的非匯出性、反 anti-hammering 和金鑰隔離所備份。  
  
使用 TPM 金鑰證明，現在可以進行全新的管理範例：系統管理員可以定義一組裝置，讓使用者用來存取公司資源（例如，VPN 或無線存取點），並具有**強**式保證不會使用其他裝置來存取它們。 這個新的存取控制範例很**強**，因為它系結至*硬體*系結的使用者身分識別，這比以軟體為基礎的認證更強。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 金鑰證明如何運作？  
一般來說，TPM 金鑰證明是以下列支柱為基礎：  
  
1.  每個 TPM 都隨附一個唯一的非對稱金鑰，稱為*簽署金鑰*（EK），由製造商燒錄。 我們將此金鑰的公開部分稱為*EKPub* ，並將相關聯的私密金鑰稱為*EKPriv*。 有些 TPM 晶片也具有 EKPub 製造商所發行的 EK 憑證。 我們稱之為*EKCert*。  
  
2.  CA 會透過 EKPub 或 EKCert，在 TPM 中建立信任。  
  
3.  使用者向 CA 證明，要求憑證的 RSA 金鑰與 EKPub 相關聯，且使用者擁有該 EKpriv。  
  
4.  CA 會發行具有特殊發佈原則 OID 的憑證，表示金鑰現在已證明為受 TPM 保護。  
  
## <a name="BKMK_DeploymentOverview"></a>部署總覽  
在此部署中，假設已設定 Windows Server 2012 R2 企業 CA。 此外，用戶端（Windows 8.1）會設定為使用憑證範本對該企業 CA 進行註冊。 

部署 TPM 金鑰證明有三個步驟：  
  
1.  **規劃 TPM 信任模型：** 第一個步驟是決定要使用哪一個 TPM 信任模型。 有3種支援的方式可執行此作業：  
  
    -   **以使用者認證為基礎的信任：** 企業 CA 會信任使用者提供的 EKPub 做為憑證要求的一部分，而且不會執行使用者網域認證以外的驗證。  
  
    -   **以 EKCert 為基礎的信任：** 企業 CA 會根據系統管理員管理的*可接受的 EK 憑證鏈*清單，驗證所提供的 EKCert 鏈是否為憑證要求的一部分。 可接受的鏈會根據製造商定義，並透過發行 CA 上的兩個自訂憑證存放區來表示（一個存放區用於中繼，另一個用於根 CA 憑證）。 此信任模式表示指定制造商的**所有**tpm 都是受信任的。 請注意，在此模式中，環境中使用的 Tpm 必須包含 EKCerts。
  
    -   **以 EKPub 為基礎的信任：** 企業 CA 會驗證提供做為憑證要求一部分的 EKPub 會出現在允許 EKPubs 的系統管理員管理清單中。 此清單會表示為檔案目錄，其中此目錄中每個檔案的名稱是允許 EKPub 的 SHA-1 雜湊。 此選項可提供最高的保證層級，但需要更多管理工作，因為每個裝置都是個別識別的。 在此信任模型中，只有已將 TPM EKPub 新增至允許 EKPubs 清單的裝置，才能夠註冊 TPM 證明憑證。  
  
    視使用的方法而定，CA 會將不同的發佈原則 OID 套用至已發行的憑證。 如需發佈原則 Oid 的詳細資訊，請參閱本主題中[設定憑證範本](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)一節中的發行原則 oid 表格。  
  
    請注意，您可以選擇 TPM 信任模型的組合。 在此情況下，CA 將會接受任何證明方法，而發佈原則 Oid 會反映所有成功的證明方法。  
  
2.  **設定憑證範本：** 設定憑證範本的說明請參閱本主題中的[部署詳細資料](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)一節。 本文不涵蓋如何將此憑證範本指派給企業 CA，或如何將註冊存取權提供給一組使用者。 如需詳細資訊，請參閱 [Checklist：設定 Ca 以發行和管理憑證 @ no__t-0。  
  
3.  **設定 TPM 信任模型的 CA**  
  
    1.  **以使用者認證為基礎的信任：** 不需要特定設定。  
  
    2.  **以 EKCert 為基礎的信任：** 系統管理員必須從 TPM 製造商取得 EKCert 鏈憑證，並將它們匯入至系統管理員在執行 TPM 金鑰證明的 CA 上建立的兩個新憑證存放區。 如需詳細資訊，請參閱本主題中的[CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)設定一節。  
  
    3.  **以 EKPub 為基礎的信任：** 系統管理員必須為每個需要 TPM 證明憑證的裝置取得 EKPub，並將其新增至允許的 EKPubs 清單。 如需詳細資訊，請參閱本主題中的[CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)設定一節。  
  
    > [!NOTE]  
    > -   這項功能需要 Windows 8.1/Windows Server 2012 R2。  
    > -   不支援協力廠商智慧卡 Ksp 的 TPM 金鑰證明。 必須使用 Microsoft 平臺密碼編譯提供者 KSP。  
    > -   TPM 金鑰證明僅適用于 RSA 金鑰。  
    > -   獨立 CA 不支援 TPM 金鑰證明。  
    > -   TPM 金鑰證明不支援[非持續性憑證處理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署詳細資料  
  
### <a name="BKMK_ConfigCertTemplate"></a>設定憑證範本  
若要設定 TPM 金鑰證明的憑證範本，請執行下列設定步驟：  
  
1.  **相容性**索引標籤  
  
    在 [**相容性設定**] 區段中：  
  
    -   請確定已為**憑證授權單位**單位選取**Windows Server 2012 R2** 。  
  
    -   確定已為**憑證收件**者選取**Windows 8.1/Windows Server 2012 R2** 。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **密碼**編譯索引標籤  
  
    確定已針對 [**提供者] 類別**選取 [**金鑰儲存提供者**]，並已針對 [**演算法名稱]** 選取 [ **RSA** ]。 確定 [**要求必須使用下列其中一個提供者**]，而且已選取 [**提供者**] 底下的 [ **Microsoft 平臺密碼編譯提供者**] 選項。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  [**金鑰證明**] 索引標籤  
  
    這是 Windows Server 2012 R2 的新索引標籤：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    從三個可能的選項中選擇證明模式。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **無**表示不得使用金鑰證明  
  
    -   **必要，如果用戶端能夠：** 允許不支援 TPM 金鑰證明的裝置上的使用者繼續註冊該憑證。 可以執行證明的使用者會與特殊發佈原則 OID 進行區別。 某些裝置可能無法執行證明，因為舊的 TPM 不支援金鑰證明，或裝置完全沒有 TPM。
  
    -   **必填：** 用戶端*必須*執行 TPM 金鑰證明，否則憑證要求將會失敗。  
  
    然後選擇 TPM 信任模型。 另外還有三個選項：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **使用者認證：** 藉由指定網域認證，讓驗證使用者擔保有效的 TPM。  
  
    -   **簽署憑證：** 裝置的 EKCert 必須透過系統管理員管理的 TPM 中繼 CA 憑證，驗證為系統管理員管理的根 CA 憑證。 如果您選擇此選項，您必須在發行 CA 上設定 EKCA 和 EKRoot 憑證存放區，如本主題的[CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)設定一節所述。  
  
    -   **簽署金鑰：** 裝置的 EKPub 必須出現在 [PKI 系統管理員管理] 清單中。 此選項可提供最高的保證層級，但需要更多的系統管理工作。 如果您選擇此選項，您必須在發行 CA 上設定 EKPub 清單，如本主題的[CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)設定一節所述。  
  
    最後，決定要在已發行的憑證中顯示哪一個發佈原則。 根據預設，每個強制型別都有相關聯的物件識別碼（OID），如果它通過該強制型別，就會插入到憑證中，如下表所述。 請注意，您可以選擇強制方法的組合。 在此情況下，CA 將會接受任何證明方法，而發佈原則 OID 會反映所有成功的證明方法。  
  
    **發行原則 Oid**  
  
    |OID|金鑰證明類型|描述|保證層級|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|「EK 已驗證」： 適用于系統管理員管理的 EK 清單|高|  
    |1.3.6.1.4.1.311.21.31|簽署憑證|「EK 憑證已驗證」：驗證 EK 憑證鏈時|中等|  
    |1.3.6.1.4.1.311.21.32|使用者認證|"EK Trusted on Use"：針對使用者-證明 EK|低|  
  
    如果已選取 [**包含發佈原則**] （預設設定），則會將 oid 插入已發行的憑證。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 憑證中有 OID 的可能用途之一，就是限制對特定裝置的 VPN 或無線網路存取。 例如，如果憑證中有 OID 1.3.6.1.4.1.311.21.30，則您的存取原則可能會允許連接（或存取不同的 VLAN）。 這可讓您限制 EKPUB 清單中有 TPM EK 的裝置的存取權。  
  
### <a name="BKMK_CAConfig"></a>CA 設定  
  
1.  **在發行 CA 上設定 EKCA 和 EKROOT 憑證存放區**  
  
    如果您為範本設定選擇 [**簽署憑證**]，請執行下列設定步驟：  
  
    1.  使用 Windows PowerShell，在將執行 TPM 金鑰證明的憑證授權單位單位（CA）伺服器上建立兩個新的憑證存放區。  
  
    2.  從您想要在企業環境中允許的製造商取得中繼和根 CA 憑證。 這些憑證必須視需要匯入先前建立的憑證存放區（EKCA 和 EKROOT）。  
  
    下列 Windows PowerShell 腳本會執行這兩個步驟。 在下列範例中，TPM 製造商 Fabrikam 已提供根憑證*FabrikamRoot*和發行 CA 憑證*contoso-fabrikamca*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **使用 EK 證明類型時的安裝 EKPUB 清單**  
  
    如果您在範本設定中選擇 [**簽署金鑰**]，則下一個設定步驟是在發行 CA 上建立及設定一個資料夾，其中包含0個位元組的檔案，每個檔案都是針對允許的 EK 的 sha-1 雜湊進行命名。 此資料夾可作為允許取得 TPM 金鑰證明憑證之裝置的「允許清單」。 因為您必須針對每個需要證明憑證的裝置手動新增 EKPUB，所以它會為企業提供保證取得 TPM 金鑰證明憑證的裝置。 設定此模式的 CA 需要兩個步驟：  
  
    1.  **建立 EndorsementKeyListDirectories 登錄專案：** 使用 Certutil 命令列工具來設定資料夾位置，其中定義了信任的 EKpubs，如下表所述。  
  
        |運算|命令語法|  
        |-------------|------------------|  
        |新增資料夾位置|certutil-setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |移除資料夾位置|certutil-setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        Certutil 命令中的 EndorsementKeyListDirectories 是一項登錄設定，如下表所述。  
  
        |值名稱|Type|Data|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< 本機或 UNC 路徑 EKPUB 允許清單 ><br /><br />範例：<br /><br />*\\ \ blueCA. contoso. com\ekpub*<br /><br />*\\ \ bluecluster1. contoso. com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration @ no__t-0 @ no__t-1  
  
        *EndorsementKeyListDirectories*會包含 UNC 或本機檔案系統路徑的清單，每個檔案都指向 CA 擁有讀取權限的資料夾。 每個資料夾可能包含零個或多個允許清單專案，其中每個專案都是名稱是受信任 EKpub 之 SHA-1 雜湊的檔案，不含副檔名。 
        建立或編輯此登錄機碼設定需要重新開機 CA，就像現有的 CA 登錄設定一樣。 不過，對 configuration 設定的編輯會立即生效，且不需要重新開機 CA。  
  
        > [!IMPORTANT]  
        > 藉由設定許可權來保護清單中的資料夾，使其無法進行篡改和未經授權的存取，如此一來，只有經過授權的系統管理員才可讀取 CA 的電腦帳戶需要唯讀存取權。  
  
    2.  **填入 EKPUB 清單：** 使用下列 Windows PowerShell Cmdlet，在每個裝置上使用 Windows PowerShell 來取得 TPM EK 的公開金鑰雜湊，然後將此公開金鑰雜湊傳送到 CA，並將其儲存在 EKPubList 資料夾中。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>疑難排解  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>憑證範本無法使用金鑰證明欄位  
如果範本設定不符合證明的需求，則無法使用 [金鑰證明] 欄位。 常見的原因如下：  
  
1.  相容性設定未正確設定。 請確定其設定如下：  
  
    1.  **憑證授權單位**：**Windows Server 2012 R2**  
  
    2.  **憑證收件**者：**Windows 8.1/Windows Server 2012 R2**  
  
2.  密碼編譯設定未正確設定。 請確定其設定如下：  
  
    1.  **提供者分類**：**金鑰儲存提供者**  
  
    2.  **演算法名稱**：**與眾不同**  
  
    3.  **提供者**:**Microsoft 平臺密碼編譯提供者**  
  
3.  要求處理設定未正確設定。 請確定其設定如下：  
  
    1.  不得選取 [**允許匯出私密金鑰**] 選項。  
  
    2.  不得選取 [封存**主體的加密私密金鑰**] 選項。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>驗證 TPM 裝置以進行證明  
使用 Windows PowerShell Cmdlet **CAEndorsementKeyInfo 確認**特定的 TPM 裝置是否受信任，以供 ca 進行證明。 有兩個選項：一個用於驗證 EKCert，另一個用於驗證 EKPub。 Cmdlet 會在 CA 的本機上執行，或使用 Windows PowerShell 遠端處理在遠端 Ca 上執行。  
  
1.  若要驗證 EKPub 的信任，請執行下列兩個步驟：  
  
    1.  **從用戶端電腦解壓縮 EKPub：** EKPub 可以透過**TpmEndorsementKeyInfo**從用戶端電腦解壓縮。 從提高許可權的命令提示字元中，執行下列程式：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **確認 CA 電腦上 EKCert 的信任：** 將解壓縮的字串（EKPub 的 SHA-1 雜湊）複製到伺服器（例如，透過電子郵件），並將它傳遞給 Confirm CAEndorsementKeyInfo 指令程式。 請注意，此參數必須是64個字元。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  若要驗證 EKCert 的信任，請執行下列兩個步驟：  
  
    1.  **從用戶端電腦解壓縮 EKCert：** EKCert 可以透過**TpmEndorsementKeyInfo**從用戶端電腦解壓縮。 從提高許可權的命令提示字元中，執行下列程式：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **確認 CA 電腦上 EKCert 的信任：** 將解壓縮的 EKCert （EkCert .cer）複製到 CA （例如，透過電子郵件或 xcopy）。 例如，如果您將憑證檔案複製到 CA 伺服器上的 "c:\diagnose" 資料夾，請執行下列動作以完成驗證：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>另請參閱  
[信賴平臺模組技術總覽](https://technet.microsoft.com/library/jj131725.aspx)  
@no__t 0External 資源：信賴平臺模組 @ no__t-0  
