---
title: 管理功能
description: 系統深入解析會公開各種可針對每項功能設定的設定，並可調整這些設定以解決部署的特定需求。 本主題說明如何透過 Windows Admin Center 或 PowerShell 管理各項功能的各種設定，並提供基本的 PowerShell 範例和 Windows Admin Center 的螢幕擷取畫面，以示範如何調整這些設定。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 3f4e80136b3c70b7a121663a6defa048d2b0e852
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766221"
---
# <a name="managing-capabilities"></a>管理功能

>適用於：Windows Server 2019

在 Windows Server 2019 中，系統深入解析會公開各種可針對每項功能設定的設定，並可調整這些設定以解決部署的特定需求。 本主題說明如何透過 Windows Admin Center 或 PowerShell 管理各項功能的各種設定，並提供基本的 PowerShell 範例和 Windows Admin Center 的螢幕擷取畫面，以示範如何調整這些設定。

>[!TIP]
>您也可以使用這些短片來協助您開始使用，並安心地管理系統深入解析： [10 分鐘內開始使用系統深入](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)解析

雖然本節提供 PowerShell 範例，但您可以使用 [系統深入解析 PowerShell 檔](/powershell/module/systeminsights/) 來查看系統深入解析中的所有 Cmdlet、參數和參數集。

## <a name="viewing-capabilities"></a>觀賞功能

若要開始使用，您可以使用 **InsightsCapability** Cmdlet 來列出所有可用的功能：

```PowerShell
Get-InsightsCapability
```
您也可以在系統深入解析延伸模組中看到這些功能：

![列出可用功能的系統深入解析的總覽頁面](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>啟用和停用功能
每項功能都可以啟用或停用。 停用功能可防止叫用該功能，而針對非預設的功能，停用功能會停止該功能的所有資料收集。 預設會啟用所有功能，而且您可以使用 **InsightsCapability 指令程式** 來檢查功能的狀態。

若要啟用或停用功能，請使用 **InsightsCapability** 和 **InsightsCapability** Cmdlet：

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
```
在 Windows Admin Center 按一下 [ **啟用** ] 或 [ **停** 用] 按鈕時，也可以透過選取的功能來切換這些設定。

### <a name="invoking-a-capability"></a>叫用功能
叫用功能可立即執行抓取預測的功能，而系統管理員 **可以在 Windows Admin Center 中按一下 [叫** 用] 按鈕或使用 **InsightsCapability** 指令程式，隨時叫用功能：

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>若要確定叫用功能不會與您電腦上的重要作業發生衝突，請考慮在上班時間排程預測。

## <a name="retrieving-capability-results"></a>正在抓取功能結果
一旦叫用某項功能，就可以使用 **InsightsCapability** 或 **InsightsCapabilityResult**來顯示最新的結果。 這些 Cmdlet 會輸出每個功能的最新 **狀態** 和 **狀態原因** ，其中描述每個預測的結果。 [[瞭解功能] 檔](understanding-capabilities.md)中會進一步說明 [**狀態**] 和 [**狀態原因**] 欄位。

此外，您可以使用 **InsightsCapabilityResult** Cmdlet 來查看最近的30個預測結果，並取出與預測相關聯的資料：

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
系統深入解析延伸模組會自動顯示預測歷程記錄，並剖析 JSON 結果的結果，讓您能夠以直覺且高精確度的圖表來呈現每個預測：

![顯示預測圖表和預測歷程記錄的單一功能頁面](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>使用事件記錄檔取出功能結果
系統深入解析會在每次功能完成預測時記錄事件。 這些事件會顯示在 **Microsoft-Windows-Insights/系統管理** 通道中，而系統深入解析會針對每個狀態發佈不同的事件識別碼：

| 預測狀態 | 事件識別碼 |
| --------------- | --------------- |
| 確定 | 151 |
| 警告 | 148 |
| Critical | 150 |
| 錯誤 | 149 |
| 無 | 132 |

>[!TIP]
>使用 [Azure 監視器](https://azure.microsoft.com/services/monitor/) 或 [System Center Operations Manager](/system-center/scom/welcome?view=sc-om-1807) 來匯總這些事件，並查看一組機器上的預測結果。


## <a name="setting-a-capability-schedule"></a>設定功能排程
除了隨選預測之外，您還可以為每項功能設定定期預測，以根據預先定義的排程自動叫用指定的功能。 使用 **InsightsCapabilitySchedule** 指令 Cmdlet 查看功能排程：

>[!TIP]
>使用 PowerShell 中的管線運算子來查看 **InsightsCapability** 指令程式所傳回之所有功能的資訊。

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

預設會啟用定期預測，不過您可以使用 **InsightsCapabilitySchedule** 和 **InsightsCapabilitySchedule** Cmdlet 隨時停用這些預測：

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

每個預設功能會排定在每天3am 執行。 不過，您可以為每個功能建立自訂排程，而系統深入解析支援各種不同的排程類型，可使用 InsightsCapabilitySchedule 指令 **程式** 進行設定：

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30
```
>[!NOTE]
>由於預設功能會分析每日資料，因此建議使用這些功能的每日排程。 若要深入瞭解 [預設功能，](understanding-capabilities.md)請參閱。

您也可以按一下 [ **設定**]，使用 Windows Admin Center 來查看和設定每項功能的排程。 目前的排程會顯示在 [ **排程** ] 索引標籤上，而且您可以使用 GUI 工具來建立新的排程：

![顯示目前排程的設定頁面](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>建立補救動作
系統深入解析可讓您根據功能的結果來啟動自訂補救腳本。 針對每個功能，您可以為每個預測狀態設定自訂 PowerShell 腳本，讓系統管理員可以自動採取更正動作，而不需要手動介入。

範例補救動作包括執行磁片清理、擴充磁片區、執行重復資料刪除、即時移轉 Vm，以及設定 Azure 檔案同步。

您可以使用 **InsightsCapabilityAction** Cmdlet 來查看每個功能的動作：

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

您可以使用 InsightsCapabilityAction 和 InsightsCapabilityAction Cmdlet 建立新 **的** 動作，或刪除現有 **的** 動作。 每個動作都會使用 **ActionCredential** 參數中指定的認證來執行。

>[!NOTE]
>在初始的系統深入解析版本中，您必須在使用者目錄之外指定補救腳本。 這將會在未來的版本中修正。

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

您也可以使用 [**設定**] 頁面中的 [**動作**] 索引標籤，使用 Windows Admin Center 設定補救動作：

![使用者可在其中指定補救動作的設定頁面](media/actions-page-contoso.png)


## <a name="additional-references"></a>其他參考資料
若要深入瞭解系統深入解析，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [系統深入解析常見問題](faq.md)