---
title: 系統深入解析概觀
description: System Insights 是 Windows Server 2019 中的新預測性分析功能。 系統深入解析預測功能-每個都是由機器學習模型所支援-在本機分析 Windows Server 系統資料（例如效能計數器和事件）可讓您深入瞭解伺服器的運作狀況，並協助您降低與被動管理部署問題相關聯的營運費用。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 0fe40bd568a043f4eec9bfc86f9e1a537c07f038
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766851"
---
# <a name="system-insights-overview"></a>系統深入解析概觀

>適用於：Windows Server 2019

System Insights 是 Windows Server 2019 中的新預測性分析功能。 系統深入解析預測功能-每個都是由機器學習模型所支援-在本機分析 Windows Server 系統資料（例如效能計數器和事件）可讓您深入瞭解伺服器的運作狀況，並協助您降低與被動管理部署問題相關聯的營運費用。

在 Windows Server 2019 中，系統深入解析提供四個預設功能，著重于容量預測、根據您先前的使用模式來預測未來的計算、網路和儲存體資源。 System Insights 也隨附可延伸的 [基礎結構](adding-and-developing-capabilities.md)，因此 Microsoft 和協力廠商可以在不更新作業系統的情況下，將新的預測功能新增至系統深入解析。

您可以透過直覺的 [Windows Admin Center](../windows-admin-center/overview.md) 延伸模組或 [直接透過 PowerShell](/powershell/module/systeminsights/)來管理系統深入解析，而系統深入解析可讓您根據部署的需求，分別設定每個預測功能。 所有預測結果都會發佈至事件記錄檔，可讓您使用 [Azure 監視器](https://azure.microsoft.com/services/monitor/) 或 [System Center Operations Manager](/system-center/scom/welcome?view=sc-om-1807) ，輕鬆地匯總和查看一組電腦的預測。

![Windows Admin Center 中的系統深入解析延伸模組，顯示 CPU 容量預測功能與繪製預測的圖表](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>本機功能
系統深入解析會完全在 Windows Server 本機上執行。 使用 Windows Server 2019 中引進的新功能，您的所有資料都會在您的電腦上直接收集、保存及分析，讓您能夠在沒有任何雲端連線的情況下實現預測性分析功能。

您的系統資料會儲存在您的電腦上，而這項資料是由不需要在雲端中重新定型的預測功能進行分析。 使用系統深入解析時，您可以將資料保留在您的電腦上，但仍可受益于預測性分析功能。

## <a name="get-started"></a>開始使用

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>觀看這些短片以瞭解開始使用和安心管理系統深入解析所需的資訊： [10 分鐘內開始使用系統深入](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)解析

### <a name="requirements"></a>規格需求
任何 Windows Server 2019 實例都有提供系統深入解析。 它會在主機和來賓機器、任何虛擬程式和任何雲端上執行。

### <a name="install-system-insights"></a>安裝系統深入解析
>[!IMPORTANT]
>系統深入解析會在本機收集並儲存最多一年的資料。 如果您想要在升級作業系統時保留資料， **請務必使用就地升級**。

#### <a name="install-the-feature"></a>安裝功能
您可以使用 Windows Admin Center 中的擴充功能安裝系統深入解析：

![System Insights 擴充功能的第0天體驗。](media/day-0-2.png)

您也可以藉由新增 **系統深入** 解析功能，或使用 PowerShell，直接透過伺服器管理員安裝系統深入解析：

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>提供意見反應
歡迎您提供意見反應，協助我們改進這項功能。 您可以使用下列通道來提交意見反應：
- **意見反應中樞**：使用 Windows 10 中的意見反應中樞工具來提出 bug 或意見反應。 當您這樣做時，請指定：
    - **Category**： Server
    - **子類別**：系統深入解析
- **Uservoice**：透過我們的 [UserVoice 頁面](https://windowsserver.uservoice.com/forums/295071-management-tools)提交功能要求。 與您的同事共用，以附議對您很重要的專案。
- **電子郵件**：如果您想要私下提交意見反應給功能小組，請傳送電子郵件至 system-insights-feed@microsoft.com 。 請記住，我們仍然會要求您使用意見反應中樞或 UserVoice。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解系統深入解析，請使用下列資源：

- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [系統深入解析常見問題](faq.md)