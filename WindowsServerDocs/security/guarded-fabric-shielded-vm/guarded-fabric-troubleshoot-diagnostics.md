---
title: 使用受防護網狀架構的診斷工具進行疑難排解
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: c102fa0503e6aac279235e1243b55e0e3cf81e1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812409"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>使用受防護網狀架構的診斷工具進行疑難排解

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

本主題描述使用受防護網狀架構診斷工具來找出並修復部署、 組態和受防護網狀架構基礎結構的進行中的作業中常見的失敗。 這包括主機守護者服務 (HGS)、 所有的受防護主機和支援的服務，例如 DNS 與 Active Directory。 診斷工具可用來執行第一個階段在分級失敗受防護網狀架構系統管理員提供的起點，來解決中斷和識別設定不正確的資產。 此工具來取代音效掌握營運受防護網狀架構並不只是用來快速確認在日常作業期間最常見的問題。

本主題中使用 cmdlet 的文件都位於[TechNet](https://technet.microsoft.com/library/mt718834.aspx)。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>快速入門

您可以呼叫下列命令從本機系統管理員權限的 Windows PowerShell 工作階段來診斷受守護的主機或 HGS 節點：
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
這會自動偵測目前的主控件的角色，並診斷可以自動偵測到的任何相關問題。  所有在此程序期間所產生的結果會顯示的緣故`-Detailed`切換。

本主題的其餘部分將提供的進階用法的詳細逐步解說`Get-HgsTrace`可執行一次診斷多部主機，以及偵測複雜的跨節點設定不正確。

## <a name="diagnostics-overview"></a>診斷概觀
與受防護的虛擬機器的任何主機上的受防護網狀架構診斷有相關工具和功能安裝，包括執行 Server Core。  目前，診斷隨附下列功能/封裝：

1. 主機守護者服務角色
2. 主機守護者 Hyper-V 支援
3. 適用於網狀架構管理的 VM 防護工具
4. 遠端伺服器管理工具 (RSAT)

這表示診斷工具會提供所有的受防護的主機、 HGS 節點、 特定的網狀架構管理伺服器和使用任何 Windows 10 工作站[RSAT](https://www.microsoft.com/download/details.aspx?id=45520)安裝。  可以從任何上述機器的用意是診斷受防護的主機中任何或 HGS 節點受防護的網狀架構中，叫用診斷使用遠端追蹤目標，診斷可以找出並連線到執行診斷的電腦以外的主機。

每個診斷為目標的主機被指 「 追蹤目標 」。  追蹤目標識別其主機名稱和角色。  角色描述指定之追蹤的目標執行受防護網狀架構中的函式。  追蹤目前目標支援`HostGuardianService`和`GuardedHost`角色。  請注意您可以一次佔用多個角色的主機，不過這不應該在生產環境中，這也會診斷，支援。  HGS 與 HYPER-V 主機應該有所區隔不同隨時和。

系統管理員可以開始執行的任何診斷工作`Get-HgsTrace`。  此命令會執行兩個不同的函式，根據在執行階段提供的參數： 追蹤集合和診斷。  結合這兩個構成 「 受防護網狀架構診斷工具的全部內容。  雖然不明確要求，最有用的診斷會要求，才可收集與系統管理員認證追蹤目標的追蹤。  如果沒有足夠的權限已由執行追蹤集合的使用者保留，追蹤需要提高權限將會失敗，而所有其他人將會通過。  這可讓部分診斷事件的權限不足運算子執行分級。 

### <a name="trace-collection"></a>追蹤集合
根據預設，`Get-HgsTrace`將只收集追蹤，並將它們儲存到暫存資料夾。  追蹤的目標主機上，填入描述如何設定主機的特殊格式檔案命名的資料夾形式。  追蹤也會包含中繼資料，說明如何診斷已叫用來收集追蹤。  診斷會使用此資料，以執行手動診斷時，解除凍結主機的相關資訊。

如有必要，您可以手動檢閱追蹤。  所有的格式可能是人類看得懂的 (XML) 或可能會輕易地進行檢查使用標準工具 (例如 X509 憑證和 Windows 密碼編譯 Shell 擴充功能)。  注意追蹤並未設計成手動診斷，且總是會比較有效的診斷功能和處理追蹤`Get-HgsTrace`。

執行追蹤集合的結果，請勿進行任何指示，指出健全狀況的某個主控件。  它們只是表示已成功地收集追蹤。  它會使用的診斷功能所需`Get-HgsTrace`來判斷是否追蹤會指出失敗的環境。

使用`-Diagnostic`參數，您可以限制只操作指定的診斷所需的這些追蹤的追蹤收集。  這會減少叫用診斷所需的權限以及所收集的資料量。

### <a name="diagnosis"></a>診斷
您可以藉由診斷收集的追蹤，提供`Get-HgsTrace`透過追蹤的位置`-Path`參數並指定`-RunDiagnostics`切換。  此外，`Get-HgsTrace`可以執行集合和診斷在單一行程中藉由提供`-RunDiagnostics`參數和追蹤的目標清單。  如果未不提供任何追蹤目標時，目前的電腦搭配為隱含的目標，其藉由檢查已安裝的 Windows PowerShell 模組推斷而來的角色。

診斷會提供顯示哪一個追蹤目標、 診斷設定，以階層式格式的結果，而且個別的診斷負責特定的失敗。  如果可以進行判斷，並採取什麼動作應該是下一步，失敗會包括補救和建議的解決方法。  根據預設，會隱藏傳遞並不相關的結果。  若要查看診斷所測試的所有項目，指定`-Detailed`切換。  這會導致不論其狀態顯示的所有結果。

可限制的診斷功能，以使用執行集合`-Diagnostic`參數。  這可讓您指定應該執行哪一個類別的診斷，針對追蹤的目標，並隱藏所有其他項目。  可用的診斷類別的範例包括網路功能、 最佳作法，以及用戶端的硬體。  請參閱[cmdlet 文件](https://technet.microsoft.com/library/mt718831.aspx)來尋找可用診斷的最新清單。

> [!WARNING]
> 無診斷資料的強式監視和事件回應管線來取代。  沒有可用於監視受防護網狀架構，以及各種可監視以即早偵測出問題的事件記錄檔通道的 System Center Operations Manager 套件。  診斷可用來快速分級這些失敗，並建立採取的動作。

## <a name="targeting-diagnostics"></a>目標的診斷

`Get-HgsTrace` 針對追蹤目標會運作。  追蹤的目標是對應到 HGS 節點或受防護網狀架構內為受防護的主機的物件。  它可以視為延伸`PSSession`其中包含只能由診斷，例如主機網狀架構中的角色所需的資訊。  目標可以是隱含產生 （例如本機或手動診斷） 或明確`New-HgsTraceTarget`命令。

### <a name="local-diagnosis"></a>本機診斷

根據預設，`Get-HgsTrace`目標 localhost （亦即，cmdlet 會叫用）。  這稱為隱含的本機目標。  中會不提供任何目標時，才會使用隱含的本機目標`-Target`參數並沒有預先存在的追蹤中找到`-Path`。

隱含的本機目標會使用角色推斷來判斷目前的主機在受防護網狀架構中所扮演的角色。  這根據已安裝的粗略對應至系統上已安裝哪些功能的 Windows PowerShell 模組。  出現與否`HgsServer`模組會導致要使角色追蹤目標`HostGuardianService`以及是否有`HgsClient`模組會導致要使角色追蹤目標`GuardedHost`。  您可指定的主控件將在此情況下，它將會被視為同時存在兩個模組`HostGuardianService`和`GuardedHost`。

因此，預設值的引動過程診斷來收集本機追蹤：
```PowerShell
Get-HgsTrace
```
...相當於下列：
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` 可以接受目標透過管線或直接透過`-Target`參數。  操作也沒有任何兩者之間的差異。

### <a name="remote-diagnosis-using-trace-targets"></a>使用追蹤目標的遠端診斷

可以藉由產生遠端的連接資訊的追蹤目標，從遠端診斷主機。  只需要是認證的主機名稱和一組能夠使用 Windows PowerShell 遠端連線。
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
此範例會產生收集遠端使用者認證的提示字元，然後診斷將會執行使用上的遠端主機`hgs-01.secure.contoso.com`完成追蹤集合。  產生的追蹤會下載到 localhost，且然後診斷。  執行時，診斷的結果會顯示相同[本機診斷](#local-diagnosis)。  同樣地，它不需要指定的角色，因為可以推斷以遠端系統上安裝的 Windows PowerShell 模組。

遠端診斷會利用 Windows PowerShell 遠端執行功能遠端主機的所有存取。  因此它是追蹤目標已啟用 Windows PowerShell 遠端功能的必要條件 (請參閱[啟用 PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) 和本機主機已正確設定為啟動連線到目標。

> [!NOTE]
> 在大部分情況下，它才需要 localhost 是相同的 Active Directory 樹系的一部分，而且是有效的 DNS 主機名稱。  如果您的環境會利用更複雜的同盟模型，或者您想要使用直接的 IP 位址進行連線，您可能需要執行額外的設定，例如設定 WinRM[受信任的主機](https://technet.microsoft.com/library/ff700227.aspx)。

您可以確認追蹤目標已正確地具現化，並設定為接受連接使用`Test-HgsTraceTarget`cmdlet:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
此命令會傳回`$True`才`Get-HgsTrace`就能夠建立與追蹤目標的遠端診斷工作階段。  發生錯誤時，此 cmdlet 會傳回之 Windows PowerShell 遠端連線的進一步疑難排解的相關狀態資訊。

#### <a name="implicit-credentials"></a>隱含的認證

在執行遠端診斷時具有足夠的權限的使用者，從遠端連線到追蹤目標，不需要提供認證以便`New-HgsTraceTarget`。  `Get-HgsTrace`指令程式會自動重複使用開啟連線時叫用此指令程式使用者的認證。

> [!WARNING]
> 若要重複使用認證，特別是當執行所謂的 「 第二個躍點。 」 時，才適用某些限制  會發生這種情況時嘗試重複使用認證從遠端工作階段內以另一部電腦。  它是為了[設定 CredSSP](https://technet.microsoft.com/library/hh849872.aspx)以支援此案例中，但這已超出範圍的受防護網狀架構管理和疑難排解。

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>使用 Windows PowerShell Just Enough Administration (JEA) 和診斷

遠端診斷支援使用 JEA 限制的 Windows PowerShell 端點。 根據預設，遠端追蹤目標都將使用預設值`microsoft.powershell`端點。  如果追蹤目標`HostGuardianService`角色，它也會嘗試使用`microsoft.windows.hgs`HGS 安裝時設定的端點。

如果您想要使用自訂端點，您必須指定工作階段設定名稱建構追蹤目標使用時發生`-PSSessionConfigurationName`參數，例如下列：

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>診斷多部主機

您可以傳遞至多個追蹤目標`Get-HgsTrace`一次。  這包括本機和遠端目標的混合。  依次要追蹤每個目標，然後將同時來診斷追蹤的每個目標。  診斷工具可用來識別複雜的跨節點設定錯誤，否則將不會偵測您的部署增加的知識。  使用此功能只需要提供的追蹤從多部主機同時 （如果是手動診斷） 或目標的多部主機時呼叫`Get-HgsTrace`（如果是遠端診斷）。

以下是使用遠端的診斷，以在網狀架構是由 HGS 的兩個節點所組成的分類和兩個受守護的主機，其中一部受防護主機的使用位置以啟動範例`Get-HgsTrace`。

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> 您不需要診斷多個節點時，診斷您整個受防護網狀架構。  在許多情況下便足以包含可能會牽涉到的所有節點，在給定的失敗狀況。  這通常是受守護的主機和一些 HGS 叢集中節點的子集。

## <a name="manual-diagnosis-using-saved-traces"></a>手動使用已儲存的追蹤的診斷

有時您可能想要重新執行診斷，而不需再次收集追蹤，或您可能沒有所需的認證從遠端同時診斷所有網狀架構主機。  手動診斷是一種機制，供您仍然可以執行整個 fabric 分級，使用`Get-HgsTrace`，但不使用遠端追蹤集合。

在執行之前手動診斷，您必須確定將分級網狀架構中的每個主機的系統管理員都準備好要代替您執行命令。  診斷追蹤輸出不會公開通常視為敏感、 任何資訊，但應在使用者判斷它是否安全地公開 （expose） 給其他人的這項資訊。

> [!NOTE]
> 追蹤不匿名，並顯示網路組態、 PKI 設定，以及其他組態，有時會被視為私人資訊。  因此，應該只是追蹤傳送給組織內受信任的實體，而永遠不會公開張貼。

若要執行手動的診斷步驟如下所示：

1. 每個主機系統管理員執行的要求`Get-HgsTrace`指定已知`-Path`和您想要針對產生的追蹤執行的診斷的清單。  例如: 

 ```PowerShell
 Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
 ```
2. 要求每個主機系統管理員封裝所產生的追蹤資料夾，並將它傳送給您。  此程序可以驅動透過電子郵件，透過檔案共用或任何其他作業的原則和程序建立您的組織的方法為基礎的機制。

3. 合併至單一資料夾中，使用任何其他內容或資料夾的所有接收的追蹤。

    * 例如，假設您有系統管理員傳送您追蹤從機器收集到四個名為 HGS-01、 HGS-02、 RR1N2608-12 和 RR1N2608 13。  每個系統管理員會寄給您一個資料夾以相同的名稱。  您會將組合的目錄結構看起來像這樣：

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

4. 執行診斷，提供組合的追蹤資料夾的路徑`-Path`參數並指定`-RunDiagnostics`切換以及您要求您的系統管理員，以收集追蹤這些診斷。  診斷會假設它無法存取路徑內找到的主機，並因此會嘗試使用預先收集的追蹤。  如果任何追蹤遺失或損毀，診斷會失敗，受影響的測試，並正常執行。  例如: 

 ```PowerShell
 Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
 ```

### <a name="mixing-saved-traces-with-additional-targets"></a>混合使用其他目標儲存追蹤

在某些情況下，您可能使用一組預先收集您想要加強與其他主控件追蹤的追蹤。  可以與其他目標，將追蹤和診斷的診斷的單一呼叫中混合使用預先收集的追蹤。

下列指示來收集，並組合上面指定的追蹤資料夾，呼叫`Get-HgsTrace`預先收集的追蹤資料夾中找不到的其他追蹤目標：

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

診斷 cmdlet 會識別預先收集的所有主機和一個額外的主機，仍然需要追蹤，而且執行所需的追蹤。  然後可診斷出預先收集和全新蒐集的所有追蹤的總和。  所產生的追蹤資料夾將包含舊和新的追蹤。
