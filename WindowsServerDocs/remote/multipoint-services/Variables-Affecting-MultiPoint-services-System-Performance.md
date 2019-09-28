---
title: 影響 MultiPoint 服務系統效能的變數
description: MultiPoint 服務的效能資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: cba973e3b0a89c26f886a67154c27831adb2c8cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394834"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>影響 MultiPoint 服務系統效能的變數
有許多變數可能會影響 MultiPoint 服務系統的整體效能。 在設計系統時，您可能會想要考慮這些事項。  
  
## <a name="usage"></a>使用量  
  
-   **應用程式**同時執行的應用程式類型和數目，特別是圖形\-繁重或記憶體密集型應用程式將會影響系統的整體效能。 如需詳細資訊，請參閱[應用程式和網際網路內容](hardware-and-performance-recommendations.md#applications-and-internet-content)。  
  
-   **網際網路使用**請考慮您的使用者是否要觀看多媒體內容或使用全動作影片的網頁。 如果同時查看太多使用者，這種類型的內容可能會使系統超載。  
  
    > [!NOTE]  
    > MultiPoint 服務中的投射功能，可讓教師將其螢幕投影到其學生監視器，而不是設計來投影全動作影片。 投影功能是針對示範用途所設計，例如顯示程式。  
  
-   **高速裝置**如果有太多使用者同時使用高速裝置（例如，網路攝影機或 DVD 播放機），這會影響系統的整體效能。  
  
## <a name="configuration"></a>組態  
  
-   **CPU、GPU 和 RAM**如需 CPU、GPU 和 RAM 的建議，請參閱本指南中的[優化 MultiPoint 服務系統效能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。  
-   **網路頻寬**針對 RDP over LAN 連線的工作站，網路頻寬和用戶端的功能（例如瘦用戶端、桌上型電腦或膝上型電腦）很重要，尤其是當影片在使用者的會話中執行時。 如果您使用 USB over Ethernet 的零用戶端，則也應該考慮網路頻寬。 所有裝置的影片資料會透過相同的乙太網路連線傳送，因此，您可以考慮在使用這些裝置時設定個別的 Gigabit 乙太網路。  
-   **RemoteFX**針對 RDP over LAN 連線的工作站，您可以使用 RemoteFX 來大幅改善高定義多媒體內容的傳遞。  
-   **顯示解析度**如果您有大量的全螢幕影片使用方式，您可能會想要考慮減少監視器解析度，以將效能最大化。  
-   **USB 零用戶端數目**伺服器上單一根中樞的 USB 零用戶端總數會直接影響影片效能。 如需詳細資訊，請參閱[適用于 USB 零用戶端連線站的版面](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations)配置。 受支援的 USB 乙太網路的零用戶端數目可能會稍微小於 USB 零用戶端的數目。  
-   **USB 頻寬**當您設計系統時，請考慮 USB 頻寬。  這對 USB 零用戶端而言特別重要，因為它會透過 USB 連線傳送影片資料。 若要將頻寬優化，請將連線到伺服器上單一 USB 埠的裝置數目降至最低。 這適用于菊輪鍊式站和中繼中樞。 如需詳細資訊，請參閱[工作站中樞](MultiPoint-services-Site-Planning.md#station-hubs)和[中繼中樞](MultiPoint-services-Site-Planning.md#intermediate-hubs)。  
  
-   **USB 類型**如果您要將三個以上的 USB 零用戶端連接到集線器，或使用高頻寬 USB 裝置，使用 USB 3.0 而不是 USB 2.0，會增加伺服器和中繼中樞之間的可用頻寬。  
  
-   **工作站**總工作站數會影響效能。 如果您有大量的圖形、處理或影片需求，您可能會想要限制整體的工作站數目。 如需詳細資訊，請參閱將[MultiPoint 服務系統效能優化](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。