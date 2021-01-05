---
title: 目標電腦的硬體需求
description: 深入瞭解安裝 Windows Server Essentials 時必須具備的硬體需求。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c20b06b9-ce0d-4c20-bf49-257c3f5dc01b
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5c1cfb86c4519ab4f98280f5e21569fb7829c081
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755234"
---
# <a name="hardware-requirements-for-the-target-computer"></a>目標電腦的硬體需求

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本節提供 Windows Server Essentials 的硬體需求。

## <a name="hardware-requirements-for-windows-server-essentials"></a>Windows Server Essentials 的硬體需求
 您必須在符合下表需求的目的電腦上安裝 Windows Server Essentials。

### <a name="table-1--system-requirements-for-windows-server-essentials"></a>表1： Windows Server Essentials 的系統需求

|元件|最小值|建議需求*|最大值|
|---------------|-------------|-------------------|-------------|
|CPU 通訊端|單一核心的需求為 1.4 GHz (64 位元處理器) 或更快速度<br /><br /> 多個核心的需求為 1.3 GHz (64 位元處理器) 或更快速度|多個核心的需求為 3.1 GHz (64 位元處理器) 或更快速度|2 個通訊端|
|記憶體 (RAM)|2 GB|16 GB|64 GB|
|硬碟和可用的儲存空間|160 GB 硬碟 (含一個 60 GB 系統磁碟分割)||沒有限制|

 * 支援 Windows Server Essentials 最大使用者和裝置限制的建議硬體需求。

## <a name="additional-hardware-and-software-requirements"></a>其他硬體和軟體需求
 下表列出其他硬體和軟體需求。

|元件|描述|
|---------------|-----------------|
|網路介面卡|Gigabit Ethernet 介面卡 (10/100/1000baseT PHY/MAC)|
|網際網路|某些功能可能需要網際網路存取 (費用可能會套用) 或 Windows Live &reg; ID 帳戶|
|支援的用戶端作業系統|-Windows 7<br />-Windows 8<br />-Macintosh OS X 10.5 至10.8。<br /><br /> **注意：** 某些功能需要專業版或更高版本。<br /><br /> 1 GB 的可用硬碟空間 (安裝之後將會釋放此磁碟的某一部分)|
|路由器|支援 IPv4 NAT 的路由器或防火牆|
|其他需求|DVD-ROM 光碟機|

 視系統設定以及您選擇安裝的應用程式和功能而定，實際需求會有所不同。 處理器效能不僅取決於處理器的時脈頻率，也取決於核心數目和處理器快取大小。 系統磁碟分割的儲存空間需求為粗估值。 如果透過網路進行安裝，可能需要額外的可用儲存空間。

 如需硬體需求的詳細資訊，請參閱 [Windows Server Catalog](https://www.windowsservercatalog.com)。

 所有伺服器硬體都應符合為系統的 Windows Server 2012 標誌程式所建立的需求。 如需詳細資訊，請參閱 [Windows Logo Program](https://www.microsoft.com/whdc/winlogo/hwrequirements.mspx)。

## <a name="see-also"></a>另請參閱

 [使用 Windows Server ESSENTIALS ADK 消費者入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)[建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

