---
title: 系統 Insights 常見問題集
description: 系統 Insights 常見問題集
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
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851059"
---
# <a name="system-insights-faq"></a>系統 Insights 常見問題集

>適用於：Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>如何使用系統 Insights 與 Azure 監視器或 System Center Operations Manager？

[Azure 監視器](https://azure.microsoft.com/services/monitor/)並[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)提供可協助您管理您的基礎結構部署的操作資訊。 系統的深入解析，相較之下，已引進本機的預測性分析功能的 Windows Server 功能。 在一起，在系統深入解析和 Azure 監視器或 SCOM 可協助跨裝置的母體擴展，呈現預測：

 Azure 監視器 」 或 「 SCOM 可以鍵關閉系統深入解析，所建立的事件系統的深入解析會輸出到事件記錄檔每一項預測的結果。 它們可以呈現這些機器特定預測機隊的 Windows 伺服器，讓您能擁有這些預測的統一的檢視整個伺服器執行個體的群組。 
 
 請參閱每一個預測的通道和事件識別碼[此處](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results)。

## <a name="how-does-system-insights-relate-to-windows-ml"></a>系統的深入解析與 Windows ML 有如何關聯？

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/)是一個平台，可讓開發人員匯入和預先定型的機器學習模型進行評分，在 Windows 裝置上。 這些模型受益於硬體加速功能，以及他們可以在本機將它評分。 

系統的深入解析是中提供本機預測性的功能，以及完整的管理體驗，包括 PowerShell 和 Windows Admin Center 整合的 Windows Server 2019 的功能。 

## <a name="can-i-use-system-insights-for-my-cluster"></a>我可以使用我的叢集系統深入解析嗎？ 

是的。 獨立系統的深入解析可以在每個個別的容錯移轉叢集節點和系統 Insights 預測使用方式的預設行為上執行，在本機儲存體、 磁碟區、 CPU 和網路。 **您也可以啟用叢集的儲存體的預測**，因此儲存預測功能預測叢集磁碟區和儲存體的使用量。 

您可以管理 Windows Admin Center 或 PowerShell 中的這些設定，這項功能的詳細的資訊位於[此處](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/)。
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>需耗費的資源是執行的預設功能？

每個預設功能是金錢來執行。 每一項功能需要較長的時間收集更多資料，但它們通常應該在幾秒內完成。 

## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [系統 Insights 概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增和開發功能](adding-and-developing-capabilities.md)
