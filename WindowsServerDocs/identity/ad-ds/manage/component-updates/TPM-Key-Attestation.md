---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: "TPM 金鑰證明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>TPM 金鑰證明

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
## <a name="overview"></a>概觀  
同時支援的 TPM 受鍵已經有 Windows 8，因為發生不 Ca 密碼編譯證明憑證的申請者私密金鑰確實由信賴平台模組」(TPM) 受保護的機制。 此更新讓 CA 和反映在發行的憑證，證明執行該證明。  
  
> [!NOTE]  
> 這篇文章假設讀取器熟悉憑證範本概念 (如需參考資料，請查看[憑證範本](https://technet.microsoft.com/library/cc730705.aspx))。 它也假設讀取器熟悉如何設定企業 Ca 憑證範本憑證的問題 (如需參考資料，[檢查清單︰ 設定的 Ca 管理憑證問題與](https://technet.microsoft.com/library/cc771533.aspx))。  
  
### <a name="terminology"></a>詞彙  
  
|詞彙|解析度|  
|--------|--------------|  
|EK|簽署金鑰。 這是 TPM（製造的時間在插入）中所包含的非對稱式鍵。 EK 是唯一的每個 TPM，以及找出。 EK 無法變更或移除。|  
|EKpub|指的是公用 EK 金鑰。|  
|EKPriv|指向私密金鑰 EK。|  
|EKCert|EK 憑證。 TPM 製造商發行憑證的 EKPub。 並非所有的 Tpm 擁有 EKCert。|  
|TPM|信賴平台模組。 TPM 設計目的是提供硬體式的安全性相關功能。 TPM 晶片都是安全的密碼編譯-處理器是設計用來執行密碼編譯作業。 晶片包括多個實體的安全機制，讓您竄改上，而無法竄改安全性功能的 TPM 惡意軟體。|  
  
### <a name="background"></a>背景  
開始使用 Windows 8，信賴平台模組 (TPM) 可用於安全性憑證中的私密金鑰。 Microsoft 平台密碼編譯提供者金鑰儲存提供者 (KSP) 可讓這個功能。 發生實作兩個問題：  

-   發生的按鍵確實受到（其他人輕鬆，欺騙軟體 KSP 為使用本機系統管理員認證 TPM KSP）TPM 不保證。

-   無法限制 Tpm 可保護企業（的系統管理員 PKI 想要控制類型的裝置，可以用來取得環境中的憑證）發行憑證的清單。  

### <a name="tpm-key-attestation"></a>TPM 金鑰證明  
TPM 金鑰證明是要求憑證實體的密碼編譯證明 CA 憑證要求 RSA 鍵受到」」或「」TPM CA 信任的能力。 TPM 信任模式中更多討論[部署概觀](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)之後本主題中的區段。  
  
### <a name="why-is-tpm-key-attestation-important"></a>為何很重要的 TPM 金鑰證明？  
TPM attested 金鑰憑證使用者提供更高的安全性保證備份非匯出性、反 hammering 和隔離的 TPM 所提供的按鍵。  
  
TPM 金鑰證明，以新的管理模式可現在：系統管理員可以定義的使用者可以使用公司的資源（例如，VPN 或 wireless 存取點）的存取，並讓裝置設定**穩固**保證任何其他裝置，可以用於存取它們。 這個新存取控制範例**穩固**因為它與*硬體繫結*使用者的身分，這是威力軟體認證。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 金鑰證明如何運作？  
一般而言，TPM 金鑰證明根據下列柱：  
  
1.  每個 TPM 隨附獨特的非對稱式金鑰，稱為*簽署金鑰*(EK)，燒錄製造商。 我們會將此鍵做為公開部分*EKPub*及相關的私密金鑰為*EKPriv*。 某些 TPM 晶片也會有核發給製造商 EKPub EK 憑證。 我們會將此憑證，以*EKCert*。  
  
2.  CA 建立信任透過 EKPub 或 EKCert 的 TPM 中。  
  
3.  密碼編譯 EKPub 相關所要求的認證 RSA 金鑰和使用者擁有 EKpriv ca 證明使用者。  
  
4.  CA 憑證問題 OID 代表按鍵會立即 attested 至受保護的 TPM 特殊 \ [發行原則。  
  
## <a name="BKMK_DeploymentOverview"></a>部署概觀  
部署，被假設 Windows Server 2012 R2 企業 CA 設定。 此外，針對企業 CA 使用註冊設定 (Windows 8.1) 戶端憑證範本。 

有三個步驟，以部署 TPM 金鑰證明：  
  
1.  **規劃 TPM 信任模式：**第一個步驟是可以選擇要使用的 TPM 信任模型。 有 3 個支援的方式執行此動作：  
  
    -   **信任根據使用者認證：**企業 CA 信任使用者提供 EKPub 憑證要求的一部分，並不會執行驗證以外的使用者網域認證。  
  
    -   **信任根據 EKCert:**企業 CA 驗證憑證要求的一部分提供系統管理員管理清單針對 EKCert 鏈結*可接受 EK 憑證鏈結*。 可接受鏈結定義的每個製造商，以在 CA（一個市集中繼），另一個用於 ca 憑證表示透過兩個自訂憑證存放區。 此信任」模式代表的**所有**指定製造商的 Tpm 所信賴。 請注意，此模式下，在 Tpm 使用中的環境中必須包含 EKCerts。
  
    -   **信任根據 EKPub:**企業 CA 驗證的提供系統管理員管理在清單中出現的憑證要求的一部分 EKPub 允許 EKPubs。 這份清單以表示的其中此 directory 中每個檔案的名稱是 SHA-2 湊允許 EKPub 的檔案。 這個選項會提供最高的保證層級，但是需要更多系統管理工作，因為排列可每個裝置。 在這個信任模式，已新增到允許清單中 EKPubs 他們 TPM 的 EKPub 的裝置允許註冊 TPM attested 憑證。  
  
    根據使用哪一種方法，CA 套用不同 \ [發行原則 OID 發行的憑證。 \ [發行原則 Oid 有關更多詳細資料，會看到的 \ [發行原則 Oid 表格[設定的憑證範本](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)本主題中的區段。  
  
    請注意，可以選擇 TPM 信任型號的組合。 若是如此，CA 將接受任何證明方法，以及 Oid 會反映成功的所有證明方法將發行原則。  
  
2.  **設定憑證範本：**中所述設定憑證範本[部署的詳細資料](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)一節中本主題。 本文章不會如何此憑證範本已指派給企業 CA 實體鍵盤保護蓋或如何註冊存取提供給使用者的群組。 如需詳細資訊，請查看[檢查清單︰ 設定的 Ca 管理憑證問題與](https://technet.microsoft.com/library/cc771533.aspx)。  
  
3.  **設定 CA TPM 信任模型**  
  
    1.  **信任根據使用者認證：**不需要任何特定的設定。  
  
    2.  **信任根據 EKCert:**系統管理員必須取得 EKCert 憑證鏈結 TPM 製造商，以及它們匯入到兩個新的憑證存放區，建立的系統管理員身分執行 TPM 金鑰證明憑證授權單位上。 如需詳細資訊，請查看[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    3.  **信任根據 EKPub:**系統管理員必須取得 EKPub 每個裝置將需要 TPM attested 憑證，並將他們新增到允許 EKPubs 的清單。 如需詳細資訊，請查看[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    > [!NOTE]  
    > -   這項功能會需要 Windows 8.1 / Windows Server 2012 R2。  
    > -   不支援的第三方智慧卡 KSPs TPM 金鑰證明。 Microsoft 平台密碼編譯提供者 KSP 必須使用。  
    > -   TPM 金鑰證明只適用於 RSA 按鍵。  
    > -   不支援的獨立 CA TPM 金鑰證明。  
    > -   TPM 金鑰證明不支援[非持續憑證處理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署的詳細資料  
  
### <a name="BKMK_ConfigCertTemplate"></a>設定的憑證範本  
若要設定的 TPM 金鑰證明憑證範本，執行下列設定步驟：  
  
1.  **相容性**的索引標籤  
  
    在**相容性設定**區段：  
  
    -   確認**Windows Server 2012 R2**選取**憑證授權單位**。  
  
    -   確認**Windows 8.1 / Windows Server 2012 R2**選取**憑證收件者**。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **密碼編譯**的索引標籤  
  
    確認**金鑰儲存提供者**已選取的**提供者分類**和**RSA**選取**演算法名稱**。 確認**要求必須使用其中一項下列提供者**選取和**Microsoft 的平台密碼編譯提供者**底下選取選項**提供者**。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **金鑰證明**的索引標籤  
  
    這是針對 Windows Server 2012 R2 的新索引標籤：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    從三個方式可以選擇證明模式。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **無：**表示金鑰證明不必須使用  
  
    -   **如果 client 需要：**允許使用者在 TPM 不支援的裝置上金鑰證明繼續註冊憑證。 使用者可以執行證明將進行區分與 OID 特殊 \ [發行原則。 部分裝置可能無法執行證明，而舊 TPM 不支援金鑰證明或不需要 TPM 在所有裝置。
  
    -   **所需的：** Client*必須*執行 TPM 金鑰證明，否則將會失敗憑證要求。  
  
    接下來的 TPM 信任模式。 再試一次選項共有三種：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **使用者的認證：**允許驗證有效的 TPM 保證來指定憑證網域中的使用者。  
  
    -   **簽署的憑證：**裝置的 EKCert 必須驗證透過管理員管理 TPM 中繼 CA 憑證管理員管理 ca 憑證。 如果您選擇此選項，您必須設定 EKCA 和 EKRoot 憑證存放區上 CA 中所述[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    -   **簽署金鑰：**裝置的 EKPub 必須 PKI 管理員管理清單中出現。 這個選項會提供的最高保證層級，但是需要更多系統努力。 如果您選擇此選項，您必須設定 CA EKPub 清單中所述[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    最後，選擇要顯示在發行的憑證的發行原則。 根據預設，每個執法類型都有它在下表中所述通過該執法類型，如果將插入憑證相關的物件識別碼 (OID)。 請注意，可以選擇組合執法方法。 若是如此，CA 將接受任一證明種方法，且將發行原則 OID 會反映出成功的所有證明方法。  
  
    **\ [發行原則 Oid**  
  
    |OID|金鑰證明類型|描述|保證層級|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|「EK 確認」: 的系統管理員管理 EK 清單|高|  
    |1.3.6.1.4.1.311.21.31|簽署的憑證|「EK 憑證確認」: EK 憑證鏈結驗證時|媒體|  
    |1.3.6.1.4.1.311.21.32|使用者的認證|「EK 信任上使用「: 的使用者 attested EK|低|  
  
    如果，將在發行憑證插入 Oid**包含 \ [發行原則**已選取（的預設設定）。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 可能使用的憑證中的 OID 是限制存取 VPN 或無線網路某些裝置。 例如，存取原則可能會允許連接（或存取不同的 VLAN）OID 1.3.6.1.4.1.311.21.30 是否出現在憑證。 這可讓您存取裝置的 TPM EK 存在於 EKPUB 清單中的限制。  
  
### <a name="BKMK_CAConfig"></a>CA 設定  
  
1.  **設定 EKCA 和 EKROOT 憑證存放區上 CA**  
  
    如果您選擇 [**簽署的憑證**範本設定中，進行下列設定步驟：  
  
    1.  使用 Windows PowerShell 來建立兩個新的憑證存放區上的憑證授權單位伺服器會執行 TPM 金鑰證明。  
  
    2.  取得中繼和根 CA 憑證製造商您想要讓您的企業環境中。 這些憑證必須匯入之前已建立憑證存放區（EKCA 和 EKROOT）視。  
  
    下列 Windows PowerShell 指令碼執行兩個步驟。 下列範例中，在 Fabrikam TPM 製造商已提供根憑證*FabrikamRoot.cer*發行 CA 憑證和*Fabrikamca.cer*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **如果使用 EK 證明類型，設定 EKPUB 清單**  
  
    如果您選擇 [**簽署金鑰**在範本設定中的下一步的設定步驟建立並設定在 CA，包含 0 位元組檔案的資料夾的允許 EK SHA-2 湊的每一個名為。 此資料夾做為 [允許清單中」的裝置，以取得 TPM 鍵 attested 憑證。 您必須手動新增的每個裝置需要 attested 的憑證 EKPUB，因為它提供企業版的裝置，以取得 TPM attested 的憑證授權保證。 設定此模式 CA 需要兩步驟：  
  
    1.  **建立 EndorsementKeyListDirectories 登錄項目：**使用 Certutil 命令列工具來設定下列表格中所述的受信任的 EKpubs 定義位置的資料夾位置的位置。  
  
        |操作|命令語法|  
        |-------------|------------------|  
        |新增資料夾位置|Certutil.exe-setreg CA\EndorsementKeyListDirectories +]<folder>「|  
        |移除資料夾位置|Certutil.exe-setreg CA\EndorsementKeyListDirectories-]<folder>「|  
  
        Certutil 命令中的 EndorsementKeyListDirectories 是登錄設定為下列表格中所述。  
  
        |值名稱|輸入|資料|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< 本機或 UNC EKPUB 路徑允許清單 ><br /><br />範例：<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories*會包含每個資料夾的 CA 讀取指向 UNC 或本機檔案系統路徑的清單。 每個資料夾可能包含零或更多允許清單項目，其中每個項目是檔案名稱的 SHA-2 hash 的受信任的 EKpub，不副檔名。 
        建立或編輯此登錄按鍵設定需要重新開機，就像現有 CA 登錄設定 CA。 不過，編輯設定立即才會生效，不需要將重新 CA。  
  
        > [!IMPORTANT]  
        > 安全遭到竄改清單中的資料夾和未經授權的存取，來設定，以便僅授權系統管理員的權限有讀取和寫入存取。 電腦的 CA 需要唯讀的權限。  
  
    2.  **填入 EKPUB 清單：**使用下列的 Windows PowerShell cmdlet 每個裝置上使用 Windows PowerShell 來取得公開金鑰的 TPM EK 湊和再傳送此公用鍵 ca hash，並將它儲存 EKPubList 資料夾。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>疑難排解  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>金鑰證明欄位並不適用於憑證範本  
金鑰證明欄位如果則無法使用範本設定不符合證明的需求。 常見的原因如下：  
  
1.  相容性設定不正確設定。 請務必設定方式如下：  
  
    1.  **憑證授權單位**: **Windows Server 2012 R2**  
  
    2.  **憑證收件者**: **Windows 8.1 / Windows Server 2012 R2**  
  
2.  密碼編譯未設定正確。 請務必設定方式如下：  
  
    1.  **提供者分類**:**金鑰儲存提供者**  
  
    2.  **演算法名稱**: **RSA**  
  
    3.  **提供者**: **Microsoft 平台密碼編譯提供者**  
  
3.  要求處理設定不正確設定。 請務必設定方式如下：  
  
    1.  **允許私密金鑰匯出**必須未選取選項。  
  
    2.  **保存主體加密私密金鑰**必須未選取選項。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>TPM 證明裝置的驗證  
使用 Windows PowerShell cmdlet、**確認-CAEndorsementKeyInfo**，以確認特定 TPM 裝置 Ca 信任的證明。 有兩個選項：一個用於驗證 EKCert，並確認 EKPub 其他。 Cmdlet 可以執行本機加拿大或遠端 Ca 使用 Windows PowerShell 遠端。  
  
1.  適用於驗證信任 EKPub 上的，執行下列兩個步驟：  
  
    1.  **從 client 電腦解壓縮 EKPub:** EKPub 可從電腦透過 client 擷取**TpmEndorsementKeyInfo 取得**。 從提升權限的命令提示字元中，執行下列動作：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **請確認 EKCert CA 的電腦上信任：**複製解壓縮的字串（SHA-2 湊 EKPub 的）伺服器（例如，透過電子郵件），並將它傳遞給確認-CAEndorsementKeyInfo cmdlet。 請注意，此參數必須 64 個字元。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  適用於驗證信任 EKCert 上的，執行下列兩個步驟：  
  
    1.  **從 client 電腦解壓縮 EKCert:** EKCert 可從電腦透過 client 擷取**TpmEndorsementKeyInfo 取得**。 從提升權限的命令提示字元中，執行下列動作：  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **請確認 KCert CA 的電腦上信任：**複製解壓縮的 EKCert (EkCert.cer) ca（例如，透過電子郵件或 xcopy）。 例如，如果您要複製 CA 伺服器上的憑證檔案的「c:\diagnose] 資料夾，執行完成驗證：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>也了  
[信賴平台模組技術概觀](https://technet.microsoft.com/library/jj131725.aspx)  
[外部資源：信賴平台模組](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


