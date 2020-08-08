---
title: 系統深入解析概觀
description: System Insights 是 Windows Server 2019 中新的預測性分析功能。 System Insights 預測功能-每個都是由機器學習模型所支援-在本機分析 Windows Server 系統資料（例如效能計數器和事件），讓您深入瞭解伺服器的運作，並協助您降低與被動管理部署中問題相關聯的營運費用。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 9bedd593cdd26b67e6e16ddea73955bb926a87a5
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996681"
---
# <a name="system-insights-overview"></a>系統深入解析概觀

>適用於：Windows Server 2019

System Insights 是 Windows Server 2019 中新的預測性分析功能。 System Insights 預測功能-每個都是由機器學習模型所支援-在本機分析 Windows Server 系統資料（例如效能計數器和事件），讓您深入瞭解伺服器的運作，並協助您降低與被動管理部署中問題相關聯的營運費用。

在 Windows Server 2019 中，System Insights 隨附四個預設功能，著重于容量預測，根據您先前的使用模式來預測計算、網路和儲存體的未來資源。 System Insights 也隨附可延伸的[基礎結構](adding-and-developing-capabilities.md)，因此 Microsoft 和協力廠商可以將新的預測功能新增至系統深入解析，而不需要更新作業系統。

您可以透過直覺的[Windows 管理中心](../windows-admin-center/overview.md)延伸模組或[直接透過 PowerShell](https://aka.ms/SystemInsightsPowerShell)來管理系統深入解析，而系統深入解析可讓您根據部署的需求，分別設定每個預測功能。 所有的預測結果都會發佈至事件記錄檔，讓您可以使用[Azure 監視器](https://azure.microsoft.com/services/monitor/)或[System Center Operations Manager](/system-center/scom/welcome?view=sc-om-1807) ，輕鬆地匯總並查看一組電腦的預測。

![Windows 系統管理中心的 System Insights 延伸模組，以圖表繪製預測來顯示 CPU 容量預測功能](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>本機功能
System Insights 完全在 Windows Server 本機上執行。 使用 Windows Server 2019 引進的新功能，您的所有資料都會在您的電腦上直接收集、保存和分析，讓您能夠在沒有任何雲端連線的情況下，實現預測性分析功能。

系統資料會儲存在您的電腦上，而這項資料是由不需要在雲端重新定型的預測性功能進行分析。 透過 System Insights，您可以將資料保留在您的電腦上，而且仍然受益于預測性分析功能。

## <a name="get-started"></a>開始使用

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>觀賞這些短片，以瞭解開始使用和安心管理系統深入解析所需的資訊： [10 分鐘內的系統深入](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)解析使用者入門

### <a name="requirements"></a>需求
System Insights 適用于任何 Windows Server 2019 實例。 它會在主機和來賓機器、任何虛擬機器上，以及任何雲端中執行。

### <a name="install-system-insights"></a>安裝 System Insights
>[!IMPORTANT]
>System Insights 會收集最多一年的資料，並將其儲存在本機。 如果您想要在升級作業系統時保留資料，**請務必使用就地升級**。

#### <a name="install-the-feature"></a>安裝功能
您可以使用 Windows 系統管理中心內的擴充功能來安裝 System Insights：

![System Insights 擴充功能的第0天體驗。](media/day-0-2.png)

您也可以藉由新增 [**系統**深入解析] 功能或使用 PowerShell，直接透過伺服器管理員安裝系統深入解析：

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>提供意見反應
我們很樂意聽到您的意見反應，協助我們改善這項功能。 您可以使用下列通道來提交意見反應：
- **意見反應中樞**：使用 Windows 10 中的意見反應中樞工具來提出 bug 或意見反應。 當您這麼做時，請指定：
    - **類別**：伺服器
    - **子類別**： System Insights
- **Uservoice**：透過我們的[uservoice 頁面](https://windowsserver.uservoice.com/forums/295071-management-tools)提交功能要求。 與您的同事共用，以附議對您很重要的專案。
- **電子郵件**：如果您想要私下向功能小組提交意見反應，請將電子郵件傳送至 system-insights-feed@microsoft.com 。 請記住，我們仍會要求您使用意見反應中樞或 UserVoice。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解「系統深入解析」，請使用下列資源：

- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [System Insights 常見問題](faq.md)