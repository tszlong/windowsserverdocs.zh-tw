---
title: 設定伺服器儲存體
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859979"
---
# <a name="configure-server-storage"></a>設定伺服器儲存體

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>硬碟設定範例  
 下表是建議的範例硬碟設定。 預估值是依據一般使用和功能而得出，但不會解決影響最佳效能的問題。 您可以依據客戶的喜好設定和需求，使用任何支援的硬碟類型來進行這些設定 (如 SATA 或 SCSI)。  
  
> [!IMPORTANT]
>   必須安裝 Windows Server Essentials 為 c： 磁碟區，並將磁碟區大小必須至少為 60 GB。 建議您在作業系統磁碟上建立兩個磁碟分割，並且不要使用 C: (系統磁碟分割) 來儲存任何商業資料。  
  
|伺服器層級|磁碟設定|  
|------------------|------------------------|  
|進入|-兩個實體磁碟<br /><br /> -以 RAID 1 鏡像集，其中包含下列設定：<br /><br /> -C： 磁碟區？ 60 GB<br /><br /> -D： 磁碟區？ 1000 GB|  
|中等|-三個實體磁碟<br /><br /> 設定為包含下列的 RAID 5 組：<br /><br /> -C： 磁碟區？ 60 GB<br /><br /> -D： 磁碟區？ 1500 GB|  
|高|-五個或多個實體磁碟總數<br /><br /> -兩個磁碟位於 RAID 1 鏡像集，其中包含 c： 磁碟區？ 100 GB<br /><br /> -在包含下列的 RAID 5 組所有剩餘的磁碟：<br /><br /> -D： 磁碟區？ 1500 GB<br /><br /> -E： 磁碟區？ 1500 GB|  
  
 這些建議考慮到已安裝的作業系統大小、伺服器使用的資料存放區平均大小，以及伺服器存留時間內的預期資料儲存成長。 磁碟區可以是單一實體磁碟上的磁碟分割，也可以置於個別的實體磁碟上。 因為伺服器會儲存重要資料，為您的客戶，建議您使用多個實體磁碟，並協助保護您的客戶的資料使用硬體 RAID 或儲存空間。  
  
## <a name="configuring-your-server-backup"></a>設定伺服器備份  
 除了伺服器的內部硬碟之外，客戶也應考慮使用外接式 USB 硬碟來進行備份。 理想狀況下，客戶至少要有兩個足夠容量的外接式硬碟來備份伺服器上的所有資料。 如果客戶使用外接式硬碟，就可以每晚讓一個磁碟離站，以進一步保護資料。  
  
## <a name="partition-configuration"></a>磁碟分割設定  
 在伺服器的初始設定期間，會在磁碟 0 最大的資料磁碟分割中建立一組包含共用資料夾的預設伺服器資料夾，以及用戶端電腦備份資料夾。  
  
## <a name="see-also"></a>另請參閱  

 [與 Windows Server Essentials ADK 快速入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [與 Windows Server Essentials ADK 快速入門](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

