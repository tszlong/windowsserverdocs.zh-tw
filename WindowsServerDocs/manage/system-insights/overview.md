---
title: 系統 Insights 概觀
description: 系統 Insights 是新的預測性分析功能，在 Windows Server 2019。 系統 Insights 預測功能-每個備份的機器學習服務模型-在本機上分析 Windows Server 系統資料，例如效能計數器和事件，深入了解您的伺服器的運作狀況，並協助您減少被動管理您的部署中的問題相關聯的營運費用。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887239"
---
# <a name="system-insights-overview"></a>系統 Insights 概觀

>適用於：Windows Server 2019

系統 Insights 是新的預測性分析功能，在 Windows Server 2019。 系統 Insights 預測功能-每個備份的機器學習服務模型-在本機上分析 Windows Server 系統資料，例如效能計數器和事件，深入了解您的伺服器的運作狀況，並協助您減少被動管理您的部署中的問題相關聯的營運費用。 

在 Windows Server 2019，系統的深入解析會隨附四個著重於容量預測、 預測未來的資源，如計算、 網路和儲存體，根據先前的使用模式的預設功能。 系統 Insights 也隨附[可擴充的基礎結構](adding-and-developing-capabilities.md)，因此 Microsoft 和第 3 方可以加入新的預測功能系統 Insights 而不需要更新作業系統。 

您可以透過直覺式的管理系統的深入解析[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview)延伸模組或[直接透過 PowerShell](https://aka.ms/SystemInsightsPowerShell)，及系統 application Insights 可讓您設定每個預測的功能分別根據您的部署需求。 所有的預測結果會發行至事件記錄檔，可讓您使用[Azure 監視器](https://azure.microsoft.com/services/monitor/)或是[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)輕鬆地彙總，並查看電腦群組的預測。

![在 Windows Admin Center，顯示與圖形繪製預測預測功能的 CPU 容量系統 Insights 擴充功能](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>本機的功能
系統的深入解析會完全在本機執行 Windows Server 上。 使用 Windows Server 2019 中引進的新功能，您所有的資料收集、 保存，並直接在您的電腦，可讓您了解預測性分析功能沒有任何雲端連線上進行分析。

您系統的資料儲存在您的電腦上，這項資料分析不需要在雲端中重新定型的預測功能。 與系統的深入解析，可以保留您的電腦上的資料，並仍獲益於預測性分析功能。 

## <a name="get-started"></a>立即開始

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>觀賞這些短片，了解您要開始，並有自信地管理系統的深入解析所需的資訊：[開始使用系統 Insights 在 10 分鐘內](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>需求
使用任何 Windows Server 2019 執行個體上系統深入解析。 它會執行主機和客體機器，在任何的 hypervisor，和任何雲端上。

### <a name="install-system-insights"></a>安裝系統深入解析
>[!IMPORTANT]
>系統 Insights 會收集和最多年份的資料儲存在本機存放區。 如果您想要保留您的資料，當升級您的作業系統，**請確定您使用就地升級**。

#### <a name="install-the-feature"></a>安裝功能
您可以安裝系統內 Windows Admin Center 使用的擴充功能的深入解析：

![系統 Insights 延伸模組的第 0 天體驗。](media/day-0-2.png)

您也可以安裝系統 Insights 直接透過 伺服器管理員藉由新增**系統 Insights**功能，或使用 PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>提供意見反應
我們很樂意聆聽您的意見反應，協助我們改善這項功能。 您可以使用下列管道來提交意見反應：
- **意見反應中樞**:使用 Windows 10 中的意見反應中樞工具，提出問題或意見反應。 當這麼做，請指定：
    - **類別目錄**:Server 
    - **Subcategory**:系統深入解析
- **UserVoice**:提交功能要求，透過我們[UserVoice 頁面](https://windowsserver.uservoice.com/forums/295071-management-tools)。 分享您的同事能附議對您至關重要的項目。
- **Email**：如果您想要提交您的意見反應，私下向功能小組，傳送電子郵件給system-insights-feed@microsoft.com。 請記住，我們仍可能會要求您使用意見反應中樞或 UserVoice。

## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增和開發功能](adding-and-developing-capabilities.md)
- [系統 Insights 常見問題集](faq.md)