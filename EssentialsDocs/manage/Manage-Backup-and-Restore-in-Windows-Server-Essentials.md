---
title: "管理備份及還原 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>管理備份及還原 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 Windows Server Essentials 提供可靠的方式執行定期備份您的伺服器和您電腦的備份。 萬一資料遺失，您可以從在伺服器上成功備份還原資料不還原整部電腦。 必要時，您可以執行完整的系統還原到您的伺服器或 client 電腦在網路。 下表描述以及其優點可用的其他備份選項。  
  
|備份的功能|描述|優點|  
|--------------------|-----------------|----------------|  
|伺服器備份|備份您執行的 Windows Server Essentials 的伺服器。 資料會備份到外接式 USB 磁碟機。<br /><br /> 如需詳細資訊，請查看[管理伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md)和[還原或修復您的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md)。|-可以還原的檔案和資料夾伺服器上。<br /><br /> -可以執行 server 的完整的系統還原。|  
|Client 電腦備份|備份您網路中的 client 電腦。 執行 Windows Server Essentials 的伺服器上為備份的資料。<br /><br /> 如需詳細資訊，請查看[管理 Client 備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)和[從現有的 client 電腦備份還原完整的系統](Restore-a-full-system-from-an-existing-client-computer-backup.md)。|-可以還原的檔案和資料夾從您的伺服器。<br /><br /> -可以執行 client 電腦的完整的系統還原。|  
| Microsoft Azure 備份|執行 online 備份檔案或資料夾的伺服器上。 當您使用 Azure 備份資料備份伺服器時，資訊是使用您複雜密碼，才能在網際網路上的資料安全中心以上傳加密。<br /><br /> 如需詳細資訊，請查看[管理 Online Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md)。|-可以還原的檔案和資料夾從您的伺服器。<br /><br /> -的增量備份，只變更檔案的傳輸至雲端。<br /><br /> -備份儲存在 Microsoft Azure 且離站減少安全並避免現場備份的媒體。|  
  
## <a name="see-also"></a>也了  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
