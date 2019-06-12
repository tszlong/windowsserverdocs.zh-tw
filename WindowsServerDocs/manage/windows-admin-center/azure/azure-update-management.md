---
title: 使用 Windows Admin Center 來管理 Azure 更新管理的作業系統更新
description: 使用 Windows Admin Center （專案檀香山） 來設定 Azure 更新管理來管理 OS 更新。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452538"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>使用 Windows Admin Center 來管理 Azure 更新管理的作業系統更新

[深入了解 Azure 與 Windows Admin Center 整合。](../plan/azure-integration-options.md)

Azure 更新管理是可讓您從單一位置，而不是在每個伺服器上管理更新和修補程式的多部機器的 Azure 自動化中的解決方案。 透過 Azure 更新管理，可以快速評估可用更新的狀態、 排定何時安裝必要的更新，並檢閱部署結果來確認已成功套用的更新。 這可能是您的電腦是否裝載其他雲端提供者，或在內部部署的 Azure Vm。 [深入了解 Azure 更新管理。](https://docs.microsoft.com/azure/automation/automation-update-management)

使用 Windows Admin Center，您可以輕鬆地設定，並使用 Azure 更新管理來保持最新的受管理的伺服器。 如果您還沒有 Log Analytics 工作區中您 Azure 訂用帳戶，Windows Admin Center 會自動設定您的伺服器，並建立所需的 Azure 資源訂用帳戶和您指定的位置。 如果您有現有的 Log Analytics 工作區時，Windows Admin Center 可以自動設定您的伺服器，來取用來自 Azure 更新管理的更新。  

若要開始，請移至 伺服器連接中的 更新 工具和選取 「 向上現在已設定 」，並提供相關的 Azure 資源的喜好設定。 

一旦您設定您的伺服器，由 Azure 更新管理來管理，您可以使用 [更新] 工具中提供的超連結來存取 Azure 更新管理。 

[了解如何停止使用 Azure 更新管理，以更新您的伺服器。](azure-monitor.md#disabling-monitoring)

請注意，您必須使用[向 Azure 註冊您的 Windows Admin Center 閘道](../configure/azure-integration.md)之前先設定 Azure 更新管理。

