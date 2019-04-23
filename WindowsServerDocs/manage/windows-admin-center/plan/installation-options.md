---
title: 安裝類型是最適合您
description: 安裝類型是最適合您的 Windows Admin Center （專案檀香山）。 在叢集上安裝容錯移轉的高可用性和恢復功能。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fae0305e454cdd10109219c6182ff612f539e9c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868009"
---
# <a name="what-type-of-installation-is-right-for-you"></a>什麼類型的安裝最適合您？

>適用於：Windows Admin Center，Windows Admin Center 預覽

## <a name="supported-operating-systems-installation"></a>支援的作業系統：安裝

您可以**安裝**下列 Windows 作業系統上的 Windows Admin Center:

| **版本** | **安裝模式** |
|-------------|-----------------------|
|Windows 10 版本 1709 或更新版本 | 桌面模式 |
|Windows Server 半年通道 | 閘道模式 |
|Windows Server 2016 | 閘道模式 |
|Windows Server 2019 | 閘道模式 |

**桌面模式：** 從 [開始] 功能表啟動，並從其安裝所在的同一部電腦連接至 Windows Admin Center 閘道 (也就是`https://localhost:6516`)

**閘道模式：** 從一部電腦上的用戶端瀏覽器連線到 Windows Admin Center 閘道 (也就是`https://servername.contoso.com`) 

> [!WARNING]
> 不支援在網域控制站上安裝 Windows Admin Center。 [深入的了解網域控制站安全性最佳做法](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。 

> [!IMPORTANT]
> 您無法使用 Internet Explorer 來管理 Windows Admin Center-您需要改為使用[支援的瀏覽器](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
)。  如果您要在 Windows Server 上安裝 Windows Admin Center，我們建議連接 Windows 10 和 Edge 的遠端管理。  或者，您可以管理在本機 Windows Server 上如果您已安裝支援的瀏覽器。

## <a name="supported-operating-systems-management"></a>支援的作業系統：Management

您可以**管理**下列 Windows 作業系統，使用 Windows Admin Center:

| 版本 | 管理*節點*透過*伺服器管理員* | 管理*叢集*透過*容錯移轉叢集管理員* | 管理*HCI*透過*HCI 叢集管理員*|
|-------------------------|---------------|-----|------------------------|
| Windows 10 版本 1709 或更新版本 | 是 （透過電腦管理） | N/A | N/A |
| Windows Server 半年通道 | 是 | 是 | N/A |
| Windows Server 2019 | 是 | 是 | 是 |
| Windows Server 2016 | 是 | 是 | 是，利用[最新累積更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 | N/A |
| Windows Server 2012 R2 | 是 | 是 | N/A |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 | N/A |
| Windows Server 2012 | 是 | 是 | N/A |
| Windows Server 2008 R2 | 是，有限的功能 | N/A | N/A |

> [!NOTE]
> Windows Admin Center 要求不包含在 Windows Server 2008 R2，2012年和 2012 R2 中的 PowerShell 功能。 如果您將會管理這些項目與 Windows Admin Center，您必須在這些伺服器上安裝 Windows Management Framework (WMF) 5.1 或更新版本。

>在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。 

>如果未安裝 WMF，您可以[下載 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="deployment-options"></a>部署選項

| ![i m g](../media/deployment-options/W10.png) | ![i m g](../media/deployment-options/gateway.png) | ![i m g](../media/deployment-options/node.png) | ![i m g](../media/deployment-options/HA.png) |
|---|---|---|---|

| 本機用戶端 | 閘道伺服器 | 受管理伺服器 | 容錯移轉叢集 |
| --- | --- | --- | --- |
| 在本機的 Windows 10 用戶端可連線至受管理的伺服器上安裝。  適用於快速入門中，測試，臨機操作或小規模案例。 |在指定的閘道伺服器上安裝和存取從任何用戶端瀏覽器連線到閘道伺服器。  適用於大規模的案例。 | 直接在受管理的伺服器，以管理本身或它的成員節點的叢集上安裝。  適用於分散式案例。 | 若要啟用高可用性閘道服務的容錯移轉叢集中部署。 適用於生產環境，以確保您的管理服務復原功能。 |

## <a name="high-availability"></a>高可用性

您可以藉由部署的容錯移轉叢集的主動-被動模型中的 Windows Admin Center 讓閘道服務的高可用性。 如果其中一個叢集中的節點失敗，Windows Admin Center 依正常程序會容錯移轉至另一個節點，可讓您繼續順暢地管理您的環境中的伺服器。

[了解如何部署 Windows Admin Center 具高可用性。](../deploy/high-availability.md)

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)
