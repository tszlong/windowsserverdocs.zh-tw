---
description: 深入瞭解：管理主機守護者服務
title: 管理主機守護者服務
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 12/10/2020
ms.openlocfilehash: 54f39b309c8853c5fd7a30178dffe0e11d7f8411
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949174"
---
# <a name="managing-the-host-guardian-service"></a>管理主機守護者服務

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

主機守護者服務 (HGS) 是受防護網狀架構解決方案的文字核心。
它負責確保主機服務提供者或企業以及執行受信任的軟體，以及管理用來啟動受防護 Vm 的金鑰，都知道網狀架構中的 Hyper-v 主機。
當租使用者決定信任您裝載其受防護的 Vm 時，它們會在您的主機守護者服務設定和管理中進行信任。
因此，請務必遵循最佳作法來管理主機守護者服務，以確保受防護網狀架構的安全性、可用性和可靠性。
下列各節中的指導方針解決了 HGS 系統管理員最常面臨的操作問題。

## <a name="limiting-admin-access-to-hgs"></a>限制對 HGS 的系統管理員存取
由於 HGS 的安全性敏感性本質，請務必確保其系統管理員是您組織的高度信任成員，而且在理想的情況下，會與您網狀架構資源的系統管理員分開。
此外，建議您只使用安全通訊協定（例如 WinRM over HTTPS）從安全工作站管理 HGS。

### <a name="separation-of-duties"></a>職責區分
設定 HGS 時，您可以選擇只針對 HGS 建立隔離的 Active Directory 樹系，或將 HGS 加入現有的受信任網域。
這項決定以及您在組織中指派系統管理員的角色，會決定 HGS 的信任界限。
誰可以存取 HGS，無論是直接以系統管理員身分，還是以其他方式來管理 (例如，可能會影響 HGS 的 Active Directory) ，都可以控制您的受防護網狀架構。
HGS 系統管理員會選擇要授權哪些 Hyper-v 主機執行受防護的 Vm，以及管理啟動受防護 Vm 所需的憑證。
具有 HGS 存取權的攻擊者或惡意管理員可以使用這項功能來授權遭入侵的主機執行受防護的 Vm，藉由移除金鑰材料來起始阻絕服務攻擊等等。

為了避免這種風險， *強烈* 建議您限制 hgs 的系統管理員之間的重迭 (包括 hgs 加入的網域) 和 hyper-v 環境。
藉由確保沒有任何系統管理員可以存取這兩個系統，攻擊者必須從2個人入侵2個不同的帳戶，才能完成他的任務來變更 HGS 原則。
這也表示，這兩個 Active Directory 環境的網域和企業系統管理員不應該是同一人，而 HGS 使用的 Active Directory 樹系與您的 Hyper-v 主機相同。
任何可以授與自己更多資源存取權的人都有安全性風險。

### <a name="using-just-enough-administration"></a>使用剛好足夠的管理
HGS 有 [足夠的管理](/powershell/scripting/learn/remoting/jea/overview) (JEA 內建的) 角色，可協助您更安全地進行管理。
JEA 可讓您將系統管理工作委派給非系統管理員使用者，這表示管理 HGS 原則的人員實際上不需要是整個電腦或網域的系統管理員。
JEA 的運作方式是限制使用者可以在 PowerShell 會話中執行的命令，並使用幕後的暫存本機帳戶 (每個使用者會話的唯一) 來執行通常需要提高許可權的命令。

HGS 隨附預先設定的2個 JEA 角色：
- **Hgs 系統管理員** ，可讓使用者管理所有 HGS 原則，包括授權新的主機來執行受防護的 vm。
- **HGS 審核者** ，只允許使用者審核現有的原則。 他們無法對 HGS 設定進行任何變更。

若要使用 JEA，您必須先建立新的標準使用者，並將其設為 HGS admins 或 HGS 審核者群組的成員。
如果您使用 `Install-HgsServer` 設定 hgs 的新樹系，則這些群組分別會命名為「*servicename* 系統管理員」和「*servicename* 審核者」，其中 *servicename* 是 hgs 叢集的網路名稱。
如果您將 HGS 加入現有的網域，您應該參考您在中指定的組名 `Initialize-HgsServer` 。

**為 HGS 系統管理員和審核者角色建立標準使用者**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**使用審核者角色審核原則**

在對 HGS 具有網路連線能力的遠端電腦上，于 PowerShell 中執行下列命令，以輸入具有審核者認證的 JEA 會話。
請務必注意，因為審核者帳戶只是標準使用者，所以無法用於一般 Windows PowerShell 遠端處理、對 HGS 的遠端桌面存取等。

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

然後，您可以使用來檢查會話中所允許的命令 `Get-Command` ，並執行任何允許的命令來進行設定。
在下列範例中，我們會檢查 HGS 上啟用的原則。

```powershell
Get-Command

Get-HgsAttestationPolicy
```

`Exit-PSSession` `exit` 當您完成使用 JEA 會話時，請輸入命令或其別名。

**使用系統管理員角色將新的原則新增至 HGS**

若要實際變更原則，您必須使用屬於 ' hgsAdministrators ' 群組的身分識別連接到 JEA 端點。
在下列範例中，我們會示範如何將新的程式碼完整性原則複製到 HGS，並使用 JEA 進行註冊。
語法可能與您習慣的不同。
這是為了因應 JEA 中的某些限制，例如無法存取完整的檔案系統。

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
來自 HGS 的事件會顯示在 Windows 事件記錄檔中的2個來源：
- **HostGuardianService-證明**
- **HostGuardianService-KeyProtection**

您可以藉由開啟事件檢視器並流覽至 HostGuardianService 證明與 Microsoft-Windows-HostGuardianService-KeyProtection，來查看這些事件。

在大型環境中，通常最好將事件轉寄至中央 Windows 事件收集器，讓事件的分析更容易。
如需詳細資訊，請參閱 [Windows 事件轉送檔](/windows/win32/wec/windows-event-collector)。

### <a name="using-system-center-operations-manager"></a>使用 System Center Operations Manager
您也可以使用 System Center 2016-Operations Manager 來監視 HGS 和受防護主機。
受防護網狀架構管理元件有事件監視器，可檢查可能導致資料中心停機的常見錯誤，包括不通過證明的主機或報告錯誤的 HGS 伺服器。

若要開始使用，請 [安裝並設定 SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) ，然後 [下載受防護網狀架構管理元件](https://www.microsoft.com/download/details.aspx?id=52764)。
隨附的管理元件指南說明如何設定管理元件，並瞭解其監視的範圍。

## <a name="backing-up-and-restoring-hgs"></a>備份與還原 HGS
### <a name="disaster-recovery-planning"></a>災害復原規劃
在草擬您的嚴重損壞修復計畫時，請務必考慮受防護網狀架構中主機守護者服務的獨特需求。
如果您遺失部分或所有 HGS 節點，您可能會面臨立即可用的問題，以防止使用者啟動其受防護的 Vm。
在遺失整個 HGS 叢集的案例中，您必須準備好完整備份 HGS 設定，以還原 HGS 叢集並繼續正常作業。
本節涵蓋針對這類案例做好準備所需的步驟。

首先，請務必瞭解 HGS 對於備份的重要性。
HGS 保留數項資訊，可協助 it 人員判斷哪些主機已獲授權可執行受防護的 Vm。
這包括：
1. 使用 Active Directory 證明) 時， (包含受信任主機的群組 Active Directory 安全識別碼;
2. 您環境中每個主機的唯一 TPM 識別碼;
3. 主機各項唯一設定的 TPM 原則;和
4. 程式碼完整性原則，可決定允許在您的主機上執行的軟體。

這些證明成品需要與主機網狀架構的系統管理員協調，才能取得，可能會在嚴重損壞之後，讓您難以再次取得此資訊。

此外，HGS 需要存取2個以上的憑證，用來加密和簽署啟動受防護 VM (金鑰保護裝置) 所需的資訊。
受防護的 Vm 擁有者會使用這些憑證 (，以授權您的網狀架構執行其 Vm) ，而且必須在嚴重損壞後還原，以獲得順暢的復原體驗。
如果您不應該在發生嚴重損壞後使用相同的憑證來還原 HGS，則每個 VM 都必須更新，以授權您的新金鑰來解密其資訊。
基於安全性理由，只有 VM 擁有者可以更新 VM 設定以授權這些新的金鑰，這表示在發生嚴重損壞後無法還原金鑰，因此每個 VM 擁有者都需要採取動作才能再次執行其 Vm。

#### <a name="preparing-for-the-worst"></a>為最糟做好準備
若要準備完全遺失 HGS，您必須採取兩個步驟：
1. 備份 HGS 證明原則
2. 備份 HGS 金鑰

[備份 HGS](#backing-up-hgs)一節中會提供如何執行這兩個步驟的指引。

此外，建議您備份已獲授權可在其 Active Directory 網域或 Active Directory 本身管理 HGS 的使用者清單，但並非必要。

應定期進行備份，以確保資訊是最新的，並且安全地儲存，以避免遭到篡改或遭竊。

**建議您不要** 備份或嘗試還原 HGS 節點的整個系統映射。
如果您遺失整個叢集，最佳作法是設定全新的 HGS 節點，並只還原 HGS 狀態，而不是整個伺服器作業系統。

#### <a name="recovering-from-the-loss-of-one-node"></a>從一個節點遺失中復原
如果您遺失一或多個節點 (但並非 HGS 叢集中的每個節點) ，您只要依照《部署指南》中的指導方針， [將節點新增至您](guarded-fabric-configure-additional-hgs-nodes.md) 的叢集即可。
證明原則會自動同步，也就是提供給 HGS 的任何憑證，以及伴隨密碼的 PFX 檔案。
針對使用指紋新增至 HGS 的憑證 (不可匯出和硬體支援的憑證，通常是) ，您必須確定每個新節點都能存取每個憑證的私密金鑰。

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>從整個叢集遺失中復原
如果您的整個 HGS 叢集停止運作，而且您無法讓它重新上線，您將需要從備份還原 HGS。
從備份還原 HGS 需要先根據 [部署指南中的指引](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)來設定新的 HGS 叢集。
在設定復原 HGS 環境以協助從主機進行名稱解析時，強烈建議使用相同的叢集名稱，但並非必要。
使用相同的名稱，可避免必須使用新的證明和金鑰保護 Url 來重新設定主機。
如果您將物件還原至支援 HGS 的 Active Directory 網域，建議您先移除代表 HGS 叢集、電腦、服務帳戶和 JEA 群組的物件，再初始化 HGS 伺服器。

設定您的第一個 HGS 節點之後 (例如已安裝並初始化) ，您將遵循 [從備份還原 HGS](#restoring-hgs-from-a-backup) 的程式，以還原證明原則和公開金鑰保護憑證的公開金鑰。
您必須根據您的憑證提供者的指引，以手動方式還原憑證的私密金鑰 (例如，在 Windows 中匯入憑證，或設定) 的 HSM 支援憑證存取權。
設定第一個節點之後，您可以繼續將 [其他節點安裝到](guarded-fabric-configure-additional-hgs-nodes.md) 叢集，直到達到您想要的容量和復原能力。

### <a name="backing-up-hgs"></a>備份 HGS
HGS 系統管理員應負責定期備份 HGS。
完整備份將包含必須適當保護的機密金鑰內容。
如果不受信任的實體會取得這些金鑰的存取權，他們就可以使用該資料來設定惡意的 HGS 環境，以危害受防護的 Vm。

**備份證明原則** 若要備份 HGS 證明原則，請在任何工作中的 HGS 伺服器節點上執行下列命令。
系統會提示您提供密碼。
此密碼是用來加密任何使用 PFX 檔案新增至 HGS 的憑證 (而非憑證指紋) 。

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> 如果您使用系統管理員信任的證明，您必須在 HGS 使用的安全性群組中個別備份成員資格，以授權受防護的主機。
> HGS 只會備份安全性群組的 SID，而不是它們內的成員資格。
> 在發生嚴重損壞的情況下，您必須重新建立群組 () ，然後再將每個受防護的主機重新新增至這些群組。

**備份憑證**

`Export-HgsServerState`命令會在執行命令時，備份任何新增至 HGS 的 PFX 憑證。
如果您使用指紋將憑證新增至 HGS (一般適用于無法匯出和硬體支援的憑證) ，您將需要手動備份憑證的私密金鑰。
若要識別哪些憑證已向 HGS 註冊，且需要以手動方式進行備份，請在任何工作中的 HGS 伺服器節點上執行下列 PowerShell 命令。

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

針對每個列出的憑證，您將需要手動備份私密金鑰。
如果您使用的是不可匯出的軟體型憑證，您應該與您的憑證授權單位單位聯繫，以確保它們具有您的憑證備份和/或可以視需要重新發出。
針對在硬體安全性模組中建立並儲存的憑證，您應該參閱您裝置的檔，以取得嚴重損壞修復計畫的指引。

您應將憑證備份與證明原則備份一起儲存在安全的位置，如此一來，就可以將這兩個片段一起還原。

**要備份的其他設定**

備份的 HGS 伺服器狀態不會包含 HGS 叢集的名稱、來自 Active Directory 的任何資訊，或是用來保護與 HGS Api 通訊的任何 SSL 憑證。
這些設定對一致性很重要，但不是讓 HGS 叢集在嚴重損壞後恢復上線的關鍵。

若要抓取 HGS 服務的名稱，請執行， `Get-HgsServer` 並記下證明和金鑰保護 url 中的一般名稱。
例如，如果證明 URL 是 " <http://hgs.contoso.com/Attestation> "，則 "hgs" 是 hgs 服務名稱。

HGS 所使用 Active Directory 網域的管理方式，就像任何其他 Active Directory 網域一樣。
當您在發生嚴重損壞後還原 HGS 時，不一定需要重新建立目前網域中的確切物件。
但是，如果您備份 Active Directory，並保留授權管理系統的 JEA 使用者清單，以及由系統管理員信任的證明用來授權受保護主機的任何安全性群組的成員資格，則會讓復原更容易。

若要識別為 HGS 設定的 SSL 憑證指紋，請在 PowerShell 中執行下列命令。
然後，您可以根據您的憑證提供者的指示，備份這些 SSL 憑證。

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>從備份還原 HGS
下列步驟說明如何從備份還原 HGS 設定。
這些步驟與您嘗試復原已執行之 HGS 實例的變更，以及當您的先前的 hgs 叢集完全遺失之後，會有全新的 HGS 叢集的情況有關。

#### <a name="set-up-a-replacement-hgs-cluster"></a>設定取代 HGS 叢集
在您可以還原 HGS 之前，您必須擁有已初始化的 HGS 叢集，以便您可以還原設定。
如果您只是將意外刪除的設定匯入到執行) 叢集的現有 (，則可以略過此步驟。
如果您要從 HGS 的完全遺失中復原，則必須依照 [部署指南中的指導](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)方針，至少安裝和初始化一個 hgs 節點。

具體而言，您需要：
1. [設定 hgs 網域或將](guarded-fabric-choose-where-to-install-hgs.md) hgs 加入現有網域
2. 使用您現有的金鑰 *或* 一組暫時金鑰來 [初始化 HGS 伺服器](guarded-fabric-initialize-hgs.md)。 從 HGS 備份檔案匯入實際金鑰之後，您可以 [移除暫時金鑰](#renewing-or-replacing-keys) 。
3. 從備份匯[入 HGS 設定](#import-settings-from-a-backup)，以還原信任的主機群組、程式碼完整性原則、tpm 基準和 tpm 識別碼

> [!TIP]
> 新的 HGS 叢集不需要使用與匯出備份檔案的 HGS 實例相同的憑證、服務名稱或網域。

#### <a name="import-settings-from-a-backup"></a>從備份匯入設定

若要從備份檔案將證明原則、PFX 憑證和非 PFX 憑證的公開金鑰還原至您的 HGS 節點，請在初始化的 HGS 伺服器節點上執行下列命令。
系統會提示您輸入建立備份時所指定的密碼。

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

如果您只想要匯入受管理員信任的證明原則或 TPM 信任的證明原則，您可以指定 `-ImportActiveDirectoryModeState` 或 `-ImportTpmModeState` 旗標以進行 [HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)。

在執行之前，請確定已安裝 Windows Server 2016 的最新累積更新 `Import-HgsServerState` 。
如果無法這麼做，可能會導致匯入錯誤。

> [!NOTE]
> 如果您在已安裝一或多個原則的現有 HGS 節點上還原原則，則匯入命令將會顯示每個重複原則的錯誤。
> 這是預期的行為，而且可以在大部分情況下安全地忽略。

#### <a name="reinstall-private-keys-for-certificates"></a>重新安裝憑證的私密金鑰
如果用來建立備份的 HGS 上所使用的任何憑證是使用指紋新增的，則備份檔案中只會包含這些憑證的公開金鑰。
這表示您必須手動安裝及/或授與這些憑證之私密金鑰的存取權，HGS 才能從 Hyper-v 主機服務要求。
完成該步驟所需的動作會根據您的憑證最初發出的方式而有所不同。
針對憑證授權單位單位所發行的軟體支援憑證，您必須洽詢 CA 以取得私密金鑰，並將其安裝在 **每個** HGS 節點上（依照其指示）。
同樣地，如果您的憑證是硬體支援的，您將需要參閱硬體安全模組廠商的檔，以在每個 HGS 節點上安裝所需的驅動程式 () s，以連線到 HSM，並授與每部電腦存取私密金鑰的許可權。

提醒您，使用指紋新增至 HGS 的憑證需要手動將私密金鑰複寫到每個節點。
您必須在新增至已還原 HGS 叢集的每個額外節點上重複此步驟。

#### <a name="review-imported-attestation-policies"></a>審核匯入的證明原則
從備份匯入設定之後，建議您仔細檢查所有匯入的原則， `Get-HgsAttestationPolicy` 以確保只有您信任的主機執行受防護的 vm 才能成功證明。
如果您發現任何不再符合安全性狀態的原則，您可以停用 [或移除它們](#review-attestation-policies)。

#### <a name="run-diagnostics-to-check-system-state"></a>執行診斷以檢查系統狀態
完成設定並還原 HGS 節點的狀態之後，您應該執行 HGS 診斷工具來檢查系統的狀態。
若要這樣做，請在您還原設定的 HGS 節點上執行下列命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

如果「整體結果」不是「通過」，則需要額外的步驟才能完成系統設定。
檢查 subtest (s 中回報的訊息，) 失敗，以取得詳細資訊。

## <a name="patching-hgs"></a>修補 HGS
請務必在推出最新的累積更新時，保持主機守護者服務節點的最新狀態。如果您要設定全新的 HGS 節點，強烈建議您先安裝任何可用的更新，再安裝 HGS 角色或進行設定。
這可確保任何新的或已變更的功能都會立即生效。

修補受防護網狀架構時，強烈建議您先升級 *所有* hyper-v 主機， **然後再升級 HGS**。
這是為了確保在更新 Hyper-v 主機 *之後* ，對 HGS 上的證明原則所做的任何變更，以提供所需的資訊。
如果更新將會變更原則的行為，將不會自動啟用它們來避免中斷您的網狀架構。
這類更新需要您遵循下一節中的指導方針，以啟用新的或已變更的證明原則。
我們建議您閱讀 Windows Server 的版本資訊，以及您安裝的任何累積更新，以檢查是否需要進行原則更新。

### <a name="updates-requiring-policy-activation"></a>需要啟用原則的更新
如果 HGS 的更新引進或大幅變更證明原則的行為，則需要額外的步驟來啟用變更的原則。
原則變更只會在匯出和匯入 HGS 狀態之後進行。
在您將累計更新套用到您環境中的所有主機和所有 HGS 節點之後，才應該啟用新的或已變更的原則。
更新每一部機器之後，請在任何 HGS 節點上執行下列命令，以觸發升級程式：

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

如果引進了新的原則，預設會停用此原則。
若要啟用新的原則，請先在 Microsoft 原則清單中找出 (首碼為 ' HGS_ ' ) 然後使用下列命令加以啟用：

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>管理證明原則
HGS 會維護數個證明原則，以定義主機必須符合才能被視為「狀況良好」且允許執行受防護 Vm 的最低需求集合。
其中有些原則是由 Microsoft 所定義，有些則是由您加入，以定義您環境中允許的程式碼完整性原則、TPM 基準和主機。
您必須定期維護這些原則，以確保主機在您更新和取代時可繼續證明正常，以及確保封鎖任何未受信任的主機或設定，而無法順利證明。

針對系統管理員信任的證明，只有一個原則可判斷主機是否狀況良好：已知受信任安全性群組中的成員資格。
TPM 證明較為複雜，而且牽涉到各種原則來測量系統的程式碼和設定，然後再判斷它是否狀況良好。

單一 HGS 可以同時設定 Active Directory 和 TPM 原則，但服務只會檢查其在主機嘗試證明時所設定之目前模式的原則。
若要檢查 HGS 伺服器的模式，請執行 `Get-HgsServer` 。

### <a name="default-policies"></a>預設原則
針對受 TPM 信任的證明，在 HGS 上設定了數個內建原則。
其中有些原則是「已鎖定」--表示基於安全性理由無法停用這些原則。
下表說明每個預設原則的用途。

原則名稱                    | 目的
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | 需要主機啟用安全開機。 這是測量啟動二進位檔和其他 UEFI 鎖定設定的必要動作。
Hgs_UefiDebugDisabled          | 確保主機未啟用內核偵錯工具。 使用者模式偵錯工具會封鎖程式碼完整性原則。
Hgs_SecureBootSettings         | 負面原則，確保主機符合至少一個 (管理員定義) TPM 基準。
Hgs_CiPolicy                   | 負面原則，以確保主機使用其中一個系統管理員定義的 CI 原則。
Hgs_HypervisorEnforcedCiPolicy | 需要由虛擬程式強制執行程式碼完整性原則。 停用此原則削弱防護核心模式程式碼完整性原則的攻擊。
Hgs_FullBoot                   | 確保主機未從睡眠或休眠狀態繼續。 主機必須適當地重新開機或關閉，才能通過此原則。
Hgs_VsmIdkPresent              | 需要在主機上執行以虛擬化為基礎的安全性。 IDK 代表將傳回給主機安全記憶體空間的資訊加密所需的金鑰。
Hgs_PageFileEncryptionEnabled  | 需要在主機上加密頁面存放。 如果已針對租使用者密碼檢查未加密的頁面，停用此原則可能會導致資訊暴露。
Hgs_BitLockerEnabled           | 需要在 Hyper-v 主機上啟用 BitLocker。 基於效能考慮，預設會停用此原則，而且不建議啟用此原則。 此原則與受防護 Vm 本身的加密無關。
Hgs_IommuEnabled               | 要求主機必須使用 IOMMU 裝置來防止直接記憶體存取攻擊。 停用此原則，並在未啟用 IOMMU 的情況下使用主機，可以公開租使用者 VM 秘密來直接進行記憶體攻擊。
Hgs_NoHibernation              | Hyper-v 主機上必須停用休眠。 停用此原則可能會允許主機將受防護的 VM 記憶體儲存到未加密的休眠檔案。
Hgs_NoDumps                    | 需要在 Hyper-v 主機上停用記憶體傾印。 如果您停用此原則，建議您設定傾印加密，以防止受防護的 VM 記憶體儲存到未加密的損毀傾印檔案。
Hgs_DumpEncryption             | 如果要在 Hyper-v 主機上啟用記憶體傾印，則需要使用 HGS 信任的加密金鑰進行加密。 如果主機未啟用傾印，則不適用此原則。 如果同時停用此原則和 *Hgs \_ NoDumps* ，受防護的 VM 記憶體可能會儲存到未加密的傾印檔案。
Hgs_DumpEncryptionKey          | 負面原則，以確保設定為允許記憶體傾印的主機使用的系統管理員定義傾印檔案加密金鑰（HGS 已知）。 停用 *Hgs \_ DumpEncryption* 時，不適用此原則。

### <a name="authorizing-new-guarded-hosts"></a>授權新的受防護主機
若要授權新的主機成為受保護的主機 (例如證明已成功) ，HGS 必須信任主機，並且在設定為使用 TPM 信任證明) 在其上執行的軟體時，才會 (。
授權新主機的步驟會根據目前設定 HGS 的證明模式而有所不同。
若要檢查受防護網狀架構的證明模式，請 `Get-HgsServer` 在任何 HGS 節點上執行。

#### <a name="software-configuration"></a>軟體設定
在新的 Hyper-v 主機上，確定已安裝 Windows Server 2016 Datacenter edition。
Windows Server 2016 Standard 無法在受防護網狀架構中執行受防護的 Vm。
主機可能已安裝桌面體驗或 Server Core。

在具有桌面體驗和伺服器核心的伺服器上，您必須安裝 Hyper-v 和主機守護者 Hyper-v 支援伺服器角色：

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>管理-信任的證明
若要在使用系統管理員信任的證明時，在 HGS 登錄新的主機，您必須先將主機新增至其所加入網域中的安全性群組。
一般而言，每個網域都會有一個安全性群組用於受防護主機。
如果您已經向 HGS 註冊該群組，您唯一需要採取的動作是重新開機主機，以重新整理其群組成員資格。

您可以藉由執行下列命令來檢查 HGS 信任哪些安全性群組：

```powershell
Get-HgsAttestationHostGroup
```

若要向 HGS 註冊新的安全性群組，請先在主機網域中) 群組的安全識別碼 (SID，並向 HGS 註冊 SID。

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

部署指南提供如何在主機網域與 HGS 之間設定信任的指示。

#### <a name="tpm-trusted-attestation"></a>TPM-信任的證明
當 HGS 在 TPM 模式中設定時，主機必須傳遞所有已鎖定的原則，以及前置詞為 "Hgs_" 的「已啟用」原則，以及至少一個 TPM 基準、TPM 識別碼和程式碼完整性原則。
每次新增主機時，您都必須向 HGS 註冊新的 TPM 識別碼。
只要主機執行相同的軟體 (，並將相同的程式碼完整性原則套用) 和 TPM 基準作為環境中的另一部主機，您就不需要加入新的 CI 原則或基準。

**為新的主機新增 TPM 識別碼** 在新主機上，執行下列命令以抓取 TPM 識別碼。
請務必指定主機的唯一名稱，協助您在 HGS 上查閱它。
如果您解除委任主機或想要防止它在 HGS 中執行受防護的 Vm，您將需要這項資訊。

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

將此檔案複製到您的 HGS 伺服器，然後執行下列命令來向 HGS 註冊主機。

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**新增 TPM 基準** 如果新的主機正在為您的環境執行新的硬體或固件設定，您可能需要建立新的 TPM 基準。
若要這樣做，請在主機上執行下列命令。

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> 如果您收到錯誤訊息，指出您的主機驗證失敗且無法成功證明，請不要擔心。
> 這是必要條件檢查，可確保您的主機可以執行受防護的 Vm，而且可能表示您尚未套用程式碼完整性原則或其他必要設定。
> 請閱讀錯誤訊息，進行任何建議的變更，然後再試一次。
> 或者，您也可以在命令中新增旗標，以略過驗證 `-SkipValidation` 。

將 TPM 基準複製到您的 HGS 伺服器，然後使用下列命令進行註冊。
建議您使用命名慣例，以協助您瞭解此 Hyper-v 主機類別的硬體和固件設定。

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**加入新的程式碼完整性原則** 如果您已變更在 Hyper-v 主機上執行的程式碼完整性原則，您必須先向 HGS 註冊新的原則，這些主機才能成功證明。
在參照主機上（作為環境中受信任 Hyper-v 電腦的主要映射），使用命令來捕捉新的 CI 原則 `New-CIPolicy` 。
我們鼓勵您針對 Hyper-v 主機 CI 原則使用 **FilePublisher** 層級和 **雜湊** 回復。
您應該先在 audit 模式中建立 CI 原則，以確保一切都能如預期般運作。
驗證系統上的範例工作負載之後，您可以強制執行原則，並將強制的版本複製到 HGS。
如需完整的程式碼完整性原則設定選項清單，請參閱 [Device Guard 檔](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。

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

當您建立、測試和強制執行原則之後，請將二進位檔案 (. p7b) 複製到您的 HGS 伺服器並註冊原則。

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**新增記憶體傾印加密金鑰**

停用 *hgs \_ NoDumps* 原則並啟用 *hgs \_ DumpEncryption* 原則時，受防護主機可以有記憶體傾印 (包括損毀傾印) 只要加密這些傾印，就能啟用損毀傾印。 受防護主機只會在已停用記憶體傾印或使用 HGS 已知的金鑰來加密時，才會通過證明。 根據預設，不會在 HGS 上設定傾印加密金鑰。

若要將傾印加密金鑰新增至 HGS，請使用 `Add-HgsAttestationDumpPolicy` Cmdlet 為 hgs 提供傾印加密金鑰的雜湊。
如果您在以傾印加密設定的 Hyper-v 主機上捕捉 TPM 基準，則雜湊會包含在 tcglog 中，並可提供給 `Add-HgsAttestationDumpPolicy` Cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

或者，您可以直接將雜湊的字串表示提供給 Cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

如果您選擇在您的受防護網狀架構中使用不同的金鑰，請務必將每個唯一的傾印加密金鑰新增至 HGS。
使用 HGS 不知道的金鑰來加密記憶體傾印的主機將不會通過證明。

如需有關 [在主機上](../../virtualization/hyper-v/manage/about-dump-encryption.md)設定傾印加密的詳細資訊，請參閱 hyper-v 檔。

#### <a name="check-if-the-system-passed-attestation"></a>檢查系統是否通過證明
使用 HGS 註冊必要的資訊之後，您應該檢查主機是否通過證明。
在新加入的 Hyper-v 主機上，執行 `Set-HgsClientConfiguration` 並提供 HGS 叢集的正確 url。
您可以藉由 `Get-HgsServer` 在任何 HGS 節點上執行來取得這些 url。

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

如果產生的狀態不是「IsHostGuarded： True」，您就必須針對設定進行疑難排解。
在未通過證明的主機上，執行下列命令以取得可能有助於解決失敗證明的問題詳細報告。

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> 如果您使用的是 Windows Server 2019 或 Windows 10 版本1809並使用程式碼完整性原則， `Get-HgsTrace` 可能會傳回程序 **代碼完整性原則** 使用中診斷的失敗。
> 當這是唯一失敗的診斷時，您可以放心地忽略此結果。

### <a name="review-attestation-policies"></a>審查證明原則
若要檢查 HGS 上設定之原則的目前狀態，請在任何 HGS 節點上執行下列命令：

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

如果您找到已啟用但不再符合安全性需求的原則 (例如，舊的程式碼完整性原則現在被視為不安全的) ，您可以在下列命令中取代原則的名稱來停用它：

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

同樣地，您可以使用 `Enable-HgsAttestationPolicy` 重新啟用原則。

如果您不再需要原則，而且想要從所有 HGS 節點中移除它，請執行 `Remove-HgsAttestationPolicy -Name 'PolicyName'` 以永久刪除原則。

## <a name="changing-attestation-modes"></a>變更證明模式
如果您使用系統管理員信任的證明啟動了受防護網狀架構，您可能會想要在您的環境中有足夠的 TPM 2.0 相容主機時，立即升級至更強的 TPM 證明模式。
當您準備好要切換時，可以預先載入所有的證明成品， (CI 原則、TPM 基準以及 HGS 中的 TPM 識別碼) ，同時繼續以系統管理員信任的證明執行 HGS。
若要這樣做，請直接依照 [授權新的受防護主機](#authorizing-new-guarded-hosts) 一節中的指示進行。

當您將所有原則新增至 HGS 之後，下一步是在您的主機上執行綜合證明嘗試，看看它們是否會以 TPM 模式通過證明。
這不會影響 HGS 目前的操作狀態。
下列命令必須在可存取環境中所有主機的電腦上執行，而且至少要有一個 HGS 節點。
如果您的防火牆或其他安全性原則防止此情況，您可以略過此步驟。
可能的話，建議您執行綜合證明，讓您知道「翻轉」至 TPM 模式是否會導致 Vm 的停機時間。

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

診斷完成之後，請檢查輸出的資訊，以判斷是否有任何主機在 TPM 模式中有失敗的證明。
重新執行診斷，直到您從每部主機取得「通過」，然後繼續將 HGS 變更為 TPM 模式。

**變更為 TPM 模式** 只需要一秒才能完成。
在任何 HGS 節點上執行下列命令，以更新證明模式。

```powershell
Set-HgsServer -TrustTpm
```

如果您遇到問題，而且需要切換回 Active Directory 模式，您可以執行此動作 `Set-HgsServer -TrustActiveDirectory` 。

確認一切都如預期般運作之後，您應該從 HGS 移除所有信任的 Active Directory 主機群組，並移除 HGS 與網狀架構網域之間的信任。
如果您將 Active Directory 信任保持原狀，有人會有風險地重新啟用信任，並將 HGS 切換至 Active Directory 模式，這樣可能會允許未受信任的程式碼在您的受防護主機上執行。

## <a name="key-management"></a>金鑰管理
受防護網狀架構解決方案會使用數個公開/私密金鑰組來驗證解決方案中各種元件的完整性，以及加密租使用者密碼。
主機守護者服務至少設定了兩個憑證 (使用公開和私密金鑰) ，可用來簽署和加密用來啟動受防護 Vm 的金鑰。
您必須小心管理這些金鑰。
如果敵人取得私密金鑰，他們將能夠 unshield 在網狀架構上執行的任何 Vm，或設定使用較弱證明原則的冒名頂替 HGS 叢集，以略過您所放置的保護。
如果您在發生嚴重損壞時遺失私密金鑰，且在備份中找不到私密金鑰，您必須設定一組新的金鑰，並讓每個 VM 重設金鑰，以授權您的新憑證。

本節涵蓋一般的金鑰管理主題，可協助您設定金鑰，使其運作正常且安全。

### <a name="adding-new-keys"></a>加入新的金鑰
雖然 HGS 必須使用一組金鑰來初始化，但您可以新增一個以上的加密和簽署金鑰給 HGS。
將新機碼新增至 HGS 的兩個最常見原因包括：
1. 支援「攜帶您自己的金鑰」案例，其中的租使用者會將其私密金鑰複製到您的硬體安全性模組，並且只授權其金鑰來啟動其受防護的 Vm。
2. 若要取代 HGS 的現有金鑰，請先新增新的金鑰，並保留這兩組金鑰，直到每個 VM 設定都已更新為使用新的金鑰。

新增金鑰的程式會根據您所使用的憑證類型而有所不同。

**選項1：新增儲存在 HSM 中的憑證**

建議用來保護 HGS 金鑰的方法是使用硬體安全模組中建立的憑證 (HSM) 。
Hsm 可確保使用您的金鑰系結至您資料中心內安全性機密裝置的實體存取。
每個 HSM 都是不同的，而且具有建立憑證和向 HGS 註冊憑證的唯一程式。
下列步驟旨在提供使用 HSM 支援之憑證的粗略指引。
如需確切的步驟和功能，請參閱 HSM 廠商的檔。

1. 在叢集中的每個 HGS 節點上安裝 HSM 軟體。 視您是否有網路或本機 HSM 裝置而定，您可能需要設定 HSM 以將您的電腦存取權授與金鑰存放區。
2. 使用 **2048 位 RSA 金鑰** 在 HSM 中建立2個憑證，以進行加密和簽署
    1. 使用您 HSM 中的 **資料加密** 金鑰使用方式屬性建立加密憑證
    2. 在 HSM 中使用 **數位簽章** 金鑰使用方式屬性建立簽署憑證
3. 依據 HSM 廠商的指引，在每個 HGS 節點的本機憑證存放區中安裝憑證。
4. 如果您的 HSM 使用細微許可權來授與特定應用程式或使用者使用私密金鑰的許可權，您必須將憑證的存取權授與 HGS 群組受管理的服務帳戶。 您可以藉由執行，找到 HGS gMSA 帳戶的名稱。 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. 將簽署憑證和加密憑證新增至 HGS，方法是在下列命令中以您的憑證取代指紋：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**選項2：新增不可匯出的軟體憑證**

如果您的公司或公開憑證授權單位單位所發行的軟體支援憑證具有不可匯出的私密金鑰，您必須使用其指紋將憑證新增至 HGS。
1. 根據您的憑證授權單位單位的指示，在您的電腦上安裝憑證。
2. 將憑證私密金鑰的讀取權限授與 HGS 群組受管理的服務帳戶。 您可以藉由執行，找到 HGS gMSA 帳戶的名稱。 `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. 使用下列命令向 HGS 註冊憑證，並以憑證的指紋取代 (變更 *加密* 來 *簽署* 簽署憑證) ：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> 您必須手動安裝私密金鑰，並將讀取權限授與每個 HGS 節點上的 gMSA 帳戶。
> HGS 無法針對憑證指紋所註冊的 *任何* 憑證自動複寫私密金鑰。

**選項3：新增 PFX 檔案中儲存的憑證**

如果您的軟體支援憑證具有可匯出的私密金鑰，而且該金鑰可以用 PFX 檔案格式儲存並以密碼保護，則 HGS 可以自動為您管理您的憑證。
使用 PFX 檔案新增的憑證會自動複寫到 HGS 叢集的每個節點，而 HGS 會保護私密金鑰的存取權。
若要使用 PFX 檔案新增憑證，請在任何 HGS 節點上執行下列命令 (變更 *加密* 以 *簽署* 簽署憑證) ：

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**識別及變更主要憑證** 雖然 HGS 可以支援多個簽署和加密憑證，但它會使用一組作為其「主要」憑證。
如果有人下載該 HGS 叢集的守護者中繼資料，則會使用這些憑證。
若要檢查目前有哪些憑證標示為主要憑證，請執行下列命令：

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

若要設定新的主要加密或簽署憑證，請使用下列命令來尋找所需憑證的指紋，並將其標示為主要：

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>更新或取代金鑰
當您建立 HGS 使用的憑證時，系統會根據您的憑證授權單位單位原則和要求資訊，將到期日指派給憑證。
一般來說，在憑證的有效性很重要的情況下（例如保護 HTTP 通訊的安全），必須在憑證到期之前更新憑證，以避免服務中斷或令人擔憂錯誤訊息。
HGS 不會使用該意義的憑證。
HGS 只是用來建立和儲存非對稱金鑰組的便利方法。
HGS 上過期的加密或簽署憑證不表示受防護 Vm 的弱點或保護遺失。
此外，HGS 不會執行憑證撤銷檢查。
如果已撤銷 HGS 憑證或頒發機構單位的憑證，則不會影響 HGS 的憑證使用。

您必須擔心 HGS 憑證的唯一時機，是因為您有理由相信其私密金鑰已遭竊。
在此情況下，受防護 Vm 的完整性會有風險，因為擁有 HGS 加密和簽署金鑰組的私用部分，足以移除 VM 上的防護保護，或是使用較弱的 HGS 伺服器來移除證明原則。

如果您發現自己在這種情況下，或為了定期重新整理憑證金鑰而需要合規性標準，下列步驟概述在 HGS 伺服器上變更金鑰的程式。
請注意，下列指導方針代表會導致 HGS 叢集所服務的每個 VM 服務中斷的重要工作。
需要適當地規劃變更 HGS 金鑰，以將服務中斷降到最低，並確保租使用者 Vm 的安全性。

在 HGS 節點上，執行下列步驟來註冊一組新的加密和簽署憑證。
請參閱有關新增 [金鑰](#adding-new-keys) 的詳細資訊一節，以瞭解將新機碼新增至 HGS 的各種方式。

1. 為您的 HGS 伺服器建立一組新的加密和簽署憑證。 在理想情況下，這些都是在硬體安全性模組中建立的。

2. 使用 **Add-HgsKeyProtectionCertificate** 註冊新的加密和簽署憑證

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```

3. 如果您使用指紋，則必須移至叢集中的每個節點來安裝私密金鑰，並授與 HGS gMSA 存取金鑰的許可權。

4. 將新憑證設為 HGS 中的預設憑證

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

此時，使用取自 HGS 節點的中繼資料所建立的防護資料將會使用新的憑證，但現有的 Vm 仍會繼續運作，因為較舊的憑證仍然存在。

為了確保所有現有的 Vm 都能使用新的金鑰，您將需要更新每部 VM 上的金鑰保護裝置。

這是一項動作，需要擁有「擁有者」守護者的 VM 擁有者 (人員或實體) 納入。 針對每個受防護的 VM，執行下列步驟：

1. 關閉 VM。 在剩餘的步驟完成之前，無法重新開啟 VM，否則您將需要再次開始處理常式。

2. 將目前的金鑰保護裝置儲存至檔案： `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`

3. 將 KP 傳送給 VM 擁有者

4. 讓擁有者從 HGS 下載更新的守護者資訊，並將其匯入至其本機系統

5. 將目前的 KP 讀入記憶體中、授與新的守護者對 KP 的存取權，並執行下列命令將其儲存至新檔案：

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```

6. 將更新的的 KP 複製回主機網狀架構。

7. 將 KP 套用至原始 VM：

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```

8. 最後，啟動 VM，並確定它已順利執行。

    > [!NOTE]
    > 如果 VM 擁有者在 VM 上設定了不正確的金鑰保護裝置，但未授權您的網狀架構執行 VM，您將無法啟動受防護的 VM。
    > 若要返回最後一個已知的正確金鑰保護裝置，請執行 `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

    更新所有 Vm 以授權新的守護者金鑰之後，您就可以停用和移除舊的金鑰。

9.  取得舊憑證的指紋 `Get-HgsKeyProtectionCertificate -IsPrimary $false`

10. 執行下列命令以停用每個憑證：

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
    ```

11. 確定 Vm 仍能從停用的憑證開始，請執行下列命令來移除 HGS 的憑證：

    ```powershell
    Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
    Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
    ```

> [!IMPORTANT]
> VM 備份會包含舊的金鑰保護裝置資訊，以允許使用舊憑證來啟動 VM。
> 如果您知道私密金鑰已遭入侵，您應該假設 VM 備份也可能遭到入侵，並採取適當的動作。
> 將 VM 設定從備份 ( >.vmcx) 將會移除金鑰保護裝置，代價是需要使用 BitLocker 修復密碼才能在下一次啟動 VM。

### <a name="key-replication-between-nodes"></a>節點之間的金鑰複寫
在設定) SSL 憑證時，必須使用相同的加密、簽署和 (設定 HGS 叢集中的每個節點。
這是為了確保 Hyper-v 主機到達叢集中的任何節點時，可能會成功提供其要求服務。

**如果您使用 PFX 憑證初始化 hgs 伺服器** ，hgs 將會自動在叢集中的每個節點上複寫這些憑證的公開和私密金鑰。
您只需要在一個節點上新增金鑰。

**如果您使用憑證參考或指紋來初始化 hgs 伺服器** ，則 hgs 只會將憑證中的 *公開金鑰* 複寫到每個節點。
此外，HGS 無法在此案例中授與本身存取任何節點上之私密金鑰的許可權。
因此，您必須負責：
1. 在每個 HGS 節點上安裝私密金鑰
2. 授與 HGS 群組受管理的服務帳戶 (gMSA) 在每個節點上存取私密金鑰的許可權，這些工作會增加額外的作業負擔，但 HSM 支援的金鑰和具有不可匯出私密金鑰的憑證需要這些工作。

**SSL 憑證** 絕對不會以任何形式進行複寫。
您必須負責使用相同的 SSL 憑證來初始化每部 HGS 伺服器，並在每次選擇更新或取代 SSL 憑證時更新每部伺服器。
取代 SSL 憑證時，建議您使用 [HgsServer](https://technet.microsoft.com/library/mt652180.aspx) 指令程式來這麼做。

## <a name="unconfiguring-hgs"></a>取消的 HGS

如果您需要解除委任或大幅重新設定 HGS 伺服器，您可以使用 [HgsServer](https://technet.microsoft.com/library/mt652176.aspx) 或 [HgsServer](https://technet.microsoft.com/library/mt652182.aspx) Cmdlet 來進行。

### <a name="clearing-the-hgs-configuration"></a>清除 HGS 設定

若要從 HGS 叢集中移除節點，請使用 [HgsServer](https://technet.microsoft.com/library/mt652176.aspx) Cmdlet。
此 Cmdlet 會在執行的伺服器上進行下列變更：

- 取消註冊證明和金鑰保護服務
- 移除 "JEA 管理端點"
- 從 HGS 容錯移轉叢集中移除本機電腦

如果伺服器是叢集中的最後一個 HGS 節點，則也會終結叢集及其對應的分散式網路名稱資源。

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

清除作業完成之後，就可以使用 [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)重新初始化 HGS 伺服器。
如果您使用 [安裝-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) 來設定 Active Directory Domain Services 網域，則在清除作業之後，該網域仍會保持設定且正常運作。

### <a name="uninstalling-hgs"></a>卸載 HGS

如果您想要移除 HGS 叢集中的節點， **並** 將其上執行的 Active Directory 網網域控制站降級，請使用 [HgsServer](https://technet.microsoft.com/library/mt652182.aspx) Cmdlet。
此 Cmdlet 會在執行的伺服器上進行下列變更：

- 取消註冊證明和金鑰保護服務
- 移除 "JEA 管理端點"
- 從 HGS 容錯移轉叢集中移除本機電腦
- 如果已設定，則會將 Active Directory 網網域控制站降級

如果伺服器是叢集中的最後一個 HGS 節點，則網域、容錯移轉叢集和叢集的分散式網路名稱資源也會終結。

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

卸載作業完成並重新啟動電腦之後，您可以使用 [HgsServer](https://technet.microsoft.com/library/mt652169.aspx) 重新安裝 ADDC 和 HGS，或將電腦加入網域，並使用 [initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)將該網域中的 HGS 伺服器初始化。

如果您不想再使用此電腦做為 HGS 節點，則可以從 Windows 移除角色。

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
