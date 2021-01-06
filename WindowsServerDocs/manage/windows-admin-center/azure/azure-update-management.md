---
title: 使用 Windows Admin Center 透過 Azure 更新管理管理作業系統更新
description: 使用 Windows Admin Center (專案 Honolulu) 來設定 Azure 更新管理以管理作業系統更新。
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 74530e4a07dd3cc05d752e15337938ed62aff559
ms.sourcegitcommit: 38664a484b62a5c6342bd5105b814f55ee4b5604
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97917669"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>使用 Windows Admin Center 透過 Azure 更新管理管理作業系統更新

[深入瞭解 Azure 與 Windows Admin Center 整合。](./index.md)

Azure 更新管理是 Azure 自動化中的解決方案，可讓您從單一位置管理多部機器的更新和修補程式，而不是每個伺服器。 透過 Azure 更新管理，您可以快速評估可用更新的狀態、排定必要更新的安裝時間，以及檢閱部署結果來確認是否已成功套用更新。 無論您的電腦是 Azure 虛擬機器 (Vm) 、由其他雲端提供者裝載，還是在內部部署環境，都可能發生這種情況。 [深入瞭解 Azure 更新管理](/azure/automation/update-management/overview)。

有了 Windows Admin Center，您就可以輕鬆地設定及使用 Azure 更新管理，讓受管理的伺服器保持在最新狀態。 如果您的 Azure 訂用帳戶中還沒有 Log Analytics 工作區，Windows Admin Center 將會自動設定您的伺服器，並在您指定的訂用帳戶和位置中建立必要的 Azure 資源。 如果您有現有的 Log Analytics 工作區，Windows Admin Center 可以自動將您的伺服器設定為使用 Azure 更新管理的更新。

若要開始使用，請移至伺服器連線中的 [更新] 工具，然後選取 [立即設定]，並為相關的 Azure 資源提供您的喜好設定。

當您將伺服器設定為受 Azure 更新管理管理之後，您就可以使用 [更新] 工具中提供的超連結來存取 Azure 更新管理。

[瞭解如何停止使用 Azure 更新管理來更新您的伺服器。](azure-monitor.md#disabling-monitoring)

請注意，在設定 Azure 更新管理之前，您必須先向 [Azure 註冊 Windows Admin Center 閘道](./azure-integration.md) 。