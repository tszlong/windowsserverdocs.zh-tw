---
title: 使用 Azure 災害復原設定 RDS 的災害復原
description: 了解如何使用 Azure 災害復原設定 RDS 部署的災害復原
ms.author: elizapo
ms.date: 06/12/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 6c0e9b97a436f51babf679d6ce0aa67c09bcfe26
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936903"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>使用 Azure Site Recovery 設定 RDS 的災害復原

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以使用 Azure Site Recovery 為您的遠端桌面服務部署建立災害復原解決方案。

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 是一項以 Azure 為基礎的服務，可協調虛擬機器的複寫、容錯移轉和復原，以提供災害復原功能。 Azure Site Recovery 支援以多種複寫技術持續複寫、保護虛擬機器和應用程式，並順暢地將其容錯移轉至私人/公用或主機服務提供者的雲端。

請使用下列資訊來建立和驗證災害復原解決方案。

## <a name="disaster-recovery-deployment-options"></a>災害復原部署選項

您可以在實體伺服器或執行 Hyper-V 或 VMWare 的虛擬機器上部署 RDS。 Azure Site Recovery 可保護次要站台或 Azure 的內部部署和虛擬部署。 下表列出站台對站台和站台對 Azure 的災害復原案例中支援的不同 RDS 部署。

| 部署類型                          | Hyper-V 站台對站台 | Hyper-V 站台對 Azure | VMWare 站台對 Azure | 實體站台對 Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| 集區虛擬桌面 (未受管理)       |是|否|否|否 |
| 集區虛擬桌面 (受管理、無 UPD) | 是|否|否|否|
| RemoteApp 和桌面工作階段 (無 UPD) | 是|是|是|是  |

## <a name="prerequisites"></a>必要條件

您必須確定已符合下列需求，才能為您的部署設定 Azure Site Recovery：

- 建立[內部部署 RDS 部署](rds-deploy-infrastructure.md)。
- 將 [Azure Site Recovery 服務保存庫](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault)新增至您的 Microsoft Azure 訂用帳戶。
- 如果您要使用 Azure 作為復原網站，請在您的 VM 上執行 [Azure 虛擬機器整備評估工具](https://azure.microsoft.com/downloads/vm-readiness-assessment/)，以確保這些 VM 與 Azure VM 和 Azure Site Recovery 服務相容。

## <a name="implementation-checklist"></a>實作檢查清單

我們日後將詳細說明為 RDS 部署啟用 Azure Site Recovery 服務的各種步驟，但此處僅提供概略的實作步驟。

| **步驟 1 - 設定災害復原的 VM**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - 下載 Microsoft Azure Site Recovery 提供者。 將其安裝在您的 VMM 伺服器或 Hyper-V 主機上。 如需相關資訊，請參閱[使用 Azure Site Recovery 複寫至 Azure 的必要條件](/azure/site-recovery/site-recovery-prereq)。                                                                                                                             |
| VMWare - 設定保護伺服器、設定伺服器和主要目標伺服器                                                                                                                                                      |
| **步驟 2 - 準備您的資源**                                                                                                                                                                                                           |
| 新增 [Azure 儲存體帳戶](/azure/storage/storage-create-storage-account)。                                                                                                                                                                                                              |
| Hyper-V - 下載 Microsoft Azure 復原服務代理程式，並將其安裝在 Hyper-V 主機伺服器上。                                                                                                                                     |
| VMWare - 確定已在所有 VM 上安裝行動服務。                                                                                                                                                                           |
| [為 VMM 雲端、Hyper-V 站台或 VMWare 站台中的 VM 啟用保護](rds-enable-dr-with-asr.md)。                                                                                                                                                                    |
| **步驟 3 - 設計您的復原計劃。**                                                                                                                                                                                                        |
| 將應您的資源 - 將內部部署網路對應至 Azure VNET。                                                                                                                                                                              |
| [建立復原計劃](rds-disaster-recovery-plan.md)。 |
| 建立測試容錯移轉，以測試復原計劃。 請確定所有 VM 皆可存取必要的資源，例如 Active Directory。 確定 RDS 的網路重新導向已設定並運作中。 如需測試復原計劃的詳細步驟，請參閱[執行測試容錯移轉](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **步驟 4 - 執行災害復原演練。**                                                                                                                                                                                                     |
| 使用計劃性與非計劃性容錯移轉執行災害復原演練。 請確定所有 VM 皆有權存取必要的資源，例如 Active Directory。 請確定所有 VM 皆有權存取必要的資源，例如 Active Directory。 如需容錯移轉以及執行演練的詳細步驟，請參閱 [Site Recovery 中的容錯移轉](/azure/site-recovery/site-recovery-failover)。|


