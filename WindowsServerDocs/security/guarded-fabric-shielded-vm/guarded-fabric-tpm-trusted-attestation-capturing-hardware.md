---
title: 擷取所需的 HGS 的 TPM 模式資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: 24d81e3d2c31b3493563f3f3e2ab3f92afff2c06
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284132"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>授權使用 TPM 型證明的受防護的主機

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

TPM 模式使用 TPM 的識別項 (也稱為平台識別項或簽署金鑰\[EKpub\]) 以開始決定特定主控件是否已獲授權為 「 受防護 」。 證明此模式會使用安全開機] 與 [程式碼完整性度量，以確保指定的 HYPER-V 主機處於狀況良好的狀態，以及執行只有受信任的程式碼。 為了讓以了解什麼是和狀況不良的證明，您必須擷取下列成品：

1.  TPM 識別項 (EKpub)

    -  這項資訊是唯一每一部 HYPER-V 主機

2.  TPM 基準 （開機測量）

    -  這是硬體的適用於在相同類別執行的所有 HYPER-V 主機

3.  程式碼完整性原則 （允許清單允許的二進位檔）

    -  這是適用於共用常見的硬體和軟體的所有 HYPER-V 主機

我們建議您擷取的基準和 「 參考主機 」 從 CI 原則也就是代表您資料中心內的 HYPER-V 硬體組態的每個唯一的類別。 從 Windows Server 1709 版開始，則會包含在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies 中範例 CI 原則。 

## <a name="versioned-attestation-policies"></a>建立版本的證明原則

Windows Server 2019 導入了新的方法，可證明，稱為*v2 證明*，其中 TPM 必須要有憑證才能 EKPub 加入 HGS。 使用 Windows Server 2016 中的 v1 證明方法允許您覆寫這項安全檢查，藉由指定-Force 旗標，當您執行新增 HgsAttestationTpmHost 或 TPM 證明的其他 cmdlet，以擷取成品。 從 Windows Server 2019，v2 證明會依預設，您必須指定-Policyversion="1.0 v1 旗標，當您執行新增 HgsAttestationTpmHost，如果您要註冊沒有憑證的 TPM。 -Force 旗標不適用於 v2 證明。 

如果要將所有主機可以只，證明客戶都資料的成品 (EKPub + TPM 基準 + CI 原則) 使用相同版本的證明。 V2 證明會先嘗試，而且如果失敗，會使用 v1 證明。 這表示如果您需要註冊的 TPM 識別碼，方法是使用 v1 證明，您需要同時指定-Policyversion="1.0 v1 旗標，若要使用 v1 證明，當您擷取 TPM 基準，並建立 CI 原則。 如果使用 v2 證明已建立的 TPM 基準和 CI 原則，然後稍後您要新增為受防護的主機沒有 TPM 的憑證，您需要重新建立每個成品-Policyversion="1.0 v1 旗標。 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>每個主控件中擷取的 TPM 識別碼 （平台識別項或 EKpub）

1.  在網狀架構的網域中，請確定每部主機上的 TPM 可供使用-也就是初始化 TPM，並取得擁有權。 您可以檢查 TPM 的狀態，藉由開啟 TPM 管理主控台 (tpm.msc) 或執行**取得 Tpm**較高權限的 Windows PowerShell 視窗中。 如果不在您的 TPM**準備**狀態時，您必須將它初始化，並設定其擁有權。 這可以在 TPM 管理主控台或藉由執行完成**Initialize Tpm**。

2.  在每個受防護主機上，執行下列命令中已提升權限的 Windows PowerShell 主控台，若要取得其 EKpub。 針對`<HostName>`，替換成唯一的主機名稱，以適合用來識別此主機的內容-這可以是其主機名稱或使用 fabric 清查服務 （如果有的話） 的名稱。 為了方便起見，命名輸出檔案使用的主機名稱。

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  重複上述步驟，每個主控件將成為受防護的主機，務必為每個 XML 檔案指定唯一名稱。

4.  HGS 系統管理員提供的 XML 檔案。

5.  在 HGS 網域中，開啟 HGS 伺服器上提升權限的 Windows PowerShell 主控台並執行下列命令。 每個 XML 檔案，重複執行命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果加入不受信任的簽署金鑰憑證 (EKCert) 有關的 TPM 識別碼時，您會遇到錯誤，請確認[受信任的 TPM 根憑證已新增](guarded-fabric-install-trusted-tpm-root-certificates.md)HGS 節點。
    > 此外，有些 TPM 廠商請勿使用 EKCerts。
    > 您可以檢查是否 EKCert 遺漏 [記事本] 之類的編輯器中開啟 XML 檔案，並檢查錯誤訊息，指出沒有 EKCert 找不到。
    > 如果此情況下，而且您信任您的電腦中的 TPM 是真確，，您可以使用`-Force`HGS 加入主機識別碼的參數。 在 Windows Server 2019，您需要同時使用`-PolicyVersion v1`參數使用時`-Force`。 這會建立原則與 Windows Server 2016 的行為一致，而需要您使用`-PolicyVersion v1`時註冊的 CI 原則和 TPM 基準線。

## <a name="create-and-apply-a-code-integrity-policy"></a>建立和套用程式碼完整性原則

程式碼完整性原則可協助確保您所信任的主機上執行執行檔都可以執行。 惡意程式碼和其他外部受信任的可執行檔的可執行檔會自動執行。

每一部受防護的主機必須具有程式碼完整性原則套用，以在 TPM 模式中執行受防護的 Vm。 您指定您信任新增至 HGS 的確切程式碼完整性原則。 程式碼完整性原則可以設定強制執行原則，封鎖不符合原則，或單純稽核的任何軟體 （也就是記錄檔事件時執行的原則中未定義的軟體）。 

從 Windows Server 1709 版開始，則會包含在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies 的 Windows 中範例程式碼完整性原則。 兩個原則建議適用於 Windows Server:

- **AllowMicrosoft**:允許由 Microsoft 所簽署的所有檔案。 這項原則建議用於伺服器應用程式，例如 SQL 或交換，或如果伺服器由 Microsoft 所發行的代理程式監視。
- **DefaultWindows_Enforced**:可讓檔案隨附於 Windows，且不允許其他 Microsoft，例如 Office 所發行的應用程式。 此原則被建議用於執行只有內建的伺服器角色和功能，例如 HYPER-V 的伺服器。 

建議您先建立 CI 原則稽核 （記錄） 模式，以查看是否遺漏任何項目，然後強制執行主應用程式的生產工作負載的原則。 

如果您使用[新增 CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet，產生您自己的程式碼完整性原則，您必須決定要使用的規則層級。 我們建議主要程度**發行者**與後援**雜湊**，可讓最以數位方式簽署軟體更新，而不變更的 CI 原則。
寫入相同的 「 發行者 」 的新軟體而不需要變更的 CI 原則也安裝在伺服器上。
未經過數位簽署的可執行檔會被雜湊--這些檔案的更新會要求您建立新的 CI 原則。
如需有關可用的 CI 原則規則層級的詳細資訊，請參閱 <<c0> [ 部署程式碼完整性原則： 原則規則和檔案規則](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules)和指令程式說明。

1.  在參考主機上，產生新的程式碼完整性原則。 下列命令會建立原則**發行者**層級使用後援**雜湊**。 然後將 XML 檔案轉換為 Windows 及 HGS 必須套用，並分別測量 CI 原則中，二進位檔案格式。

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >上述命令會建立僅稽核模式中的 CI 原則。 它不會封鎖未經授權的二進位檔案從主機上執行。 在生產環境中，您應該只會使用強制執行的原則。

2.  請在您可以輕鬆地找到您的程式碼完整性原則檔案 （XML 檔案）。 您必須編輯此檔案之後，強制執行的 CI 原則或合併在未來的更新對系統所做的變更。

3.  適用於參考主機的 CI 原則：

    1.  執行下列命令來設定要使用您的 CI 原則的電腦。 您也可以部署的 CI 原則[群組原則](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy)或是[System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm)。

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  重新啟動主機，以套用原則。

3.  執行一般工作負載，以測試程式碼完整性原則。 這可能包括執行中 Vm，任何網狀架構管理代理程式、 備份代理程式，或疑難排解在電腦上的工具。 請檢查是否有任何程式碼完整性違規，並更新您的 CI 原則如有必要。

4.  變更為強制模式的 CI 原則，藉由執行下列命令，針對您已更新的 CI 原則 XML 檔。

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  CI 原則套用至所有主機 （使用相同的硬體和軟體組態） 使用下列命令：

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >將 CI 原則套用至主機以及在這些電腦上的任何軟體更新時，則要小心。 任何不符合規範的 CI 原則的核心模式驅動程式可能會防止啟動機器。 

6.  提供二進位檔案 (在此範例中，HW1CodeIntegrity\_enforced.p7b) 給 HGS 系統管理員。

7.  在 HGS 網域中，將程式碼完整性原則複製到一個 HGS 伺服器，並執行下列命令。

    針對`<PolicyName>`，指定描述的主機，它會套用到類型的 CI 原則的名稱。 最佳的作法是品牌/型號機器和在其上執行任何特殊的軟體設定為其命名。<br>針對`<Path>`，指定的路徑和檔案名稱的程式碼完整性原則。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>擷取硬體的每個唯一類別的 TPM 基準

需要為每個唯一的類別，在您的資料中心網狀架構中的硬體 TPM 基準。 再次使用 「 參考主機 」。 

1. 在參考主機上，請確定已安裝 HYPER-V 角色和主機守護者 HYPER-V 支援功能。

    >[!WARNING]
    >主機守護者 HYPER-V 支援功能可讓虛擬化保護可能與某些裝置不相容的程式碼完整性。 我們強烈建議您在實驗室中測試此組態，再啟用這項功能。 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. 若要擷取的基準原則，請在提升權限的 Windows PowerShell 主控台中執行下列命令。
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >您必須使用 **-SkipValidation**啟用旗標，如果參考主機並沒有安全開機，存在 iommu 所致，啟用虛擬式安全性和執行或套用程式碼完整性原則。 這些驗證旨在讓您知道在主機上執行受防護的 VM 的最低需求。 使用-SkipValidation 旗標不會變更指令程式的輸出它只會關閉的錯誤。

3.  HGS 系統管理員提供的 TPM 基準 （TCGlog 檔案）。

4.  在 HGS 網域中，將 TCGlog 檔案複製到一個 HGS 伺服器，並執行下列命令。 一般而言，您將它所代表的硬體類別命名原則時 （例如，"製造商型號版本 」）。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
