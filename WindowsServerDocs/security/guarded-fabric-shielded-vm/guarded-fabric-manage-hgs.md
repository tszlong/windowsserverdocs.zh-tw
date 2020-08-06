---
title: 管理主機守護者服務
ms.prod: windows-server
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.openlocfilehash: 19bf253a4cd669020442ca80f77c141f19ab94fe
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769456"
---
# <a name="managing-the-host-guardian-service"></a>管理主機守護者服務

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

主機守護者服務 (HGS) 是受防護網狀架構解決方案的成為。
它負責確保主機服務提供者或企業可以得知網狀架構中的 Hyper-v 主機，並執行受信任的軟體，以及管理用來啟動受防護 Vm 的金鑰。
當租使用者決定信任您裝載其受防護的 Vm 時，它們會將其信任放在您的主機守護者服務的設定和管理中。
因此，在管理主機守護者服務時請務必遵循最佳作法，以確保受防護網狀架構的安全性、可用性和可靠性。
下列各節中的指導方針可解決 HGS 系統管理員最常遇到的操作問題。

## <a name="limiting-admin-access-to-hgs"></a>限制對 HGS 的管理員存取
由於 HGS 的安全性敏感性性質，請務必確定其系統管理員是貴組織的高度信任成員，而且在理想情況下，與網狀架構資源的系統管理員分開。
此外，建議您只使用安全通訊協定（如 WinRM over HTTPS）從安全的工作站管理 HGS。

### <a name="separation-of-duties"></a>職責區分
設定 HGS 時，您可以選擇只針對 HGS 建立隔離的 Active Directory 樹系，或將 HGS 加入現有的受信任網域。
這項決定，以及您在組織中指派系統管理員的角色，會決定 HGS 的信任界限。
誰可以存取 HGS，不論是直接身為系統管理員，或間接做為其他專案的系統管理員 (例如可能影響 HGS 的 Active Directory) ，都能控制您的受防護網狀架構。
HGS 系統管理員會選擇哪些 Hyper-v 主機已獲授權可執行受防護的 Vm，並管理啟動受防護 Vm 所需的憑證。
具有 HGS 存取權的攻擊者或惡意系統管理員，可以使用這項功能來授權遭入侵的主機執行受防護的 Vm，藉由移除金鑰內容來起始拒絕服務攻擊。

為避免此風險，*強烈*建議您限制 hgs (的系統管理員之間的重迭，包括 hgs 加入的網域) 和 hyper-v 環境。
藉由確保沒有任何一位系統管理員可以存取這兩個系統，攻擊者就必須從2個人入侵2個不同的帳戶，才能完成他的任務來變更 HGS 原則。
這也表示兩個 Active Directory 環境的網域和企業系統管理員應該不是同一人，而 HGS 則不應該使用與 Hyper-v 主機相同的 Active Directory 樹系。
任何能夠授與自己更多資源存取權的人，都會造成安全性風險。

### <a name="using-just-enough-administration"></a>僅使用足夠的系統管理
HGS 提供足夠的系統[管理](https://aka.ms/JEAdocs) (JEA 內建) 角色，協助您更安全地管理它。
JEA 可協助您將系統管理工作委派給非系統管理員的使用者，這表示管理 HGS 原則的人員實際上不需要是整個電腦或網域的系統管理員。
JEA 的運作方式是限制使用者可以在 PowerShell 會話中執行的命令，並在幕後使用暫時的本機帳戶 (每個使用者會話的唯一) 執行通常需要提高許可權的命令。

HGS 隨附兩個預先設定的 JEA 角色：
- **Hgs 系統管理員**可讓使用者管理所有 HGS 原則，包括授權新主機來執行受防護的 vm。
- **HGS 審核者**，只允許使用者審查現有的原則。 它們無法對 HGS 設定進行任何變更。

若要使用 JEA，您必須先建立新的標準使用者，並將其設為 HGS 管理員或 HGS 審核者群組的成員。
如果您使用 `Install-HgsServer` 設定 hgs 的新樹系，則這些群組會分別命名為「*servicename*系統管理員」和「*servicename*審核者」，其中*servicename*是 HGS 叢集的網路名稱。
如果您將 HGS 加入現有的網域，您應該參考您在中指定的組名 `Initialize-HgsServer` 。

**建立 HGS 系統管理員和審核者角色的標準使用者**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**審核審查者角色的原則**

在具有 HGS 網路連線的遠端電腦上，于 PowerShell 中執行下列命令，以使用審查員認證輸入 JEA 會話。
請務必注意，由於審查者帳戶只是標準使用者，因此無法用於一般的 Windows PowerShell 遠端功能、遠端桌面存取 HGS 等等。

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

接著，您可以使用來檢查會話中允許的命令 `Get-Command` ，並執行任何允許的命令來審查設定。
在下列範例中，我們會檢查 HGS 上啟用的原則。

```powershell
Get-Command

Get-HgsAttestationPolicy
```

`Exit-PSSession` `exit` 當您完成使用 JEA 會話時，請輸入命令或其別名。

**使用系統管理員角色將新原則新增至 HGS**

若要實際變更原則，您需要使用屬於 ' hgsAdministrators ' 群組的身分識別來連接到 JEA 端點。
在下列範例中，我們會示範如何將新的程式碼完整性原則複製到 HGS，並使用 JEA 進行註冊。
語法可能與您習慣的不同。
這是為了配合 JEA 中的某些限制，例如無法存取完整檔案系統。

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
來自 HGS 的事件會顯示在 Windows 事件記錄檔中的 [2 來源] 底下：
- **HostGuardianService-證明**
- **HostGuardianService-KeyProtection**

您可以藉由開啟事件檢視器並流覽至 HostGuardianService-windows-HostGuardianService-KeyProtection，來查看這些事件。

在大型環境中，通常最好是將事件轉寄到中央 Windows 事件收集器，讓事件的分析更容易。
如需詳細資訊，請參閱[Windows 事件轉送檔](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx)。

### <a name="using-system-center-operations-manager"></a>使用 System Center Operations Manager
您也可以使用 System Center 2016-Operations Manager 來監視 HGS 和受防護的主機。
受防護的網狀架構管理元件具有事件監視器，可檢查可能導致資料中心停機的常見錯誤，包括未通過證明的主機，以及報告錯誤的 HGS 伺服器。

若要開始使用，請[安裝並設定 SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) ，並[下載受防護的網狀架構管理元件](https://www.microsoft.com/download/details.aspx?id=52764)。
隨附的管理元件指南說明如何設定管理元件，並瞭解其監視的範圍。

## <a name="backing-up-and-restoring-hgs"></a>備份與還原 HGS
### <a name="disaster-recovery-planning"></a>災害復原規劃
草擬您的嚴重損壞修復計畫時，請務必考慮您的受防護網狀架構中主機守護者服務的獨特需求。
如果您遺失部分或所有 HGS 節點，您可能會面臨立即可用的問題，以防止使用者啟動其受防護的 Vm。
在您遺失整個 HGS 叢集的案例中，您必須準備好 HGS 設定的完整備份，才能還原 HGS 叢集並繼續正常作業。
本節涵蓋準備這類案例所需的步驟。

首先，請務必瞭解 HGS 的重要性，以進行備份。
HGS 會保留數項資訊，協助 it 判斷哪些主機已獲授權可執行受防護的 Vm。
這包括：
1. 使用 Active Directory 證明) 時，Active Directory 包含信任主機 (群組的安全識別碼;
2. 環境中每個主機的唯一 TPM 識別碼;
3. 主機的每個唯一設定的 TPM 原則;和
4. 判斷哪些軟體可以在您的主機上執行的程式碼完整性原則。

這些證明成品需要與主控網狀架構的系統管理員協調，才能取得，這可能會在嚴重損壞之後難以再次取得這項資訊。

此外，HGS 需要存取2個或更多憑證，用來加密和簽署啟動受防護 VM 所需的資訊， (金鑰保護裝置) 。
這些憑證是受防護 Vm 的擁有者所使用的已知 (，可授權您的網狀架構執行其 Vm) 而且必須在發生嚴重損壞的情況時還原，以獲得順暢的復原體驗。
如果您未在嚴重損壞之後使用相同的憑證來還原 HGS，則每個 VM 都必須更新，以授權新的金鑰來解密其資訊。
基於安全性理由，只有 VM 擁有者可以更新 VM 設定以授權這些新的金鑰，這表示在嚴重損壞之後無法還原金鑰，會導致每個 VM 擁有者都需要採取動作，讓其 Vm 再次執行。

#### <a name="preparing-for-the-worst"></a>針對最差的準備
若要準備完全遺失 HGS，您必須採取2個步驟：
1. 備份 HGS 證明原則
2. 備份 HGS 金鑰

[備份 HGS](#backing-up-hgs)一節中會提供如何執行這兩個步驟的指導方針。

此外，建議您備份在其 Active Directory 網域或 Active Directory 本身授權管理 HGS 的使用者清單，但並非必要。

應定期執行備份，以確保資訊是最新的，並安全地儲存，以避免遭到篡改或遭竊。

**不建議**備份或嘗試還原 HGS 節點的整個系統映射。
當您遺失整個叢集時，最佳做法是設定全新的 HGS 節點，並只還原 HGS 狀態，而不是整個伺服器作業系統。

#### <a name="recovering-from-the-loss-of-one-node"></a>從一個節點遺失中復原
如果您遺失一或多個節點 (而不是 HGS 叢集中的每個節點) ，您可以依照部署指南中的指導方針，[將節點新增至您](guarded-fabric-configure-additional-hgs-nodes.md)的叢集中。
證明原則會自動同步，如同提供給 HGS 的任何憑證，以及隨附密碼的 PFX 檔案。
針對使用指紋新增至 HGS 的憑證 (不可匯出和硬體支援的憑證（通常) ），您必須確保每個新節點都能夠存取每個憑證的私密金鑰。

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>從整個叢集遺失中復原
如果您的整個 HGS 叢集中斷，而且您無法讓它重新上線，您將需要從備份還原 HGS。
從備份還原 HGS 牽涉到根據[《部署指南》中的指引](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)來設定新的 hgs 叢集。
在設定復原 HGS 環境以協助從主機進行名稱解析時，強烈建議（但非必要）使用相同的叢集名稱。
使用相同的名稱，可避免必須以新的證明和金鑰保護 Url 來重新設定主機。
如果您將物件還原至支援 HGS 的 Active Directory 網域，建議您先移除代表 HGS 叢集、電腦、服務帳戶和 JEA 群組的物件，再初始化 HGS 伺服器。

一旦設定您的第一個 HGS 節點 (例如，它已安裝並初始化) ，您將遵循[從備份還原 HGS 中](#restoring-hgs-from-a-backup)的程式來還原證明原則和公開金鑰保護憑證的公開部分。
您必須根據憑證提供者的指導方針，手動還原憑證的私密金鑰 (例如在 Windows 中匯入憑證，或設定 HSM 支援憑證) 的存取權。
設定第一個節點之後，您可以繼續在叢集上[安裝其他節點](guarded-fabric-configure-additional-hgs-nodes.md)，直到達到您想要的容量和復原能力為止。

### <a name="backing-up-hgs"></a>備份 HGS
HGS 系統管理員應該負責定期備份 HGS。
完整備份將包含必須適當保護的機密金鑰內容。
如果未受信任的實體取得這些金鑰的存取權，他們就可以使用該資料來設定惡意的 HGS 環境，以危害受防護的 Vm。

**備份證明原則**若要備份 HGS 證明原則，請在任何工作中的 HGS 伺服器節點上執行下列命令。
系統會提示您提供密碼。
此密碼是用來加密使用 PFX 檔案新增至 HGS 的任何憑證，而不是憑證指紋)  (。

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> 如果您使用系統管理員信任的證明，您必須分別備份 HGS 用來授權受防護主機的安全性群組成員資格。
> HGS 只會備份安全性群組的 SID，而不是它們內的成員資格。
> 在事件中，這些群組會在嚴重損壞時遺失，您必須重新建立 () 的群組，然後再將每個受防護的主機重新加入其中。

**備份憑證**

`Export-HgsServerState`命令會在命令執行時，備份新增至 HGS 的任何 PFX 型憑證。
如果您使用指紋將憑證新增至 HGS (一般會針對不可匯出和硬體支援的憑證) ，您必須手動備份憑證的私密金鑰。
若要識別哪些憑證已向 HGS 註冊，而且需要以手動方式進行備份，請在任何工作中的 HGS 伺服器節點上執行下列 PowerShell 命令。

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

針對每個列出的憑證，您將需要手動備份私密金鑰。
如果您使用的是無法匯出的軟體憑證，您應洽詢您的憑證授權單位單位，以確保憑證的備份和/或可以視需要重新發出。
針對已建立並儲存在硬體安全性模組中的憑證，您應該參閱裝置的檔，以取得嚴重損壞修復計畫的指引。

您應該將憑證備份連同證明原則備份一起儲存在安全的位置，如此一來，這兩個部分就可以同時還原。

**要備份的其他設定**

備份的 HGS 伺服器狀態不會包含 HGS 叢集的名稱、Active Directory 的任何資訊，或用來保護與 HGS Api 通訊的任何 SSL 憑證。
這些設定對一致性而言很重要，但在嚴重損壞之後讓 HGS 叢集恢復上線，並不重要。

若要捕捉 HGS 服務的名稱，請執行 `Get-HgsServer` 並記下證明和金鑰保護 url 中的一般名稱。
例如，如果證明 URL 是 " <http://hgs.contoso.com/Attestation> "，則 "hgs" 是 hgs 服務名稱。

HGS 使用的 Active Directory 網域應該與任何其他 Active Directory 網域一樣進行管理。
在嚴重損壞之後還原 HGS 時，您不一定需要重新建立目前網域中的確切物件。
不過，如果您備份 Active Directory 並保留授權管理系統的 JEA 使用者清單，以及系統管理員信任的證明用來授權受防護主機的任何安全性群組的成員資格，則會使復原變得更容易。

若要識別為 HGS 設定的 SSL 憑證指紋，請在 PowerShell 中執行下列命令。
然後，您可以根據您的憑證提供者指示來備份這些 SSL 憑證。

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>從備份還原 HGS
下列步驟說明如何從備份還原 HGS 設定。
這些步驟與您嘗試復原對已執行之 HGS 實例所做的變更，以及在完全遺失先前的 HGS 叢集之後，如果您想要建立新的 HGS 叢集，這兩種情況都是相關的。

#### <a name="set-up-a-replacement-hgs-cluster"></a>設定取代 HGS 叢集
在您可以還原 HGS 之前，您需要有已初始化的 HGS 叢集，才能還原設定。
如果您只是匯入不小心刪除到執行) 叢集之現有 (的設定，可以略過此步驟。
如果您要從 HGS 完全遺失的情況下復原，則必須遵循[部署指南中的指導](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)方針，安裝並初始化至少一個 hgs 節點。

具體而言，您需要：
1. [設定 hgs 網域或將](guarded-fabric-choose-where-to-install-hgs.md)hgs 加入現有的網域
2. 使用現有的金鑰*或*一組暫時金鑰來[初始化 HGS 伺服器](guarded-fabric-initialize-hgs.md)。 從 HGS 備份檔案匯入實際金鑰之後，您可以[移除暫時金鑰](#renewing-or-replacing-keys)。
3. 從您的備份匯[入 HGS 設定](#import-settings-from-a-backup)，以還原受信任的主機群組、程式碼完整性原則、tpm 基準和 tpm 識別碼

> [!TIP]
> 新的 HGS 叢集不需要使用與您的備份檔案匯出所在的 HGS 實例相同的憑證、服務名稱或網域。

#### <a name="import-settings-from-a-backup"></a>從備份匯入設定

若要從備份檔案將證明原則、PFX 憑證和非 PFX 憑證的公開金鑰還原至您的 HGS 節點，請在初始化的 HGS 伺服器節點上執行下列命令。
系統會提示您輸入建立備份時所指定的密碼。

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

如果您只想要匯入系統管理員信任的證明原則或 TPM 信任的證明原則，可以藉由指定 `-ImportActiveDirectoryModeState` HgsServerState 的或 `-ImportTpmModeState` 旗[Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)標來執行此動作。

在執行之前，請確定已安裝 Windows Server 2016 的最新累計更新 `Import-HgsServerState` 。
如果無法這樣做，可能會導致匯入錯誤。

> [!NOTE]
> 如果您在已安裝一或多個這些原則的現有 HGS 節點上還原原則，則匯入命令會顯示每個重複原則的錯誤。
> 這是預期的行為，而且在大部分情況下都可以放心地忽略。

#### <a name="reinstall-private-keys-for-certificates"></a>重新安裝憑證的私密金鑰
如果用來建立備份的 HGS 上所使用的任何憑證是使用指紋加入，則備份檔案中只會包含這些憑證的公開金鑰。
這表示您必須針對每個憑證手動安裝及/或授與私密金鑰的存取權，HGS 才能服務來自 Hyper-v 主機的要求。
完成該步驟所需的動作會根據您的憑證最初發行的方式而有所不同。
針對憑證授權單位單位所發行的軟體支援憑證，您必須聯絡 CA 以取得私密金鑰，並根據其指示將它安裝在**每個**HGS 節點上。
同樣地，如果您的憑證具有硬體支援，您必須查閱硬體安全模組廠商的檔，以在每個 HGS 節點上安裝必要的驅動程式 (s) ，以連線至 HSM 並授與每部電腦對私密金鑰的存取權。

提醒您，使用指紋新增至 HGS 的憑證需要手動將私密金鑰複寫到每個節點。
您必須在新增至已還原之 HGS 叢集的每個額外節點上重複此步驟。

#### <a name="review-imported-attestation-policies"></a>檢查匯入的證明原則
當您從備份匯入設定之後，建議您仔細檢查所有匯入的原則，並使用 `Get-HgsAttestationPolicy` 來確保只有您信任的主機可以成功進行證明。
如果您發現任何不再符合安全性狀態的原則，您可以停用[或移除它們](#review-attestation-policies)。

#### <a name="run-diagnostics-to-check-system-state"></a>執行診斷以檢查系統狀態
在您完成設定和還原 HGS 節點的狀態之後，您應該執行 HGS 診斷工具來檢查系統的狀態。
若要這麼做，請在您還原設定的 HGS 節點上執行下列命令：

```powershell
Get-HgsTrace -RunDiagnostics
```

如果「整體結果」不是「通過」，則需要額外的步驟才能完成系統的設定。
查看 subtest (s) 中回報的訊息失敗，以取得詳細資訊。

## <a name="patching-hgs"></a>修補 HGS
請務必先安裝最新的累積更新，讓主機守護者服務節點保持在最新狀態。如果您要設定全新的 HGS 節點，強烈建議您在安裝 HGS 角色或設定之前，先安裝任何可用的更新。
這可確保任何新的或已變更的功能會立即生效。

修補您的受防護網狀架構時，強烈建議您先升級*所有*hyper-v 主機，**再升級 HGS**。
這是為了確保在 Hyper-v 主機更新*之後*，會對 HGS 上的證明原則進行任何變更，以提供所需的資訊。
如果更新會變更原則的行為，則不會自動啟用，以避免中斷您的網狀架構。
這類更新會要求您遵循下一節中的指導方針，以啟用新的或已變更的證明原則。
我們鼓勵您閱讀 Windows Server 的版本資訊，以及您安裝的任何累積更新，以檢查是否需要原則更新。

### <a name="updates-requiring-policy-activation"></a>需要原則啟用的更新
如果 HGS 的更新引進或大幅變更證明原則的行為，則需要額外的步驟才能啟動已變更的原則。
原則變更只會在匯出和匯入 HGS 狀態之後才會生效。
您應該只在將累計更新套用至環境中的所有主機和所有 HGS 節點之後，才啟動新的或已變更的原則。
每一部電腦都更新之後，請在任何 HGS 節點上執行下列命令，以觸發升級程式：

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

如果引進新的原則，預設將會停用。
若要啟用新的原則，請先在 Microsoft 原則清單中找到 (前面加上 ' HGS_ ' ) ，然後使用下列命令加以啟用：

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>管理證明原則
HGS 會維護數個證明原則，以定義主機必須符合才能被視為「狀況良好」並允許執行受防護 Vm 的最小需求集。
其中有些原則是由 Microsoft 所定義，有些則是由您新增，以定義您環境中可允許的程式碼完整性原則、TPM 基準和主機。
這些原則的定期維護是必要的，以確保主機可以在您更新和取代時繼續正確證明，以及確保任何未受信任的主機或設定遭到封鎖而無法成功證明。

針對系統管理員信任的證明，只有一個原則可判斷主機是否狀況良好：已知、受信任安全性群組中的成員資格。
TPM 證明更為複雜，而且牽涉到各種原則來測量系統的程式碼和設定，再判斷其是否狀況良好。

單一 HGS 可以同時設定 Active Directory 和 TPM 原則，但服務只會檢查目前模式的原則，當主機嘗試證明時，它會針對該狀態進行設定。
若要檢查 HGS 伺服器的模式，請執行 `Get-HgsServer` 。

### <a name="default-policies"></a>預設原則
針對 TPM 信任證明，HGS 上已設定數個內建原則。
其中有些原則是「已鎖定」，表示基於安全考慮，它們無法停用。
下表說明每個預設原則的用途。

原則名稱                    | 目的
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | 需要主機啟用安全開機。 這是測量啟動二進位檔和其他 UEFI 鎖定設定的必要動作。
Hgs_UefiDebugDisabled          | 確保主機未啟用內核偵錯工具。 使用者模式偵錯工具會透過程式碼完整性原則加以封鎖。
Hgs_SecureBootSettings         | 否定原則，確保主機至少符合一個 (系統管理員定義) TPM 基準。
Hgs_CiPolicy                   | 否定原則，以確保主機使用其中一個系統管理員定義的 CI 原則。
Hgs_HypervisorEnforcedCiPolicy | 需要由管理者強制執行程式碼完整性原則。 停用此原則會削弱您對核心模式程式碼完整性原則攻擊的保護。
Hgs_FullBoot                   | 確保主機不會從睡眠或休眠狀態繼續。 主機必須適當地重新開機或關閉，才能通過此原則。
Hgs_VsmIdkPresent              | 需要在主機上執行以虛擬化為基礎的安全性。 IDK 代表加密傳送回主機安全記憶體空間之資訊所需的金鑰。
Hgs_PageFileEncryptionEnabled  | 需要在主機上加密頁面存放。 如果檢查租使用者密碼未加密的頁面，停用此原則可能會導致資訊洩漏。
Hgs_BitLockerEnabled           | 需要在 Hyper-v 主機上啟用 BitLocker。 基於效能考慮，預設會停用此原則，因此不建議啟用。 此原則與受防護 Vm 本身的加密無關。
Hgs_IommuEnabled               | 需要主機具有使用的 IOMMU 裝置，以防止直接記憶體存取攻擊。 停用此原則，並在未啟用 IOMMU 的情況下使用主機，可以公開租使用者 VM 秘密來直接進行記憶體攻擊。
Hgs_NoHibernation              | 需要在 Hyper-v 主機上停用休眠。 停用此原則可能會允許主機將受防護的 VM 記憶體儲存到未加密的休眠檔案。
Hgs_NoDumps                    | 需要在 Hyper-v 主機上停用記憶體傾印。 如果您停用此原則，建議您設定傾印加密，以防止受防護的 VM 記憶體儲存到未加密的損毀傾印檔案。
Hgs_DumpEncryption             | 需要記憶體傾印（如果在 Hyper-v 主機上啟用），以使用 HGS 信任的加密金鑰進行加密。 如果主機上未啟用傾印，則不適用此原則。 如果同時停用此原則和*Hgs \_ NoDumps* ，受防護的 VM 記憶體可能會儲存到未加密的傾印檔案。
Hgs_DumpEncryptionKey          | 否定原則，以確保設定為允許記憶體傾印的主機使用名為 HGS 的系統管理員定義傾印檔案加密金鑰。 停用*Hgs \_ DumpEncryption*時，不適用此原則。

### <a name="authorizing-new-guarded-hosts"></a>授權新的受防護主機
若要授權新主機成為受防護主機 (例如，) 成功證明，HGS 必須信任主機 (，並在設定為使用 TPM 信任證明時，) 在其上執行的軟體。
授權新主機的步驟會根據目前設定 HGS 的證明模式而有所不同。
若要檢查您的受防護網狀架構的證明模式，請 `Get-HgsServer` 在任何 HGS 節點上執行。

#### <a name="software-configuration"></a>軟體設定
在新的 Hyper-v 主機上，確定已安裝 Windows Server 2016 Datacenter edition。
Windows Server 2016 Standard 無法在受保護的網狀架構中執行受防護的 Vm。
主機可能已安裝桌面體驗或 Server Core。

在具有「桌面體驗」和「伺服器核心」的伺服器上，您必須安裝 Hyper-v 和「主機守護者」 Hyper-v 支援伺服器角色：

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>管理員-信任的證明
若要在使用系統管理員信任的證明時，在 HGS 中註冊新主機，您必須先將主機新增至其加入網域中的安全性群組。
一般來說，每個網域都有一個安全性群組用於受防護的主機。
如果您已經向 HGS 註冊該群組，則唯一需要採取的動作是重新開機主機，以重新整理其群組成員資格。

您可以執行下列命令來檢查 HGS 信任哪些安全性群組：

```powershell
Get-HgsAttestationHostGroup
```

若要向 HGS 註冊新的安全性群組，請先在主機的網域中，) 群組 (SID 的安全識別碼，並向 HGS 註冊 SID。

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

《部署指南》提供如何設定主機網域與 HGS 之間信任的指示。

#### <a name="tpm-trusted-attestation"></a>TPM 信任證明
當 HGS 以 TPM 模式設定時，主機必須傳遞所有已鎖定的原則，並在前面加上 "Hgs_" 的「已啟用」原則，以及至少一個 TPM 基準、TPM 識別碼和程式碼完整性原則。
每次您新增新的主機時，都必須向 HGS 註冊新的 TPM 識別碼。
只要主機執行相同的軟體 (並將相同的程式碼完整性原則套用) 和 TPM 基準作為您環境中的另一部主機，您就不需要加入新的 CI 原則或基準。

**為新主機新增 TPM 識別碼**在新的主機上，執行下列命令來捕捉 TPM 識別碼。
請務必指定主控制項的唯一名稱，以協助您在 HGS 上查閱它。
如果您解除委任主機，或想要防止它在 HGS 中執行受防護的 Vm，您將需要這項資訊。

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

將此檔案複製到您的 HGS 伺服器，然後執行下列命令以向 HGS 註冊主機。

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**新增 TPM 基準**如果新主機正在執行您環境的新硬體或固件設定，您可能需要建立新的 TPM 基準。
若要這麼做，請在主機上執行下列命令。

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> 如果您收到錯誤，指出您的主機驗證失敗且無法成功證明，請不要擔心。
> 這是必要條件檢查，可確保您的主機可以執行受防護的 Vm，而且可能表示您尚未套用程式碼完整性原則或其他必要設定。
> 閱讀錯誤訊息，進行任何建議的變更，然後再試一次。
> 或者，您也可以在命令中新增旗標，以略過驗證 `-SkipValidation` 。

將 TPM 基準複製到您的 HGS 伺服器，然後使用下列命令進行註冊。
我們鼓勵您使用命名慣例，以協助您瞭解此 Hyper-v 主機類別的硬體和固件設定。

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**加入新的程式碼完整性原則**如果您已變更在 Hyper-v 主機上執行的程式碼完整性原則，則必須先向 HGS 註冊新的原則，這些主機才能成功進行證明。
在參照主機上，它可作為您環境中受信任 Hyper-v 電腦的主要映射，使用命令來捕捉新的 CI 原則 `New-CIPolicy` 。
我們鼓勵您使用 Hyper-v 主機 CI 原則的**FilePublisher**層級和**雜湊**回退。
您應該先在 audit 模式中建立 CI 原則，以確保所有專案都能如預期般運作。
驗證系統上的範例工作負載之後，您可以強制執行原則，並將強制版本複製到 HGS。
如需程式碼完整性原則設定選項的完整清單，請參閱[Device Guard 檔](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。

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

建立、測試並強制執行原則之後，請將二進位檔案 (. p7b) 複製到 HGS 伺服器，並註冊原則。

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**新增記憶體傾印加密金鑰**

停用*hgs \_ NoDumps*原則並啟用*hgs \_ DumpEncryption*原則時，受防護主機的記憶體傾印 (包括損毀傾印) 在這些傾印加密後才會啟用。 只有當受防護主機已停用記憶體傾印，或使用 HGS 已知的金鑰組其進行加密時，才會通過證明。 根據預設，不會在 HGS 上設定傾印加密金鑰。

若要將傾印加密金鑰新增至 HGS，請使用指令 `Add-HgsAttestationDumpPolicy` 程式，為 hgs 提供傾印加密金鑰的雜湊。
如果您在使用傾印加密設定的 Hyper-v 主機上捕捉 TPM 基準，則雜湊會包含在 tcglog 中，並可提供給 `Add-HgsAttestationDumpPolicy` Cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

或者，您可以將雜湊的字串表示直接提供給 Cmdlet。

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

如果您選擇在您的受防護網狀架構中使用不同的金鑰，請務必將每個唯一的傾印加密金鑰新增至 HGS。
使用不知道 HGS 的金鑰來加密記憶體傾印的主機將不會通過證明。

如需[在主機上](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)設定傾印加密的詳細資訊，請參閱 hyper-v 檔。

#### <a name="check-if-the-system-passed-attestation"></a>檢查系統是否通過證明
向 HGS 註冊所需的資訊之後，您應該檢查主機是否通過證明。
在新增的 Hyper-v 主機上，執行 `Set-HgsClientConfiguration` 並為您的 HGS 叢集提供正確的 url。
這些 Url 可以藉由 `Get-HgsServer` 在任何 HGS 節點上執行來取得。

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

如果產生的狀態未指出「IsHostGuarded： True」，您將需要針對設定進行疑難排解。
在通過證明的主機上，執行下列命令，以取得可協助您解決失敗證明之問題的詳細報告。

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> 如果您使用 Windows Server 2019 或 Windows 10，版本1809並使用程式碼完整性原則， `Get-HgsTrace` 可能會傳回程序**代碼完整性原則**作用中診斷的失敗。
> 當這是唯一失敗的診斷時，您可以放心地忽略此結果。

### <a name="review-attestation-policies"></a>審查證明原則
若要檢查 HGS 上所設定原則的目前狀態，請在任何 HGS 節點上執行下列命令：

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

如果您發現已啟用的原則不再符合您的安全性需求 (例如舊的程式碼完整性原則，但現在被視為不安全的) ，您可以在下列命令中取代原則的名稱來停用它：

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

同樣地，您可以使用 `Enable-HgsAttestationPolicy` 重新啟用原則。

如果您不再需要原則，而且想要將它從所有 HGS 節點移除，請執行 `Remove-HgsAttestationPolicy -Name 'PolicyName'` 來永久刪除原則。

## <a name="changing-attestation-modes"></a>變更證明模式
如果您使用系統管理員信任的證明來啟動受防護的網狀架構，當您的環境中有足夠的 TPM 2.0 相容主機時，您可能會想要升級到更強的 TPM 證明模式。
當您準備切換時，您可以預先載入所有證明成品， (CI 原則、TPM 基準和 HGS 中) 的 TPM 識別碼，同時繼續以系統管理員信任的證明執行 HGS。
若要這樣做，只需遵循[授權新的受防護主機](#authorizing-new-guarded-hosts)一節中的指示。

將所有的原則新增至 HGS 之後，下一步是在您的主機上執行綜合證明嘗試，以查看它們是否會以 TPM 模式通過證明。
這不會影響 HGS 的目前操作狀態。
下列命令必須在可存取環境中所有主機和至少一個 HGS 節點的電腦上執行。
如果您的防火牆或其他安全性原則阻止這種情況，您可以略過此步驟。
可能的話，建議您執行綜合證明，以清楚指出「翻轉」到 TPM 模式是否會導致 Vm 停機。

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

完成診斷之後，請參閱輸出資訊，以判斷是否有任何主機在 TPM 模式中有失敗的證明。
重新執行診斷，直到您從每部主機取得「傳遞」，然後繼續將 HGS 變更為 TPM 模式。

**變更為 TPM 模式**只需要一秒鐘即可完成。
在任何 HGS 節點上執行下列命令，以更新證明模式。

```powershell
Set-HgsServer -TrustTpm
```

如果您遇到問題，而且需要切換回 Active Directory 模式，您可以執行來完成此動作 `Set-HgsServer -TrustActiveDirectory` 。

確認一切都如預期般運作之後，您應該從 HGS 移除所有受信任的 Active Directory 主機群組，並移除 HGS 和網狀架構網域之間的信任。
如果您將 Active Directory 信任保持在原處，則會有其他人重新啟用信任並將 HGS 切換至 Active Directory 模式的風險，這可能會允許未受信任的程式碼在您的受防護主機上執行取消核取。

## <a name="key-management"></a>金鑰管理
受防護的網狀架構解決方案會使用數種公開/私密金鑰組來驗證解決方案中各種元件的完整性，並加密租使用者密碼。
主機守護者服務設定時，至少有兩個憑證 (具有公開和私密金鑰) ，用來簽署和加密用來啟動受防護 Vm 的金鑰。
這些金鑰必須謹慎管理。
如果攻擊者取得私密金鑰，他們將能夠 unshield 在您的網狀架構上執行的任何 Vm，或設定使用較弱證明原則的冒名頂替 HGS 叢集，以略過您所放置的保護。
如果您在發生嚴重損壞時遺失私密金鑰，而在備份中找不到，則必須設定一組新的金鑰，並讓每個 VM 重新註冊，以授權您的新憑證。

本節涵蓋一般金鑰管理主題，協助您設定金鑰，使其運作正常。

### <a name="adding-new-keys"></a>加入新的金鑰
雖然 HGS 必須以一組金鑰進行初始化，但您可以將一個以上的加密和簽署金鑰加入 HGS。
將新金鑰新增至 HGS 的兩個最常見原因如下：
1. 支援「攜帶您自己的金鑰」案例，其中的租使用者會將其私密金鑰複製到您的硬體安全性模組，並只授權其金鑰來啟動其受防護的 Vm。
2. 若要取代 HGS 的現有金鑰，請先新增新的金鑰，並保留這兩組金鑰，直到每個 VM 設定都更新為使用新的金鑰為止。

新增金鑰的程式會根據您所使用的憑證類型而有所不同。

**選項1：新增儲存在 HSM 中的憑證**

我們建議用來保護 HGS 金鑰的方法，就是使用硬體安全模組中建立的憑證 (HSM) 。
Hsm 可確保您的金鑰使用，系結至資料中心內安全性敏感裝置的實體存取。
每個 HSM 都不同，而且具有建立憑證的唯一程式，並向 HGS 註冊。
下列步驟旨在提供使用 HSM 支援憑證的粗略指引。
請參閱 HSM 廠商的檔，以取得確切的步驟和功能。

1. 在叢集中的每個 HGS 節點上安裝 HSM 軟體。 根據您的網路或本機 HSM 裝置而定，您可能需要設定 HSM，以授與電腦存取金鑰存放區的許可權。
2. 使用**2048 位 RSA 金鑰**在 HSM 中建立2個憑證，以進行加密和簽署
    1. 使用 HSM 中的 [**資料加密**金鑰使用方法] 屬性來建立加密憑證
    2. 使用 HSM 中的**數位簽章**金鑰使用方式屬性來建立簽署憑證
3. 根據您的 HSM 廠商指導方針，在每個 HGS 節點的本機憑證存放區中安裝憑證。
4. 如果您的 HSM 使用細微許可權授與特定應用程式或使用者使用私密金鑰的許可權，您必須將憑證的 HGS 群組受管理的服務帳戶授與存取權。 您可以藉由執行來尋找 HGS gMSA 帳戶的名稱`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. 將簽署和加密憑證新增至 HGS，方法是在下列命令中，將指紋取代為您的憑證：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**選項2：新增不可匯出的軟體憑證**

如果您的公司或具有不可匯出私密金鑰的公用憑證授權單位單位所發行之軟體支援的憑證，您將需要使用其指紋將憑證新增至 HGS。
1. 根據您的憑證授權單位單位指示，在您的電腦上安裝憑證。
2. 將憑證私密金鑰的讀取權限授與 HGS 群組受管理的服務帳戶。 您可以藉由執行來尋找 HGS gMSA 帳戶的名稱`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. 使用下列命令向 HGS 註冊憑證，並以憑證的指紋取代 (變更*加密*來*簽署*簽署憑證) ：

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> 您必須手動安裝私密金鑰，並將讀取權限授與每個 HGS 節點上的 gMSA 帳戶。
> HGS 無法自動複寫其指紋所註冊之*任何*憑證的私密金鑰。

**選項3：新增儲存在 PFX 檔案中的憑證**

如果您的軟體支援憑證具有可匯出的私密金鑰，且可以使用 PFX 檔案格式儲存並受到密碼保護，則 HGS 可以自動為您管理您的憑證。
使用 PFX 檔案新增的憑證會自動複寫至 HGS 叢集的每個節點，而 HGS 則會保護私密金鑰的存取權。
若要使用 PFX 檔案新增新憑證，請在任何 HGS 節點上執行下列命令 (變更*加密*以*簽署*簽署憑證) ：

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**識別和變更主要憑證**雖然 HGS 可以支援多個簽署和加密憑證，但它會使用一組做為其「主要」憑證。
這些是當有人下載該 HGS 叢集的守護者中繼資料時，將會使用的憑證。
若要檢查哪些憑證目前已標示為主要憑證，請執行下列命令：

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

若要設定新的主要加密或簽署憑證，請使用下列命令來尋找所需憑證的指紋，並將其標示為主要憑證：

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>更新或取代金鑰
當您建立 HGS 所使用的憑證時，將會根據您的憑證授權單位單位原則和您的要求資訊，將到期日指派給憑證。
一般來說，在憑證的有效性很重要的案例中（例如保護 HTTP 通訊），憑證必須在到期之前更新，以避免服務中斷或令人擔憂錯誤訊息。
HGS 不會使用這種方式的憑證。
HGS 只是使用憑證做為建立和儲存非對稱金鑰組的便利方式。
HGS 上過期的加密或簽署憑證並未指出受防護 Vm 的弱點或遺失保護。
此外，HGS 不會執行憑證撤銷檢查。
如果已撤銷 HGS 憑證或發行授權單位的憑證，就不會影響 HGS 「憑證的使用」。

您需要擔心 HGS 憑證的唯一時機，是因為您有理由相信它的私密金鑰已遭竊。
在此情況下，受防護 Vm 的完整性會有風險，因為擁有 HGS 加密和簽署金鑰組的私用部分，足以移除 VM 上的防護保護，或建立具有較弱證明原則的假 HGS 伺服器。

如果您在這種情況下發現自己，或需要定期重新整理憑證金鑰的合規性標準，下列步驟會概述在 HGS 伺服器上變更金鑰的程式。
請注意，下列指引代表一項重大的工作，這會導致服務中斷至 HGS 叢集所服務的每個 VM。
若要將服務中斷的情況降到最低，並確保租使用者 Vm 的安全性，則需要適當規劃變更 HGS 金鑰。

在 HGS 節點上，執行下列步驟來註冊一組新的加密和簽署憑證。
請參閱新增[金鑰](#adding-new-keys)的一節，以取得詳細資訊將新金鑰加入 HGS 的各種方式。

1. 為您的 HGS 伺服器建立一組新的加密和簽署憑證。 在理想的情況下，這些會在硬體安全性模組中建立。

2. 向**HgsKeyProtectionCertificate**註冊新的加密和簽署憑證

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```

3. 如果您使用指紋，您必須移至叢集中的每個節點來安裝私密金鑰，並授與 HGS gMSA 對金鑰的存取權。

4. 將新憑證設為 HGS 中的預設憑證

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

此時，使用從 HGS 節點取得的中繼資料所建立的防護資料將會使用新的憑證，但是現有的 Vm 將會繼續工作，因為舊的憑證仍然存在。

為了確保所有現有的 Vm 都能使用新的金鑰，您必須更新每個 VM 上的金鑰保護裝置。

這是一項動作，要求 VM 擁有者 (人員或實體必須擁有「擁有者」守護者) 才會參與。 針對每個受防護的 VM，執行下列步驟：

1. 關閉 VM。 在剩餘的步驟完成之前，VM 無法重新開啟，否則您將需要再次啟動程式。

2. 將目前的金鑰保護裝置儲存至檔案：`Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`

3. 將 KP 轉移給 VM 擁有者

4. 讓擁有者下載來自 HGS 的已更新的監護人資訊，並將它匯入其本機系統

5. 將目前的 KP 讀取到記憶體中，並將此 KP 的存取權授與新的監護人，並藉由執行下列命令將它儲存至新檔案：

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```

6. 將更新的 KP 複製回主控網狀架構。

7. 將 KP 套用至原始 VM：

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```

8. 最後，啟動 VM，並確認其執行成功。

    > [!NOTE]
    > 如果 VM 擁有者在 VM 上設定了不正確的金鑰保護裝置，但未授權您的網狀架構執行 VM，您將無法啟動受防護的 VM。
    > 若要回到最後一個已知的正確金鑰保護裝置，請執行`Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

    更新所有 Vm 以授權新的守護者金鑰之後，您就可以停用和移除舊金鑰。

9.  從取得舊憑證的指紋`Get-HgsKeyProtectionCertificate -IsPrimary $false`

10. 執行下列命令來停用每個憑證：

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
    ```

11. 確保 Vm 仍可在停用憑證的情況下啟動之後，請執行下列命令以移除 HGS 中的憑證：

    ```powershell
    Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
    Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
    ```

> [!IMPORTANT]
> VM 備份將包含舊的金鑰保護裝置資訊，可讓舊憑證用來啟動 VM。
> 如果您知道您的私密金鑰已遭盜用，您應該假設 VM 備份也會遭到入侵，並採取適當的動作。
> 從備份 ( 終結 VM 設定。 .vmcx) 將會移除金鑰保護裝置，代價是需要使用 BitLocker 修復密碼來下次啟動 VM。

### <a name="key-replication-between-nodes"></a>節點之間的金鑰複寫
HGS 叢集中的每個節點都必須在設定) SSL 憑證時，使用相同的加密、簽署和 (進行設定。
這是為了確保 Hyper-v 主機連到叢集中的任何節點都可以成功服務其要求。

**如果您使用以 PFX 為基礎的憑證初始化 HGS 伺服器，** 則 hgs 會自動將這些憑證的公開和私密金鑰複寫到叢集中的每個節點。
您只需要在一個節點上加入索引鍵。

**如果您使用憑證參考或指紋初始化 hgs 伺服器**，則 hgs 只會將憑證中的*公開金鑰*複寫至每個節點。
此外，在此案例中，HGS 無法對任何節點上的私密金鑰授與其本身的存取權。
因此，您必須負責：
1. 在每個 HGS 節點上安裝私密金鑰
2. 授與 HGS 群組受管理的服務帳戶 (gMSA) 存取每個節點上的私密金鑰時，這些工作會增加額外的作業負擔，不過，HSM 支援的金鑰和具有不可匯出私密金鑰的憑證則需要它們。

**SSL 憑證**絕對不會以任何形式進行複寫。
您必須負責使用相同的 SSL 憑證來初始化每一部 HGS 伺服器，並在您選擇要更新或取代 SSL 憑證時，更新每部伺服器。
取代 SSL 憑證時，建議您使用[HgsServer](https://technet.microsoft.com/library/mt652180.aspx)指令程式來執行此動作。

## <a name="unconfiguring-hgs"></a>取消已取消的 HGS

如果您需要解除委任或大幅重新設定 HGS 伺服器，您可以使用[HgsServer](https://technet.microsoft.com/library/mt652176.aspx)或[HgsServer 指令程式](https://technet.microsoft.com/library/mt652182.aspx)來執行此動作。

### <a name="clearing-the-hgs-configuration"></a>清除 HGS 設定

若要從 HGS 叢集移除節點，請使用[HgsServer](https://technet.microsoft.com/library/mt652176.aspx) Cmdlet。
此 Cmdlet 會在其執行所在的伺服器上進行下列變更：

- 取消註冊證明和金鑰保護服務
- 移除 "microsoft JEA 管理端點
- 從 HGS 容錯移轉叢集中移除本機電腦

如果伺服器是叢集中的最後一個 HGS 節點，則叢集及其對應的分散式網路名稱資源也會遭到終結。

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

清除作業完成後，就可以使用[HgsServer](https://technet.microsoft.com/library/mt652185.aspx)來重新初始化 HGS 伺服器。
如果您使用[HgsServer](https://technet.microsoft.com/library/mt652169.aspx)來設定 Active Directory Domain Services 網域，該網域會在清除作業之後維持設定和運作。

### <a name="uninstalling-hgs"></a>卸載 HGS

如果您想要從 HGS 叢集移除節點，**並**將其上執行的 Active Directory 網網域控制站降級，請使用[HgsServer](https://technet.microsoft.com/library/mt652182.aspx) Cmdlet。
此 Cmdlet 會在其執行所在的伺服器上進行下列變更：

- 取消註冊證明和金鑰保護服務
- 移除 "microsoft JEA 管理端點
- 從 HGS 容錯移轉叢集中移除本機電腦
- 如果已設定，則會將 Active Directory 網網域控制站降級

如果伺服器是叢集中的最後一個 HGS 節點，則網域、容錯移轉叢集和叢集的分散式網路名稱資源也會遭到終結。

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

完成卸載作業並重新啟動電腦之後，您可以使用[安裝 HgsServer](https://technet.microsoft.com/library/mt652169.aspx)重新安裝 ADDC 和 HGS，或將電腦加入網域，並使用[initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx)將該網域中的 HGS 伺服器初始化。

如果您不想再使用該電腦做為 HGS 節點，可以從 Windows 移除該角色。

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
