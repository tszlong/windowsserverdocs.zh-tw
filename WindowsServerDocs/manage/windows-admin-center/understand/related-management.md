---
title: Windows Admin Center 相關管理解決方案
description: Windows Admin Center 如何與比較和補充其他 Microsoft 監視與管理解決方案/產品 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296710"
---
# Windows Admin Center，並從 Microsoft 的相關的管理解決方案

>適用於：Windows Admin Center、Windows Admin Center 預覽版

[Windows Admin Center](windows-admin-center.md)是傳統的內建的伺服器管理工具的情況下，您可能使用遠端桌面 (RDP) 連線至伺服器以進行疑難排解，或設定的演進。 它有並非用來取代其他現有的 Microsoft 管理解決方案;而是與相輔相成，而這些解決方案，如下所述。

## 遠端伺服器管理工具 (RSAT)

[遠端伺服器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)是一組 GUI 和 PowerShell 來管理選用的角色和功能在 Windows Server 中的工具。 RSAT 有許多 Windows Admin Center 不會有的功能。 我們可能會增加一些最常使用的工具 RSAT 在 Windows Admin center 未來。 任何新的 Windows Server 角色或適用於管理需要 GUI 功能將會在 Windows Admin Center 中。

## Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune)是以雲端為基礎的企業行動力管理服務，可讓您管理 iOS、 Android、 Windows 與 macOS 裝置，根據一組原則。 Intune 著重在讓您能夠用來控制您的工作力如何存取並分享資訊的安全的公司資訊。 相反地，Windows Admin Center 不原則導向，但是可以讓臨機操作管理 Windows 10 和 Windows Server 系統，透過遠端 PowerShell 和 WMI 透過 WinRM。

## Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/)是混合式雲端平台，可讓您從您的資料中心傳遞 Azure 服務。 使用 PowerShell 或系統管理員入口網站，類似於用來存取和管理傳統 Azure 服務的傳統 Azure 入口網站管理 azure Stack。 Windows Admin Center 不用來管理 Azure Stack 基礎結構，但您可以使用它來[管理 Azure IaaS 虛擬機器](../azure/manage-azure-vms.md)（執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012），或針對個別的實體進行疑難排解您的 Azure Stack 環境中部署的伺服器。

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center)是在內部的資料中心適用於管理解決方案部署、 設定、 管理、 監視您的整個資料中心。 System Center 可讓您查看您的環境中的所有系統的狀態，雖然 Windows Admin Center 可讓您向下切入到特定的伺服器，來管理或疑難排解更細微的工具。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **構思工作 「 內建 」 平台 & 工具** | **資料中心管理 & 監視** |
| 隨附於 Windows Server 授權 –**沒有額外的費用**，就像 MMC 和其他傳統的內建工具 | **完整**的套件的額外的值，在您的環境和平台上的解決方案 |
| **輕量型**、 瀏覽器為基礎的遠端管理 Windows Server 執行個體，**任何一處**;RDP 替代項目 | 管理 & 監視器**異質**系統**縮放比例**，包括 HYPER-V 及 VMware，Linux |
|**深入**單一伺服器 & 單一叢集向下切入進行疑難排解，設定 & 維護|佈建; 基礎結構自動化與自助; 基礎結構和監視**廣度**的工作負載|
|最佳化的管理的**個別**2-4 節點**HCI**叢集，整合 HYPER-V、 儲存空間直接存取，以及 SDN|部署 & 管理 HYPER-V、 **datacenter 縮放**比例的 Windows 伺服器叢集，從與 SCVMM**裸機**|
|**監視 HCI 上**只;叢集健康情況服務會將儲存歷程記錄。 第 1 個和第 3 方**管理工具擴充功能**可延伸的平台|**可延伸** & **可調整監視**平台的 SCOM，與警示、 通知、 監視; 第三方工作負載SQL 的歷程記錄|
|最簡單的橋接轉**混合式**;上架，並使用各種不同的資料保護、 複寫、 更新及更多的 Azure 服務|**內建**的資料保護，複寫時，更新 (DPM/VMM/SCCM)。 混合式與整合 Log Analytics 及地圖服務|
|**凸顯了平台功能**的 Windows Server： 存放裝置移轉服務、 儲存體複本、 系統深入解析等等。|**其他平台**： Orchestrator/SMA 中的自動化。使用 SCSM & 其他整合服務的管理工具|

#### 每一個都可提供目標的值各不相同;**一起更好**以補充的功能。
