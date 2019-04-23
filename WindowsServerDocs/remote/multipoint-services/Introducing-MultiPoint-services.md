---
title: 簡介 MultiPoint 服務
description: 提供 MultiPoint 服務時，一種方法讓多位使用者共用系統的概觀
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 86d240092282e7cc29eebe638e5a97312e22baff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844209"
---
# <a name="introducing-multipoint-services"></a>簡介 MultiPoint 服務
Windows Server 2016 中的 multiPoint 服務角色可讓多位使用者，每個都有各自擁有獨立且熟悉的 Windows 體驗，同時共用一部電腦。有數種方式，使用者可以存取其工作階段。 一種方法是由伺服器使用的遠端處理所[遠端桌面應用程式](../remote-desktop-services/clients/remote-desktop-clients.md)與任何裝置。 另一種方式是透過實體附加至 MultiPoint server 的站台的站台：  
  
-   直接與電腦上的視訊連接埠  
  
-   透過特殊的 usb 極簡型用戶端 （也稱為多功能的 USB 集線器），以及透過類似的 USB-over-乙太網路裝置。  
  
-   透過區域網路 (LAN)  
  
每一種方法更詳細地說明[MultiPoint 服務站台](MultiPoint-services-Stations.md)本文件稍後的。  
  
本文件探討到當您打算部署 MultiPoint 服務，請考慮下列因素：  
  
-   桌面與 MultiPoint 服務系統中使用何種類型：您將需要工作階段、 虛擬機器或 Windows 電腦嗎？  
  
-   [選取的 MultiPoint 服務系統的硬體](Selecting-Hardware-for-Your-MultiPoint-services-System.md):您應該進行哪些硬體決策？  
  
-   [硬體需求以及效能建議](Hardware-Requirements-and-Performance-Recommendations.md):什麼硬體是為了 MultiPoint 服務？  
  
-   [MultiPoint 服務網站規劃](MultiPoint-services-Site-Planning.md):將執行 MultiPoint 服務和其站台的電腦位於何處，以及如何將它們設定嗎？  
  
-   [網路考量和使用者帳戶](Network-Considerations-and-User-Accounts.md):MultiPoint 服務系統的部署所在的網路功能環境可能會影響如何管理使用者帳戶。 什麼是您的網路環境？ 如何管理使用者帳戶？  
  
-   [儲存檔案使用 MultiPoint 服務](Storing-Files-with-MultiPoint-services.md):將使用者儲存檔案，以及如何將存取它們？  
  
-   [預先部署檢查清單](Predeployment-Checklist.md)  