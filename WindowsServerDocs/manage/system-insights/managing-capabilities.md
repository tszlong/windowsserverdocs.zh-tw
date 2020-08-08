---
title: 管理功能
description: System Insights 會公開可針對每項功能設定的各種設定，而且可以微調這些設定來解決部署的特定需求。 本主題說明如何透過 Windows 管理中心或 PowerShell 管理每項功能的各種設定，並提供基本的 PowerShell 範例和 Windows 管理中心的螢幕擷取畫面，以示範如何調整這些設定。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: e82b27d2d746592b29b86a66ee34b21f8605a0d8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940159"
---
# <a name="managing-capabilities"></a>管理功能

>適用於：Windows Server 2019

在 Windows Server 2019 中，System Insights 會公開可針對每項功能設定的各種設定，而且可以微調這些設定來解決部署的特定需求。 本主題說明如何透過 Windows 管理中心或 PowerShell 管理每項功能的各種設定，並提供基本的 PowerShell 範例和 Windows 管理中心的螢幕擷取畫面，以示範如何調整這些設定。

>[!TIP]
>您也可以使用這些短片，協助您開始使用並安心地管理系統深入解析： [10 分鐘內的系統深入](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)解析使用者入門

雖然本節提供 PowerShell 範例，但您可以使用[System Insights PowerShell 檔](https://aka.ms/systeminsightspowershell)來查看 system insights 中的所有 Cmdlet、參數和參數集。

## <a name="viewing-capabilities"></a>觀賞功能

若要開始，您可以使用**InsightsCapability** Cmdlet 來列出所有可用的功能：

```PowerShell
Get-InsightsCapability
```
這些功能也會顯示在 System Insights 延伸模組中：

![System Insights 列出可用功能的總覽頁面](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>啟用和停用功能
每項功能都可以啟用或停用。 停用功能可防止叫用該功能，而且對於非預設的功能，停用功能會停止該功能的所有資料收集。 根據預設，會啟用所有功能，您可以使用**InsightsCapability 指令程式**來檢查功能的狀態。

若要啟用或停用功能，請使用**InsightsCapability**和**InsightsCapability** Cmdlet：

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
```
這些設定也可以藉由選取 [Windows 管理中心] 中的功能來切換，請按一下 [**啟用**] 或 [**停**用] 按鈕。

### <a name="invoking-a-capability"></a>叫用功能
叫用功能會立即執行取得預測的功能，而且系統管理員可以按一下 Windows 系統管理中心的 [叫用] 按鈕或使用**InsightsCapability** **Cmdlet，隨時**叫用功能：

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>若要確保叫用功能不會與您電腦上的重大作業衝突，請考慮在非上班時間排程預測。

## <a name="retrieving-capability-results"></a>正在抓取功能結果
一旦叫用功能之後，就可以使用**InsightsCapability**或**InsightsCapabilityResult**來顯示最新的結果。 這些 Cmdlet 會輸出每項功能的最新**狀態**和**狀態原因**，其中描述每個預測的結果。 [**狀態**] 和 [**狀態原因**] 欄位會在「[瞭解功能」檔](understanding-capabilities.md)中進一步說明。

此外，您可以使用**InsightsCapabilityResult** Cmdlet 來查看最後30個預測結果，並取出與預測相關聯的資料：

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
System Insights 延伸模組會自動顯示預測歷程記錄，並剖析 JSON 結果的結果，為您提供每個預測的直覺、高精確度圖表：

![顯示預測圖表和預測歷程記錄的單一功能頁面](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>使用事件記錄檔取出功能結果
「System Insights」會在每次功能完成預測時記錄事件。 這些事件會顯示在**Microsoft-Windows 系統深入解析/管理**通道中，而 System Insights 會針對每個狀態發行不同的事件識別碼：

| 預測狀態 | 事件識別碼 |
| --------------- | --------------- |
| 確定 | 151 |
| 警告 | 148 |
| 重大 | 150 |
| 錯誤 | 149 |
| None | 132 |

>[!TIP]
>使用[Azure 監視器](https://azure.microsoft.com/services/monitor/)或[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)來匯總這些事件，並查看機器群組之間的預測結果。


## <a name="setting-a-capability-schedule"></a>設定功能排程
除了隨選預測之外，您還可以設定每項功能的定期預測，以便在預先定義的排程上自動叫用指定的功能。 使用**InsightsCapabilitySchedule** Cmdlet 來查看功能排程：

>[!TIP]
>在 PowerShell 中使用管線運算子，以查看**InsightsCapability**指令程式所傳回之所有功能的資訊。

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

定期預測會預設為啟用，但可以使用**InsightsCapabilitySchedule**和**Disable-InsightsCapabilitySchedule** Cmdlet 隨時停用：

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

每個預設功能會排定在每天的3am 執行。 不過，您可以針對每個功能建立自訂排程，而 System Insights 支援各種不同的排程類型，可以使用**InsightsCapabilitySchedule 指令程式**來設定：

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30
```
>[!NOTE]
>由於預設功能會分析每日資料，因此建議使用這些功能的每日排程。 在[這裡](understanding-capabilities.md)深入瞭解預設功能。

您也可以按一下 [**設定**]，使用 [Windows 管理中心] 來查看和設定每項功能的排程。 目前的排程會顯示在 [**排程**] 索引標籤上，您可以使用 GUI 工具來建立新的排程：

![顯示目前排程的 [設定] 頁面](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>建立補救動作
系統深入解析可讓您根據功能的結果，啟動自訂補救腳本。 針對每個功能，您可以針對每個預測狀態設定自訂的 PowerShell 腳本，讓系統管理員自動採取更正動作，而不需要手動介入。

範例補救動作包括執行磁片清理、擴充磁片區、執行重復資料刪除、即時移轉 Vm，以及設定 Azure 檔案同步。

您可以使用**InsightsCapabilityAction** Cmdlet 來查看每項功能的動作：

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

您可以使用 InsightsCapabilityAction 和**Remove-InsightsCapabilityAction** Cmdlet 來建立新**的**動作或刪除現有的動作。 每個動作都是使用**ActionCredential**參數中指定的認證來執行。

>[!NOTE]
>在初始的 System Insights 版本中，您必須指定使用者目錄以外的補救腳本。 這將會在未來的版本中修正。

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

您也可以使用 [**設定**] 頁面中的 [**動作**] 索引標籤，透過 Windows 系統管理中心來設定修復動作：

![使用者可以在其中指定補救動作的 [設定] 頁面](media/actions-page-contoso.png)


## <a name="additional-references"></a>其他參考資料
若要深入瞭解「系統深入解析」，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [System Insights 常見問題](faq.md)