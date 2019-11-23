---
title: 設定伺服器儲存體
description: 說明如何使用 Windows Server Essentials
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
ms.openlocfilehash: 718080c050dadc20837ab6b11a677029227e1709
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865038"
---
# <a name="configure-server-storage"></a>設定伺服器儲存體

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>硬碟設定範例  
 下表是建議的範例硬碟設定。 預估值是依據一般使用和功能而得出，但不會解決影響最佳效能的問題。 您可以依據客戶的喜好設定和需求，使用任何支援的硬碟類型來進行這些設定 (如 SATA 或 SCSI)。  
  
> [!IMPORTANT]
>   Windows Server Essentials 必須安裝為 C：磁片區，且磁片區大小必須至少為 60 GB。 建議您在作業系統磁碟上建立兩個磁碟分割，並且不要使用 C: (系統磁碟分割) 來儲存任何商業資料。  
  
|伺服器層級|磁碟設定|  
|------------------|------------------------|  
|項目|-兩個實體磁片<br /><br /> -設定為 RAID 1 鏡像集，其中包含下列各項：<br /><br /> -C：磁片區？ 60 GB<br /><br /> -D： volume？ 1000 GB|  
|中等|-三個實體磁片<br /><br /> -設定為 RAID 5 集合，其中包含下列各項：<br /><br /> -C：磁片區？ 60 GB<br /><br /> -D： volume？ 1500 GB|  
|高|-五個以上的實體磁片總計<br /><br /> -RAID 1 鏡像集內的兩個磁片，其中包含 C： volume？ 100 GB<br /><br /> -RAID 5 集合中的所有剩餘磁片，其中包含下列各項：<br /><br /> -D： volume？ 1500 GB<br /><br /> -E：磁片區？ 1500 GB|  
  
 這些建議考慮到已安裝的作業系統大小、伺服器使用的資料存放區平均大小，以及伺服器存留時間內的預期資料儲存成長。 磁碟區可以是單一實體磁碟上的磁碟分割，也可以置於個別的實體磁碟上。 由於伺服器會儲存客戶的重要資料，因此建議您使用硬體 RAID 或儲存空間，以使用多個實體磁片並協助保護您的客戶資料。  
  
## <a name="configuring-your-server-backup"></a>設定伺服器備份  
 除了伺服器的內部硬碟之外，客戶也應考慮使用外接式 USB 硬碟來進行備份。 理想狀況下，客戶至少要有兩個足夠容量的外接式硬碟來備份伺服器上的所有資料。 如果客戶使用外接式硬碟，就可以每晚讓一個磁碟離站，以進一步保護資料。  
  
## <a name="partition-configuration"></a>磁碟分割設定  
 在伺服器的初始設定期間，會在磁碟 0 最大的資料磁碟分割中建立一組包含共用資料夾的預設伺服器資料夾，以及用戶端電腦備份資料夾。  
  
## <a name="see-also"></a>請參閱  

 [消費者入門與 Windows Server ESSENTIALS ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [消費者入門與 Windows Server ESSENTIALS ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映射](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂](../install/Additional-Customizations.md)   
 [準備映射以進行部署](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

