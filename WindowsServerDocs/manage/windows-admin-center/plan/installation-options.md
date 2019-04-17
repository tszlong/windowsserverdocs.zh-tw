---
title: 什麼類型的安裝最適合您
description: 本主題描述 Windows Admin Center，包括 Windows 10 電腦或使用 Windows 伺服器上安裝多個系統管理員在不同的安裝選項。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296660"
---
# 什麼類型的安裝最適合您？

>適用於：Windows Admin Center、Windows Admin Center 預覽版

本主題描述 Windows Admin Center，包括 Windows 10 電腦或使用 Windows 伺服器上安裝多個系統管理員在不同的安裝選項。 若要在 Azure VM 上安裝 Windows Admin Center，請參閱[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)。

## 支援的作業系統： 安裝

您可以**安裝**Windows Admin Center 在下列 Windows 作業系統：

| **版本** | **安裝模式** |
|-------------|-----------------------|
|Windows 10 版本 1709 或更新版本 | 桌面模式 |
|Windows Server 半年通道 | 閘道模式 |
|Windows Server 2016 | 閘道模式 |
|Windows Server 2019 | 閘道模式 |

**桌面模式：** 從 [開始] 功能表啟動，並從安裝所在的同一部電腦連線至 Windows Admin Center 閘道 (亦即`https://localhost:6516`)

**閘道模式：** 從不同的電腦上的用戶端瀏覽器連線至 Windows Admin Center 閘道 (亦即`https://servername.contoso.com`) 

> [!WARNING]
> 不支援在網域控制站上安裝 Windows Admin Center。 [閱讀更多有關網域控制站安全性最佳做法](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。 

> [!IMPORTANT]
> 您無法使用 Internet Explorer 來管理 Windows Admin Center-您需要改為使用[支援的瀏覽器](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
)。  如果您 Windows Server 上安裝 Windows Admin Center，我們建議管理 Windows 10 和 Edge 遠端連線。  或者，您可以在本機上管理 Windows Server 如果您已安裝支援的瀏覽器。

## 支援的作業系統： 管理

您可以**管理**下列 Windows 作業系統，使用 Windows Admin Center:

| 版本 | 管理透過*伺服器管理員**節點* | 管理*叢集*透過*容錯移轉叢集管理員* | 管理*HCI*透過*HCI 叢集管理員*|
|-------------------------|---------------|-----|------------------------|
| Windows 10 版本 1709 或更新版本 | 是 （透過電腦管理） | 無 | 無 |
| Windows Server 半年通道 | 是 | 是 | 無 |
| Windows Server 2019 | 是 | 是 | 是 |
| Windows Server 2016 | 是 | 是 | 是，搭配[最新累積更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 | 無 |
| Windows Server 2012 R2 | 是 | 是 | 無 |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 | 無 |
| Windows Server 2012 | 是 | 是 | 無 |
| Windows Server 2008 R2 | \ [是，受限制的功能 | 無 | 無 |

> [!NOTE]
> Windows Admin Center 需要 Windows Server 2008 R2、 2012年和 2012 R2 中未包含的 PowerShell 功能。 如果您將會管理這些使用 Windows Admin Center，您將需要在這些伺服器上安裝 Windows Management Framework (WMF) 5.1 版或更高版本。

>在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。 

>如果未安裝 WMF，您可以[下載 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## 部署選項

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| 本機用戶端 | 閘道伺服器 | 受管理的伺服器 | 容錯移轉叢集 |
| --- | --- | --- | --- |
| 在本機的 Windows 10 用戶端可以連線至受管理的伺服器上安裝。  適合用於快速入門，測試，臨機操作或小型的縮放比例的案例。 |指定的閘道伺服器上安裝，並從任何用戶端瀏覽器存取連線到閘道伺服器。  適合用於大規模的案例。 | 在受管理的伺服器，為了管理本身或很成員節點的叢集上直接安裝。  適合用於分散式案例。 | 若要啟用高可用性閘道服務的容錯移轉叢集中部署。 適合用於生產環境，以確保您的管理服務的可復原性。 |

## 高可用性

您可以藉由在容錯移轉叢集上主動被動模式中部署 Windows Admin Center 啟用高可用性的閘道服務。 如果其中一個叢集節點失敗時，Windows Admin Center 正常容錯移轉至另一個節點，讓您繼續順暢地管理您的環境中的伺服器。

[了解如何使用高可用性部署 Windows Admin Center。](../deploy/high-availability.md)

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)
