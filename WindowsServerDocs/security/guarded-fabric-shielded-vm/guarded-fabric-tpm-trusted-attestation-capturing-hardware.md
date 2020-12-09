---
title: 捕獲 HGS 所需的 TPM 模式資訊
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 04/01/2019
ms.openlocfilehash: 8ce4528ec7e8143c6f9af977079eed1cf8cc3940
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865657"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>使用以 TPM 為基礎的證明授權受防護主機

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

TPM 模式使用 TPM 識別碼 (也稱為平臺識別碼或簽署金鑰 \[ EKpub \]) 開始判斷特定主機是否授權為「受防護」。 這種證明模式使用安全開機和程式碼完整性測量，以確保指定的 Hyper-v 主機處於狀況良好的狀態，而且只執行信任的程式碼。 為了讓證明瞭解什麼是且狀況不良，您必須先捕捉下列成品：

1.  TPM 識別碼 (EKpub) 

    -  這項資訊對每一部 Hyper-v 主機都是唯一的

2.  TPM 基準 (開機測量) 

    -  這適用于在相同硬體類別上執行的所有 Hyper-v 主機

3.  程式碼完整性原則 (允許的二進位檔的允許清單) 

    -  這適用于所有共用一般硬體和軟體的 Hyper-v 主機

我們建議您從「參考主機」中，為您的資料中心內的每個唯一類別的 Hyper-v 硬體設定，捕捉基準和 CI 原則。 從 Windows Server 1709 版開始，範例 CI 原則包含在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies。

## <a name="versioned-attestation-policies"></a>已建立版本的證明原則

Windows Server 2019 引進了新的證明方法，稱為 *v2 證明*，其中必須要有 TPM 憑證才能將 EKPub 新增至 HGS。 Windows Server 2016 中使用的 v1 證明方法，可讓您在執行 Add-HgsAttestationTpmHost 或其他 TPM 證明 Cmdlet 來捕捉成品時，藉由指定-Force 旗標來覆寫這項安全檢查。 從 Windows Server 2019 開始，預設會使用 v2 證明，您必須在執行 Add-HgsAttestationTpmHost 時指定-PolicyVersion v1 旗標（如果您需要註冊 TPM，而不使用憑證）。 -Force 旗標不適用於 v2 證明。

主機只能證明所有成品 (EKPub + TPM 基準 + CI 原則) 使用相同版本的證明。 首先會嘗試 V2 證明，如果失敗，則會使用 v1 證明。 這表示，如果您需要使用 v1 證明註冊 TPM 識別碼，則在您捕獲 TPM 基準並建立 CI 原則時，也必須指定-PolicyVersion v1 旗標以使用 v1 證明。 如果 TPM 基準和 CI 原則是使用 v2 證明建立的，稍後您需要新增不含 TPM 憑證的受防護主機，您必須使用-PolicyVersion v1 旗標重新建立每個成品。

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>為每部主機捕獲 (平臺識別碼或 EKpub) 的 TPM 識別碼

1.  在網狀架構網域中，確定每部主機上的 TPM 都已備妥可供使用，也就是已初始化 TPM 並取得擁有權。 您可以藉由開啟 TPM 管理主控台 (的) 或在提高許可權的 Windows PowerShell 視窗中執行 **取得 tpm** ，來檢查 tpm 的狀態。 如果您的 TPM 未處於 [ **就緒** ] 狀態，則必須將它初始化並設定其擁有權。 這可以在 TPM 管理主控台中或藉由執行 **Initialize-TPM** 來完成。

2.  在每個受防護主機上，在提高許可權的 Windows PowerShell 主控台中執行下列命令，以取得其 EKpub。 針對 `<HostName>` ，請將唯一的主機名稱替換成適合用來識別此主機的名稱，這可以是其主機名稱或網狀架構清查服務所使用的名稱（如果有的話)  (）。 為了方便起見，請使用主機名稱來命名輸出檔。

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  針對將成為受保護主機的每個主機重複上述步驟，並確定為每個 XML 檔案指定一個唯一的名稱。

4.  將產生的 XML 檔案提供給 HGS 系統管理員。

5.  在 HGS 網域中，開啟 HGS 伺服器上提高許可權的 Windows PowerShell 主控台，然後執行下列命令。 針對每個 XML 檔案重複此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果您在新增有關未受信任簽署金鑰憑證的 TPM 識別碼時發生錯誤 (EKCert) ，請確定 [已將受信任的 TPM 根憑證新增](guarded-fabric-install-trusted-tpm-root-certificates.md) 至 HGS 節點。
    > 此外，有些 TPM 廠商不會使用 EKCerts。
    > 您可以藉由在編輯器（例如 [記事本]）中開啟 XML 檔案，並檢查指出找不到任何 EKCert 的錯誤訊息，來檢查是否遺漏 EKCert。
    > 如果是這種情況，而且您信任電腦中的 TPM 是真實的，您可以使用 `-Force` 參數將主機識別碼新增至 HGS。 在 Windows Server 2019 中，您也必須在 `-PolicyVersion v1` 使用時使用參數 `-Force` 。 這會建立與 Windows Server 2016 行為一致的原則，而且您也需要在 `-PolicyVersion v1` 註冊 CI 原則和 TPM 基準時使用。

## <a name="create-and-apply-a-code-integrity-policy"></a>建立並套用程式碼完整性原則

程式碼完整性原則可協助確保只允許執行您信任在主機上執行的可執行檔。
受信任的可執行檔以外的惡意程式碼和其他可執行檔無法執行。

每個受防護主機必須套用程式碼完整性原則，才能在 TPM 模式中執行受防護的 Vm。
您可以藉由將所信任的確切程式碼完整性原則新增至 HGS 來指定它們。
您可以設定程式碼完整性原則來強制執行原則、封鎖不符合原則的任何軟體，或只在) 執行原則中未定義的軟體時， (記錄事件。

從 Windows Server 1709 版開始，範例程式碼完整性原則會隨附于 Windows 的 C:\Windows\schemas\CodeIntegrity\ExamplePolicies。 建議針對 Windows Server 使用兩個原則：

- **AllowMicrosoft**：允許由 Microsoft 簽署的所有檔案。 這項原則建議用於伺服器應用程式（例如 SQL 或 Exchange），或如果伺服器是由 Microsoft 發行的代理程式監視。
- **DefaultWindows_Enforced**：只允許 Windows 隨附的檔案，而且不允許由 Microsoft （例如 Office）發行的其他應用程式。 建議僅執行內建伺服器角色和功能（例如 Hyper-v）的伺服器使用此原則。

建議您先在 audit (記錄) 模式中建立 CI 原則，以查看它是否遺失任何檔案，然後強制執行主機生產工作負載的原則。

如果您使用 [CIPolicy 指令程式](/powershell/module/configci/new-cipolicy) 來產生您自己的程式碼完整性原則，您將需要決定要使用的規則層級。
我們建議主要的「 **發行者** 」層級使用「回復為 **雜湊**」，這可讓您更新大部分數位簽署的軟體，而不需要變更 CI 原則。
相同發行者所撰寫的新軟體也可以安裝在伺服器上，而不需要變更 CI 原則。
未數位簽署的可執行檔將會雜湊處理--這些檔案的更新將會要求您建立新的 CI 原則。
如需可用 CI 原則規則層級的詳細資訊，請參閱 [部署程式碼完整性原則：原則規則和檔案規則](/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) 和 Cmdlet 說明。

1.  在參考主機上，產生新的程式碼完整性原則。 下列命令會在「 **發行者** 」層級建立具有回 **希** 的原則。 然後，它會將 XML 檔案轉換成二進位檔案格式 Windows 和 HGS 需要分別套用和測量 CI 原則。

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >上述命令只會在 audit 模式中建立 CI 原則。 它不會封鎖在主機上執行未經授權的二進位檔。 您應該只在生產環境中使用強制執行的原則。

2.  將您的程式碼完整性原則檔 (XML 檔案) ，以便輕鬆找到它。 您稍後將需要編輯此檔案，以強制執行 CI 原則，或合併對系統的未來更新所做的變更。

3.  將 CI 原則套用至您的參考主機：

    1.  執行下列命令，將電腦設定為使用您的 CI 原則。 您也可以使用 [群組原則](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) 或 [SYSTEM CENTER VIRTUAL MACHINE MANAGER](/system-center/vmm/guarded-deploy-host#manage-and-deploy-code-integrity-policies-with-vmm)來部署 CI 原則。

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  重新開機主機以套用原則。

3.  藉由執行一般工作負載來測試程式碼完整性原則。 這可能包括執行中的 Vm、任何網狀架構管理代理程式、備份代理程式，或電腦上的疑難排解工具。 檢查是否有任何程式碼完整性違規，並視需要更新您的 CI 原則。

4.  針對更新的 CI 原則 XML 檔案執行下列命令，以將 CI 原則變更為強制模式。

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  使用下列命令，將 CI 原則套用至所有主機 (使用相同的硬體和軟體配置) ：

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }

    Restart-Computer
    ```

    >[!NOTE]
    >將 CI 原則套用至主機以及更新這些電腦上的任何軟體時，請務必小心。 任何不符合 CI 原則規範的核心模式驅動程式，可能會導致電腦無法啟動。

6.  提供二進位檔案 (在此範例中，HW1CodeIntegrity \_ 強制執行。 p7b) 給 HGS 系統管理員。

7.  在 HGS 網域中，將程式碼完整性原則複製到 HGS 伺服器，然後執行下列命令。

    針對 `<PolicyName>` ，指定 CI 原則的名稱，以描述其所套用的主機類型。 最佳做法是將它命名為您電腦的製作/型號和在其上執行的任何特殊軟體設定。<br>`<Path>`在中，指定程式碼完整性原則的路徑和檔案名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>為每個獨特的硬體類別捕獲 TPM 基準

您的資料中心網狀架構中的每個唯一硬體類別都需要 TPM 基準。 再次使用「參考主控制項」。

1. 在參照主機上，確認已安裝 Hyper-v 角色和主機守護者 Hyper-v 支援功能。

    >[!WARNING]
    >主機守護者 Hyper-v 支援功能可針對可能與某些裝置不相容的程式碼完整性，啟用虛擬化型保護。 強烈建議您先在實驗室中測試此設定，再啟用此功能。 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. 若要捕獲基準原則，請在提高許可權的 Windows PowerShell 主控台中執行下列命令。

    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >如果參照主機未啟用安全開機，則您必須使用 **-SkipValidation** 旗標，其中有一個 IOMMU、啟用虛擬化的安全性，以及已套用的程式碼完整性原則。 這些驗證的設計目的是要讓您瞭解在主機上執行受防護 VM 的最低需求。 使用-SkipValidation 旗標並不會變更 Cmdlet 的輸出;它只會讓閉嘴錯誤。

3.  提供 TPM 基準 (TCGlog 檔案) 給 HGS 系統管理員。

4.  在 HGS 網域中，將 TCGlog 檔案複製到 HGS 伺服器，然後執行下列命令。 一般來說，您會將原則命名在它所代表的硬體類別之後 (例如「製造商型號修訂」 ) 。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
