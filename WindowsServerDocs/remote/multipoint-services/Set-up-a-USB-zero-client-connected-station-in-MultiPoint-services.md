---
title: 零個用戶端連線的站台的 USB 設定 MultiPoint 服務中
description: 了解如何在 MultiPoint 服務中建立的 USB 零的用戶端站台
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878929"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>零個用戶端連線的站台的 USB 設定 MultiPoint 服務中
當您使用 usb 極簡型用戶端建立 MultiPoint 服務站台時，每個站台的監視會連接到 usb 極簡型用戶端上的視訊連接埠，如下圖所示。 如需有關這個主題以及其他站台類型的詳細資訊，請參閱[MultiPoint 站台](MultiPoint-services-Stations.md)。
  
**MultiPoint 服務系統，直接視訊連線站台與兩個 usb 極簡型用戶端連線站台**  
  
![USB 極簡型用戶端連線的工作站](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> 設定 USB 零個用戶端連線的站台之前，請務必安裝您的視訊卡與 usb 極簡型用戶端的最新驅動程式。 過時的驅動程式可防止無法順利完成的 MultiPoint 服務組態。 如需相關指示，請參閱 <<c0> [ 更新及安裝裝置驅動程式，如有需要](Update-and-install-device-drivers-if-needed.md)。  
  
> [!IMPORTANT]  
> 如果您使用 USB over Ethernet 零用戶端，請從您的廠商，而不是此程序，以使用乙太網路連線來設定網路裝置中遵循的指示。  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>若要設定 USB 零個用戶端連線的站台  
  
1.  將視訊監視器纜線連接 USB 上的 DVI 或 VGA 視訊顯示連接埠零個用戶端，如下圖所示。  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
2.  在電腦上開啟的 USB 連接埠來連接 usb 極簡型用戶端。  
  
    ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
3.  Usb 極簡型用戶端連接鍵盤和滑鼠。  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
4.  如果您使用外部供電 usb 極簡型用戶端，將 usb 極簡型用戶端的電源線接上電源插座。  
  
5.  將視訊監視器的電源線連接到電源插座。  
  
6.  如果系統提示您裝置與站台建立關聯，請在監視器 來完成安裝程序遵循的指示。 （一般來說，USB 零個用戶端連線的站台是站台相關聯自動新增至伺服器。）