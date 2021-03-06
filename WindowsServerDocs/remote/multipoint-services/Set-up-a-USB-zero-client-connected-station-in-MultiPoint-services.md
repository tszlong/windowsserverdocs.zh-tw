---
title: 在 MultiPoint 服務中設定 USB 零用戶端連線的工作站
description: 瞭解如何在 MultiPoint 服務中建立 USB 零用戶端工作站
ms.date: 07/22/2016
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: e0db1c5013529e77aa757f086f842a1ad9007868
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953764"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>在 MultiPoint 服務中設定 USB 零用戶端連線的工作站
當您使用 USB 零用戶端來建立 MultiPoint 服務工作站時，每個工作站的監視會連接到 USB 零用戶端上的視訊連接埠，如下圖所示。 如需此和其他工作站類型的詳細資訊，請參閱[MultiPoint 電臺](MultiPoint-services-Stations.md)。

**MultiPoint 服務系統，其中有一個直接連接視頻的工作站和兩個 USB 零用戶端連線的工作站**

![USB 極簡型用戶端連線的工作站](./media/WMS11_diagram7.gif)

> [!IMPORTANT]
> 設定 USB 零用戶端連線的工作站之前，請務必安裝視訊卡和 USB 零用戶端的最新驅動程式。 過時的驅動程式可能會導致 MultiPoint 服務設定無法順利完成。 如需指示，請參閱視[需要更新及安裝設備磁碟機](Update-and-install-device-drivers-if-needed.md)。

> [!IMPORTANT]
> 如果您使用 USB over 乙太網路的零用戶端，請遵循廠商提供的指示（而不是此程式），使用 Ethernet 連線來設定網路上的裝置。

### <a name="to-set-up-a-usb-zero-client-connected-station"></a>設定 USB 零用戶端連線的工作站

1.  將視頻監視器纜線連接到 USB 零用戶端上的 DVI 或 VGA 視頻顯示埠，如下圖所示。

    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)

2.  將 USB 零用戶端連接到電腦上開啟的 USB 埠。

    ![MultiPoint 服務 USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)

3.  將鍵盤和滑鼠連接到 USB 零用戶端。

    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)

4.  如果您使用外部供電的 USB 零用戶端，請將 USB 零用戶端的電源線連接到電源插座。

5.  將視訊監視器的電源線連接到電源插座。

6.  如果系統提示您將裝置與工作站建立關聯，請遵循監視器上的指示來完成安裝。  (通常會在您將其新增至伺服器時，自動將 USB 零用戶端連線的工作站與工作站相關聯。 ) 