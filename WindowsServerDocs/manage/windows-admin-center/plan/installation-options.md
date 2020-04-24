---
title: 什麼類型的安裝最適合您
description: 本主題說明 Windows Admin Center 的不同安裝選項，包括在 Windows 10 電腦上安裝，或是在供多個系統管理員使用的 Windows Server 上安裝。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: bd7ec8a5a072cbda99b036718d24ec1908fb8b53
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "81269245"
---
# <a name="what-type-of-installation-is-right-for-you"></a>什麼類型的安裝最適合您？

本主題說明 Windows Admin Center 的不同安裝選項，包括在 Windows 10 電腦上安裝，或是在供多個系統管理員使用的 Windows Server 上安裝。 若要在 Azure 中的 VM 上安裝 Windows Admin Center，請參閱＜[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)＞。

## <a name="installation-types"></a>安裝:類型

![img](../media/deployment-options/install-options.PNG)

| 本機用戶端                                | 閘道伺服器                                  | 受管理的伺服器                               | 容錯移轉叢集                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [安裝](../deploy/install.md)在可連線至受管理伺服器的本機 Windows 10 用戶端上。  適用於快速入門、測試、隨選或小規模案例。 |[安裝](../deploy/install.md)在指定的閘道伺服器上，並從任何可連線至閘道伺服器的用戶端瀏覽器進行存取。  適用於大規模案例。 | 直接[安裝](../deploy/install.md)在受管理的伺服器上，用以管理其本身或其為成員節點的所屬叢集。  適用於分散式案例。 | [部署](#high-availability)在容錯移轉叢集中，以啟用閘道服務的高可用性。 適用於生產環境，以確保管理服務的復原能力。 |

## <a name="installation-supported-operating-systems"></a>安裝:支援的作業系統

您可以在下列 Windows 作業系統中**安裝** Windows Admin Center：

| **平台**                       | **安裝模式** |
| -----------------------------------| --------------------- |
| Windows 10                         | 本機用戶端 |
| Windows Server 半年通道 | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |
| Windows Server 2016                | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |
| Windows Server 2019                | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |

若要操作 Windows Admin Center：

- **在本機用戶端案例中：** 從 [開始] 功能表啟動 Windows Admin Center 閘道，然後藉由存取 `https://localhost:6516` 從用戶端的網頁瀏覽器連線至該閘道。
- **在其他案例中：** 從用戶端瀏覽器透過 URL (例如 `https://servername.contoso.com`) 連線至不同電腦上的 Windows Admin Center 閘道。

> [!WARNING]
> 不支援在網域控制站上安裝 Windows Admin Center。 [深入了解網域控制站安全性最佳做法](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。

## <a name="installation-supported-web-browsers"></a>安裝:支援的網頁瀏覽器

Microsoft Edge (包括 [Microsoft Edge Insider](https://microsoftedgeinsider.com)) 和 Google Chrome 已在 Windows 10 上測試過且受到支援。 其他網頁瀏覽器 (包括 Internet Explorer 和 Firefox) 目前並未納入我們的測試矩陣中，因此不受「正式」  支援。 這些瀏覽器在執行 Windows Admin Center 時可能會發生問題。 例如，Firefox 有其本身的憑證存放區，因此您必須將 `Windows Admin Center Client` 憑證匯入至 Firefox，才能在 Windows 10 上使用 Windows Admin Center。 如需詳細資訊，請參閱[瀏覽器特有的已知問題](../support/known-issues.md#browser-specific-issues)。

## <a name="management-target-supported-operating-systems"></a>管理目標：支援的作業系統

您可以使用 Windows Admin Center 來**管理**下列 Windows 作業系統：

| 版本 | 透過*伺服器管理員*來管理*節點* | 透過*叢集管理員*進行管理 |
| ------------------------- |--------------- | ----- |
| Windows 10 | 是 (透過電腦管理) | 不適用 |
| Windows Server 半年通道 | 是 | 是 |
| Windows Server 2019 | 是 | 是 |
| Windows Server 2016 | 是 | 是，使用[最新的累計更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 |
| Windows Server 2012 R2 | 是 | 是 |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 |
| Windows Server 2012 | 是 | 是 |

> [!NOTE]
> Windows Admin Center 需要 Windows Server 2012 和 2012 R2 中未包含的 PowerShell 功能。 如果您將使用 Windows Admin Center 來管理這些平台，您必須在這些伺服器上安裝 Windows Management Framework (WMF) 5.1 版或更新版本。
> 
> 請在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF，且版本是 5.1 或更新版本。 
> 
> 如果尚未安裝 WMF，您可以[下載 WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。

## <a name="high-availability"></a>高可用性

您可以在容錯移轉叢集的主動-被動模型中部署 Windows Admin Center，藉以啟用閘道服務的高可用性。 如果叢集中的其中一個節點失敗，則 Windows Admin Center 會適當地容錯移轉至另一個節點，讓您繼續順暢地管理環境中的伺服器。

[了解如何部署具有高可用性的 Windows Admin Center。](../deploy/high-availability.md)

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)
