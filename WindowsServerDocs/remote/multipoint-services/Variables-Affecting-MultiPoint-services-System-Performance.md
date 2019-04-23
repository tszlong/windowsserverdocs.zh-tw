---
title: 影響 MultiPoint 服務系統效能的變數
description: MultiPoint 服務的效能資訊
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867579"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>影響 MultiPoint 服務系統效能的變數
有許多變數可能會影響 MultiPoint 服務系統的整體效能。 若要設計您的系統時，請考慮這些。  
  
## <a name="usage"></a>使用量  
  
-   **應用程式**的型別和執行在同一時間，特別是圖形的應用程式數目\-繁重或記憶體密集應用程式將會影響您的系統的整體效能。 如需詳細資訊，請參閱 <<c0> [ 應用程式和網際網路內容](hardware-and-performance-recommendations.md#applications-and-internet-content)。  
  
-   **使用網際網路**考慮多媒體內容或使用完整影片影片的網頁，是否要檢視您的使用者。 如果太多使用者同時檢視，這種類型的內容可以多載系統。  
  
    > [!NOTE]  
    > 在 MultiPoint 服務中，以允許老師螢幕投影至其學生監視，[投影] 功能並不適用於專案完整影片的影片。 [投影] 功能被設計用於示範用途，例如顯示程序。  
  
-   **高速裝置**如果太多使用者同時使用高速的裝置，例如網路攝影機或 DVD 播放機，這會影響系統的整體效能。  
  
## <a name="configuration"></a>組態  
  
-   **CPU、 GPU 和 RAM**請參閱 <<c2> [ 最佳化 MultiPoint 服務系統效能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)在本指南中的 CPU、 GPU 和 RAM 的建議。  
-   **網路頻寬**RDP over Lan 連接的站台、 網路頻寬和用戶端 （例如精簡型用戶端、 桌面電腦或膝上型電腦） 的功能很重要，特別是當使用者工作階段中正在執行的視訊。 如果您使用 USB over Ethernet 零用戶端時，網路頻寬也應該考量。 所有裝置的視訊資料會透過相同的乙太網路連線，傳送，因此您可能想要使用這些裝置時，請考慮設定個別的乙太網路。  
-   **RemoteFX** RDP 移轉網路連線的站台，您可以使用 RemoteFX，大幅改善的高畫質多媒體內容傳遞。  
-   **顯示器解析度**如果您有大量的全螢幕視訊的使用量，您可能要考慮減少的監視器解析度，以將效能最大化。  
-   **Usb 極簡型用戶端數目**的伺服器上的單一根集線器上的 usb 極簡型用戶端總數會直接影響視訊的效能。 如需詳細資訊，請參閱 < [USB 零用戶端連線站台的版面配置](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations)。 USB over Ethernet 零用戶端站台支援的數目可能會稍微小於 usb 極簡型用戶端數目。  
-   **USB 頻寬**設計您的系統時，請考慮 USB 頻寬。  這是針對 usb 極簡型用戶端，透過 USB 連線傳送視訊資料特別重要。 若要最佳化頻寬，最小化連線到伺服器的單一 USB 連接埠的裝置數目。 這適用於菊輪鍊結工作站與中繼的中樞。 如需詳細資訊，請參閱 <<c0> [ 站台集線器](MultiPoint-services-Site-Planning.md#station-hubs)並[中繼集線器](MultiPoint-services-Site-Planning.md#intermediate-hubs)。  
  
-   **USB 類型**使用 USB 3.0 而不是 USB 2.0 會增加伺服器與中繼的中樞之間的可用頻寬，如果您要將三個以上的 usb 極簡型用戶端連接至中樞，或如果您使用的高頻寬的 USB 裝置。  
  
-   **站台**站台的總數會影響效能。 如果您有大量的圖形、 處理或視訊的需求，您可能想要限制的站台整體的數目。 如需詳細資訊，請參閱 <<c0> [ 最佳化 MultiPoint 服務系統效能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。