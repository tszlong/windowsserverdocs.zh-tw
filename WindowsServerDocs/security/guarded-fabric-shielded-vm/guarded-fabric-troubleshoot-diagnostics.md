---
title: 使用受防護網狀架構診斷工具進行疑難排解
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 01/14/2020
ms.openlocfilehash: 5aa8b0f93a98e9cbf142476e6b05056d9a47eefd
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864807"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>使用受防護網狀架構診斷工具進行疑難排解

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本主題說明如何使用「受防護網狀架構診斷」工具來識別及修復部署、設定和受防護網狀架構基礎結構進行中的一般失敗。 這包括主機守護者服務 (HGS) 、所有受防護主機和支援的服務，例如 DNS 和 Active Directory。 診斷工具可用來在對失敗的受防護網狀架構進行分級時執行第一階段，為系統管理員提供解決中斷的起點，並識別設定不正確的資產。 此工具並不是為了讓音效無法操作受防護網狀架構，而只是為了快速確認日常作業期間最常遇到的問題。

您可以在 [HgsDiagnostics 模組參考](/powershell/module/hgsdiagnostics/)中找到本文中所使用之 Cmdlet 的完整檔。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)]

## <a name="quick-start"></a>快速入門

您可以從具有本機系統管理員許可權的 Windows PowerShell 會話呼叫下列內容，以診斷受防護的主機或 HGS 節點：

```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```

這會自動偵測目前主機的角色，並診斷任何可以自動偵測到的相關問題。  此程式期間所產生的所有結果都會因為參數是否存在而顯示 `-Detailed` 。

本主題的其餘部分將提供詳細的逐步解說，說明如何 `Get-HgsTrace` 進行一次診斷多部主機，以及偵測複雜的跨節點錯誤設定等工作。

## <a name="diagnostics-overview"></a>診斷總覽

已安裝受防護虛擬機器相關工具和功能的任何主機（包括執行 Server Core 的主機）都可以使用受防護網狀架構診斷。  目前，診斷功能包含下列功能/套件：

1. 主機守護者服務角色
2. 主機守護者 Hyper-V 支援
3. 適用於網狀架構管理的 VM 防護工具
4. 遠端伺服器管理工具 (RSAT)

這表示可以在所有受防護的主機、HGS 節點、某些網狀架構管理伺服器，以及任何已安裝 [RSAT](https://www.microsoft.com/download/details.aspx?id=45520) 的 Windows 10 工作站上，都可以使用診斷工具。  診斷可以從上述任何一部機器叫用，其目的是要診斷受防護網狀架構中的任何受防護主機或 HGS 節點;使用遠端追蹤目標，診斷可以找出並連接到執行診斷的電腦以外的主機。

診斷設為目標的每個主機稱為「追蹤目標」。  追蹤目標是由其主機名稱和角色來識別。  角色會描述指定的追蹤目標在受防護網狀架構中執行的函數。  目前，追蹤目標支援 `HostGuardianService` 和 `GuardedHost` 角色。  請注意，主機可能會一次佔用多個角色，且診斷也支援此功能，但這不應該在實際執行環境中完成。  HGS 和 Hyper-v 主機應該隨時保持獨立和相異。

系統管理員可以藉由執行來開始任何診斷工作 `Get-HgsTrace` 。  此命令會根據執行時間所提供的參數來執行兩個不同的函式：追蹤收集和診斷。  這兩個組合構成整個受防護網狀架構診斷工具。  雖然並非明確需要，但最有用的診斷只需要追蹤目標上的系統管理員認證即可收集的追蹤。  如果執行追蹤集合的使用者沒有足夠的許可權，則需要提高許可權的追蹤將會失敗，而其他則會通過。  這可在具有低許可權的運算子正在執行分級的事件中進行部分診斷。

### <a name="trace-collection"></a>追蹤集合

依預設， `Get-HgsTrace` 只會收集追蹤並將其儲存至暫存資料夾。  追蹤的格式是以目標主機命名的資料夾，並以描述如何設定主機的特殊格式化檔案來填滿。  追蹤也包含中繼資料，描述如何叫用診斷以收集追蹤。  診斷會使用此資料來解除凍結執行手動診斷時的主機相關資訊。

如有必要，可以手動審核追蹤。  所有格式都是人類看得懂的 (XML) 或可使用標準 (工具（例如 X509 憑證和 Windows 密碼編譯 Shell 延伸模組) ）來立即進行檢查。  不過請注意，追蹤並非針對手動診斷所設計，而且在使用的診斷功能處理追蹤時，一律會更有效率 `Get-HgsTrace` 。

執行追蹤集合的結果並不會對指定主機的健全狀況做出任何指示。  它們只是表示已成功收集追蹤。  您必須使用的診斷功能 `Get-HgsTrace` 來判斷追蹤是否指出失敗的環境。

`-Diagnostic`您可以使用參數，將追蹤集合限制在操作指定診斷所需的追蹤。  這會減少所收集的資料量，以及叫用診斷所需的許可權。

### <a name="diagnosis"></a>診斷

您可以透過參數提供追蹤的 `Get-HgsTrace` 位置並指定參數，來診斷收集的追蹤 `-Path` `-RunDiagnostics` 。  此外， `Get-HgsTrace` 也可以藉由提供 `-RunDiagnostics` 參數和追蹤目標清單，在單一階段中執行收集和診斷。  如果未提供任何追蹤目標，則會使用目前的機器作為隱含目標，並藉由檢查已安裝的 Windows PowerShell 模組來推斷其角色。

診斷會以階層格式提供結果，顯示哪些追蹤目標、診斷集和個別診斷負責特定的失敗。  如果您可以針對接下來應該採取的動作進行判斷，則失敗會包含補救和解決建議。  依預設，會隱藏傳遞和不相關的結果。  若要查看診斷所測試的所有專案，請指定 `-Detailed` 參數。  這會使所有結果顯示，不論其狀態為何。

您可以限制使用參數執行的診斷集 `-Diagnostic` 。  這可讓您指定應該針對追蹤目標執行哪些診斷類別，並隱藏所有其他類別。  可用診斷類別的範例包括網路功能、最佳作法和用戶端硬體。  請參閱 [Cmdlet 檔](https://technet.microsoft.com/library/mt718831.aspx) ，以尋找可用診斷的最新清單。

> [!WARNING]
> 診斷不會取代強式監視和事件回應管線。  有 System Center Operations Manager 套件可用來監視受防護的網狀架構，以及各種可監視以及早偵測問題的事件記錄通道。  然後，您可以使用診斷來快速分級這些失敗，並建立一堂動作。

## <a name="targeting-diagnostics"></a>鎖定目標診斷

`Get-HgsTrace` 針對追蹤目標運作。  追蹤目標是對應至 HGS 節點或受防護網狀架構中受防護主機的物件。  您可以將它視為的延伸模組， `PSSession` 其中包含了診斷所需的資訊，例如網狀架構中主機的角色。  您可以隱含地產生目標 (例如本機或手動診斷) 或使用命令明確地產生。 `New-HgsTraceTarget`

### <a name="local-diagnosis"></a>本機診斷

根據預設， `Get-HgsTrace` 將以 localhost 為目標 (也就是) 叫用 Cmdlet 的位置。  這稱為隱含本機目標。  只有在參數中未提供任何目標 `-Target` ，且在中找不到預先存在的追蹤時，才會使用隱含的本機目標 `-Path` 。

隱含的本機目標會使用角色推斷，判斷目前主機在受防護網狀架構中扮演的角色。  這是以已安裝的 Windows PowerShell 模組為基礎，這些模組大致上與系統上已安裝的功能有關。  模組的存在 `HgsServer` 會導致追蹤目標採用角色 `HostGuardianService` ，而模組的存在會 `HgsClient` 導致追蹤目標採用該角色 `GuardedHost` 。  給定的主機可能會有這兩個模組，在這種情況下，會將它視為 `HostGuardianService` 和 `GuardedHost` 。

因此，在本機收集追蹤的診斷預設調用：

```PowerShell
Get-HgsTrace
```

...相當於下列專案：

```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```

> [!TIP]
> `Get-HgsTrace` 可以透過管線或直接透過參數接受目標 `-Target` 。  這兩個操作性之間沒有任何差異。

### <a name="remote-diagnosis-using-trace-targets"></a>使用追蹤目標進行遠端診斷

您可以藉由使用遠端連線資訊來產生追蹤目標，以從遠端診斷主機。  所有必要的都是主機名稱，以及一組能夠使用 Windows PowerShell 遠端連線來連接的認證。
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
此範例會產生收集遠端使用者認證的提示，然後使用中的遠端主機來執行診斷， `hgs-01.secure.contoso.com` 以完成追蹤收集。  產生的追蹤會下載到 localhost，然後診斷。  診斷結果的呈現方式與執行 [本機診斷](#local-diagnosis)時相同。  同樣地，您不需要指定角色，因為它可以根據遠端系統上安裝的 Windows PowerShell 模組加以推斷。

遠端診斷利用遠端主機的所有存取 Windows PowerShell 遠端處理。  因此，追蹤目標必須啟用 Windows PowerShell 遠端功能 (請參閱 [啟用 >enable-psremoting](/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7)) ，以及是否已正確設定 localhost 來啟動目標的連接。

> [!NOTE]
> 在大部分的情況下，只有 localhost 必須是相同 Active Directory 樹系的一部分，而且使用的是有效的 DNS 主機名稱。  如果您的環境使用更複雜的同盟模型，或您想要使用直接 IP 位址來進行連線，您可能需要執行其他設定，例如設定 WinRM [受信任的主機](/previous-versions/technet-magazine/ff700227(v=msdn.10))。

您可以使用 Cmdlet，確認追蹤目標是否已正確具現化並設定為接受連接 `Test-HgsTraceTarget` ：
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
`$True`只有在 `Get-HgsTrace` 能夠與追蹤目標建立遠端診斷會話時，此命令才會傳回。  失敗時，此 Cmdlet 會傳回相關的狀態資訊，以進一步疑難排解 Windows PowerShell 遠端連線。

#### <a name="implicit-credentials"></a>隱含認證

從具有足夠許可權的使用者執行遠端診斷以從遠端連線到追蹤目標時，不需要提供認證給 `New-HgsTraceTarget` 。  開啟連線時，此 `Get-HgsTrace` Cmdlet 會自動重複使用叫用 Cmdlet 的使用者認證。

> [!WARNING]
> 某些限制適用于重複使用認證，特別是在執行所謂的「第二個躍點」。  當您嘗試在遠端會話內部重複使用認證至另一部電腦時，就會發生這種情況。  您必須 [設定 CredSSP](/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7) 來支援此案例，但這不在受防護網狀架構管理與疑難排解的範圍之外。

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>使用 Windows PowerShell 足夠的系統管理 (JEA) 和診斷

遠端診斷支援使用 JEA 受限的 Windows PowerShell 端點。 根據預設，遠端追蹤目標會使用預設端點進行連接 `microsoft.powershell` 。  如果追蹤目標具有 `HostGuardianService` 角色，則也會嘗試使用 `microsoft.windows.hgs` 安裝 HGS 時所設定的端點。

如果您想要使用自訂端點，您必須在使用參數來建立追蹤目標時，指定會話設定名稱，如下所示 `-PSSessionConfigurationName` ：

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>診斷多部主機

您可以一次將多個追蹤目標傳遞給 `Get-HgsTrace` 。  這包括本機和遠端目標的混合。  每個目標會輪流追蹤，然後將會同時診斷每個目標的追蹤。  診斷工具可以使用更多的部署知識，找出不會偵測到的複雜跨節點錯誤。  使用這項功能只需要在手動診斷) 的情況下，同時提供多部主機的追蹤 (，或在 `Get-HgsTrace` 遠端診斷) 的情況下，以多部 (主機為目標來提供追蹤。

以下範例會示範如何使用遠端診斷，將由兩個 HGS 節點和兩個受防護主機所組成的網狀架構分級，其中會使用其中一個受防護主機來啟動 `Get-HgsTrace` 。

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> 診斷多個節點時，您不需要診斷整個受防護網狀架構。  在許多情況下，您就足以包含可能牽涉到特定失敗狀況的所有節點。  這通常是受防護主機的子集，以及來自 HGS 叢集的一些節點數目。

## <a name="manual-diagnosis-using-saved-traces"></a>使用已儲存的追蹤進行手動診斷

有時您可能會想要重新執行診斷，而不需要再次收集追蹤，或者您可能沒有必要的認證可同時在網狀架構中遠端診斷所有主機。  手動診斷是一種機制，您仍然可以使用來執行整個網狀架構分級 `Get-HgsTrace` ，但不使用遠端追蹤集合。

執行手動診斷之前，您必須確定網狀架構中要分級的每個主機的系統管理員都已就緒，而且願意代表您執行命令。  診斷追蹤輸出不會公開任何通常視為機密資訊的資訊，但在使用者上，有可能會決定是否可以安全地向其他人公開這項資訊。

> [!NOTE]
> 追蹤不會匿名，而且會顯示網路設定、PKI 設定，以及其他有時候視為私用資訊的設定。  因此，您應該只將追蹤傳送至組織內的受信任實體，而且永遠不會公開公佈。

執行手動診斷的步驟如下：

1. 要求每個主機系統管理員執行 `Get-HgsTrace` 指定已知的 `-Path` 診斷清單，以及您想要針對產生的追蹤執行的診斷清單。  例如：

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```

2. 要求每個主機系統管理員封裝產生的追蹤資料夾，並將它傳送給您。  此程式可透過電子郵件、檔案共用，或任何其他機制（以您組織所建立的作業原則和程式為基礎）來驅動。

3. 將所有收到的追蹤合併成單一資料夾，不含其他內容或資料夾。

    * 例如，假設您的系統管理員將從四部機器（名為 HGS-01、HGS-02、RR1N2608-12 及 RR1N2608-13）收集到的追蹤傳送給您。  每位系統管理員都會傳送相同名稱的資料夾給您。  您會組合如下所示的目錄結構：

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. 執行診斷，提供參數上所組合追蹤資料夾的路徑， `-Path` 並指定參數，以及 `-RunDiagnostics` 您要求系統管理員收集追蹤的診斷資訊。  診斷會假設它無法存取路徑內找到的主機，因此只會嘗試使用預先收集的追蹤。  如果有任何追蹤遺失或損毀，診斷將只會使受影響的測試失敗，並正常進行。  例如：

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>將儲存的追蹤與其他目標混合

在某些情況下，您可能會有一組預先收集的追蹤，您想要使用額外的主機追蹤來增強這些追蹤。  您可以將預先收集的追蹤與其他目標混合在一起，以在診斷的單一呼叫中進行追蹤和診斷。

依照下列指示收集和組合上面指定的追蹤資料夾， `Get-HgsTrace` 使用在預先收集的追蹤資料夾中找不到的其他追蹤目標來呼叫：

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
```

診斷 Cmdlet 將會識別所有預先收集的主機，以及另一部仍需要追蹤的主機，並且將會執行必要的追蹤。  接著會診斷所有預先收集和最近收集之追蹤的總和。  產生的追蹤資料夾將會包含舊的和新的追蹤。

## <a name="known-issues"></a>已知問題

當您在 Windows Server 2019 或 Windows 10 版本1809與較新的作業系統版本上執行時，受防護網狀架構診斷模組有已知的限制。
使用下列功能可能會導致錯誤的結果：

* 主機金鑰證明
* 適用于 SQL Server Always Encrypted 案例的僅限證明 HGS 設定 () 
* 在 HGS 伺服器上使用 v1 原則構件，證明原則預設值為 v2

`Get-HgsTrace`使用這些功能時的失敗不一定表示 HGS 伺服器或受保護主機的設定不正確。
使用其他診斷工具（例如 `Get-HgsClientConfiguration` 在受防護主機上）來測試主機是否已通過證明。
