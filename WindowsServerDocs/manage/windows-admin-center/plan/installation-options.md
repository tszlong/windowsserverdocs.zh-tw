---
title: 最適合您的安裝類型
description: 本主題說明 Windows 管理中心的不同安裝選項，包括在 Windows 10 電腦上安裝，或由多個系統管理員使用 Windows server。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950509"
---
# <a name="what-type-of-installation-is-right-for-you"></a>什麼類型的安裝最適合您？

本主題說明 Windows 管理中心的不同安裝選項，包括在 Windows 10 電腦上安裝，或由多個系統管理員使用 Windows server。 若要在 Azure 中的 VM 上安裝 Windows 管理中心，請參閱[在 azure 中部署 windows 系統管理中心](../azure/deploy-wac-in-azure.md)。

## <a name="installation-types"></a>安裝：類型

![img](../media/deployment-options/install-options.PNG)

| 本機用戶端                                | 閘道伺服器                                  | 受管理伺服器                               | 容錯移轉叢集                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| 在可連線到受管理伺服器的本機 Windows 10 用戶端上[安裝](../deploy/install.md)。  適用于快速入門、測試、臨機操作或小型規模案例。 |將[安裝](../deploy/install.md)在指定的閘道伺服器上，並從任何可連線到閘道伺服器的用戶端瀏覽器進行存取。  適用于大規模案例。 | 直接[安裝](../deploy/install.md)在受管理的伺服器上，以管理本身或其所屬的叢集。  適用于分散式案例。 | 在容錯移轉叢集中[部署](#high-availability)，以啟用閘道服務的高可用性。 適用于生產環境，以確保管理服務的復原能力。 |

## <a name="installation-supported-operating-systems"></a>安裝：支援的作業系統

您可以在下列 Windows 作業系統上**安裝**Windows 系統管理中心：

| **平台**                       | **安裝模式** |
| -----------------------------------| --------------------- |
| Windows 10                         | 本機用戶端 |
| Windows Server 半年通道 | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |
| WIN ENT LTSB 2016 Finnish 64 Bits                | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |
| Windows Server 2019                | 閘道伺服器、受管理的伺服器、容錯移轉叢集 |

若要操作 Windows 系統管理中心：

- **在本機用戶端案例中：** 從 [開始] 功能表啟動 Windows 管理中心閘道，然後藉由存取 `https://localhost:6516`從用戶端網頁瀏覽器連線到該閘道。
- **在其他案例中：** 從用戶端瀏覽器透過其 URL 連接到不同電腦上的 Windows 管理中心閘道，例如 `https://servername.contoso.com`

> [!WARNING]
> 不支援在網域控制站上安裝 Windows 系統管理中心。 [閱讀更多有關網域控制站安全性最佳作法的資訊](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。

## <a name="installation-supported-web-browsers"></a>安裝：支援的網頁瀏覽器

Microsoft Edge （包括[Microsoft edge insider](https://microsoftedgeinsider.com)）和 Google Chrome 已在 Windows 10 上進行測試和支援。 其他網頁瀏覽器（包括 Internet Explorer 和 Firefox）目前不屬於我們的測試矩陣，因此不會*正式*支援。 這些瀏覽器可能會在執行 Windows 系統管理中心時發生問題。 例如，Firefox 有自己的憑證存放區，因此您必須將 `Windows Admin Center Client` 憑證匯入 Firefox，才能在 Windows 10 上使用 Windows 系統管理中心。 如需詳細資訊，請參閱[瀏覽器特有的已知問題](../support/known-issues.md#browser-specific-issues)。

## <a name="management-target-supported-operating-systems"></a>管理目標：支援的作業系統

您可以使用 Windows 系統管理中心來**管理**下列 windows 作業系統：

| 版本 | 透過*伺服器管理員*管理*節點* | 透過叢集*管理員*管理 |
| ------------------------- |--------------- | ----- |
| Windows 10 | 是（透過電腦管理） | 無 |
| Windows Server 半年通道 | [是] | [是] |
| Windows Server 2019 | [是] | [是] |
| WIN ENT LTSB 2016 Finnish 64 Bits | [是] | 是，具有[最新的累計更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | [是] | [是] |
| Windows Server 2012 R2 | [是] | [是] |
| Microsoft Hyper-V Server 2012 R2 | [是] | [是] |
| Windows 2012 Server | [是] | [是] |
| Windows Server 2008 R2 | 是，有限的功能 | 無 |

> [!NOTE]
> Windows 管理中心需要不包含在 Windows Server 2008 R2、2012和 2012 R2 中的 PowerShell 功能。 如果您要使用 Windows 系統管理中心來管理這些服務，您必須在這些伺服器上安裝 Windows Management Framework （WMF）5.1 版或更新版本。
> 
> 在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。 
> 
> 如果未安裝 WMF，您可以[下載 wmf 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。

## <a name="high-availability"></a>高可用性

您可以藉由在容錯移轉叢集的主動-被動模型中部署 Windows 管理中心，來啟用閘道服務的高可用性。 如果叢集中的其中一個節點失敗，Windows 系統管理中心會適當地故障轉到另一個節點，讓您在環境中繼續以順暢的方式管理伺服器。

[瞭解如何部署具有高可用性的 Windows 管理中心。](../deploy/high-availability.md)

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)
