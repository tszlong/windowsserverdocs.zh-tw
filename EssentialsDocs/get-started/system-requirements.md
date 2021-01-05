---
title: Windows Server Essentials 系統需求
description: 瞭解 Windows Server Essentials 的系統需求。
ms.date: 10/31/2013
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 9a35006330452850dd10def998a7fdf2d8c8e853
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711693"
---
# <a name="system-requirements-for-windows-server-essentials"></a>Windows Server Essentials 系統需求

>適用于： Windows Server 2019 Essentials、Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

  Windows Server Essentials 伺服器軟體是僅限64位的作業系統。 表1定義 Windows Server Essentials 建議的最低硬體需求。 表 2 為伺服器的其他硬體與軟體需求。


## <a name="table-1-system-requirements-for-windows-server-essentials"></a>表 1. Windows Server Essentials 的系統需求

|元件|最小值|建議需求*|最大值|
|---------------|-------------|-------------------|-------------|
|CPU 通訊端|單一核心的需求為 1.4 GHz (64 位元處理器) 或更快速度<br /><br /> 多個核心的需求為 1.3 GHz (64 位元處理器) 或更快速度|多個核心的需求為 3.1 GHz (64 位元處理器) 或更快速度|2 個通訊端|
|記憶體 (RAM)|2 GB<br /><br /> 4 GB (如果您要部署 Windows Server Essentials 為虛擬機器)|16 GB|64 GB|
|硬碟和可用的儲存空間|160 GB 硬碟，含 60 GB 系統磁碟分割||沒有限制|

 *建議的硬體需求支援最大使用者和裝置限制。

## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>表 2. Windows Server Essentials 的其他硬體和軟體需求

|元件|描述|
|---------------|-----------------|
|網路介面卡|Gigabit Ethernet 介面卡 (10/100/1000baseT PHY/MAC)|
|網際網路|某些功能可能會需要網際網路存取權 (可能需支付費用) 或 Microsoft 帳戶|
|支援的用戶端作業系統| Windows 10、Windows 8.1、Windows 8、Windows 7、Macintosh OS X 版本10.5 到10.8。<br /><br /> **注意：** 某些功能需要專業版或更高版本。<br /><br /> 1 GB 的可用硬碟空間 (安裝之後將會釋放此磁碟的某一部分)|
|路由器|支援 IPv4 NAT 或 IPv6 的路由器或防火牆|
|其他需求|DVD-ROM 光碟機|

 視系統設定以及您選擇安裝的應用程式和功能而定，實際需求會有所不同。 處理器效能不僅取決於處理器的時脈頻率，也取決於核心數目和處理器快取大小。 系統磁碟分割的儲存空間需求為粗估值。 如果透過網路進行安裝，可能需要額外的可用儲存空間。

 如需硬體需求的詳細資訊，請參閱 [Windows Server Catalog](https://www.windowsservercatalog.com/)。

 所有伺服器硬體都應該符合針對系統的 Windows Server 2012 R2 標誌程式所建立的需求。 如需詳細資訊，請參閱 [Windows Logo Program](/previous-versions/windows/hardware/hck/dn641155(v=vs.85))。

> [!IMPORTANT]
> Windows Server Essentials 不支援動態磁碟。

## <a name="additional-references"></a>其他參考資料

-   [安裝 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

-   [Windows Server Essentials 系統需求](system-requirements.md)