---
title: 系統深入解析常見問題
description: 系統深入解析常見問題
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: f522623fa88c5ea979d405be013fa26937c658f1
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879827"
---
# <a name="system-insights-faq"></a>系統深入解析常見問題

>適用於：Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>您如何搭配 Azure 監視器或 System Center Operations Manager 來使用系統深入解析？

[Azure 監視器](https://azure.microsoft.com/services/monitor/) 和 [System Center Operations Manager](/system-center/scom/welcome?view=sc-om-1807&preserve-view=true) 在您的部署中提供操作資訊，以協助您管理基礎結構。 相較之下，「系統深入解析」是一項 Windows Server 功能，它引進了本機的預測性分析功能。 系統深入解析和 Azure 監視器或 SCOM 可搭配使用，以協助在各裝置人口之間呈現預測：

 Azure 監視器或 SCOM 可以關閉系統深入解析所建立的事件，因為系統深入解析會將每個預測的結果輸出到事件記錄檔。 他們可以在一群 Windows 伺服器上呈現這些電腦專屬的預測，讓您能夠在一組伺服器實例之間統一查看這些預測。

 請在 [這裡](./managing-capabilities.md#retrieving-capability-results)查看每個預測的通道和事件識別碼。

## <a name="how-does-system-insights-relate-to-windows-ml"></a>系統深入解析與 Windows ML 有何關聯？

[WINDOWS ML](/windows/uwp/machine-learning/) 是一種平臺，可讓開發人員在 Windows 裝置上匯入預先定型的機器學習模型，並對其進行評分。 這些模型可受益于硬體加速，並可在本機進行評分。

「系統深入解析」是 Windows Server 2019 中的一項功能，可提供本機預測功能以及完整的管理體驗，包括 PowerShell 和 Windows Admin Center 整合。

## <a name="can-i-use-system-insights-for-my-cluster"></a>我可以針對叢集使用系統深入解析嗎？

是。 系統深入解析可以在每個個別的容錯移轉叢集節點上獨立執行，而系統深入解析的預設行為則是在本機儲存體、磁片區、CPU 和網路上使用。 **您也可以針對叢集儲存體啟用預測**，讓儲存體預測功能預測叢集磁片區和儲存體的使用量。

您可以在 Windows Admin Center 或 PowerShell 中管理這些設定， [此處](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/)提供有關這項功能的詳細資訊。


## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>執行預設功能有多高？

每個預設功能的執行成本低廉。 當您收集更多資料時，每項功能都需要較長的時間來執行，但它們通常只需要幾秒鐘的時間即可完成。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解系統深入解析，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
