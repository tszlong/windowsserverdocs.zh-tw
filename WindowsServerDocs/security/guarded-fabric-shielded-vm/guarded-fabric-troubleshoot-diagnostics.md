---
title: 使用受防護的網狀架構診斷工具進行疑難排解
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: 3cf2b71113e812774cfb39b2ed21df8b41f83f12
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856411"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>使用受防護的網狀架構診斷工具進行疑難排解

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

本主題說明如何使用受防護的網狀架構診斷工具，來找出並修復部署、設定和受防護網狀架構基礎結構的持續性作業中常見的失敗。 這包括主機守護者服務（HGS）、所有受防護的主機和支援的服務，例如 DNS 和 Active Directory。 診斷工具可以用來在分級失敗的受防護網狀架構時執行第一階段，為系統管理員提供解決中斷的起點，並找出設定錯誤的資產。 此工具並不是為了讓您瞭解如何操作受防護的網狀架構，而只是為了快速驗證日常作業期間最常遇到的問題。

本文中所使用 Cmdlet 的完整檔可在[HgsDiagnostics 模組參考](https://docs.microsoft.com/powershell/module/hgsdiagnostics/?view=win10-ps)中找到。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)]

## <a name="quick-start"></a>快速啟動

您可以從具有本機系統管理員許可權的 Windows PowerShell 會話呼叫下列內容，以診斷受防護主機或 HGS 節點：

```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```

這會自動偵測目前主機的角色，並診斷可自動偵測到的任何相關問題。  此程式期間所產生的所有結果都會顯示，因為 `-Detailed` 參數是否存在。

本主題的其餘部分將提供詳細的逐步解說，說明如何使用 `Get-HgsTrace` 的先進方法，來進行一次診斷多部主機，以及偵測複雜的跨節點設定錯誤。

## <a name="diagnostics-overview"></a>診斷總覽

已安裝受防護虛擬機器相關工具和功能的任何主機上都可以使用受保護的網狀架構診斷，包括執行 Server Core 的主機。  目前，診斷包含在下列功能/套件中：

1. 主機守護者服務角色
2. 主機守護者 Hyper-V 支援
3. 適用於網狀架構管理的 VM 防護工具
4. 遠端伺服器管理工具 (RSAT)

這表示在所有受防護主機、HGS 節點、特定網狀架構管理伺服器，以及安裝了[RSAT](https://www.microsoft.com/download/details.aspx?id=45520)的任何 Windows 10 工作站上，都可以使用診斷工具。  診斷可以從上述任何電腦叫用，並以診斷受防護網狀架構中任何受防護主機或 HGS 節點的目的來進行;使用遠端追蹤目標，診斷可以找出並連接到執行診斷的電腦以外的主機。

診斷目標的每個主機稱為「追蹤目標」。  追蹤目標是由其主機名稱和角色所識別。  角色會描述指定的追蹤目標在受防護網狀架構中執行的函式。  目前，追蹤目標支援 `HostGuardianService` 和 `GuardedHost` 角色。  請注意，主機可以同時佔用多個角色，而且診斷也會支援這種情況，但這不應該在生產環境中完成。  HGS 和 Hyper-v 主機隨時都應保持獨立且不同的狀態。

系統管理員可以藉由執行 `Get-HgsTrace`來開始任何診斷工作。  此命令會根據執行時間所提供的參數執行兩個不同的函式：追蹤收集和診斷。  這兩個組合構成了整個受防護網狀架構診斷工具。  雖然不明確需要，但最有用的診斷只需要追蹤目標上的系統管理員認證才能收集的追蹤。  如果執行追蹤集合的使用者擁有不足的許可權，則需要提高許可權的追蹤將會失敗，而其他所有專案則會通過。  這可在具有低許可權的操作員正在執行分級的事件中進行部分診斷。 

### <a name="trace-collection"></a>追蹤集合

根據預設，`Get-HgsTrace` 只會收集追蹤，並將它們儲存至暫存資料夾。  追蹤的格式為在目標主機之後命名的資料夾，並填入特別格式化的檔案，說明如何設定主機。  這些追蹤也包含中繼資料，描述如何叫用診斷來收集追蹤。  此資料會在執行手動診斷時，供診斷用來解除凍結主機的相關資訊。

如有必要，可以手動檢查追蹤。  所有格式都是人們可讀取的（XML），也可以使用標準工具（例如 X509 憑證和 Windows 密碼編譯 Shell 延伸模組）來立即進行檢查。  不過，請注意，追蹤並不是針對手動診斷而設計，而且在使用 `Get-HgsTrace`的診斷功能處理追蹤時，一律會更有效率。

執行追蹤集合的結果不會對指定之主控制項的健全狀況做出任何指示。  它們只是表示已成功收集追蹤。  您必須使用 `Get-HgsTrace` 的診斷工具來判斷追蹤是否表示失敗的環境。

使用 `-Diagnostic` 參數，您可以將追蹤集合限制為只有操作指定之診斷所需的追蹤。  這會減少所收集的資料量，以及叫用診斷所需的許可權。

### <a name="diagnosis"></a>診斷

藉由提供 `Get-HgsTrace` 追蹤的位置，透過 `-Path` 參數並指定 `-RunDiagnostics` 參數，即可診斷收集的追蹤。  此外，`Get-HgsTrace` 可以藉由提供 `-RunDiagnostics` 參數和追蹤目標清單，在單一階段中執行收集和診斷。  如果未提供任何追蹤目標，則會使用目前的電腦作為隱含目標，並藉由檢查已安裝的 Windows PowerShell 模組來推斷其角色。

診斷會以階層格式提供結果，顯示哪些追蹤目標、診斷集和個別診斷會負責特定的失敗。  失敗包括補救和解決建議，以便判斷接下來應採取什麼動作。  根據預設，會隱藏傳遞和不相關的結果。  若要查看診斷所測試的所有專案，請指定 `-Detailed` 參數。  這會導致所有結果顯示，不論其狀態為何。

您可以限制使用 `-Diagnostic` 參數執行的診斷集合。  這可讓您指定應該針對追蹤目標來執行的診斷類別，並隱藏所有其他類別。  可用診斷類別的範例包括網路功能、最佳作法和用戶端硬體。  請參閱[Cmdlet 檔](https://technet.microsoft.com/library/mt718831.aspx)，以尋找最新的可用診斷清單。

> [!WARNING]
> 診斷不會取代強大的監視和事件回應管線。  有一個 System Center Operations Manager 套件可用來監視受防護的網狀架構，以及各種可監視以及早偵測問題的事件記錄檔通道。  然後，可以使用診斷來快速將這些失敗分類，並建立一項動作。

## <a name="targeting-diagnostics"></a>目標診斷

`Get-HgsTrace` 會針對追蹤目標進行操作。  追蹤目標是一個物件，它會對應至受保護網狀架構內的 HGS 節點或受防護主機。  您可以將它視為 `PSSession` 的延伸模組，其中包括診斷所需的資訊，例如在網狀架構中的主機角色。  目標可以隱含地產生（例如本機或手動診斷），或明確地使用 `New-HgsTraceTarget` 命令。

### <a name="local-diagnosis"></a>本機診斷

根據預設，`Get-HgsTrace` 會將目標設為 localhost （也就是叫用 Cmdlet 的位置）。  這就是所謂的隱含本機目標。  只有當 `-Target` 參數中未提供任何目標，而且在 `-Path`中找不到既有的追蹤時，才會使用隱含的本機目標。

隱含本機目標會使用角色推斷來判斷目前主機在受防護網狀架構中所扮演的角色。  這是以已安裝的 Windows PowerShell 模組為基礎，其大致對應于系統上已安裝的功能。  `HgsServer` 模組的存在會使追蹤目標將角色 `HostGuardianService`，而 `HgsClient` 模組的存在會使追蹤目標接受角色 `GuardedHost`。  給定的主機可能會同時有這兩個模組，在這種情況下，它會被視為 `HostGuardianService` 和 `GuardedHost`。

因此，診斷的預設調用會在本機收集追蹤：

```PowerShell
Get-HgsTrace
```

...相當於下列內容：

```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```

> [!TIP]
> `Get-HgsTrace` 可以透過管線或直接透過 `-Target` 參數來接受目標。  這兩個操作性之間沒有任何差異。

### <a name="remote-diagnosis-using-trace-targets"></a>使用追蹤目標進行遠端診斷

藉由使用遠端連線資訊產生追蹤目標，可以從遠端診斷主機。  所有必要的都是主機名稱，以及能夠使用 Windows PowerShell 遠端執行功能連接的一組認證。
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
這個範例會產生收集遠端使用者認證的提示，然後在 `hgs-01.secure.contoso.com` 使用遠端主機來執行診斷，以完成追蹤收集。  產生的追蹤會下載至 localhost，然後進行診斷。  診斷結果的顯示方式與執行[本機診斷](#local-diagnosis)時相同。  同樣地，您也不需要指定角色，因為它可以根據遠端系統上安裝的 Windows PowerShell 模組來推斷。

遠端診斷會針對遠端主機的所有存取，利用 Windows PowerShell 遠端功能。  因此，追蹤目標必須啟用 Windows PowerShell 遠端功能（請參閱[Enable PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)），而且 localhost 已正確設定，以便啟動目標的連接。

> [!NOTE]
> 在大部分情況下，只有 localhost 屬於相同 Active Directory 樹系，且使用有效的 DNS 主機名稱。  如果您的環境使用更複雜的同盟模型，或您想要使用直接 IP 位址進行連線，您可能需要執行其他設定，例如設定 WinRM[信任的主機](https://technet.microsoft.com/library/ff700227.aspx)。

您可以使用 `Test-HgsTraceTarget` Cmdlet，確認追蹤目標已正確具現化並設定為接受連接：
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
只有在 `Get-HgsTrace` 能夠與追蹤目標建立遠端診斷會話時，這個命令才會傳回 `$True`。  失敗時，此 Cmdlet 會傳回相關的狀態資訊，以進一步疑難排解 Windows PowerShell 遠端連線。

#### <a name="implicit-credentials"></a>隱含認證

當您從具有足夠許可權的使用者執行遠端診斷以從遠端連線到追蹤目標時，不需要提供認證來 `New-HgsTraceTarget`。  `Get-HgsTrace` Cmdlet 會在開啟連線時，自動重複使用叫用 Cmdlet 的使用者認證。

> [!WARNING]
> 有些限制適用于重複使用認證，特別是在執行所謂的「第二個躍點」時。  嘗試從遠端會話內部將認證重複使用到另一部電腦時，就會發生這種情況。  您必須[設定 CredSSP](https://technet.microsoft.com/library/hh849872.aspx)以支援此案例，但這不在受防護網狀架構管理和疑難排解的範圍內。

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>使用 Windows PowerShell 的系統管理（JEA）和診斷功能剛好足夠

遠端診斷支援使用 JEA 限制的 Windows PowerShell 端點。 根據預設，遠端追蹤目標會使用預設的 `microsoft.powershell` 端點進行連接。  如果追蹤目標具有 `HostGuardianService` 角色，則也會嘗試使用在安裝 HGS 時所設定的 `microsoft.windows.hgs` 端點。

如果您想要使用自訂端點，您必須在使用 `-PSSessionConfigurationName` 參數來建立追蹤目標時指定會話設定名稱，如下所示：

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>診斷多部主機

您可以一次將多個追蹤目標傳遞給 `Get-HgsTrace`。  這包括混合使用本機和遠端目標。  每個目標會輪流追蹤，然後會同時診斷每個目標的追蹤。  診斷工具可以使用您的部署知識，以識別複雜的跨節點錯誤處理，否則無法偵測。  使用這項功能只需要同時提供多個主機的追蹤（在手動診斷的情況下），或在呼叫 `Get-HgsTrace` 時以多部主機為目標（如果是遠端診斷）。

以下範例會使用遠端診斷來分級由兩個 HGS 節點和兩個受防護主機組成的網狀架構，其中其中一部受防護主機用來啟動 `Get-HgsTrace`。

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> 您不需要在診斷多個節點時診斷整個受防護的網狀架構。  在許多情況下，必須包含可能牽涉到指定失敗狀況的所有節點。  這通常是受防護主機的子集，以及來自 HGS 叢集的一些節點數。

## <a name="manual-diagnosis-using-saved-traces"></a>使用儲存的追蹤進行手動診斷

有時候，您可能會想要重新執行診斷，而不要再次收集追蹤，或者您可能沒有必要的認證可以同時從遠端診斷網狀架構中的所有主機。  手動診斷是一種機制，您仍然可以使用 `Get-HgsTrace`，但不使用遠端追蹤收集來執行整個網狀架構分級。

在執行手動診斷之前，您必須確定網狀架構中每個將分級的主機的系統管理員已準備就緒，並願意代表您執行命令。  診斷追蹤輸出不會公開任何一般視為機密的資訊，不過，它是由使用者負責判斷是否可以安全地向其他人公開此資訊。

> [!NOTE]
> 追蹤不會匿名及顯示網路設定、PKI 設定，以及有時視為私人資訊的其他設定。  因此，追蹤應該只會傳送到組織內受信任的實體，且永遠不會公開張貼。

執行手動診斷的步驟如下：

1. 要求每部主機管理員執行 `Get-HgsTrace` 指定已知的 `-Path`，以及您想要針對產生的追蹤執行的診斷清單。  例如，

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```

2. 要求每部主機管理員封裝產生的追蹤資料夾，並將它傳送給您。  此程式可透過電子郵件、檔案共用或任何其他機制（根據貴組織所建立的操作原則和程式）來驅動。

3. 將所有接收的追蹤合併至單一資料夾，而不含其他內容或資料夾。

    * 例如，假設您有系統管理員將您從名為 HGS-01、HGS-02、RR1N2608-12 和 RR1N2608-13 的四部電腦所收集的追蹤傳送給您。  每位系統管理員都已將相同名稱的資料夾傳送給您。  您將會組合如下所示的目錄結構：

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

4. 執行診斷，並在 `-Path` 參數上提供組合追蹤資料夾的路徑，並指定 `-RunDiagnostics` 參數，以及您要求系統管理員收集追蹤的診斷。  診斷會假設它無法存取在路徑內找到的主機，因此會嘗試僅使用預先收集的追蹤。  如果有任何追蹤遺失或損毀，診斷將只會使受影響的測試失敗，並正常進行。  例如，

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>混合具有其他目標的已儲存追蹤

在某些情況下，您可能會有一組預先收集的追蹤，您想要使用額外的主機追蹤來增強。  您可以混合預先收集的追蹤與其他目標，以便在診斷的單一呼叫中進行追蹤和診斷。

遵循指示收集並組合上述指定的追蹤資料夾，使用在預先收集的追蹤資料夾中找不到的其他追蹤目標來呼叫 `Get-HgsTrace`：

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

診斷 Cmdlet 會識別所有預先收集的主機，以及另一個仍然需要追蹤並執行必要追蹤的主機。  接著會診斷所有預先收集和最近收集的追蹤總和。  產生的追蹤資料夾將同時包含舊的和新的追蹤。

## <a name="known-issues"></a>已知問題

在 Windows Server 2019 或 Windows 10 1809 版和更新版本的作業系統上執行時，受防護的網狀架構診斷模組具有已知的限制。
使用下列功能可能會導致錯誤的結果：

* 主機金鑰證明
* 僅限證明 HGS 設定（適用于 SQL Server Always Encrypted 案例）
* 在證明原則預設為 v2 的 HGS 伺服器上使用 v1 原則構件

使用這些功能時 `Get-HgsTrace` 失敗，不一定表示 HGS 伺服器或受防護主機的設定不正確。
在受防護主機上使用其他診斷工具（例如 `Get-HgsClientConfiguration`），以測試主機是否已通過證明。
