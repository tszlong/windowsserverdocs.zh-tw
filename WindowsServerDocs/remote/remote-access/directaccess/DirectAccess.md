---
title: DirectAccess
description: 您可以使用本主題來瞭解 Windows Server 2016 中 DirectAccess 的簡短總覽。
manager: brianlic
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 166d56da7127c19636e08b4ac8fe5912530d0c37
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947064"
---
# <a name="directaccess"></a>DirectAccess

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 DirectAccess 的簡短總覽，包括支援 DirectAccess 的伺服器和用戶端作業系統，以及適用于 Windows Server 2016 的其他 DirectAccess 檔連結。

> [!NOTE]
> 除了本主題之外，還提供下列 DirectAccess 檔。
>
> -   [Windows Server 中的 DirectAccess 部署路徑](DirectAccess-Deployment-Paths-in-Windows-Server.md)
> -   [部署 DirectAccess 的必要條件](Prerequisites-for-Deploying-DirectAccess.md)
> -   [DirectAccess 不支援的設定](DirectAccess-Unsupported-Configurations.md)
> -   [DirectAccess 測試實驗室指南](DirectAccess-Test-Lab-Guides.md)
> -   [DirectAccess 已知問題](DirectAccess-Known-Issues.md)
> -   [DirectAccess 容量規劃](DirectAccess-Capacity-Planning.md)
> -   [DirectAccess 離線網域加入](DirectAccess-Offline-Domain-Join.md)
> -   [對 DirectAccess 進行疑難排解](Troubleshooting-DirectAccess.md)
> -   [使用消費者入門 Wizard 部署單一 DirectAccess 伺服器](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)
> -   [使用進階設定部署單一 DirectAccess 伺服器](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)
> -   [將 DirectAccess 新增至現有的遠端存取 (VPN) 部署](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)

DirectAccess 允許遠端使用者連線到組織網路資源，而不需要傳統虛擬私人網路 (VPN) 連接。 使用 DirectAccess 連線時，遠端用戶端電腦一律會連線至您的組織-不需要遠端使用者來啟動和停止連線，因為 VPN 連線需要此連接。 此外，您的 IT 系統管理員可以在每次執行時管理 DirectAccess 用戶端電腦，以及連線到網際網路。

>[!IMPORTANT]
>請勿嘗試在 Microsoft Azure 的虛擬機器 VM 上部署遠端 \( 訪問 \) 。 不支援在 Microsoft Azure 中使用遠端存取。 您無法在 Azure VM 中使用遠端存取，在 Windows Server 2016 或舊版 Windows Server 中部署 VPN、DirectAccess 或任何其他遠端存取功能。 如需詳細資訊，請參閱 [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

DirectAccess 僅支援加入網域的用戶端，包括 DirectAccess 的作業系統支援。

下列伺服器作業系統支援 DirectAccess。

-   您可以將所有版本的 Windows Server 2016 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。

-   您可以將所有版本的 Windows Server 2012 R2 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。

-   您可以將所有版本的 Windows Server 2012 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。

-   您可以將所有版本的 Windows Server 2008 R2 部署為 DirectAccess 用戶端或 DirectAccess 伺服器。

下列用戶端作業系統支援 DirectAccess。

-   Windows 10 Enterprise

-   Windows 10 企業版2015長期維護分支 (LTSB) 

-   Windows 8 和 8.1 Enterprise

-   Windows 7 旗艦版

-   Windows 7 Enterprise
