---
title: Windows Server 版本 2004 和 20H2 中的新功能
description: Windows Server 版本 2004 和 20H2 的新功能。
ms.topic: article
author: Heidilohr
ms.author: helohr
ms.date: 05/27/2020
ms.localizationpriority: high
ms.openlocfilehash: f44dcc7a1c7fab2d99f0358794bd5b2aedb87b75
ms.sourcegitcommit: b82dfbfdc5df1327feb8be053a760739b01e3031
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2020
ms.locfileid: "92255865"
---
# <a name="whats-new-in-windows-server-version-2004-and-20h2"></a>Windows Server 版本 2004 和 20H2 中的新功能

>適用於：Windows Server (半年通道)

若要深入了解 Windows 的最新功能，請參閱 [Windows Server 的新功能](whats-new-in-windows-server.md)。 本主題說明 Windows Server 版本 2004 和 20H2 中的一些新功能。

Windows Server 版本 20H2 是 Windows Server 版本 2004 的下一個半年通道版本。 這個版本著重於可靠性、效能和其他一般改善項目，但沒有任何新功能。 與其他半年通道版本一樣，該版本在發行後提供 18 個月的支援。 如需半年通道版本支援日期的詳細資訊，請參閱 [Windows Server 版本資訊](windows-server-release-info.md)。

## <a name="server-core-container-improvements"></a>Server Core 容器改進功能

我們減少了 Server Core 容器映像的整體大小，以提升下載速度和效能。 其中包含下列增強功能：

- 已從 Server Core 容器映像移除大部分的 NGEN 影像，以使映像大小變小。
- 建立在 Server Core 容器映像上的 .NET Framework 執行階段映像，現在已針對 ASP.NET 應用程式和 Windows PowerShell 指令碼效能進行最佳化。
- .NET 小組也確保每一個 NGEN 影像只有一個副本，讓 .NET Framework 影像產生更小的影像。

為了讓您詳細了解這些容器的大小，下表為從 [2020 年 5 月的安全性更新](https://support.microsoft.com/help/4561769/windows-server-containers-for-may-2020) (也稱為「5B」更新) 為止，目前版本與舊版的容器比較表。

| 容器版本 | 下載大小 | 磁碟上的大小 |
|---|---|---|
| Windows Server 版本 1903 | 2.311 GB | 5.1 GB |
| Windows Server 版本 1909 | 2.257 GB | 4.97 GB |
| Windows Server 版本 2004 | 1.830 GB | 3.98 GB |

如需 Windows Server 版本 2004 更新的詳細資訊，請參閱[我們的部落格文章](https://techcommunity.microsoft.com/t5/containers/windows-server-version-2004-now-available/ba-p/1419194)。 如需深入了解 Windows 容器更新的一般資訊，請參閱[更新 Windows Server 容器](/virtualization/windowscontainers/deploy-containers/update-containers/)。
