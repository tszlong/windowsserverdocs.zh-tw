---
title: 收集安裝所需的硬體與裝置驅動程式
description: 針對 MultiPoint 服務安裝所需驅動程式的相關資訊
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: cfbb8c8b68768c72b869df539c93f05e7e01d256
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394698"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>收集安裝所需的硬體與裝置驅動程式
在您開始部署 MultiPoint 服務系統之前，您將需要：  
  
-   **伺服器的硬體元件**-目前安裝任何其他視訊卡或其他系統元件。  
  
-   **工作站的硬體元件**-如需為您的環境規劃工作站的詳細資訊，請參閱[選取 MultiPoint 服務系統的硬體](Selecting-Hardware-for-Your-MultiPoint-services-System.md)。
-   **視訊卡的最新驅動程式**-如果您的 OEM 或裝置製造商未提供這些專案，您將需要從裝置製造商的網站下載。  
  
-   **最新的 usb 零用戶端驅動程式**-如果您使用 USB 零的用戶端站，您必須安裝最新的 usb 零用戶端驅動程式。  
  
    > [!IMPORTANT]  
    > 針對 MultiPoint 服務安裝，您必須安裝64位版本的任何驅動程式。  
  
> [!TIP]  
> 如果您要在已安裝不同 Windows 版本的電腦上安裝 MultiPoint 服務，您應該在開始安裝 Windows Server 之前，先找出 Device Manager 中的視訊卡製作和型號，並確定您可以取得驅動程式，也就是適用于 Windows Server 2016。 開啟 Device Manager，從 [**開始**] 畫面開啟 [**電腦管理**]。 然後，在主控台樹中，按一下 [ **Device Manager**]。