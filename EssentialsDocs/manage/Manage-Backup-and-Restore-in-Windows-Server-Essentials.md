---
title: 管理 Windows Server Essentials 中的備份與還原
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828519"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的備份與還原

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 Windows Server Essentials 提供可靠的方法來定期備份您的伺服器和網路電腦。 如果發生資料遺失，您可以從伺服器上的成功備份還原資料，而不需要還原整部電腦。 必要時，您可以將整個系統還原到您的伺服器或網路中的用戶端電腦。 下表說明可供您使用的不同備份選項及其優點。  
  
|備份功能|描述|優點|  
|--------------------|-----------------|----------------|  
|伺服器備份|會備份執行 Windows Server Essentials 的伺服器。 資料會備份到外部 USB 磁碟機。<br /><br /> 如需詳細資訊，請參閱 <<c0> [ 管理伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md)並[還原或修復您的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md)。|-可以還原您的伺服器上檔案和資料夾。<br /><br /> -可以執行完整系統還原，您的伺服器。|  
|用戶端電腦備份|會備份網路中的用戶端電腦。 資料會備份在執行 Windows Server Essentials 的伺服器上。<br /><br /> 如需詳細資訊，請參閱 <<c0> [ 管理的用戶端備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)並[從現有的用戶端電腦備份還原完整的系統](Restore-a-full-system-from-an-existing-client-computer-backup.md)。|-可以從您的伺服器還原檔案和資料夾。<br /><br /> -可以執行用戶端電腦的完整系統的還原。|  
| Microsoft Azure 備份|會在伺服器上執行檔案或資料夾的線上備份。 當您使用 Azure 備份來備份伺服器資料時，會先上傳到安全的資料中心，在網際網路上使用您的複雜密碼加密的資訊。<br /><br /> 如需詳細資訊，請參閱 <<c0> [ 管理線上備份](Manage-Online-Backup-in-Windows-Server-Essentials.md)。|-可以從您的伺服器還原檔案和資料夾。<br /><br /> -之後進行增量備份時，只有變更的檔案會傳輸至雲端。<br /><br /> 備份會儲存在 Microsoft Azure 且離站存放，減少現場備份媒體安全防護的需求。|  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
