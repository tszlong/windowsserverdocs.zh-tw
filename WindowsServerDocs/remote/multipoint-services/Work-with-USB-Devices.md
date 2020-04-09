---
title: 使用 USB 裝置
description: 瞭解 USB 裝置如何與 MultiPoint 服務搭配使用
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: a33f2b83-bbc2-4fc1-8a94-aaa985dfe1f9
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d366e8c61da86d0e47b2ce99d08a2046c8f8bd0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858031"
---
# <a name="work-with-usb-devices"></a>使用 USB 裝置
您可以將裝置連接到 MultiPoint 服務系統中的電腦，或是連接到 MultiPoint「站台集線器」。 裝置的連接位置和裝置類型會影響裝置可供系統上的所有使用者使用、僅限於個別使用者使用，還是任何使用者都無法使用。 以下是不同連接類型的範例︰  
  
-   如果您將裝置 (例如印表機或 USB 大型存放裝置) 直接連接到電腦，則 MultiPoint 服務系統上的所有工作階段使用者都可以存取此裝置。 虛擬桌面站台使用者將無法存取直接連接到電腦的裝置。  
  
-   如果您將裝置 (例如鍵盤、滑鼠、「音訊裝置」或大型存放裝置) 連接到站台集線器，則只有登入該 MultiPoint 服務站台的使用者可以使用此裝置。  
  
-   如果您將特定類型的裝置 (例如鍵盤或滑鼠) 連接到電腦，則系統上的任何使用者都無法使用這些裝置。  
  
下表顯示裝置清單，以及裝置的運作方式 (根據裝置連接到系統的位置)。 有關如何連接站中樞的資訊，請參閱[使用站中樞](#working-with-station-hubs)。 如需如何將影片監視器連接到工作站的詳細資訊，請參閱使用[影片裝置](Work-with-Video-Devices.md)。  
  
|||||  
|-|-|-|-|  
|**主設備**|**直接連接到電腦時的行為**|**連線到工作站時的行為**|**紀錄**|  
|鍵盤|不建議將鍵盤直接連接到電腦。|僅供站台使用者存取。|如果鍵盤包含 USB 連接埠，則鍵盤內的 USB 集線器可能是站台集線器。 只有正在使用該鍵盤的使用者可以使用連接到該連接埠的其他 USB 裝置。<p>某些站台集線器配備可轉換為集線器內 USB 連接的 PS\/2 滑鼠連接埠。|  
|滑鼠|不建議將滑鼠直接連接到電腦。|僅供站台使用者存取。|某些站台集線器配備可轉換為集線器內 USB 連接的 PS\/2 滑鼠連接埠。|  
|USB 集線器|請參閱[使用工作站中樞](#working-with-station-hubs)。|請參閱[使用工作站中樞](#working-with-station-hubs)。||  
|視訊監視器|請參閱[MultiPoint 服務影片裝置](work-with-video-devices.md)。|請參閱[MultiPoint 服務影片裝置](work-with-video-devices.md)。||  
|音訊輸出裝置，例如耳機|不建議將音訊輸出裝置直接連接到電腦。|僅供站台使用者存取。|某些站台集線器配備可轉換為集線器內 USB 音訊連接的類比音訊連接埠。|  
|音訊輸入裝置，例如麥克風|不建議將音訊輸入裝置直接連接到電腦。|僅供站台使用者存取。|某些站台集線器配備可轉換為集線器內 USB 音訊連接的類比音訊連接埠。|  
|印表機|可供系統上的所有使用者存取。 *|僅供站台使用者存取。||  
|USB 大型存放裝置|可供系統上的所有使用者存取。\*|僅供站台使用者存取。|這些裝置包括 USB 快閃磁碟機、外接式硬碟機和數位相機。|  
|網路攝影機|可供系統上的所有使用者存取。 *|僅供站台使用者存取。|一次只能有一位使用者連接到攝影機。|  
  
\* 登入虛擬桌面工作站的使用者看不到連線到主機電腦的裝置。  
  
如需如何設定站台的詳細資訊，請參閱[設定站台](Set-Up-a-Station.md)。  
  
### <a name="working-with-station-hubs"></a>使用站台集線器  
當 USB 集線器連接到 MultiPoint 服務系統時，有四種可能的使用狀況。 以下每種狀況根據集線器的類型和連接到系統的位置，所連接裝置的可供存取情形各有不同。  
  
-   您可以使用連接到 MultiPoint 服務系統中之電腦的站台集線器 (已連接鍵盤)，來建立 MultiPoint 服務站台。 鍵盤和滑鼠會使用集線器上可用的連接埠連接到站台集線器。 視訊監視器則會連接到電腦的視訊連接埠，或連接到站台集線器的視訊卡 (如果有的話)。 然後，鍵盤、滑鼠和監視器會與 MultiPoint 服務站台「建立關聯」。  
  
-   當電腦的連接埠不足以供所需的裝置使用時，您可以使用連接到 MultiPoint 服務系統中之電腦的 USB 集線器 (未連接鍵盤)，將更多裝置連接到電腦。 MultiPoint 服務系統的所有使用者都可以使用連接到此 USB 集線器的所有裝置。 這不是 MultiPoint 服務站台集線器。  
  
-   您可以使用連接到 MultiPoint 服務系統中之電腦的供電 USB 集線器 (也稱為中繼集線器)，來連接建立 MultiPoint 站台所使用的其他 USB 集線器。  
  
-   您可以使用連接到站台集線器的 USB 集線器，將更多裝置連接到站台集線器。 鍵盤必須直接連接到站台集線器。  
  
如需如何設定 MultiPoint 服務站台的詳細資訊，請參閱[設定站台](Set-Up-a-Station.md)。  
  
## <a name="see-also"></a>另請參閱  
[使用視訊裝置](Work-with-Video-Devices.md)  
[管理站台硬體](Manage-Station-Hardware.md)  
[設定站台](Set-Up-a-Station.md)