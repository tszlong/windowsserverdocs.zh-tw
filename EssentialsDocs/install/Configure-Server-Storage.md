---
title: "設定伺服器的儲存空間"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>設定伺服器的儲存空間

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

## <a name="sample-hard-disk-configurations"></a>範例硬碟設定  
 下表建議的範例硬碟設定。 估計根據一般使用和功能，但它們不會影響到最佳效能問題的地址。 您可以使用任何類型的支援的硬碟機（例如 SATA 或 SCSI），這些組態依據喜好設定和您客戶需求。  
  
> [!IMPORTANT]
>   Windows Server Essentials 必須先安裝為 c：磁碟區，並磁碟區大小必須至少 60 GB。 建議您兩個磁碟分割磁碟上建立您的作業系統，並使用 c:（系統磁碟分割）來儲存的任何企業資料。  
  
|伺服器層級|磁碟設定|  
|------------------|------------------------|  
|項目|-有兩個實體磁碟<br /><br /> -設定為鏡射 raid-1 集，其中包含下列：<br /><br /> -C：磁碟區嗎？ 60 GB<br /><br /> -D 磁碟區嗎？ 1000 GB|  
|媒體|-三個實體磁碟<br /><br /> 設定以包含下列 raid-5 設定：<br /><br /> -C：磁碟區嗎？ 60 GB<br /><br /> -D 磁碟區嗎？ 1500 GB|  
|高|-五個或更多總實體磁碟<br /><br /> 的鏡像 raid-1 集，其中包含 c：磁碟區中兩個磁碟嗎？ 100 GB<br /><br /> 的剩餘所有磁碟包含下列 raid-5 設定：<br /><br /> -D 磁碟區嗎？ 1500 GB<br /><br /> -E: 磁碟區嗎？ 1500 GB|  
  
 這些建議考慮作業系統已安裝的大小，平均使用伺服器，資料儲存空間及預期的資料儲存到期間的成長伺服器的大小。 磁碟區可以將單一實體磁碟上的磁碟分割或可以將它們放在不同的實體磁碟。 因為伺服器會儲存您客戶重要的資料，建議您使用多個實體的磁碟，並協助保護您客戶的資料使用硬體 RAID 或儲存空間。  
  
## <a name="configuring-your-server-backup"></a>設定您的伺服器備份  
 除了內部硬碟伺服器上，針對考慮備份使用 USB 外接式硬碟。 最好客戶必須使用務備份所有資料的伺服器上的兩個以上外接式硬碟。 如果外接式硬碟、客戶可能需要一部磁碟離每晚進一步保護資料。  
  
## <a name="partition-configuration"></a>磁碟分割設定  
 在初始設定伺服器，一組預設包含共用的資料夾和 client 電腦備份資料夾伺服器資料夾會建立在 0 上最大資料磁碟分割。  
  
## <a name="see-also"></a>也了  

 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [開始使用 Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

