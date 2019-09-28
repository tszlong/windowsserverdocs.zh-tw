---
title: 設定站台
description: 瞭解如何在 MultiPoint 服務中設定工作站
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 95a332a3d15e82047b46cc19f168f945cdb334d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389450"
---
# <a name="set-up-a-station"></a>設定站台
MultiPoint 服務「站台」通常是由「站台集線器」、滑鼠、鍵盤和視訊監視器所組成。 本主題說明如何將硬體裝置連接到站台集線器，以建立 MultiPoint 服務站台。  
  
站台集線器是將周邊裝置連接到 MultiPoint 服務系統中之電腦的硬體裝置。 MultiPoint 服務支援兩種站台集線器類型︰  
  
-   **USB 集線器：** 符合通用序列匯流排2.0 或更新版本規格的一般多埠 USB 擴充集線器。 這類集線器通常有兩個、四個或更多 USB 連接埠，可讓多個 USB 裝置連接到電腦上的單一 USB 連接埠。 USB 集線器通常是獨立的裝置，可能是外部電源或匯流排驅動。 搭配 MultiPoint 服務當作站台集線器使用時，建議您使用含有四個以上連接埠的集線器。  
  
    > [!IMPORTANT]  
    > 如果您打算將鍵盤和滑鼠以外的 USB 裝置連接到集線器，建議您使用外部供電的集線器以獲得最佳效能。  
  
-   **多功能集線器：** 透過 USB 埠連接到電腦的擴充中樞，可讓各種非 USB 裝置連線到中樞，包括影片監視器。 多功能集線器是由特定硬體製造商所產生，而且可能需要安裝裝置特定驅動程式。  
  
如果您想要新增站台至 MultiPoint 服務系統，請先確定有足夠的連接埠可供所要使用的站台硬體使用。 此外，您必須為 MultiPoint 服務系統保護適當數目的*用戶端存取使用權（cal）* 。  
  
## <a name="setting-up-station-hardware"></a>設定站台硬體  
本節中的程序說明如何將 MultiPoint 服務站台硬體連接到不同類型的站台集線器。  
  
### <a name="to-set-up-a-station-with-a-usb-hub"></a>使用 USB 集線器設定站台  
  
1.  您必須先「結束」所有使用者「工作階段」，再關閉電腦和您 MultiPoint 服務系統中的其他供電裝置，才能連接新的站台。  
  
2.  將新的視訊監視器纜線連接到電腦上的視訊顯示連接埠，如下圖所示：  
  
    ![視訊連線到 USB 集線器架構系統的影像](./media/WMSVideoConnection.gif)  
  
3.  將新的 USB 集線器連接到電腦上已開啟的 USB 連接埠：  
  
    ![MultiPoint Server USB 集線器連線的影像](./media/WMSUSBHubConnection.gif)  
  
4.  將鍵盤和滑鼠連接到 USB 集線器︰  
  
    ![USB 集線器輸入裝置連線的影像](./media/WMSUSBDeviceConnection.gif)  
  
5.  將視訊監視器的電源線連接到電源插座。  
  
6.  啟動電腦。  
  
7.  MultiPoint 服務隨即啟動。 遵循新工作站的影片監視器上出現的指示，將裝置與新的工作站建立關聯。  
  
### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>使用多功能集線器設定站台  
  
1.  您必須先結束所有使用者工作階段，再關閉電腦和您 MultiPoint 服務系統中的其他供電裝置，才能連接新的站台。  
  
2.  將新的視訊監視器纜線連接到多功能集線器上的 DVI 或 VGA 視訊顯示連接埠，如下圖所示：  
  
    ![多功能集線器與視訊連線的影像](./media/WMSMultifunctionHubVideoConnection.gif)  
  
3.  將新的多功能集線器連接到電腦上已開啟的 USB 連接埠：  
  
    ![多功能集線器連線的影像](./media/WMSMultifunctionHubConnection.gif)  
  
4.  將鍵盤和滑鼠連接到多功能集線器上的 PS2 或 USB 連接器︰  
  
    ![多功能集線器與 PS2 接頭的影像](./media/WMSMultifunctionHubPS2Connection.gif)  
  
5.  將視訊監視器的電源線連接到電源插座。  
  
6.  啟動電腦。  
  
7.  MultiPoint 服務隨即啟動。 若出現提示，請依照新電臺影片監視器上出現的指示，將裝置*關聯*到新的工作站。  
  
## <a name="see-also"></a>另請參閱  
[結束使用者工作階段](End-a-User-Session.md)  
[重新啟動或關閉](Restart-or-Shut-Down.md)  
[管理站台硬體](Manage-Station-Hardware.md)  
[使用 USB 裝置](Work-with-USB-Devices.md)