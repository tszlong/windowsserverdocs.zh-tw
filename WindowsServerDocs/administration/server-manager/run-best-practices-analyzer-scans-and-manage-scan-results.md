---
title: 執行最佳做法分析程式掃描及管理掃描 Results_1
description: 伺服器管理員
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8c0440da49e6e78afece1af3ee8357ddf846e7e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895735"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>執行最佳做法分析程式掃描與管理掃描結果

>適用於：Windows Server 2016

在 Windows 管理中，「最佳做法」** 是專家定義在一般情況下最理想的伺服器設定方式的指導方針。 例如，就大多數伺服器應用程式而言，僅開啟應用程式用來與其他網路電腦通訊所需的連接埠，並封鎖未使用的連接埠，便是一種最佳做法。 雖然違反最佳做法 (甚至是重要違規) 也不一定會有問題，但那樣的伺服器設定可能導致效能不佳、可靠性差、未預期衝突、安全性風險增加或其他潛在問題。

 (BPA) 的最佳做法分析程式是 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 中提供的伺服器管理工具。 BPA 可以掃描安裝在執行 Windows Server 2012 或 Windows Server 2008 R2 的受管理伺服器上的角色，並將最佳做法違規報告給系統管理員，藉此協助系統管理員減少最佳做法違規。

您可以從伺服器管理員、使用 BPA GUI，或在 Windows PowerShell 中使用 Cmdlet，來執行最佳做法分析程式 (BPA) 掃描。 從 Windows Server 2012 開始，您可以在多部伺服器上一次掃描一個角色或多個角色，不論您是使用伺服器管理員主控台或 Windows PowerShell Cmdlet 中的 [最佳做法分析程式] 磚來執行掃描。 您也可以指示 BPA 排除或略過不需要看到的掃描結果。

本主題包含下列各節。

-   [尋找 BPA](#BKMK_find)

-   [BPA 的運作方式](#BKMK_how)

-   [對角色執行最佳做法分析程式掃描](#BKMK_BPAscan)

-   [管理掃描結果](#BKMK_manage)

## <a name="find-bpa"></a><a name=BKMK_find></a>尋找 BPA
您可以在 Windows Server 2012 R2 和 Windows Server 2012 中伺服器管理員的角色和伺服器群組頁面上找到最佳做法分析程式磚，或者可以使用提高的使用者權限開啟 Windows PowerShell 會話，以執行最佳做法分析程式 Cmdlet。

## <a name="how-bpa-works"></a><a name=BKMK_how></a>BPA 的運作方式
BPA 的運作方式是以八種不同的效率、可信度和可靠性的最佳做法規則來測量角色的合規性。 衡量的結果會是下表中三種嚴重性等級的任一種。

|嚴重性層級|描述|
|---------|--------|
|錯誤|當角色未滿足最佳做法規則的條件時，就會傳回錯誤結果，而且可以預期會發生功能問題。|
|資訊|當角色滿足最佳做法規則的條件時，就會傳回資訊結果。|
|警告|如果不符合標準的結果會在未進行變更時引發問題，則會傳回警告結果。 應用程式目前運作時可能符合標準，但是如果未變更其設定或原則設定則可能不會滿足規則的條件。 例如，如果角色無法使用授權伺服器，則遠端桌面服務的掃描會顯示警告結果，因為即使掃描時遠端連線不在作用中，也不會導致授權伺服器無法讓新的遠端連線取得有效的用戶端存取授權。|

### <a name="rule-categories"></a>規則類別
下表描述最佳作法規則分類，針對在最佳做法分析程式掃描期間測量的角色。

|類別名稱|描述|
|---------|--------|
|安全性|安全性規則會套用來測量角色的相對風險，以暴露于未經授權或惡意使用者的威脅，或是機密或專屬資料遺失或遭竊。|
|效能|效能規則適用于在給定角色工作負載的預期期間內，測量角色處理要求並在企業中執行其規定責任的能力。|
|設定|設定規則用於識別可能需要修改的角色設定，讓角色能有效執行。 設定規則有助於避免設定中的衝突，以防止可能的錯誤訊息或角色無法執行企業中規定之職責的情形。|
|原則|原則規則適用于識別可能需要修改的群組原則或 Windows 登錄設定，才能以最佳且安全的方式操作角色。|
|作業|操作規則用於識別角色執行企業中規定的工作時可能發生的失敗情形。|
|前置部署|前置部署規則會在企業中部署已安裝的角色之前套用。 它們讓系統管理員可以在將角色用於實際執行之前評估是否已滿足最佳做法。|
|部署後|為角色啟動所有必要的服務後且該角色已在企業中執行後，就需要套用後續部署規則。|
|必要條件|先決條件規則說明角色需要哪些組態設定、原則設定及功能，BPA 才能從其他類別套用特定規則。 掃描結果中的先決條件指出不正確的設定、遺失程式、未正確啟用或停用的原則、登錄機碼設定或其他設定導致 BPA 在掃描期間無法套用一或多個規則。 必要條件結果並不代表相容或不相容。 它代表無法套用某個規則，因此不是掃描結果的一部分。|

## <a name="performing-best-practices-analyzer-scans-on-roles"></a><a name=BKMK_BPAscan></a>對角色執行最佳做法分析程式掃描
您可以使用伺服器管理員中的 BPA GUI 或使用 Windows PowerShell Cmdlet，在角色上執行 BPA 掃描。

在 Windows Server 2012 R2 和 Windows Server 2012 中，某些角色會在啟動 BPA 掃描之前，提示您指定其他參數，例如，正在執行角色部分的特定伺服器或共用的名稱，或是子模型的識別碼。 對於要求您指定其他參數的模型上的 BPA 掃描，請使用 BPA Cmdlet；BPA GUI 無法接受其他像是子模型識別碼的參數。 例如，子模型識別碼 **FSRM** 代表檔案伺服器資源管理員的檔案服務 BPA 子模型，這是檔案和存放服務的角色服務。 若只要在檔案伺服器 Resource Manager 角色服務上執行掃描，請使用 Windows PowerShell Cmdlet 執行 BPA 掃描，並將參數新增 `SubmodelId` 至您的 Cmdlet。

雖然您無法將其他參數傳遞到您在 BPA GUI 中啟動的掃描，伺服器管理員中的 BPA 磚會顯示最新 BPA 掃描的結果，而不論掃描的啟動方式為何。

-   [使用 BPA GUI 掃描角色](#BKMK_GUIscan)

-   [使用 Windows PowerShell Cmdlet 掃描角色](#BKMK_PSscan)

### <a name="scanning-roles-by-using-the-bpa-gui"></a><a name=BKMK_GUIscan></a>使用 BPA GUI 掃描角色
遵循下列步驟，在 BPA GUI 中掃描一或多個角色。

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>使用 BPA GUI 掃描角色

1.  執行下列其中一項動作以開啟伺服器管理員（如果尚未開啟）。

    -   在 Windows 工作列上，按一下 [伺服器管理員] 按鈕。

    -   在 [**開始**] 畫面上，按一下 [伺服器管理員] 磚。

2.  在瀏覽窗格中，開啟角色或群組頁面。

    從角色或群組頁面執行 BPA 掃描，以掃描已安裝於該群組中伺服器上的所有角色。

3.  在 [**最佳做法分析**程式] 磚的 [工作 **] 功能表上，按一下 [** **啟動 BPA 掃描**]。

4.  根據針對您選取的角色或群組所評估的規則數目而定，BPA 掃描可能需要數分鐘的時間才能完成。

### <a name="scanning-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_PSscan></a>使用 Windows PowerShell Cmdlet 掃描角色
使用下列程式，使用 Windows PowerShell Cmdlet 掃描一或多個角色。

> [!NOTE]
> 本節中的程序不會顯示所有 BPA Cmdlet 與參數。 如需 Windows PowerShell 中的 BPA 作業的詳細資訊，請在 Windows PowerShell 會話中輸入**Get-help***BPACmdlet***-Full**，其中*BPACmdlet*可以是下列其中一個值。 您也可以在[Windows Server 技術中心](https://go.microsoft.com/fwlink/p/?LinkId=240177)上尋找 BPA Cmdlet 說明主題。

-   **Get-bpamodel**

-   **叫用-Get-bpamodel**

-   **Get-bparesult**

-   **設定-Get-bparesult**

#### <a name="to-scan-a-single-role-by-using-windows-powershell-cmdlets"></a><a name=BKMK_singlerole></a>使用 Windows PowerShell Cmdlet 掃描單一角色

1.  執行下列其中一項動作，以提升的使用者權限執行 Windows PowerShell。

    -   若要從 [**開始**] 畫面以系統管理員身分執行 Windows powershell，請以滑鼠右鍵按一下 [**應用程式**] 結果中的 [ **windows powershell** ] 磚，然後在應用程式行上，按一下 [以**系統管理員身分執行**]。

    -   若要從桌面以系統管理員身分執行 Windows PowerShell，請在工作列中的**Windows powershell**快捷方式上按一下滑鼠右鍵，然後按一下 [以**系統管理員身分執行**]。

2.  從 Windows PowerShell 3.0 開始，第一次使用模組的 Cmdlet 時，Cmdlet 模組會自動匯入到您的 Windows PowerShell 會話。 不需要匯入或載入 BPA Cmdlet 模組。

3.  藉由輸入**get-bpamodel** Cmdlet，尋找可執行 BPA 掃描的所有角色模型識別碼，如下列範例所示。

    `Get-Bpamodel`

4.  在步驟 3 的結果中，找到要執行 BPA 掃描的角色模型識別碼。

5.  輸入下列任一個命令，開始進行特定角色的 BPA 掃描。 如果有多個角色，可以使用逗點分隔模型識別碼。

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **範例：** `Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > 模型識別碼包含 [識別碼]**** 欄中所顯示的完整路徑；例如，**Microsoft/Windows/Hyper-V**。

    您也可以透過將 `Get-BPAmodel` Cmdlet 的結果以管線方式傳遞給 `Invoke-BPAmodel` Cmdlet，從步驟 3 的結果中開始在特定的角色上進行掃描，如下列範例所示。

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    執行這個指令程式時，若未指定模型識別碼，會將 Cmdlet 傳回的所有模型傳送 `Get-BPAmodel` 到 `Invoke-BPAmodel` Cmdlet，開始掃描已新增至伺服器管理員伺服器集區的伺服器上可用的所有模型。

#### <a name="to-scan-all-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_allroles></a>使用 Windows PowerShell Cmdlet 掃描所有角色

1.  使用提高的使用者權限開啟 Windows PowerShell 會話（如果尚未開啟的話）。 如需相關指示，請參閱後續程序。

2.  將可執行 BPA 掃描的所有角色以管線方式傳遞給 `Invoke-BPAmodel` Cmdlet 以啟動掃描程序。

    `Get-BPAmodel | Invoke-BPAmodel`

3.  當掃描完成時，Windows PowerShell 會針對每個掃描的角色傳回類似下列的結果。

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="manage-scan-results"></a><a name=BKMK_manage></a>管理掃描結果
在 GUI 中完成 BPA 掃描之後，您可以在 BPA 磚中檢視 BPA 掃描結果。 當您在磚中選取結果時，磚中的預覽窗格便會顯示結果內容，包含角色是否會與相關聯最佳做法相容的指示。 如果結果不符合規範，而您想要知道如何解決結果內容中所述的問題，錯誤和警告結果內容中的超連結會在 Windows Server 技術中心上開啟詳細的解決方案說明主題。

> [!NOTE]
> BPA 掃描結果不會自動儲存或封存。 在模型或子模型上執行新掃描會覆寫最後一次掃描的結果。 若要儲存 BPA 掃描結果以封存、列印或傳送到其他地方，請參閱本節的[以不同格式檢視 Windows PowerShell 工作階段的 BPA 結果](#BKMK_formats)。

### <a name="exclude-and-include-bpa-results"></a>排除與包含 BPA 結果
如果您不需要看到某些 BPA 結果（例如，BPA 掃描中經常發生但不需要解決方案的結果），您可以使用 Windows PowerShell 中的 BPA GUI 或 BPA Cmdlet 來排除結果。 這類結果隨時可以重新包含在內。

> [!NOTE]
> 當您排除結果時，也會從檢視或受管理的伺服器中將它們排除。 其他的系統管理員無法在受管理的伺服器上看到排除的結果。 若只要在本機伺服器管理員主控台中排除 view 的結果，請建立自訂查詢，而不是使用 [**排除結果**] 命令。

#### <a name="exclude-scan-results"></a><a name=BKMK_exclude></a>排除掃描結果
[排除]**** 設定是永久性；除非再次包含您排除的結果，否則，未來在相同電腦上進行相同模型的掃描時仍會持續排除它們。

您可以將 `Set-BPAResult` Cmdlet 與 `Exclude` 參數搭配使用，排除掃描結果。 如同在伺服器管理員的 [最佳做法分析程式] 磚中，您可以排除個別的結果物件，也可以排除欄位 (分類、標題和嚴重性的一組結果，例如) 等於或包含指定的值。 例如，您可以從模型的一組掃描結果中排除所有 [效能]**** 結果。

> [!NOTE]
> 在使用本節的程序之前，您必須對模型至少執行一次 BPA 掃描。

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>使用 GUI 排除掃描結果

1.  在伺服器管理員中開啟角色或伺服器群組頁面。

2.  在角色或伺服器群組的 [最佳做法分析程式] 磚中，以滑鼠右鍵按一下清單中的結果，然後按一下 [**排除結果**]。

    結果便不會再顯示於結果清單中。

3.  若要在 GUI 中檢視排除的結果，請執行內建的 [排除的結果]**** 查詢。 按一下 [儲存的搜尋查詢]****，然後按一下 [排除的結果]****。

    執行 [排除的結果]**** 查詢之後，記下磚的子標題文字、清單中顯示的結果描述，以及對於 [排除的結果]**** 所做的變更。 只會在清單中顯示排除的結果。

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell Cmdlet 排除掃描結果

1.  使用提高的使用者權限開啟 Windows PowerShell 工作階段。

2.  執行下列命令，以從模型掃描排除特定結果。

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq Value} | Set-BPAResult -Exclude $true`

    上述命令會針對*模型識別碼*所代表的模型識別碼，抓取 BPA 掃描結果專案。

    命令的第二部分會篩選 `Get-BPAResult` Cmdlet 的結果，僅擷取結果欄位 (由 *Field Name* 來表示) 的值符合雙引號內文字的掃描結果。

    命令的最後部分 (在第二個直立線字元之後) 會排除 Cmdlet 前面部分篩選的結果。

    **範例：** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>包含掃描結果
當您想要檢視被排除的掃描結果時，可以包含這些掃描結果。 [包含]**** 設定是永久性；包含的結果未來在相同電腦上的相同模組中進行的掃描仍會維持包含狀態。

##### <a name="to-include-scan-results-by-using-the-gui"></a><a name=BKMK_gui></a>使用 GUI 包含掃描結果

1.  在伺服器管理員中開啟角色或伺服器群組頁面。

2.  在角色或伺服器群組的 [最佳做法分析程式] 磚中，以滑鼠右鍵按一下 [**排除的結果**] 查詢清單中排除的結果，然後按一下 [**包含結果**]。

    結果便不會再顯示於排除的結果清單中。 按一下 [全部清除]**** 來清除查詢，以便在所有包含的結果清單中檢視包含的結果。

##### <a name="to-include-scan-results-by-using-windows-powershell-cmdlets"></a><a name=BKMK_cmdlets></a>使用 Windows PowerShell Cmdlet 包含掃描結果

1.  使用提高的使用者權限開啟 Windows PowerShell 工作階段。

2.  輸入下列命令然後按 **Enter**，以從模型掃描包含特定結果。

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq Value } | Set-BPAResult -Exclude $false`

    上述命令會針對*模型識別碼*所代表的模型，抓取 BPA 掃描結果專案。

    命令的第二部分（在第一個管線字元 ( 之後） **|** ) 會篩選 get-bparesult 指令**程式**的結果，只取出結果欄位（以功能變數名稱表示）的值符合引號中文字的掃描*Field Name*結果。

    命令的最後部分 (在第二個直立線字元之後) 會將 **-Exclude** 參數的值設定為 **false**，以包含 Cmdlet 的第二部分篩選的結果。

    **範例：** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>在 Windows PowerShell 中檢視和匯出 BPA 掃描結果
若要使用 Windows PowerShell Cmdlet 來查看和管理掃描結果，請參閱下列程式。 在您可以使用下列任一個程序之前，請至少在一個模型或子模型上至少執行一次 BPA 掃描。

#### <a name="to-view-results-of-the-most-recent-scan-of-a-role-by-using-windows-powershell"></a><a name=BKMK_recentPS></a>使用 Windows PowerShell 檢視角色的最近掃描結果

1.  使用提高的使用者權限開啟 Windows PowerShell 工作階段。

2.  取得特定模型識別碼的最近掃描結果。 輸入下列程式，其中模型是以*模型識別碼*表示，然後按**enter**。 您可以使用逗號分隔模型識別碼，來取得多個模型識別碼的結果。

    `Get-BPAResult <model ID>`

    **範例：** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    如果您掃描了模型的子模型（例如角色服務），請在 Cmdlet 中包含子模型識別碼，只取得該子模型的結果。

    **範例：** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="to-view-or-save-bpa-results-from-windows-powershell-sessions-in-different-formats"></a><a name=BKMK_formats></a>以不同格式檢視或儲存 Windows PowerShell 工作階段的 BPA 結果

-   在 Windows PowerShell 中，每個 BPA 結果都會與下列類似。

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    執行下列其中一項動作。

    -   若要將表格中的 BPA 結果格式化，請執行下列 Cmdlet，從前述範例新增想要看到的結果內容。

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **範例：** `Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   若要在 GUI 型資料格檢視器中，利用文字字串篩選條件以及按一下就可排序結果的欄標題，將 BPA 結果格式化，請執行下列 Cmdlet。

        `Get-BPAResult <model ID> | OGV`

    -   若要將 BPA 結果匯出到可以封存或傳送給電子郵件收件者的 HTML 檔案，請執行下列 Cmdlet，其中*path*代表您要儲存 HTML 結果的路徑和檔案名。

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **範例：** `Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   若要將 BPA 結果匯出為逗號分隔值 (CSV) 文字檔，請執行下列 Cmdlet，其中*path*代表您要儲存 CSV 結果的路徑與文字檔名稱。 CSV 結果可以匯入 Microsoft Excel，或是在試算表或方格中顯示資料的其他程式。

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **範例：** `Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>另請參閱
[Windows Server 技術中心上的最佳做法分析程式解決方案內容](https://go.microsoft.com/fwlink/p/?LinkId=241597) 
[在伺服器管理員磚中篩選、排序及查詢資料](filter-sort-and-query-data-in-server-manager-tiles.md) 
[使用伺服器管理員管理多部遠端伺服器](manage-multiple-remote-servers-with-server-manager.md)
