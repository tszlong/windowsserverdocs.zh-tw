---
title: Windows Admin Center 的相關管理解決方案
description: Windows Admin Center 與其他 Microsoft 監視和管理解決方案/產品 (Project Honolulu) 有何不同和補強
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d681e5007cd3ae3c14de774df0bc85abc23b51d7
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371682"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center 與相關的 Microsoft 管理解決方案

>適用於：Windows Admin Center、Windows Admin Center 預覽版

[Windows Admin Center](windows-admin-center.md) 是傳統內建伺服器管理工具的進化，適用於您可能使用了遠端桌面 (RDP) 連線至伺服器以進行疑難排解或設定的情況。 其目的並不是要取代其他現有的 Microsoft 管理解決方案，而是要補強這些解決方案，如下所述。

## <a name="remote-server-administration-tools-rsat"></a>遠端伺服器管理工具 (RSAT)

[遠端伺服器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) 是 GUI 和 PowerShell 工具的集合，可管理 Windows Server 中的選用角色和功能。 RSAT 有許多 Windows Admin Center 所沒有的功能。 未來我們可能會將 RSAT 中最常用的一些工具新增至 Windows Admin Center。 任何需要以 GUI 管理的新 Windows Server 角色或功能，都將納入 Windows Admin Center 中。

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) 是一項雲端式企業行動管理服務，可讓您根據一組原則來管理 iOS、Android、Windows 和 macOS 裝置。 Intune 的主要目標是要讓您控制工作人員存取和共用資訊的方式，藉以保護公司資訊。 相反地，Windows Admin Center 並非以原則為導向，但可讓您使用遠端 PowerShell 和 WMI over WinRM 進行 Windows 10 和 Windows Server 系統的臨機操作管理。

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) 是混合式雲端平台，可讓您從資料中心提供 Azure 服務。 Azure Stack 可使用 PowerShell 或系統管理員入口網站來進行管理，類似於用來存取和管理傳統 Azure 服務的傳統 Azure 入口網站。 Windows Admin Center 的主要用途並非管理 Azure Stack 基礎結構，但您可以用它來[管理 Azure IaaS 虛擬機器](../azure/manage-azure-vms.md) (執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012)，或是對部署在 Azure Stack 環境中的個別實體伺服器進行疑難排解。

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) 是一個內部部署資料中心管理解決方案，可用來部署、設定、管理及監視您的整個資料中心。 System Center 可讓您查看環境中所有系統的狀態，而 Windows Admin Center 則可讓您向下切入特定伺服器，以使用更精細的工具對其進行管理或疑難排解。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **重新構思的「內建」平台和工具** | **資料中心管理和監視** |
| 隨附於 Windows Server 授權 – **無需額外費用**，如同 MMC 和其他傳統內建工具 | **完整**的解決方案套件可為您的環境和各個平台帶來附加價值 |
| **隨處**對 Windows Server 執行個體進行以瀏覽器為基礎的**輕量型**遠端管理；這是 RDP 的替代方式 | **大規模**管理和監視**異質**系統，包括 Hyper-V、VMware 和 Linux |
|**深層**的單一伺服器與單一叢集向下切入，以進行疑難排解、設定與維護|基礎結構佈建；自動化和自助式；基礎結構和工作負載監視**廣度**|
|**個別**的 2–4 節點 **HCI** 叢集的最佳化管理，整合 Hyper-V、儲存空間直接存取與 SDN|透過 SCVMM 從**裸機**部署和管理**資料中心規模**的 Hyper-V、Windows Server 叢集|
|僅**監視 HCI**；叢集健康情況服務儲存歷程記錄。 第一方和第三方**系統管理工具擴充功能**的可延伸平台|SCOM 中的**可延伸** & **可擴充監視**平台，具有警示、通知、第三方工作負載監視、採用 SQL 的歷程記錄|
|最簡單的**混合式**橋接；內建並使用各種 Azure 服務以進行資料保護、複寫、更新等作業|**內建**資料保護、複寫、更新 (DPM/VMM/SCCM)。 使用 Log Analytics 與服務對應的混合式整合|
|瞭解 Windows Server 的**平臺功能**：儲存體遷移服務、儲存體複本、系統深入解析等。|**其他平臺**： ORCHESTRATOR/SMA 中的自動化。與 SCSM & 其他服務管理工具整合|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>兩者可個別提供既定的價值；搭配使用可藉由互補功能**相輔相成**。
