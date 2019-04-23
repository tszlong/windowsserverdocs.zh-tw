---
title: 管理功能
description: 系統的深入解析會公開各種不同的每一項功能，可以設定的設定，並可以微調這些設定，以您的部署需要的特定位址。 本主題描述如何管理透過 Windows Admin Center 或 PowerShell，提供基本的 PowerShell 範例和示範如何調整這些設定的 Windows Admin Center 螢幕擷取畫面的每一項功能的各種設定。
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868019"
---
# <a name="managing-capabilities"></a>管理功能

>適用於：Windows Server 2019

在 Windows Server 2019，系統的深入解析會公開各種不同的每一項功能，可以設定的設定，可以微調這些設定，以您的部署需要的特定位址。 本主題描述如何管理透過 Windows Admin Center 或 PowerShell，提供基本的 PowerShell 範例和示範如何調整這些設定的 Windows Admin Center 螢幕擷取畫面的每一項功能的各種設定。 

>[!TIP]
>您也可以使用這些短片可協助您開始使用，並有自信地管理系統的深入解析：[開始使用系統 Insights 在 10 分鐘內](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

雖然本節中的 PowerShell 範例，您可以使用[系統 Insights PowerShell 文件](https://aka.ms/systeminsightspowershell)若要查看所有的 cmdlet、 參數和參數的集合內系統的深入解析。 

## <a name="viewing-capabilities"></a>檢視功能

若要開始，您可以列出所有可用的功能，使用**Get InsightsCapability** cmdlet: 

```PowerShell
Get-InsightsCapability
``` 
這些功能也會顯示在系統 Insights 擴充功能的：

![列出可用功能的系統深入解析的 [概觀] 頁面](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>啟用和停用功能
每一項功能可以啟用或停用。 停用一項功能可防止這項功能所叫用和非預設功能，停用功能會停止這項功能的所有資料收集。 根據預設，會啟用所有功能，以及您可以檢查功能，使用的狀態**Get InsightsCapability** cmdlet。 

若要啟用或停用功能，請使用**啟用 InsightsCapability**並**停用 InsightsCapability** cmdlet:

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
這些設定也可以切換所選取一項功能在 Windows Admin Center 滑鼠**啟用**或是**停用**按鈕。

### <a name="invoking-a-capability"></a>叫用的功能
叫用一項功能時，立即執行的功能，可擷取預測和系統管理員可以叫用一項功能隨時依序按一下**Invoke**按鈕，在 Windows Admin Center 或使用**叫用 InsightsCapability** cmdlet:

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>若要確定叫用一項功能不在您的電腦上的重要作業相衝突，請考慮將排程關閉營業時間的預測。

## <a name="retrieving-capability-results"></a>擷取能力結果
一旦已叫用一項功能，最新的結果會顯示使用**Get InsightsCapability**或是**Get InsightsCapabilityResult**。 這些 cmdlet 會輸出最近**狀態**並**狀態描述**的每一項功能，其會描述每一項預測的結果。 **狀態**並**狀態描述**中，會進一步說明欄位[了解功能文件](understanding-capabilities.md)。 

此外，您可以使用**Get InsightsCapabilityResult** cmdlet 來檢視過去 30 的預測結果，並擷取與預測相關聯的資料： 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
系統 Insights 擴充功能會自動顯示預測的歷程記錄，並剖析 JSON 結果，讓您的直覺式、 高精確性的圖形的每個預測的結果：

![顯示預測的圖表和預測的歷程記錄的單一功能頁面](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>使用事件記錄檔擷取能力結果
系統的深入解析記錄的事件功能完成預測每次。 這些事件會顯示在**Microsoft-Windows-系統-Insights/Admin**通道和系統的深入解析發佈的每個狀態不同的事件識別碼：   

| 預測的狀態 | 事件識別碼 |
| --------------- | --------------- |
| 確定 | 151 |
| 警告 | 148 |
| 嚴重 | 150 |
| 錯誤 | 149 |
| None | 132 |

>[!TIP]
>使用[Azure 監視器](https://azure.microsoft.com/services/monitor/)或是[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)彙總這些事件，並查看電腦群組的預測結果。


## <a name="setting-a-capability-schedule"></a>設定功能排程
除了隨預測中，您可以設定定期預測的每一項功能，讓預先定義的排程會自動叫用指定的功能。 使用**Get InsightsCapabilitySchedule** cmdlet 來查看功能排程： 

>[!TIP]
>若要查看所傳回的所有功能的資訊，在 PowerShell 中使用管線運算子**Get InsightsCapability** cmdlet。

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

依預設會啟用定期的預測，不過在任何時候使用停用**啟用 InsightsCapabilitySchedule**並**停用 InsightsCapabilitySchedule** cmdlet:

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

每個預設功能會排定為每天上午 3 點執行。 不過，您可以建立自訂的排程，每項功能，以及系統 Insights 支援各種不同的排程類型，您可以使用設定**組 InsightsCapabilitySchedule** cmdlet: 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>預設功能分析每日的資料，因為它建議使用這些功能的每日排程。 深入了解的預設功能[此處](understanding-capabilities.md)。

您也可以檢視和設定排程的每一項功能，即可使用 Windows Admin Center**設定**。 顯示目前的排程**排程**索引標籤上，而且您可以使用 GUI 工具來建立新的排程：

![設定頁面上顯示目前的排程](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>建立的修復動作
系統的深入解析可讓您開始進行一項功能的結果為基礎的自訂補救指令碼。 針對每項功能，您可以設定自訂的 PowerShell 指令碼，每個預測狀態，可讓系統管理員不需要手動介入的情況下自動採取更正動作。 

範例的修復動作包括執行磁碟清理，延伸磁碟區，執行重複資料刪除，即時移轉的 Vm，並設定 Azure 檔案同步。

您可以看到每個功能使用的動作**Get InsightsCapabilityAction** cmdlet:

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

您可以建立新的動作，或刪除現有的動作，使用**組 InsightsCapabilityAction**並**移除 InsightsCapabilityAction** cmdlet。 使用中所指定的認證來執行每個動作**ActionCredential**參數。

>[!NOTE]
>在初始系統 Insights 版本中，您必須指定外部使用者目錄的補救指令碼。 這將會在即將推出的版本中修正。

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

您也可以使用 Windows Admin Center 來設定使用的補救動作**動作**索引標籤內**設定**頁面：

![使用者可以在其中指定的修復動作的 [設定] 頁面](media/actions-page-contoso.png)


## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [系統 Insights 概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [新增和開發功能](adding-and-developing-capabilities.md)
- [系統 Insights 常見問題集](faq.md)