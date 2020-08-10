---
title: Windows Server 版本 1809 中的新功能
description: Windows Server 版本 1809 的新功能
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.localizationpriority: high
ms.openlocfilehash: cb3bf10b0c89f60a5cebb7109ebc4a7de46f8961
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941637"
---
# <a name="whats-new-in-windows-server-version-1809"></a>Windows Server 版本 1809 中的新功能

>適用於：Windows Server (半年通道)

若要深入了解 Windows 的最新功能，請參閱 [Windows Server 的新功能](whats-new-in-windows-server.md)。 本主題說明 Windows Server 版本 1809 中的一些新功能。

## <a name="container-networking-with-kubernetes"></a>搭配 Kubernetes 的容器網路功能

Windows Server 2019 中的[搭配 Kubernetes 的容器網路功能](../networking/sdn/technologies/containers/container-networking-overview.md)透過強化平台網路復原能力和支援容器網路功能外掛程式，大幅改善 Windows 上的 Kubernetes 的可用性。
此外，客戶在 Kubernetes 網路安全性上部署工作負載，可使用內嵌工具來保護 Linux 和 Windows 服務。

## <a name="group-managed-service-accounts-for-containers"></a>適用於容器的群組受管理的服務帳戶需求

Windows Server 版本 1809 已改進使用群組受管理的服務帳戶 (gMSA) 來存取網路資源之容器的延展性和可靠性。

## <a name="host-device-access-for-containers"></a>容器的主機裝置存取

可指派簡易匯流排給獨立處理序的 Windows Server 容器。
在容器中執行的應用程式如果需要透過 SPI、I2C、GPIO 和 UART/COM 交談，現在可以執行這項操作。

## <a name="additional-features"></a>其他功能
除了 Windows Server 版本 1809 中的新功能，下列適用於 [Windows Server 2019](../get-started-19/get-started-19.md) 的新特色和功能也適用於 Windows Server 版本 1809：

* 容器改進功能
* HTTP/2
* Kubernetes 支援
* Windows 上的 Linux 容器
* [低額外延遲背景傳輸 (LEDBAT)](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)
* 虛擬工作負載的網路效能改進功能
* [Server Core 應用程式相容性功能隨選安裝 (FOD)](../get-started-19/install-fod-19.md)
* [存放裝置移轉服務 (SMS)](../storage/whats-new-in-storage.md#storage-spaces-direct)
* 儲存體複本
* 系統深入解析
* Windows Defender 進階威脅防護 (ATP)
* Windows Defender ATP 惡意探索防護
* [Windows Time 服務](../networking/windows-time-service/insider-preview.md)
