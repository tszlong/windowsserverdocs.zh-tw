---
title: 使用 Windows Admin Center 來管理作業系統更新使用 Azure 更新管理
description: 使用 Windows Admin Center (Project Honolulu) 來設定 Azure 更新管理來管理作業系統更新。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296915"
---
# 使用 Windows Admin Center 來管理作業系統更新使用 Azure 更新管理

[深入了解 Azure 與 Windows Admin Center 整合。](../plan/azure-integration-options.md)

Azure 更新管理是在 Azure 自動化中，可讓您管理更新與多部電腦的修補程式，從單一位置，而不是以個別伺服器為基礎的解決方案。 使用 Azure 更新管理，您可以快速地評估可用的更新的狀態、 排程安裝所需的更新，以及檢閱部署結果，以確認已成功套用更新。 這可能是您的機器是否為 Azure Vm，由其他雲端提供者，或在內部部署裝載。 [深入了解 Azure 更新管理。](https://docs.microsoft.com/azure/automation/automation-update-management)

使用 Windows Admin Center，您可以輕鬆地設定，並使用 Azure 更新管理，讓您受管理的伺服器保持在最新狀態。 如果您還沒有 Log Analytics 工作區中您的 Azure 訂閱，Windows Admin Center 將會自動設定您的伺服器，並建立所需的 Azure 資源中的訂閱和您指定的位置。 如果您有現有的 Log Analytics 工作區，Windows Admin Center 可以自動設定您的伺服器以取用來自 Azure 更新管理更新。  

若要開始，請移至 [更新] 工具中的伺服器連接和選取 「 設定現在 」，並提供相關的 Azure 資源的喜好設定。 

一旦您已設定為受 Azure 更新管理您的伺服器，您可以使用超連結在更新工具中提供存取 Azure 更新管理。 

[了解如何停止使用 Azure 更新管理來更新您的伺服器。](azure-monitor.md#disabling-monitoring)

請注意，您必須[註冊您的 Windows Admin Center 閘道與 Azure](..\configure\azure-integration.md)設定 Azure 更新管理之前先。

