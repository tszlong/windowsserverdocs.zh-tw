---
title: Nano Server 在 Windows Server 半年通道中的變更
description: 在新的 Windows Server 維護模型中，Nano Server 只是有一些功能變更的容器作業系統。
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c12ca84826a92fa045eb84b55e7406392161280b
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452800"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Nano Server 在 Windows Server 半年通道中的變更

>適用於：Windows Server 半年通道

如果您已經在執行 Nano Server[視窗 Server 半年通道](../get-started-19/servicing-channels-19.md)服務模型可能會很熟悉，因為它先前已由最新分支，Business (CBB) 模型。 Windows Server 半年通道是相同的模型的新名稱。 在此模型中，Nano Server 預期每年會有兩到三次的功能更新版本。

不過，從 Windows Server，版本 1803，Nano Server 目前僅供**容器基底 OS 映像**。 您必須將其當做容器主機 (例如 Windows Server 的 Server Core 安裝) 中的容器來執行。 在此版本與在舊版本中執行以 Nano 伺服器 為基礎的容器，有下列不同之處：

- Nano Server 已針對 .NET Core 應用程式進行最佳化。
- Nano Server 比 Windows Server 2016 版本還要小。
- 預設不再包含 PowerShell Core、.NET Core 和 WMI are，但是您可以在建置容器時將 [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) 和 [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) 容器封裝納入。
- 不再有維護堆疊包含在 Nano Server 中。 Microsoft 會將更新的 Nano 容器發佈到您重新部署的 Docker Hub。
- 您可以使用 Docker 對新的 Nano 容器進行疑難排解。
- 您現在可以在 IoT 核心版上執行 Nano 容器。

## <a name="related-topics"></a>相關主題

- [Windows 容器文件](http://aka.ms/windowscontainers)
- [視窗 Server 半年通道概觀](../get-started-19/servicing-channels-19.md)
