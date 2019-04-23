---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: TPM 金鑰證明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3cfc047fe1a66617abbda1de5f2c5842dcbdeb12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862879"
---
# <a name="tpm-key-attestation"></a>TPM 金鑰證明

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

**作者**:Justin Turner 資深支援高階工程師，與 Windows 群組  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
同時支援的 TPM 保護的金鑰自以來已存在 Windows 8 中，沒有任何 Ca 以密碼編譯方式證明憑證要求者私密金鑰實際上受信賴平台模組 (TPM) 有保護機制。 此更新可讓 CA 執行的證明，並反映在發行的憑證的證明。  
  
> [!NOTE]  
> 本文假設讀者已熟悉使用憑證範本概念 (如需參考，請參閱[憑證範本](https://technet.microsoft.com/library/cc730705.aspx))。 它也會假設讀者已熟悉如何設定企業 Ca 根據憑證範本發行憑證 (如需參考，請參閱[檢查清單：設定 Ca 以發行和管理憑證](https://technet.microsoft.com/library/cc771533.aspx))。  
  
### <a name="terminology"></a>詞彙  
  
|詞彙|定義|  
|--------|--------------|  
|EK|簽署金鑰。 這是在 TPM （插入在製造時間） 內的非對稱金鑰。 EK 對每個 TPM 是唯一的而且可以識別它。 無法變更或移除 EK。|  
|EKpub|指的是公開金鑰 EK。|  
|EKPriv|參考的 EK 私用的索引鍵。|  
|EKCert|EK 憑證。 TPM 製造商發行憑證 EKPub。 並非所有 tpm 都有 EKCert。|  
|TPM|信賴平台模組。 TPM 被設計來提供硬體為基礎的安全性相關功能。 TPM 晶片是安全的密碼編譯處理器，其設計目的是執行密碼編譯操作。 此晶片包含多個實體安全性機制，使它具備防竄改功能，在 TPM 安全性功能的加持下，惡意程式碼軟體便無法進行竄改。|  
  
### <a name="background"></a>背景  
從 Windows 8 開始，受信任的平台模組 (TPM) 可用來保護憑證的私密金鑰。 Microsoft 平台密碼編譯提供者金鑰儲存提供者 (KSP) 可讓這項功能。 沒有實作的兩個考量：  

-   沒有索引鍵實際受的 TPM （有心人士可以輕鬆地欺騙軟體 KSP 做為使用本機系統管理員認證的 TPM KSP） 保證。

-   無法限制可保護已發行的憑證 （的 PKI 系統管理員想要控制的裝置，可用來取得環境中的憑證類型） 的企業的 Tpm 的清單。  

### <a name="tpm-key-attestation"></a>TPM 金鑰證明  
TPM 金鑰證明是要求憑證之實體的能力，以密碼編譯方式證明 CA 憑證要求中的 RSA 金鑰受到"a"或"the"CA 信任的 TPM。 TPM 信任模型會更詳細的討論[部署概觀](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)本主題稍後的章節。  
  
### <a name="why-is-tpm-key-attestation-important"></a>TPM 金鑰證明為何如此重要？  
使用 TPM 證明金鑰的使用者憑證提供更高的安全性保證，由非匯出性、 防止攻擊，和隔離的 TPM 所提供的索引鍵。  
  
TPM 金鑰證明，新的管理架構，就可以：系統管理員可以定義一組使用者可用來存取公司資源 （例如 VPN 或無線存取點），以及擁有的裝置**強式**保證沒有其他裝置，可用來存取它們。 這個新的存取控制開發架構是**強**因為它會繫結至*硬體繫結*使用者身分識別，也就是以軟體為基礎的認證比更強。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 金鑰證明如何運作？  
一般情況下，TPM 金鑰證明根據下列方針：  
  
1.  每個 TPM 隨附唯一的非對稱金鑰，稱為*簽署金鑰*(EK)，燒錄製造商。 我們將這個金鑰的公開部分*EKPub*並為相關聯的私密金鑰*EKPriv*。 某些 TPM 晶片也有核發給製造商 EKPub EK 憑證。 我們將為此憑證*EKCert*。  
  
2.  CA 會在 TPM 透過 EKPub 或 EKCert 的信任。  
  
3.  向 CA 的事件，是正在要求憑證的 RSA 金鑰為密碼編譯相關 EKPub 和使用者擁有 EKpriv 證明的使用者。  
  
4.  在 CA 簽發憑證與特殊的發行原則代表索引鍵現在受 TPM 通過證明的 OID。  
  
## <a name="BKMK_DeploymentOverview"></a>部署概觀  
在此部署中，它會假設已設定 Windows Server 2012 R2 的企業 CA。 此外，用戶端 (Windows 8.1) 設定為針對使用企業 CA 註冊憑證範本。 

有三個步驟部署 TPM 金鑰證明：  
  
1.  **計劃的 TPM 信任模型：** 第一個步驟是決定要使用哪一個 TPM 信任模型。 有 3 個支援的方式執行此動作：  
  
    -   **根據使用者認證的信任：** 企業 CA 信任使用者提供 EKPub 憑證要求的一部分，並不會執行驗證以外使用者的網域認證。  
  
    -   **根據 EKCert 的信任：** 企業 CA 會驗證憑證要求的一部分提供的系統管理員管理清單對 EKCert 鏈結*可接受的 EK 憑證鏈結*。 可接受的鏈結所定義的每個製造商，並在發行 CA （一個中繼存放區），另一個用於根 CA 憑證以兩個自訂憑證存放區。 這個信任模式表示**所有**從某一特定製造商的 Tpm 是受信任。 請注意，在此模式中，在環境中使用的 Tpm 必須包含 EKCerts。
  
    -   **根據 EKPub 的信任：** 企業 CA 驗證所提供的憑證要求的組件出現在系統管理員管理清單中的 EKPub 允許 EKPubs。 這份清單會表示為其中的此目錄中的每個檔案名稱是允許 EKPub 的 sha-2 雜湊的檔案的目錄。 此選項提供最高的保證層級，但需要更多的系統管理工作，因為個別識別每個裝置。 在此信任模型中，只有已新增至 EKPubs 允許清單的使用者的 TPM EKPub 的裝置，才允許註冊 TPM 通過證明的憑證。  
  
    根據使用哪一種方法時，CA 將發行的憑證中套用不同的發行原則 OID。 如需進一步瞭解發佈原則 Oid 的詳細資訊，請參閱發行原則 Oid 的表格[設定的憑證範本](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)本主題中的區段。  
  
    請注意，可選擇 TPM 信任模型的組合。 在此情況下，CA 將接受任一項證明方法和 Oid 會反映所有成功的證明方法的發行原則。  
  
2.  **設定憑證範本：** 設定憑證範本所述[部署詳細資料](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)本主題中的區段。 這篇文章並未涵蓋如何將此憑證範本指派到企業 CA 或如何註冊存取權提供給一群使用者。 如需詳細資訊，請參閱[檢查清單：設定 Ca 以發行和管理憑證](https://technet.microsoft.com/library/cc771533.aspx)。  
  
3.  **設定 CA 進行的 TPM 信任模型**  
  
    1.  **根據使用者認證的信任：** 不不需要任何特定的設定。  
  
    2.  **根據 EKCert 的信任：** 系統管理員必須從 TPM 製造商取得 EKCert 鏈結憑證，並將它們匯入兩個新憑證存放區，系統管理員執行 TPM 金鑰證明在 CA 上建立。 如需詳細資訊，請參閱 < [CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    3.  **根據 EKPub 的信任：** 系統管理員必須取得每個裝置，會需要 TPM 通過證明的憑證，並將其新增至允許的 EKPubs EKPub。 如需詳細資訊，請參閱 < [CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    > [!NOTE]  
    > -   這項功能需要 Windows 8.1 / Windows Server 2012 R2。  
    > -   不支援 TPM 金鑰證明的協力廠商的智慧卡 Ksp。 您必須使用 Microsoft 平台密碼編譯提供者的 KSP。  
    > -   TPM 金鑰證明僅適用於 RSA 金鑰。  
    > -   TPM 金鑰證明不支援獨立 CA。  
    > -   TPM 金鑰證明不支援[非持續性憑證處理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署詳細資料  
  
### <a name="BKMK_ConfigCertTemplate"></a>設定憑證範本  
若要設定 TPM 金鑰證明的憑證範本，請執行下列設定步驟：  
  
1.  **相容性** 索引標籤  
  
    在 **相容性設定**區段：  
  
    -   請確定**Windows Server 2012 R2**選取**憑證授權單位**。  
  
    -   請確定**Windows 8.1 / Windows Server 2012 R2**選取**憑證收件者**。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **密碼編譯** 索引標籤  
  
    請確定**金鑰儲存提供者**選取**提供者類別目錄**並**RSA**選取**演算法名稱**。 請確定**要求必須使用下列提供者的其中一個**已選取和**Microsoft 平台密碼編譯提供者**選項底下**提供者**。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **金鑰證明** 索引標籤  
  
    這是適用於 Windows Server 2012 R2 的新索引標籤：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    選擇證明模式從三個可能的選項。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None:** 表示必須不會用金鑰證明  
  
    -   **必要的如果用戶端能夠：** 不支援 TPM 金鑰證明，以繼續該憑證註冊的裝置上，可讓使用者。 使用者可以執行證明會區別與特殊的發行原則 OID。 某些裝置可能無法執行證明，因為舊的 TPM 金鑰證明或沒有 TPM 的所有裝置不支援。
  
    -   **必要：** 用戶端*必須*執行 TPM 金鑰證明，否則憑證要求將會失敗。  
  
    然後選擇 TPM 信任模式。 一次，有三個選項：  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **使用者認證：** 允許驗證的使用者來擔保為有效的 tpm，藉由指定其網域認證。  
  
    -   **簽署憑證：** 裝置的 EKCert 必須透過系統管理員管理 TPM 中繼 CA 憑證以系統管理員管理的根 CA 憑證進行驗證。 如果您選擇此選項時，您必須設定 EKRROT 和 EKRoot 憑證存放區，在發行 CA 上的，如中所述[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    -   **簽署金鑰：** 裝置的 EKPub 必須出現在 PKI 系統管理員管理清單中。 此選項提供最高的保證層級，但需要更多的系統管理工作。 如果您選擇此選項時，您必須設定發行 CA 上 EKPub 清單中所述[CA 設定](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主題中的區段。  
  
    最後，決定要顯示在發行的憑證發行原則。 根據預設，每個強制類型都有相關聯的物件識別碼 (OID)，會插入到憑證中，如果通過該強制型別下, 表中所述。 請注意，可選擇強制方法的組合。 在此情況下，CA 將接受任一證明方法，並發佈原則 OID 會反映所有成功的證明方法。  
  
    **發行原則 Oid**  
  
    |OID|金鑰證明類型|描述|保證層級|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|「 EK 驗證 」: 如需系統管理員管理 EK 清單|高|  
    |1.3.6.1.4.1.311.21.31|簽署憑證|「 EK 憑證驗證: 」當驗證 EK 憑證鏈結|中等|  
    |1.3.6.1.4.1.311.21.32|使用者認證|「 EK 信任上使用: 」針對使用者證明 EK|低|  
  
    Oid 會插入到發行的憑證，如果**包含發佈原則**是選取 （預設設定）。  
  
    ![TPM 金鑰證明](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 可能的用途之一有出現在憑證 OID 是限制存取 VPN 或無線網路到特定裝置。 比方說，您的存取原則可讓連線 （或存取不同的 VLAN） 是否存在於憑證 OID 1.3.6.1.4.1.311.21.30。 這可讓您限制的裝置的 TPM 的 EK 位於 EKPUB 清單的存取權。  
  
### <a name="BKMK_CAConfig"></a>CA 設定  
  
1.  **設定發行 CA 上的 EKRROT 和 EKROOT 憑證存放區**  
  
    如果您選擇**簽署憑證**對於範本設定，請執行下列設定步驟：  
  
    1.  您可以使用 Windows PowerShell，將執行 TPM 金鑰證明的憑證授權單位 (CA) 伺服器上建立兩個新的憑證存放區。  
  
    2.  取得中繼和根 CA 憑證，從您想要允許您的企業環境中的製造商。 必須將這些憑證匯入到先前建立的憑證存放區 （EKRROT 和 EKROOT） 視需要。  
  
    下列 Windows PowerShell 指令碼會執行兩個步驟。 在下列範例中，Fabrikam TPM 製造商提供的根憑證*FabrikamRoot.cer*和 發行的 CA 憑證*Fabrikamca.cer*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **如果使用 EK 證明類型，設定 EKPUB 清單**  
  
    如果您選擇**簽署金鑰**範本設定下, 一步 的設定步驟來建立及發行的 CA，其中包含 0 位元組檔案上設定的資料夾，每個名為允許的 EK 的 sha-2 雜湊。 這個資料夾可做為 「 允許清單 」 的允許取得 TPM 金鑰證明憑證的裝置。 您必須手動加入 EKPUB 需要 attested 的憑證的每個裝置，因為它提供企業可保證已獲授權可取得 TPM 金鑰證明認證的裝置。 設定 CA，此模式中需要兩個步驟：  
  
    1.  **建立 EndorsementKeyListDirectories 登錄項目：** 您可以使用 Certutil 命令列工具來設定下表中所述的信任的 EKpubs 定義所在的資料夾位置。  
  
        |運算|命令語法|  
        |-------------|------------------|  
        |新增資料夾位置|certutil.exe -setreg CA\EndorsementKeyListDirectories +"<folder>"|  
        |移除資料夾位置|certutil.exe -setreg CA\EndorsementKeyListDirectories -"<folder>"|  
  
        在 certutil 命令 EndorsementKeyListDirectories 是一種登錄設定表中所述。  
  
        |值名稱|類型|資料|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< 本機或 UNC 路徑 EKPUB 允許清單 ><br /><br />範例：<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories*將會包含清單的 UNC 或本機檔案系統路徑，分別指向 CA 具有讀取權限的資料夾。 每個資料夾可能包含零個或多個允許清單項目，其中每個項目是檔案的名稱是 sha-2 雜湊的受信任的 EKpub，不含檔案副檔名。 
        建立或編輯此登錄機碼設定需要重新啟動 CA，如同現有的 CA 登錄組態設定。 不過，編輯的組態設定會立即生效，而且不需要重新啟動 CA。  
  
        > [!IMPORTANT]  
        > 保護免於竄改清單中的資料夾和未經授權的存取，藉由設定權限，只有獲授權的系統管理員具有讀取和寫入權限。 CA 的電腦帳戶需要唯讀存取。  
  
    2.  **填入 EKPUB 清單：** 使用下列 Windows PowerShell cmdlet，每個裝置上使用 Windows PowerShell 中取得 TPM 的 EK 公開金鑰雜湊，然後傳送給 CA 的這個公開金鑰雜湊，並將它存放 EKPubList 資料夾。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>疑難排解  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>金鑰證明欄位都無法使用的憑證範本  
無法使用，如果範本設定不符合需求的證明金鑰證明欄位。 常見的原因如下：  
  
1.  未正確設定的相容性設定。 請確定設定方式，如下所示：  
  
    1.  **憑證授權單位**：**Windows Server 2012 R2**  
  
    2.  **憑證收件者**:**Windows 8.1/Windows Server 2012 R2**  
  
2.  密碼編譯設定未正確設定。 請確定設定方式，如下所示：  
  
    1.  **提供者類別目錄**:**金鑰儲存提供者**  
  
    2.  **演算法名稱**:**RSA**  
  
    3.  **提供者**:**Microsoft 平台密碼編譯提供者**  
  
3.  要求處理設定未正確設定。 請確定設定方式，如下所示：  
  
    1.  **允許私密金鑰匯出**不得選取選項。  
  
    2.  **封存主體的加密私密金鑰**不得選取選項。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>TPM 證明的裝置的驗證  
使用 Windows PowerShell cmdlet**確認 CAEndorsementKeyInfo**，若要確認特定的 TPM 裝置是否受信任的 Ca 可以證明。 有兩個選項： 一個用於驗證 EKCert，和另一個則用於驗證 EKPub。 此 cmdlet 會執行在本機在 CA 上，或在遠端 Ca 上使用 Windows PowerShell 遠端執行功能。  
  
1.  驗證信任 EKPub 上的，執行下列兩個步驟：  
  
    1.  **從用戶端電腦擷取 EKPub:** 您可以從用戶端電腦透過擷取 EKPub **Get TpmEndorsementKeyInfo**。 從提升權限的命令提示字元中，執行下列命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **確認在 CA 電腦上 EKCert 的信任：** 將擷取的字串 （EKPub sha-2 雜湊） 複製到伺服器 （例如，透過電子郵件），並將它傳遞給 Confirm CAEndorsementKeyInfo cmdlet。 請注意，此參數必須是 64 個字元。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  確認上 EKCert 的信任，執行下列兩個步驟：  
  
    1.  **從用戶端電腦擷取 EKCert:** 您可以從用戶端電腦透過擷取 EKCert **Get TpmEndorsementKeyInfo**。 從提升權限的命令提示字元中，執行下列命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **確認在 CA 電腦上 EKCert 的信任：** 將擷取的 EKCert (EkCert.cer) 複製到 CA （例如，透過電子郵件或 xcopy）。 例如，如果您複製憑證檔案 」 c:\diagnose"資料夾，在 CA 伺服器上的，執行下列命令來完成驗證：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>另請參閱  
[受信任的平台模組技術概觀](https://technet.microsoft.com/library/jj131725.aspx)  
[外部資源：信賴平台模組](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
