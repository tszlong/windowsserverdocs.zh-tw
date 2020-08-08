---
title: 捕捉 HGS 所需的 TPM 模式資訊
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 04/01/2019
ms.openlocfilehash: dedd7a3629b4381fd5f78f70a39f6906cab0573d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995391"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>使用 TPM 型證明授權受防護主機

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

TPM 模式會使用 TPM 識別碼 (也稱為平臺識別碼或簽署金鑰 \[ EKpub \]) ，以開始判斷特定主機是否授權為「受防護」。 這種證明模式使用安全開機和程式碼完整性測量，以確保指定的 Hyper-v 主機處於狀況良好的狀態，而且只執行受信任的程式碼。 為了讓證明能夠瞭解哪些專案狀況不良，您必須捕捉下列成品：

1.  TPM 識別碼 (EKpub) 

    -  這項資訊對每一部 Hyper-v 主機都是唯一的

2.  TPM 基準 (開機測量) 

    -  這適用于在相同硬體類別上執行的所有 Hyper-v 主機

3.  程式碼完整性原則 (允許的二進位檔的白名單) 

    -  這適用于所有共用通用硬體和軟體的 Hyper-v 主機

我們建議您從「參照主機」中捕捉基準和 CI 原則，這代表資料中心內 Hyper-v 硬體設定的每個唯一類別。 從 Windows Server 1709 版開始，範例 CI 原則會包含在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies。

## <a name="versioned-attestation-policies"></a>已建立版本的證明原則

Windows Server 2019 引進了證明的新方法，稱為*v2 證明*，其中必須要有 TPM 憑證，才能將 EKPub 新增至 HGS。 Windows Server 2016 中使用的 v1 證明方法，可讓您在執行 HgsAttestationTpmHost 或其他 TPM 證明 Cmdlet 來捕捉成品時，藉由指定-Force 旗標來覆寫此安全性檢查。 從 Windows Server 2019 開始，預設會使用 v2 證明，如果您需要註冊沒有憑證的 TPM，請在執行 HgsAttestationTpmHost 時指定-PolicyVersion v1 旗標。 -Force 旗標無法與 v2 證明搭配使用。

主機只能證明所有成品 (EKPub + TPM 基準 + CI 原則) 使用相同版本的證明。 首先會嘗試 V2 證明，如果失敗，則會使用 v1 證明。 這表示如果您需要使用 v1 證明來註冊 TPM 識別碼，則當您捕捉 TPM 基準並建立 CI 原則時，也必須指定-PolicyVersion v1 旗標來使用 v1 證明。 如果 TPM 基準和 CI 原則是使用 v2 證明所建立，而稍後您需要新增不含 TPM 憑證的受防護主機，則必須使用-PolicyVersion v1 旗標來重新建立每個成品。

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>針對每部主機 (平臺識別碼或 EKpub) 中，捕獲 TPM 識別碼

1.  在網狀架構網域中，請確定每部主機上的 TPM 已準備好可供使用，也就是已初始化 TPM 並取得擁有權。 您可以在已提升許可權的 Windows PowerShell 視窗中，藉由開啟 tpm 管理主控台 (的) 或執行**取得**tpm，來檢查 tpm 的狀態。 如果您的 TPM 不是處於 [**就緒**] 狀態，您必須將它初始化並設定其擁有權。 這可以在 TPM 管理主控台中或藉由執行**初始化-TPM**來完成。

2.  在每部受防護主機上，在提升許可權的 Windows PowerShell 主控台中執行下列命令，以取得其 EKpub。 針對 `<HostName>` ，請將唯一的主機名稱替換成適合用來識別此主機的名稱，這可以是其主機名稱或網狀架構清查服務所使用的名稱（如果有) 的話） (。 為方便起見，請使用主機名稱來命名輸出檔案。

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  針對將成為受防護主機的每部主機重複上述步驟，並務必為每個 XML 檔案提供唯一的名稱。

4.  將產生的 XML 檔案提供給 HGS 系統管理員。

5.  在 HGS 網域中，于 HGS 伺服器上開啟提升許可權的 Windows PowerShell 主控台，然後執行下列命令。 針對每個 XML 檔案重複此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果您在新增與不受信任的簽署金鑰憑證有關的 TPM 識別碼時遇到錯誤 (EKCert) ，請確定已將[受信任的 TPM 根憑證新增](guarded-fabric-install-trusted-tpm-root-certificates.md)至 HGS 節點。
    > 此外，某些 TPM 廠商不會使用 EKCerts。
    > 您可以在 [記事本] 之類的編輯器中開啟 XML 檔案，並檢查是否有指出找不到 EKCert 的錯誤訊息，以檢查是否遺漏 EKCert。
    > 如果是這種情況，而且您信任電腦中的 TPM 是真實的，您可以使用 `-Force` 參數將主機識別碼新增至 HGS。 在 Windows Server 2019 中，當您使用時，也必須使用 `-PolicyVersion v1` 參數 `-Force` 。 這會建立與 Windows Server 2016 行為一致的原則，而且您也需要在 `-PolicyVersion v1` 註冊 CI 原則和 TPM 基準時使用。

## <a name="create-and-apply-a-code-integrity-policy"></a>建立和套用程式碼完整性原則

程式碼完整性原則有助於確保只允許執行您信任在主機上執行的可執行檔。
受信任的可執行檔以外的惡意程式碼和其他可執行檔無法執行。

每個受防護主機都必須套用程式碼完整性原則，才能以 TPM 模式執行受防護的 Vm。
藉由將您信任的確切程式碼完整性原則新增至 HGS，即可加以指定。
程式碼完整性原則可設定為強制執行原則、封鎖任何不符合原則的軟體，或只是在原則中未定義的軟體) 執行時，只需 audit (記錄事件。

從 Windows Server 1709 版開始，Windows 會在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 中包含範例程式碼完整性原則。 Windows Server 建議使用兩個原則：

- **AllowMicrosoft**：允許 Microsoft 簽署的所有檔案。 這項原則建議用於 SQL 或 Exchange 等伺服器應用程式，或者，如果伺服器是由 Microsoft 所發行的代理程式所監視。
- **DefaultWindows_Enforced**：只允許 Windows 隨附的檔案，而且不允許 Microsoft 發行的其他應用程式，例如 Office。 針對僅執行內建伺服器角色和功能（例如 Hyper-v）的伺服器，建議使用此原則。

建議您先在 audit (記錄) 模式中建立 CI 原則，以查看它是否遺漏任何專案，然後強制執行主機生產工作負載的原則。

如果您使用 CIPolicy Cmdlet 來產生自己的[程式](/powershell/module/configci/new-cipolicy?view=win10-ps)代碼完整性原則，就必須決定要使用的規則層級。
我們建議使用回溯至**Hash**的主要「**發行者**」層級，以允許更新最多數位簽署的軟體，而不需要變更 CI 原則。
相同發行者所撰寫的新軟體也可以安裝在伺服器上，而不需要變更 CI 原則。
未數位簽署的可執行檔將會雜湊--這些檔案的更新將會要求您建立新的 CI 原則。
如需可用 CI 原則規則層級的詳細資訊，請參閱[部署程式碼完整性原則：原則規則和檔案規則](/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules)和 Cmdlet 說明。

1.  在參照主機上，產生新的程式碼完整性原則。 下列命令會在**發行者**層級建立原則，並將其回復為**Hash**。 接著，它會將 XML 檔案轉換成二進位檔案格式，Windows 和 HGS 必須分別套用和測量 CI 原則。

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >上述命令只會在 audit 模式中建立 CI 原則。 它不會封鎖在主機上執行未授權的二進位檔。 您應該只在生產環境中使用強制執行的原則。

2.  將您的程式碼完整性原則檔案保存 (XML 檔案) 您可以輕鬆地找到它。 您稍後必須編輯此檔案，以強制執行 CI 原則，或合併變更中未來對系統所做的更新。

3.  將 CI 原則套用至您的參考主機：

    1.  執行下列命令，將電腦設定為使用您的 CI 原則。 您也可以使用[群組原則](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy)或[SYSTEM CENTER VIRTUAL MACHINE MANAGER](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm)來部署 CI 原則。

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  重新開機主機以套用原則。

3.  執行一般工作負載來測試程式碼完整性原則。 這可能包括執行中的 Vm、任何網狀架構管理代理程式、備份代理程式，或電腦上的疑難排解工具。 檢查是否有任何程式碼完整性違規，並視需要更新您的 CI 原則。

4.  針對已更新的 CI 原則 XML 檔案執行下列命令，以將 CI 原則變更為 [強制] 模式。

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  使用下列命令，將 CI 原則套用至所有使用相同硬體和軟體設定)  (的主機：

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }

    Restart-Computer
    ```

    >[!NOTE]
    >將 CI 原則套用至主機，以及更新這些電腦上的任何軟體時，請務必小心。 任何不符合 CI 原則的核心模式驅動程式都可能會使電腦無法啟動。

6.  提供此範例中 (的二進位檔案， \_ 並強制執行 HW1CodeIntegrity。) 給 HGS 系統管理員。

7.  在 HGS 網域中，將程式碼完整性原則複製到 HGS 伺服器，然後執行下列命令。

    針對 `<PolicyName>` ，指定 CI 原則的名稱，以描述其適用的主機類型。 最佳做法是在您的機器的製作/型號和其上執行的任何特殊軟體設定之後，將它命名為。<br>針對 `<Path>` ，指定程式碼完整性原則的路徑和檔案名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>為每個唯一的硬體類別捕獲 TPM 基準

您的資料中心網狀架構中每個唯一類別的硬體都需要 TPM 基準。 再次使用「參照主機」。

1. 在參照主機上，請確定已安裝 Hyper-v 角色和主機守護者 Hyper-v 支援功能。

    >[!WARNING]
    >主機守護者 Hyper-v 支援功能可啟用程式碼完整性的虛擬化保護，這可能會與某些裝置不相容。 我們強烈建議您先在實驗室中測試這項設定，再啟用此功能。 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. 若要捕獲基準原則，請在提升許可權的 Windows PowerShell 主控台中執行下列命令。

    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >如果參照主機未啟用安全開機、具有 IOMMU、已啟用並執行虛擬化安全性，或已套用程式碼完整性原則，則您將需要使用 **-SkipValidation**旗標。 這些驗證的設計，是為了讓您知道在主機上執行受防護 VM 的最低需求。 使用-SkipValidation 旗標並不會變更 Cmdlet 的輸出;它只會讓閉嘴錯誤。

3.  提供 TPM 基準 (TCGlog 檔案) 給 HGS 系統管理員。

4.  在 HGS 網域中，將 TCGlog 檔案複製到 HGS 伺服器，然後執行下列命令。 一般來說，您會將原則命名為它所代表的硬體類別， (例如「製造商型號修訂」 ) 。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)