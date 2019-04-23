---
title: 設定為使用 Azure 的災害復原的 RDS 的災害復原
description: 了解如何使用 RDS 部署進行災害復原的 Azure 災害復原
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878169"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>設定為使用 Azure Site Recovery 的 RDS 的災害復原

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用 Azure Site Recovery 建立的災害復原解決方案，為您的遠端桌面服務部署。 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview)是以 Azure 為基礎的服務，可提供災害復原功能，由協調複寫、 容錯移轉和復原的虛擬機器。 Azure Site Recovery 支援多種複寫技術來持續複寫、 保護，並順暢地容錯移轉虛擬機器和應用程式至私密/公用或主機服務提供者的雲端。 

使用下列資訊來建立和驗證的災害復原解決方案。

## <a name="disaster-recovery-deployment-options"></a>災害復原部署選項

您可以在執行 HYPER-V 或 VMWare 虛擬機器或實體伺服器上部署 RDS。 Azure Site Recovery 可以保護內部部署和虛擬部署至次要網站或 Azure。 下表顯示不同支援 RDS 部署中的站對站和站台至 Azure 的災害 recvoery 案例。

| 部署類型                          | HYPER-V 站台對站 | HYPER-V 網站至 Azure | VMWare 站台-Azure | 實體是 「 站台-Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| (Unmanaged) 的集區虛擬桌面       |是|否|否|否 |
| 集區虛擬桌面 （受控沒有 UPD） | 是|否|否|否|
| Remoteapp 和桌面工作階段 (沒有 UPD) | 是|是|是|是  |

## <a name="prerequisites"></a>先決條件

您可以設定 Azure Site Recovery 部署之前，請確定您符合下列需求：

- 建立[內部部署上的 RDS 部署](rds-deploy-infrastructure.md)。
- 新增[Azure Site Recovery 服務保存庫](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault)您 Microsoft Azure 訂用帳戶。
- 如果您要使用 Azure 作為復原網站，執行[Azure 虛擬機器整備評估工具](https://azure.microsoft.com/downloads/vm-readiness-assessment/)您要確保其與 Azure Vm 和 Azure Site Recovery 服務相容的 Vm 上。
 
## <a name="implementation-checklist"></a>實作檢查清單

我們將討論的各種不同的步驟來啟用 Azure Site Recovery 服務，為您的 RDS 部署的詳細資料，但以下是高階的實作步驟。

| **步驟 1-設定 Vm 的災害復原**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V-下載 Microsoft Azure Site Recovery 提供者。 請將它安裝在您的 VMM 伺服器或 HYPER-V 主機。 請參閱[使用 Azure Site Recovery 複寫至 Azure 的必要條件](/azure/site-recovery/site-recovery-prereq)的資訊。                                                                                                                             |
| VMWare-設定保護伺服器、 設定伺服器和主要目標伺服器                                                                                                                                                      |
| **步驟 2-準備您的資源**                                                                                                                                                                                                           |
| 新增[Azure 儲存體帳戶](/azure/storage/storage-create-storage-account)。                                                                                                                                                                                                              |
| Hyper-V-下載 Microsoft Azure 復原服務代理程式，並將它安裝在 HYPER-V 主機伺服器上。                                                                                                                                     |
| VMWare-請確定所有的 Vm 上安裝行動服務。                                                                                                                                                                           |
| [為在 VMM 雲端、 HYPER-V 網站或在 VMWare 站台的 Vm 啟用保護](rds-enable-dr-with-asr.md)。                                                                                                                                                                    |
| **步驟 3： 設計您的復原方案。**                                                                                                                                                                                                        |
| 將您的資源-對應到 Azure Vnet 的內部部署網路對應。                                                                                                                                                                              |
| [建立復原計劃](rds-disaster-recovery-plan.md)。 |
| 建立測試容錯移轉，以測試復原計劃。 請確定所有 Vm 可以都存取必要的資源，像是 Active Directory。 請確定已重新導向的網路和 RDS 的工作 如需測試您的復原方案的詳細步驟，請參閱[執行測試容錯移轉](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **步驟 4： 執行災害復原演練。**                                                                                                                                                                                                     |
| 執行災害復原演練，使用計劃性與非計劃性容錯移轉。 請確定所有的 Vm 有存取必要的資源，例如 Active Directory。 請確定所有的 Vm 有存取必要的資源，例如 Active Directory。 如需容錯移轉，以及如何執行演練的詳細步驟，請參閱[Site Recovery 中的容錯移轉](/azure/site-recovery/site-recovery-failover)。|


