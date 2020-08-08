---
title: System Insights 常見問題
description: System Insights 常見問題
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 9f746e71b64497835fc5f0f90e9b46c03b63fd15
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971995"
---
# <a name="system-insights-faq"></a>System Insights 常見問題

>適用於：Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>您要如何搭配 Azure 監視器或 System Center Operations Manager 來使用系統深入解析？

[Azure 監視器](https://azure.microsoft.com/services/monitor/)和[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)可在整個部署中提供操作資訊，以協助您管理基礎結構。 相反地，系統深入解析是引進本機預測性分析功能的 Windows Server 功能。 同時，System Insights 和 Azure 監視器或 SCOM 可以協助您在擴展裝置的情況下呈現預測：

 Azure 監視器或 SCOM 可以關閉 System Insights 所建立的事件，因為系統深入解析會將每個預測的結果輸出至事件記錄檔。 他們可以在一群 Windows 伺服器上呈現這些機器專屬的預測，讓您可以統一地查看一組伺服器實例的這些預測。

 請在[此處](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results)查看每個預測的通道和事件識別碼。

## <a name="how-does-system-insights-relate-to-windows-ml"></a>System Insights 與 Windows ML 有何關聯？

[WINDOWS ML](https://docs.microsoft.com/windows/uwp/machine-learning/)是一個平臺，可讓開發人員在 Windows 裝置上匯入和評分預先定型的機器學習模型。 這些模型受益于硬體加速，而且可以在本機進行評分。

System Insights 是 Windows Server 2019 中的一項功能，可提供本機預測功能以及完整的管理體驗，包括 PowerShell 和 Windows 管理中心整合。

## <a name="can-i-use-system-insights-for-my-cluster"></a>我可以將系統深入解析用於我的叢集嗎？

是。 System Insights 可以獨立地在每個個別的容錯移轉叢集節點上執行，而 System Insights 的預設行為會預測本機儲存體、磁片區、CPU 和網路的使用量。 **您也可以啟用叢集儲存體的預測**，讓儲存體預測功能預測叢集磁片區和儲存體的使用量。

您可以在 Windows 系統管理中心或 PowerShell 中管理這些設定，[這裡](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/)有提供這項功能的詳細資訊。


## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>執行預設功能有多昂貴？

每個預設功能的執行成本都不高。 當您收集更多資料時，每個功能都需要較長的時間來執行，但通常只需幾秒鐘的時間就能完成。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解「系統深入解析」，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
