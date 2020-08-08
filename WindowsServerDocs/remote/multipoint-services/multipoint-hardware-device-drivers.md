---
title: 收集安裝所需的硬體與裝置驅動程式
description: 針對 MultiPoint 服務安裝所需驅動程式的相關資訊
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: d66c7fe216d5c2503f536e387b4e013e74092745
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992773"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>收集安裝所需的硬體與裝置驅動程式
在您開始部署 MultiPoint 服務系統之前，您將需要：

-   **伺服器的硬體元件**-目前安裝任何其他視訊卡或其他系統元件。

-   **工作站的硬體元件**-如需為您的環境規劃工作站的詳細資訊，請參閱[選取 MultiPoint 服務系統的硬體](./select-hardware-mps.md)。
-   **視訊卡的最新驅動程式**-如果您的 OEM 或裝置製造商未提供這些專案，您將需要從裝置製造商的網站下載。

-   **最新的 usb 零用戶端驅動程式**-如果您使用 USB 零的用戶端站，您必須安裝最新的 usb 零用戶端驅動程式。

    > [!IMPORTANT]
    > 針對 MultiPoint 服務安裝，您必須安裝64位版本的任何驅動程式。

> [!TIP]
> 如果您要在已安裝不同 Windows 版本的電腦上安裝 MultiPoint 服務，您應該在開始安裝 Windows Server 之前，先找出 Device Manager 的視訊卡製作和模型，並確定您可以取得適用于 Windows Server 2016 的驅動程式。 開啟 Device Manager，從 [**開始**] 畫面開啟 [**電腦管理**]。 然後，在主控台樹中，按一下 [ **Device Manager**]。