---
title: 使用 Windows 系統管理中心來管理 Azure 更新管理的作業系統更新
description: 使用 Windows 管理中心 (Project 檀香山) 設定 Azure 更新管理來管理作業系統更新。
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 3818e06780ac22f56ed3d44209041f58c1070409
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940059"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>使用 Windows 系統管理中心來管理 Azure 更新管理的作業系統更新

[深入瞭解 Azure 與 Windows 管理中心的整合。](../plan/azure-integration-options.md)

Azure 更新管理是 Azure 自動化中的解決方案，可讓您從單一位置管理多部電腦的更新和修補程式，而不是以每一伺服器為基礎。 透過 Azure 更新管理，您可以快速評估可用更新的狀態、排定必要更新的安裝時間，以及檢閱部署結果來確認是否已成功套用更新。 無論您的電腦是由其他雲端提供者或內部部署裝載的 Azure Vm，都可以這麼做。 [深入瞭解 Azure 更新管理。](https://docs.microsoft.com/azure/automation/automation-update-management)

透過 Windows 系統管理中心，您可以輕鬆地設定和使用 Azure 更新管理，讓受管理的伺服器保持在最新狀態。 如果您的 Azure 訂用帳戶中還沒有 Log Analytics 工作區，Windows 管理中心會自動設定您的伺服器，並在您指定的訂用帳戶和位置中建立必要的 Azure 資源。 如果您有現有的 Log Analytics 工作區，Windows 管理中心可以自動設定您的伺服器，以使用來自 Azure 更新管理的更新。

若要開始使用，請前往伺服器連線中的 [更新] 工具，然後選取 [立即安裝]，並提供相關 Azure 資源的喜好設定。

將伺服器設定為受 Azure 更新管理管理之後，您就可以使用 [更新] 工具中提供的超連結來存取 Azure 更新管理。

[瞭解如何停止使用 Azure 更新管理來更新您的伺服器。](azure-monitor.md#disabling-monitoring)

請注意，在設定 Azure 更新管理之前，您必須[向 azure 註冊您的 Windows 管理中心閘道](../configure/azure-integration.md)。

