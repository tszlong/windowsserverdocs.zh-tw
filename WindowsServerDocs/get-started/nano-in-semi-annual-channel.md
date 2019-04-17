---
title: Nano Server 在 Windows Server 半年度管道中的變更
description: 在新的 Windows Server 維護模型中，Nano Server 只是有一些功能變更的容器作業系統。
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 7e68d292c32ce58c786a3242203330fcae985913
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2018
ms.locfileid: "2711953"
---
# Nano Server 在 Windows Server 半年度管道中的變更

>適用於：Windows Server 半年度管道


如 [Windows Server 半年通道概觀](semi-annual-channel-overview.md)中所述，Windows Server 版本 1803 是半年通道中的最新版本。

如果您已經在執行 Nano 伺服器，對此維護模型就會很熟悉，因為最新商務分支 (CBB) 模型原先即已提供此服務。 Windows Server 新推出的半年通道只是同一模型的新名稱。 在此模型中，Nano 伺服器預期每年會有兩到三次的功能更新版本。

不過，在此次發行的 Windows Server 版本 1803 中，Nano 伺服器僅以**容器基底 OS 映像**的形式來提供。 您必須將其當做容器主機 (例如 Windows Server 的 Server Core 安裝) 中的容器來執行。 在此版本與在舊版本中執行以 Nano 伺服器 為基礎的容器，有下列不同之處：

- Nano Server 已針對 .NET Core 應用程式進行最佳化。
- Nano Server 比 Windows Server 2016 版本還要小。
- 預設不再包含 PowerShell Core、.NET Core 和 WMI are，但是您可以在建置容器時將 [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) 和 [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) 容器封裝納入。
- 不再有維護堆疊包含在 Nano Server 中。 Microsoft 會將更新的 Nano 容器發佈到您重新部署的 Docker Hub。
- 您可以使用 Docker 對新的 Nano 容器進行疑難排解。
- 您現在可以在 IoT 核心版上執行 Nano 容器。

## 相關主題
當測試人員計畫開始時，請在 [Windows 容器文件](http://aka.ms/windowscontainers)中尋找詳細資訊。
