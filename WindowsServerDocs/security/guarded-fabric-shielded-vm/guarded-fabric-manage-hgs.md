---
title: 管理主機守護者服務
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: dab27e71e42970507f321271edda90f6d161c691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447388"
---
# <a name="managing-the-host-guardian-service"></a>管理主機守護者服務

> 適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

主機守護者服務 (HGS) 是受防護網狀架構解決方案的核心。
它負責確保網狀架構中的 HYPER-V 主機，已知的主機服務提供者或企業，並執行受信任的軟體和管理用來啟動受防護的 Vm 的金鑰。
當租用戶決定信任您裝載其受防護的 Vm 時，它們會放置在您的組態和管理的主機守護者服務的信賴。
因此，務必遵循最佳做法，管理主機守護者服務，以確保安全性、 可用性和可靠性受防護網狀架構時。
下列各節中的指引可解決最常見的操作問題，面臨的 HGS 系統管理員。

## <a name="limiting-admin-access-to-hgs"></a>限制系統管理員存取權 HGS
由於 HGS 的安全性機密本質，請務必確保其系統管理員是受高度信任的組織的成員和，在理想情況下，個別從網狀架構資源的系統管理員。
此外，建議您只從安全的工作站使用安全通訊的通訊協定，例如透過 HTTPS 的 WinRM 管理 HGS。

### <a name="separation-of-duties"></a>責任劃分
當 HGS 設定，您可以選擇只針對 HGS，或加入現有且受信任網域中的 HGS 建立隔離的 Active Directory 樹系。
這項決策，以及您組織中指派系統管理員角色可決定信任界限的 HGS。
人有權存取 HGS，身為系統管理員直接或間接身為系統管理員的其他項目 (例如 Active Directory)，可能會影響 HGS，是否有您的受防護網狀架構的控制。
HGS 系統管理員選擇哪些 HYPER-V 主機已獲授權執行受防護的 Vm 和管理受防護的 vm 啟動所需的憑證。
攻擊者或惡意的系統管理員有權存取 HGS 可以使用此能力，來授權執行受防護的 Vm，藉由移除金鑰內容，以及其他起始阻絕服務攻擊遭入侵的主機。

若要避免此風險，就*強*建議您限制您的 HGS （包括 HGS 已加入網域） 的系統管理員之間的重疊和 HYPER-V 環境。
藉由確定沒有一個系統管理員可以存取這兩個系統，攻擊者需要 2 個不同的帳戶，才能完成他的使命，若要變更的 HGS 原則 2 個人會危害。
這也表示，兩個 Active Directory 環境的網域和企業系統管理員不應該是同一人，也不應該 HGS 使用相同的 Active Directory 樹系做為 HYPER-V 主機。
可以授與本身更多資源的存取權的任何人帶來安全性風險。

### <a name="using-just-enough-administration"></a>使用恰到好處的系統管理
HGS 隨附[Just Enough Administration](https://aka.ms/JEAdocs) (JEA) 可協助您更安全地管理它內建的角色。
JEA 有助於讓您委派給非系統管理員使用者，這表示管理 HGS 原則的人不實際需要的整部電腦或網域系統管理員的管理工作。
JEA 的運作方式是限制哪些使用者可以在 PowerShell 工作階段中執行的命令，並使用暫時的本機帳戶，在幕後 （唯一在每個使用者工作階段中使用） 執行通常需要提高權限的命令。

HGS 會隨附預先設定的 2 JEA 角色：
- **HGS 系統管理員**可讓使用者能夠管理所有的 HGS 原則，包括授權新的主控件執行受防護的 Vm。
- **HGS 檢閱者**只允許使用者稽核現有原則的權限。 HGS 設定，它們無法進行任何變更。

若要使用 JEA，您需要建立新的標準使用者，並將其 HGS 系統管理員 」 或 「 HGS 檢閱者群組的成員。
如果您使用`Install-HgsServer`設定 HGS 的新樹系，將會命名為這些群組 」*servicename*系統管理員 」 和 「*servicename*檢閱者 」，分別其中*servicename*是 HGS 叢集網路名稱。
如果 HGS 加入現有網域時，您應該參閱您在中指定的群組名稱`Initialize-HgsServer`。

**建立標準使用者的 HGS 系統管理員和檢閱者角色**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**檢閱者角色的稽核原則**

具備網路連線到 HGS，遠端電腦上執行下列命令在 PowerShell 中輸入檢閱者認證的 JEA 工作階段。
請務必注意，檢閱者帳戶是只以標準使用者，因為它無法使用一般的 Windows PowerShell 遠端，HGS 等等的遠端桌面存取。

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

接著，您可以選取的工作階段中允許的命令`Get-Command`並執行任何允許的命令，來稽核設定。
在下列範例中，我們會檢查 HGS 上啟用的原則。

```powershell
Get-Command

Get-HgsAttestationPolicy
```

輸入命令`Exit-PSSession`或其別名`exit`，當您完成使用 JEA 工作階段。 

**HGS 使用系統管理員角色中加入新的原則**

若要實際變更原則時，您需要連接到 JEA 端點，以 'hgsAdministrators' 群組成員的身分識別。
在下列範例中，我們示範如何將新的程式碼完整性原則複製到 HGS，並使用 JEA 登錄它。
語法可能會不同於會用來的。
這是為了容納在 JEA 限制的一些像不包含完整的檔案系統存取。

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>監視 HGS
### <a name="event-sources-and-forwarding"></a>事件來源和轉送
HGS 事件會顯示在 Windows 事件記錄檔在 2 個來源：
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

您可以開啟 [事件檢視器] 並瀏覽至 Microsoft Windows-HostGuardianService 證明與 Microsoft Windows-HostGuardianService KeyProtection 檢視這些事件。

在大型環境中，它通常最好是將事件轉送到中央的 Windows 事件收集器，以方便分析的事件。
如需詳細資訊，請參閱[Windows 事件轉送的文件](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx)。

### <a name="using-system-center-operations-manager"></a>使用 System Center Operations Manager
您也可以使用 System Center 2016-Operations Manager 監視 HGS 和受防護的主機。
受防護網狀架構管理組件具有事件的監視，以檢查有可能會導致資料中心停機，包括主機未通過證明與 HGS 伺服器報告錯誤的常見錯誤設定。

若要開始，[安裝和設定 SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager)並[下載的受防護網狀架構管理組件](https://www.microsoft.com/download/details.aspx?id=52764)。
包含的管理組件指南說明如何設定管理組件，並了解其監視的範圍。

## <a name="backing-up-and-restoring-hgs"></a>備份和還原 HGS
### <a name="disaster-recovery-planning"></a>規劃嚴重損壞修復
時草擬您的災害復原計劃，務必考慮您的受防護網狀架構中的 「 主機守護者服務的獨特需求。
如果您遺失部分或全部的 HGS 節點，可能會遇到會防止使用者啟動其受防護的 Vm 的立即可用性問題。
在此案例中您會遺失您整個 HGS 叢集，您必須擁有的 HGS 設定手上，若要還原您的 HGS 叢集，並恢復正常作業的完整備份。
本章節涵蓋準備這類案例所需的步驟。

首先，務必了解什麼是 HGS 重要備份。
HGS 會保留幾項資訊可協助它判斷哪些主機已獲授權執行受防護的 Vm。
這包括：
1. 包含群組的 active Directory 安全性識別元受信任的主機 （當使用 Active Directory 證明）;
2. 您的環境中; 每個主機的的唯一 TPM 識別碼
3. TPM 的主機; 每個唯一的組態原則和
4. 判斷哪些軟體可在您的主機上執行的程式碼完整性原則。

這些證明成品需要搭配若要取得，主控網狀架構系統管理員可能就不容易取得這項資訊在災害發生後，一次。

此外，HGS 需要 2 個以上的憑證，用來加密及簽署啟動受防護的 VM （金鑰保護裝置） 所需的資訊的存取權。
這些憑證眾所週知 （由受防護的 Vm 的擁有者來授權您的網狀架構執行其 Vm），而且必須以順暢地復原體驗在災害後還原。
應該不還原 HGS 使用相同的憑證在災害發生後，每個 VM 會需要更新授權您新的金鑰來解密他們的資訊。
基於安全性理由，只有 VM 擁有者才可以更新 VM 組態，以授權這些新的金鑰，在嚴重損壞會導致每個 VM 擁有者無需採取動作以取得其 Vm 一次執行之後，還原您的金鑰的意義失敗。

#### <a name="preparing-for-the-worst"></a>做好最壞的情況
若要準備 HGS 完全遺失，有 2 個您必須採取的步驟：
1. 備份 HGS 證明原則
2. HGS 金鑰備份

中提供有關如何執行兩個步驟的指引[備份 HGS](#backing-up-hgs)一節。

此外建議，但並非必要，您備份的授權，可管理其 Active Directory 網域中的 HGS 或 Active Directory 本身的使用者清單。

應該定期製作備份，才能確保的資訊是最新狀態並安全地以避免遭到竄改或竊取儲存。

很**不建議使用**備份，或嘗試還原 HGS 節點的整個系統映像。
萬一您遺失整個叢集，最佳做法是設定全新的 HGS 節點，並還原只 HGS 狀態，不是整個伺服器作業系統。

#### <a name="recovering-from-the-loss-of-one-node"></a>從一個節點遺失中復原
如果您遺失一或多個節點 （但並非每個節點） HGS 叢集中，您可以直接[將節點新增至您的叢集](guarded-fabric-configure-additional-hgs-nodes.md)deployment guide 》 中的指導方針。
證明原則會自動同步，因為將任何憑證，這提供給 HGS 作為 PFX 檔案及隨附的密碼。
加入 HGS 使用指紋的憑證 (非可匯出及硬體支援的憑證，通常)，您必須確定每個新的節點具有每個憑證的私密金鑰存取權。

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>從整個叢集遺失中復原
如果您整個 HGS 叢集停止運作，您就無法將其恢復上線，您必須從備份還原 HGS。
從備份還原 HGS 牽涉到的第一設定每個新的 HGS 叢集[deployment guide 》 中的指導方針](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。
它是強烈建議您，但並非必要，可協助從主機的名稱解析設定復原 HGS 環境時使用相同的叢集名稱。
使用相同的名稱，就不需要重新設定新的證明與金鑰保護 Url 的主機。
如果您還原備份 HGS 的 Active Directory 網域的物件，則建議您移除之前初始化 HGS 伺服器代表 HGS 叢集、 電腦、 服務帳戶和 JEA 群組的物件。

一旦您已設定您的第一個 HGS 節點 （例如它已經安裝並初始化），您將可以依照下的程序[從備份還原的 HGS](#restoring-hgs-from-a-backup)證明原則和金鑰保護的公用部分還原憑證。
您必須還原您的憑證，以手動方式是根據您的憑證提供者的指導方針的私用金鑰 （例如在 Windows 中，將憑證匯入或設定 hsm 型憑證的存取權）。
第一個節點設定好之後，您可以繼續[安裝在叢集中的其他節點](guarded-fabric-configure-additional-hgs-nodes.md)直到到達容量和您想要的恢復功能。

### <a name="backing-up-hgs"></a>備份 HGS
HGS 系統管理員應該負責備份定期執行的 HGS。
完整備份會包含敏感性必須受到適當保護的金鑰材料。
不受信任的實體應該取得這些金鑰的存取權，他們就可以使用內容，以設定惡意的 HGS 環境，以影響受防護的 Vm。

**備份證明原則**要備份的 HGS 證明原則，執行下列命令的任何工作 HGS 伺服器節點上。
系統會提示您提供密碼。
此密碼用來加密任何新增至 HGS 使用 PFX 檔案 （而不是憑證指紋） 的憑證。

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> 如果您使用系統管理信任證明，您必須分別備份在授權的受防護的主機使用 HGS 的安全性群組的成員資格。
> HGS 只會備份 SID 的安全性群組，非其內成員資格。
> 在事件在發生災害時，會遺失這些群組，您必須重新建立群組，然後再次新增，每一部受防護的主機。

**備份憑證**

`Export-HgsServerState`命令會備份任何 PFX 憑證加入當時的 HGS 命令執行。
如果您將憑證加入 HGS 使用憑證指紋 （一般用於不可匯出和支援硬體的憑證） 時，您必須手動備份您的憑證的私密金鑰。
若要識別哪些憑證向 HGS 註冊，而且需要手動備份，請執行下列 PowerShell 命令的任何工作 HGS 伺服器節點上。

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

針對每個所列的憑證，您必須手動備份私密金鑰。
如果您使用非匯出以軟體為基礎的憑證，您應該連絡您的憑證授權單位，以確保它們有憑證的備份及 （或） 可以發出視。
建立並儲存在硬體安全性模組中的憑證，您應該參閱您的裝置，如需規劃嚴重損壞修復的指引的文件。

如此這兩項才能一起還原，您應該儲存憑證備份以及證明原則備份在安全的位置。

**若要備份的其他設定**

備份設定 HGS 伺服器狀態不會包含您的 HGS 叢集名稱，從 Active Directory 的任何資訊或任何使用 SSL 憑證來保護與 HGS Api 通訊。
這些設定是很重要的一致性，但若要取得您的 HGS 叢集上線，在災害發生後就不再重要。

若要擷取的 HGS 服務名稱，請執行`Get-HgsServer`並記下中證明與金鑰保護 Url 的一般名稱。
比方說，如果證明 URL 為"<http://hgs.contoso.com/Attestation>"，"hgs 」 是 HGS 服務名稱。

由 HGS 的 Active Directory 網域應管理像是任何其他 Active Directory 網域。
還原時 HGS 在災害發生後，您就不一定需要重新建立目前網域中存在的確切物件。
不過，它會讓復原輕鬆如果您備份 Active Directory，並保留一份獲授權管理系統，以及用來授權受守護的主機系統管理信任證明的任何安全性群組的成員資格的 JEA 使用者。

若要識別為 HGS 設定 SSL 憑證的指紋，請在 PowerShell 中執行下列命令。
您接著可以備份這些 SSL 憑證，根據您的憑證提供者的指示。

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>從備份還原 HGS
下列步驟說明如何從備份還原 HGS 設定。
步驟都與您嘗試復原您已在執行的 HGS 執行個體，並在您要在您的上一個完全遺失之後建立全新的 HGS 叢集時所做的變更這兩種情況。

#### <a name="set-up-a-replacement-hgs-cluster"></a>設定取代 HGS 叢集
您可以還原 HGS 之前，您需要有初始化的 HGS 叢集可供您還原設定。
如果您只需匯入至現有的 （執行） 叢集意外刪除的設定，您可以略過此步驟。
如果您正從 HGS 完全遺失中復原，您必須安裝和初始化至少一個 HGS 節點以下[deployment guide 》 中的指導方針](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。

具體來說，您將需要：
1. [設定 HGS 網域](guarded-fabric-choose-where-to-install-hgs.md)或加入現有網域中的 HGS
2. [初始化 HGS 伺服器](guarded-fabric-initialize-hgs.md)使用您現有的金鑰*或*一組暫存索引鍵。 您可以[移除暫存索引鍵](#renewing-or-replacing-keys)HGS 從匯入您的實際金鑰之後備份檔案。
3. [匯入 HGS 設定](#import-settings-from-a-backup)從您的備份來還原受信任的主機群組、 程式碼完整性原則，TPM 基準和 TPM 識別碼

> [!TIP]
> 新的 HGS 叢集不需要使用相同的憑證、 服務名稱或網域與 HGS 執行個體匯出您的備份檔案的來源。

#### <a name="import-settings-from-a-backup"></a>從備份匯入設定

若要還原證明原則、 PFX 憑證，並從備份檔案，您的 HGS 節點的非 PFX 憑證的公開金鑰會執行下列命令初始化的 HGS 伺服器節點上。
系統會提示您輸入建立備份時所指定的密碼。

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

如果您只想要匯入系統管理信任證明原則或 TPM 信任證明原則，則可以藉由指定`-ImportActiveDirectoryModeState`或是`-ImportTpmModeState`旗標，用於[匯入 HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)。

Windows Server 2016 已安裝在執行之前，請確定最新的累積更新`Import-HgsServerState`。
若要這樣做可能會導致匯入錯誤。

> [!NOTE]
> 如果您還原上現有的 HGS 節點已經有一或多個安裝這些原則的原則時，匯入命令會顯示針對每個重複的原則錯誤。
> 這是預期的行為，而且在大部分情況下可以放心忽略。

#### <a name="reinstall-private-keys-for-certificates"></a>重新安裝憑證的私密金鑰
如果任何上 HGS 建立備份時所使用的憑證已新增使用指紋，只有這些憑證的公開金鑰會納入備份的檔案。
這表示，您必須手動安裝和/或 HGS 可以從 HYPER-V 主機的要求提供服務之前，將私用金鑰的存取權授與每個這些憑證。
完成該步驟所需的動作取決於如何初次發行您的憑證。
對於軟體為基礎的憑證，憑證授權單位所核發，您必須連絡您的 CA 取得私用金鑰，並將它安裝在**每個**HGS 節點，每個指令。
同樣地，如果您的憑證與硬體支援，您必須連線到 HSM，並授與每個機器私密金鑰存取權的每一個 HGS 節點上安裝必要的驅動程式的硬體安全性模組廠商的文件，請參閱。

提醒您，新增至 HGS 使用指紋的憑證都需要手動複寫的每個節點的私用金鑰。
您必須重複此步驟中每個要新增到已還原的 HGS 叢集的其他節點上。

#### <a name="review-imported-attestation-policies"></a>檢閱已匯入的證明原則
您已從備份匯入您的設定之後，建議您仔細檢閱所有匯入的原則使用`Get-HgsAttestationPolicy`以確定只有您信任來執行受防護的 Vm 的主機，才能夠成功證明。
如果您發現它不再符合您的安全性狀態的任何原則，您可以[停用或移除它們](#review-attestation-policies)。

#### <a name="run-diagnostics-to-check-system-state"></a>執行診斷，以檢查系統狀態
完成設定並還原您的 HGS 節點的狀態之後，您應該執行 HGS 診斷工具來檢查系統的狀態。
若要這樣做，下列命令 HGS 在節點上執行您用來還原組態：

```powershell
Get-HgsTrace -RunDiagnostics
```

如果 「 整體結果 」 是 「 通過 」，額外的步驟，才能完成設定系統。
請檢查回報 subtest(s) 失敗如需詳細資訊中的訊息。

## <a name="patching-hgs"></a>修補 HGS
請務必保持您的主機守護者服務節點的最新狀態，藉由安裝最新的累積更新，當它推出。如果您要設定全新的 HGS 節點，強烈建議您安裝任何可用的更新，才能安裝 HGS 角色，或加以設定。
這樣做可確保任何新的或變更的功能將會立即生效。

當修補程式的受防護網狀架構，強烈建議您先升級*所有*HYPER-V 主機**然後再升級 HGS**。
這是為了確保 HGS 證明原則的任何變更都會*之後*HYPER-V 主機已更新為提供它們所需的資訊。
如果更新要變更原則的行為，它們將不會自動啟用以避免中斷您的網狀架構。
這類更新都需要您依照下列章節，以啟用新的或已變更的證明原則中的指導方針。
我們鼓勵您閱讀版本資訊適用於 Windows Server 和檢查原則更新是否需要安裝任何累計更新。

### <a name="updates-requiring-policy-activation"></a>需要原則啟用的更新
如果 HGS 的更新會導入了或大幅變更證明原則的行為，額外的步驟，才能啟用變更的原則。
匯出和匯入的 HGS 狀態後才制定原則變更。
您套用至所有主機與 HGS 的所有節點的累積更新，您的環境中之後，應該只能啟用新的或變更原則。
每一部機器已更新之後，若要升級的程序會觸發任何 HGS 節點上執行下列命令：

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

如果引進了新的原則，則會停用預設。
若要啟用新的原則，先找到 Microsoft 原則 （具有前置的 'HGS_'） 的清單，並使用下列命令加以啟用：

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>證明的管理原則
HGS 會維護數個證明原則會定義最小的一組需求主機必須符合才能被視為 「 良好 」，而且允許執行受防護的 Vm。
這些原則中的某些 Microsoft 所定義，其他人會新增您環境中定義的可允許的程式碼完整性原則，TPM 基準和主機。
這些原則的定期維護，才能確保主機能夠繼續證明正確更新，並取代，並確保任何不受信任的主機或設定已封鎖成功證明。

針對系統管理信任證明沒有用來決定是否狀況良好主機只能有一個原則： 中已知且信任的安全性群組的成員資格。
TPM 證明更為複雜，並牽涉到不同的原則，來測量程式碼和系統的設定，然後再判斷它是否狀況良好。

單一的 HGS 可以使用原則來設定 Active Directory 與 TPM，但該服務只會檢查目前的模式，當主機嘗試證明時，它已設定好的原則。
若要檢查您的 HGS 伺服器模式，請執行`Get-HgsServer`。

### <a name="default-policies"></a>預設原則
TPM 信任證明時，有數個內建的原則上 HGS 設定。
這些原則中的某些會 「 鎖定 」，這表示它們無法停用基於安全性考量。
下表說明每個預設原則的目的。

原則名稱                    | 用途
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | 需要主機，以啟用安全開機。 這是為了測量啟動二進位檔和其他與 UEFI 鎖定的設定。
Hgs_UefiDebugDisabled          | 可確保主機並沒有啟用核心偵錯工具。 使用者模式偵錯工具會遭到封鎖的程式碼完整性原則。
Hgs_SecureBootSettings         | 負數的原則，以確保主機符合至少一個 （系統管理員定義） 的 TPM 基準。
Hgs_CiPolicy                   | 負數的原則，以確保主機使用其中一個系統管理員定義的 CI 原則。
Hgs_HypervisorEnforcedCiPolicy | 需要由 hypervisor 所強制執行的程式碼完整性原則。 停用此原則會削弱您防範核心模式程式碼完整性原則攻擊。
Hgs_FullBoot                   | 可確保主機未無法從睡眠或休眠狀態恢復執行。 主機必須正確地重新啟動或關閉，若要通過這個原則。
Hgs_VsmIdkPresent              | 需要在主機上執行的虛擬化架構安全性。 IDK 表示加密資訊傳回給主應用程式安全的記憶體空間所需的金鑰。
Hgs_PageFileEncryptionEnabled  | 需要加密在主機上的分頁檔。 停用此原則可能導致資料曝光，如果未加密的分頁檔會進行檢查的租用戶密碼。
Hgs_BitLockerEnabled           | 需要在 HYPER-V 主機上啟用 BitLocker。 基於效能考量的預設會停用此原則，並建議您不要啟用。 此原則的受防護的 Vm 本身加密並無任何影響。
Hgs_IommuEnabled               | 需要主機具有 IOMMU 裝置，以防止直接記憶體存取攻擊的使用中。 停用此原則，並使用主機，而不需要啟用 IOMMU 可以公開將記憶體內部攻擊的租用戶 VM 密碼。
Hgs_NoHibernation              | 需要停用 HYPER-V 主機上的休眠。 停用此原則可能會允許未加密的休眠檔案來儲存受防護的 VM 記憶體的主機。
Hgs_NoDumps                    | 需要停用 HYPER-V 主機上的記憶體傾印。 如果您停用此原則時，建議您設定儲存到未加密的損毀傾印檔案時，防止受防護的 VM 記憶體的傾印加密。
Hgs_DumpEncryption             | 如果啟用在 HYPER-V 主機上，使用信任的 HGS 的加密金鑰加密，則需要記憶體傾印。 如果主機上未啟用傾印，則不適用此原則。 如果此原則並*Hgs\_NoDumps*都會停用，受防護的 VM 記憶體無法儲存到未加密的傾印檔案。
Hgs_DumpEncryptionKey          | 負數的原則，以確保主機設定為允許記憶體傾印會使用已知 HGS 系統管理員定義的傾印檔案加密金鑰。 此原則不會套用的時機*Hgs\_DumpEncryption*已停用。

### <a name="authorizing-new-guarded-hosts"></a>新增授權受防護主機
若要新的主機，以成為受防護的主機 （例如證明成功），HGS 必須信任主應用程式和 （當設定為使用 TPM 信任證明） 在其上執行的軟體。
若要授權的新主機步驟根據目前設定 HGS 的證明模式而有所不同。
若要檢查您的受防護網狀架構的證明模式，請執行`Get-HgsServer`HGS 的任何節點上。

#### <a name="software-configuration"></a>軟體設定
在新的 HYPER-V 主機上，請確定已安裝的 Windows Server 2016 Datacenter edition。
Windows Server 2016 Standard 不在受防護網狀架構執行受防護的 Vm。
主應用程式可能已安裝桌面體驗] 或 [Server Core。

在含有桌面體驗的伺服器和 Server Core 上，您需要安裝 HYPER-V 和主機守護者 HYPER-V 支援的伺服器角色：

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>系統管理信任證明
若要使用系統管理信任證明時，請在 HGS 註冊新的主機，您必須先在已加入網域中新增主應用程式安全性群組。
一般而言，每一個定義域將會有一個安全性群組受守護的主機。
如果您已經與 HGS 註冊該群組，唯一需要採取的動作是重新啟動主機重新整理其群組成員資格。

您可以檢查哪些安全性群組時，所信任的 HGS 上，執行下列命令：

```powershell
Get-HgsAttestationHostGroup
```

若要向 HGS 註冊新的安全性群組，請先擷取主機的網域中的群組安全性識別碼 (SID)，並向 HGS 註冊 SID。

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

部署指南中提供有關如何設定 HGS 與主應用程式定義域之間的信任。

#### <a name="tpm-trusted-attestation"></a>TPM 信任證明
當 HGS 設定 TPM 模式時，主機必須通過所有鎖定的原則和 「 啟用 」 的原則，加上"Hgs_ 」，以及至少一個 TPM 基準、 TPM 識別碼和程式碼完整性原則。
每次您新增新的主機，您必須向 HGS 註冊新的 TPM 識別碼。
只要主機正在執行相同的軟體 （且已套用相同的程式碼完整性原則） 為您的環境中的另一部主機的 TPM 基準，您將不需要將新的 CI 原則或基準。

**加入新的主控件的 TPM 識別碼**在新的主機上，執行下列命令，以擷取 TPM 識別碼。
請務必指定之主控件將協助您查閱 HGS 上唯一的名稱。
如果您解除委任主機，或想要防止在 HGS 執行受防護的 Vm，您將需要這項資訊。

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

這個檔案複製到您的 HGS 伺服器，然後執行下列命令，以向 HGS 註冊主機。

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**加入新的 TPM 基準**如果新的硬體或韌體設定為您的環境執行新的主機，您可能需要採用新的 TPM 基準。
若要這樣做，請在主機上執行下列命令。

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> 如果您收到錯誤，指出您的主機驗證失敗，將不成功，證明客戶資料，請不要擔心。
> 這是必要條件檢查，以確定您的主機可以執行受防護的 Vm，並可能表示您已不尚未套用的程式碼完整性原則或其他必要設定。
> 讀取錯誤訊息，進行任何變更，所建議，然後再試一次。
> 或者，您可以跳過這一次驗證加上`-SkipValidation`命令旗標。

將 TPM 基準複製到您的 HGS 伺服器，然後使用下列命令註冊。
我們鼓勵您使用的命名慣例，可協助您了解這個類別的 HYPER-V 主機的硬體和韌體設定。

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**加入新的程式碼完整性原則**如果您已變更您的 HYPER-V 主機上執行的程式碼完整性原則，您必須向 HGS 註冊新的原則之前可以成功證明這些主機。
在參考主機上，以做為受信任的 HYPER-V 機器，您的環境中的主要映像，擷取新的 CI 原則使用`New-CIPolicy`命令。
我們鼓勵您使用**FilePublisher**層級並**雜湊**後援的 HYPER-V 主機 CI 原則。
您應該先建立 CI 原則，在稽核模式，以確保一切運作正常。
驗證之後在系統上的以範例工作負載，您可以強制執行原則，並強制執行的版本複製到 HGS。
如需程式碼完整性原則組態選項的完整清單，請參閱[Device Guard 文件](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

一旦您建立的原則之後，測試並強制執行，將二進位檔案 (.p7b) 複製到您的 HGS 伺服器，並註冊原則。

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**新增記憶體傾印加密金鑰**

當*Hgs\_NoDumps*原則已停用並*Hgs\_DumpEncryption*原則已啟用，允許 （包括損毀傾印） 的記憶體傾印一定要受守護的主機已啟用，只要這些傾印會加密。 受防護的主機只會通過證明，如果它們已停用的記憶體傾印，或者使用已知 HGS 金鑰加密。 根據預設，任何傾印加密金鑰上 HGS 不設定。

若要加入 HGS 傾印加密金鑰，請使用`Add-HgsAttestationDumpPolicy`HGS 提供您的傾印加密金鑰的雜湊的 cmdlet。
如果您擷取傾印加密設定的 HYPER-V 主機上的 TPM 基準時，雜湊納入 tcglog，且可供`Add-HgsAttestationDumpPolicy`cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

或者，您可以直接提供給 cmdlet 的雜湊的字串表示。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

請務必將每個唯一的傾印加密金鑰新增至 HGS，如果您選擇使用您的受防護網狀架構的不同索引鍵。
會使用不知道 HGS 金鑰加密記憶體傾印的主機不會通過證明。

請參閱 HYPER-V 文件如需詳細資訊的相關[設定傾印在主機上的加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。

#### <a name="check-if-the-system-passed-attestation"></a>檢查系統是否通過證明
在之後與 HGS 註冊所需的資訊，您應該檢查是否主機通過證明。
在新加入 HYPER-V 主機上，執行`Set-HgsClientConfiguration`並提供您的 HGS 叢集正確的 Url。
可取得這些 Url，請執行`Get-HgsServer`HGS 的任何節點上。

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

如果產生的狀態不會指出 「 IsHostGuarded:為 true，則 「 您必須針對組態進行疑難排解。
在通過證明，主機上，執行下列命令，以取得詳細的報表，可協助您解決失敗的證明的問題。

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> 如果您使用 Windows Server 2019 或 Windows 10 版本 1809年，並使用程式碼完整性原則，`Get-HgsTrace`可能傳回的失敗**程式碼完整性原則 Active**診斷。
> 僅失敗診斷時，您可以放心忽略此結果。

### <a name="review-attestation-policies"></a>證明審核
若要檢閱目前的 HGS 上設定的原則狀態，請 HGS 的任何節點上執行下列命令：

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

如果您發現啟用不再符合您的安全性需求 （例如舊程式碼完整性原則會被視為不安全的） 原則，您可以停用其取代下列命令中的原則名稱：

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

同樣地，您可以使用`Enable-HgsAttestationPolicy`重新啟用的原則。

如果您不再需要的原則且想来移除所有的 HGS 節點，執行`Remove-HgsAttestationPolicy -Name 'PolicyName'`永久刪除該原則。

## <a name="changing-attestation-modes"></a>變更證明模式
如果您開始使用系統管理信任證明您受防護網狀架構，您可能要升級至更更強的 TPM 證明模式，只要您的環境中有足夠的 TPM 2.0 相容主機。
當您準備好切換時，您可以預先載入中 HGS 的證明成品 （CI 原則、 TPM 基準和 TPM 識別碼） 的所有時繼續執行 HGS 使用系統管理信任證明。
若要這樣做，只需依照中的指示[授權新的受防護的主機](#authorizing-new-guarded-hosts)一節。

一旦您已加入您的原則的所有 HGS 下, 一個步驟就是您查看如果它們還會通過證明，TPM 模式的主機上執行的綜合證明嘗試。
這不會影響 HGS 的目前操作狀態。
必須執行下列命令，可存取所有的環境和至少一個 HGS 節點中的主機電腦上。
如果您的防火牆或其他安全性原則防止這個情況，您可以略過此步驟。
如果可能的話，我們建議您執行綜合的證明，讓您清楚指出是否 「 翻轉 」 為 TPM 模式會造成停機時間為您的 Vm。 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

完成診斷之後檢閱輸出的資訊，以判斷任何主機會有無法證明在 TPM 模式。
重新執行診斷，直到您取得 「 pass 」，從每個主機，則繼續進行將 HGS 變更 TPM 模式。

**變更為 TPM 模式**採用秒即可完成。
若要更新的證明模式的任何 HGS 節點上執行下列命令。

```powershell
Set-HgsServer -TrustTpm
```

如果您遇到問題而需要切換回 Active Directory 模式，則可以藉由執行`Set-HgsServer -TrustActiveDirectory`。

一旦確認一切如預期般運作之後，您應該從 HGS 移除所有受信任的 Active Directory 主機群組，並移除 HGS 和網狀架構的網域之間的信任。
如果您將保留在位置的 Active Directory 信任，就可能有人重新啟用信任和 Active Directory 模式下，可能會允許不受信任的程式碼，以執行受防護主機上的 未選取要切換 HGS。

## <a name="key-management"></a>金鑰管理
受防護網狀架構解決方案會使用數個的公開/私密金鑰組，以驗證在方案中的各種元件的完整性，並將租用戶的密碼加密。
主機守護者服務已使用至少兩個憑證 （使用公用和私用金鑰），用來簽署與加密金鑰用來啟動受防護的 Vm。
必須仔細管理這些金鑰。
如果攻擊者取得私密金鑰，他們將能夠 unshield 網狀架構上執行的任何 Vm，或設定會使用較弱的證明原則，略過您放就地保護冒充的 HGS 叢集。
您應該在發生災害時遺失的私密金鑰，找不到備份中，您必須設定新的金鑰組並在重新授權您的新憑證為索引鍵的每個 VM。

本節涵蓋一般的金鑰管理主題，以協助您設定您的金鑰，讓他們能夠正常運作和安全。

### <a name="adding-new-keys"></a>加入新的金鑰
雖然 HGS 必須初始化使用一組金鑰，您可以新增一個以上的加密和簽署金鑰來 HGS。
為什麼您會加入新的金鑰 HGS 兩個最常見原因如下：
1. 若要支援 「 攜帶您自己的金鑰 」 案例，其中租用戶將其私密金鑰複製到您的硬體安全性模組，而只能授權讓他們啟動其受防護的 Vm 的金鑰。
2. 若要將現有的索引鍵的 HGS，第一次新增新的金鑰，並保留這兩組金鑰，直到每個 VM 組態已更新為使用新的金鑰。

新增您新的索引鍵的程序根據您使用的憑證類型而有所不同。

**選項 1:將憑證儲存在 HSM 中新增**

我們建議的方法，來保護 HGS 金鑰是使用硬體安全性模組 (HSM) 中建立的憑證。
Hsm，請確定使用您的金鑰會繫結至實體存取您的資料中心的安全性顧慮裝置。
每個受 HSM 不同且唯一的處理程序，來建立憑證，並向 HGS 註冊它們。
下列步驟可提供概略方針使用 HSM 支援的憑證。
如需確切的步驟和功能，請參閱 HSM 廠商的文件。

1. 在叢集中的每一個 HGS 節點上安裝 HSM 軟體。 根據您是否有網路或本機 HSM 裝置，您可能需要設定 HSM，以授與您的電腦存取其金鑰存放區。
2. 建立 2 個憑證中使用 HSM **2048 位元 RSA 金鑰**用於加密和簽署
    1. 建立加密憑證**資料編密**金鑰在您 HSM 中的 usage 屬性
    2. 建立與簽署憑證**數位簽章**金鑰在您 HSM 中的 usage 屬性
3. 您的 HSM 廠商指導的每個 HGS 節點的本機憑證存放區中安裝憑證。
4. 如果您的 HSM 使用細微權限授與特定的應用程式或使用者使用的私用金鑰的權限，您必須授與您 HGS 群組受控服務帳戶憑證的存取權。 您可以執行，以尋找 HGS gMSA 帳戶的名稱 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. 加入 HGS 的簽署和加密憑證指紋取代這些憑證的下列命令中的：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**選項 2:新增不可匯出的軟體憑證**

如果您有貴公司所發出的支持軟體憑證或非可匯出的私密金鑰是公開憑證授權單位，您必須將您的憑證新增至 HGS 使用其憑證指紋。
1. 根據您的憑證授權單位的指示您電腦上安裝憑證。
2. 授與 HGS 群組受控服務帳戶讀取私密金鑰的憑證。 您可以執行，以尋找 HGS gMSA 帳戶的名稱 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. 向 HGS 使用下列命令，並在您的憑證指紋替換中的憑證 (變更*加密*要*簽署*的簽署憑證):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> 您必須手動安裝的私密金鑰，並授與 gMSA 帳戶，每個 HGS 節點上的 「 讀取 」 權限。
> HGS 無法自動複寫的私密金鑰*任何*註冊其憑證指紋的憑證。

**選項 3:將 PFX 檔案中儲存的憑證**

如果您有可匯出私密金鑰，可為 PFX 檔案格式儲存及使用密碼保護的支持軟體憑證，HGS 可以自動為您管理您的憑證。
使用 PFX 檔案新增的憑證會自動複寫至您的 HGS 叢集中的每個節點並 HGS 保護私用金鑰的存取權。
若要加入新的憑證使用 PFX 檔案，請執行下列命令 HGS 的任何節點上 (變更*加密*要*簽署*的簽署憑證):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**識別及變更主要憑證**雖然 HGS 可以支援多個簽章和加密的憑證，它會使用一組做為其 「 主要 」 的憑證。
這些是如果有人下載守護者中繼資料，為該 HGS 叢集就會使用憑證。
若要檢查哪個憑證目前標示為主要憑證，請執行下列命令：

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

若要設定新的主要加密或簽署憑證，找到所需的憑證指紋，並將它標示為主要使用下列命令：

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>更新或取代金鑰
當您建立 HGS 所使用的憑證時，憑證會指派一個到期日，根據您的憑證授權單位原則與您要求的資訊。
一般來說，在其中憑證的有效性是很重要，例如保護 HTTP 通訊的情況下，憑證必須更新以避免服務中斷或令人擔憂的錯誤訊息在到期前。
這個方面來看，HGS 不使用憑證。
HGS 只使用憑證作為簡便的方式，來建立及儲存非對稱金鑰組。
弱式或遺失的受防護 Vm 的保護，不會指出已過期的加密或 HGS 的簽署憑證。
此外，HGS 不會執行憑證撤銷檢查。
如果 HGS 憑證或發行授權單位的憑證已撤銷，並不會影響憑證 HGS 的使用。

您不必擔心 HGS 憑證的唯一時間是如果您有理由相信它的私密金鑰已遭到竊取。
在此情況下，您受防護的 Vm 的完整性有風險中是因為 HGS 加密和簽署金鑰組的私用部分擁有足以移除 VM 上的虛擬機器防護保護或假的 HGS 伺服器具有較弱的證明原則，就能。

如果您發現自己處於這種情況下，或定期重新整理憑證金鑰所需的合規性標準，則下列步驟會概述變更 HGS 伺服器上的索引鍵的程序。
請注意下列指導方針，代表相當費力，會導致服務中斷由 HGS 叢集中的每個 VM 的工作。
適當的規劃，變更 HGS 金鑰，才能減少服務中斷，並確保租用戶虛擬機器的安全性。

HGS 在節點上，執行下列步驟來註冊新的加密和簽章憑證組。
請參閱 〈[加入新的金鑰](#adding-new-keys)如需詳細資訊的各種方式將新的金鑰新增至 HGS。
1. 建立新的加密和簽署憑證，讓您的 HGS 伺服器組。 在理想情況下，這些會在硬體安全性模組中建立。
2. 註冊新的加密和簽署憑證與**新增 HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. 如果您使用指紋時，您必須移至叢集中安裝私密金鑰，並授與 HGS gMSA 金鑰的存取權的每個節點。
4. 在新的憑證的預設憑證 HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

此時，防護資料使用取自 HGS 節點的中繼資料建立會使用新的憑證，但現有的 Vm 仍會繼續運作，因為較舊的憑證仍然會有的。
為了確保所有現有的 Vm 將會使用新的金鑰，您必須更新每個 VM 上的金鑰保護裝置。
這是需要參與其中的 VM 擁有者 （個人或 「 擁有者 」 守護者擁有的實體） 的動作。
針對每個受防護的 VM，執行下列步驟：
5. 關閉 VM。 VM 無法開啟回之前的剩餘的步驟都完成，否則您必須重新啟動程序。
6. 將目前的金鑰保護裝置儲存到檔案： `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. VM 擁有者的傳輸 KP
8. 具有擁有者下載更新的守護者資訊，從 HGS 並匯入其本機系統
9. 讀入記憶體中的目前 KP、 的新守護者存取權授與 KP，並將它儲存到新的檔案中，執行下列命令：

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. 將更新的 KP 複製回到的主控網狀架構
11. 適用於 KP 原始的 VM:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. 最後，啟動 VM，並確認已順利執行。

> [!NOTE]
> 如果 VM 擁有者 VM 上設定不正確的金鑰保護裝置，而且不會授權您的網狀架構執行的 VM，您將無法啟動受防護的 VM。
> 若要回到上一個已知良好的金鑰保護裝置，請執行 `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

一旦授權的新守護者金鑰更新所有 Vm 之後，您可以停用並移除舊的金鑰。

13. 取得從舊的憑證指紋 `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. 執行下列命令來停用每個憑證：  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Vm 皆開始仍然可以停用，將憑證從移除憑證 HGS 藉由執行下列命令：

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> VM 備份會包含舊的金鑰保護裝置資訊，讓舊的憑證，用來啟動 VM。
> 如果您知道您的私人金鑰已遭入侵時，您應該假設 VM 備份可能會遭到洩漏，喔，並採取適當動作。
> 終結的備份 (.vmcx) 的 VM 組態，將會移除金鑰保護裝置，但代價是需要使用 BitLocker 修復密碼，可在下一次啟動 VM。

### <a name="key-replication-between-nodes"></a>主要節點之間的複寫
相同的加密、 簽署，而且 （當設定），就必須設定 HGS 叢集中的每個節點的 SSL 憑證。
這是為了確保連絡叢集中任何節點的 HYPER-V 主機可以成功地處理其要求。

**如果您初始化與 PFX 憑證的 HGS 伺服器**則 HGS 會自動在叢集中的每個節點間複寫這些憑證的公用和私用金鑰。
您只需要一個節點上新增的機碼。

**如果初始化與憑證參考的 HGS 伺服器**或只會複寫的憑證指紋，則 HGS*公用*索引鍵中每個節點的憑證。
此外，HGS 無法授與本身存取在此案例中的任何節點上的私密金鑰。
因此，它是由您負責：
1. HGS 的每個節點上安裝的私用金鑰
2. 授與 HGS 群組受控服務帳戶 (gMSA) 存取權，每個節點上的私密金鑰這些工作會新增額外作業的負擔，不過它們所需的 hsm 型金鑰，並使用非可匯出私密金鑰憑證。

**SSL 憑證**永遠不會複寫任何形式。
您必須負責初始化相同的 SSL 憑證的每一部 HGS 伺服器，並更新每一部伺服器，每當您選擇要更新或取代的 SSL 憑證。
當取代的 SSL 憑證，建議您可以使用[組 HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet。

## <a name="unconfiguring-hgs"></a>取消設定 HGS

如果您需要解除委任或大幅重新設定 HGS 伺服器，做法使用[清除 HgsServer](https://technet.microsoft.com/library/mt652176.aspx)或是[解除安裝 HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet。

### <a name="clearing-the-hgs-configuration"></a>清除 HGS 設定

若要從叢集移除節點 HGS，使用[清除 HgsServer](https://technet.microsoft.com/library/mt652176.aspx) cmdlet。
此 cmdlet 會在執行所在的伺服器上進行下列變更：

- 取消註冊的證明和金鑰保護服務
- 移除 「 microsoft.windows.hgs"JEA 管理端點
- 在本機電腦移除 HGS 容錯移轉叢集

如果伺服器是在叢集中的最後一個 HGS 節點，也會終結，叢集和其對應的分散式網路名稱資源。

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

清除作業完成之後，可以使用重新初始化 HGS 伺服器[Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)。
如果您使用[安裝 HgsServer](https://technet.microsoft.com/library/mt652169.aspx)若要設定 Active Directory 網域服務網域，該網域之後仍會保留已設定且可運作的清除作業。

### <a name="uninstalling-hgs"></a>解除安裝 HGS

如果您想要從 HGS 叢集中移除節點**並**在其上執行 Active Directory 網域控制站降級，請使用[解除安裝 HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet。
此 cmdlet 會在執行所在的伺服器上進行下列變更：

- 取消註冊的證明和金鑰保護服務
- 移除 「 microsoft.windows.hgs"JEA 管理端點
- 在本機電腦移除 HGS 容錯移轉叢集
- 如果設定，將 Active Directory 網域控制站，降級

如果伺服器是在叢集中的最後一個 HGS 節點，也會終結網域、 容錯移轉叢集和叢集的分散式網路名稱資源。

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

解除安裝作業已完成並重新啟動電腦之後，您可以重新安裝 ADDC 和使用的 HGS[安裝 HgsServer](https://technet.microsoft.com/library/mt652169.aspx)或將電腦加入網域，並初始化與該網域中的 HGS 伺服器[初始化 HgsServer](https://technet.microsoft.com/library/mt652185.aspx)。

如果您不再想要使用的 HGS 節點的電腦，您就可以從 Windows 移除角色。

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
